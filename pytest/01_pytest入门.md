# 01 pytest入门


## 一、Pytest快速入门和基础讲解

### 前言

- 目前有两种纯测试的测试框架，pytest和unittest
- unittest应该是广为人知，而且也是老框架了，很多人都用来做自动化，无论是UI还是接口
- pytest是基于unittest开发的另一款更高级更好用的单元测试框架
- 出去面试也好，跟别人说起来也好，pytest的逼格明显高于unittest

 

### 为什么要用Pytest

#### pytest 的官方网站介绍，它具有如下特点：

1. 非常容易上手，入门简单，文档丰富，文档中有很多实例可以参考
2. 能够支持简单的单元测试和复杂的功能测试
3. 支持参数化
4. 执行测试过程中可以将某些测试跳过（skip），或者对某些预期失败的case标记成失败
5. 支持重复执行(return)失败的 case
6. 支持运行由 nose, unittest 编写的测试 case
7. 可生成 html 报告
8. 方便的和持续集成工具 jenkins 集成
9. 可支持执行部分用例
10. 具有很多第三方插件，并且可以自定义扩展

### 安装Pytest

#### cmd运行



```
pip install -U pytest

pip3 install pytest -i http://pypi.douban.com/simple/ --trusted-host pypi.douban.com
```

#### 查看版本

```
pytest --version
```

 

### 快速开始

```python
# -*- coding: utf-8 -*-


def func(x):
    return x + 1


def test_answer():
    assert func(3) == 5


class TestClass:
    def test_one(self):
        x = "this"
        assert "h" in x

    def test_two(self):
        x = "hello"
        assert hasattr(x, "check")
```

然后，cmd进入当前文件目录，直接执行

```python
pytest
```

#### 知识点

- 如果只执行 pytest ，会查找当前目录及其子目录下以 test_*.py 或 *_test.py 文件，找到文件后，在文件中找到以  test 开头函数并执行
- 如果只想执行某个文件，可以 pytest start.py 
- 加上-q，就是显示简单的结果： pytest -q start.py 

### Pytest用例的设计原则

用Pytest写用例时候，一定要按照下面的规则去写，否则不符合规则的测试用例是不会执行的

- 文件名以 test_*.py 文件和*_test.py
- 以  test_ 开头的函数
- 以  Test 开头的类，不能包含 __init__ 方法
- 以  test_ 开头的类里面的方法
- 所有的包 package 必须要有__init__.py 文件

### Pytest执行用例规则

**注意，下面讲的都是在cmd中执行pytest命令**

#### 1、某个目录下所有的用例



```python
pytest
```

 

#### 2、执行某一个 py 文件下用例 



```python
pytest 脚本名称.py
```

　　

#### 3、运行start.py 模块里面的某个函数，或者某个类，某个类里面的方法

加v和不加-v都可以，加-v的话，打印的信息更详细

```python
pytest -v 08_mark.py::TestClass::test_method

pytest 08_mark.py::TestClass::test_method

pytest start.py::test_answer
```

 

#### 4、运行start.py 模块里面,测试类里面的某个方法



```python
pytest start.py::TestClass::test_two
```

 

#### 5、-m 标记表达式（后续讲解）



```python
 pytest -m login
```

将运行用 @pytest.mark.login 装饰器修饰的所有测试，后面再展开讲标记哦

 

#### 6、-q 简单打印，只打印测试用例的执行结果



```python
 pytest -q start.py
```

 

#### 7、-s 详细打印



```python
 pytest -s start.py
```

 

#### 8、-x 遇到错误时停止测试



```python
pytest start.py -x
```

 

#### 9、—maxfail=num，当用例错误个数达到指定数量时，停止测试



```python
pytest start.py --maxfail=1
```

 

#### 10、-k 匹配用例名称

执行测试用例名称包含http的所有用例



```python
pytest -s -k http start.py
```

 

#### 11、-k 根据用例名称排除某些用例



```python
1 pytest -s -k "not http" start.py
```

 

#### 12、-k 同时匹配不同的用例名称



```python
pytest -s -k "method or weibo" start.py
```

- 与unittest不同，pytest使用的是python自带的assert关键字来进行断言
- assert关键字后面可以接一个表达式，只要表达式的最终结果为True，那么断言通过，用例执行成功，否则用例执行失败 

## 二、assert断言详细使用

### 前言

- 与unittest不同，pytest使用的是python自带的assert关键字来进行断言
- assert关键字后面可以接一个表达式，只要表达式的最终结果为True，那么断言通过，用例执行成功，否则用例执行失败

### assert小栗子

想在抛出异常之后输出一些提示信息，执行之后就方便查看是什么原因了



```
# 异常信息
def f():
    return 3
def test_function():
    a = f()
    assert a % 2 == 0, "判断 a 为偶数，当前 a 的值为：%s" % a
```

#### 执行结果

![img](../../../blog/source/picture/1896874-20200406130902312-172338387.png)

 

### 常用断言

pytest 里面断言实际上就是 python 里面的 assert 断言方法，常用的有以下几种

- assert xx ：判断 xx 为真
- assert not xx ：判断 xx 不为真
- assert a in b ：判断 b 包含 a
- assert a == b ：判断 a 等于 b
- assert a != b ：判断 a 不等于 b

 

### 异常断言

可以使用 pytest.raises 作为上下文管理器，当抛出异常时可以获取到对应的异常实例



```
# 断言异常
def test_zero_division():
    with pytest.raises(ZeroDivisionError):
        1 / 0
```

 

**断言场景：**断言它抛的异常是不是预期想要的

**代码执行：**1/0

**预期结果：**抛的异常是ZeroDivisionError: division by zero

**如何断言：**通常是断言异常的 type 和 value 值了

**具体方式：**这里 1/0 的异常类型是 ZeroDivisionError，异常的 value 值是 divisionby zero



```
# 详细断言异常
def test_zero_division_long():
    with pytest.raises(ZeroDivisionError) as excinfo:
        1 / 0

    # 断言异常类型 type
    assert excinfo.type == ZeroDivisionError
    # 断言异常 value 值
    assert "division by zero" in str(excinfo.value)
```

excinfo ：是一个异常信息实例

**主要属性：** .type 、 .value 、 .traceback 

**注意：**断言 type 的时候，异常类型是不需要加引号的，断言 value值的时候需转 str

 

### 拓展一：match

可以将 match 关键字参数传递给上下文管理器，以测试正则表达式与异常的字符串表示形式是否匹配

**注意：**这种方法只能断言value，不能断言type



```python
# 自定义消息
def test_zero_division_long():
    with pytest.raises(ZeroDivisionError, match=".*zero.*") as excinfo:
        1 / 0
```

该 match 方法的regexp参数与 re.search 函数匹配，因此在上面的示例中 match='zero' 也可以使用

![img](../../../blog/source/picture/1896874-20200406135355943-1838438116.png)

![img](../../../blog/source/picture/1896874-20200406135407511-1382697695.png)

 

### 拓展二：检查断言装饰器



```python
# 断言装饰器
@pytest.mark.xfail(raises=ZeroDivisionError)
def test_f():
    1 / 0
```

#### 执行结果

#### ![img](../../../blog/source/picture/1896874-20200406140623462-429848886.png)

#### 知识点

- 代码抛出异常，但是和raises指定的异常类相匹配，所以不会断言失败
- 它相当于一个**检查异常装饰器** ,**功能：**检查是否有异常，不确定是否有异常
- with pytest.raise(ZeroDivisionError) 对于故意测试异常代码的情况，使用可能会更好
- `而@pytest.mark.xfail(raises=ZeroDivisionError) `对于检查未修复的错误（即，可能会发生异常），使用检查断言可能会更好

## 三、setup和teardown的详细使用 

### 前言

用过unittest的童鞋都知道，有两个前置方法，两个后置方法；分别是

- setup()
- setupClass()
- teardown()
- teardownClass()

Pytest也贴心的提供了类似setup、teardown的方法，并且还超过四个，一共有十种

- **模块级别：**setup_module、teardown_module
- **函数级别：**setup_function、teardown_function，不在类中的方法
- **类级别：**setup_class、teardown_class
- **方法级别：**setup_method、teardown_method
- **方法细化级别：**setup、teardown

 

### 代码

用过unittest的童鞋，对这个前置、后置方法应该不陌生了，我们直接来看代码和运行结果



```python
#!/usr/bin/env python
# -*- coding: utf-8 -*-

import pytest


def setup_module():
    print("=====整个.py模块开始前只执行一次:打开浏览器=====")


def teardown_module():
    print("=====整个.py模块结束后只执行一次:关闭浏览器=====")


def setup_function():
    print("===每个函数级别用例开始前都执行setup_function===")


def teardown_function():
    print("===每个函数级别用例结束后都执行teardown_function====")


def test_one():
    print("one")


def test_two():
    print("two")


class TestCase():
    def setup_class(self):
        print("====整个测试类开始前只执行一次setup_class====")

    def teardown_class(self):
        print("====整个测试类结束后只执行一次teardown_class====")

    def setup_method(self):
        print("==类里面每个用例执行前都会执行setup_method==")

    def teardown_method(self):
        print("==类里面每个用例结束后都会执行teardown_method==")

    def setup(self):
        print("=类里面每个用例执行前都会执行setup=")

    def teardown(self):
        print("=类里面每个用例结束后都会执行teardown=")

    def test_three(self):
        print("three")
    
    def test_four(self):
        print("four")


if __name__ == '__main__':
    pytest.main(["-q", "-s", "-ra", "setup_teardown.py"])
```



#### 执行结果

![img](../../../blog/source/picture/1896874-20200406142633570-607433520.png)

## 四、fixture的详细使用

### 前言

- 前面一篇讲了setup、teardown可以实现在执行用例前或结束后加入一些操作，但这种都是针对整个脚本全局生效的
- 如果有以下场景：用例 1 需要先登录，用例 2 不需要登录，用例 3 需要先登录。很显然无法用 setup 和 teardown 来实现了
- fixture可以让我们自定义测试用例的前置条件

 

### fixture的优势

- 命名方式灵活，不局限于 setup 和teardown 这几个命名
- conftest.py 配置里可以实现数据共享，不需要 import 就能自动找到fixture
- scope="module" 可以实现多个.py 跨文件共享前置
- scope="session" 以实现多个.py 跨文件使用一个 session 来完成多个用例

 

### fixture参数列表



```python
@pytest.fixture(scope="function", params=None, autouse=False, ids=None, name=None)
def test():
    print("fixture初始化的参数列表")
```

#### 参数列表

- scope：可以理解成fixture的作用域，默认：function，还有class、module、package、session四个**【常用】**
- autouse：默认：False，需要用例手动调用该fixture；如果是True，所有作用域内的测试用例都会自动调用该fixture
- name：默认：装饰器的名称，同一模块的fixture相互调用建议写个不同的name

#### 注意

session的作用域：是整个测试会话，即开始执行pytest到结束测试

 

### 测试用例如何调用fixture

1. 将fixture名称作为测试用例函数的输入参数
2. 测试用例加上装饰器：@pytest.mark.usefixtures(fixture_name)
3. fixture设置autouse=True



```python
#!/usr/bin/env python
# -*- coding: utf-8 -*-

import pytest

# 调用方式一
@pytest.fixture
def login():
    print("输入账号，密码先登录")


def test_s1(login):
    print("用例 1：登录之后其它动作 111")


def test_s2():  # 不传 login
    print("用例 2：不需要登录，操作 222")


# 调用方式二
@pytest.fixture
def login2():
    print("please输入账号，密码先登录")


@pytest.mark.usefixtures("login2", "login")
def test_s11():
    print("用例 11：登录之后其它动作 111")


# 调用方式三
@pytest.fixture(autouse=True)
def login3():
    print("====auto===")


# 不是test开头，加了装饰器也不会执行fixture
@pytest.mark.usefixtures("login2")
def loginss():
    print(123)
```

#### 执行结果

![img](../../../blog/source/picture/1896874-20200406152931436-955648196.png)

#### 知识点

- 在类声明上面加 @pytest.mark.usefixtures() ，代表这个类里面所有测试用例都会调用该fixture
- 可以叠加多个 @pytest.mark.usefixtures() ，先执行的放底层，后执行的放上层
- 可以传多个fixture参数，先执行的放前面，后执行的放后面
- 如果fixture有返回值，用 @pytest.mark.usefixtures() 是无法获取到返回值的，必须用传参的方式（方式一）

 

### fixture的实例化顺序

- 较高 scope 范围的fixture（session）在较低 scope 范围的fixture（ function 、 class ）之前实例化**【session > package > module > class > function】**
- 具有相同作用域的fixture遵循测试函数中声明的顺序，并遵循fixture之间的依赖关系**【在fixture_A里面依赖的fixture_B优先实例化，然后到fixture_A实例化】**
- 自动使用（autouse=True）的fixture将在显式使用（传参或装饰器）的fixture之前实例化



```python
#!/usr/bin/env python
# -*- coding: utf-8 -*-

import pytest

order = []

@pytest.fixture(scope="session")
def s1():
    order.append("s1")


@pytest.fixture(scope="module")
def m1():
    order.append("m1")


@pytest.fixture
def f1(f3, a1):
    # 先实例化f3, 再实例化a1, 最后实例化f1
    order.append("f1")
    assert f3 == 123


@pytest.fixture
def f3():
    order.append("f3")
    a = 123
    yield a


@pytest.fixture
def a1():
    order.append("a1")


@pytest.fixture
def f2():
    order.append("f2")


def test_order(f1, m1, f2, s1):
    # m1、s1在f1后，但因为scope范围大，所以会优先实例化
    assert order == ["s1", "m1", "f3", "a1", "f1", "f2"]
```

#### 执行结果

断言成功

 

### 关于fixture的注意点

