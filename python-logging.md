# Logging in Python with sub-modules and configuration files

*Python logging introduces lots of concepts and configuration options. I'm going to smooth them out for good.*



Dealing with the Logging package in Python requires to understand a couple of concepts. I start with a little bit of boring theory. Since this is an ongoing research, you may find some mistakes here and there. I will update this article as my discovery continues.

## The concept of hierarchy

In Python you print logs with instances of the `Logger` class. Loggers are hierachical objects, organized by a sort of namespace tree. They reflect the structure of your app and the various modules/packages you import. The root of the hierarchy is called **the root logger** (or **root** for brevity): you typically deal with it in your application's entry point (e.g. `main()`).

Thanks to that hierarchy, you can define which logger should output its stuff or enable/disable the entire chain completely. In a common scenario you create a logger for each module/package of your app and use it right away to print messages, without any initial setup. They are the child loggers, after all. Then you configure how the entire family of loggers works in the root logger (e.g. in your `main()` entry point).

## Loggers, handlers, filters, formatters

While logging in Python, there are basically four objects working together. The **logger** is what you call when you actually want to print a message somewhere. A **handler** grabs that message and redirects it to a particular destination: console output, file, network and so on. A **formatter** defines the layout of each message string. Finally a **filter** gives you the ability to specify what to log, and what to mute, instead. I've never had the urge to use a filter, so I will leave it out for now. Configuring a logger means to tell those four objects how they should interact.

## Import and call a logger in a module

I'm writing a very simple app, which is composed of a `main.py` and a `core.py` module, and I want to log some messages. The first step is importing the logging module and initialize an object.

```python
# core.py

import logging
log = logging.getLogger(__name__)
```

I have just created a logger based on the module's fully qualified name (`__name__`). This is a common best practice and you should stick with it. Now I can start logging with the following methods:

```python
#core.py

def do_something():
  log.debug('debug message')
  log.info('info message')
  log.warning('warning message')
  log.error('error message')
  log.critical('critical message')
  log.log('log message')
  log.exception('exception message')
```

The next step is to configure the logger so that I can actually output those messages wherever needed.

## Configure the logger via INI file

The Logging configuration occurs right in my `main.py` file, using a sub-module called `logging.config`. Remember: I'm configuring the root logger now, because I am at the top level of my application.

```python
# main.py

import core
import logging.config

logging.config.fileConfig('/path/to/my/configuration.ini')
core.do_something()
```

I import my `core` module (and use it) and `logging.config`, telling it where to retrieve the configuration file. That's basically it: we are done with `main.py`.

## The configuration file

This is the part I found to be the most tricky. First of all you have to define your four logging objects (actually three: I leave out the filter).

```ini
[loggers]
keys=root,core

[handlers]
keys=consoleHandler

[formatters]
keys=defaultFormatter
```

I'm just giving some names here: I have two loggers, **root** and **core** (the one in my `core.py` module, remember?), a handler I called **consoleHandler** and a formatter **defaultFormatter**. I invented those names. Root logger must be always defined in this file, otherwise Python complains about it.

Now, for each logger, I define some properties:

```ini
[logger_root]
handlers=consoleHandler

[logger_core]
handlers=consoleHandler
level=DEBUG
qualname=core
propagate=0
```

First of all, take care of the syntax `[logger_<your-logger-name>]`. For the root logger I choose to handle it with **consoleHandler**. I do the same for the **core** logger, but I also define:

- the debug **level**;
- the **qualname** - the name used by the application to get the logger. In my case is **core**, as I did in `core.py` with the instruction `logging.getLogger(__name__)`, and there `__name__` corresponds to **core**;
- **propagate** - if true, events logged by this logger will be passed to the upper level.

We are done with the loggers. Now I proceed to configure the formatter and the handler:

```ini
[handler_consoleHandler]
class=logging.StreamHandler
formatter=defaultFormatter
args=(sys.stdout,)

[formatter_defaultFormatter]
format=%(levelname)s %(asctime)s %(filename)s - %(message)s
```

The same syntax applies here: `[handler_<your-handler-name>]` and `[formatter_<your-formatter-name>]`.

My **consoleHandler** uses:

- the **class** `logging.StreamHandler` to output the messages. That class simply redirects any string to the console;
- my **defaultFormatter** as a **formatter**;
- **args** as the list of arguments to the constructor for the handler class. In my example I want to print to the standard ouput.

Finally my **defaultFormatter** formats the strings according to my needs.

## Logging from the `__main__` module

When you run a Python script as `python script.py`, the file you feed into the Python interpreter is a module itself and its fully qualified name is `__main__`. This is why it is also called the **main module**. So if you want to print log messages from the main module you need to include it too in the configuration file, like this:

```ini
[loggers]
keys=root,main,[...]

[...]

[logger_main]
handlers=consoleHandler
level=DEBUG
qualname=__main__
propagate=0
```

It took me a while to figure it out!

## Useful links

List of handlers and their parameters: https://docs.python.org/3.4/library/logging.handlers.html
List of formatting parameters: https://docs.python.org/3.4/library/logging.html#logrecord-attributes
List of debug levels: https://docs.python.org/3.4/howto/logging.html#when-to-use-logging

## Sources

Antonym.org - A real Python logging example ([link](http://antonym.org/2005/03/a-real-python-logging-example.html))
Pythonlibrary.org - Python 101: An Intro to logging ([link](http://www.blog.pythonlibrary.org/2012/08/02/python-101-an-intro-to-logging/))
Eric.themoritzfamily.com - Learning Python logging ([link](http://eric.themoritzfamily.com/learning-python-logging.html))
Docs.python.org - Logging HowTo ([link](https://docs.python.org/3.4/howto/logging.html))