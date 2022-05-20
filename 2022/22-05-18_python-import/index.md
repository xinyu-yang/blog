# Python relative import 深入理解


<!--more-->

随着程序的复杂，会逐渐将不同的功能放置在不同的文件中。项目也从简单脚本的单一文件转变为更加复杂的文件结构，这就涉及到不同文件的导入问题。

以前写的程序都比较简单，即使有多个文件，但也是单层次的扁平结构，操作起来也比较简单，尝试一下就可以正确导入。

但是当文件有了更复杂的层次之后，发现文件导入并没有自己想象的那么简单。每次都是盲人摸象般的盲目尝试，直到程序不会报错。靠着这种开盲盒般的操作方式，即使最后得出正确结果，也不知道怎么回事。

也看过一些关于文件导入的文章，但一是没有自己的实际操作；二是很多文章讲的比较肤浅，不涉及本质；甚至有的文章还会给读者错误的导向，害人不浅（这一点后面会说）。

## 从 `ModuleNotFoundError` 说起

一般报该错误，常见的有两个原因：

1. 如果是第三方包，那么是相应的模块没有安装，直接 `pip install *` 安装即可。
2. 如果自己写的程序，那么就是包导入的路径不对。本文主要关注的也是该问题。

如上所述，如果是第二种情况，即包导入的位置不对。那么我们就应该添加包的查询位置，`sys.path` 就是用来指定包查找位置的列表。

下面将以下述文件结构举例：

```bash
src
	├── foo
	│   ├── foo.py
	│   ├── bar.py
	│   ├── __init__.py
	├── hello.py
	├── main.py
```

这里 `hello.py` 中的代码为：

```python
def helloworld:
    print("hello world")
```

`bar.py`中的代码为：

```python
def bar:
    print("This is in bar")
```

接下来试图从`foo.py` 中导入该函数：

```python
# foo.py
import hello

hello.helloworld()
```

该程序理所当然报 `ModuleNotFoundError`，这是因为包的位置没有被添加到导入路径中，前面也提到，`sys.path`其实是一个列表，所以我们只需将需要的路径添加到该列表中即可。接下来修改代码如下（很多博客中写的都是此方法）：

```python
# foo.py
import sys
sys.path.append("..")
import hello

hello.helloworld()
```

如果我们在路径 `foo` 下运行：

```bash
$ python foo.py
```

那么程序可以顺利执行。**但是**如果我们在程序根目录下执行：

```bash
$ python ./foo/foo.py
```

那么依然会报 `ModuleNotFoundError` 。经过分析，发现这是因为我们在添加文件路径到 `sys.path` 的时候，加入的只是相对路径。

而相对路径是相对于现在的执行路径而言的，当前的执行目录为根目录，根目录的上一层当然没有要找的 `hello`模块，所以程序当然报错。

所以合理的方式是直接导入绝对路径，那么无论在哪里运行，都是可以找到相应的模块的，具体代码如下：

```bash
# foo.py
import sys, os
sys.path.append(os.path.join(sys.path[0], ".."))
import hello

hello.helloworld()
```

