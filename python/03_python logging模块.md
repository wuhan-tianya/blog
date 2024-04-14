# 03 python logging模块


# 1.logging模块简介

 logging模块是Python内置的标准模块，主要用于输出运行日志，可以设置输出日志的等级、日志保存路径、日志文件回滚等；相比print，具备如下优点：

1. 可以通过设置不同的日志等级，在release版本中只输出重要信息，而不必显示大量的调试信息；
2. print将所有信息都输出到标准输出中，严重影响开发者从标准输出中查看其它数据；logging则可以由开发者决定将信息输出到什么地方，以及怎么输出；
3. 其它特点：basicConfig整个工程只在第一次配置时生效。

# 2.logging模块使用

## 2.1 基本使用

配置logging基本的设置，然后在控制台输出日志：

```python
import logging
logging.basicConfig(level = logging.INFO,format = '%(asctime)s - %(name)s - %(levelname)s - %(message)s')
logger = logging.getLogger(__name__)
logger.info("Start print log")
logger.debug("Do something")
logger.warning("Something maybe fail.")
logger.info("Finish")
```

运行时控台输出

```python
2022-11-22 21:30:25,616 - __main__ - INFO - Start print log
2022-11-22 21:30:25,616 - __main__ - WARNING - Something maybe fail.
2022-11-22 21:30:25,616 - __main__ - INFO - Finish
```

logging.basicConfig函数参数

- filename：指定日志文件名
- filemode：和file函数意义相同，指定日志文件的打开模式，'w'或者'a'
- format：指定输出的格式和内容，format可以输出很多有用的信息

参数作用：

- %(levelno)s：打印日志级别的数值
- %(levelname)s：打印日志级别的名称
- %(pathname)s：打印当前执行程序的路径，其实就是sys.argv[0]
- %(filename)s；：打印当前执行程序名
- %(funcName)s：打印日志的当前函数
- %(lineno)d：打印日志的当前行号
- %(asctime)s：打印日志的时间
- %(thread)d：打印线程ID
- %(threadName)s：打印线程名称
- %(process)d：打印进程ID
- %(message)s：打印日志信息
- datefmt：指定时间格式，同time.strftime()
- level：设置日志级别，默认为logging.WARNNING
- stream：指定将日志的输出流，可以指定输出到sys.stderr，sys.stdout或者文件，默认输出到sys.stderr，当stream和filename同时指定时，stream被忽略

> 注：此方法控制台测试时，basicConfig整个工程只在第一次配置时生效，整个输出均为控制台

一条日志信息对应的是一个事件的发生，而一个事件通常需要包括以下几个内容：

1. 事件的严重程度--日志级别（%(levelname)s：打印日志级别的名称）
2. 事件发生时间（%(asctime)s：打印日志的时间）
3. 事件发生位置（%(process)d：打印进程ID，%(funcName)s：打印日志的当前函数，%(lineno)d：打印日志的当前行号）
4. 事件内容（%(message)s：打印日志信息）

## 2.2 将日志同时输出到屏幕和日志文件

logger中添加StreamHandler，可以将日志输出到屏幕上：

```python

import logging
 
logger = logging.getLogger(__name__)
logger.setLevel(level=logging.INFO)
 
file_handler = logging.FileHandler("log.txt")  # 文件句柄
file_handler.setLevel(logging.INFO)
formatter = logging.Formatter('%(asctime)s - %(name)s - %(levelname)s - %(message)s')
file_handler.setFormatter(formatter)
logger.addHandler(file_handler)
 
console_handler = logging.StreamHandler()  # 流句柄
console_handler.setLevel(logging.INFO)
logger.addHandler(console_handler)
 
def main():
    logger.info("Start print log")
    logger.debug("Do something")
    logger.warning("Something maybe fail.")
    logger.info("Finish")
    
main() 
```

结果如下：

```python
终端输出：
Start print log
Something maybe fail.
Finish

文件输出：
2022-11-22 21:36:23,738 - __main__ - INFO - Start print log
2022-11-22 21:36:23,738 - __main__ - WARNING - Something maybe fail.
2022-11-22 21:36:23,738 - __main__ - INFO - Finish
```

