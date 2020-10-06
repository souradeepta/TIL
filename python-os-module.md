#  Python Files and directories related methods

[ref: https://pylessons.com/Python-3-basics-tutorial-files-and-directories/]



There are many sources, which provide a wide range of utility methods to handle and manipulate files and directories with Python on Windows, Linux and other operating systems, but in this tutorial we will cover only most often used methods.

All files are contained within various directories and Python has no problems handling them. The `*os*` module has several methods that help you perform file-processing operations, such as create, remove, rename or change directories. We will cover mentioned methods here, but keep in mind, that all further methods require to `*import os*` build in module.



###### **Renaming and Deleting Files:**

So lets start with `*rename()*` method. This method takes two arguments: current filename and new filename.

`*rename()*` syntax would be:

```python
os.rename(current_file_name, new_file_name)
```



Following is the example to rename in local directory existing Python.py file:

```python
import os

# Rename a file from python.py to python_rename.py
os.rename("new_file.py", "renamed_file.py")
```



We use the `*remove()*` method to delete files by supplying the name of the file to be deleted as the argument. Example:

```python
import os

# Delete file Python_rename.py
os.remove("renamed_file")
```



###### **Directories related methods:**

Same `*os*` module has several methods that help you work with directories:



###### The mkdir() Method:

We can use the `*mkdir()*` method of the `*os*` module to create directories in the current directory. We need to give an argument to this method which contains the name of the directory to be created.
And here is simple example:

```python
import os

# Create a directory "python_test"
os.mkdir("python_test")
```



###### The chdir() Method:

We can use the `*chdir()*` method to change the current working directory. The `*chdir()*` method takes an argument, which is the name of the directory that you want to make the current working directory.
Example:

```python
import os

# Changing a directory to "C:/"
os.chdir("C:/")
# In C:/ create a directory "test"
os.mkdir("test")
```



###### The getcwd() Method:

The `*getcwd()*` method will display the current working directory.
Example:

```python
import os

# This would give location of the current directory
os.getcwd()
```



###### The rmdir() Method:

The `*rmdir()*` method deletes the directory, which is passed as an argument in the method. It is required to give fully qualified name of the directory, otherwise it would search for that directory in the current directory.
Example:

```python
import os

# Changing a directory to "C:/"
os.chdir("C:/")
# This would  remove "test"  directory.
os.rmdir("test")
```