# 高级依赖关系

## 参数化的依赖关系

我们已经看过的所有依赖关系都是固定的函数或类。

但是也有一些情况下，您希望能够对依赖关系设置参数，而不必声明许多不同的函数或类。

假设我们想要一个依赖关系来检查查询参数 `q` 是否包含一些固定的内容。

但是我们希望能够参数化该固定内容。

## 一个“可调用的”实例

在Python中，有一种方法可以使类的实例成为“可调用的”。

不是类本身(它已经是一个可调用的)，而是该类的实例。

为此，我们声明一个 `__call__` 方法:

```Python hl_lines="10"
{!../../../docs_src/dependencies/tutorial011.py!}
```

在这个例子中，**FastAPI** 将会使用这个 `__call__` 检查额外的参数和子依赖，之后你的 *路径操作函数* 也会调用这个方法为参数传递一个值。

## 参数化实例

现在，我们可以使用 `__init__` 声明实例的参数来“参数化”依赖关系:

```Python hl_lines="7"
{!../../../docs_src/dependencies/tutorial011.py!}
```

在这个例子中，**FastAPI* *永远不会触及或关心 `__init__`，我们将直接在我们的代码中使用它。

## 创建实例

我们可以用以下方法创建这个类的实例:

```Python hl_lines="16"
{!../../../docs_src/dependencies/tutorial011.py!}
```

这样我们就可以“参数化”我们的依赖关系，现在它有 `“bar”` 作为checker.fixed_content 属性的值。

## 使用实例作为依赖关系

然后，我们可以在 `Depends(checker)` 中使用这个 `checker` ，而不是 `Depends(FixedContentQueryChecker)`，因为依赖项 `checker` 是实例，而不是类本身。

当解决依赖关系时， **FastAPI** 会像这样调用这个 `checker` :

```Python
checker(q="somequery")
```

...并将在我们的 *路径操作函数* 中的任何返回值做为依赖关系的值，并赋值给 `fixed_content_included` 参数:

```Python hl_lines="20"
{!../../../docs_src/dependencies/tutorial011.py!}
```

!!! tip
    这一切似乎都有些做作。它的用途可能还不是很清楚。

    这些实例故意简化，但展示了它是如何工作的。

    在关于安全性的章节中，有些实用函数也是以同样的方式实现的。

    如果您理解了所有这些，您就已经知道了这些用于安全的实用工具在底层是如何工作的。
