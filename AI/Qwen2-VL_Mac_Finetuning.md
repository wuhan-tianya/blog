# 🍎 Qwen2-VL (2B) Mac 本地微调实战文档 (LLaMA-Factory + uv 极速版)

本文档专为 Mac M1/M2/M3 (如 M1 Pro 16GB) 设备量身定制，抛弃传统吃内存的 PyTorch 训练方式，使用目前业界最流行的 **LLaMA-Factory** 框架搭配 **uv** 极速环境管理，实现 Qwen2-VL 视觉大模型的本地丝滑微调。

---

## 第一阶段：拉取代码与极速环境配置 (使用 uv)

`uv` 是目前最快的 Python 包和环境管理工具，非常契合这套高效的工作流。我们将把 LLaMA-Factory 下载到本地并配置好环境。

**1. 下载 LLaMA-Factory 源码并进入目录**
```bash
cd ~/Desktop
git clone https://github.com/hiyouga/LLaMA-Factory.git
cd LLaMA-Factory
```

**2. 使用 uv 创建环境并极速安装依赖**
```bash
# 创建并激活虚拟环境
uv venv
source .venv/bin/activate

# 极速安装核心依赖 (自动识别 Mac 环境)
uv pip install -e ".[torch,metrics]"
uv pip install torchvision huggingface_hub
```

**3. 配置国内镜像源与 Mac 专属环境变量**
```bash
# 解决国内下载模型慢的问题
export HF_ENDPOINT=https://hf-mirror.com

# 【Mac 专属救命配置】解除 PyTorch 在苹果芯片上的显存限制，防止微调时报错
export PYTORCH_MPS_HIGH_WATERMARK_RATIO=0.0
```
*(建议把上面这两句 `export` 也记录下来，每次打开新终端微调前都执行一下)*

---

## 第二阶段：模型下载 (国内镜像极速方案)

虽然框架可以自动下载，但由于视觉模型通常有几个 GB 大小，网络一旦断开就会报错。所以**强烈建议先手动下载到本地**。

在终端中输入以下命令：

**1. 指定路径并下载 Qwen2-VL (2B)**
```bash
# 把模型下载到当前目录下的 Qwen2-VL-2B-Instruct 文件夹里
huggingface-cli download Qwen/Qwen2-VL-2B-Instruct --local-dir ./Qwen2-VL-2B-Instruct
```
*(下载完成后，当前目录就会多出一个 `Qwen2-VL-2B-Instruct` 文件夹)*

---

## 第三阶段：准备数据集 (Easy Dataset 一键生成)

LLaMA-Factory 对视觉数据集有严格的格式要求（ShareGPT 格式），且**图片必须使用绝对路径**。假设您本地的原始数据整理在一个 CSV 文件里（包含 `image_path`, `instruction`, `response` 三列）。

**1. 创建一键生成脚本**
新建一个 Python 文件，命名为 `easy_dataset_gen.py`，粘贴以下代码：

```python
import json
import os
import pandas as pd

def generate_easy_dataset():
    # 1. 读取本地数据表 (如果是 Excel 可改为 read_excel)
    df = pd.read_csv("local_data.csv")
    
    dataset = []
    for _, row in df.iterrows():
        # 2. 自动转换绝对路径，防止训练时找不到图片
        abs_img_path = os.path.abspath(row['image_path'])
        
        # 3. 组装 ShareGPT 格式
        item = {
            "messages": [
                {
                    "role": "user",
                    "content": f"<image>{row['instruction']}" 
                },
                {
                    "role": "assistant",
                    "content": str(row['response'])
                }
            ],
            "images": [abs_img_path]
        }
        dataset.append(item)
        
    # 4. 导出 JSON
    output_name = "my_vlm_data.json"
    with open(output_name, "w", encoding="utf-8") as f:
        json.dump(dataset, f, ensure_ascii=False, indent=2)
        
    print(f"🎉 成功生成 {len(dataset)} 条数据！保存为 {output_name}")

if __name__ == "__main__":
    generate_easy_dataset()
```

**2. 使用 uv 极速运行脚本 (阅后即焚模式)**
无需提前 `pip install pandas`，直接用 `uv` 挂载运行：
```bash
uv run --with pandas python easy_dataset_gen.py
```

**3. 注册数据集**
把生成的 `my_vlm_data.json` 移动到 `LLaMA-Factory/data/` 目录下。
打开 `LLaMA-Factory/data/dataset_info.json`，在最前面注册您的数据集：
```json
{
  "easy_vlm_dataset": {
    "file_name": "my_vlm_data.json",
    "formatting": "sharegpt",
    "columns": {
      "messages": "messages",
      "images": "images"
    }
  },
  // ... 其他默认数据集保持不变
}
```

---

## 第四阶段：启动 WebUI (小白最爱)

LLaMA-Factory 的最大优势在于无需记忆命令行参数，直接在网页里可视化操作。

```bash
llamafactory-cli webui
```
浏览器会自动打开 `http://localhost:7860`。

---

## 第五阶段：在网页上配置并开始微调

在打开的 WebUI 中，依次完成以下“填表”：

1. **Model Name (模型名称)**：选择 `Qwen2-VL-2B-Instruct`
2. **Model Path (模型路径)**：填入第二阶段下载的本地文件夹路径（如 `/Users/xxx/Desktop/LLaMA-Factory/Qwen2-VL-2B-Instruct`）。
3. **Finetuning method (微调方法)**：保持 `lora`。
4. **Compute type (计算类型)**：Mac 推荐选择 `fp16`。
5. **Dataset (数据集)**：下拉找到并勾选您注册的 `easy_vlm_dataset`。
6. **参数设置 (求稳策略)**：
   * Batch size (批处理大小): 设为 `1` (16GB 内存防爆显存最佳设置)。
   * Gradient accumulation (梯度累积): 设为 `4` 或 `8`。
   * Epochs (训练轮数): `3` ~ `5`。
7. **点击页面最下方的【Start (开始)】按钮！**

*观察网页下方绘制的 Loss 曲线，这就说明您的 M1 Pro 正在奋力“炼丹”了！*

---

## 第六阶段：测试与导出

1. **实时聊天测试**：
   训练完成后，在网页顶部切换到 **“Chat (聊天/测试)”**。
   左侧选择刚刚训练的 **Adapter (LoRA 适配器)**，点击 **Load Model (加载模型)**。
   传一张新图片，进行提问测试。
2. **导出完整模型**：
   在 **“Export (导出)”** 选项卡，把基础模型和 LoRA 权重合并，导出为一个独立的专属模型，方便日后直接部署使用。