可以发现，logging有一个日志处理的主对象，其他处理方式都是通过addHandler添加进去，各个处理方式之间相互独立。logging中包含的handler主要有如下几种：

| **handler**名称     | 位置                                 | 作用                                               |
| ------------------- | ------------------------------------ | -------------------------------------------------- |
| StreamHandler       | logging.StreamHandler                | 日志输出到流，可以是sys.stderr，sys.stdout或者文件 |
| FileHandler         | logging.FileHandler                  | 日志输出到文件                                     |
| BaseRotatingHandler | logging.handlers.BaseRotatingHandler | 基本的日志回滚方式                                 |
| RotatingHandler     | logging.handlers.RotatingHandler     | 日志回滚方式，支持日志文件最大数量和日志文件回滚   |
| TimeRotatingHandler | logging.handlers.TimeRotatingHandler | 日志回滚方式，在一定时间区域内回滚日志文件         |
| SocketHandler       | logging.handlers.SocketHandler       | 远程输出日志到TCP/IP sockets                       |
| DatagramHandler     | logging.handlers.DatagramHandler     | 远程输出日志到UDP sockets                          |
| SMTPHandler         | logging.handlers.SMTPHandler         | 远程输出日志到邮件地址                             |
| SysLogHandler       | logging.handlers.SysLogHandler       | 日志输出到syslog                                   |
| NTEventLogHandler   | logging.handlers.NTEventLogHandler   | 远程输出日志到Windows NT/2000/XP的事件日志         |
| MemoryHandler       | logging.handlers.MemoryHandler       | 日志输出到内存中的指定buffer                       |
| HTTPHandler         | logging.handlers.HTTPHandler         | 通过"GET"或者"POST"远程输出到HTTP服务器            |

### 2.2.3 日志滚动和过期删除

1.使用RotatingFileHandler，可以实现日志按大小回滚（设置，最多备份几个日志文件，每个日志文件最大值）

```python
import logging
from logging.handlers import RotatingFileHandler # 按文件大小回滚handler
 
def main():
    logger = logging.getLogger(__name__)
    logger.setLevel(level=logging.INFO)
    # 定义一个RotatingFileHandler，最多备份3个日志文件，每个日志文件最大1K
    rHandler = RotatingFileHandler("log.txt", maxBytes=1 * 1024, backupCount=3)
    rHandler.setLevel(logging.INFO)
    formatter = logging.Formatter('%(asctime)s - %(name)s - %(levelname)s - %(message)s')
    rHandler.setFormatter(formatter)
 
    console = logging.StreamHandler()
    console.setLevel(logging.INFO)
    console.setFormatter(formatter)
 
    logger.addHandler(rHandler)
    logger.addHandler(console)
 
    logger.info("Start print log")
    logger.debug("Do something")
    logger.warning("Something maybe fail.")
    logger.warning("Something maybe fail.")
    logger.info("Finish")
    
main()        
```

2.使用TimedRotatingFileHandler，可实现日志按时间回滚

```python
import time
import logging
from logging.handlers import TimedRotatingFileHandler  # 设置日志按时间回滚
 
def backroll():
    # 日志打印格式
    log_fmt = '%(asctime)s\tFile \"%(filename)s\",line %(lineno)s\t%(levelname)s: %(message)s'
    formatter = logging.Formatter(log_fmt)
 
    # 创建TimedRotatingFileHandler对象
    log_file_handler = TimedRotatingFileHandler(filename="ds_update", when="M", interval=2, backupCount=2)
    log_file_handler.setFormatter(formatter)
    logging.basicConfig(level=logging.INFO)
 
    # 添加句柄
    log = logging.getLogger()
    log.addHandler(log_file_handler)
 
    # 循环打印日志
    log_content = "test log"
    count = 0
    while count < 30:
        log.error(log_content)
        time.sleep(0.1)
        count = count + 1
    log.removeHandler(log_file_handler)
 
backroll()
```

其中：

filename：日志文件名的prefix

when：是一个字符串，用于描述滚动周期的基本单位，字符串的值及意义如下：

- **“S”**: Seconds
- **“M”**: Minutes
- **“H”**: Hours
- **“D”**: Days
- **“W”**: Week day (0=Monday)
- **“midnight”**: Roll over at midnight
- **interval**: 滚动周期，单位有when指定，比如：when=’D’,interval=1，表示每天产生一个日志文件
- **backupCount**: 表示日志文件的保留个数

