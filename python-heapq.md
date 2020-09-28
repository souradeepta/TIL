# The Python heapq Module: Using Heaps and Priority Queues

## 

## What Are Heaps?

Heaps are **concrete** data structures, whereas priority queues are **abstract** data structures. An abstract data structure determines the [interface](https://realpython.com/python-interface/), while a concrete data structure defines the implementation.

Heaps are commonly used to implement priority queues. They’re the most popular concrete data structure for implementing the priority queue abstract data structure.

Concrete data structures also specify **performance guarantees**. Performance guarantees define the relationship between the *size* of the structure and the *time* operations take. Understanding those guarantees allows you to predict how much time the program will take as the size of its inputs change.

### Data Structures, Heaps, and Priority Queues

Abstract data structures specify operations and the relationships between them. The priority queue abstract data structure, for example, supports three operations:

1. **is_empty** checks whether the queue is empty.
2. **add_element** adds an element to the queue.
3. **pop_element** pops the element with the highest priority.

Priority queues are commonly used for optimizing task execution, in which the goal is to work on the task with the highest priority. After a task is completed, its priority is lowered, and it’s returned to the queue.

There are two different conventions for determining the priority of an element:

1. The *largest* element has the highest priority.
2. The *smallest* element has the highest priority.

These two conventions are equivalent because you can always reverse the effective order. For example, if your elements consist of [numbers](https://realpython.com/python-numbers/), then using negative numbers will flip the conventions around.

The Python `heapq` module uses the second convention, which is generally the more common of the two. Under this convention, the *smallest* element has the highest priority. This might sound surprising, but it’s often quite useful. In the real-life examples you’ll see later, this convention will simplify your code.

**Note:** The Python `heapq` module, and the heap data structure in general, is *not* designed to allow finding any element except the smallest one. For retrieval of any element by size, a better option is a [binary search tree](https://realpython.com/binary-search-python/).

Concrete data structures implement the operations defined in an abstract data structure and further specify performance guarantees.

The heap implementation of the priority queue guarantees that both pushing (adding) and popping (removing) elements are **logarithmic time** operations. This means that the time it takes to do push and pop is proportional to the **base-2 logarithm** of the number of elements.

### Implementation of Heaps

A heap implements a priority queue as a **complete binary tree**. In a [binary tree](https://en.wikipedia.org/wiki/Binary_tree), each node will have at most two children. In a *complete* binary tree, all levels except possibly the deepest one are full at all times. If the deepest level is incomplete, then it will have the nodes as far to the left as possible.

The **completeness property** means that the depth of the tree is the base-2 logarithm of the number of elements, rounded up. Here’s an example of a complete binary tree:

[![Complete Binary Tree Satisfying the Heap Property](https://files.realpython.com/media/heap-tree.4b4413ff133c.png)](https://files.realpython.com/media/heap-tree.4b4413ff133c.png)

In this particular example, all levels are complete. Each node except for the deepest ones has exactly two children. There are a total of seven nodes in three levels. Three is the base-2 logarithm of seven, rounded up.

The single node at the base level is called the **root** node. It might seem weird to call the node at the top of the tree the root, but this is the common convention in programming and computer science.

The performance guarantees in a heap depend on how elements percolate up and down the tree. The practical result of this is that the number of comparisons in a heap is the base-2 logarithm of the size of the tree.

In a heap tree, the value in a node is always smaller than both of its children. This is called the **heap property**. This is different from a [binary search tree](https://en.wikipedia.org/wiki/Binary_search_tree), in which only the left node will be smaller than the value of its parent.

The algorithms for both pushing and popping rely on temporarily violating the heap property, then fixing the heap property through comparisons and replacements up or down a single branch.

For example, to push an element onto a heap, Python adds the new node to the next open slot. If the bottom layer isn’t full, then the node is added to the next open slot at the bottom. Otherwise, a new level is created and then the element is added to the new bottom layer.

Once the node is added, Python compares it to its parent. If the heap property is violated, then the node and its parent are switched, and the check begins again at the parent. This continues until the heap property holds or the root has been reached.

Similarly, when popping the smallest element, Python knows that, because of the heap property, the element is at the root of the tree. It replaces the element with the last element at the deepest layer and then checks if the heap property is violated down the branch.

### Uses of Priority Queues

A priority queue, and a heap as an implementation of a priority queue, is useful for programs that involve finding an element that is extreme in some way. For example, you can use a priority queue for any of the following tasks:

- Getting the three most popular blog posts from hit data
- Finding the fastest way to get from one point to the other
- Predicting which bus will be the first to arrive at a station based on arrival frequency

Another task for which you could use a priority queue is scheduling emails. Imagine a system that has several kinds of emails, each of which needs to be sent at a certain frequency. One kind of email needs to go out every fifteen minutes, and another needs to be sent every forty minutes.

A scheduler could add both types of email to the queue with a **timestamp** indicating when the email next needs to be sent. Then the scheduler could look at the element with the smallest timestamp—indicating that it’s next in line to be sent—and calculate how long to sleep before sending.

When the scheduler wakes up, it would process the relevant email, take the email out of the priority queue, calculate the next timestamp, and put the email back in the queue at the correct location.

## Heaps as Lists in the Python `heapq` Module

Although you saw the heap described earlier as a tree, it’s important to remember that it’s a *complete* binary tree. Completeness means that it’s always possible to tell how many elements are at each layer except the last one. Because of this, heaps can be implemented as a [list](https://realpython.com/courses/lists-tuples-python/). This is what the Python `heapq` module does.

There are three rules that determine the relationship between the element at the index `k` and its surrounding elements:

1. Its first child is at `2*k + 1`.
2. Its second child is at `2*k + 2`.
3. Its parent is at `(k - 1) // 2`.

The rules above tell you how to visualize a list as a complete binary tree. Keep in mind that an element always has a parent, but some elements don’t have children. If `2*k` is beyond the end of the list, then the element doesn’t have any children. If `2*k + 1` is a valid index but `2*k + 2` is not, then the element has only one child.

The heap property means that if `h` is a heap, then the following will never be `False`:

```
h[k] <= h[2*k + 1] and h[k] <= h[2*k + 2]
```



It might raise an `IndexError` if any of the indices is beyond the length of the list, but it will never be `False`.

In other words, an element must always be smaller than the elements that are at twice its index plus one and twice its index plus two.

Here’s a visual of a list that satisfies the heap property:

[![Heap Implemented as a List](https://files.realpython.com/media/heap-list.2db422e53ead.png)](https://files.realpython.com/media/heap-list.2db422e53ead.png)

The arrows go from element `k` to elements `2*k + 1` and `2*k + 2`. For example, the first element in a Python list has the index `0`, so its two arrows point at indices `1` and `2`. Notice how the arrows always go from a smaller value to a bigger value. This is how you can check that the list satisfies the heap property.

### Basic Operations

The Python `heapq` module implements heap operations on lists. Unlike many other modules, it does *not* define a custom class. The Python `heapq` module has functions that work on lists directly.

Usually, as in the email example above, elements will be inserted into a heap one by one, starting with an empty heap. However, if there’s already a list of elements that needs to be a heap, then the Python `heapq` module includes `heapify()` for turning a list into a valid heap.

The following code uses `heapify()` to turn `a` into a **heap**.

The Python `heapq` module implements heap operations on lists. Unlike many other modules, it does *not* define a custom class. The Python `heapq` module has functions that work on lists directly.

Usually, as in the email example above, elements will be inserted into a heap one by one, starting with an empty heap. However, if there’s already a list of elements that needs to be a heap, then the Python `heapq` module includes `heapify()` for turning a list into a valid heap.

The following code uses `heapify()` to turn `a` into a **heap**.

You can check that even though `7` comes after `8`, the list `a` still obeys the heap property. For example, `a[2]`, which is `3`, is less than `a[2*2 + 2]`, which is `7`.

As you can see, `heapify()` modifies the list in place but doesn’t sort it. A heap doesn’t have to be sorted to satisfy the heap property. However, since every sorted list *does* satisfy the heap property, running `heapify()` on a sorted list won’t change the order of elements in the list.

The other basic operations in the Python `heapq` module assume that the list is already a heap. It’s useful to note that an empty list or a list of length one will always be a heap.

Since the root of the tree is the first element, you don’t need a dedicated function to read the smallest element nondestructively. The first element, `a[0]`, will always be the smallest element.

To pop the smallest element while preserving the heap property, the Python `heapq` module defines `heappop()`.

Here’s how to use `heappop()` to pop an element:

\>>>

```python
>>> import heapq
>>> a = [1, 2, 3, 5, 6, 8, 7]
>>> heapq.heappop(a)
1
>>> a
[2, 5, 3, 7, 6, 8]
```



The function returns the first element, `1`, and preserves the heap property on `a`. For example, `a[1]` is `5` and `a[1*2 + 2]` is `6`.

