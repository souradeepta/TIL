# An Introduction to Boost

What Does Boost Provide?

What follows is a minimal listing of components. There are about 50 major sub-components in Boost at the moment. The following components were those that I felt logically progressed from the components in the STL, were easy to integrate, or were especially significant to most programmers.



## Smart Pointers

Smart pointers are tools that prevent resource leaks [(especially in the presence of exceptions)](http://www.cuj.com/documents/s=8191/cujcexp2012sutter2/), promote the concept of 'Initialization is Resource Acquisition'. They emulate, to a certain extent, garbage collection like behavior.

Most of the limitation of `std::auto_ptr` are relatively well known:

- `std::auto_ptr`'s cannot be stored within a standard containers.
- `std::auto_ptr`'s cannot (easily) be used to implement the pImpl (pointer-to-implementation) idiom.
- `std::auto_ptr `does not work with arrays.

The 5 types of Boost smart pointers overcome these flaws and provide many extra features.

- Custom delete functions can be supplied.
- Detection of incomplete template types.

[Comparison of Boost and Loki Smart Pointers](http://www.cuj.com/documents/s=7980/cujcexp2008sutter/) The New C++: Smart(er) Pointers - Herb Sutter
[Introduction to uses of the Boost smart pointers](http://www.cuj.com/documents/s=7981/cujcexp2007hyslop/) Conversations: Getting to the Point - Herb Sutter and Jim Hyslop



## Composers

Functors and binders have become a common part of using the STL, but using most standard library implementations it can still be difficult to combine multiple functions. Composers allow functors to be combined in several ways, minimizing the amount of times that users have to write their own loops.

[Boost::compose index](http://www.boost.org/libs/compose/compose.html)
The C++ Standard Library - A Tutorial and Reference (Nicolai M. Josuttis)

### Any

A component which provides a type safe way to move any type of component, without having to rely upon void pointers or unions. The design principles for this component is at least as significant as the component itself (derivation of a template class from a non template base class). Something similar to `boost::any `appears in Alexandrescu’s Modern C++ Design in the guise of Functors and Functor Implementations.

[boost::any main page](http://www.boost.org/doc/html/any.html)
[boost::any Theory](http://www.two-sdg.demon.co.uk/curbralan/papers/ValuedConversions.pdf)
[Introduction to uses of Boost Any](http://www.cuj.com/documents/s=7988/cujcexp1912hyslop/) Conversations: I'd Hold Anything for You - Herb SutterBind and Function

Bind and Function are specified as two separate components, but they are extensions (any number of arguments) of binders and functors concept which are currently in place in the Standard Library.

[boost::function main page](http://www.boost.org/doc/html/function.html)
[boost::bind main page](http://www.boost.org/libs/bind/bind.html)

The Lambda Library

The lambda library provides a shortcut for producing binders, functors and composers using expression templates. My personal opinion on the library is that developers would need some practice to recognise it’s use. Libraries like the Lambda library are probably the way of the future for C++, but at the moment, I think I’m prepared to have slower uglier code that I know the next guy can understand.

[The Boost Lambda Library Index](http://www.boost.org/libs/lambda/doc/index.html)
Further information on the basics of expression templates was published in the March issue of the C/C++ users journal (C++ Expression Templates – Angelika Langer and Klaus Kreft)The Boost Graph Library (The BGL)

The BGL is a huge library, with a large amount of support material and good sample programs. “The Boost Graph Library, The: User Guide and Reference Manual” has been published by Addison-Wesley in the C++ In Depth Series (The same fantastic series that includes 'Exceptional C++', 'More Exceptional C++' and 'Modern C++ Design'), which I believe is testament to the quality of the library.

[Boost Graph Library Table of Contents](http://www.boost.org/libs/graph/doc/table_of_contents.html)
[The Boost Graph Library, The: User Guide and Reference Manual](http://www.awprofessional.com/catalog/product.asp?product_id={B7B10FCE-2354-4A3E-8792-FCFCECDB355D}) Book ReviewThread

Developed by Code Project regular William E. Kempf, the threads library makes it seem almost as easy to do threads in C++ as it is in Java. It requires linking to an additional library, built with BoostJam.

[boost::threads index](http://www.boost.org/libs/thread/doc/index.html)
[William E. Kempf on boost::thread](http://www.cuj.com/documents/s=8470/cuj0205kempf/)Spirit parser generator framework

Jonathan de Halleux has written an excellent introduction to the [Spirit Parser Generator Framework](http://www.codeproject.com/cpp/spiritintro.asp?target=spirit) with comprehensive links to relevant material

What Else?

- Regular Expressions
- Traits
- File System (Directory Iteration)
- Iterator Adaptors
- Maths and Matrices
- A Template metaprogramming framework
- Tuples
- Python