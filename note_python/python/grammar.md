# python基础语法



### 列表推导

```python
devices = [mx.gpu(i) for i in range(mx.context.num_gpus())]
```

> 当你执行 `devices = [mx.gpu(i) for i in range(mx.context.num_gpus())]` 这段代码时，它使用了列表推导式。让我们逐步解释：
>
> 1. `range(mx.context.num_gpus())`: 这部分创建了一个整数范围，从0到 `mx.context.num_gpus() - 1`，这个范围表示可用 GPU 的索引。
>
> 2. `mx.gpu(i) for i in ...`: 这是列表推导式的语法。它使用上一步中生成的每个索引 `i` 来创建一个 MXNet GPU 设备对象，即 `mx.gpu(i)`。
>
> 3. 最终，整个表达式被包装在方括号 `[]` 中，表示这是一个列表。因此，`devices` 将包含所有可用 GPU 的 MXNet GPU 设备对象。
>
> 这段代码的目的是动态地获取并存储系统上所有可用的 GPU 设备。这在深度学习中很有用，因为你可以根据系统配置在训练模型时选择使用特定的 GPU。例如，你可以使用 `ctx=devices[0]` 将模型指定在第一块 GPU 上运行。这种方式使得代码更具有灵活性，可以适应不同系统上的不同 GPU 数量。



### with 关键字

> ```python
> """
> with 语句实质是上下文管理。
> 1、上下文管理协议。包含方法__enter__() 和 __exit__()，支持该协议对象要实现这两个方法。
> 2、上下文管理器，定义执行with语句时要建立的运行时上下文，负责执行with语句块上下文中的进入与退出操作。
> 3、进入上下文的时候执行__enter__方法，如果设置as var语句，var变量接受__enter__()方法返回值。
> 4、如果运行时发生了异常，就退出上下文管理器。调用管理器__exit__方法。
> """
> ```



##### **kwargs

```
def __init__(self, A1, A2, A3, K, N, F, Tr, Td, Tw, Tp, **kwargs):
```

> 在 Python 中，`**kwargs` 是一种用于处理函数参数的特殊语法。它允许函数接受不定数量的关键字参数，并将它们打包成一个字典。
>
> 下面是一个简单的例子：
>
> ```python
> def example_function(**kwargs):
>     for key, value in kwargs.items():
>         print(f"{key}: {value}")
> 
> # 使用函数并传递关键字参数
> example_function(name="John", age=25, city="New York")
> ```
>
> 在这个例子中，`**kwargs` 将关键字参数收集到一个字典中，使得函数能够处理任意数量和任意名字的关键字参数。在函数内部，`kwargs` 就是一个字典，其中的键是参数名，对应的值是参数的值。
>
> 在你提到的代码片段中：
>
> ```python
> def __init__(self, A1, A2, A3, K, N, F, Tr, Td, Tw, Tp, **kwargs):
>     super(MISTAGCN, self).__init__(**kwargs)
>     # ...
> ```
>
> `**kwargs` 允许这个构造函数接受除了 `A1, A2, A3, K, N, F, Tr, Td, Tw, Tp` 之外的其他关键字参数。这在定义类时是一种灵活的方式，可以方便地扩展构造函数接受的参数。





