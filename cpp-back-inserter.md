# std::back_inserter in C++

**std::back_inserter** constructs a back-insert iterator that inserts new elements at the end of the container to which it is applied. It is defined inside the header file **.**

A back-insert iterator is a special type of **output iterator** designed to allow algorithms that usually overwrite elements (such as copy) to instead insert new elements automatically at the end of the container.

**Syntax:**

```
std::back_inserter (Container& x);

x: Container in which new elements will 
be inserted at the end.

Returns: A back_insert_iterator that inserts 
elements at the end of container x.
```



```c++
// C++ program to demonstrate std::back_inserter 
#include <iostream> 
#include <iterator> 
#include <vector> 
#include <algorithm> 

using namespace std; 
int main() 
{ 
    // Declaring first container 
    vector<int> v1 = { 1, 2, 3 }; 
  
    // Declaring second container for 
    // copying values 
    vector<int> v2 = { 4, 5, 6 }; 
  
    // Using std::back_inserter inside std::copy 
    std::copy(v1.begin(), v1.end(), std::back_inserter(v2)); 
    // v2 now contains 4 5 6 1 2 3 
  
    // Displaying v1 and v2 
    cout << "v1 = "; 
  
    int i; 
    for (i = 0; i < 3; ++i) { 
        cout << v1[i] << " "; 
    } 
  
    cout << "\nv2 = "; 
    for (i = 0; i < 6; ++i) { 
        cout << v2[i] << " "; 
    } 
  
    return 0; 
} 
```

Output:

```
v1 = 1 2 3
v2 = 4 5 6 1 2 3
```

- No prior Knowledge of size of container required : 

  One of the scenario where such a function can be extremely helpful is when we don’t know the size of the container, i.e., how many elements will be inserted into it, so one way is to make that container of extremely large size, but the most efficient way will be to use `std::back_inserter()` in such a case, without declaring the size of the container.

  

  ```c++
  // C++ program to demonstrate std::back_inserter 
  #include <iostream> 
  #include <iterator> 
  #include <vector> 
  #include <algorithm> 
  using namespace std; 
  int main() 
  { 
      // Declaring first container 
      vector<int> v1 = { 1, 2, 3 }; 
    
      // Declaring second container without specifying 
      // its size 
      vector<int> v2; 
    
      // Using std::back_inserter inside std::copy 
      std::copy(v1.begin(), v1.end(), std::back_inserter(v2)); 
      // v2 now contains 1 2 3 
    
      // Displaying v1 and v2 
      cout << "v1 = "; 
    
      int i; 
      for (i = 0; i < 3; ++i) { 
          cout << v1[i] << " "; 
      } 
    
      cout << "\nv2 = "; 
      for (i = 0; i < 3; ++i) { 
          cout << v2[i] << " "; 
      } 
    
      return 0; 
  } 
  ```

  Output:

  ```
  v1 = 1 2 3
  v2 = 1 2 3
  ```

  **Explanation:** Here, we had to copy v1 into v2, but let’s say we are in some scenario where we do not know how many elements will be copied into v2, so we will not specify its size and later on copy into it using std::back_inserter().

  **Why not v2.begin() in place of back_inserter()??** Many of you will be thinking that why haven’t we used v2.begin() in place of std::back_inserter(v2), then you need to rethink that since we have not declared the size of v2, therefore, there are no elements in it, and so there will be no beginning and therefore, v2.begin() will throw an error here.

**Points to Remember:**

1. One of the pitfalls of std::back_inserter is that it can be **used with only those containers that have push_back as one of its methods** like in case of vector, list and deque.

2. **push_back() vs back_inserter():** Now, you may be thinking that push_back() and back_inserter are similar, but they are not. When you have to pass an iterator in the algorithm, then you should use back_inserter like in above case, while for normally inserting the values at the end of the container, push_back() can be used.

3. In place of using `std::back_inserter`, we can create a `back_insert_iterator` and then use it as eventually, `std::back_inserter` returns a `back_insert_iterator` only.

   

   ```c++
   // C++ program to demonstrate back_insert_iterator 
   #include <iostream> 
   #include <iterator> 
   #include <vector> 
   #include <algorithm> 
   using namespace std; 
   int main() 
   { 
       // Declaring first container 
       vector<int> v1 = { 1, 2, 3 }; 
     
       // Declaring second container for 
       // copying values 
       vector<int> v2 = { 4, 5, 6 }; 
     
       // Declaring a back_insert_iterator 
       std::back_insert_iterator<std::vector<int> > back_i1(v2); 
     
       // Using the iterator in the copy() 
       std::copy(v1.begin(), v1.end(), back_i1); 
       // v2 now contains 4 5 6 1 2 3 
     
       // Displaying v1 and v2 
       cout << "v1 = "; 
     
       int i; 
       for (i = 0; i < 3; ++i) { 
           cout << v1[i] << " "; 
       } 
     
       cout << "\nv2 = "; 
       for (i = 0; i < 6; ++i) { 
           cout << v2[i] << " "; 
       } 
     
       return 0; 
   } 
   ```

   Output:

   ```
   v1 = 1 2 3
   v2 = 4 5 6 1 2 3
   ```