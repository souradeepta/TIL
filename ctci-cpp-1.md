# CTCI C and C++

Chapter 13



- ```cpp
  #define NAME_SIZE 50 // Defines a macro
  ```

- Fields within the class can also be initialized as below if the member id is to be assigned before the actual object is created and before the remainder of the constructor code is called. Useful when have constant fields that can only be assigned a value once.

  ```cpp
  Person1(int a){
  	id = a;
  }
  
  Person(int a) : id(a){
  ...
  }
  ```

- Static binding for function resolution at compile time

- Declare a function as `virutal` for run-time binding.  Another usage is when we can't /don't implement a method for the parent class

- virtual function has only keyword but `pure-virtual functions` are different and do not have a body. By defining the below function as "pure virtual function" the class is now an abstract class and we cannot instantiate it.

  ```cpp
  virtual bool addCourse(string s) = 0;
  ```

- **Virtual destructor**: used to call the child class destructor first and then the parent class to be in order. Destructor in base class declared as virtual for avoiding dangling-pointers.

  ```cpp
  Person *p = new Student();
  delete p; // deletes Person first and leaves Student behind
  ```

  Hence add the keyword `virutal` to parent class destructor.

- Functions can specify default values and they must be on the right side of the function declaration.

- ```cpp
  // Operator overloading
  BookShefl Bookshelf::operator_(Packet &other) {...}
  ```

- Two pointers can equal each other, such that changing one's value also changes the other's value.

- Size of pointer depends on architectures. 32 bits on 32-bit arch.

- A `reference` is an alias for a pre-existing object and it does not have memory of its own. Unlike pointers they cannot be null or reassigned to another piece of memory.

  ```cpp
  int & b = a;
  ```

- incrementing pointer skips ahead bytes.

- Templates allow reuse.

  ```cpp
  template <class T>
  class ShiftedList {
  	T * ARRAY;
  	int offset, size;
  public:
  	ShiftedList( int sz) : offset(0), size(sz) {
  	array = new T[size];
  	}
  	T GetAt(int i){
  	return 0;
  	}
  	...
  }
  int main() {
  	ShiftedList<int > * list = new ShiftedList<int>[size];
  	...
  }
  ```

- hash table has an amortized O(1) lookup and insert assuming few collisions. Collisions can be resolved by `chaining` . It is unsorted whereas in STL map values are added in BST based on keys. No collisions can occur and since tree is balanced guaranteed insert and lookup of O(log N). Hash table implemented using an array of linked lists, where each node in the linked list holds value and the original key.
  - A good has function distributes the keys well
  - collisions are handled by chaining
  - methods to dynamically increase/decrease the hash table size 

- Cyclic array can be used as LRU cache

- Virtual function depends on a "vtable". "vtable" stores addresses of the  virtual functions of that class. If virtual function is not overridden in the derived class, vtable of the derived class stores address of the function in the parent class.  "vptr" variable in all such classes points to the "vtable" of that class. This allows ``Dynamic-binding`

- Deep-copy can be performed using `malloc` and `memcpy` to copy the pointer objects too.

- The keyword `volatile` informs the compiler that the value of variable can change from the outside b the OS, HW, or another thread and not the code,

  ```cpp
  int volatile x; //or
  volatile innt x;
  ```

- ``Smart-pointer` provides safety from dangling pointers, memory leaks and allocation failures using reference counter to the pointer. If reference counter to a pointer is 0 then then free the memory created by the integer and destroy the object. Make sure to override the "=" operator for assignment.

- 