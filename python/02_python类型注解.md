# 02 python类型注解


# python类型提示

Python 运行时不强制执行函数和变量类型注解，但这些注解可用于类型检查器、IDE、静态检查器等第三方工具。（换句话说，本文下面所说的类型，都是供代码检查器，IDE提示用的，并不是说你设置了类型，就真的变成像 java 那样的强类型定义语言了）

# 简单入门

python 是一门动态语言，因此在你拿到一个变量之前，你无法知道一个变量到底是什么类型的，因此当你传递一个变量给一个函数，尽管你内心知道这个参数会是某个特定的类型，但是 IDE 不知道它会是什么类型，因此它不会给你任何的代码提示。

因此，Python 3.5、3.6 新增了两个特性 PEP 484 和 PEP 526，帮助 IDE 为我们提供更智能的提示（并不会影响语言本身），简单的写法：

```python
# 声明变量时使用
num: int = 1
str1: str
    

# 函数参数也可以使用
def test(num: int = 1):
    return num
```

> 在定义一个变量时，直接在后面 `: 类型` 就可以给这个变量指定一个类型，后面可以继续写赋值语句 `= 1`，不写赋值语句也不会出错

## 指定函数返回值类型

```python
def test(num: int = 1) -> int:  # -> 可以指明函数的返回值类型
    return num


def test(num: int = 1) -> None:  # -> 可以指明函数的返回值类型
    return None
```

> 在函数的冒号之前， `->` 可以指定函数的返回值类型
> 注意，None 是一种类型提示特例

## 查看函数注解

我们可以通过 `__annotations__` 查看一个函数的注解

```python
def test(a: str, b: int = 2) -> None:
    pass

print(test.__annotations__)
{'a': <class 'str'>, 'b': <class 'int'>, 'return': None}
```

## 常用类型

- int, long, float: 整型,长整形,浮点型;
- bool, str: 布尔型，字符串类型；
- list, tuple, dict, set: 列表，元组，字典, 集合;
- Iterable,Iterator: 可迭代类型，迭代器类型；
- Generator: 生成器类型；

> python3.9以后可以直接使用 list, dict, set, tuple 等类型，3.9 以前可能需要 `from typing import List, Dict, Set, Tuple`
>
> py3.9 需要从 `from collections.abc import Iterable, Generator, Iterator`，3.9以前需要：`from typing import Iterable, Generator, Iterator`

### list，set

list，set 类型提示，只可以给它们的内部元素指定**一种类型**，否则会报错：

```python
a: list[int] = []  # list[类型] ，list内部只能指定一种类型

a.append(1)
a.append('2')  # 尽管指定了 int 类型，但是可以添加 str 类型的元素。

# ---------------  同理： set[] ----------------------------

b: set[int] = {1}
```

> 我们之前说过，类型提示只是进行提示用的，并不会真的校验我们给它的赋值。
>
> list，set后面跟的是中括号： `[]`

### tuple

tuple 可以指定多种类型。

```python
c: tuple[int, str] = (1, '2')  # 元组的第一个元素是 int 类型，第二个元素是 str 类型
```

如果想给 tuple 指定一种类型，但是你 tuple 的长度大于1，需要加上 `...`

```python
c: tuple[int, ...] = (1, 2, 3, 4)  # 代表无论元组有多少元素，都是 int 类型
```

### dict

dict 可以指定两种类型：分别代表键，值的类型：

```python
d: dict[str, int] = {'1': 1}
```

### 嵌套类型

各个类型之间也可以嵌套：

```python
e: dict[str, list[int]] = {'1': [1,2]}
```

### 类型的别名

其实类型的重命名，应该属于python的用法，不是类型提示的特殊语法：

```python
My_type = dict[str, list[int]]  # 给类型重命名
e:  My_type = {'1': [1,2]}
```

### TypeAlias 别名

和上面别名一样，只不过显式的说明，这个一个别名。

```python
from typing import TypeAlias

Factors: TypeAlias = list[int]  # 相当于 Factors = list[int]
    
x: Factors = [1,2,3]
```

### 自定义类型：NewType

和重命名有点类似，但是自定义类型相当于创建了某个类型的子类

```python
UserId = NewType('UserId', int)  # 创建一个新类型(类型名字，类型)

num: UserId = UserId('1')
print(num, type(num))  # 1 <class 'str'>
```

> 可以看出，真实的数据类型还是 `str`, 说明了类型提示并不会对真实的代码产生影响，只是IDE类型提示时会显示该填入某种类型。

### Literal

字面量类型，只接受指定的内容。

```python
from typing import Literal

M = Literal['r','w','rb']  # 用户输入别的内容，会进行代码提示

def test(mode: M):
    pass

test('xb')  # Expected type 'Literal['r', 'w', 'rb']', got 'Literal['xb']' instead 
```

