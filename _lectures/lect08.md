---
num: "Lecture 8"
desc: "Queues, Deques, Linked Lists"
ready: true
lecture_date: 2026-08-19 14:00:00.00-7:00
---

Recorded Lecture: [8_19_26](https://drive.google.com/file/d/1igIIXSh2a8rJYWdvlCqUvZoB-Qjn95i7/view?usp=drive_link)

# Queue

* A Queue is linear data structure that has the First In, First Out (FIFO) property
* Analogy: Think about standing in line (no cutting!)
* Similar to a Stack, we can implement a Queue data structure using a Python List

![Queue.png](Queue.png)

```
# pytests
def test_insertIntoQueue():
    q = Queue()
    assert q.isEmpty() == True
    assert q.size() == 0
    q.enqueue("Customer 1")
    q.enqueue("Customer 2")
    assert q.isEmpty() == False
    assert q.size() == 2
    
def test_removeFromQueue():
    q = Queue()
    q.enqueue("Customer 1")
    q.enqueue("Customer 2")
    assert q.dequeue() == "Customer 1"
    assert q.isEmpty() == False
    assert q.size() == 1
    assert q.dequeue() == "Customer 2"
    assert q.isEmpty() == True
    assert q.size() == 0
```
```
class Queue:
    def __init__(self):
        self.items = []

    def isEmpty(self):
        return self.items == []

    def enqueue(self, item):
        self.items.insert(0, item)

    def dequeue(self):
        return self.items.pop()

    def size(self):
        return len(self.items)
```

## Big-O analysis

* enqueue() : O(n)
* dequeue() : O(1)

# Deque

* Pronounced "Deck"
* A Deque is a linear data structure that is more flexible than a stack and a queue
    * A deque is also known as a "double-ended queue"
    * A deque allows us to insert and remove from both ends
        * Unlike a stack where we can only insert and remove from one end (top)
        * And a queue where we can insert in the front and remove from the end of a list
* Similar to a Stack and a Queue, we can implement Deques with a Python List

![Deque.png](Deque.png)

```
# pytests
def test_Deque():
    d = Deque()
    assert d.isEmpty() == True
    assert d.size() == 0
    d.addFront(10)
    d.addFront(20)
    d.addRear(30)
    d.addRear(40)
    assert d.isEmpty() == False
    assert d.size() == 4
    assert d.removeFront() == 20
    assert d.removeRear() == 40
    assert d.isEmpty() == False
    assert d.size() == 2
    assert d.removeRear() == 30
    assert d.removeRear() == 10
    assert d.isEmpty() == True
    assert d.size() == 0
```
```
class Deque:
    def __init__(self):
        self.items = []

    def isEmpty(self):
        return self.items == []

    def addFront(self, item):
        self.items.append(item)

    def addRear(self, item):
        self.items.insert(0, item)

    def removeFront(self):
        return self.items.pop()

    def removeRear(self):
        return self.items.pop(0)

    def size(self):
        return len(self.items)
```

## Big-O analysis

* addFront()    : O(1)
* addRear()     : O(n)
* removeFront() : O(1)
* removeRear()  : O(n)

# Linked Lists

* Python lists are just one way to implement a List type structure
* The underlying structure of a Python List stores information in contiguous memory
    * This is why certain operations like inserting into index 0 requires the shifting of elements to make room
* There is another way to implement a List type structure that performs better in certain operations (and worse in others)
    * This way doesn’t organize data in contiguous memory, so maintaining the list structure doesn’t need to shift elements around
* **Linked Lists** are List collection structures that are not stored in contiguous memory
    * But this structure still provides relative positioning of the data in the List

## Node

* A `Node` is an item in the LinkedList
* A Node contains the data that we are storing in the list and a reference to the next Node in the Linked List

## LinkedList

* A `LinkedList` manages and maintains the chain of nodes as a List collection
* It contains a **head** reference to the **first** node in the Linked List chain
    * As long as we know where the first element is, we can walk down the Linked List and visit every node in the structure
* Methods in the LinkedList class should maintain the links between the nodes
    * These methods maintain the "links" between the nodes in order to keep the LinkedList structure in a valid state

## Class Definition for a Node Object

```
# A Node object maintains data and next attributes
# and has getters / setters for these attributes

class Node:
    def __init__(self, data):
        self.data = data
        self.next = None

    def getData(self):
        return self.data

    def getNext(self):
        return self.next

    def setData(self, newData):
        self.data = newData

    def setNext(self, newNext):
        self.next = newNext
```