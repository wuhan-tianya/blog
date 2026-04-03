# Claude Code 进阶使用指南

Claude Code 是 Anthropic 推出的一款运行在终端的 AI 编程助手工具。它可以直接访问你的本地代码库，并通过自然语言执行各种复杂的编程、重构和分析任务。除了基础的对话写代码外，掌握它的进阶用法能让你的开发效率迎来质的飞跃。

## 1. 工程上下文与 CLAUDE.md 规范

Claude Code 会自动扫描项目代码，但如果你希望它完全遵循你的团队规范，最佳实践是在项目根目录建立一个 `CLAUDE.md` 文件。

### CLAUDE.md 的核心作用
它类似于系统提示词（System Prompt），你可以把**代码风格**、**构建命令**、**测试命令**以及**特定框架的注意事项**写在里面：

```markdown
# 构建与测试
- 启动本地开发：`npm run dev`
- 运行测试：`npm run test`

# 代码规范
- 使用 TypeScript 编写，严格开启 `strict` 模式。
- React 组件优先使用函数式组件 (Functional Components) 和 Hooks。
- 变量命名使用驼峰式 (camelCase)，常量使用全大写加下划线 (UPPER_SNAKE_CASE)。
- 提交代码前，必须先进行 lint 和 format。
```

当 Claude Code 在该目录下运行时，它会自动读取并始终遵循这些规则，极大地减少了你每次都要重复说明背景的麻烦。

## 2. 巧用 Claude Code Router 进行多模型路由与降本增效

如果你经常遇到 API 限制或者希望降低调用成本，可以使用 [Claude Code Router](https://github.com/musistudio/claude-code-router) 这样的第三方工具链进行模型路由切换。

你可以将日常较为基础的修改、文件搜索或者后台任务交由相对经济的模型（如 `gemini-3.1-flash` 或者 本地的 Ollama 模型），而将核心重构、架构设计交由顶配模型（如 `gemini-3.1-pro` 或 `claude-3-7-sonnet`）。

### 常用 Router 策略配置
编辑 `~/.claude-code-router/config.json`：
- **`default`** / **`think`**: 采用高智力模型（如 `gemini-3.1-pro`）
- **`background`**: 采用轻量化/免费模型（如 `gemini-3.1-flash`）

通过运行 `ccr code` 启动后，你甚至可以在对话中随时输入 `/model` 命令动态切换当前使用的模型，满足各种复杂场景的需求。

## 3. 核心命令与场景组合

除了聊天，Claude Code 提供了大量终端 slash (`/`) 命令。以下是进阶场景下的必杀技组合：

### 场景一：深层 Debug 与问题修复
当代码报错时，不要直接把错误贴给它。使用以下策略：
1. **输入错误日志**：直接粘贴带有 stack trace 的报错。
2. **引导排查**：不要立即要求修改。可以说：“梳理 `src/` 下涉及该接口的所有引用，不要立即修改，先给出修复方案。”

### 场景二：大规模代码重构
进行重构时，不要让 Claude Code 一次性改完。这会导致不可控的错误。
1. 先对话规划：“我计划把这个庞大的 User 类拆分成 Repository 和 Service，你先分析一下目前的依赖关系，并输出一个重构步骤大纲。”
2. 确认大纲无误后，分步执行：“按照你给出的大纲，现在执行第一步：创建 Service 接口并把验证逻辑抽离过去。”

## 4. 权限与自动化控制 (Permission & Safety)

Claude Code 拥有直接读取和修改你本地文件、甚至执行终端命令（如 `npm install` 等）的能力。对于复杂的重构，你可能会觉得每次都需要 `y` 回车确认很繁琐。

- **自动批准特定命令**：你可以在配置中允许特定的安全命令免确认，或者在启动时通过 Flag `claude --permission-mode auto`（请极其谨慎地使用此功能，建议仅在沙箱或 git 状态干净时使用）。
- **结合 Git 工作流**：在让 AI 大规模操作前，请养成先 `git commit` 或者新建分支的习惯。AI 写错代码时，你可以轻易使用 `git restore .` 退回。如果 AI 对代码做了较大调整，直接跟它说：“帮我生成一个语义化的 git commit message，并执行 commit 操作”。

## 5. 与 CI/CD 及自动化脚本结合

借助于 `--print` 或非交互式模式 (`NON_INTERACTIVE_MODE`)，你可以在 GitHub Actions 中运行 Claude Code，让其帮你自动 Code Review。

```yaml
# 示例：在 GitHub Actions 中自动 Review 代码
- name: Code Review
  run: |
    claude "Review the changes in the latest commit. Point out any security flaws or performance issues. Only output markdown." --print > review_report.md
```
*（如果你搭配前面提到的 Router，也可以将 Review 任务低成本地交由各类开源、本地化的大语言模型完成。）*

## 总结

进阶使用 Claude Code 的核心在于：**把它当成一个初级/中级的协作工程师，而不是一个简单的问答机**。
给它提供好的规范 (`CLAUDE.md`)，用拆解的思维去指引它（先设计、再编码），并合理利用工具链（Router）管理成本与速度，你的 AI 结对编程体验将会得到极大的飞跃。
