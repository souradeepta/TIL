# C++11 Introduction



rvalue references

For **lvalue**, **rvalue** (temporaries or literals which were intended to never be modifiable in C++0x), and **references**, please visit [References for Built-in Types](https://www.bogotobogo.com/cplusplus/references.php#referencesforbuiltintypes).

The C++11 Standard introduces **rvalue references** which bind **only** to **rvalues** are declared with **two ampersands** rather than one:

```c++
int&& rvalue_ref = 99;
```

In C++0x, we needed **const** to assign an rvalue to a reference. However, this is a deliberate exception on the part of the standard, introduced before we had rvalue references in order to allow you to pass temporaries to functions taking references. This allows implicit conversions so that we can write code like this:

```c++
void print(std::string const& s);
print("implicit conversion");  // creates temporary std::string object
```

Let's go back to **rvalue** issue.
Note that we cannot bind **rvalue references** to **lvalues**. If we do as below,

```c++
int j = 99;
int&& k = j;       
```

we will get an error something like this:

```c++
Error: 'initializing' : cannot convert from 'int' to 'int &&'
                 You cannot bind an lvalue to an rvalue reference
```

An expression is an **rvalue** if it results in a temporary object as shown in the example below.

```c++
#include <iostream>

int getValue ()
{
    int ii = 10;
    return ii;
}

int main()
{
    std::cout << getValue();
    return 0;
}
```

The **getValue()** is an **rvalue**.
Note that the value being returned is not a reference to **ii**, but it's just a temporary value.

In C++0x, we could use a **normal** or **lvalue reference** to bind **temporary** object, but only if it was **const**:

```c++
const int& val = getValue(); // OK
int& val = getValue(); // NOT OK
```

In C++11, however, the **rvalue reference** let us bind a mutable reference to an **rvalue**, but not an **lvalue**. In other words, **rvalue** references are perfect for detecting if a value is temporary object or not.

```c++
const int&& val = getValue(); // OK
int&& val = getValue(); //  OK
```

Let's compare both references:

```c++
void printReference (const int& value)
{
        cout << value;
}
 
void printReference (int&& value)
{
        cout << value;
}
```

The printReference() function taking a **const lvalue** reference will accept any argument whether it be an **lvalue** or an **rvalue**. However, the second overloaded printReference*() taking an **rvalue** reference.



In other words, by using the **rvalue references**, we can use **function overloading** to determine whether function parameters are lvalues or rvalues by having one overloaded function taking an lvalue reference and another taking an rvalue reference. In other words, **C++11** introduces a new **non-const reference** type called an **rvalue reference**, identified by **T&&**. This refers to temporaries that are permitted to be modified after they are initialized, which is the cornerstone of **move semantics**.

```c++
#include <iostream>

using namespace std;

void printReference (const int& value)
{
        cout << "lvalue: value = " << value << endl;
}
 
void printReference (int&& value)
{
        cout << "rvalue: value = " << value << endl;
}

int getValue ()
{
    int temp_ii = 99;
    return temp_ii;
}

int main()
{ 
	int ii = 11;
	printReference(ii);
	printReference(getValue());
	return 0;
}
```

With the output:

```c++
lvalue: value = 11
rvalue: value = 99
```





Move semantics

Now we have a way (overloading functions - taking **const lvalue** or **rvalue**) to determine if a reference variable refers to a **temporary** object or to a **permanent** object. So, how it can be used?

The main usage of **rvalue** references is to create a **move constructor** and **move assignment operator**. A move constructor, like a copy constructor, takes an instance of an object as its argument and creates a new instance from original object. However, the move constructor can avoid memory reallocation because we know it has been provided a temporary object, so rather than copy the fields of the object, we will move them.

In other words, the **rvalue references** and **move semantics** allows us to avoid unnecessary copies when working with temporary objects. So, the resources needed for the temporary objects can be used for other objects.

The **rvalues** are typically temporary, and they can be modified. If we know that our function parameter is an **rvalue**, we can use it as a temporary storage, or take its contents without affecting the output of our program. This means that we can just **move** the contents rather than copy the contents of an **rvalues** parameter. This saves a lot of memory allocation and provides a lot of scope for optimization for large dynamic structures.

Let's look at the following two classes: **Array** class with regular copy constructor and **MovableArray** class using **rvalue** with **move constructor**.

```c++
#include <iostream>

using namespace std;

class Array
{
public:
	// constructor
	Array(int n): size(n), ptr(new int[n]) {}

	// copy constructor
	Array(const Array& arr): size(arr.size), ptr(new int[arr.size])
	{
		for (int i = 0; i < size; i++) 
			ptr[i] = arr.ptr[i];
	}

	// destructor
	~Array() { delete[] ptr; }

private:
	int *ptr;
	int size;
};

class MovableArray
{
public:

	// default constructor
	MovableArray(): size(128), ptr(new int[128]) 
	{ 
		cout << "MovableArray - default constructor\n"; 
	}

	// constructor
	MovableArray(int n): size(n), ptr(new int[n]())  
	{ 
		cout << "MovableArray - constructor\n"; 
	}

	// move constructor
	MovableArray(MovableArray&& arr)
		: size(arr.size), ptr(arr.ptr)
	{
		cout << "MovableArray - move constructor\n";
		arr.ptr = nullptr;	
	}

	// copy constructor
	MovableArray(MovableArray& arr)
		: size(arr.size), ptr(new int[arr.size])
	{
		cout << "MovableArray - copy constructor\n";
		for (int i = 0; i < size; i++) 
			ptr[i] = arr.ptr[i];
	}

	// destructor
	~MovableArray()
	{
		cout << "MovableArray - destructor\n";
		delete[] ptr;
	}

private:
	int *ptr;
	int size;
};

// The function func_temporary_array_object returns the object by value
// a temporary object is returned.
MovableArray func_temporary_array_object(int sz)
{
	MovableArray m(sz); // constructor
	return m;  // move constructor 
}   // desctructor when out of scopt

int main()
{ 
	MovableArray marray1(3);  // constructor
	MovableArray marray2(marray1);  //copy constructor
	MovableArray marray3(func_temporary_array_object(3));
	return 0;
}
```

With output:

```c++
MovableArray - constructor
MovableArray - copy constructor
MovableArray - constructor
MovableArray - move constructor
MovableArray - destructor
MovableArray - destructor
MovableArray - destructor
MovableArray - destructor
```

The copy constructor is doing both allocate memory and copy every value from the array one by one, and it may hit the performance of our code.

On the other hand, the move constructor is actually doing less work than the copy constructor. Note that the follwong two facts from the move constructor:

1. The parameter is a **non-const rvalue reference**
   If we had taken a **const rvalue reference**, we were not able to set **arr.ptr** to **nullptr**.
2. **arr.ptr** is set to **nullptr**
   Why do we have to set **arr.ptr = nullptr**?
   The reason is the destructor. When the temporary object goes out of scope, its destructor will be invoked, and it will free **arr.ptr**. The same **arr.ptr** that we just copied! If we don't set **arr.ptr** to **nullptr**, the move would not really be a move. It would just be a copy that introduces a crash later on once we start using freed memory. In other words, since the temporary will never be used again, no code will try to access the null pointer, and because the pointer is null, its memory is not deleted when it goes out of scope. Hence, the operation not only forgoes the expense of a constructor - deep copy, but is safe and invisible. This is the whole point of a move constructor: to avoid a copy by changing the original, temporary object.



As another illustration of **lvalue** vs **rvalue**, let's think about a function that takes a **vector<T>** as a parameter and needs to have an internal copy for modification, without touching the original. The old way (C++0X) of doing this would be to take the parameter as a **const lvalue reference** and make the copy internally:

```c++
#include <iostream>
#include <vector>

using namespace std;

vector<int> copy_both_rvalue_lvalue(vector<int> const&  v)
{
     std::vector<int> vec(v);

     return vec;
}

void copy_rvalue(std::vector<int> && vv)
{
     vv.push_back(5);
}
int main()
{ 
	vector<int> v(3, 555);
	vector<int> v_new = copy_both_rvalue_lvalue(v);
	return 0;
}
```

When **copy_both_rvalue_lvalue()** is called, it constructs a vector, **vec**. In our case, since we want to keep our original vector untouched, we need a second copy. But what happens at the return statement?

We have additional copies: one into a **temporary object** to be returned, and a second when the vector assignment operator runs on the line **v_new = copy_both_rvalue_lvalue(v);**. We may be able to get away with the first copy by the optimized compiler, but we cannot avoid the assignment to **v_new** will have to copy all the values again, which requires a new memory allocation and another iteration over the entire vector.

In the example above, the following line of code :

```c++
vector<int> copy_both_rvalue_lvalue(vector<int> const&  v)
```

allows the function to take both **lvalues** and **rvalues** by taking the parameter as a **const lvalue reference**. However, it forces the copy in every case. In other words, in C++0x, since there was no way to tell if an object was a temporary or not, we had to run the same code in the assignment operator or copy constructor, no matter where the value came from.



Fortunately, in C++11, as we discussed in previous section, if we overload the function with a version that takes an **rvalues**, we can avoid the copy in the **rvalues** case, because we know we can freely modify the original:

```c++
void copy_rvalue(vector<int> && v)
{
     v.push_back(10);
}
```

Now, if the function in question is the **constructor** of our class, we can steal the innards of the **rvalues** and use them for our new instance. Consider the class in the code below. In the default constructor a chunk of memory is allocated and in the destructor the allocated memory is freed as usual.

```c++
class Z
{
public:
    // default constructor
    Z(): data(new int[500]){}

    // destructor
    ~Z() { delete [] data; }

    // copy constructor
    Z(const Z& obj): data(new int[500])
    {
        std::copy(obj.data, obj.data+500, data);
    }

    // move constructor that takes the old value by rvalue reference 
    Z(Z&& obj): data(obj.data)
    {
        obj.data = nullptr;
    }

private:
    int* data;
};
```

The copy constructor is defined just as we expect: allocate a new block of memory and copy the data across. However, we also have a new constructor that takes the old value by **rvalue** reference. This is the **move constructor**. In this case we just copy the pointer to the data and leave the obj instance with a null pointer, saving ourselves a chunk of memory and time **when creating variables from rvalues**. Since the temporary will never be used again, no code will try to access the null pointer, and because the pointer is null, its memory is not deleted when it goes out of scope. Hence, the operation not only forgoes the expense of a [constructor - deep copy](https://www.bogotobogo.com/cplusplus/constructor.php#copy_constructor), but is safe and invisible.

The **rvalue** references can provide performance benefits to existing code without needing to make any changes outside the standard library. The type of the returned value of a function returning a **vector<T>** temporary does not need to be changed explicitly to **vector<T>** && to invoke the move constructor, as temporaries are considered rvalues automatically. (However, if **vector<T>** is a C++03 version without a move constructor, then the copy constructor will be invoked with a const **vector<T>&**, incurring a significant memory allocation.)

The move constructor of the class **Z** in the above example, is just an optimization. However, in some cases, it makes sense to provide a move constructor even when it doesn't make sense to provide a copy constructor. For example, the whole point of **std::unique_ptr<>** is that each non-null instance is the one and only pointer to its object, so a copy constructor makes no sense. However, a move constructor allows **ownership of the pointer to be transferred** between instances and permits **std::unique_ptr<>** to be used as a function return value-the pointer is moved rather than copied.

If we want to explicitly move from a named object that we know we'll no longer use, we can cast it to an rvalue either by using **static_cast<Z&&>** or by calling **std::move()** as in the code below:

```c++
Z z1;
Z z2=std::move(z1);
Z z3=static_cast<Z&&>(z2);
```

This can be beneficial when you wish to move the parameter value into a local or member variable without copying, because although an rvalue reference parameter can bind to rvalues, within the function itself it is treated as an lvalue:

```c++
void foo(Z&& z)
{
     // copy
     Z fa(z);  

     // move
     Z fb(std::move(z));               
}
```



Here is a complete code:

```c++
#include <iostream>

using namespace std;

class Z
{
public:
    // default constructor
    Z(): data(new int[500]){ cout << "default constructor\n";}

    // destructor
    ~Z() 
    { 
	 cout << "destructor\n";
	 delete [] data; 
    }

    // copy constructor
    Z(const Z& obj): data(new int[500])
    {
	cout << "copy constructor\n";
        std::copy(obj.data, obj.data+500, data);
    }

    // move constructor that takes the old value by rvalue reference 
    Z(Z&& obj): data(obj.data)
    {
	cout << "move constructor\n";
        obj.data = nullptr;
    }

private:
    int* data;
};

void foo(Z&& z)
{
	cout << "Z fa(z) - copy\n";
	Z fa(z);  
	cout << "Z fb(std::move(z)) - move\n";
	Z fb(std::move(z));               
}

int main ()
{
	cout << "main: foo(Z())\n";
	foo(Z());   
	cout << "main: Z z\n";
	Z z;
        // foo(z);

	return 0;
}
```

Output from the code:

```c++
main: foo(Z())
default constructor
Z fa(z) - copy
copy constructor
Z fb(std::move(z)) - move
move constructor
destructor
destructor
destructor
main: Z z
default constructor
destructor
```

If we remove the comment of the last line: **foo(z);**, we get the following error:

```c++
error: 'foo' : cannot convert parameter 1 from 'Z' to 'Z &&'
               You cannot bind an lvalue to an rvalue reference
```

**Move semantics** are extensively used in the **Thread Library** where:

1. copies make no semantic sense but resources can be transferred
2. and as an optimization to avoid expensive copies where the source is going to be destroyed anyway.



Function templates & rvalue references

When the function parameter is an **rvalue** reference to a template parameter, automatic template argument type deduction deduces the type to be an **lvalue** reference if an **lvalue** is supplied or a plain unadorned type if an **rvalue** is supplied. Let's look at the example below:

```c++
template<typename T>
void foo(T&& t)
{}
```

If we make a call it with an rvalue as follows, then **T** is deduced to be the type of the value:

```c++
foo(99);                // foo<int>(99)
foo(3.14);              // foo<double>(3.14)              
foo(std::string());     // foo<std::string>(std::string())
```

However, if we call foo with an **lvalue**, **T** is deduced to be an **lvalue** reference:

```c++
int i=99;
foo(i);                 // foo<int&>(i)
```

Because the function parameter is declared **T&&**, this is therefore a reference to a reference, which is treated as just the original reference type. The signature of **foo<int&>()** is thus:

```c++
void foo<int&>(int& t);
```

This allows a single function template to accept both **lvalue** and **rvalue** parameters and is used by the **std::thread** constructor so that the supplied callable object can be moved into internal storage rather than copied if the parameter is an **rvalue**.





Lambda Functuions

C++11 provides the ability to create anonymous functions, called lambda functions. It allows a function to be defined at the point where it's needed in another expression. It is a function that we can write inline in our code in order to pass in to another function.

A lambda expression defines a self-contained function that takes no parameters and relies only on global variables and functions. It doesn't even have to return a value. Such a lambda expression is a series of statements enclosed in braces, prefixed with [], called **lambda introducer** or **capture specification** which tells the compiler we're creating a lambda function, **[](){}**.

The most simplest form of a lambda function is the one takes no parameter list, though it does nothing:

```
[] {}
```

Let's look at lambda functions more useful:

```c++
#include <iostream>
#include <vector>
#include <algorithm>

void foo() { std::cout << "foo()\n"; }
void bar() { std::cout << "bar()\n"; }

int main ()
{
	// 1st lambda function
	auto f  = []() {
		foo();
		bar();
	};
	// the lambda function does something here
	f();

        // 2nd lambda function
	std::vector<int> v(5, 99);
	std::for_each(v.begin(), v.end(), [](int i){std::cout << i << "\n";});

	return 0;
}
```

The output from the run:

```c++
foo()
bar()
99
99
99
99
99
```

The first lambda expression is rather unusual because it has no parameters inside the parentheses. If we need to take parameters, we can do this by following the lambda introducer with a parameter list just like for a normal function as shown in the second example which writes all the elements of the vector.







C++ 11 - Pros and Cons

The followings are from [C++ style guide](http://google-styleguide.googlecode.com/svn/trunk/cppguide.xml#C++11).

C++11 is the latest ISO C++ standard. It contains significant changes both to the language and libraries. Use only approved libraries and language extensions from C++11 (formerly known as C++0x). Consider portability to other environments before using C++11 features in our project.

1. **Pros**
   C++11 has become the official standard, and eventually will be supported by most C++ compilers. It standardizes some common C++ extensions that we use already, allows shorthands for some operations, and has some performance and safety improvements.

2. **Cons**
   The C++11 standard is substantially more complex than its predecessor (1,300 pages versus 800 pages), and is unfamiliar to many developers. The long-term effects of some features on code readability and maintenance are unknown. We cannot predict when its various features will be implemented uniformly by tools that may be of interest (gcc, icc, clang, Eclipse, etc.).
   As with Boost, some C++11 extensions encourage coding practices that hamper readability - for example by removing checked redundancy (such as type names) that may be helpful to readers, or by encouraging **template metaprogramming**. Other extensions duplicate functionality available through existing mechanisms, which may lead to confusion and conversion costs.

3. Decision

   Use only C++11 libraries and language features that have been approved for use. Currently only the following C++11 features are approved:

   1. auto (for local variables only).
   2. Use of >> with no intervening space to close multiple levels of template arguments, as in **set<list<string> >**, where C++03 required a space as in **set<list<string> >**.
   3. Range-based for loops.
   4. Use of the **LL** and **ULL** suffixes on numeric literals to guarantee that their type is at least 64 bits wide.
   5. **Variadic macros** (but note that use of macros is discouraged).
   6. All of the new STL algorithms in the <algorithm> and <numeric> headers, except for the versions of min, max, and minmax whose signatures contain initializer lists.
   7. Use of local types as template parameters.
   8. **nullptr** and **nullptr_t**.