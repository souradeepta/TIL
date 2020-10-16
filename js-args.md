# The arguments object

[
  ](https://developer.mozilla.org/en-US/docs/Web)

**`arguments`** is an `Array`-like object accessible inside [functions](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Guide/Functions) that contains the values of the arguments passed to that function.

<iframe class="interactive interactive-js" frameborder="0" height="250" src="https://interactive-examples.mdn.mozilla.net/pages/js/functions-arguments.html" title="MDN Web Docs Interactive Example" width="100%" style="margin: 0px; padding: 10px; border: 1px solid rgb(234, 242, 244); max-width: 100%; box-sizing: border-box; background-color: rgb(245, 249, 250); color: rgb(51, 51, 51); height: 490px; width: 1014px;"></iframe>

## Description

**Note:** If you're writing ES6 compatible code, then [rest parameters](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Functions/rest_parameters) should be preferred.

**Note:** “Array-like” means that `arguments` has a [`length`](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Functions/arguments/length) property and properties indexed from zero, but it doesn't have [`Array`](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/Array)'s built-in methods like [`forEach()`](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/Array/forEach) or [`map()`](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/Array/map). See [§Description](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Functions/arguments#Description) for details.

The `arguments` object is a local variable available within all non-[arrow](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Functions/Arrow_functions) functions. You can refer to a function's arguments inside that function by using its `arguments` object. It has entries for each argument the function was called with, with the first entry's index at `0`.

For example, if a function is passed 3 arguments, you can access them as follows:

```js
arguments[0] // first argument
arguments[1] // second argument
arguments[2] // third argument
```

Each argument can also be set or reassigned:

```js
arguments[1] = 'new value';
```

The `arguments` object is not an [`Array`](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/Array). It is similar, but lacks all `Array` properties except [`length`](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/Array/length). For example, it does not have the [`pop()`](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/Array/pop) method.

However, it can be converted to a real `Array`:

```js
var args = Array.prototype.slice.call(arguments);
// Using an array literal is shorter than above but allocates an empty array
var args = [].slice.call(arguments); 
```

As you can do with any Array-like object, you can use ES2015's [`Array.from()`](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/Array/from) method or [spread syntax](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Operators/Spread_syntax) to convert `arguments` to a real Array:

```js
let args = Array.from(arguments);
// or
let args = [...arguments];
```

The `arguments` object is useful for functions called with more arguments than they are formally declared to accept. This technique is useful for functions that can be passed a variable number of arguments, such as [`Math.min()`](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/Math/min). This example function accepts any number of string arguments and returns the longest one:

```js
function longestString() {
  var longest = '';
  for (var i=0; i < arguments.length; i++) {
    if (arguments[i].length > longest.length) {
      longest = arguments[i];
    }
  }
  return longest;
}
```

You can use [`arguments.length`](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Functions/arguments/length) to count how many arguments the function was called with. If you instead want to count how many parameters a function is declared to accept, inspect that function's [`length`](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/Function/length) property.

### Using typeof with arguments



The [`typeof`](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Operators/typeof) operator returns `'object'` when used with `arguments`

```js
console.log(typeof arguments); // 'object' 
```

The type of individual arguments can be determined by indexing `arguments`:

```html
console.log(typeof arguments[0]); // returns the type of the first argument
```

## Properties

- [`arguments.callee`](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Functions/arguments/callee)

  Reference to the currently executing function that the arguments belong to. Forbidden in strict mode.

- [`arguments.length`](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Functions/arguments/length)

  The number of arguments that were passed to the function.

- [`arguments[@@iterator\]`](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Functions/arguments/@@iterator)

  Returns a new [Array iterator](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/Array/@@iterator) object that contains the values for each index in `arguments`.

## Examples

### Defining a function that concatenates several strings



This example defines a function that concatenates several strings. The function's only formal argument is a string containing the characters that separate the items to concatenate.

```js
function myConcat(separator) {
  let args = Array.prototype.slice.call(arguments, 1);
  return args.join(separator);
}
```

You can pass as many arguments as you like to this function. It returns a string list using each argument in the list:

```js
// returns "red, orange, blue"
myConcat(', ', 'red', 'orange', 'blue');

// returns "elephant; giraffe; lion; cheetah"
myConcat('; ', 'elephant', 'giraffe', 'lion', 'cheetah');

// returns "sage. basil. oregano. pepper. parsley"
myConcat('. ', 'sage', 'basil', 'oregano', 'pepper', 'parsley');
```

### Defining a function that creates HTML lists



This example defines a function that creates a string containing HTML for a list. The only formal argument for the function is a string that is "`u`" if the list is to be [unordered (bulleted)](https://developer.mozilla.org/en-US/docs/Web/HTML/Element/ul), or "`o`" if the list is to be [ordered (numbered)](https://developer.mozilla.org/en-US/docs/Web/HTML/Element/ol). The function is defined as follows:

```js
function list(type) {
  var html = '<' + type + 'l><li>';
  var args = Array.prototype.slice.call(arguments, 1);
  html += args.join('</li><li>');
  html += '</li></' + type + 'l>'; // end list 
  return html; 
}
```

You can pass any number of arguments to this function, and it adds each argument as a list item to a list of the type indicated. For example:

```js
let listHTML = list('u', 'One', 'Two', 'Three');

/* listHTML is:
"<ul><li>One</li><li>Two</li><li>Three</li></ul>"
*/
```

### Rest, default, and destructured parameters



The `arguments` object can be used in conjunction with [rest](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Functions/rest_parameters), [default](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Functions/Default_parameters), and [destructured](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Operators/Destructuring_assignment) parameters.

```js
function foo(...args) {
  return args;
}
foo(1, 2, 3); // [1, 2, 3]
```

While the presence of rest, default, or destructured parameters does not alter [the behavior of the `arguments` object in strict mode code](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Strict_mode#Making_eval_and_arguments_simpler), there are subtle differences for non-strict code.

In strict-mode code, the `arguments` object behaves the same whether or not a function is passed rest, default, or destructured parameters. That is, assigning new values to variables in the body of the function will not affect the `arguments` object. Nor will assigning new variables to the `arguments` object affect the value of variables.

**Note:** You cannot write a `"use strict";` directive in the body of a function definition that accepts rest, default, or destructured parameters. Doing so will throw [a syntax error](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Errors/Strict_Non_Simple_Params).

Non-strict functions that are passed only simple parameters (that is, not rest, default, or restructured parameters) will sync the value of variables new values in the body of the function with the `arguments` object, and vice versa:

```js
function func(a) {
  arguments[0] = 99; // updating arguments[0] also updates a
  console.log(a);
}
func(10); // 99
```

And also:

```js
function func(a) {
  a = 99; // updating a also updates arguments[0]
  console.log(arguments[0]);
}
func(10); // 99
```

Conversely, non-strict functions that **are** passed rest, default, or destructured parameters **will not** sync new values assigned to argument variables in the function body with the `arguments` object. Instead, the `arguments` object in non-strict functions with complex parameters **will always** reflect the values passed to the function when the function was called (this is the same behavior as exhibited by all strict-mode functions, regardless of the type of variables they are passed):

```js
function func(a = 55) {
  arguments[0] = 99; // updating arguments[0] does not also update a
  console.log(a);
}
func(10); // 10
```

And also:

```js
function func(a = 55) {
  a = 99; // updating a does not also update arguments[0]
  console.log(arguments[0]);
}
func(10); // 10
```

And also:

```js
// An untracked default parameter
function func(a = 55) {
  console.log(arguments[0]);
}
func(); // undefined
```