添加了 @pytest.fixture ，如果fixture还想依赖其他fixture，需要用函数传参的方式，不能用 @pytest.mark.usefixtures() 的方式，否则会不生效



```python
@pytest.fixture(scope="session")
def open():
    print("===打开浏览器===")

@pytest.fixture
# @pytest.mark.usefixtures("open") 不可取！！！不生效！！！
def login(open):
    # 方法级别前置操作setup
    print(f"输入账号，密码先登录{open}")
```

 

**前面讲的，其实都是setup的操作，那么现在就来讲下teardown是怎么实现的**

 

### fixture之yield实现teardown

用fixture实现teardown并不是一个独立的函数，而是用 yield 关键字来开启teardown操作



```python
#!/usr/bin/env python
# -*- coding: utf-8 -*-

import pytest


@pytest.fixture(scope="session")
def open():
    # 会话前置操作setup
    print("===打开浏览器===")
    test = "测试变量是否返回"
    yield test
    # 会话后置操作teardown
    print("==关闭浏览器==")


@pytest.fixture
def login(open):
    # 方法级别前置操作setup
    print(f"输入账号，密码先登录{open}")
    name = "==我是账号=="
    pwd = "==我是密码=="
    age = "==我是年龄=="
    # 返回变量
    yield name, pwd, age
    # 方法级别后置操作teardown
    print("登录成功")


def test_s1(login):
    print("==用例1==")
    # 返回的是一个元组
    print(login)
    # 分别赋值给不同变量
    name, pwd, age = login
    print(name, pwd, age)
    assert "账号" in name
    assert "密码" in pwd
    assert "年龄" in age


def test_s2(login):
    print("==用例2==")
    print(login)
```

#### yield注意事项

- 如果yield前面的代码，即setup部分已经抛出异常了，则不会执行yield后面的teardown内容
- 如果测试用例抛出异常，yield后面的teardown内容还是会正常执行

### yield+with的结合

```python
# 官方例子
@pytest.fixture(scope="module")
def smtp_connection():
    with smtplib.SMTP("smtp.gmail.com", 587, timeout=5) as smtp_connection:
        yield smtp_connection  # provide the fixture value
```

该 smtp_connection 连接将测试完成执行后已经关闭，因为 smtp_connection 对象自动关闭时， with 语句结束。

### addfinalizer 终结函数

```python
@pytest.fixture(scope="module")
def test_addfinalizer(request):
    # 前置操作setup
    print("==再次打开浏览器==")
    test = "test_addfinalizer"

    def fin():
        # 后置操作teardown
        print("==再次关闭浏览器==")

    request.addfinalizer(fin)
    # 返回前置操作的变量
    return test


def test_anthor(test_addfinalizer):
    print("==最新用例==", test_addfinalizer)
```

#### 注意事项

- 如果 request.addfinalizer() 前面的代码，即setup部分已经抛出异常了，则不会执行 request.addfinalizer() 的teardown内容（和yield相似，应该是最近新版本改成一致了）
- 可以声明多个终结函数并调用

## 五、测试用例执行后的几种状态

### 用例执行状态

用例执行完成后，每条用例都有自己的状态，常见的状态有

- passed：测试通过
- failed：断言失败
- error：用例本身写的质量不行，本身代码报错（譬如：fixture不存在，fixture里面有报错）
- xfail：预期失败，加了 @pytest.mark.xfail() 

 

### error的例子一：参数不存在



```python
def pwd():
    print("获取用户名")
    a = "yygirl"
    assert a == "yygirl123"


def test_1(pwd):
    assert user == "yygirl"
```

pwd参数并不存在，所以用例执行error

 

### error的例子二：fixture有错



```python
@pytest.fixture()
def user():
    print("获取用户名")
    a = "yygirl"
    assert a == "yygirl123"
    return a


def test_1(user):
    assert user == "yygirl"
```

- fixture里面断言失败，所以fixture会报错；

- 因为test_1调用了错误的fixture，所以error表示用例有问题

 

### failed的例子一：断言失败



```
@pytest.fixture()
def pwd():
    print("获取密码")
    a = "yygirl"
    return a


def test_2(pwd):
    assert pwd == "yygirl123"
```

fixture返回的变量断言失败

 

### failed的例子二：执行期间抛出了异常



```
@pytest.fixture()
def pwd():
    print("获取密码")
    a = "polo"
    return a


def test_2(pwd):
    raise NameError
    assert pwd == "polo"
```

因为用例执行期间抛出了异常

  

### xfail的例子：代码有异常，且和raised的异常类匹配



```
# 断言装饰器
@pytest.mark.xfail(raises=ZeroDivisionError)
def test_f():
    1 / 0
```

代码有异常，且和raised的异常类匹配，所以是xfail**（算测试通过的一种，表示符合期望捕捉到的异常）**，并不算failed

如果和raised的异常类不匹配，则是failed

### 总结

- 测试用例的代码有异常，包括主动抛出异常或代码有异常，都算failed
- 当测试用例调用的fixture有异常，或传入的参数有异常的时候，都算error
- 如果一份测试报告中，error的测试用例数量越多，说明测试用例质量越差

## 六、conftest.py的详细讲解

### 什么是conftest.py

可以理解成一个专门存放fixture的配置文件

 

### 实际开发场景

多个测试用例文件（test_*.py）的所有用例都需要**用登录功能来作为前置操作**，那就不能把登录功能写到某个用例文件中去了

 

### 如何解决上述场景问题？

conftest.py的出现，就是为了解决上述问题，单独管理一些全局的fixture

 

### conftest.py配置fixture注意事项

- pytest会默认读取conftest.py里面的所有fixture
- conftest.py 文件名称是固定的，不能改动
- conftest.py只对同一个package下的所有测试用例生效
- 不同目录可以有自己的conftest.py，一个项目中可以有多个conftest.py
- 测试用例文件中不需要手动import conftest.py，pytest会自动查找

 

## 实际项目中的小案例

这是一个目录

![img](../../../blog/source/picture/1896874-20200408224205809-596157978.png)

 

### 06conftest目录下

#### conftest.py代码

#### 最顶层的conftest，一般写全局的fixture，在Web UI自动化中，可能会初始化driver

```python
#!/usr/bin/env python
# -*- coding: utf-8 -*-

import pytest

@pytest.fixture(scope="session")
def login():
    print("====登录功能，返回账号，token===")
    name = "testyy"
    token = "npoi213bn4"
    yield name, token
    print("====退出登录！！！====")


@pytest.fixture(autouse=True)
def get_info(login):
    name, token = login
    print(f"== 每个用例都调用的外层fixture：打印用户token： {token} ==")
```

 

#### test_1.py代码

#### 同级目录下的第一条测试用例

```python
def test_get_info(login):
    name, token = login
    print("***基础用例：获取用户个人信息***")
    print(f"用户名:{name}, token:{token}")
```

 

#### 06_run.py代码

#### 运行06conftest目录下所有测试用例

```python
import pytest

if __name__ == '__main__':
    pytest.main(["-s", "../06conftest/"])
```

 

### test_51job目录下

#### conftest.py代码

#### 配置一些针对51job这个网站的测试用例独有的fixture，譬如：打开51job网站

```python
import pytest

@pytest.fixture(scope="module")
def open_51(login):
    name, token = login
    print(f"###用户 {name} 打开51job网站###")
```

 

#### test_case1.py代码

#### 某个功能模块下的测试用例

```python
def test_case2_01(open_51):
    print("51job，列出所有职位用例")


def test_case2_02(open_51):
    print("51job，找出所有python岗位")
```

 

### test_toutiao目录下

#### test_case1.py代码

包没有__init__.py文件也没有conftest.py文件

```python
def test_no_fixture(login):
    print("==没有__init__测试用例，我进入头条了==", login)
```

 

### test_weibo目录下

#### conftest.py代码

#### 配置一些针对weibo这个网站的测试用例独有的fixture，譬如：打开weibo网站

```python
import pytest

@pytest.fixture(scope="function")
def open_weibo(login):
    name, token = login
    print(f"&&& 用户 {name} 返回微博首页 &&&")
```



#### test_case1.py代码

#### 某个功能模块下的测试用例

```python
class TestWeibo:
    def test_case1_01(self, open_weibo):
        print("查看微博热搜")

    def test_case1_02(self, open_weibo):
        print("查看微博范冰冰")
```

 

### 运行06_run.py的结果

![img](../../../blog/source/picture/1896874-20200408224906333-993246421.png)

##  七、skip、skipif跳过用例

### 前言

- pytest.mark.skip 可以标记无法在某些平台上运行的测试功能，或者您希望失败的测试功能
- 希望满足某些条件才执行某些测试用例，否则pytest会跳过运行该测试用例
- 实际常见场景：跳过非Windows平台上的仅Windows测试，或者跳过依赖于当前不可用的外部资源（例如数据库）的测试

 

### @pytest.mark.skip

跳过执行测试用例，有可选参数reason：跳过的原因，会在执行结果中打印



```python
#!/usr/bin/env python
# -*- coding: utf-8 -*-

import pytest


@pytest.fixture(autouse=True)
def login():
    print("====登录====")


def test_case01():
    print("我是测试用例11111")


@pytest.mark.skip(reason="不执行该用例！！因为没写好！！")
def test_case02():
    print("我是测试用例22222")


class Test1:

    def test_1(self):
        print("%% 我是类测试用例1111 %%")

    @pytest.mark.skip(reason="不想执行")
    def test_2(self):
        print("%% 我是类测试用例2222 %%")


@pytest.mark.skip(reason="类也可以跳过不执行")
class TestSkip:
    def test_1(self):
        print("%% 不会执行 %%")
```

#### 执行结果

![img](../../../blog/source/picture/1896874-20200409140037005-2099522412.png)

#### 知识点

- @pytest.mark.skip 可以加在函数上，类上，类方法上
- 如果加在类上面，类里面的所有测试用例都不会执行
- 以上小案例都是针对：整个测试用例方法跳过执行，**如果想在测试用例执行期间跳过不继续往下执行呢？**

 

### pytest.skip()函数基础使用

**作用：**在测试用例执行期间强制跳过不再执行剩余内容

**类似：**在Python的循环里面，满足某些条件则break 跳出循环



```python
def test_function():
    n = 1
    while True:
        print(f"这是我第{n}条用例")
        n += 1
        if n == 5:
            pytest.skip("我跑五次了不跑了")
```

#### 执行结果