其中`sys.path[0]`通过阅读[官方文档](https://docs.python.org/3/library/sys.html)可以得知，其代表启动 Python 解释器的脚本的目录路径。

这里就把文件的绝对路径以相对路径（`".."`）的方式添加到了查找列表中。添加后的 `sys.path` 如下：

```bash
['**/src/foo', '/usr/local/lib/python38.zip', '/usr/local/lib/python3.8', '/usr/local/lib/python3.8/lib-dynload', '/home/yang/ML/py4flink/lib/python3.8/site-packages', '**/src/foo/..']
```

## 相对导入 Relative Import

按照上述方式可以将模块整个导入，但是有时候我们并不想直接导入整个包，而只想导入包的一部分，比如 `hello.py`文件下 `helloworld` 函数。那么我们那么尝试着只导入该函数：

```python
# foo.py
import sys, os
sys.path.append(os.path.join(sys.path[0], ".."))
from hello import helloworld

helloworld()
```

通过测试发现，不管是在根目录还是在 `foo`目录下都可以正常运行。

接下来我们尝试着从`bar.py`中导入`bar` 函数：

```python
# foo.py
import sys, os
from bar import bar

bar()
```

同样，不管从根目录还是从 `foo` 目录下运行 `foo.py`都是没有问题的。

此时我们尝试另外一种操作，假设在根目录下的 `main.py` 文件中需要调用 `foo.py`中的某个函数 `foo` ，其代码如下：

```python
# foo.py
import sys, os
sys.path.append(os.path.join(sys.path[0], ".."))
from bar import bar

def foo():
    bar()
    print("This is in foo")
```

这是一个很常见的操作，我们需要在 `main`文件下调用包里的某个文件，然后包里的文件又是相互调用的关系。然后我们尝试在 `main` 文件中导入该函数，代码如下：

```python
# main.py
from foo import foo

foo.foo()
```

然后程序居然会报如下错误：

```bash
 File "**src/foo/foo.py", line 6, in foo
    from bar import bar
ModuleNotFoundError: No module named 'bar'
```

这是为什么呢？按理说 `foo.py`和 `bar.py`同属一个目录，直接从本目录下导入应该没有问题吧，为解决该问题，我们先打印出一些信息，在`foo.py`中添加一些调试代码，同时将导入`bar`的操作后移：

```python
# foo.py
import sys, os
sys.path.append(os.path.join(sys.path[0], ".."))

def foo():
    print("This is in foo")
    print(sys.path)
    print(f'{__name__}, __file__: {__file__}, __package__: {repr(__package__)}')
    from bar import bar
    bar()
```

再次执行 `main` 程序，在报错之前，打印出如下结果：

```bash
['**/src', '/usr/local/lib/python38.zip', '/usr/local/lib/python3.8', '/usr/local/lib/python3.8/lib-dynload', '/home/yang/ML/py4flink/lib/python3.8/site-packages', '**/src/..']
__main__, __file__: **/src/foo/foo.py, __package__: 'foo'
```

发现导入文件的路径变成了根目录，所以无法找到`bar`文件的路径了。为了验证该猜想，我们手动在`main`文件中把`bar`文件的导入路径加上：

```python
# main.py
import os, sys
sys.path.append(os.path.join(sys.path[0], "foo"))
from foo import foo

foo.foo()
```

运行 `main` 程序，顺利执行！

但是在写程序的时候，我们不可能像这样把所有包中的文件绝对引用路径全都加上。我们还是希望不管被哪个程序导入，包里所有文件的相对引用位置是不变的。

通过阅读官方文档得知，我们可以采用包相对导入来解决这个问题。相对导入的形式类似于`bash`中的相对路径，通过在包前面加`.`来表示相对位置，一个代表本目录，两个代表上级目录，三个代表。。。

根据该方案，我们将 `foo.py`中的导入操作修改如下：

```python
from .bar import bar
```

再次运行 `main` 程序，顺利执行！

此时好像一切都没有问题了，然后我们尝试着再次执行 `foo.py` ，发现居然又报错了，这次错误变成了：

```bash
File "foo/foo.py", line 8, in foo
    from .bar import bar
ImportError: attempted relative import with no known parent package
```

加上一个点之后居然就导入不了了！经过网上搜索，找到了一些解决方案【[1](https://stackoverflow.com/a/28154841)】【[2](https://iq-inc.com/importerror-attempted-relative-import/)】。

但是还是不太懂根本原因，后来看了[官方文档](https://docs.python.org/3/reference/import.html)和[该回答](https://stackoverflow.com/a/21233334)中关于`__package__`变量的解释，我才大概搞明白是怎么回事。其中一些重要的摘录如下：

```html
__package__:
...
This attribute is used instead of __name__ to calculate explicit relative imports for main modules, as defined in PEP 366. It is expected to have the same value as __spec__.parent.

Changed in version 3.6: The value of __package__ is expected to be the same as __spec__.parent.
```

大概意思就是该变量用于计算相对导入的位置，并且该变量的值必须和`__spec__.parent`的值相同。

根据上文可知，当我们执行`main.py`的时候，我们所打印出的`__package__`值为`'foo'` 而当我们单独执行 `foo.py`的时候，其打印出的值为：

```bash
__main__, __file__: foo/foo.py, __package__: None
```

所以我认为根本的原因是：当单独执行`foo.py`的时候，其所在目录下的文件并不被当作 python module，而只是被当成是普通的脚本进行执行。因此相应的`__package__`值为`None` ，进而进行relative import的时候，因不满足上述条件，所以报错。

而当从`main.py`中导入的时候，`foo.py`目录中的文件都被当成是`foo`包的一部分，所以可以使用relative import导入成功。

那么能不能以模块的方式运行`foo.py`呢？答案是可以的，只需要在执行的时候加上`-m`选项，比如：

```bash
$ python -m foo.foo
```

那么`foo.py`就会被当成一个模块来执行。同时我还从[该回答](https://stackoverflow.com/a/28151907)中学到一种方式：

```bash
if __name__ == '__main__':
    from bar import bar
else:
    from .bar import bar
```

这样就可以避免出错了。

至此，大功告成！

## 总结

最后说些总结：

1. 参考[该文](https://iq-inc.com/importerror-attempted-relative-import/)，除`main.py`等少数包之外，尽量将所有被调用的文件组织成包的形式。
2. 尽量采用`import`直接导入本地文件，而不是`from * import *`
3. 如果一定要使用`from * import *`，对于相同路径下的文件，一定要根据是否封装为模块选择合适的导入方式，或者是像上述所说，加上一个判断。