## 2.3 设置日志消息等级

可以设置不同的日志等级，用于控制日志的输出，只有级别大于或等于日志记录器指定级别的日志记录才会被输出，小于该级别的日志记录将会被丢弃：

| 日志等级 | 范围                                                         |
| -------- | ------------------------------------------------------------ |
| FATAL    | 致命错误                                                     |
| CRITICAL | 严重错误，导致应用程序不能继续运行时记录的信息，如内存耗尽、磁盘空间为空，一般很少使用 |
| ERROR    | 发生错误时，由于一个更严重的问题导致某些功能不能正常运行时记录的信息，如IO操作失败或者连接问题 |
| WARNING  | 当某些不期望的事情发生时记录的信息，但此时程序可以正常运行，如用户登录密码错误 |
| INFO     | 处理请求或者状态变化等日常事务，通常只记录关键节点信息，用于确认一切都是按照我们预期的那样进行工作 |
| DEBUG    | 调试过程中使用DEBUG等级，最详细的日志信息，如算法中每个循环的中间状态 |

## 2.4 捕获traceback

Python中的traceback模块被用于跟踪异常返回信息，可以在logging中记录下traceback，代码：

```python
import logging
 
logger = logging.getLogger(__name__)
logger.setLevel(level=logging.INFO)
handler = logging.FileHandler("log.txt")
handler.setLevel(logging.INFO)
formatter = logging.Formatter('%(asctime)s - %(name)s - %(levelname)s - %(message)s')
handler.setFormatter(formatter)
 
console = logging.StreamHandler()
console.setLevel(logging.INFO)
 
logger.addHandler(handler)
logger.addHandler(console)
 
def main():
    logger.info("Start print log")
    logger.debug("Do something")
    logger.warning("Something maybe fail.")
    try:
        open("sklearn.txt", "rb")
    except Exception:
        logger.error("Faild to open sklearn.txt from logger.error", exc_info=True) #logger.exception(msg,_args)等价于logger.error(msg,exc_info = True,_args)
    logger.info("Finish")
 
main()
```

输出log：

```python
2022-11-22 21:53:26,824 - __main__ - INFO - Start print log
2022-11-22 21:53:26,825 - __main__ - WARNING - Something maybe fail.
2022-11-22 21:53:26,825 - __main__ - ERROR - Faild to open sklearn.txt from logger.error
Traceback (most recent call last):
  File "/Users/xiongdejian/project/python_project/study/logging_module/捕获traceback.py", line 28, in main
    open("sklearn.txt", "rb")
IOError: [Errno 2] No such file or directory: 'sklearn.txt'
2022-11-22 21:53:26,825 - __main__ - INFO - Finish
```

## 2.5 多模块使用logging

主模块mainModule.py：

```python
import logging
import subModule
 
def main():
    logger = logging.getLogger("mainModule")  # 为logger起个名字
    logger.setLevel(level=logging.INFO)
    handler = logging.FileHandler("log.txt")
    handler.setLevel(logging.INFO)
    formatter = logging.Formatter('%(asctime)s - %(name)s - %(levelname)s - %(message)s')
    handler.setFormatter(formatter)
 
    console = logging.StreamHandler()
    console.setLevel(logging.INFO)
    console.setFormatter(formatter)
 
    logger.addHandler(handler)
    logger.addHandler(console)
 
    logger.info("creating an instance of subModule.subModuleClass")
    a = subModule.SubModuleClass()
    logger.info("calling subModule.subModuleClass.doSomething")
    a.doSomething()
    logger.info("done with  subModule.subModuleClass.doSomething")
    logger.info("calling subModule.some_function")
    subModule.som_function()
    logger.info("done with subModule.some_function")
 
main()
```

子模块subModule.py：

```python
import logging
module_logger = logging.getLogger("mainModule.sub")  # mainModule logger子类
 
class SubModuleClass(object):
    def __init__(self):
        self.logger = logging.getLogger("mainModule.sub.module")
        self.logger.info("creating an instance in SubModuleClass")
 
    def doSomething(self):
        self.logger.info("do something in SubModule")
        a = []
        a.append(1)
        self.logger.debug("list a = " + str(a))
        self.logger.info("finish something in SubModuleClass")
 
def som_function():
    module_logger.info("call function some_function")
```

