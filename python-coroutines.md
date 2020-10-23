# Python Coroutines

## 3.4. Coroutines

### 3.4.1. Awaitable Objects

An [awaitable](https://docs.python.org/3.9/glossary.html#term-awaitable) object generally implements an [`__await__()`](https://docs.python.org/3.9/reference/datamodel.html#object.__await__) method. [Coroutine objects](https://docs.python.org/3.9/glossary.html#term-coroutine) returned from [`async def`](https://docs.python.org/3.9/reference/compound_stmts.html#async-def) functions are awaitable.

Note

 

The [generator iterator](https://docs.python.org/3.9/glossary.html#term-generator-iterator) objects returned from generators decorated with [`types.coroutine()`](https://docs.python.org/3.9/library/types.html#types.coroutine) or [`asyncio.coroutine()`](https://docs.python.org/3.9/library/asyncio-task.html#asyncio.coroutine) are also awaitable, but they do not implement [`__await__()`](https://docs.python.org/3.9/reference/datamodel.html#object.__await__).

- `object.``__await__`(*self*)

  Must return an [iterator](https://docs.python.org/3.9/glossary.html#term-iterator). Should be used to implement [awaitable](https://docs.python.org/3.9/glossary.html#term-awaitable) objects. For instance, [`asyncio.Future`](https://docs.python.org/3.9/library/asyncio-future.html#asyncio.Future) implements this method to be compatible with the [`await`](https://docs.python.org/3.9/reference/expressions.html#await) expression.





### 3.4.2. Coroutine Objects

[Coroutine objects](https://docs.python.org/3.9/glossary.html#term-coroutine) are [awaitable](https://docs.python.org/3.9/glossary.html#term-awaitable) objects. A coroutine’s execution can be controlled by calling [`__await__()`](https://docs.python.org/3.9/reference/datamodel.html#object.__await__) and iterating over the result. When the coroutine has finished executing and returns, the iterator raises [`StopIteration`](https://docs.python.org/3.9/library/exceptions.html#StopIteration), and the exception’s `value` attribute holds the return value. If the coroutine raises an exception, it is propagated by the iterator. Coroutines should not directly raise unhandled [`StopIteration`](https://docs.python.org/3.9/library/exceptions.html#StopIteration) exceptions.

Coroutines also have the methods listed below, which are analogous to those of generators (see [Generator-iterator methods](https://docs.python.org/3.9/reference/expressions.html#generator-methods)). However, unlike generators, coroutines do not directly support iteration.

*Changed in version 3.5.2:* It is a [`RuntimeError`](https://docs.python.org/3.9/library/exceptions.html#RuntimeError) to await on a coroutine more than once.

- `coroutine.``send`(*value*)

  Starts or resumes execution of the coroutine. If *value* is `None`, this is equivalent to advancing the iterator returned by [`__await__()`](https://docs.python.org/3.9/reference/datamodel.html#object.__await__). If *value* is not `None`, this method delegates to the [`send()`](https://docs.python.org/3.9/reference/expressions.html#generator.send) method of the iterator that caused the coroutine to suspend. The result (return value, [`StopIteration`](https://docs.python.org/3.9/library/exceptions.html#StopIteration), or other exception) is the same as when iterating over the [`__await__()`](https://docs.python.org/3.9/reference/datamodel.html#object.__await__) return value, described above.

- `coroutine.``throw`(*type*[, *value*[, *traceback*]])

  Raises the specified exception in the coroutine. This method delegates to the [`throw()`](https://docs.python.org/3.9/reference/expressions.html#generator.throw) method of the iterator that caused the coroutine to suspend, if it has such a method. Otherwise, the exception is raised at the suspension point. The result (return value, [`StopIteration`](https://docs.python.org/3.9/library/exceptions.html#StopIteration), or other exception) is the same as when iterating over the [`__await__()`](https://docs.python.org/3.9/reference/datamodel.html#object.__await__) return value, described above. If the exception is not caught in the coroutine, it propagates back to the caller.

- `coroutine.``close`()

  Causes the coroutine to clean itself up and exit. If the coroutine is suspended, this method first delegates to the [`close()`](https://docs.python.org/3.9/reference/expressions.html#generator.close) method of the iterator that caused the coroutine to suspend, if it has such a method. Then it raises [`GeneratorExit`](https://docs.python.org/3.9/library/exceptions.html#GeneratorExit) at the suspension point, causing the coroutine to immediately clean itself up. Finally, the coroutine is marked as having finished executing, even if it was never started.Coroutine objects are automatically closed using the above process when they are about to be destroyed.



### 3.4.3. Asynchronous Iterators

An *asynchronous iterator* can call asynchronous code in its `__anext__` method.

Asynchronous iterators can be used in an [`async for`](https://docs.python.org/3.9/reference/compound_stmts.html#async-for) statement.

- `object.``__aiter__`(*self*)

  Must return an *asynchronous iterator* object.

- `object.``__anext__`(*self*)

  Must return an *awaitable* resulting in a next value of the iterator. Should raise a [`StopAsyncIteration`](https://docs.python.org/3.9/library/exceptions.html#StopAsyncIteration) error when the iteration is over.

An example of an asynchronous iterable object:

```python
class Reader:
    async def readline(self):
        ...

    def __aiter__(self):
        return self

    async def __anext__(self):
        val = await self.readline()
        if val == b'':
            raise StopAsyncIteration
        return val
```

*New in version 3.5.*

*Changed in version 3.7:* Prior to Python 3.7, `__aiter__` could return an *awaitable* that would resolve to an [asynchronous iterator](https://docs.python.org/3.9/glossary.html#term-asynchronous-iterator).

Starting with Python 3.7, `__aiter__` must return an asynchronous iterator object. Returning anything else will result in a [`TypeError`](https://docs.python.org/3.9/library/exceptions.html#TypeError) error.



### 3.4.4. Asynchronous Context Managers

An *asynchronous context manager* is a *context manager* that is able to suspend execution in its `__aenter__` and `__aexit__` methods.

Asynchronous context managers can be used in an [`async with`](https://docs.python.org/3.9/reference/compound_stmts.html#async-with) statement.

- `object.``__aenter__`(*self*)

  Semantically similar to [`__enter__()`](https://docs.python.org/3.9/reference/datamodel.html#object.__enter__), the only difference being that it must return an *awaitable*.

- `object.``__aexit__`(*self*, *exc_type*, *exc_value*, *traceback*)

  Semantically similar to [`__exit__()`](https://docs.python.org/3.9/reference/datamodel.html#object.__exit__), the only difference being that it must return an *awaitable*.

An example of an asynchronous context manager class:

```python
class AsyncContextManager:
    async def __aenter__(self):
        await log('entering context')

    async def __aexit__(self, exc_type, exc, tb):
        await log('exiting context')
```

