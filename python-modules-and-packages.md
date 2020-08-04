# Modules and packages: how to create a Python project

A quick and dirty tutorial on how to get things done.

Do you sweat every time you have to organize a bunch of new Python code? Should you make a module, a package, or both? Don't worry, I've been there thousand times. In this article I will show you the rationale behind a Python module, the differences with a package and how the two things interact together.



## Python modules vs. Python packages

A **module** is a Python file which contains code, saved with the `.py` extension. Every time you write a function, a class or a statement and save it to a `.py` file you are actually creating a new module. A module can be executed by the Python interpreter directly, e.g. `python script.py` and thus called **main module**, or imported by other modules. Modules are a way to lay out your program in different files for easier maintenance.

When you have many modules in your project it's good practice to organize them into folders. For example, say I'm working on a very primitive game in Python called Fancy Game: I would like to structure the directory as follows:

```nolang
fancy_game/
    models/
        player.py
        monster.py
    audio/
        mixer.py
        effects.py
        player.py
    graphics/
        renderer.py
        screen.py
    common/
        constants.py
    main.py
```

A **package** is simply a collection of Python modules organized into folders. In my Fancy Game the packages could be: `models`, `audio`, `graphics` and `common`. The `fancy_game` folder is not a package itself, because it is intended to be run directly by Python (i.e. `python main.py`). Sometimes you want to create a library instead to be imported in other Python programs, so the entire root folder would become a package too (made of many sub-packages).

Having a project or a library organized into packages is a good thing: a) your source code is even more modularized and b) packages provide protection against name clashes with other modules. We'll see why in a minute.



## Turn a folder into a Python package

Python has to be instructed about which directory should become a package. To do this, simply add an empty file called `__init__.py` inside each desired folder. This is a special file used to mark directories on disk as Python package directories. So, my Fancy Game folder structure would be:

```nolang
fancy_game/
    models/
        __init__.py        <--- new __init__.py file added
        player.py
        monster.py
    audio/
        __init__.py        <--- new __init__.py file added
        mixer.py
        effects.py
        player.py
    graphics/
        __init__.py        <--- new __init__.py file added
        renderer.py
        screen.py
    common/
        __init__.py        <--- new __init__.py file added
        constants.py
    main.py
```

Notice how there is no `__init__.py` in the root folder: this is because my game (i.e. `main.py`) is intended to be run directly from the Python interpreter. In case of a library, simply add the special file into the root directory as well.



## Importing modules from packages

Now that the whole structure has been set up, the code inside `main.py` needs to import some modules from the various packages in order to make the game work. To import a module from a package you have to follow the **dotted module name** syntax. For example, in the main module I want to import the `player` module from the `audio` package:

```python
# main.py
import audio.player
```

More generally, the rule is `import [package].[module]`. This also works in case you have nested packages: `import [package1].[package2].[module]` and so on.

Once imported, the module must be referenced with its full name. So if I want to use the function `play_sound()` from within the `audio.player` module I have to call it as `audio.player.play_sound()`. As mentioned above, this is a good way to avoid name clashes across different modules: I can easily import the module `model.player` without messing up with its homonym `audio.player`:

```python
# main.py
import audio.player
import model.player

# Two modules with the same name: no problem
audio.player.play_sound()
model.player.run()
```

### Importing modules from above

Sometimes a deep-buried module needs to import stuff from the upper level. For example, the `audio.player` module might need something inside `common.constants`. There are two ways of doing it:

1. absolute import — import the module as if the importer is located in the root directory. Python is able to figure out the right path. For example, in `audio.player` just do `import common.constants`. This is my favorite option;
2. relative import — use the formula `from [module] import [name]` with **dots** to indicate the current and parent packages involved in the import. For example, in `audio.player` you can call `from .. import common.constants`. One dot means the current package, two dots is up one level, three dots is up two levels and so on. I'm not a huge fan of this one, as relative imports break easily when you move modules around.



## Shorten module names

Using long names such as `models.monster.Skeleton` is quite inconvenient. You can shorten a module name while importing it with an **alias**, for example:

```python
# main.py
import models.monster as monster
```

Now `models.monster` is available as `monster`. Just keep in mind that this way might lead to name clashes across modules.



## Sources

Alex Dzyoba - [Hitchhiker's guide to the Python imports](https://alex.dzyoba.com/blog/python-import/)
Stack Overflow - [What is __init__.py for?](https://stackoverflow.com/questions/448271/what-is-init-py-for)
Python docs - [6. Modules](https://docs.python.org/3/tutorial/modules.html)
Python reference - [7.11. The import statement](https://docs.python.org/3/reference/simple_stmts.html#the-import-statement)

[Python](https://www.internalpointers.com/tag/python)