执行之后，在控制和日志文件log.txt中输出：

```python
2022-11-22 21:55:59,240 - mainModule - INFO - creating an instance of subModule.subModuleClass
2022-11-22 21:55:59,240 - mainModule.sub.module - INFO - creating an instance in SubModuleClass
2022-11-22 21:55:59,240 - mainModule - INFO - calling subModule.subModuleClass.doSomething
2022-11-22 21:55:59,240 - mainModule.sub.module - INFO - do something in SubModule
2022-11-22 21:55:59,240 - mainModule.sub.module - INFO - finish something in SubModuleClass
2022-11-22 21:55:59,240 - mainModule - INFO - done with  subModule.subModuleClass.doSomething
2022-11-22 21:55:59,241 - mainModule - INFO - calling subModule.some_function
2022-11-22 21:55:59,241 - mainModule.sub - INFO - call function some_function
2022-11-22 21:55:59,241 - mainModule - INFO - done with subModule.some_function
```

​	首先在主模块定义了logger'mainModule'，并对它进行了配置，就可以在解释器进程里面的其他地方通过getLogger('mainModule')得到的对象都是一样的，不需要重新配置，可以直接使用。定义的该logger的子logger，都可以共享父logger的定义和配置，所谓的父子logger是通过命名来识别，任意以'mainModule'开头的logger都是它的子logger，例如'mainModule.sub'。

​	实际开发一个application，首先可以通过logging配置文件编写好这个application所对应的配置，可以生成一个根logger，如'PythonAPP'，然后在主函数中通过fileConfig加载logging配置，接着在application的其他地方、不同的模块中，可以使用根logger的子logger，如'PythonAPP.Core'，'PythonAPP.Web'来进行log，而不需要反复的定义和配置各个模块的logger。

# 3.通过JSON或者YAML文件配置logging模块

​	尽管可以在Python代码中配置logging，但是这样并不够灵活，最好的方法是使用一个配置文件来配置。在Python 2.7及以后的版本中，可以从字典中加载logging配置，也就意味着可以通过JSON或者YAML文件加载日志的配置。

## 3.1 通过json文件配置

JSON配置文件：

```json
{
    "version": 1,
    "disable_existing_loggers": false,
    "formatters": {
        "simple": {
            "format": "%(asctime)s - %(name)s - %(levelname)s - %(message)s"
        }
    },
    "handlers": {
        "console": {
            "class": "logging.StreamHandler",
            "level": "DEBUG",
            "formatter": "simple",
            "stream": "ext://sys.stdout"
        },
        "info_file_handler": {
            "class": "logging.handlers.RotatingFileHandler",
            "level": "INFO",
            "formatter": "simple",
            "filename": "info.log",
            "maxBytes": "10485760",
            "backupCount": 20,
            "encoding": "utf8"
        },
        "error_file_handler": {
            "class": "logging.handlers.RotatingFileHandler",
            "level": "ERROR",
            "formatter": "simple",
            "filename": "errors.log",
            "maxBytes": 10485760,
            "backupCount": 20,
            "encoding": "utf8"
        }
    },
    "loggers": {
        "my_module": {
            "level": "ERROR",
            "handlers": ["info_file_handler"],
            "propagate": "no"
        }
    },
    "root": {
        "level": "INFO",
        "handlers": ["console", "info_file_handler", "error_file_handler"]
    }
}
```

通过JSON加载配置文件，然后通过logging.dictConfig配置logging：

```python
import json
import logging.config
import os
 
def setup_logging(default_path="logging.json", default_level=logging.INFO, env_key="LOG_CFG"):
    path = default_path
    value = os.getenv(env_key, None)
    if value:
        path = value
    if os.path.exists(path):
        with open(path, "r") as f:
            config = json.load(f)
            logging.config.dictConfig(config)
    else:
        logging.basicConfig(level=default_level)
 
def func():
    logging.info("start func")
    logging.info("exec func")
    logging.info("end func")
 
if __name__ == "__main__":
    setup_logging(default_path="logging.json")
    func()
```

## 3.2 通过YAML配置

YAML配置文件：

