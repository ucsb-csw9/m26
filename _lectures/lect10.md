---
num: "Lecture 10"
desc: "Linked Lists cont., Quadratic Sorting Algorithms"
ready: true
lecture_date: 2026-08-25 14:00:00.00-7:00
---

Recorded Lecture: [8_25_26](https://drive.google.com/file/d/1txvYQxKhJaQxuW_FpEyjqBb6tcSJ-F6Y/view?usp=drive_link)

# Ordered Linked Lists
* We’ve discussed Unordered Linked Lists where the position of the nodes did not matter with respect to each other
* An Ordered Linked List is similar to an Unordered Linked List except (as you might guess) the nodes in the list are ordered with respect to each other
* The implementation of both are similar, except we have to maintain the ordered property of the nodes when we manage the list
	* Most methods can be the same, but adding the node requires us to put a node in the correct position (instead of simply adding to the front of the list)
	* Consider two cases:
		1. Adding to the front of the Linked List
		2. Adding to the middle / end of the Linked List

* Let’s implement the `add` method to see what inserting an item in the middle of the list would look like
	* Note: I did this in the `LinkedList` class, but it would be better to implement this in an `OrderedLinkedList` class. I did this because I'll only test the `add` method with sorted elements in the Linked List.

```
# add method to maintain an Ordered Linked List
def add(self, item):
	current = self.head
	previous = None
	stop = False

	# find the correct place in the list to add
	while current != None and not stop:
		if current.getData() > item:
			stop = True
		else:
			previous = current
			current = current.getNext()

	# create Node with item to add
	temp = Node(item)

	# Case 1: insert at the front of the list
	if previous == None:
		temp.setNext(self.head)
		self.head = temp
	else: # Case 2: insert not at front of list
		temp.setNext(current)
		previous.setNext(temp)

# Method to get the items from front to back
def getList(self):
	current = self.head
	output = ""
	while current != None:
		output += str(current.getData()) + " "
		current = current.getNext()
	output = output[:len(output)-1] # remove end space
	return output
```
```
# Test to check if adding elements maintain order
def test_insertIntoOrderedList():
	ll = LinkedList()
	ll.add(35)
	ll.add(50)
	ll.add(10)
	ll.add(40)
	ll.add(20)
	ll.add(30)
	assert ll.getList() == \
		"10 20 30 35 40 50"
	ll.add(5)
	assert ll.getList() == \
		"5 10 20 30 35 40 50"
	ll.add(60)
	assert ll.getList() == \
		"5 10 20 30 35 40 50 60"
```

# Quadratic Sorting Algorithms

* So far, we’ve discussed some ways of searching through a list
	* Linear search - start at the beginning and check every element in the list
		* Does not require elements to be sorted
		* O(n) complexity
	* Binary search - check the middle, then check left or right sub lists
		* Does require elements to be sorted
		* O(log n) complexity
* But we haven’t discussed HOW to sort an unordered list
* There are many ways to do this, and some approaches are better than others (for example, Bogosort is pretty bad! <https://en.wikipedia.org/wiki/Bogosort>). We’ll discuss several sorting algorithms and analyze their performance
* Quadratic Sorting Algorithms are done in O(n<sup>2</sup>)

## Bubble Sort
<b>Idea:</b> Bubble sort will make several passes through the list and swap adjacent elements to ensure the largest element ends up at the end of the list (assuming we’re sorting in ascending order)

```
def bubbleSort(aList):
	for passnum in range(len(aList)-1,0,-1):
		for i in range(passnum):
			if aList[i] > aList[i+1]:
				# swap (bubble up!)
				temp = aList[i]
				aList[i] = aList[i+1]
				aList[i+1] = temp
```
```
# Bubble sort pytest
def test_bubbleSort():
	list1 = [1,2,3,4,5,6]
	list2 = [2,2,2,2,2,2]
	list3 = []
	list4 = [6,7,5,3,1]
	bubbleSort(list1)
	assert list1 == [1,2,3,4,5,6]
	bubbleSort(list2)
	assert list2 == [2,2,2,2,2,2]
	bubbleSort(list3)
	assert list3 == []
	bubbleSort(list4)
	assert list4 == [1,3,5,6,7]
```

## Bubble Sort Analysis

* Notice that we have to make at most n-1 comparisons during the first iteration through the list
	* Then n-2 comparisons during the 2nd iteration, etc.
* So if count the number of comparisons in this algorithm, we have
	* 1 + 2 + 3 + ... + (n-2) + (n-1)
* This summation can be simplified to n(n+1)/2
	* We can deduce that this algorithm is **O(n<sup>2</sup>)**
* Also note that in the BEST case scenario, the list is already sorted
	* Each iteration does nothing, but we still compare the values anyways, thus performing unnecessary work
	* In order to improve situations where the list becomes sorted during the swaps, we can check if a swap occurred during an iteration
		* If yes, continue algorithm
		* If no, then the list is sorted, so stop the algorithm!
		* An implementation of this optimization is shown in the textbook

# Selection Sort

**Idea:** Similar to Bubble Sort, we make passes through the list and find the largest element. We then swap the largest element in the correct place (each iteration will place the largest element at the end of the list assuming we’re sorting in ascending order)
* It’s not necessary to swap adjacent elements in like Bubble Sort
* Think of it as "selecting" the largest element and then placing it in the correct place

```
def selectionSort(aList):
	for fillslot in range(len(aList)-1,0,-1):
		positionOfMax=0
		for location in range(1,fillslot+1):
			if aList[location]>aList[positionOfMax]:
				positionOfMax = location

		# swap largest element in correct place
		temp = aList[fillslot]
		aList[fillslot] = aList[positionOfMax]
		aList[positionOfMax] = temp
```
```
def test_selectionSort():
	list1 = [1,2,3,4,5,6]
	list2 = [2,2,2,2,2,2]
	list3 = []
	list4 = [6,7,5,3,1]
	selectionSort(list1)
	assert list1 == [1,2,3,4,5,6]
	selectionSort(list2)
	assert list2 == [2,2,2,2,2,2]
	selectionSort(list3)
	assert list3 == []
	selectionSort(list4)
	assert list4 == [1,3,5,6,7]
```

## Selection Sort Analysis

* Similar to Bubble Sort, we have to make n-1 comparisons during the first iteration through the list
	* Then n-2 comparisons during the 2nd iteration, etc.
* If we count the number of comparisons in this algorithm, we have
	* n-1 + n-2 + ... + 2 + 1 = n(n+1)/2
	* **O(n<sup>2</sup>)**
* Note: we only do one swap operation per iteration unlike Bubble Sort