### Optional

这个类型代表的是当前参数是一个可选参数，只能接受一种类型，`Optional[X]` 等同于 `X | None` (或者`Union[X, None]`)，也就说是，它默认包含了 None：

```python
from typing import Optional

def test(x: Optional[int]=1):
    return x
```

### Callable

`Callable[[Arg1Type, Arg2Type], ReturnType]` 可以用来提示一个可调用函数：

```python
from typing import Callable

def test(a: int, b: int) -> int:  # 第一个函数，接受两个 int 参数，返回 int
    return a+b

def test2(func: Callable[[int, int], int]):  # 第二个函数，类型是可调用对象：这个可调用对象的参数是 2 个int, 返回值是一个 int
    return func(1, 2)

test2(test)
```

> Callable 接受两个值，第一个值是包含可调用对象的所有参数类型的列表。第二个值是可调用对象的返回值类型。
>
> 如果调用对象没有参数，或者有放多参数，可以这样写：
>
> `Callable[[], int]` 无参数，返回 int
>
> `Callable[..., int]` 不限定参数，返回 int

### ClassVar 类变量

声明类变量的类型

```shell
class Starship:
    stats: ClassVar[dict[str, int]] = {} # class variable
    damage: int = 10                     # instance variable
```

## 泛型 Generic

```python
from typing import TypeVar, Generic

T = TypeVar('T')  # 声明一个任意类型

# A 是一个有类型的类
class A(Generic[T]):  # 继承自 Generic
    def __init__(self, value: T):  # 初始化值：T 类型
        self.value = value

    def get_value(self) -> T:  # 返回 T 类型
        return self.value

    def set_value(self, value: T):  # 设置的值：T 类型
        self.value = value


def test(a: A[int]):  # test 函数接受的参数，要是 A[int] 类型
    print(a.value)
    a.set_value('str1')  # 会提示：Expected type 'int' (matched generic type 'T'), got 'str' instead
    print(a.get_value())


a = A[str]('str1')  # 实例化一个 str 类型的 A 对象
# b = A[int](2)  # 也可以实例化一个 int 类型的 A 对象
test(a)

```

> 上面的例子中，A 类继承了 `Generic[T]` ，代表了 A 类是一个**有类型的类**。我们在实例化它时，可以**随意**指定它的类型：`A[int](2)` ,但是一旦我们指定了某个类型，这个类中所有的 `T`类型 都会和实例化时的类型一致：`int`
>
> 因此在 `test(a: A[int])` 函数中，我们指定了要接受 `A[int]` 类型的参数。而我们传递的却是 `A[str]("str1")` 对象。所以调用 `a.set_value("str1")` 时，IDE 会提示类型不正确（但是代码能跑通，还是之前说的，它仅仅是做类型提示，不会真的影响赋值操作）

## 多类型

### Any

任意类型

```python
from typing import Any

a: Any = None
a = []          # OK
a = 2           # OK
```

### TypeVar

TypeVar 可以指定多种类型，或者任意类型

**任意类型**

```python
from typing import TypeVar, list

T = TypeVar("T")  # 代表了任意类型

x: T = 2
y: T = 'abc'
```

**指定类型**

```python
from typing import TypeVar, list


My = TypeVar('My', int, str, list[int])  # 自定义一个类型，可以是 int 或 str 或 list[int]

xx: My = 'ss'
yy: My = [1]
```

> 注意这里的 `TypeVar()` ，后面用的是 `()` ，不是 `[]`

### Union

Union 也可以指定多种类型。

```python
from typing import TypeVar, list, Union

x:Union[int, str] = '1'  # 可以是 int 或 str 类型
```

Union 是不分顺序的，并且如果有多个重复的类型，会自动去重：

```python
print(
    Union[int, str, int, Union[int, list[int]]] == Union[list[int], int, str]
)

# True
```

> 这个例子中，重复的 `int` 会自动去重，并且里面类型的顺序也不重要。等式前面的Union中还嵌套了一个 `Union[int, list[int]]` ，也会被提取出来

### @overload

有一些函数经常使用 `@overload` 装饰器，这其实也是类型提示的一种：

```python
from typing import overload


@overload
def test(a: int):  # 仅用来进行参数的类型检查，说明它支持 int 类型
	...


@overload
def test(a: str):  # 仅用来类型检查，说明支持 str
	...


def test(a):  # 很重要，必须写一个不使用的正常的函数。这个函数才是真正被调用的函数。
	print(type(a))


test(2)
```

> `@overload` 装饰器可以修饰支持多个不同参数类型组合的函数或方法。后续必须紧跟一个非 @overload 装饰定义的同样的函数（）。@overload-装饰定义仅是为了协助类型检查器， 因为该装饰器会被非 @overload-装饰定义覆盖
