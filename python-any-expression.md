# How to Use any() in Python

As a Python programmer, you’ll frequently deal with [Booleans](https://realpython.com/python-boolean/) and [conditional statements](https://realpython.com/python-conditional-statements/)—sometimes very complex ones. In those situations, you may need to rely on tools that can simplify logic and consolidate information. Fortunately, **`any()`** in Python is such a tool. It looks through the elements in an iterable and returns a single value indicating whether any element is true in a Boolean context, or **truthy.**

**In this tutorial, you’ll learn:**

- How to use `any()`
- How to decide between `any()` and `or`

Let’s dive right in!

**Python Pit Stop:** This tutorial is a **quick** and **practical** way to find the info you need, so you’ll be back to your project in no time!

**Free Bonus:** [Click here to get a Python Cheat Sheet](https://realpython.com/any-python/) and learn the basics of Python 3, like working with data types, dictionaries, lists, and Python functions.

## How to Use `any()` in Python

Imagine that you’re writing a program for your employer’s recruiting department. You might want to schedule interviews with candidates who meet any of the following criteria:

1. Know Python already
2. Have five or more years of developer experience
3. Have a degree

One tool you could use to write this conditional expression is [`or`](https://realpython.com/python-or-operator/):

```python
# recruit_developer.py
def schedule_interview(applicant):
    print(f"Scheduled interview with {applicant['name']}")

applicants = [
    {
        "name": "Devon Smith",
        "programming_languages": ["c++", "ada"],
        "years_of_experience": 1,
        "has_degree": False,
        "email_address": "devon@email.com",
    },
    {
        "name": "Susan Jones",
        "programming_languages": ["python", "javascript"],
        "years_of_experience": 2,
        "has_degree": False,
        "email_address": "susan@email.com",
    },
    {
        "name": "Sam Hughes",
        "programming_languages": ["java"],
        "years_of_experience": 4,
        "has_degree": True,
        "email_address": "sam@email.com",
    },
]
for applicant in applicants:
    knows_python = "python" in applicant["programming_languages"]
    experienced_dev = applicant["years_of_experience"] >= 5

    meets_criteria = (
        knows_python
        or experienced_dev
        or applicant["has_degree"]
    )
    if meets_criteria:
        schedule_interview(applicant)
```

In the above example, you check each applicant’s credentials and schedule an interview if the applicant meets any of your three criteria.

**Technical Detail:** Python’s `any()` and `or` aren’t limited to evaluating Boolean expressions. Instead, Python performs a [truth value test](https://docs.python.org/3/library/stdtypes.html#truth-value-testing) on each argument, evaluating whether the expression is [**truthy** or **falsy**](https://realpython.com/python-operators-expressions/#evaluation-of-non-boolean-values-in-boolean-context). For example, nonzero integer values are considered truthy and zero is considered falsy:

\>>>

```python
>>> 1 or 0
1
```

In this example, `or` evaluated the nonzero value `1` as truthy even though it’s not of type Boolean. `or` returned `1` and didn’t need to evaluate the truthiness of `0`. Later in this tutorial, you’ll learn more about the return value and argument evaluation of `or`.

If you execute this code, then you’ll see that Susan and Sam will get interviews:

```python
$ python recruit_developer.py
Scheduled interview with Susan Jones
Scheduled interview with Sam Hughes
```

The reason the program chose to schedule interviews with Susan and Sam is that Susan already knows Python and Sam has a degree. Notice each candidate only needed to meet one criterion.

Another way to evaluate the applicants’ credentials is to use `any()`. When you use `any()` in Python, you must pass the applicants’ credentials as an [iterable](https://realpython.com/lessons/looping-over-iterables/) argument:

```python
for applicant in applicants:
    knows_python = "python" in applicant["programming_languages"]
    experienced_dev = applicant["years_of_experience"] >= 5

    credentials = (
        knows_python,
        experienced_dev,
        applicant["has_degree"],
    )
    if any(credentials):
        schedule_interview(applicant)
```

When you use `any()` in Python, keep in mind that you can pass any iterable as an argument:

\>>>

```python
>>> any([0, 0, 1, 0])
True

>>> any(set((True, False, True)))
True

>>> any(map(str.isdigit, "hello world"))
False
```

In each example, `any()` loops through a different Python iterable, testing the truth of each element until it finds a truthy value or checks every element.

**Note:** The last example uses Python’s built-in [`map()`](https://realpython.com/python-map-function/), which returns an iterator in which every element is the result of passing the next character in the string to `str.isdigit()`. This is a useful way to use `any()` for more complex checks.

You may be wondering if `any()` is merely a dressed-up version of `or`. In the next section, you’ll learn the differences between these tools.

## How to Distinguish Between `or` and `any()`

There are two main differences between `or` and `any()` in Python:

1. Syntax
2. Return value

First, you’ll learn about how syntax affects the usability and readability of each tool. Second, you’ll learn the types of values that each tool returns. Knowing these differences will help you decide which tool is best for a given situation.

### Syntax

`or` is an [operator](https://realpython.com/lessons/operators-and-built-functions/), so it takes two arguments, one on either side:

\>>>

```python
>>> True or False
True
```

`any()`, on the other hand, is a function that takes one argument, an iterable of objects that it loops through to evaluate truthiness:

\>>>

```python
>>> any((False, True))
True
```

This difference in syntax is significant because it affects each tool’s usability and readability. For example, if you have an iterable, then you can pass the iterable directly to `any()`. To get similar behavior from `or`, you’d need to use a loop or a function like [`reduce()`](https://realpython.com/any-python/):

\>>>

```python
>>> import functools
>>> functools.reduce(lambda x, y: x or y, (True, False, False))
True
```

In the above example, you used [`reduce()`](https://realpython.com/lessons/python-reduce-function/) to pass an iterable as an argument to `or`. This could be done much more efficiently with `any`, which directly accepts iterables as arguments.

To illustrate another way that the syntax of each tool affects its usability, imagine that you want to avoid testing a condition if any preceding condition is `True`:

```python
def knows_python(applicant):
    print(f"Determining if {applicant['name']} knows Python...")
    return "python" in applicant["programming_languages"]

def is_local(applicant):
    print(f"Determine if {applicant['name']} lives near the office...")

should_interview = knows_python(applicant) or is_local(applicant)
```

If `is_local()` takes a relatively long time to execute, then you don’t want to call it when `knows_python()` has already returned `True`. This is called **lazy** evaluation, or [**short-circuit** evaluation](https://realpython.com/python-operators-expressions/#compound-logical-expressions-and-short-circuit-evaluation). By default, `or` evaluates conditions lazily, whereas `any` does not.

In the above example, the program wouldn’t even need to determine if Susan is local because it already confirmed that she knows Python. That’s good enough to schedule an interview. In this situation, calling functions lazily with `or` would be the most efficient approach.

Why not use `any()` instead? You learned above that `any()` takes an iterable as an argument, and Python evaluates the conditions according to the iterable type. So, if you use a list, Python will execute both `knows_python()` and `is_local()` during the creation of that list before calling `any()`:

```python
should_interview = any([knows_python(applicant), is_local(applicant)])
```

Here, Python will call `is_local()` for every applicant, even for those who know Python. Because `is_local()` will take a long time to execute and is sometimes unnecessary, this is an inefficient implementation of the logic.

There are ways to make Python call functions lazily when you’re using iterables, such as building an iterator with `map()` or using a [generator expression](https://realpython.com/introduction-to-python-generators/#building-generators-with-generator-expressions):

```python
any((meets_criteria(applicant) for applicant in applicants))
```

This example uses a [generator expression](https://realpython.com/courses/python-generators/) to generate Boolean values indicating whether an applicant meets the criteria for an interview. Once an applicant meets the criteria, `any()` will return `True` without checking the remaining applicants. But keep in mind that these types of workarounds also present their own issues and may not be appropriate in every situation.

The most important thing to remember is that the syntactic difference between `any()` and `or` can affect their usability.

Syntax isn’t the only difference that affects the usability of these tools. Next, let’s take a look at the different return values for `any()` and `or` and how they might influence your decision on which tool to use.

### Return Value

Python’s `any()` and `or` return different types of values. `any()` returns a Boolean, which indicates whether it found a truthy value in the iterable:

\>>>

```python
>>> any((1, 0))
True
```

In this example, `any()` found a truthy value (the integer `1`), so it returned the Boolean value `True`.

`or`, on the other hand, returns the first truthy value it finds, which will not necessarily be a Boolean. If there are no truthy values, then `or` returns the last value:

\>>>

```python
>>> 1 or 0
1

>>> None or 0
0
```

In the first example, `or` evaluated `1`, which is truthy, and returned it without evaluating `0`. In the second example, `None` is falsy, so `or` evaluated `0` next, which is also falsy. But since there are no more expressions to check, `or` returns the last value, `0`.

When you’re deciding which tool to use, it’s helpful to consider if you want to know the *actual* value of the object or just whether a truthy value exists somewhere in the collection of objects.

## Conclusion

Congratulations! You’ve learned the ins and outs of using `any()` in Python and the differences between `any()` and `or`. With a deeper understanding of these two tools, you’re well prepared to decide between them in your own code.

You now know:

- How to use **`any()`** in Python
- Why you’d use **`any()`** instead of **`or`**

If you would like to continue learning about conditional expressions and how to use tools like `or` and `any()` in Python, then you can check out the following resources:

- [`operator.or_()`](https://docs.python.org/3.4/library/operator.html#operator.or_)
- [`all()`](https://docs.python.org/3/library/functions.html#all)
- [`while` loops](https://realpython.com/python-while-loop/)



- `all`(*iterable*)

  Return `True` if all elements of the *iterable* are true (or if the iterable is empty). Equivalent to:`def all(iterable):    for element in iterable:        if not element:            return False    return True `

- `any`(*iterable*)

  Return `True` if any element of the *iterable* is true. If the iterable is empty, return `False`. Equivalent to:`def any(iterable):    for element in iterable:        if element:            return True    return False `

- `ascii`(*object*)

  As [`repr()`](https://docs.python.org/3/library/functions.html#repr), return a string containing a printable representation of an object, but escape the non-ASCII characters in the string returned by [`repr()`](https://docs.python.org/3/library/functions.html#repr) using `\x`, `\u` or `\U` escapes. This generates a string similar to that returned by [`repr()`](https://docs.python.org/3/library/functions.html#repr) in Python 2.

- `bin`(*x*)

  Convert an integer number to a binary string prefixed with “0b”. The result is a valid Python expression. If *x* is not a Python [`int`](https://docs.python.org/3/library/functions.html#int) object, it has to define an [`__index__()`](https://docs.python.org/3/reference/datamodel.html#object.__index__) method that returns an integer. Some examples:>>>`>>> bin(3) '0b11' >>> bin(-10) '-0b1010' `If prefix “0b” is desired or not, you can use either of the following ways.>>>`>>> format(14, '#b'), format(14, 'b') ('0b1110', '1110') >>> f'{14:#b}', f'{14:b}' ('0b1110', '1110') `See also [`format()`](https://docs.python.org/3/library/functions.html#format) for more information.