![img](https://img2020.cnblogs.com/blog/1896874/202004/1896874-20200409140933987-1689454456.png)

 

### pytest.skip(msg="",allow_module_level=False)

当 allow_module_level=True 时，可以设置在模块级别跳过整个模块

```python
#!/usr/bin/env python
# -*- coding: utf-8 -*-
import sys
import pytest

if sys.platform.startswith("win"):
    pytest.skip("skipping windows-only tests", allow_module_level=True)


@pytest.fixture(autouse=True)
def login():
    print("====登录====")


def test_case01():
    print("我是测试用例11111")
```

#### 执行结果



```python
collecting ... 
Skipped: skipping windows-only tests
collected 0 items / 1 skipped

============================= 1 skipped in 0.15s ==============================
```

 

### @pytest.mark.skipif(condition, reason="")

**作用：**希望有条件地跳过某些测试用例

**注意：**condition需要返回True才会跳过



```python
@pytest.mark.skipif(sys.platform == 'win32', reason="does not run on windows")
class TestSkipIf(object):
    def test_function(self):
        print("不能在window上运行")
```

#### 执行结果



```python
collecting ... collected 1 item

07skip_sipif.py::TestSkipIf::test_function SKIPPED                       [100%]
Skipped: does not run on windows


============================= 1 skipped in 0.04s ==============================
```

 

### 跳过标记

- 可以将 pytest.mark.skip 和 pytest.mark.skipif 赋值给一个标记变量
- 在不同模块之间共享这个标记变量
- 若有多个模块的测试用例需要用到相同的 skip 或 skipif ，可以用一个单独的文件去管理这些通用标记，然后适用于整个测试用例集



```python
# 标记
skipmark = pytest.mark.skip(reason="不能在window上运行=====")
skipifmark = pytest.mark.skipif(sys.platform == 'win32', reason="不能在window上运行啦啦啦=====")


@skipmark
class TestSkip_Mark(object):

    @skipifmark
    def test_function(self):
        print("测试标记")

    def test_def(self):
        print("测试标记")


@skipmark
def test_skip():
    print("测试标记")
```

#### 执行结果



```python
collecting ... collected 3 items

07skip_sipif.py::TestSkip_Mark::test_function SKIPPED                    [ 33%]
Skipped: 不能在window上运行啦啦啦=====

07skip_sipif.py::TestSkip_Mark::test_def SKIPPED                         [ 66%]
Skipped: 不能在window上运行=====

07skip_sipif.py::test_skip SKIPPED                                       [100%]
Skipped: 不能在window上运行=====


============================= 3 skipped in 0.04s ==============================
```

 

## pytest.importorskip( modname: str, minversion: Optional[str] = None, reason: Optional[str] = None )

**作用：**如果缺少某些导入，则跳过模块中的所有测试

**参数列表**

- modname：模块名
- minversion：版本号
- reason：跳过原因，默认不给也行



```python
pexpect = pytest.importorskip("pexpect", minversion="0.3")


@pexpect
def test_import():
    print("test")
```

 

#### 执行结果一：如果找不到module



```python
Skipped: could not import 'pexpect': No module named 'pexpect'
collected 0 items / 1 skipped
```

 

#### 执行结果一：如果版本对应不上



```python
Skipped: module 'sys' has __version__ None, required is: '0.3'
collected 0 items / 1 skipped
```

## 八、使用自定义标记mark

### 前言

- pytest 可以支持自定义标记，自定义标记可以把一个 web 项目划分多个模块，然后指定模块名称执行
- 譬如我可以标明哪些用例是window下执行的，哪些用例是mac下执行的，在运行代码时候指定mark即可

 

### 上代码



```python
#!/usr/bin/env python
# -*- coding: utf-8 -*-
import pytest


@pytest.mark.weibo
def test_weibo():
    print("测试微博")


@pytest.mark.toutiao
def test_toutiao():
    print("测试头条")


@pytest.mark.toutiao
def test_toutiao1():
    print("再次测试头条")


@pytest.mark.xinlang
class TestClass:
    def test_method(self):
        print("测试新浪")


def testnoMark():
    print("没有标记测试")
```

 

#### cmd敲运行命令



```python
pytest -s -m weibo 08_mark.py
```

 

#### 执行结果

![img](../../../blog/source/picture/1896874-20200409213339765-380395952.png)

 

#### 如何避免warnings

- 创建一个pytest.ini文件**（后续详解）**
- 加上自定义mark，如下图
- **注意：**pytest.ini需要和运行的测试用例同一个目录，或在根目录下作用于全局

![img](../../../blog/source/picture/1896874-20200409213514482-434242000.png)

 

#### 如果不想标记weibo的用例，我们直接取反即可



```python
pytest -s -m "not weibo" 08_mark.py
```

![img](../../../blog/source/picture/1896874-20200410132234930-1457001816.png)

 

#### 如果想执行多个自定义标记的用例



```python
pytest -s -m "toutiao or weibo" 08_mark.py
```

![img](../../../blog/source/picture/1896874-20200410132231668-571418711.png)

## 九、参数化@pytest.mark.parametrize

### 前言

pytest允许在多个级别启用测试参数化：

-  pytest.fixture() 允许fixture有参数化功能（后面讲解）
-  @pytest.mark.parametrize 允许在测试函数或类中定义多组参数和fixtures
-  pytest_generate_tests 允许定义自定义参数化方案或扩展（拓展）

 

### 参数化场景

只有测试数据和期望结果不一样，但操作步骤是一样的测试用例可以用上参数化；

可以看看下面的栗子

 

### 未参数化的代码



```python
def test_1():
    assert 3 + 5 == 9


def test_2():
    assert 2 + 4 == 6


def test_3():
    assert 6 * 9 == 42
```

可以看到，三个用例都是加法然后断言某个值，重复写三个类似的用例有点冗余

 

### 利用参数化优化之后的代码



```python
@pytest.mark.parametrize("test_input,expected", [("3+5", 8), ("2+4", 6), ("6*9", 42)])
def test_eval(test_input, expected):
    print(f"测试数据{test_input},期望结果{expected}")
    assert eval(test_input) == expected
```

 

##### 执行结果

![img](../../../blog/source/picture/1896874-20200410134540490-1334187587.png)

可以看到，只有一条用例，但是利用参数化输入三组不同的测试数据和期望结果，最终执行的测试用例数=3，可以节省很多代码

 

#### 实际Web UI自动化中的开发场景，比如是一个登录框

1. 你肯定需要测试**账号空、密码空、账号密码都为空、账号不存在、密码错误、账号密码正确**等情况
2. 这些用例的区别就在于**输入的测试数据和对应的交互结果**
3. 所以我们可以只写一条登录测试用例，然后把多组测试数据和期望结果参数化，节省很多代码量

 

### 源码分析

def parametrize(self,argnames, argvalues, indirect=False, ids=None, scope=None): 

##### **argnames**

**源码解析：**a comma-separated string denoting one or more argument names, or a list/tuple of argument strings.

**含义：**参数名字

**格式：**字符串"arg1,arg2,arg3"**【需要用逗号分隔】**

**备注：**源码中写了可以是参数字符串的list或者tuple，但博主实操过是不行的，不知道是不是写的有问题，大家可以看看评论下

**示例**



```python
@pytest.mark.parametrize(["name", "pwd"], [("yy1", "123"), ("yy2", "123")])  
@pytest.mark.parametrize(("name", "pwd"), [("yy1", "123"), ("yy2", "123")])  
@pytest.mark.parametrize("name,pwd", [("yy1", "123"), ("yy2", "123")])
```

**备注**

有朋友提出现在上面标注错的两个可以正常使用了 

 

##### **argvalues**

**源码解析：**

- The list of argvalues determines how often a test is invoked with different argument values.
- If only one argname was specified argvalues is a list of values.**【只有一个参数，则是值列表】**
- If N argnames were specified, argvalues must be a list of N-tuples, where each tuple-element specifies a value for its respective argname.**【如果有多个参数，则用元组来存每一组值】**

**含义：**参数值列表

**格式：**必须是列表，如：[ val1,val2,val3 ]

如果只有一个参数，里面则是值的列表如：**@pytest.mark.parametrize("username", ["yy", "yy2", "yy3"])**

如果有多个参数例，则需要用元组来存放值，一个元组对应一组参数的值，如：**@pytest.mark.parametrize("name,pwd", [("yy1", "123"), ("yy2", "123"), ("yy3", "123")])**

**备注：**虽然源码说需要list包含tuple，但我试了下，tuple包含list，list包含list也是可以的........ 

 

##### ids

**含义：**用例的ID

**格式：**传一个字符串列表

**作用：**可以标识每一个测试用例，自定义测试数据结果的显示，为了增加可读性

**强调：**ids的长度需要与测试数据列表的长度一致

 

##### indirect

**作用：**如果设置成True，则把传进来的参数当**函数执行**，而不是一个参数（下一篇博文即讲解）

 

**讲完源码，对方法有更深入的了解了，我们就讲讲常用的场景**

 

### 装饰测试类



```python
@pytest.mark.parametrize('a, b, expect', data_1)
class TestParametrize:

    def test_parametrize_1(self, a, b, expect):
        print('\n测试函数11111 测试数据为\n{}-{}'.format(a, b))
        assert a + b == expect

    def test_parametrize_2(self, a, b, expect):
        print('\n测试函数22222 测试数据为\n{}-{}'.format(a, b))
        assert a + b == expect
```

 

##### 执行结果

![img](../../../blog/source/picture/1896874-20200410234743433-487154828.png)

 

##### 重点

当装饰器 @pytest.mark.parametrize 装饰测试类时，会将数据集合传递给类的所有测试用例方法

 

### “笛卡尔积”，多个参数化装饰器



```python
# 笛卡尔积，组合数据
data_1 = [1, 2, 3]
data_2 = ['a', 'b']


@pytest.mark.parametrize('a', data_1)
@pytest.mark.parametrize('b', data_2)
def test_parametrize_1(a, b):
    print(f'笛卡尔积 测试数据为 ： {a}，{b}')
```

 

##### 执行结果

![img](../../../blog/source/picture/1896874-20200410235331463-1433687745.png)

##### 重点知识

- 一个函数或一个类可以装饰多个 @pytest.mark.parametrize 
- 这种方式，最终生成的用例数是n*m，比如上面的代码就是：参数a的数据有3个，参数b的数据有2个，所以最终的用例数有3*2=6条
- 当参数化装饰器有很多个的时候，用例数都等于n*n*n*n*....

 

### 参数化 ，传入字典数据



```python
# 字典
data_1 = (
    {
        'user': 1,
        'pwd': 2
    },
    {
        'user': 3,
        'pwd': 4
    }
)


@pytest.mark.parametrize('dic', data_1)
def test_parametrize_1(dic):
    print(f'测试数据为\n{dic}')
    print(f'user:{dic["user"]},pwd{dic["pwd"]}')
```

没啥特别的，只是数据类型是常见的dict而已

 

##### 执行结果



```python
09parametrize.py::test_parametrize_1[dic0] PASSED                        [ 50%]测试数据为
{'user': 1, 'pwd': 2}
user:1,pwd2

09parametrize.py::test_parametrize_1[dic1] PASSED                        [100%]测试数据为
{'user': 3, 'pwd': 4}
user:3,pwd4
```

 

### 参数化，标记数据



```python
# 标记参数化
@pytest.mark.parametrize("test_input,expected", [
    ("3+5", 8),
    ("2+4", 6),
    pytest.param("6 * 9", 42, marks=pytest.mark.xfail),
    pytest.param("6*6", 42, marks=pytest.mark.skip)
])
def test_mark(test_input, expected):
    assert eval(test_input) == expected
```

 

##### 执行结果

![img](../../../blog/source/picture/1896874-20200412144723018-908939123.png)

 

### 参数化，增加可读性



```python
# 增加可读性
data_1 = [
    (1, 2, 3),
    (4, 5, 9)
]

# ids
ids = ["a:{} + b:{} = expect:{}".format(a, b, expect) for a, b, expect in data_1]


@pytest.mark.parametrize('a, b, expect', data_1, ids=ids)
class TestParametrize(object):

    def test_parametrize_1(self, a, b, expect):
        print('测试函数1测试数据为{}-{}'.format(a, b))
        assert a + b == expect

    def test_parametrize_2(self, a, b, expect):
        print('测试函数2数据为{}-{}'.format(a, b))
        assert a + b == expect
```

 

##### 执行结果

![img](../../../blog/source/picture/1896874-20200412145321771-874304562.png)

 

##### 知识点

多少组数据，就要有多少个id，然后组成一个id的列表

**作用：**主要是为了更加清晰看到用例的含义

## 十、fixture 传参数 request的详细使用

### 前言

- 为了提高复用性，我们在写测试用例的时候，会用到不同的fixture，比如：最常见的登录操作，大部分的用例的前置条件都是登录
- 假设不同的用例想登录不同的测试账号，那么登录fixture就不能把账号写死，需要通过传参的方式来完成登录操作

 

### 案例一：传单个参数



```python
import pytest


@pytest.fixture()
def login(request):
    name = request.param
    print(f"== 账号是：{name} ==")
    return name


data = ["pyy1", "polo"]
ids = [f"login_test_name is:{name}" for name in data]


@pytest.mark.parametrize("login", data, ids=ids, indirect=True)
def test_name(login):
    print(f" 测试用例的登录账号是：{login} ")
```

#### 执行结果



```python
collecting ... collected 2 items

10fixture_request.py::test_name[login_test_name is:pyy1] == 账号是：pyy1 ==
PASSED          [ 50%] 测试用例的登录账号是：pyy1 

10fixture_request.py::test_name[login_test_name is:polo] == 账号是：polo ==
PASSED          [100%] 测试用例的登录账号是：polo 
```

#### 知识点

- 添加 indirect=True 参数是为了把 login 当成一个函数去执行，而不是一个参数，并且将data当做参数传入函数
- def test_name(login) ，这里的login是获取fixture返回的值

 

### 案例二：多个参数



```python
@pytest.fixture()
def logins(request):
    param = request.param
    print(f"账号是：{param['username']}，密码是：{param['pwd']}")
    return param


data = [
    {"username": "name1", "pwd": "pwd1"},
    {"username": "name2", "pwd": "pwd2"},
]


@pytest.mark.parametrize("logins", data, indirect=True)
def test_name_pwd(logins):
    print(f"账号是：{logins['username']}，密码是：{logins['pwd']}")
```

#### 执行结果



```python
10fixture_request.py::test_name_pwd[logins0] 账号是：name1，密码是：pwd1
PASSED                      [ 50%]账号是：name1，密码是：pwd1

10fixture_request.py::test_name_pwd[logins1] 账号是：name2，密码是：pwd2
PASSED                      [100%]账号是：name2，密码是：pwd2
```

#### 知识点

如果需要传多个参数，需要通过字典去传

 

### 案例三：多个fixture（只加一个装饰器）

**这种更常用**



```python
# 多个fixture
@pytest.fixture(scope="module")
def input_user(request):
    user = request.param
    print("登录账户：%s" % user)
    return user


@pytest.fixture(scope="module")
def input_psw(request):
    psw = request.param
    print("登录密码：%s" % psw)
    return psw


data = [
    ("name1", "pwd1"),
    ("name2", "pwd2")
]


@pytest.mark.parametrize("input_user,input_psw", data, indirect=True)
def test_more_fixture(input_user, input_psw):
    print("fixture返回的内容:", input_user, input_psw)
```

#### 执行结果



```python
10fixture_request.py::test_more_fixture[name1-pwd1] 登录账户：name1
登录密码：pwd1
PASSED               [ 50%]fixture返回的内容: name1 pwd1

10fixture_request.py::test_more_fixture[name2-pwd2] 登录账户：name2
登录密码：pwd2
PASSED               [100%]fixture返回的内容: name2 pwd2
```

 

### 案例四：多个fixture（叠加装饰器）



```python
# 多个fixture
@pytest.fixture(scope="function")
def input_user(request):
    user = request.param
    print("登录账户：%s" % user)
    return user


@pytest.fixture(scope="function")
def input_psw(request):
    psw = request.param
    print("登录密码：%s" % psw)
    return psw


name = ["name1", "name2"]
pwd = ["pwd1", "pwd2"]


@pytest.mark.parametrize("input_user", name, indirect=True)
@pytest.mark.parametrize("input_psw", pwd, indirect=True)
def test_more_fixture(input_user, input_psw):
    print("fixture返回的内容:", input_user, input_psw)
```

#### 执行结果



```python
10fixture_request.py::test_more_fixture[pwd1-name1] 登录账户：name1
登录密码：pwd1
PASSED               [ 25%]fixture返回的内容: name1 pwd1

10fixture_request.py::test_more_fixture[pwd1-name2] 登录账户：name2
登录密码：pwd1
PASSED               [ 50%]fixture返回的内容: name2 pwd1

10fixture_request.py::test_more_fixture[pwd2-name1] 登录账户：name1
登录密码：pwd2
PASSED               [ 75%]fixture返回的内容: name1 pwd2

10fixture_request.py::test_more_fixture[pwd2-name2] 登录账户：name2
登录密码：pwd2
PASSED               [100%]fixture返回的内容: name2 pwd2
```

测试用例数=2*2=4条

## 十一、失败重跑插件pytest-rerunfailures的详细使用

### 环境前提

以下先决条件才能使用pytest-rerunfailures

- Python 3.5, 最高 3.8, or PyPy3
- pytest 5.0或更高版本

 

### 安装插件



```python
pip3 install pytest-rerunfailures -i http://pypi.douban.com/simple/ --trusted-host pypi.douban.com
```

 

### 提前了解重点

**命令行参数：**--reruns n（重新运行次数），--reruns-delay m（等待运行秒数）

**装饰器参数：**reruns=n（重新运行次数），reruns_delay=m（等待运行秒数）

 

### 重新运行所有失败的用例

要重新运行所有测试失败的用例，使用 --reruns 命令行选项，并指定要运行测试的最大次数：



```python
pytest --reruns 5 -s 
```

#### 知识点

运行失败的 fixture 或 setup_class 也将重新执行

 

#### 添加重新运行的延时

要在两次重试之间增加延迟时间，使用 --reruns-delay 命令行选项，指定下次测试重新开始之前等待的秒数



```python
pytest --reruns 5 --reruns-delay 10 -s
```

 

### 重新运行指定的测试用例

要将单个测试用例添加flaky装饰器 @pytest.mark.flaky(reruns=5) ，并在测试失败时自动重新运行，需要指定最大重新运行的次数

 

#### 例子



```python
import pytest


@pytest.mark.flaky(reruns=5)
def test_example():
    import random
    assert random.choice([True, False, False])
```

#### 执行结果



```python
collecting ... collected 1 item

11_reruns.py::test_example RERUN                                         [100%]
11_reruns.py::test_example PASSED                                        [100%]

========================= 1 passed, 1 rerun in 0.05s ==========================
```

 

#### 同样的，这个也可以指定重新运行的等待时间



```python
@pytest.mark.flaky(reruns=5, reruns_delay=2)
def test_example():
    import random
    assert random.choice([True, False, False])
```

 

#### 注意事项

如果指定了用例的重新运行次数，则在命令行添加 --reruns 对这些用例是不会生效的 

 

### 兼容性问题

- 不可以和fixture装饰器一起使用： @pytest.fixture() 
- 该插件与pytest-xdist的 --looponfail 标志不兼容
- 该插件与核心--pdb标志不兼容

## 十二、测试结果生成HTML报告插件之pytest-html的详细使用

### 环境前提

Python3.6+

 

### 安装插件



```python
pip3 install pytest-html -i http://pypi.douban.com/simple/ --trusted-host pypi.douban.com
```

 

### 快速入门



```python
pytest --html=report.html
```

会在当前目录下创建一个report.html的测试报告

 

#### 合并CSS

上面命令生成的报告，css是独立的，分享报告的时候样式会丢失，为了更好的分享发邮件展示报告，可以把css样式合并到html里



```python
pytest --html=report.html --self-contained-html
```

 

#### 注意事项

- 在将文件或链接添加到独立报告时，插件会发出warnings；
- 在html测试报告中可能无法按预期显示文件或链接

##  十三、重复执行用例插件之pytest-repeat的详细使用

### 前言

- 平常在做功能测试的时候，经常会遇到某个模块不稳定，偶然会出现一些bug，对于这种问题我们会针对此用例反复执行多次，最终复现出问题来
- 自动化运行用例时候，也会出现偶然的bug，可以针对单个用例，或者针对某个模块的用例重复执行多次

### 环境前提

- Python 2.7、3.4+或PyPy
- py.test 2.8或更高版本

 

### 安装插件



```python
pip3 install pytest-repeat -i http://pypi.douban.com/simple/ --trusted-host pypi.douban.com
```

 

### 快速入门

结合之前讲到的失败重跑、输出html报告插件来敲命令行

两种方式皆可，等号或空格

- count=2
- count 2



```python
pytest --html=report.html --self-contained-html  -s --reruns=5 --count=2 10fixture_request.py
```

 

### 重复测试直到失败（重点！）

- 如果需要验证偶现问题，可以一次又一次地运行相同的测试直到失败，这个插件将很有用
- 可以将pytest的 -x 选项与pytest-repeat结合使用，以强制测试运行程序在第一次失败时停止



```python
py.test --count=1000 -x test_file.py
```

 

#### 例子



```python
def test_example():
    import random
    flag = random.choice([True, False])
    print(flag)
    assert flag
```

 

#### 执行命令

　　pytest -s --count 5 -x 13repeat.py

 

#### 执行结果

![img](../../../blog/source/picture/1896874-20200413135848048-1986596726.png)

 

### @pytest.mark.repeat(count)

如果要在代码中将某些测试用例标记为执行重复多次，可以使用 @pytest.mark.repeat(count) 



```python
@pytest.mark.repeat(5)
def test_repeat():
    print("测试用例执行")
```

 

#### 执行命令



```python
pytest -s 13repeat.py
```

 

#### 执行结果

![img](../../../blog/source/picture/1896874-20200413142259708-1248397004.png)

 

### --repeat-scope

命令行参数

**作用：**可以覆盖默认的测试用例执行顺序，类似fixture的scope参数

- function：默认，范围针对每个用例重复执行，再执行下一个用例
- class：以class为用例集合单位，重复执行class里面的用例，再执行下一个
- module：以模块为单位，重复执行模块里面的用例，再执行下一个
- session：重复整个测试会话，即所有测试用例的执行一次，然后再执行第二次

 

#### 案例一：class



```python
class Test_repeat:
    def test_repeat3(self):
        print("测试用例执行333")

class Test_repeat2:
    def test_repeat3(self):
        print("测试用例执行444")
```

#### 执行命令



```python
pytest -s --count=2 --repeat-scope=class 13repeat.py
```

#### 执行结果

![img](../../../blog/source/picture/1896874-20200413143418525-1712642726.png)

 

#### 案例二：module



```python
def test_repeat1():
    print("测试用例执行111")


def test_repeat2():
    print("测试用例执行222")


class Test_repeat:
    def test_repeat3(self):
        print("测试用例执行333")
```

#### 执行命令



```python
pytest -s --count=2 --repeat-scope=module 13repeat.py
```

#### 执行结果

![img](../../../blog/source/picture/1896874-20200413143422428-1300950331.png)

 

### 兼容性问题

pytest-repeat不能与unittest.TestCase测试类一起使用。无论--count设置多少，这些测试始终仅运行一次，并显示警告

## 十四、配置文件pytest.ini的详细使用

### 前言

pytest配置文件可以改变pytest的运行方式，它是一个固定的文件pytest.ini文件，读取配置信息，按指定的方式去运行

 

### 非test文件

pytest里面有些文件是非test文件

- pytest.ini：pytest的主配置文件，可以改变pytest的默认行为
- conftest.py：测试用例的一些fixture配置
- _*init*_.py：识别该文件夹为python的package包

 

### 查看pytest.ini的配置选项

cmd执行



```python
pytest --help
```

 

找到这部分内容



```python
[pytest] ini-options in the first pytest.ini|tox.ini|setup.cfg file found:

  markers (linelist):   markers for test functions
  empty_parameter_set_mark (string):
                        default marker for empty parametersets
  norecursedirs (args): directory patterns to avoid for recursion
  testpaths (args):     directories to search for tests when no files or directories are given in the command line.
  usefixtures (args):   list of default fixtures to be used with this project
  python_files (args):  glob-style file patterns for Python test module discovery
  python_classes (args):
                        prefixes or glob names for Python test class discovery
  python_functions (args):
                        prefixes or glob names for Python test function and method discovery
  disable_test_id_escaping_and_forfeit_all_rights_to_community_support (bool):
                        disable string escape non-ascii characters, might cause unwanted side effects(use at your own
                        risk)
  console_output_style (string):
                        console output: "classic", or with additional progress information ("progress" (percentage) |
                        "count").
  xfail_strict (bool):  default for the strict parameter of xfail markers when not given explicitly (default: False)
  enable_assertion_pass_hook (bool):
                        Enables the pytest_assertion_pass hook.Make sure to delete any previously generated pyc cache
                        files.
  junit_suite_name (string):
                        Test suite name for JUnit report
  junit_logging (string):
                        Write captured log messages to JUnit report: one of no|log|system-out|system-err|out-err|all
  junit_log_passing_tests (bool):
                        Capture log information for passing tests to JUnit report:
  junit_duration_report (string):
                        Duration time to report: one of total|call
  junit_family (string):
                        Emit XML for schema: one of legacy|xunit1|xunit2
  doctest_optionflags (args):
                        option flags for doctests
  doctest_encoding (string):
                        encoding used for doctest files
  cache_dir (string):   cache directory path.
  filterwarnings (linelist):
                        Each line specifies a pattern for warnings.filterwarnings. Processed after -W/--pythonwarnings.
  log_print (bool):     default value for --no-print-logs
  log_level (string):   default value for --log-level
  log_format (string):  default value for --log-format
  log_date_format (string):
                        default value for --log-date-format
  log_cli (bool):       enable log display during test run (also known as "live logging").
  log_cli_level (string):
                        default value for --log-cli-level
  log_cli_format (string):
                        default value for --log-cli-format
  log_cli_date_format (string):
                        default value for --log-cli-date-format
  log_file (string):    default value for --log-file
  log_file_level (string):
                        default value for --log-file-level
  log_file_format (string):
                        default value for --log-file-format
  log_file_date_format (string):
                        default value for --log-file-date-format
  log_auto_indent (string):
                        default value for --log-auto-indent
  faulthandler_timeout (string):
                        Dump the traceback of all threads if a test takes more than TIMEOUT seconds to finish. Not
                        available on Windows.
  addopts (args):       extra command line options
  minversion (string):  minimally required pytest version
  rsyncdirs (pathlist): list of (relative) paths to be rsynced for remote distributed testing.
  rsyncignore (pathlist):
                        list of (relative) glob-style paths to be ignored for rsyncing.
  looponfailroots (pathlist):
                        directories to check for changes
```

 

### pytest.ini应该放哪里？

就放在项目根目录下 ，不要乱放，不要乱起其他名字

 

**接下来讲下常用的配置项**

 

### marks

**作用：**测试用例中添加了 @pytest.mark.webtest 装饰器，如果不添加marks选项的话，就会报warnings

**格式：**list列表类型

**写法：**



```python
[pytest]
markers =
    weibo: this is weibo page
    toutiao: toutiao
    xinlang: xinlang
```

 

### xfail_strict

**作用：**设置xfail_strict = True可以让那些标记为@pytest.mark.xfail但实际通过显示XPASS的测试用例被报告为失败

**格式：**True 、False（默认），1、0

**写法：**



```python
[pytest]

# mark标记说明
markers =
    weibo: this is weibo page
    toutiao: toutiao
    xinlang: xinlang

xfail_strict = True
```

 

**具体代码栗子**

未设置 xfail_strict = True 时，测试结果显示XPASS



```python
@pytest.mark.xfail()
def test_case1():
    a = "a"
    b = "b"
    assert a != b
```

collecting ... collected 1 item

02断言异常.py::test_case1 XPASS [100%]

============================= 1 xpassed in 0.02s ==============================

 

已设置 xfail_strict = True 时，测试结果显示failed



```python
collecting ... collected 1 item

02断言异常.py::test_case1 FAILED                                         [100%]
02断言异常.py:54 (test_case1)
[XPASS(strict)] 

================================== FAILURES ===================================
_________________________________ test_case1 __________________________________
[XPASS(strict)] 
=========================== short test summary info ===========================
FAILED 02断言异常.py::test_case1
============================== 1 failed in 0.02s ==============================
```

 

### **addopts**

**作用：**addopts参数可以更改默认命令行选项，这个当我们在cmd输入一堆指令去执行用例的时候，就可以用该参数代替了，省去重复性的敲命令工作

**比如：**想测试完生成报告，失败重跑两次，一共运行两次，通过分布式去测试，如果在cmd中写的话，命令会很长



```python
pytest -v --rerun=2 --count=2 --html=report.html --self-contained-html -n=auto
```

 

每次都这样敲不太现实，addopts就可以完美解决这个问题



```python
[pytest]

# mark
markers =
    weibo: this is weibo page
    toutiao: toutiao
    xinlang: xinlang

xfail_strict = True

# 命令行参数
addopts = -v --reruns=1 --count=2 --html=reports.html --self-contained-html -n=auto
```

**加了addopts之后，我们在cmd中只需要敲pytest就可以生效了！！**

 

### log_cli

**作用：**控制台实时输出日志

**格式：**log_cli=True 或False（默认），或者log_cli=1 或 0

#### log_cli=0的运行结果

![img](../../../blog/source/picture/1896874-20200415100511883-1706958706.png)

 

#### log_cli=1的运行结果

![img](../../../blog/source/picture/1896874-20200415100540480-1964956329.png)

#### 结论

很明显，加了log_cli=1之后，可以清晰看到哪个package下的哪个module下的哪个测试用例是否passed还是failed；

**所以平时测试代码是否有问题的情况下推荐加！！！但如果拿去批量跑测试用例的话不建议加，谁知道会不会影响运行性能呢？**

 

### norecursedirs

**作用：**pytest 收集测试用例时，会递归遍历所有子目录，包括某些你明知道没必要遍历的目录，遇到这种情况，可以使用 norecursedirs 参数简化 pytest 的搜索工作**【还是挺有用的！！！】**

**默认设置：** norecursedirs = .* build dist CVS _darcs {arch} *.egg 

**正确写法：**多个路径用空格隔开



```python
[pytest]

norecursedirs = .* build dist CVS _darcs {arch} *.egg venv src resources log report util
```

 

### 更改测试用例收集规则

pytest默认的测试用例收集规则

- 文件名以 test_*.py 文件和 *_test.py
- 以 test_ 开头的函数
- 以 Test 开头的类，不能包含 __init__ 方法
- 以 test_ 开头的类里面的方法

 

我们是可以修改或者添加这个用例收集规则的；当然啦，是建议在原有的规则上添加的，如下配置



```python
[pytest]

python_files =     test_*  *_test  test*
python_classes =   Test*   test*
python_functions = test_*  test*
```

## 十五、多重校验插件之pytest-assume的详细使用

### 前言

pytest中可以用python的assert断言，也可以写多个断言，但一个失败，后面的断言将不再执行

 

### 安装插件



```python
pip3 install pytest-assume -i http://pypi.douban.com/simple/ --trusted-host pypi.douban.com
```

 

### assert多重断言



```python
def test_add1():
    assert 1 + 4 == 5
    assert 1 + 3 == 3
    assert 2 + 5 == 7
    assert 2 + 5 == 9
    print("测试完成")
```

#### 执行结果

![img](../../../blog/source/picture/1896874-20200415132322553-569119361.png)

#### 结论

可以看到，第二行断言失败之后，后面的断言也不会执行，包括正常的代码

 

### pytest.assume多重断言



```python
def test_add2():
    pytest.assume(1 + 4 == 5)
    pytest.assume(1 + 3 == 3)
    pytest.assume(2 + 5 == 7)
    pytest.assume(2 + 5 == 9)
    print("测试完成")
```

#### 执行结果

![img](../../../blog/source/picture/1896874-20200415132847122-1838621760.png)

#### 结论

- 可以看到，第二行即使断言失败，后面的断言还是会继续执行
- 这有助于我们分析和查看到底一共有哪些断言是失败的
- 而且最后的代码也还会正常执行，比直接用assert更高效

## 十六、分布式测试插件之pytest-xdist的详细使用

### 前言

- 平常我们功能测试用例非常多时，比如有1千条用例，假设每个用例执行需要1分钟，如果单个测试人员执行需要1000分钟才能跑完
- 当项目非常紧急时，会需要协调多个测试资源来把任务分成两部分，于是执行时间缩短一半，如果有10个小伙伴，那么执行时间就会变成十分之一，大大节省了测试时间
- 为了节省项目测试时间，10个测试同时并行测试，这就是一种分布式场景
- 同样道理，当我们自动化测试用例非常多的时候， 一条条按顺序执行会非常慢，**pytest-xdist的出现就是为了让自动化测试用例可以分布式执行，从而节省自动化测试时间**
- **pytest-xdist是属于进程级别的并发**

 

### 分布式执行用例的设计原则（重中之重的重点）

- 用例之间是独立的，用例之间没有依赖关系，用例可以完全独立运行**【独立运行】**
- 用例执行没有顺序，随机顺序都能正常执行**【随机执行】**
- 每个用例都能重复运行，运行结果不会影响其他用例**【不影响其他用例】**

 

### 插件安装



```python
pip3 install pytest-xdist -i http://pypi.douban.com/simple/ --trusted-host pypi.douban.com
```

 

### pytest-xdist通过一些独特的测试执行模式扩展了pytest

- **测试运行并行化：**如果有多个CPU或主机，则可以将它们用于组合的测试运行。 这样可以加快开发速度或使用远程计算机的特殊资源。
- **--looponfail：**在子进程中重复运行测试。 每次运行之后，pytest都会等到项目中的文件更改后再运行之前失败的测试。 重复此过程，直到所有测试通过，然后再次执行完整运行。
- **跨平台覆盖：**您可以指定不同的Python解释程序或不同的平台，并在所有这些平台上并行运行测试。

 

### 快速入门

这是运行代码的包结构



```python
14xdist是项目文件夹名称
│  conftest.py
│  test_1.py
│  __init__.py
│              
├─test_51job
│  │  conftest.py
│  │  test_case1.py
│  │  __init__.py 
│          
├─test_toutiao
│  │  test_case2.py
│
├─test_weibo
│  │  conftest.py
│  │  test_case3.py
│  │  __init__.py 
│          
```

#### 具体代码

#### 最外层的conftest.py

![img](https://images.cnblogs.com/OutliningIndicators/ContractedBlock.gif) View Code

 

#### 最外层的test_1.py

![img](https://images.cnblogs.com/OutliningIndicators/ContractedBlock.gif) View Code

 

#### test_51job包下的conftest.py

![img](https://images.cnblogs.com/OutliningIndicators/ContractedBlock.gif) View Code

 

#### test_51job包下的test_case1.py

![img](https://images.cnblogs.com/OutliningIndicators/ContractedBlock.gif) View Code

 

#### test_toutiao包下的test_case2.py

![img](https://images.cnblogs.com/OutliningIndicators/ContractedBlock.gif) View Code

 

#### test_weibo包下的conftest.py

![img](https://images.cnblogs.com/OutliningIndicators/ContractedBlock.gif) View Code

 

#### test_weibo包下的test_case3.py

![img](https://images.cnblogs.com/OutliningIndicators/ContractedBlock.gif) View Code

 

#### 不使用分布式测试的命令和所需执行时间



```python
pytest -s
```

![img](../../../blog/source/picture/1896874-20200414092305407-782175120.png)

 

可以看到，执行一条用例大概1s（因为每个用例都加了 sleep(1) ），一共30条用例，总共运行30s；那么如果有1000条用例，执行时间就真的是1000s

 

#### 使用分布式测试的命令和所需执行时间



```python
pytest -s -n auto
```

![img](../../../blog/source/picture/1896874-20200414092647002-1058636098.png)

 

#### 知识点

- 可以看到，最终运行时间只需要6s，我的电脑是真6核，假12核
- -n auto：可以自动检测到系统的CPU核数；从测试结果来看，检测到的是逻辑处理器的数量，即假12核
- 使用auto等于利用了所有CPU来跑用例，此时CPU占用率会特别高

 

#### 可以指定需要多少个CPU来跑用例



```python
pytest -s -n 2
```

![img](../../../blog/source/picture/1896874-20200414093640698-1947498338.png)

 

#### pytest-xdist是可以和pytest-html很好的相结合



```python
pytest -s -n auto --html=report.html --self-contained-html
```

 

### pytest-xdist按照一定的顺序执行

 pytest-xdist默认是无序执行的，可以通过 --dist 参数来控制顺序

 --dist=loadscope 

- 将按照同一个模块module下的函数和同一个测试类class下的方法来分组，然后将每个测试组发给可以执行的worker，**确保同一个组的测试用例在同一个进程中执行**
- 目前无法自定义分组，按类class分组优先于按模块module分组

 

 --dist=loadfile 

按照同一个文件名来分组，然后将每个测试组发给可以执行的worker，**确保同一个组的测试用例在同一个进程中执行**

 

### 如何让scope=session的fixture在test session中仅仅执行一次

pytest-xdist是让每个worker进程执行属于自己的测试用例集下的所有测试用例

这意味着在不同进程中，不同的测试用例可能会调用同一个scope范围级别较高（例如session）的fixture，该fixture则会被执行多次，这不符合scope=session的预期

 

#### 如何解决？

虽然pytest-xdist没有内置的支持来确保会话范围的夹具仅执行一次，但是可以通过使用锁定文件进行进程间通信来实现。

 

#### 小栗子

1. 下面的示例只需要执行一次login（因为它是只需要执行一次来定义配置选项，等等）
2. 当第一次请求这个fixture时，则会利用FileLock仅产生一次fixture数据
3. 当其他进程再次请求这个fixture时，则会从文件中读取数据



```python
import pytest
from filelock import FileLock


@pytest.fixture(scope="session")
def login():
    print("====登录功能，返回账号，token===")
    with FileLock("session.lock"):
        name = "testyy"
        token = "npoi213bn4"
        # web ui自动化
        # 声明一个driver，再返回

        # 接口自动化
        # 发起一个登录请求，将token返回都可以这样写

    yield name, token
    print("====退出登录！！！====")
```

## 十七、pytest-xdist分布式测试的原理和流程

## pytest-xdist分布式测试的原理

#### 前言

- xdist的分布式类似于**一主多从**的结构，master机负责下发命令，控制slave机；slave机根据master机的命令执行特定测试任务
- 在xdist中，主是master，从是workers

 

#### 大致原理

1. xdist会产生一个或多个workers，workers都通过master来控制
2. 每个worker负责执行完整的测试用例集，然后按照master的要求运行测试，而master机不执行测试任务

 

## pytest-xdist分布式测试的流程

### 第一步：创建worker

1. master会在总测试会话（test session）开始前产生一个或多个worker
2. master和worker之间是通过execnet和网关来通信的
3. 实际编译执行测试代码的worker可能是本地机器也可能是远程机器

 

### 第二步：收集测试项用例

1. 每个worker类似一个迷你型的pytest执行器
2. worker会执行一个完整的test collection过程**【收集所有测试用例的过程】**
3. 然后把测试用例的ids返回给master
4. master是不会执行任何测试用例集的

#### 注意

所以为什么上面通过分布式测试的结果截图是没有输出用例的print内容，因为主机并不执行测试用例，pycharm相当于一个master

 

### 第三步：master检测workers收集到的测试用例集

1. master接收到所有worker收集的测试用例集之后，master会进行一些完整性检查，以确保所有worker都收集到一样的测试用例集（包括顺序）
2. 如果检查通过，会将测试用例的ids列表转换成简单的索引列表，每个索引对应一个测试用例的在原来测试集中的位置
3. 这个方案可行的原因是：所有的节点都保存着相同的测试用例集
4. 并且使用这种方式可以**节省带宽，**因为master只需要告知workers需要执行的测试用例**对应的索引**，而不用告知完整的测试用例信息

 

### 第四步：测试用例分发

--dist-mode选项

**each：**master将完整的测试索引列表分发到每个worker

**load：**master将大约25%的测试用例以轮询的方式分发到各个worker，剩余的测试用例则会等待workers执行完测试用例以后再分发

 

#### 注意

可以使用 pytest_xdist_make_scheduler 这个hook来实现自定义测试分发逻辑。

 

### 第五步：测试用例的执行

1. workers 重写了  pytest_runtestloop ：pytest的默认实现是循环执行所有在test session这个对象里面收集到的测试用例
2. 但是在xdist里, workers实际上是等待master为其发送需要执行的测试用例
3. 当worker收到测试任务, 就顺序执行  pytest_runtest_protocol 
4. 值得注意的一个细节是：workers 必须始终保持至少一个测试用例在的任务队列里, 以兼容 pytest_runtest_protocol(item, nextitem)  hook的参数要求，为了将 nextitem传给hook
5. worker会在执行最后一个测试项前等待master的更多指令
6. 如果它收到了更多测试项, 那么就可以安全的执行  pytest_runtest_protocol , 因为这时nextitem参数已经可以确定
7. 如果它收到一个 "shutdown"信号, 那么就将 nextitem 参数设为 None, 然后执行 pytest_runtest_protocol

 

### 第六步：测试用例再分发（--dist-mode=load）

1. 当workers开始/结束执行时，会把测试结果返回给master，这样其他pytest hook比如： pytest_runtest_protocol 和  pytest_runtest_protocol 就可以正常执行
2. master在worker执行完一个测试后，基于测试执行时长以及每个work剩余测试用例综合决定是否向这个worker发送更多的测试用例

 

### 第七步：测试结束

1. 当master没有更多执行测试任务时，它会发送一个“shutdown”信号给所有worker
2. 当worker将剩余测试用例执行完后退出进程
3. master等待所有worker全部退出
4. 然此时仍需要处理诸如 pytest_runtest_logreport 等事件

## 十八、超美测试报告插件之allure-pytest的基础使用

### 官方介绍

1. Allure Framework是一种灵活的轻量级多语言测试报告工具，不仅可以以简洁的Web报告形式非常简洁地显示已测试的内容，也允许参与开发过程的每个人从日常测试中提取最大程度的有用信息
2. 从开发/质量保证的角度来看，Allure报告可以缩短常见缺陷的生命周期：可以将测试失败划分为bug和损坏的测试，还可以配置log，step，fixture，attachments，timings，历史记录以及与TMS的集成以及Bug跟踪系统，因此负责任的开发人员和测试人员将掌握所有信息
3. 从管理人员的角度来看，Allure提供了一个清晰的“全局”，涵盖了已涵盖的功能，缺陷聚集的位置，执行时间表的外观以及许多其他方便的事情
4. Allure的模块化和可扩展性确保您始终能够微调某些东西，以使Allure更适合您

 

### 个人介绍

1. 对于管理层来说，测试报告当然是越直观、简洁、数据清晰越好，而Allure就满足以上这么多点，而且很好的和pytest集成了
2. 相比于pytest-html来说，Allure的报告真的是十全十美鸭！！
3. 唯一不足的就是，拓展功能需要在测试用例集上加装饰器

 

### 环境配置

https://www.cnblogs.com/poloyy/p/13886651.html

 

### 安装插件



```python
pip3 install allure-pytest -i http://pypi.douban.com/simple/ --trusted-host pypi.douban.com
```

 

### 快速入门

这是运行代码的包结构



```python
# 是项目文件夹名称
15allure
│  conftest.py
│  test_1.py
│  __init__.py
│              
├─test_51job
│  │  conftest.py
│  │  test_case1.py
│  │  __init__.py 
│          
├─test_toutiao
│  │  test_case2.py
│
├─test_weibo
│  │  conftest.py
│  │  test_case3.py
│  │  __init__.py 
│  
```

 

#### 最外层的conftest.py

![img](https://images.cnblogs.com/OutliningIndicators/ContractedBlock.gif) View Code

 

#### 最外层的test_1.py

![img](https://images.cnblogs.com/OutliningIndicators/ContractedBlock.gif) View Code

 

#### test_51job包下的conftest.py

![img](https://images.cnblogs.com/OutliningIndicators/ContractedBlock.gif) View Code

 

#### test_51job包下的test_case1.py

![img](https://images.cnblogs.com/OutliningIndicators/ContractedBlock.gif) View Code

 

#### test_toutiao包下的test_case2.py

![img](https://images.cnblogs.com/OutliningIndicators/ContractedBlock.gif) View Code

 

#### test_weibo包下的conftest.py

![img](https://images.cnblogs.com/OutliningIndicators/ContractedBlock.gif) View Code

 

#### test_weibo包下的test_case3.py

![img](https://images.cnblogs.com/OutliningIndicators/ContractedBlock.gif) View Code

 

#### 执行命令

要使Allure能够在测试执行期间收集测试结果，只需添加 --alluredir 选项，并提供指向应存储结果的文件夹的路径



```python
pytest -n auto --alluredir=allure
```

 

#### 生成出来的结果

可以看到，这不是我们想要的结果，一堆json、txt文件....

![img](../../../blog/source/picture/1896874-20200416133934690-1488645011.png)

 

#### 要在测试完成后查看实际报告，需要使用Allure命令行来让测试结果生成报告



```python
allure serve allure
```

然后就会自动在默认浏览器中显示生成的报告

![img](../../../blog/source/picture/1896874-20200416133939321-270564959.png)

 

### 查看suites（函数级别的测试用例）

从包名-模块名-测试用例

![img](../../../blog/source/picture/1896874-20200416135945754-1615428832.png)

 

### 查看suites（类级别的测试用例）

从包名-模块名-类名-测试用例

![img](../../../blog/source/picture/1896874-20200416135949209-2093306085.png)

 

### 查看测试用例详情

- **parameters**：如果用了 @pytest.mark.parametrize ，在右侧的parameters是可以看到传了什么参数和对应的值
- **set up**：调用fixture的前置操作
- **tear down**：调用fixture的后置操作　

![img](../../../blog/source/picture/1896874-20200416140444076-1695464512.png)

 

### Allure报告结构

- Overview：总览
- Categories：类别，默认是分了failed和error，凡是执行结果是其中一个的都会被归到类里面，可以通过这里快捷查看哪些用例是failed和error的
- Suites：测试套件，就是所有用例的层级关系，可以根据package、module、类、方法来查找用例
- Graphs：测试结果图形化，包括用例执行结果的分布图，优先级，耗时等
- Timeline：可以看到测试用例精确的测试时序（执行顺序），包括执行时间
- Behaviors：行为驱动，根据epic、feature、story来分组测试用例（后面会讲到）
- Packages：这就是按照package、module来分组测试用例了

## 十九、我们需要掌握的allure特性

### 前言

前面我们介绍了allure的快速入门，只是单纯的敲allure命令而已

其实allure还有内置的特性可以让我们在pytest代码里面用起来，然后我们生成的报告更加直观、详细、贴合管理层的心意...

 

### Environment

可以理解成**环境变量参数**，没有什么实际作用，个人觉得只是为了让别人知道本次测试的运行环境参数而已，显示啥都是自己定的

**注意！！默认是没有的哦**

![img](../../../blog/source/picture/1896874-20200416190714772-747732691.png)

 

### 如何添加Environment呢

通过创建environment.properties或者environment.xml文件，并把文件存放到allure-results(这个目录是生成最后的html报告之前，生成依赖文件的目录)目录下，就是 --alluredir 后面跟的目录

像我这里目录就是allure，所以放在allure下面 --alluredir allure 

[![img](../../../blog/source/picture/1896874-20200416192513518-762408880.png)

 

#### environment.properties



```python
Browser=Chrome
Browser.Version=81.0.4044.92
Stand=Production
ApiUrl=127.0.0.1/login
python.Version=3.7.2
```

 

**或者**

#### environment.xml



```python
<environment>
    <parameter>
        <key>Browser</key>
        <value>Chrome</value>
    </parameter>
    <parameter>
        <key>Browser.Version</key>
        <value>81.0.4044.92</value>
    </parameter>
    <parameter>
        <key>Stand</key>
        <value>Production</value>
    </parameter>
        <parameter>
        <key>ApiUrl</key>
        <value>127.0.0.1/login</value>
    </parameter>
        <parameter>
        <key>python.Version</key>
        <value>3.7.2</value>
    </parameter>
</environment>
```

**注意！都不可以写中文哦！！！！亲测！！会乱码**

 

#### **运行之后，我们看看allure报告的environment**

[![img](../../../blog/source/picture/1896874-20200416192739245-1776053197.png)

 

### Categories

**直译：**分类

**通俗理解：**测试用例结果的分类

默认情况下，有两类缺陷：

1. Product defects 产品缺陷（测试结果：failed）
2. Test defects 测试缺陷（测试结果：error/broken）

我们是可以创建自定义缺陷分类的，将 categories.json 文件添加到allure-results目录即可（和上面environment.properties放同一个目录）

 

#### categories.json



```python
[
  {
    "name": "Ignored tests", 
    "matchedStatuses": ["skipped"] 
  },
  {
    "name": "Infrastructure problems",
    "matchedStatuses": ["broken", "failed"],
    "messageRegex": ".*bye-bye.*" 
  },
  {
    "name": "Outdated tests",
    "matchedStatuses": ["broken"],
    "traceRegex": ".*FileNotFoundException.*" 
  },
  {
    "name": "Product defects",
    "matchedStatuses": ["failed"]
  },
  {
    "name": "Test defects",
    "matchedStatuses": ["broken"]
  }
]
```

 

#### 讲下参数的含义

- **name**：分类名称
- **matchedStatuses**：测试用例的运行状态，默认["failed", "broken", "passed", "skipped", "unknown"]
- **messageRegex**：测试用例运行的错误信息，默认是 .* ，是通过正则去匹配的哦！
- **traceRegex**：测试用例运行的错误堆栈信息，默认是 .* ，也是通过正则去匹配的哦！

#### 注意

这里的name是可以写中文的哦！

 

### Flaky test

**用法：**在类或者方法上直接加 @Flaky 

**官方也说了：**可以将整个测试类标记为Flaky

 

#### 那什么是Flaky呢？

- 简单来说就是，不够稳定的测试用例集，有可能前阵子还运行成功，过阵子就运行失败，理解成“闪烁”
- 标记成Flaky的好处就是：当用例失败的情况下，我们能获取足够详细的信息，毕竟有可能某些测试用例是非常重要的
- 如果不标记为Flaky的话，可能就要禁用这些测试

## 二十、allure的特性，@allure.step()、allure.attach的详细使用

### 前言

allure除了支持pytest自带的特性之外（fixture、parametrize、xfail、skip），自己本身也有强大的特性可以在pytest中使用

 

### @allure.step 

- allure报告最重要的一点是，它允许对每个测试用例进行非常详细的步骤说明
- 通过 @allure.step() 装饰器，可以让测试用例在allure报告中显示更详细的测试过程

 

#### 示例代码



```python
#!/usr/bin/env python
# -*- coding: utf-8 -*-
import allure


@allure.step("第一步")
def passing_step():
    pass


@allure.step("第二步")
def step_with_nested_steps():
    nested_step()


@allure.step("第三步")
def nested_step():
    nested_step_with_arguments(1, 'abc')


@allure.step("第四步{0}，{arg2}")
def nested_step_with_arguments(arg1, arg2):
    pass


@allure.step("第五步")
def test_with_nested_steps():
    passing_step()
    step_with_nested_steps()
```

 

#### 测试用例在allure上的显示

![img](../../../blog/source/picture/1896874-20200417135246245-902116613.png)

#### 知识点

-  step() 只有一个参数，就是title，你传什么，在allure上就显示什么
- 可以像python字符串一样，支持位置参数和关键字参数 {0}，{arg2}，可看第四步那里，如果函数的参数没有匹配成功就会报错哦
-  step() 的使用场景，给我感觉就是，当方法之间嵌套会比较有用，否则的话只会显示一个步骤，类似下面图

[![img](https://img2020.cnblogs.com/blog/1896874/202004/1896874-20200417133631873-1202403389.png)](https://img2020.cnblogs.com/blog/1896874/202004/1896874-20200417133631873-1202403389.png)

 

### allure.attach（挺有用的）

**作用：**allure报告还支持显示许多不同类型的附件，可以补充测试结果；自己想输出啥就输出啥，挺好的

**语法：** allure.attach(body, name, attachment_type, extension) 

#### **参数列表**

- **body**：要显示的内容（附件）
- **name**：附件名字
- **attachment_type**：附件类型，是 allure.attachment_type 里面的其中一种
- **extension**：附件的扩展名（比较少用）

 

#### allure.attachment_type提供了哪些附件类型？

![img](../../../blog/source/picture/1896874-20200417200221337-763341073.png)

 

#### 语法二： allure.attach.file(source, name, attachment_type, extension) 

source：文件路径，相当于传一个文件

其他参数和上面的一致

 

#### 其中一个测试用例的代码栗子



```python
#!/usr/bin/env python
# -*- coding: utf-8 -*-

import allure
import pytest


@pytest.fixture
def attach_file_in_module_scope_fixture_with_finalizer(request):
    allure.attach('在fixture前置操作里面添加一个附件txt', 'fixture前置附件', allure.attachment_type.TEXT)

    def finalizer_module_scope_fixture():
        allure.attach('在fixture后置操作里面添加一个附件txt', 'fixture后置附件',
                      allure.attachment_type.TEXT)

    request.addfinalizer(finalizer_module_scope_fixture)


def test_with_attacments_in_fixture_and_finalizer(attach_file_in_module_scope_fixture_with_finalizer):
    pass


def test_multiple_attachments():
    allure.attach('<head></head><body> 一个HTML页面 </body>', 'Attach with HTML type', allure.attachment_type.HTML)
    allure.attach.file('./reports.html', attachment_type=allure.attachment_type.HTML)
```

 

#### 运行之后看结果

![img](../../../blog/source/picture/1896874-20200417203657146-1495579440.png)

**这是一个txt附件**

 

 ![img](../../../blog/source/picture/1896874-20200417204119583-92174005.png)

**这是一个用了 allure.attach() 来插入一段自己写的HTML和 allure.attach.file() 来导入一个已存在的HTML文件（pytest-html报告）**

## 二十一、allure的特性，@allure.description()、@allure.title()的详细使用

### 前言

上一篇文章介绍了两种allure的特性

- **@allure.step() 装饰器：**可以设置测试步骤，让测试用例的执行过程更加详细
- **allure.attach() 函数：**可以设置需要显示在allure报告的附件，包含了多种类型，可以通过allure.attachment_type查看支持的类型

这一篇幅，我们主要来讲解另外两个特性，可以增加报告的可读性哦！

- @allure.description()
- @allure.title()

它们用法极其相近，只是作用不一样而已

 

### @allure.description()

#### **作用**

可以添加足够详细的测试用例描述，以便于管理层查看哦哈哈哈

 

#### 语法格式，有三种

1. @allure.description(str）
2. 在测试用例函数声明下方添加 """ """
3. @allure.description_html(str）：相当于传一个HTML代码组成的字符串，类似 allure.attach() 中传HTML

**注意：**方式一方式二的效果和作用是一致的， 哪个方便哪个来



```python
#!/usr/bin/env python
# -*- coding: utf-8 -*-


import allure

import allure

# 方式一
@allure.description("""
这是一个@allure.description装饰器
没有特别的用处
""")
def test_description_from_decorator():
    assert 42 == int(6 * 7)

# 方式二
def test_unicode_in_docstring_description():
    """
    当然，在方法声明的下一行这样子写，也算一种添加description的方式哦
    """
    assert 42 == int(6 * 7)

# 方式三
@allure.description_html("""
<h1>Test with some complicated html description</h1>
<table style="width:100%">
  <tr>
    <th>Firstname</th>
    <th>Lastname</th>
  </tr>
  <tr align="center">
    <td>William</td>
    <td>Smith</td>
</table>
""")
def test_html_description():
    assert True
```

 

#### 方式一的allure报告

![img](../../../blog/source/picture/1896874-20200418154011153-1321880660.png)

 

#### 方式二的allure报告

![img](../../../blog/source/picture/1896874-20200418154044384-2117386341.png)

 

#### 方式三的allure报告

![img](../../../blog/source/picture/1896874-20200418154041997-557868599.png)

 

### @allure.title()

#### 作用

- 使得测试用例的标题更具有可读性，毕竟我们可以写成中文
- 支持占位符传递关键字参数哦（动态标题，结合 @pytest.mark.parametrize 使用）

 

#### 具体栗子一



```python
#!/usr/bin/env python
# -*- coding: utf-8 -*-
import pytest, allure


@allure.title("前置操作：登录")
@pytest.fixture
def test_loginss(request):
    params = request.param
    name = params["username"]
    pwd = params["pwd"]
    allure.attach(f"这是测试用例传的参数{params}")
    print(name, pwd, params)
    yield name, pwd


@allure.title("成功登录，测试数据是：{test_loginss}")
@pytest.mark.parametrize("test_loginss", [
    {"username": "name1", "pwd": "pwd1"},
    {"username": "name2", "pwd": "pwd2"}], indirect=True)
def test_success_login(test_loginss):
    name, pwd = test_loginss
    allure.attach(f"账号{name},密码{pwd}")
```

 

#### 运行结果，查看allure报告

这是一次综合多个之前学到的方法来完成的栗子，已经具体标出来啦！

![img](../../../blog/source/picture/1896874-20200418163324281-2106601763.png)

 

#### 具体栗子二



```python
@allure.title("多个参数{name},{phone},{age}")
@pytest.mark.parametrize("name,phone,age", [
    (1, 2, 3),
    (4, 5, 6),
    (7, 8, 9)
])
def test_test_test(name, phone, age):
    print(name, phone, age)
```

 

#### 运行结果，查看allure报告

![img](https://img2020.cnblogs.com/blog/1896874/202004/1896874-20200418164434431-948219776.png)

 

### 总结

如果没有添加 @allure.title() 的话，测试用例的标题默认就是函数名，这样的可读性不高，毕竟咱们是中国人，显示中文title还是很有必要的~所以墙裂建议大伙儿加上啦！

## 二十二、allure的特性，@allure.link()、@allure.issue()、@allure.testcase()的详细使用

### 前言

上一篇文章介绍了两种allure的特性

- @allure.description() ：添加测试用例描述，一共三种方式哦！
- @allure.title()：指定测试用例标题，默认是函数名哦！

这一篇幅，我们主要来讲解最后三个常见特性，主要是为了将allure报告和测试管理系统集成，可以更快速的跳转到公司内部地址

- @allure.link()
- @allure.issue()
- @allure.testcase()

 

### 先看看三个装饰器源码



```python
def link(url, link_type=LinkType.LINK, name=None):
    return safely(plugin_manager.hook.decorate_as_link(url=url, link_type=link_type, name=name))


def issue(url, name=None):
    return link(url, link_type=LinkType.ISSUE, name=name)


def testcase(url, name=None):
    return link(url, link_type=LinkType.TEST_CASE, name=name)
```

#### 知识点

- issue()和testcase()其实调用的也是link()，**只是link_type不一样**
- 必传参数 url：跳转的链接
- 可选参数 name：显示在allure报告的名字，如果不传就是显示完整的链接；**建议传！！不然可读性不高**
- 可以理解成：三个方法是一样的，我们都提供跳转链接和名字，只是链接的type不一样，最终显示出来的**样式不一样****而已【type不一样，样式不一样】**
- 如果你喜欢，只用@allure.link()也可以
- 而出现三个装饰器的原因是为了更好地将**链接分类【访问链接、Bug链接、测试用例链接】**

 

**看完源码和知识点，其实我们就没必要针对三个方法都展开来讲了，直接上代码，看报告的样式区别！**

 

### **代码栗子**



```python
#!/usr/bin/env python
# -*- coding: utf-8 -*-


import allure

TEST_CASE_LINK = 'https://github.com/qameta/allure-integrations/issues/8#issuecomment-268313637'


@allure.link('https://www.youtube.com/watch?v=4YYzUTYZRMU')
def test_with_link():
    pass


@allure.link('https://www.youtube.com/watch?v=Su5p2TqZxKU', name='点击我看一看youtube吧')
def test_with_named_link():
    pass


@allure.issue('140', 'bug issue链接')
def test_with_issue_link():
    pass


@allure.testcase(TEST_CASE_LINK, '测试用例地址')
def test_with_testcase_link():
    pass
```

 

#### 运行结果，查看allure报告

 

#### @allure.link()不传name参数时的样式

**不传name的话，如果链接很长，可读性就比较差啦！** 

![img](../../../blog/source/picture/1896874-20200418172039534-32664525.png)

 

#### @allure.link()传了name参数时的样式

![img](../../../blog/source/picture/1896874-20200418172043072-1700939340.png)

 

#### @allure.testcase()的样式

**其实跟link()没有太大区别.....**

![img](https://img2020.cnblogs.com/blog/1896874/202004/1896874-20200418172045421-315980894.png)

 

#### @allure.issue()的样式

**多了个虫子哈哈哈哈**

![img](https://img2020.cnblogs.com/blog/1896874/202004/1896874-20200418172037397-61372367.png)

 

### 总结

- 为了减少程序的阅读复杂性，其实可以统一用@allure.link()
- 传name，写好链接描述，就知道这个链接是干嘛的啦，反正三个装饰器的作用都是一样的，就是样式略微不同.....

## 二十三、allure 打标记之 @allure.epic()、@allure.feature()、@allure.story() 的详细使用

### 前言

- 前面几篇文章主要介绍了allure的特性，这篇文章我们就来讲下allure的标记用法
- 有时候我们写pytest的时候，会用到 @pytest.mark 但并不会显示在allure报告上
- 而allure也提供了三种类型的标记装饰器，它们是可以显示在allure报告上的

 

### allure的标记装饰器

- BDD样式的标记装饰器
- 优先级（严重程度）标记装饰器
- 自定义标记装饰器

 

### BDD标记装饰器

提供了三个装饰器

- **@allure.epic：**敏捷里面的概念，定义史诗，往下是 feature
- **@allure.feature：**功能点的描述，理解成模块往下是 story
- **@allure.story：**故事，往下是 title

 

### 栗子一（story+feature）

##### 测试代码



```python
#!/usr/bin/env python
# -*- coding: utf-8 -*-

import allure


def test_without_any_annotations_that_wont_be_executed():
    pass


@allure.story('epic_1')
def test_with_epic_1():
    pass


@allure.story('story_1')
def test_with_story_1():
    pass


@allure.story('story_2')
def test_with_story_2():
    pass


@allure.feature('feature_2')
@allure.story('story_2')
def test_with_story_2_and_feature_2():
    pass
```

 

##### 无标记装饰器

**我们先看看没有设置标记装饰器时，allure报告是咋样的**

![img](../../../blog/source/picture/1896874-20200419143545313-1192847935.png)

 ![img](../../../blog/source/picture/1896874-20200419143547509-461702889.png)

 

##### 添加装饰器

**加了 @allure.feature 和 @allure.story 之后的 allure 报告**

![img](../../../blog/source/picture/1896874-20200419143646436-112943561.png)

![img](../../../blog/source/picture/1896874-20200419143700757-1044518704.png)

 

##### 知识点

- story 是 feature 的子集，当测试用例有 @allure.feature、@allure.story 时，在报告上会先显示 feature，点开之后再显示 story**【可以想象成，安徒生童话（feature）有很多个童话故事（story）】**
- 如果不加 @allure.feature、@allure.story 时，在Behaviors栏目下，测试用例都不会分类显示，当用例多的时候可能看的花里胡哨

 

### 栗子二

##### 前言

这里应用了包括前面所讲的全部装饰器

 

##### 测试代码



```python
#!/usr/bin/env python
# -*- coding: utf-8 -*-

import os

import allure
import pytest


@pytest.fixture(scope="session")
def login_fixture():
    print("=== 前置登录 ===")


@allure.step("步骤1")
def step_1():
    print("操作步骤---------------1")


@allure.step("步骤2")
def step_2():
    print("操作步骤---------------2")


@allure.step("步骤3")
def step_3():
    print("操作步骤---------------3")


@allure.epic("epic 相当于总体描述")
@allure.feature("测试模块")
class TestAllureALL:

    @allure.testcase("https://www.cnblogs.com/poloyy/", '测试用例,点我一下')
    @allure.issue("https://www.cnblogs.com/poloyy/p/12219145.html", 'Bug 链接,点我一下')
    @allure.title("用例的标题")
    @allure.story("story one")
    @allure.severity("critical")
    def test_case_1(self, login_fixture):
        """
        小菠萝测试笔记地址：https://www.cnblogs.com/poloyy/
        """
        print("测试用例1")
        step_1()
        step_2()

    @allure.story("story two")
    def test_case_2(self, login_fixture):
        print("测试用例2")
        step_1()
        step_3()


@allure.epic("另一个 epic")
@allure.feature("查找模块")
class TestAllureALL2:
    @allure.story("story three")
    def test_case_3(self, login_fixture):
        print("测试用例3")
        step_1()

    @allure.story("story four")
    def test_case_4(self, login_fixture):
        print("测试用例4")
        step_3()


if __name__ == '__main__':
    pytest.main(['-s', '-q', '--alluredir', './allure'])
    os.system('allure -c ./allure')
    os.system('allure serve ./allure-report')
```

 

##### allure 报告

![img](../../../blog/source/picture/1896874-20201027222800445-1537547444.png)

![img](../../../blog/source/picture/1896874-20201027222813521-133031475.png)

 

### 总结

倘若是用 pytest+allure 写项目，又想用 @pytest.mark.xxx 来给不同的用例添加标记的话，可以尝试用 @allure.feature、@allure.story 替换，毕竟可以显示在报告上

 

##### 提出问题

用命令行方式运行时，可以指定运行某个story、feature、epic吗？

 

##### 自问自答

当然可以，跟 @pytest.mark.xxx 指定标签运行的方式没啥区别，添加下面的命令行参数就行

- --allure-epics
- --allure-features
- --allure-stories

 

##### 举栗子



```python
# 只运行 epic 名为 test 的测试用例
pytest --alluredir ./report/allure --allure-epics=test

# 只运行 feature 名为 模块 的测试用例
pytest --alluredir ./report/allure --allure-features=模块

# 只运行 story1、story2 的测试用例（也可以不用=号 空格就行了哦）
pytest tests.py --allure-stories story1,story2

# 指定 feature+story
pytest tests.py --allure-features feature2 --allure-stories story2
```

##  二十四、allure 环境准备

### allure 和 pytest 相关环境安装



```python
# allure
pip3 install allure-pytest -i http://pypi.douban.com/simple/ --trusted-host pypi.douban.com

#pytest
pip3 install pytest -i http://pypi.douban.com/simple/ --trusted-host pypi.douban.com
```

 

### 配置 allure 环境变量

allure是一个命令行工具，可以去 github 下载最新版：https://github.com/allure-framework/allure2/releases

![img](../../../blog/source/picture/1896874-20201027184939003-1176034393.png)

 

##### 解压到本地

![img](../../../blog/source/picture/1896874-20201027190928087-698454146.png)

 

##### 配置环境变量

![img](../../../blog/source/picture/1896874-20201027190931037-761207454.png)

## @allure.severity 标记用例级别

### 前言

- 平时写测试用例也会划分优先级
- 同样，allure 也提供用例级别，在 allure 报告可以清晰看到不同级别用例的缺陷数量 

 

### 用例等级介绍

##### allure 提供的枚举类

![img](../../../blog/source/picture/1896874-20201028101707576-1311685517.png)

 

##### 等级介绍

- blocker：阻塞缺陷（功能未实现，无法下一步）
- critical：严重缺陷（功能点缺失）
- normal： 一般缺陷（边界情况，格式错误）
- minor：次要缺陷（界面错误与ui需求不符）
- trivial： 轻微缺陷（必须项无提示，或者提示不规范）

 

### 实际栗子

#### 测试代码



```python
#!/usr/bin/env python
# -*- coding: utf-8 -*-

import allure


def test_with_no_severity_label():
    pass


@allure.severity(allure.severity_level.TRIVIAL)
def test_with_trivial_severity():
    pass


@allure.severity(allure.severity_level.NORMAL)
def test_with_normal_severity():
    pass


@allure.severity(allure.severity_level.NORMAL)
class TestClassWithNormalSeverity(object):

    def test_inside_the_normal_severity_test_class(self):
        """ 测试类优先级 normal；看看测试用例是否会自动继承优先级 """
        print()

    @allure.severity(allure.severity_level.CRITICAL)
    def test_inside_the_normal_severity_test_class_with_overriding_critical_severity(self):
        """
        测试类优先级 normal
        测试用例优先级 critical
        """
        pass


@allure.severity("normal")
def test_case_1():
    """ normal 级别测试用例 """
    print("test case 11111111")


@allure.severity("critical")
def test_case_2():
    """ critical 级别测试用例 """
    print("test case 222222222")


@allure.severity("blocker")
def test_case_3():
    """ blocker 级别测试用例 """
    print("test case 4444444")


@allure.severity("minor")
def test_case_4():
    """ minor 级别测试用例 """
    print("test case 11111111")


def test_case_5():
    """ 没标记 severity 的用例默认为 normal"""
    print("test case 5555555555")
```

 

#### allure 报告

##### 测试用例详情

![img](../../../blog/source/picture/1896874-20201028102517170-1478952294.png)

多了个 severity 字段

 

##### 统计图表

![img](../../../blog/source/picture/1896874-20201028102539419-243158042.png)

可以看到不同 severity 测试用例运行的统计数据

 

#### 必然测试失败的测试代码

将上面代码的三个测试用例故意让它测试失败



```python
@allure.severity("normal")
def test_case_1():
    """ normal 级别测试用例 """
    assert (1 == 2)


@allure.severity("critical")
def test_case_2():
    """ critical 级别测试用例 """
    assert (1 == 2)


@allure.severity("blocker")
def test_case_3():
    """ blocker 级别测试用例 """
    assert (1 == 2)
```

 

##### 再来看看 allure 报告的统计图表

![img](../../../blog/source/picture/1896874-20201028102738341-707019425.png)

- 这里用的是中文报告，其实可以看到 severity 官方是翻译为优先级，但是如果自己去翻译软件翻译的话是严重程度，我个人更偏向于理解为优先级
- 会同时显示同一个优先级的失败、通过用例数，以及哪条用例是失败、通过的

 

### 命令行参数 allure-severities

当然，也可以根据优先级选择需要运行的测试用例

 

##### 具体栗子

仍然是上面的代码，打开 cmd



```python
# 只运行 severity=blocker、critical 的测试用例
pytest test_severity.py -sq --alluredir=./allure --allure-severities=blocker,critical

# 写法二
pytest test_severity.py -sq --alluredir=./allure --allure-severities blocker,critical
```

 

##### 运行结果

![img](../../../blog/source/picture/1896874-20201028103226885-438391816.png)

severi=blocker、critical 的测试用例就三条，可以看看上面的代码

## 二十五、清空 allure 历史报告记录

### 背景

- pytest 运行 测试用例生成 allure 报告时，当测试用例名称修改后重新运行，会保留历史运行记录
- 又或者分开运行两个测试用例文件，但是 allure 报告生成目录是同一个，那么 allure 报告会同时显示两个文件的测试用例运行情况
- 咱们来看看这种情况

 

#### 目录结构

下面两个栗子都是这个目录结构

![img](../../../blog/source/picture/1896874-20201028110652149-1920030839.png)

 

#### 指定测试用例文件名称的栗子

##### test_1.py 的代码



```python
#!/usr/bin/env python
# -*- coding: utf-8 -*-



def test_1():
    print("test_1 文件的测试用例1")


def test_2():
    print("test_1 文件的测试用例2")
```

 

##### 运行命令

进入该目录下，cmd 运行



```python
pytest test_1.py --alluredir=./allure
```

 

##### allure 报告

![img](../../../blog/source/picture/1896874-20201028111346609-886062831.png)

只有两条用例

 

##### 修改后的 test_1.py 的代码



```python
def test_11():
    print("test_1 文件的测试用例1")


def test_22():
    print("test_1 文件的测试用例2")
```

 

##### 再次运行命令，查看 allure 报告

![img](../../../blog/source/picture/1896874-20201028111355331-1963122828.png)

四条用例，包含了历史的两条，这不是我们希望看到的

 

#### 分开运行测试用例文件的栗子

##### test_2.py 的代码



```python
#!/usr/bin/env python
# -*- coding: utf-8 -*-



def test_1():
    print("test_1 文件的测试用例1")


def test_2():
    print("test_1 文件的测试用例2")
```

 

##### 分开运行 test_1 和 test_2 两个测试用例文件



```python
# 先运行第一个
pytest test_1.py --alluredir=./allure

# 再运行第二个，此时应该希望 allure 报告只有 test_2.py 的测试用例
pytest test_2.py --alluredir=./allure
```

 

##### 查看 allure 报告

![img](../../../blog/source/picture/1896874-20201028112004419-420743254.png)

出现了 test_1.py 的测试用例，这不是想要的结果

 

### --clean-alluredir 参数

##### 前言

- pytest 提供了 --clean-alluredir 参数可以清空 allure 报告生成的目录
- 可以看看 pytest 的说明文档

 



```python
pytest -h
```

![img](../../../blog/source/picture/1896874-20201028112503115-1419060590.png)

 

##### 将上面的栗子重新运行



```python
# 先运行第一个
pytest test_1.py --alluredir=./allure

# 再运行第二个，此时应该希望 allure 报告只有 test_2.py 的测试用例
pytest test_2.py --alluredir=./allure --clean-alluredir
```

 

##### 运行结果

![img](../../../blog/source/picture/1896874-20201028113553241-1279045150.png)

## 二十六、allure 命令行参数

### 先看看 allure 命令的帮助文档

cmd 敲



```python
allure -h
```

 

##### allure 命令的语法格式



```python
allure [options] [command] [command options]
```

 

##### options 列表



```python
  Options:
    --help 命令行帮助文档
    -q, --quiet
      切换至安静模式
      Default: false
    -v, --verbose
      切换至冗长模式
      Default: false
    --version
      版本信息
      Default: false
```

 

##### command 列表

1. generate
2. serve
3. open
4. plugin

这里只讲前三个常用的

 

### generate 命令行参数

##### 作用

生成 allure 的html 报告

 

##### 语法格式



```python
generate [options]  allure 结果目录
```

 

**注：**allure 结果目录就是运行 pytest 命令，--alluredir 跟的那个目录



```python
pytest -sq --alluredir= ./allure
```

 

##### 命令选项

![img](../../../blog/source/picture/1896874-20201028123446680-921838785.png)

主要就是用 -c 、 -o 两个参数

 

### open 命令行参数

##### 作用

打开生成的 allure 报告，就是打开 generate 命令生成的报告

 

##### 语法格式



```python
open [options] allure报告目录
```

 

**注：**allure 报告目录就是运行 allure generate 命令，-o 跟的那个目录



```python
allure generate -o ./allure-report
```

 

##### 命令选项

![img](../../../blog/source/picture/1896874-20201028141707575-281214400.png) 

### serve 命令行参数

##### 作用

启动 allure 服务，打开 allure 报告

 

##### 语法格式



```python
serve [options] allure 结果目录
```

 

**注：**allure 结果目录就是运行 pytest 命令，--alluredir 跟的那个目录



```python
pytest -sq --alluredir= ./allure
```

 

##### 命令选项

![img](../../../blog/source/picture/1896874-20201028142713421-258584445.png)

 

### 浏览器打开 allure 报告的两种方式

#### allure serve

标准写法



```python
# 执行 pytest，指定 allure 结果目录
pytest -sq --alluredir=./allure

# 打开 allure 报告
allure serve ./allure
```

 

#### allure generate + allure open

 标准写法



```python
# 执行 pytest，指定 allure 结果目录
pytest -sq --alluredir=./allure

# 生成 allure 的 html 报告
allure generate -c -o ./allure-report ./allure

# 打开 allure 报告
allure open ./allure-report
```

当然不写 -o 也可以

 

##### 看看 allure-report 的目录结构

![img](../../../blog/source/picture/1896874-20201028143452978-709464647.png)

- 这种方式的目录会好看很多，不只是一堆 json 文件
- 而且直接打开 index.html 也是能看到 allure 报告的

## 二十七、参数化 parametrize + @allure.title() 动态生成标题

### 前言

- 参数化 @pytest.mark.parametrize 的学习：https://www.cnblogs.com/poloyy/p/12675457.html
- 默认 allure 报告上的测试用例标题不设置**默认**就是用例名称，这样可读性不高
- 当**结合** @pytest.mark.parametrize 参数化完成数据驱动时，如果标题写死，这样可读性也不高
- 所以我们希望标题可以动态的生成，来看看如何做吧

 

### 参数化无标题的栗子

##### 测试代码



```python
#!/usr/bin/env python
# -*- coding: utf-8 -*-

import allure
import pytest


@pytest.fixture()
def login(request):
    """登录"""
    param = request.param
    print(f"账号是：{param['username']}，密码是：{param['pwd']}")
    # 返回
    return {"code": 0, "msg": "success!"}

datas = [
    {"username": "name1", "pwd": "pwd1"},
    {"username": "name2", "pwd": "pwd2"},
    {"username": "name3", "pwd": "pwd3"}
]

@allure.story('登录功能')
@pytest.mark.parametrize('login', datas, indirect=True)
def test_login1(login):
    """
    登录测试用例1
    """
    assert login['code'] == 0
```

 

##### allure 报告

 ![img](../../../blog/source/picture/1896874-20201028155125698-768492106.png)

标题就是方法名+参数化的数据，看着可读性就不咋滴

 

### 参数化有标题写死的栗子

##### 测试代码

将上面的测试代码添加一个 @allure.title 就可以了



```python
@allure.story('登录功能')
@allure.title('登录测试用例2')
@pytest.mark.parametrize('login', datas, indirect=True)
def test_login2(login):
    """
    登录测试用例2
    """
    assert login['code'] == 0
```

 

##### allure 报告

![img](../../../blog/source/picture/1896874-20201028155328645-761365730.png)

因为参数化可以生成三条用例，所以三条用例都用了同一个 title，可读性也不咋滴

 

### 参数化使用 ids 的栗子

##### 测试代码



```python
#!/usr/bin/env python
# -*- coding: utf-8 -*-

import allure
import pytest


@pytest.fixture()
def login(request):
    """登录"""
    param = request.param
    print(f"账号是：{param['username']}，密码是：{param['pwd']}")
    # 返回
    return {"code": 0, "msg": "success!"}

datas = [
    {"username": "name1", "pwd": "pwd1"},
    {"username": "name2", "pwd": "pwd2"},
    {"username": "name3", "pwd": "pwd3"}
]

ids = [
    "username is name1,pwd is pwd1",
    "username is name2,pwd is pwd2",
    "username is name3,pwd is pwd3"
]

@allure.story('登录功能')
@pytest.mark.parametrize('login', datas, ids=ids, indirect=True)
def test_login1(login):
    """
    登录测试用例1
    """
    assert login['code'] == 0
```

 

##### allure 报告

![img](../../../blog/source/picture/1896874-20201028162540505-406268594.png)

 

### 参数化动态生成标题的栗子

##### 测试代码



```python
#!/usr/bin/env python
# -*- coding: utf-8 -*-

import allure
import pytest


@pytest.fixture()
def login(request):
    """登录"""
    param = request.param
    print(f"账号是：{param['username']}，密码是：{param['pwd']}")
    # 返回
    return {"code": 0, "msg": "success!"}


datas = [
    {"username": "name1", "pwd": "pwd1"},
    {"username": "name2", "pwd": "pwd2"},
    {"username": "name3", "pwd": "pwd3"}
]

data2 = [
    ("name1", "123456"),
    ("name2", "123456"),
    ("name3", "123456")
]


@allure.story('分别传值')
@allure.title('登录测试用例2-账号是：{username}-密码是：{pwd}')
@pytest.mark.parametrize('username,pwd', data2)
def test_login1(username, pwd):
    """
    登录测试用例1
    """
    print(username, pwd)


@allure.story('字典参数化')
@allure.title('登录测试用例2-{dict}')
@pytest.mark.parametrize('dict', datas)
def test_login2(dict):
    """
    登录测试用例1
    """
    print(dict['username'], dict['pwd'])


@allure.story('传值进fixture')
@allure.title('登录测试用例2{login}')
@pytest.mark.parametrize('login', datas, indirect=True)
def test_login3(login):
    """
    登录测试用例2
    """
    assert login['code'] == 0
```

 

##### allure 报告

![img](../../../blog/source/picture/1896874-20220228212938514-1376266261.png)

传入的如果是一个字典则显示完整字典值

 

### 参数化动态生成标题最优方案的栗子

##### 测试代码



```python
#!/usr/bin/env python
# -*- coding: utf-8 -*-

import allure
import pytest

data = [
    ("name1", "123456", "name1 登录成功"),
    ("name2", "123456", "name2 登录失败"),
    ("name3", "123456", "name3 登录成功")
]


@allure.story('分别传值')
@allure.title('登录测试用例-{title}')
@pytest.mark.parametrize('username,pwd,title', data)
def test_login1(username, pwd, title):
    """
    登录测试用例1
    """
    print(username, pwd)
```

 

##### allure 报告

![img](../../../blog/source/picture/1896874-20201028173259929-297643896.png)

 

##### 这种做法的优点

- 可以自定义各式各样的标题
- 单独一个值去维护标题值
- 可读性比较好，容易维护

## 二十八、详解 allure.dynamic 动态生成功能

### 前言

-  @allure.title 和 @allure.description 都是装饰器，给测试用例提供标题和描述
- 其实 allure 还提供了在测试用例执行过程中动态指定标题和描述等标签的方法
- 如： allure.dynamic.description allure.dynamic.title 

 

### allure.dynamic 的源代码



```python
class Dynamic(object):

    @staticmethod
    def title(test_title):
        plugin_manager.hook.add_title(test_title=test_title)

    @staticmethod
    def description(test_description):
        plugin_manager.hook.add_description(test_description=test_description)

    @staticmethod
    def description_html(test_description_html):
        plugin_manager.hook.add_description_html(test_description_html=test_description_html)

    @staticmethod
    def label(label_type, *labels):
        plugin_manager.hook.add_label(label_type=label_type, labels=labels)

    @staticmethod
    def severity(severity_level):
        Dynamic.label(LabelType.SEVERITY, severity_level)

    @staticmethod
    def feature(*features):
        Dynamic.label(LabelType.FEATURE, *features)

    @staticmethod
    def story(*stories):
        Dynamic.label(LabelType.STORY, *stories)

    @staticmethod
    def tag(*tags):
        Dynamic.label(LabelType.TAG, *tags)

    @staticmethod
    def link(url, link_type=LinkType.LINK, name=None):
        plugin_manager.hook.add_link(url=url, link_type=link_type, name=name)

    @staticmethod
    def issue(url, name=None):
        Dynamic.link(url, link_type=LinkType.ISSUE, name=name)

    @staticmethod
    def testcase(url, name=None):
        Dynamic.link(url, link_type=LinkType.TEST_CASE, name=name)
```

 

##### 重点

上面有的方法都能进行动态修改，如：



```
allure.dynamic.feature
allure.dynamic.link
allure.dynamic.issue
allure.dynamic.testcase
allure.dynamic.story
allure.dynamic.title
allure.dynamic.description
```

 

### title 的栗子

##### 测试代码



```python
@allure.title("装饰器标题")
def test_1():
    print(123)
    allure.dynamic.title("动态标题")
```

 

##### allure 报告

![img](../../../blog/source/picture/1896874-20201029124423518-305611670.png)

### description 的栗子

##### 测试代码



```python
def test_1():
    """
    动态设置描述
    """
    print(123)
    allure.dynamic.description("动态描述")
    allure.dynamic.title("动态标题")
```

 

##### allure 报告

![img](../../../blog/source/picture/1896874-20201028220350336-1102469402.png)

可以看到**动态描述**会覆盖**动态设置描述**

 

### 结合 parametrize

##### 测试代码



```python
data = [
    ("name1", "123456", "name1 登录成功"),
    ("name2", "123456", "name2 登录失败"),
    ("name3", "123456", "name3 登录成功")
]


@pytest.mark.parametrize('username,pwd,title', data)
def test_2(username, pwd, title):
    """
    登录测试用例1
    """
    print(username, pwd)
    allure.dynamic.title(title)
```

 

##### allure 报告

![img](../../../blog/source/picture/1896874-20201029124815524-464475328.png)

 

### 其他属性的栗子

##### 测试代码



```python
def test_2():
    allure.dynamic.feature('动态feature')
    allure.dynamic.story('动态story')
    allure.dynamic.link("https://www.cnblogs.com/poloyy/p/1.html", '动态Link')
    allure.dynamic.issue("https://www.cnblogs.com/poloyy/p/2.html", '动态Issue')
    allure.dynamic.testcase("https://www.cnblogs.com/poloyy/p/3.html", '动态testcase')
```

 

##### allure 报告

![img](../../../blog/source/picture/1896874-20201029130901391-441197616.png)

