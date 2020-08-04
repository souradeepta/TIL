# A quick primer on type traits in modern C++

*Discovering one of the pillars of C++ generic programming.*



**Type traits** are a clever technique used in C++ template metaprogramming that gives you the ability to inspect and transform the properties of *types*.

For example, given a generic type `T` — it could be `int`, `bool`, `std::vector` or whatever you want — with type traits you can ask the compiler some questions: is it an integer? Is it a function? Is it a pointer? Or maybe a class? Does it have a destructor? Can you copy it? Will it throw exceptions? ... and so on. This is extremely useful in **conditional compilation**, where you instruct the compiler to pick the right path according to the type in input. We will see an example shortly.

Type traits can also apply some transformation to a type. For example, given `T`, you can add/remove the `const` specifier, the reference or dereference the pointer, or yet turn it into a signed/unsigned type and many other crazy operations. Extremely handy when writing libraries that make use of templates.

The beauty of these techniques is that everything takes place at *compile time* with no runtime penalties: it's template metaprogramming, after all. I assume you know a bit about C++ templates for the rest of this article. [This guide](https://www.codeproject.com/Articles/257589/An-Idiots-Guide-to-Cplusplus-Templates-Part-1) is a great introduction if you don't.



## What is a type trait?

A type trait is a simple template struct that contains a member constant, which in turn holds the answer to the question the type trait asks or the transformation it performs. For example, let's take a look at `std::is_floating_point`, one of the many type traits defined by the C++ Standard Library in the [``](https://en.cppreference.com/w/cpp/header/type_traits) header:

```c++
template<typename T> 
struct is_floating_point;
```

This type trait tells whether a type `T` is floating point or not. The member constant — called `value` for type traits that ask a question — will be either set to `true` or `false` according to the type passed in as template argument.

On the other hand, for example `std::remove_reference` is a type trait that alters the type `T` it takes in input:

```c++
template<typename T>
struct remove_reference;
```

This type trait basically turns `T&` into `T`. The member constant — called `type` for those type traits that modify a type — contains the result of the transformation.



## How do I use a type trait?

Simply instantiate the template struct with the type you want, then inspect its member constant and act accordingly. For example, let's say you just want to print out if a type is floating point or not:

```c++
#include <iostream>
#include <type_traits>

class Class {};

int main() 
{
    std::cout << std::is_floating_point<Class>::value << '\n';
    std::cout << std::is_floating_point<float>::value << '\n';
    std::cout << std::is_floating_point<int>::value << '\n';
}
```

This program will output:

```nolang
0
1
0
```

### How does it work exactly?

In the snippet above you are passing three different types to the template struct `std::is_floating_point`: a custom `Class` type, a `float` and an `int`. The compiler, as with any regular template stuff, will generate three different structs for you under the hood:

```c++
struct is_floating_point_Class {
    static const bool value = false;
};

struct is_floating_point_float {
    static const bool value = true;
};

struct is_floating_point_int {
    static const bool value = false;
};
```

At this point it's just a matter of reading the `value` member inside those structs created by the compiler. Being static, you have to access the member constant with the `::` syntax. Just keep in mind that this is template metaprogramming, so everything takes place at *compile time*.



## Type traits in action, part 1: conditional compilation

Now that we have grasped the idea behind type traits, let's try to use them in some real world scenarios. Suppose you have two functions for the same algorithm: one that works with signed integers and another one that is super optimized for unsigned ones. You want the compiler to pick the signed one when an `int` is passed in and the unsigned one in case of an `unsigned int` to take advantage of the optimizations. This is the conditional compilation I mentioned before.

For this task I will be using three tools:

- the C++17 `if constexpr` syntax: an `if` statement that works at compile time;
- the C++11 `static_assert` function that, as the name implies, triggers an assert at compile time if the condition is not met;
- two self-explanatory type traits: `std::is_signed` and `std::is_unsigned`.

The code looks like this:

```c++
void algorithm_signed  (int i)      { /*...*/ } 
void algorithm_unsigned(unsigned u) { /*...*/ } 


template <typename T>
void algorithm(T t)
{
    if constexpr(std::is_signed<T>::value)
        algorithm_signed(t);
    else
    if constexpr (std::is_unsigned<T>::value)
        algorithm_unsigned(t);
    else
        static_assert(std::is_signed<T>::value || std::is_unsigned<T>::value, "Must be signed or unsigned!");
}
```

In words, the template function `algorithm` acts as a *dispatcher*: when instantiated, the compiler will grab the right function according to the type `T` passed in. If signed, `algorithm_signed` will be included; if unsigned, `algorithm_unsigned` will be included instead. Finally, throw a static assertion (i.e. a build error) if the type doesn't meet the criteria.

Some usage examples:

```c++
algorithm(3);       // T is int, include algorithm_signed()

unsigned x = 3;
algorithm(x);       // T is unsigned int, include algorithm_unsigned()

algorithm("hello"); // T is string, build error!
```



## Type traits in action, part 2: altering types

