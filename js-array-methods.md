# How To Use Array Methods in JavaScript: Mutator Methods

[Development](https://www.digitalocean.com/community/tags/development)[JavaScript](https://www.digitalocean.com/community/tags/javascript)

- [![img](https://community-cdn-digitalocean-com.global.ssl.fastly.net/variants/NozA8zXAgVXdHk4QLYXoAKbH/6a540d0de9b44df9962f62205489ba6e2a5beab748fa47e816b60597a256ba9b)](https://www.digitalocean.com/community/users/taniarascia)

- By [Tania Rascia](https://www.digitalocean.com/community/users/taniarascia)

  Published onAugust 10, 2017 32.2kviews

### Introduction

[Arrays](https://www.digitalocean.com/community/tutorials/understanding-data-types-in-javascript#arrays) in JavaScript consist of a list of elements. JavaScript has many useful built-in methods to work with arrays. Methods that modify the original array are known as **mutator** methods, and methods that return a new value or representation are known as [**accessor** methods](https://www.digitalocean.com/community/tutorials/how-to-use-array-methods-in-javascript-accessor-methods). In this tutorial, we will focus on mutator methods.

In order to get the most out of this tutorial, you should have some familiarity with creating, indexing, modifying, and looping through arrays, which you can review in the tutorial [Understanding Arrays in JavaScript](https://www.digitalocean.com/community/tutorials/understanding-arrays-in-javascript).

Arrays are similar to [strings](https://www.digitalocean.com/community/tutorials/how-to-index-split-and-manipulate-strings-in-javascript), in that they both consist of a sequence of elements that can be accessed via index number. However, it is important to remember that strings are an immutable datatype, meaning they cannot be changed. Arrays, on the other hand, are mutable, which means that many array methods will affect the original array, not a  of the array.

This tutorial will go through adding and removing elements, reversing, replacing, and otherwise modifying elements in an array.

**Note:** Array methods are properly written out as `Array.prototype.method()`, as `Array.prototype` refers to the `Array` object itself. For simplicity, we will simply list the name as `method()`.



## isArray()

Before we get into mutator methods, let’s look at the `isArray()` method to test whether objects are arrays. This is a [Boolean](https://www.digitalocean.com/community/tutorials/understanding-data-types-in-javascript#booleans) method that returns `true` if the value of a variable is equal to an array. If the object is not an array, this method returns `false`.

```js
let fish = [ "piranha", "barracuda", "koi", "eel" ];

// Test if fish variable is an array
Array.isArray(fish);
```

 



```
Outputtrue
```

The `isArray()` method is useful because the `typeof` operator we would normally use for testing returns `object` when used with arrays, and sometimes knowing the distinction between an object and an `Array` object is necessary.

Note that `isArray()` is written differently from most array methods, with the array variable being provided as an argument to the method.

Now that we know how to check to ensure that an object is an array, let’s move on to mutator methods.



## pop()

The first mutator method we’ll look at is the `pop()` method, which removes the last element from the end of an array.

We’ll begin with our `fish` array.

```js
let fish = [ "piranha", "barracuda", "koi", "eel" ];
```

 



Let’s initialize the `pop()` method in order to remove the last item. In this case, it will be the string literal `"eel"`.

```js
// Use pop method to remove an item from the end of an array
fish.pop();
```

 



We’ll call our array to ensure that the array is returned without the last item:

```js
fish;
```

 



```
Output[ 'piranha', 'barracuda', 'koi' ]
```

We’ve successfully removed `"eel"` from the `fish` array. The `pop()` method takes no additional parameters.



## shift()

Another mutator method, the `shift()` method removes the first element from the beginning of an array.

```js
let fish = [ "piranha", "barracuda", "koi", "eel" ];
```

 



We will use `shift()` to remove `"piranha"` from index `0` and shift all the rest of the elements down by one index number.

```js
// Use shift method to remove an item from the beginning of an array
fish.shift();

fish;
```

 



```
Output[ 'barracuda', 'koi', 'eel' ]
```

In this example, `"piranha"` has been removed and each item has shifted down one index number. For this reason, it is generally preferred to use the `pop()` method whenever possible, as the other array elements will maintain their index positions.



## push()

The `push()` mutator method adds a new element or elements to the end of an array.

```js
let fish = [ "piranha", "barracuda", "koi", "eel" ];
```

 



In order to add an item at the end, we write the new element as a parameter of the function.

```js
// Use push method to add an item to the end of an array
fish.push("swordfish");

fish;
```

 



```
Output[ 'piranha', 'barracuda', 'koi', 'eel', 'swordfish' ]
```

It is also possible to add multiple new values to the array. For example, `fish.push("swordfish", "dragonfish")` would have added items to index `4` and `5`.



## unshift()

The `unshift()` mutator array method adds a new element or elements to the beginning of an array.

```js
let fish = [ "piranha", "barracuda", "koi", "eel" ];
```

 



```js
// Use unshift method to add an item to the beginning of an array
fish.unshift("shark");

fish;
```

 



```
Output[ 'shark', 'piranha', 'barracuda', 'koi', 'eel' ]
```

In the above example, `"shark"` was added to index position `0`, shifting all the other array elements by one. Just as with `shift()`, you can add multiple comma-separated items to the array at once.

`pop()` and `push()` affect the end of an array, and `shift()` and `unshift()` affect the beginning of an array. An easy way to remember this is to keep in mind that `shift()` and `unshift()` will change all the index numbers of the returned array



## splice()

The `splice()` method can add or remove an item from any position in an array. A mutator method, `splice()` can either add or remove, or add and remove simultaneously.

`splice()` takes three parameters — the index number to start at, the number of items to remove, and items to add (optional).

```
splice(index number, number of items to remove, items to add)
```

`splice(0, 0, "new")` would add the string `"new"` to the beginning of an array, and delete nothing.

Let’s look at a few examples below at how `splice()` can add and remove items in an array.

### Adding with `splice()`

If we set our second parameter (items to remove) as `0`, `splice()` will delete zero items. In this way, we can choose to only add an item starting at any index number, making `splice()` more powerful than `push()` or `unshift()`, which only add items to the end or beginning of an array.

```js
let fish = [ "piranha", "barracuda", "koi", "eel" ];

// Splice a new item number into index position 1
fish.splice(1, 0, "manta ray");

fish;
```

 



```js
Output[ 'piranha', 'manta ray', 'barracuda', 'koi', 'eel' ]
```

 



The new string, `"manta ray"`, has been added into the array, starting at index `1`.

### Removing with `splice()`

If we leave the third parameter (items to add) blank, we can simply remove an item from any point in the array.

```js
let fish = [ "piranha", "barracuda", "koi", "eel" ];

// Remove two items, starting at index position 1
fish.splice(1, 2);

fish;
```

 



```js
Output[ 'piranha', 'eel' ]
```

 



We deleted two items from the array, starting with index `1`, `"barracuda"`. If the second argument is removed, all items to the end of the array will be removed.

### Adding and Removing with `splice()`

Using all the parameters at once, we can both add and remove items from an array at the same time.

To demonstrate this, let’s remove the same items as we did above, and add a new one in their positions.

```js
let fish = [ "piranha", "barracuda", "koi", "eel" ];

// Remove two items and add one
fish.splice(1, 2, "manta ray");

fish;
```

 



```
Output[ 'piranha', 'manta ray', 'eel' ]
```

`splice()` is a powerful method for modifying any part of an array. Note that `splice()` is not to be confused with `slice()` an accessor array which will make a  of a section of an array.



## reverse()

The `reverse()` method reverses the order of the elements in an array.

```js
let fish = [ "piranha", "barracuda", "koi", "eel" ];
```

 



Using `reverse()`, the last element will be first, and the first element will be last.

```js
// Reverse the fish array
fish.reverse();

fish;
```

 



```
Output[ 'eel', 'koi', 'barracuda', 'piranha' ]
```

The `reverse()` array method has no parameters.



## fill()

The `fill()` method replaces all the elements in an array with a static value.

```js
let fish = [ "piranha", "barracuda", "koi", "eel" ];
```

 



In the `fish` array, we have four items. Let’s apply `fill()`.

```js
// Replace all values in the array with "shark"
fish.fill("shark");

fish;
```

 



```
Output[ 'shark', 'shark', 'shark', 'shark' ]
```

All four items in the array have been replaced with the same value, `"shark"`. `fill()` also takes optional arguments of start and end points.

```js
fish.fill("shark", 1) // > [ 'piranha', 'shark', 'shark', 'shark' ]
fish.fill("shark", 1, 3); // > [ 'piranha', 'shark', 'shark', 'eel' ]
```

 



Using `fill()` we can replace one or more elements in an array with a static value.



## sort()

The `sort()` method sorts the elements in an array based on the first character in the element. In the case that the first character is identical, it will continue down the line and compare the second character, and so on.

By default, `sort()` will alphabetize an array of strings that are all either uppercase or lowercase.

```js
let fish = [ "piranha", "barracuda", "koi", "eel" ];

// Sort items in array
fish.sort();

fish;
```

 



```
Output[ 'barracuda', 'eel', 'koi', 'piranha' ]
```

Since `sort()` is based on the first unicode character, it will sort uppercase items before lowercase.

Let’s modify our original array so that one of our strings begin with an uppercase letter.

```js
let fish = [ "piranha", "barracuda", "Koi", "eel" ];

fish.sort();

fish;
```

 



```
Output[ 'Koi', 'barracuda', 'eel', 'piranha' ]
```

Numbers come before both uppercase and lowercase characters.

We can again modify the array to include a number in one of the string items.

```js
let fish = [ "piranha", "barracuda", "Koi", "1 eel" ];

fish.sort();
```

 



```
Output[ '1 eel', 'Koi', 'barracuda', 'piranha' ]
```

`sort()` will not sort an array of numbers by size by default. Instead, it will only check the first character in the number.

```js
let numbers = [ 42, 23, 16, 15, 4, 8 ];

numbers.sort();
```

 



```
Output[ 15, 16, 23, 4, 42, 8 ]
```

In order to sort numbers properly, you could create a comparison function as an argument.

```js
// Function to sort numbers by size
const sortNumerically = (a, b) => {
  return a - b;
}

numbers.sort(sortNumerically);
```

 



```
Output[ 4, 8, 15, 16, 23, 42 ]
```

The `sortNumerically` comparison function allowed us to sort as intended. `sort()` will apply the change to the original array.



## Conclusion

In this tutorial, we reviewed the major mutator array methods in JavaScript. Mutator methods modify the original array they are used on, as opposed to creating a  like accessor methods do. We learned how to add and remove elements to the beginning or end of an array, as well as sorting, reversing, and replacing the value of array items.

To review the basics of arrays, read [Understanding Arrays in JavaScript](https://www.digitalocean.com/community/tutorials/understanding-arrays-in-javascript). To see a complete list of all array methods, view the [Array reference on Mozilla Developer Network](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/Array).



# How To Use Array Methods in JavaScript: Accessor Methods

[Development](https://www.digitalocean.com/community/tags/development)[JavaScript](https://www.digitalocean.com/community/tags/javascript)

- [![img](https://community-cdn-digitalocean-com.global.ssl.fastly.net/variants/NozA8zXAgVXdHk4QLYXoAKbH/6a540d0de9b44df9962f62205489ba6e2a5beab748fa47e816b60597a256ba9b)](https://www.digitalocean.com/community/users/taniarascia)

- By [Tania Rascia](https://www.digitalocean.com/community/users/taniarascia)

  Published onAugust 14, 2017 22.5kviews

### Introduction

The JavaScript array is a data type that consists of a list of elements. There are many useful built-in methods to work with arrays in JavaScript. Methods that modify the original array are known as **mutator** methods, and methods that return a new value or representation are known as **accessor** methods. In this tutorial, we will focus on accessor methods.

In order to get the most out of this tutorial, you should have some familiarity with creating, indexing, modifying, and looping through arrays, which you can review in the tutorial [Understanding Arrays in JavaScript](https://www.digitalocean.com/community/tutorials/understanding-arrays-in-javascript).

This tutorial will go over methods that will concatenate arrays, convert arrays to strings,  portions of an array to a new array, and find the indices of arrays.

**Note:** Array methods are properly written out as `Array.prototype.method()`, as `Array.prototype` refers to the `Array` object itself. For simplicity, we will simply list the name as `method()`.



## concat()

The `concat()` method merges two or more arrays together to form a new array.

In the below example, we will create two arrays of types of shellfish and combine them into one new array.

```js
// Create arrays of monovalves and bivalves
let monovalves = [ "abalone", "conch" ];
let bivalves = [ "oyster", "mussel", "clam" ];

// Concatenate them together into shellfish variable
let shellfish = monovalves.concat(bivalves);
```

 



Once we call the new array, we’ll see that it consists of a combination of the two original arrays:

```js
shellfish;
```

 



```
Output[ 'abalone', 'conch', 'oyster', 'mussel', 'clam' ]
```

The `concat()` method can take multiple arguments, effectively allowing you to concatenate many arrays together with a single method.



## join()

The `join()` method converts all the elements of an array into a new string.

```js
let fish = [ "piranha", "barracuda", "koi", "eel" ];
```

 



If no argument is given, the output of `join()` will be a comma-separated string with no extra whitespace.

```js
// Join the elements of an array into a string
let fishString = fish.join();

fishString;
```

 



```
Output'piranha,barracuda,koi,eel'
```

In order to include whitespace or another separator, you can add a string of your separator as a parameter to the `join()` method. This parameter will contain the separator you would like between each array element.

```js
// Join the elements of an array into a string
let fishString = fish.join(', ');

fishString;
```

 



```
Output'piranha, barracuda, koi, eel'
```

In the above example, writing `', '` with whitespace separated the array items in a more readable fashion. An empty string provided as an argument will remove the default commas completely.



## slice()

The `slice()` method copies a portion of an array to a new array.

```js
let fish = [ "piranha", "barracuda", "koi", "eel" ];
```

 



Suppose we would like to  the last two items in the array to a new array. We would start with the index number of the first element we want, which is `2` for `koi`. We would end with the index number *following* the last element we want. Because the last element, `eel`, has the index number of `3`, we would put `4`.

```js
// Slice a new array from 2 to 5
let fishWithShortNames = fish.slice(2, 4);

fishWithShortNames;
```

 



```
Output[ 'koi', 'eel' ]
```

In this particular case, since `eel` is the last item in the array, the second argument is actually unnecessary. `slice()` will start at the first index and stop at the end of the array if no second argument is provided.

```js
// Slice a new array from 2 to the end of the array
let fishWithShortNames = fish.slice(2);

fishWithShortNames;
```

 



```
Output[ 'koi', 'eel' ]
```

`slice()` is not to be confused with the [mutator method `splice()`](https://www.digitalocean.com/community/tutorials/how-to-use-array-methods-in-javascript-mutator-methods#splice()), which can add or delete items from the original array.



## indexOf()

The `indexOf()` method returns the index number of the first instance of an element.

In the below example, we have a string in which `barracuda` is listed twice.

```js
let fish = [ "piranha", "barracuda", "koi", "barracuda" ];
```

 



We will use `indexOf()` to find the first instance.

```js
// Find the first instance of an element
fish.indexOf("barracuda");
```

 



```
Output1
```

If the given argument is a value that does not exist in the array, the console will return `-1`.

```js
fish.indexOf("shark");
```

 



```
Output-1
```

The `indexOf()` method is especially useful in arrays that contain many items.



## lastIndexOf()

The `lastIndexOf()` method returns the index number of the last instance of an element.

We can test on the same example from `indexOf()`, which includes `barracuda` twice.

```js
let fish = [ "piranha", "barracuda", "koi", "barracuda" ];

// Find the last instance of an element
fish.lastIndexOf("barracuda");
```

 



```
Output3
```

`lastIndexOf()` will search the array starting from the end and return the first index number it finds.



## Conclusion

In this tutorial, we reviewed the major built-in accessor array methods in JavaScript. Accessor methods create a new  or representation of an array, as opposed to mutating or modifying the original.

We learned how to concatenate arrays together, which combines them end-to-end, as well as how to convert arrays into comma-separated strings. We also learned how to  portions of an array into a new array, and find the first and last indices of a given element in an array.



# How To Use Array Methods in JavaScript: Iteration Methods

[Development](https://www.digitalocean.com/community/tags/development)[JavaScript](https://www.digitalocean.com/community/tags/javascript)

- [![img](https://community-cdn-digitalocean-com.global.ssl.fastly.net/variants/NozA8zXAgVXdHk4QLYXoAKbH/6a540d0de9b44df9962f62205489ba6e2a5beab748fa47e816b60597a256ba9b)](https://www.digitalocean.com/community/users/taniarascia)

- By [Tania Rascia](https://www.digitalocean.com/community/users/taniarascia)

  Published onAugust 15, 2017 102kviews

### Introduction

In JavaScript, the [array](https://www.digitalocean.com/community/tutorials/understanding-data-types-in-javascript#arrays) data type consists of a list of elements. There are many useful built-in methods available for JavaScript developers to work with arrays. Methods that modify the original array are known as [**mutator** methods](https://www.digitalocean.com/community/tutorials/how-to-use-array-methods-in-javascript-mutator-methods), and methods that return a new value or representation are known as [**accessor** methods](https://www.digitalocean.com/community/tutorials/how-to-use-array-methods-in-javascript-accessor-methods).

There is a third class of array methods, known as **iteration** methods, which are methods that operate on every item in an array, one at a time. These methods are closely associated with loops. In this tutorial, we will be focusing on iteration methods.

In order to get the most out of this tutorial, you should have some familiarity with creating, indexing, modifying, and looping through arrays, which you can review in the tutorial [Understanding Arrays in JavaScript](https://www.digitalocean.com/community/tutorials/understanding-arrays-in-javascript).

In this tutorial, we will use iteration methods to loop through arrays, perform functions on each item in an array, filter the desired results of an array, reduce array items down to a single value, and search through arrays to find values or indices.

**Note:** Array methods are properly written out as `Array.prototype.method()`, as `Array.prototype` refers to the `Array` object itself. For simplicity, we will simply list the name as `method()`.



## Understanding Arrow Functions

Many examples throughout this tutorial will be using JavaScript [arrow function expressions](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Functions/Arrow_functions), which are represented by an equals sign followed by a greater than sign: `=>`.

A **function** is a block of reusable code that can be executed. Traditionally, a function can be written with the following syntax:

```js
var example = function() {
  // code to execute
}

example();
```

 

Copy

The latest version of JavaScript at the time of writing allows for the use of arrow functions, which can be written with the following syntax:

```js
var example = () => {
  // code to execute
}

example();
```

 

Copy

The parentheses in either case may contain parameters. When there is only one parameter, the parentheses can be omitted, as such:

```js
var example = parameter1 => {
  // code to execute
}
```

 

Copy

Throughout the examples in this tutorial, we will use the arrow function syntax. To read and understand more about functions in JavaScript, read the [Functions reference on the Mozilla Developer Network](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Functions).



## forEach()

The `forEach()` method calls a function for each element in an array.

Let’s start with the following array assigned to the variable `fish`:

```js
let fish = [ "piranha", "barracuda", "cod", "eel" ];
```

 

Copy

We can use `forEach()` to print each item in the `fish` array to the console.

```js
// Print out each item in the array
fish.forEach(individualFish => {
    console.log(individualFish);
})
```

 

Copy

Once we do so, we’ll receive the following output:

```
Outputpiranha
barracuda
cod
eel
```

Another way to do this is using the [`for` loop](https://www.digitalocean.com/community/tutorials/how-to-construct-for-loops-in-javascript) keyword and testing it against the length property of the array.

```js
// Loop through the length of the array
for (let i = 0; i < fish.length; i++) {
    console.log(fish[i]);
}
```

 

Copy

The above code will have the same output as using the `forEach()` method. As an iteration method specifically intended for use with arrays, `forEach()` is more concise and straightforward for this particular task.



## map()

The `map()` method creates a new array with the results of a function call on each element in the array.

For an example of how to use the iteration method `map()`, we can print each iteration of a loop to the console. `map()` does not mutate the original array, it instead returns a new array value. Unlike `forEach()`, the `map()` method must be assigned to a new variable.

```js
let fish = [ "piranha", "barracuda", "cod", "eel" ];

// Print out each item in the array
let printFish = fish.map(individualFish => {
    console.log(individualFish);
});

printFish;
```

 

Copy

```
Outputpiranha
barracuda
cod
eel
```

We can also use `map()` to change the values of each item in an array. To demonstrate this, we’ll add an `s` to the end of each item in the `fish` array to pluralize each word.

```js
// Pluralize all items in the fish array
let pluralFish = fish.map(individualFish => {
    return `${individualFish}s`;
});

pluralFish;
```

 

Copy

```
Output[ 'piranhas', 'barracudas', 'cods', 'eels' ]
```

The original `fish` variable is unchanged, but `pluralFish` now contains a modified version of the original variable.



## filter()

The `filter()` method creates a new array with the elements that pass the result of a given test.

We could use `filter()` to return a new array containing only the items in a list that start with a specific letter. To do this, we can utilize [string indexing](https://www.digitalocean.com/community/tutorials/how-to-index-split-and-manipulate-strings-in-javascript#how-strings-are-indexed) to call the first item (or letter) in each string item of the array.

```js
let seaCreatures = [ "shark", "whale", "squid", "starfish", "narwhal" ];

// Filter all creatures that start with "s" into a new list
let filteredList = seaCreatures.filter(creature => {
  return creature[0] === "s";
});

filteredList;
```

 

Copy

```
Output[ 'shark', 'squid', 'starfish' ]
```

We tested which items in the array have an `s` at the `0` index, and assigned the result into a new variable.

`filter()` is an iteration method, and does not mutate the original array.



## reduce()

The `reduce()` method will reduce an array to a single value.

This is seen commonly with numbers, such as finding the sum of all the numbers in an array.

```js
let numbers = [ 42, 23, 16, 15, 4, 8 ];

// Get the sum of all numerical values
let sum = numbers.reduce((a, b) => {
    return a + b;
});

sum;
```

 

Copy

```
Output108
```

`reduce()` can also be used with [strings](https://www.digitalocean.com/community/tutorials/understanding-data-types-in-javascript#strings) and other [data types](https://www.digitalocean.com/community/tutorials/understanding-data-types-in-javascript). The value returned by `reduce()` can be a number, string, array, or other data type. `reduce()` is an iteration method that does not mutate the original array.



## find()

The `find()` method returns the first value in an array that passes a given test.

As an example, we will create an array of sea creatures.

```js
let seaCreatures = [ "whale", "octopus", "shark", "cuttlefish", "flounder" ];
```

 

Copy

Then we will use the `find()` method to test if any of the creatures in the array are cephalopods.

```js
// Check if a given value is a cephalopod
const isCephalopod = cephalopod => {
    return [ "cuttlefish", "octopus" ].includes(cephalopod);
}

seaCreatures.find(isCephalopod);
```

 

Copy

```
Outputoctopus
```

Since `octopus` was the first entry in the array to satisfy the test in the `isCephalopod()` function, it is the first value to be returned.

The `find()` method can help you work with arrays that contain many values.



## findIndex()

The `findIndex()` method returns the first index in an array that passes a given test.

We can use the same `seaCreatures` example from the `find()` method.

```js
let seaCreatures = [ "whale", "octopus", "shark", "cuttlefish", "flounder" ];
```

 

Copy

Using the `isCephalopod` test, we will find the index number instead of the value of the first match.

```js
// Check if a given value is a cephalopod
const isCephalopod = cephalopod => {
    return [ "cuttlefish", "octopus" ].includes(cephalopod);
}

seaCreatures.findIndex(isCephalopod);
```

 

Copy

```
Output1
```

`octopus` is the first item to match the test and has an index of `1`, therefore it is the index number that is returned.

If the test is not satisfied, `findIndex()` will return `-1`.

```js
const isThereAnEel = eel => {
    return [ "eel" ].includes(eel);
}

seaCreatures.findIndex
```

 

Copy

```
Output-1
```

The `findIndex()` method is particularly useful when working with arrays containing many items.



## Conclusion

In this tutorial, we reviewed the major built-in iteration array methods in JavaScript. Iteration methods operate on every item in an array, and often perform a new function. We went over how to loop through arrays, change the value of each item in an array, filter and reduce arrays, and find values and indices.