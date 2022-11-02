Python Counter class is part of [Collections](https://www.digitalocean.com/community/tutorials/python-collections) module. Counter is a subclass of [Dictionary](https://www.digitalocean.com/community/tutorials/python-dictionary) and used to keep track of elements and their count.

## Python Counter

Counter is an unordered collection where elements are stored as `Dict` keys and their count as dict value. Counter elements count can be positive, zero or negative integers. However there is no restriction on it’s keys and values. Although values are intended to be numbers but we can store other objects too.

## Creating Python Counter Object

We can create an empty Counter or start with some initial values too.

    from collections import Counter
    
    # empty Counter
    counter = Counter()
    print(counter)  # Counter()
    
    # Counter with initial values
    counter = Counter(['a', 'a', 'b'])
    print(counter)  # Counter({'a': 2, 'b': 1})
    
    counter = Counter(a=2, b=3, c=1)
    print(counter)  # Counter({'b': 3, 'a': 2, 'c': 1})
    

We can also use any Iterable as argument for creating Counter object. So [string literal](https://www.digitalocean.com/community/tutorials/python-string) and [List](https://www.digitalocean.com/community/tutorials/python-list) can be used too for creating Counter object.

    # Iterable as argument for Counter
    counter = Counter('abc')
    print(counter)  # Counter({'a': 1, 'b': 1, 'c': 1})
    
    # List as argument to Counter
    words_list = ['Cat', 'Dog', 'Horse', 'Dog']
    counter = Counter(words_list)
    print(counter)  # Counter({'Dog': 2, 'Cat': 1, 'Horse': 1})
    
    # Dictionary as argument to Counter
    word_count_dict = {'Dog': 2, 'Cat': 1, 'Horse': 1}
    counter = Counter(word_count_dict)
    print(counter)  # Counter({'Dog': 2, 'Cat': 1, 'Horse': 1})
    

As I mentioned above, we can use non-numeric data for count values too, but that will defect the purpose of Counter class.

    # Counter works with non-numbers too
    special_counter = Counter(name='Pankaj', age=20)
    print(special_counter)  # Counter({'name': 'Pankaj', 'age': 20})
    

## Python Counter Methods

Let’s look into Counter class methods and some other operations we can perform on it.

### Getting Count of Elements

    # getting count
    counter = Counter({'Dog': 2, 'Cat': 1, 'Horse': 1})
    countDog = counter['Dog']
    print(countDog)  # 2
    

If we try to get the count of non-existing key, it will return 0 and not throw `KeyError`.

    # getting count for non existing key, don't cause KeyError
    print(counter['Unicorn'])  # 0
    

### Setting Count of Elements

We can also set the count of existing element in the counter. If the element doesn’t exist, then it gets added to the counter.

    counter = Counter({'Dog': 2, 'Cat': 1, 'Horse': 1})
    # setting count
    counter['Horse'] = 0
    print(counter)  # Counter({'Dog': 2, 'Cat': 1, 'Horse': 0})
    
    # setting count for non-existing key, adds to Counter
    counter['Unicorn'] = 1
    print(counter)  # Counter({'Dog': 2, 'Cat': 1, 'Unicorn': 1, 'Horse': 0})
    

### Deleting an element from Counter

We can use `del` to delete an element from the counter object.

    # Delete element from Counter
    del counter['Unicorn']
    print(counter)  # Counter({'Dog': 2, 'Cat': 1, 'Horse': 0})
    

### elements()

This method returns the list of elements in the counter. Only elements with positive counts are returned.

    counter = Counter({'Dog': 2, 'Cat': -1, 'Horse': 0})
    
    # elements()
    elements = counter.elements()  # doesn't return elements with count 0 or less
    for value in elements:
        print(value)
    

Above code will print “Dog” two times because it’s count is 2. Other elements will be ignored because they don’t have positive count. Counter is an unordered collection, so elements are returned in no particular order.

### most_common(n)

This method returns the most common elements from the counter. If we don’t provide value of ‘n’ then sorted dictionary is returned from most common the least common elements. We can use slicing to get the least common elements on this sorted dictionary.

    counter = Counter({'Dog': 2, 'Cat': -1, 'Horse': 0})
    
    # most_common()
    most_common_element = counter.most_common(1)
    print(most_common_element)  # [('Dog', 2)]
    
    least_common_element = counter.most_common()[:-2:-1]
    print(least_common_element)  # [('Cat', -1)]
    

### subtract() and update()

Counter `subtract()` method is used to subtract element counts from another counter. `update()` method is used to add counts from another counter.

    counter = Counter('ababab')
    print(counter)  # Counter({'a': 3, 'b': 3})
    c = Counter('abc')
    print(c)  # Counter({'a': 1, 'b': 1, 'c': 1})
    
    # subtract
    counter.subtract(c)
    print(counter)  # Counter({'a': 2, 'b': 2, 'c': -1})
    
    # update
    counter.update(c)
    print(counter)  # Counter({'a': 3, 'b': 3, 'c': 0})
    

## Python Counter Arithmetic Operations

We can perform some arithmetic operations on Counters too, just like numbers. However only elements with positive count are returned with these operations.

    # arithmetic operations
    c1 = Counter(a=2, b=0, c=-1)
    c2 = Counter(a=1, b=-1, c=2)
    
    c = c1 + c2  # return items having +ve count only
    print(c)  # Counter({'a': 3, 'c': 1})
    
    c = c1 - c2  # keeps only +ve count elements
    print(c)  # Counter({'a': 1, 'b': 1})
    
    c = c1 & c2  # intersection min(c1[x], c2[x])
    print(c)  # Counter({'a': 1})
    
    c = c1 | c2  # union max(c1[x], c2[x])
    print(c)  # Counter({'a': 2, 'c': 2})
    

## Miscellaneous Operations on Python Counter

Let’s look at some code snippets for miscellaneous operations we can perform on Counter objects.

    counter = Counter({'a': 3, 'b': 3, 'c': 0})
    # miscellaneous examples
    print(sum(counter.values()))  # 6
    
    print(list(counter))  # ['a', 'b', 'c']
    print(set(counter))  # {'a', 'b', 'c'}
    print(dict(counter))  # {'a': 3, 'b': 3, 'c': 0}
    print(counter.items())  # dict_items([('a', 3), ('b', 3), ('c', 0)])
    
    # remove 0 or negative count elements
    counter = Counter(a=2, b=3, c=-1, d=0)
    counter = +counter
    print(counter)  # Counter({'b': 3, 'a': 2})
    
    # clear all elements
    counter.clear()
    print(counter)  # Counter()
    

That’s all for Python Counter class.