Type traits are also used to apply transformation to types. A typical usage of this magic comes from the [C++ Standard Library](https://www.internalpointers.com/post/c-c-standard-library) and [`std::move`](https://en.cppreference.com/w/cpp/utility/move): the utility function that turns a type `T` into an rvalue reference `T&&`. This is an important operation that paves the way for [move semantics](https://www.internalpointers.com/post/c-rvalue-references-and-move-semantics-beginners).

Internally, `std::move` makes use of the [`std::remove_reference`](https://en.cppreference.com/w/cpp/types/remove_reference) type trait to shave off the `&` (if any) from the type in input and to return a *clean* `T` with the `&&` attached. A possible implementation:

```c++
template <typename T>
typename remove_reference<T>::type&& move(T&& arg)
{
  return static_cast<typename remove_reference<T>::type&&>(arg);
}
```

Transformations like this one are widespread across the whole Standard Library, often used to optimize how function parameters flow across nested template function calls. All in all some of these type traits are rarely useful for average C++ projects, unless you are writing a library or performing some clever metaprogramming tricks.

## Beautifying type traits

Reading `::value` and `::type` everywhere in your code is confusing. Luckily, C++14 and greater introduced a simplified syntax thanks to some helper aliases that end with `_v` and `_t` respectively. So for example:

```c++
std::is_signed<T>::value;     /* ---> */   std::is_signed_v<T>;
std::remove_const<T>::type;   /* ---> */   std::remove_const_t<T>;
```

These helpers exist for all type traits that query a type or apply a transformation on it.



## More type traits trivia and further readings

Type traits act as a foundation for many C++ features and, as always, in this article I've barely scratched their surface. The following is a list of additional topics that deserve more love in the future.

### The source of type traits knowledge

How does a type trait know about a type? How can it infer that, for example, `std::is_signed_v<T>` is true for an `int`? Most basic type traits are the result of template metaprogramming tricks, [SFINAE](https://en.wikipedia.org/wiki/Substitution_failure_is_not_an_error), [tag dispatch](https://arne-mertz.de/2016/10/tag-dispatch/) and other techniques from the dark corners of C++.

Some type traits need additional help instead. For example the `std::is_abstract` type trait — which tells if a type is an abstract class or not — can't be generated with template metaprogramming alone. For this reason developers who work on the Standard Library make use of **intrinsics**: special built-in functions provided by the compiler that give more insight about the type in question, thanks to the deep knowledge a compiler has on the program it takes in input. More information [here](https://stackoverflow.com/questions/20181702/which-type-traits-cannot-be-implemented-without-compiler-hooks) and [here](https://github.com/Quuxplusone/from-scratch/blob/master/include/scratch/bits/type-traits/compiler-magic.md).

### Type traits and concepts

**Concepts** are an important addition in C++20: an elegant and expressive way to put a constraint on the types a template function or class can take in. For example, in the conditional compilation example above I could have used a concept instead of triggering the static assertion at the end. Not surprisingly, concepts are based on the numerous type traits defined in the Standard Library. More information about concepts [here](https://en.cppreference.com/w/cpp/language/constraints).

### Type traits provide introspection

**Introspection** is the ability of a program to examine the type or properties of an object. For example, with introspection you can ask an object if it has a specific member function in order to call it.

C++ is not capable of introspection at *runtime*, but as we saw in this article it does a good job at compile time thanks to type traits. We definitely used compile-time introspection when we checked if `T` was signed or not in the previous examples.

On the other hand, **reflection** refers to the ability of a program to observe and alter its own structure or its behavior. There's no such thing in C++ for now, but some programming artists are working on crazy libraries such as [magic_get](https://github.com/apolukhin/magic_get) by leveraging the power of type traits combined to template metaprogramming. There are also some proposals to include reflection in modern C++, drafted [here](http://www.open-std.org/jtc1/sc22/wg21/docs/papers/2019/n4818.pdf) and [here](http://www.open-std.org/jtc1/sc22/wg21/docs/papers/2019/p1240r1.pdf). Time will tell...



## Sources

Cppreference — [Type](https://en.cppreference.com/w/cpp/language/type)
Cppreference — [Types](https://en.cppreference.com/w/cpp/types)
Cppreference — [static_assert](https://en.cppreference.com/w/cpp/language/static_assert)
IBM — [Introduction to Type Traits in the C++ standard library](https://www.ibm.com/developerworks/community/blogs/5894415f-be62-4bc0-81c5-3956e82276f3/entry/introduction_to_type_traits_in_the_c_standard_library?lang=en)
Wikipedia — [Reflection (computer programming)](https://en.wikipedia.org/wiki/Reflection_(computer_programming))
Wikipedia — [Type introspection](https://en.wikipedia.org/wiki/Type_introspection)
Wikipedia — [Intrinsic function](https://en.wikipedia.org/wiki/Intrinsic_function)
gracicot.github.io — [Reflection in C++ Part 1: The Present](https://gracicot.github.io/reflection/2018/04/03/reflection-present.html)
StackOverflow — [Which type_traits cannot be implemented without compiler hooks?](https://stackoverflow.com/questions/20181702/which-type-traits-cannot-be-implemented-without-compiler-hooks)
Visual Studio Magazine — [C++ Introspection](https://visualstudiomagazine.com/articles/2013/06/03/c-introspection.aspx)