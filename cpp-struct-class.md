# [Struct Constructor in C++?](https://stackoverflow.com/questions/1127396/struct-constructor-in-c)



In C++ the only difference between a `class` and a `struct` is that members and base classes are private by default in classes, whereas they are public by default in structs.

So structs can have constructors, and the syntax is the same as for classes.

If you declare your struct like this:

```cpp
typedef struct{
int x;
foo(){};
} foo;
```

You will have problems trying to declare a constructor. This is of course because you haven't actually declared a struct named "foo", you've created an anonymous struct and assigned it the alias "foo". This also means you will not be able to use "foo" with a scoping operator in a cpp file:

foo.h:

```cpp
typedef struct{
int x;
void myFunc(int y);
} foo;
```

foo.cpp:

```cpp
//<-- This will not work because the struct "foo" was never declared.
void foo::myFunc(int y)
{
  //do something...
}
```

To fix this, you must either do this:

```cpp
struct foo{
int x;
foo(){};
};
```

or this:

```cpp
typedef struct foo{
int x;
foo(){};
} foo;
```

Where the latter creates a struct called "foo" and gives it the alias "foo" so you don't have to use the `struct` keyword when referencing it.



--------

a struct is basically treated as a class in C++. This allows you to have a constructor which can be used to initialise the struct with default values. Below, the constructor takes `sz` and `b` as arguments, and initializes the other variables to some default values.

```cpp
struct blocknode
{
    unsigned int bsize;
    bool free;
    unsigned char *bptr;
    blocknode *next;
    blocknode *prev;

    blocknode(unsigned int sz, unsigned char *b, bool f = true,
              blocknode *p = 0, blocknode *n = 0) :
              bsize(sz), free(f), bptr(b), prev(p), next(n) {}
};
```

Usage:

```cpp
unsigned char *bptr = new unsigned char[1024];
blocknode *fblock = new blocknode(1024, btpr);
```

------------------

Class, Structure and Union is described in below table in short.

[![enter image description here](https://i.stack.imgur.com/h8YRL.png)](https://i.stack.imgur.com/h8YRL.png)

-----------------

example but using **this** keyword when setting value in constructor:

```cpp
#include <iostream>

using namespace std;

struct Node {
    int value;

    Node(int value) {
        this->value = value;
    }

    void print()
    {
        cout << this->value << endl;
    }
};

int main() {
    Node n = Node(10);
    n.print();

    return 0;
}
```