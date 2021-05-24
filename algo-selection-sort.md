# Selection Sort Algorithm

#### In this tutorial, you will learn how selection sort works. Also, you will find working examples of selection sort in C, C++, Java and Python.

Selection sort is an algorithm that selects the smallest element from an unsorted list in each iteration and places that element at the beginning of the unsorted list.

------

## How Selection Sort Works?

1. Set the first element as `minimum`

   .

   ![Selection Sort Steps](https://cdn.programiz.com/sites/tutorial2program/files/Selection-sort-0-initial-array.png)Select first element as minimum

2. Compare `minimum` with the second element. If the second element is smaller than `minimum`

   , assign the second element as` minimum`. Compare `minimum`with the third element. Again, if the third element is smaller, then assign `minimum` to the third element otherwise do nothing. The process goes on until the last element.

   ![Selection Sort Steps](https://cdn.programiz.com/sites/tutorial2program/files/Selection-sort-0-comparision.png)Compare minimum with the remaining elements

3. After each iteration, `minimum`  is placed in the front of the unsorted list.

   ![Selection Sort Steps](https://cdn.programiz.com/sites/tutorial2program/files/Selection-sort-0-swapping.png)Swap the first with minimum

4. For each iteration, indexing starts from the first unsorted element. Step 1 to 3 are repeated until all the elements are placed at their correct positions.

   ![Selection Sort Steps](https://cdn.programiz.com/sites/tutorial2program/files/Selection-sort-0.png)The first iteration

   ![Selection sort steps](https://cdn.programiz.com/sites/tutorial2program/files/Selection-sort-1.png)The second iteration

   ![Selection sort steps](https://cdn.programiz.com/sites/tutorial2program/files/Selection-sort-2.png)The third iteration

   ![Selection sort steps](https://cdn.programiz.com/sites/tutorial2program/files/Selection-sort-3_1.png)The fourth iteration

------

## Selection Sort Algorithm

```c++
selectionSort(array, size)
  repeat (size - 1) times
  set the first unsorted element as the minimum
  for each of the unsorted elements
    if element < currentMinimum
      set element as new minimum
  swap minimum with first unsorted position
end selectionSort
```

------

## Python, Java and C/C++ Examples

[Python](https://www.programiz.com/dsa/selection-sort#python-code)

[Java](https://www.programiz.com/dsa/selection-sort#java-code)

[C](https://www.programiz.com/dsa/selection-sort#c-code)

[C++](https://www.programiz.com/dsa/selection-sort#cpp-code)

```python
# Selection sort in Python


def selectionSort(array, size):
   
    for step in range(size):
        min_idx = step

        for i in range(step + 1, size):
         
            # to sort in descending order, change > to < in this line
            # select the minimum element in each loop
            if array[i] < array[min_idx]:
                min_idx = i
         
        # put min at the correct position
        (array[step], array[min_idx]) = (array[min_idx], array[step])


data = [-2, 45, 0, 11, -9]
size = len(data)
selectionSort(data, size)
print('Sorted Array in Ascending Order:')
print(data)
```

------

## Complexity

| Cycle | Number of Comparison |
| :---- | :------------------- |
| 1st   | (n-1)                |
| 2nd   | (n-2)                |
| 3rd   | (n-3)                |
| ...   | ...                  |
| last  | 1                    |

Number of comparisons: `(n - 1) + (n - 2) + (n - 3) + ..... + 1 = n(n - 1) /2` nearly equals to `n2`.

**Complexity** = `O(n2)`

Also, we can analyze the complexity by simply observing the number of loops. There are 2 loops so the complexity is `n*n = n2`.

**Time Complexities:**

- **Worst Case Complexity:** `O(n2)`
  If we want to sort in ascending order and the array is in descending order then, the worst case occurs.
- **Best Case Complexity:** `O(n2)`
  It occurs when the array is already sorted
- **Average Case Complexity:** `O(n2)`
  It occurs when the elements of the array are in jumbled order (neither ascending nor descending).



The time complexity of the selection sort is the same in all cases. At every step, you have to find the minimum element and put it in the right place. The minimum element is not known until the end of the array is not reached.

**Space Complexity:**

Space complexity is `O(1)` because an extra variable `temp` is used.

------

## Selection Sort Applications

The selection sort is used when:

- a small list is to be sorted
- cost of swapping does not matter
- checking of all the elements is compulsory
- cost of writing to a memory matters like in flash memory (number of writes/swaps is `O(n)` as compared to `O(n2)` of bubble sort)



As many of the others have said, it all comes down to how many moves are made. In Selection sort, a maximum of n moves are made, whereas in Bubble Sort, up to n moves are made for each element, so up to n^2 total moves are made. It’s these moves that are memory-intensive so Selection sort becomes even more efficient than Bubble sort the larger the list is.

It’s also important to note, as others have mentioned, that with small list, the difference in time these two methods will take is negligible. With larger lists, there are other sorting methods that far out-perform both of these methods, so these methods aren’t practical for large-scale use.