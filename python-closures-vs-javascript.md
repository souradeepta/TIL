# [Python closure vs javascript closure](https://stackoverflow.com/questions/18502095/python-closure-vs-javascript-closure)

[Ask Question](https://stackoverflow.com/questions/ask)

The following closure function works fine in javascript.

```javascript
function generateNextNumber(startNumber) {
    var current = startNumber;
    return function(){
        return current += 1;
    }
}

var getNextNumber = generateNextNumber(10);
for (var i = 0; i < 10; i++) {
    console.log(getNextNumber());
}
```

I tried to do the same in Python

```python
def generateNextNumber(startNumber):
    current = startNumber
    def tempFunction():
        current += 1
        return current
    return tempFunction

getNextNumber = generateNextNumber(10)
for i in range(10):
    print (getNextNumber())
```

I get the following error

```python
Traceback (most recent call last):
  File "/home/thefourtheye/Desktop/Test1.py", line 10, in <module>
    print (getNextNumber())
  File "/home/thefourtheye/Desktop/Test1.py", line 4, in tempFunction
    current += 1
UnboundLocalError: local variable 'current' referenced before assignment
```

When I printed the `vars()` and `locals()` inside `tempFunction`, they confirm that `current` is present.

```python
({'current': 10}, {'current': 10})
```

But when I modified the program a little like this

```python
def generateNextNumber(startNumber):
    current = {"Number" : startNumber}
    def tempFunction():
        current["Number"] += 1
        return current["Number"]
    return tempFunction
```

it works. I can't reason why this works. Can anyone explain please?



ans



Python assumes that all variables in a function are local. This is to avoid accidental use of a global variable of the same name, or in an enclosing scope. In some important way, this difference is consequence of the fact that in Python local variable declaration is automatic/implicit while in JavaScript it is not (you have to use `var`). Solutions:

### Use a `global` declaration

```python
def generateNextNumber(startNumber):
    global current
    current= startNumber
    def tempFunction():
        global current
        current += 1
        return current 
    return tempFunction
```

Valid in some cases, but in yours only one instance of `tempFunction` could be active at the same time.

### Use a function attribute

```python
def generateNextNumber(startNumber):
    def tempFunction():
        tempFunction.current += 1
        return tempFunction.current
    tempFunction.current= startNumber
    return tempFunction
```

Uses the fact that functions are objects (and thus can have attributes), that they are instantiated when they are declared, and that they become local to the enclosing function (or module, in which case they are really global). This also works because the name `tempFunction` is used for the first time inside its own definition with a "member access" `.` operator and thus not assumed local. Something similar happens with the "call" `()`, and "element access" `[]` operators. The later case explains why your code works.

### Force the name to be assumed non-local

```python
def generateNextNumber(startNumber):
    current= type("OnTheFly",(),{})()
    current.value= startNumber
    def tempFunction():
        current.value += 1
        return current.value
    return tempFunction
```

This was already explained in the previous section. By using the member access operator `.` we are saying "`current` already exists", and thus it's searched in the enclosing scope. In this particular case, we are creating a class using the `type` function and immediately creating an instance of it (with the second set of parantheses). Instead of a general object, we could have also used a list or a dictionary. The second case was a very common solution.

### Use a function object

```python
def generateNextNumber(startNumber):
    class TempFunction:
        def __call__(self):
            self.current += 1
            return self.current
    tempFunction= TempFunction()
    tempFunction.current= startNumber
    return tempFunction
```

Any object whose class has a **call** method **is** a function and thus can be called with the function call operator `()`. This is extremely related to the two previous cases.

### Use a `nonlocal` declaration

```python
def generateNextNumber(startNumber):
    current= startNumber
    def tempFunction():
        nonlocal current
        current += 1
        return current
    return tempFunction
```

In the same way that `global` means... well, global, `nonlocal` means "in the immediately preceding scope". Valid in Python 3 and maybe later versions of Python 2.

### Use generators

```python
def generateNextNumber(current):
    while True :
        current+= 1
        yield current
```

This is probably the most "Pythonic" way to approach not the general problem of nonlocal variable access, but the specific case you used to explain it. I couldn't finish without mentioning it. You need to call it with a minor change, though:

```python
getNextNumber = generateNextNumber(10)
for i in range(10):
    print (getNextNumber.next())
```

When driving a `for` the call to `next()` is implicit (but the generator can not be infinite as in my example).