---
num: "Lecture 11"
desc: "Quadratic Sorting Algorithms cont., Mergesort"
ready: true
lecture_date: 2026-08-26 14:00:00.00-7:00
---

Recorded Lecture: [8_26_26](https://drive.google.com/file/d/14w80HSMFPRflWzJsWp9x0C7TpaXuM4E-/view?usp=drive_link)

# Insertion Sort

**Idea:** We want to work left-to-right and insert unsorted elements into the sorted left portion of the list
* For example, the first element is sorted by default.
    * Then we check the second element and determine if it goes before or after the first element
    * Then we check the third element and determine if it goes before, in the middle, or after the first two (sorted) elements
    * etc
* Note that in order to make "room" for the inserted element, we have to shift the elements greater than the inserted element up by one

**Analogy:** Similar to how one might sort a deck of cards
    * Work left-to-right, take a card and "insert" it into the correct position on the left portion of the sorted deck

```
def insertionSort(aList):
    for index in range(1,len(aList)):

        currentvalue = aList[index]
        position = index

        # shift elements over by one
        while position > 0 and aList[position-1] > currentvalue:
            aList[position] = aList[position-1]
            position = position-1

        # insert element in appropriate place
        aList[position] = currentvalue
```
```
def test_insertionSort():
    list1 = [1,2,3,4,5,6]
    list2 = [2,2,2,2,2,2]
    list3 = []
    list4 = [6,7,5,3,1]
    insertionSort(list1)
    assert list1 == [1,2,3,4,5,6]
    insertionSort(list2)
    assert list2 == [2,2,2,2,2,2]
    insertionSort(list3)
    assert list3 == []
    insertionSort(list4)
    assert list4 == [1,3,5,6,7]
```

## Insertion Sort Analysis

* When we check where to insert an element, we do up to 1 swap on the first iteration in order to make room for the element to be inserted in the sorted left portion of the list
    * Then we do up to two swaps on the second iteration
    * Then up to three swaps on the third iteration, etc.
    * So we get: 1 + 2 + ... + n-2 + n-1 swaps
    * **O(n<sup>2</sup>)**
* Let’s also look at the BEST case scenario (the list is already sorted)
    * Here, we still go through n elements, but no swaps occur because each position is in the correct place
    * So in the best case scenario, we have **O(n)**

# Divide and Conquer Algorithms

* Subdivide a larger problem into smaller problems
* Solve each smaller part
* Combine solutions of smaller sub problems back into the larger problem
* We’ve seen this pattern with recursion where the problem can be subdivided
* So far, we’ve talked about bubble sort, selection sort, and insertion sort
    * These algorithms generally run in O(n<sup>2</sup>)
* But better sorting algorithms exist!
    * We can improve our run time to **O(n log n)**

# Merge Sort

* **Idea:**
* Break a list into sub sublists where size == 1
    * A sublist with 1 element is considered sorted
* Merge each small sublist together to form sorted larger list
* Continue to merge sublists back into the original list

## Hand drawn example

![mergesortExample.png](mergesortExample.png)

## Merge Sort Implementation

```
def mergeSort(alist):
    if len(alist) > 1:
        mid = len(alist) // 2

        # uses additional space to create the left / right
        # halves
        lefthalf = alist[:mid]
        righthalf = alist[mid:]

        # recursively sort the lefthalf, then righthalf
        mergeSort(lefthalf)
        mergeSort(righthalf)

        # merge two sorted sublists (left / right halves)
        # into the original list (alist)
        i = 0 # index for lefthalf sublist
        j = 0 # index for righthalf sublist
        k = 0 # index for alist

        while i < len(lefthalf) and j < len(righthalf):
            if lefthalf[i] <= righthalf[j]:
                alist[k] = lefthalf[i]
                i = i + 1
            else:
                alist[k] = righthalf[j]
                j = j + 1
            k = k + 1

        # put the remaining lefthalf elements (if any) into
        # alist
        while i < len(lefthalf):
            alist[k] = lefthalf[i]
            i = i + 1
            k = k + 1

        # put the remaining righthalf elements (if any) into
        # alist
        while j < len(righthalf):
            alist[k] = righthalf[j]
            j = j + 1
            k = k + 1
```
```
# pytest
def test_mergeSort():
   numList1 = [9,8,7,6,5,4,3,2,1]
   numList2 = [1,2,3,4,5,6,7,8,9]
   numList3 = []
   numList4 = [1,9,2,8,3,7,4,6,5]
   numList5 = [5,4,6,3,7,2,8,1,9]
   mergeSort(numList1)
   mergeSort(numList2)
   mergeSort(numList3)
   mergeSort(numList4)
   mergeSort(numList5)

   assert numList1 == [1,2,3,4,5,6,7,8,9]
   assert numList2 == [1,2,3,4,5,6,7,8,9]
   assert numList3 == []
   assert numList4 == [1,2,3,4,5,6,7,8,9]
   assert numList5 == [1,2,3,4,5,6,7,8,9]
```

# Merge Sort Analysis

![mergesortAnalysis.png](mergesortAnalysis.png)

* Merge Sort breaks the list into two equal parts per recursive iteration
* Note that the height (number of levels) of the tree is **log n** (for 8 nodes, we have a height of 3).
* For each level, the merge step needs to compare `n` elements
    * So if there are **log n** levels and we need to do `n` comparisons to merge the elements for each level, then we have: **O(n log n)** running time
* One disadvantage of Merge Sort is it requires O(n) additional space when creating the left and right sublists
    * Can be problematic if the list we're trying to sort is extremely large
