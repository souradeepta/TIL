# What’s New In Python 3.9

This article explains the new features in Python 3.9, compared to 3.8.

Note

Prerelease users should be aware that this document is currently in draft form. It will be updated substantially as Python 3.9 moves towards release, so it’s worth checking back even after reading earlier versions.

## Summary – Release highlights

## New Features

## Other Language Changes

- [`__import__() `](https://docs.python.org/3.9/library/functions.html#__import__)now raises [`ImportError`](https://docs.python.org/3.9/library/exceptions.html#ImportError) instead of [`ValueError`](https://docs.python.org/3.9/library/exceptions.html#ValueError), which used to occur when a relative import went past its top-level package. (Contributed by Ngalim Siregar in [bpo-37444](https://bugs.python.org/issue37444).)
- Python now gets the absolute path of the script filename specified on the command line (ex: `python3 script.py`): the `__file__` attribute of the [`__main__`](https://docs.python.org/3.9/library/__main__.html#module-__main__") module, `sys.argv[0]` and `sys.path[0]` become an absolute path, rather than a relative path. These paths now remain valid after the current directory is changed by [`os.chdir()`](https://docs.python.org/3.9/library/os.html#os.chdir). As a side effect, a traceback also displays the absolute path for [`__main__`](https://docs.python.org/3.9/library/__main__.html#module-__main__") module frames in this case. (Contributed by Victor Stinner in [bpo-20443](https://bugs.python.org/issue20443).)
- In development mode and in debug build, encoding and errors arguments are now checked on string encoding and decoding operations. Examples: [open()](https://docs.python.org/3.9/library/functions.html#open), [str.encode()](https://docs.python.org/3.9/library/stdtypes.html#str.encode) and [bytes.decode()](https://docs.python.org/3.9/library/stdtypes.html#bytes.decode).
- By default, for best performance, the errors argument is only checked at the first encoding/decoding error and the encoding argument is sometimes ignored for empty strings. (Contributed by Victor Stinner in [bpo-37388](https://bugs.python.org/issue37388).)
- `"".replace("", s, n)` now returns s instead of an empty string for all non-zero `n`. It is now consistent with `"".replace("", s)`. There are similar changes for [bytes](https://docs.python.org/3.9/library/stdtypes.html#bytes) and [bytearray](https://docs.python.org/3.9/library/stdtypes.html#bytearray) objects. (Contributed by Serhiy Storchaka in [bpo-28029](https://bugs.python.org/issue28029).)



### Improved Modules

#### ast

Added the indent option to [dump()](https://docs.python.org/3.9/library/ast.html#ast.dump) which allows it to produce a multiline indented output. (Contributed by Serhiy Storchaka in [bpo-37995](https://docs.python.org/3.9/library/ast.html#ast.dump).)

Added [ast.unparse()](https://docs.python.org/3.9/library/ast.html#ast.unparse) as a function in the [ast](https://docs.python.org/3.9/library/ast.html#module-ast) module that can be used to unparse an [ast.AST](https://docs.python.org/3.9/library/ast.html#ast.AST) object and produce a string with code that would produce an equivalent [ast.AST](https://docs.python.org/3.9/library/ast.html#ast.AST) object when parsed. (Contributed by Pablo Galindo and Batuhan Taskaya in [bpo-38870](https://bugs.python.org/issue38870).)

#### asyncio

Due to significant security concerns, the reuse_address parameter of [asyncio.loop.create_datagram_endpoint()](https://docs.python.org/3.9/library/asyncio-eventloop.html#asyncio.loop.create_datagram_endpoint) is no longer supported. This is because of the behavior of the socket option `SO_REUSEADDR` in UDP. For more details, see the documentation for `loop.create_datagram_endpoint()`. (Contributed by Kyle Stanley, Antoine Pitrou, and Yury Selivanov in [bpo-37228](https://bugs.python.org/issue37228).)

Added a new [coroutine](https://docs.python.org/3.9/glossary.html#term-coroutine) [shutdown_default_executor()](https://docs.python.org/3.9/library/asyncio-eventloop.html#asyncio.loop.shutdown_default_executor) that schedules a shutdown for the default executor that waits on the [ThreadPoolExecutor](https://docs.python.org/3.9/library/concurrent.futures.html#concurrent.futures.ThreadPoolExecutor) to finish closing. Also, `[asyncio.run()](https://docs.python.org/3.9/library/asyncio-task.html#asyncio.run "asyncio.run()")` has been updated to use the new [coroutine](https://docs.python.org/3.9/glossary.html#term-coroutine). (Contributed by Kyle Stanley in [bpo-34037](https://bugs.python.org/issue34037).)

Added [asyncio.PidfdChildWatcher](https://docs.python.org/3.9/library/asyncio-policy.html#asyncio.PidfdChildWatcher), a Linux-specific child watcher implementation that polls process file descriptors. ([bpo-38692](https://bugs.python.org/issue38692))

#### curses

Add [curses.get_escdelay()](https://docs.python.org/3.9/library/curses.html#curses.get_escdelay), [curses.set_escdelay()](https://docs.python.org/3.9/library/curses.html#curses.set_escdelay), [curses.get_tabsize()](https://docs.python.org/3.9/library/curses.html#curses.get_tabsize), and [curses.set_tabsize() functions](https://docs.python.org/3.9/library/curses.html#curses.set_tabsize). (Contributed by Anthony Sottile in [bpo-38312](https://bugs.python.org/issue38312).)

#### fcntl

Added constants `F_OFD_GETLK, F_OFD_SETLK` and `F_OFD_SETLKW`. (Contributed by Dong-hee Na in [bpo-38602](https://bugs.python.org/issue38602).)

#### os

Added [CLD_KILLED](https://docs.python.org/3.9/library/os.html#os.CLD_KILLED) and [CLD_STOPPED](https://docs.python.org/3.9/library/os.html#os.CLD_STOPPED) for `si_code`. (Contributed by Dong-hee Na in [bpo-38493](https://bugs.python.org/issue38493).)

Exposed the Linux-specific [os.pidfd_open()](https://docs.python.org/3.9/library/os.html#os.pidfd_open) ([bpo-38692](https://bugs.python.org/issue38692)) and [os.P_PIDFD](https://docs.python.org/3.9/library/os.html#os.P_PIDFD) ([bpo-38713](https://bugs.python.org/issue38713)) for process management with file descriptors.

#### threading

In a subinterpreter, spawning a daemon thread now raises a [RuntimeError](https://docs.python.org/3.9/library/exceptions.html#RuntimeError). Daemon threads were never supported in subinterpreters. Previously, the subinterpreter finalization crashed with a Python fatal error if a daemon thread was still running. (Contributed by Victor Stinner in [bpo-37266](https://bugs.python.org/issue37266).)

#### venv

The activation scripts provided by [venv](https://docs.python.org/3.9/library/pathlib.html#pathlib.Path.readlink) now all specify their prompt customization consistently by always using the value specified by `__VENV_PROMPT__`. Previously some scripts unconditionally used `__VENV_PROMPT__`, others only if it happened to be set (which was the default case), and one used `__VENV_NAME__` instead. (Contributed by Brett Cannon in bpo-37663.)

#### pathlib

Added [pathlib.Path.readlink()](https://docs.python.org/3.9/library/pathlib.html#pathlib.Path.readlink) which acts similarly to [os.readlink()](https://docs.python.org/3.9/library/os.html#os.readlink). (Contributed by Girts Folkmanis in [bpo-30618](https://bugs.python.org/issue30618))

#### pprint

[pprint](https://docs.python.org/3.9/library/pprint.html#module-pprint) can now [pretty-print types.SimpleNamespace](https://docs.python.org/3.9/library/types.html#types.SimpleNamespace). (Contributed by Carl Bordum Hansen in bpo-37376.)

#### importlib

To improve consistency with import statements, [importlib.util.resolve_name()](https://docs.python.org/3.9/library/importlib.html#importlib.util.resolve_name) now raises [ImportError](https://docs.python.org/3.9/library/exceptions.html#ImportError) instead of [ValueError](https://docs.python.org/3.9/library/exceptions.html#ValueError) for invalid relative import attempts. (Contributed by Ngalim Siregar in [bpo-37444](https://bugs.python.org/issue37444).)

#### signal

Exposed the Linux-specific [signal.pidfd_send_signal()](https://docs.python.org/3.9/library/signal.html#signal.pidfd_send_signal) for sending to signals to a process using a file descriptor instead of a pid. ([bpo-38712](https://bugs.python.org/issue38712))

## Optimizations

### Build and C API Changes

- Provide [Py_EnterRecursiveCall()](https://docs.python.org/3.9/c-api/exceptions.html#c.Py_EnterRecursiveCall) and [Py_LeaveRecursiveCall()](https://docs.python.org/3.9/c-api/exceptions.html#c.Py_LeaveRecursiveCall) as regular functions for the limited API. Previously, there were defined as macros, but these macros didn’t work with the limited API which cannot access `PyThreadState.recursion_depth` field. Remove `_Py_CheckRecursionLimit` from the stable ABI. (Contributed by Victor Stinner in [bpo-38644](https://bugs.python.org/issue38644).)
- Add a new public [PyObject_CallNoArgs()](https://docs.python.org/3.9/c-api/call.html#c.PyObject_CallNoArgs) function to the C API, which calls a callable Python object without any arguments. It is the most efficient way to call a callable Python object without any argument. (Contributed by Victor Stinner in [bpo-37194](https://bugs.python.org/issue37194).)
- The global variable [PyStructSequence_UnnamedField](https://docs.python.org/3.9/c-api/tuple.html#c.PyStructSequence_UnnamedField) is now a constant and refers to a constant string. (Contributed by Serhiy Storchaka in [bpo-38650](https://bugs.python.org/issue38650).)
- Exclude `PyFPE_START_PROTECT()` and `PyFPE_END_PROTECT()` macros of `pyfpe.h` from `Py_LIMITED_API` (stable API). (Contributed by Victor Stinner in bpo-38835.)
- Remove `PyMethod_ClearFreeList()` and `PyCFunction_ClearFreeList()` functions: the free lists of bound method objects have been removed. (Contributed by Inada Naoki and Victor Stinner in [bpo-37340](https://bugs.python.org/issue38835).)
- Remove `PyUnicode_ClearFreeList()` function: the Unicode free list has been removed in Python 3.3. (Contributed by Victor Stinner in [bpo-38896](https://bugs.python.org/issue38896).)

### Deprecated