```yaml
version: 1
disable_existing_loggers: False
formatters:
        simple:
            format: "%(asctime)s - %(name)s - %(levelname)s - %(message)s"
handlers:
    console:
            class: logging.StreamHandler
            level: DEBUG
            formatter: simple
            stream: ext://sys.stdout
    info_file_handler:
            class: logging.handlers.RotatingFileHandler
            level: INFO
            formatter: simple
            filename: info.log
            maxBytes: 10485760
            backupCount: 20
            encoding: utf8
    error_file_handler:
            class: logging.handlers.RotatingFileHandler
            level: ERROR
            formatter: simple
            filename: errors.log
            maxBytes: 10485760
            backupCount: 20
            encoding: utf8
loggers:
    my_module:
            level: ERROR
            handlers: [info_file_handler]
            propagate: no
root:
    level: INFO
    handlers: [console,info_file_handler,error_file_handler]

```

​	通过YAML加载配置文件，通过`logging.dictConfig`配置logging。如下，就可以在运行程序的时候调用`setup_logging`来启动日志记录了。它默认会读取`logging.json`或`logging.yaml`文件

```python
import yaml
import logging.config
import os

def setup_logging(default_path = "logging.yaml",default_level = logging.INFO,env_key = "LOG_CFG"):
    path = default_path
    value = os.getenv(env_key,None)
    if value:
        path = value
    if os.path.exists(path):
        with open(path,"r") as f:
            config = yaml.load(f)
            logging.config.dictConfig(config)
    else:
        logging.basicConfig(level = default_level)

def func():
    logging.info("start func")

    logging.info("exec func")

    logging.info("end func")

if __name__ == "__main__":
    setup_logging(default_path = "logging.yaml")
    func()
```



# 4. 扩展阅读

1.logging日志中formatter

```python
# 定义handler的输出格式
formatter = logging.Formatter('%(asctime)s - %(name)s - %(levelname)s - %(message)s')
fh.setFormatter(formatter)
```

2.logging子模块

# 5. 配置实例

1.常规配置：文本，Print

```python
import os
import logging
import datetime
 
__all__ = ["log_init"]
 
# log 配置参数
"""
level配置：
    线上环境配为ERROR或者WARNING；收集运行信息，则配为INFO。
    线下测试配为INFO、DEBUG。
format_str配置：
    分隔符： -
    时间：%(asctime)s
    级别：%(levelname)s
    发生位置：%(name)s - %(funcName)s - %(lineno)d
    事件内容：%(message)s
"""
 
config = {
    "console": False,  # 是否在控制台显示
    "level": logging.ERROR,  # INFO < WARNING < ERROR，
    "format_str": "%(asctime)s - %(levelname)s - %(name)s - %(funcName)s - %(lineno)d - %(message)s",
}
 
 
def get_file_name():
    log_dir = "./log"
    if not os.path.exists(log_dir):
        os.makedirs(log_dir)
 
    pid = os.getpid()
    cur_date = datetime.date.today() - datetime.timedelta(days=0)
    filename = '%s/log_%s_%s.log' % (log_dir, cur_date, str(pid))
    return filename
 
 
def log_init():
    logger = logging.getLogger()
    logger.setLevel(level=config["level"])
    formatter = logging.Formatter(config["format_str"])
 
    filename = get_file_name()
    file_handler = logging.FileHandler(filename)
    file_handler.setLevel(level=config["level"])
    file_handler.setFormatter(formatter)
    logger.addHandler(file_handler)
 
    if config["console"]:
        console_handler = logging.StreamHandler()
        console_handler.setLevel(level=config["level"])
        console_handler.setFormatter(formatter)
        logger.addHandler(console_handler)
 
    logger.info("logging config ok.")
```

2.常规配置，仅在控制台输出

```python
import logging
 
def log_init(level=logging.DEBUG):
    logger = logging.getLogger()
    logger.setLevel(level=level)
    formatter = logging.Formatter("%(levelname)s - %(lineno)d - %(message)s")
 
    console_handler = logging.StreamHandler()
    console_handler.setLevel(level=level)
    console_handler.setFormatter(formatter)
    logger.addHandler(console_handler)
 
    logger.info("logging config ok.")
 
log_init(level=logging.DEBUG)
```



