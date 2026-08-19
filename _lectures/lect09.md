---
num: "Lecture 9"
desc: "Midterm (Linked Lists cont.)"
ready: true
lecture_date: 2026-08-20 14:00:00.00-7:00
---

Recorded Lecture: [Linked Lists](https://drive.google.com/file/d/1eOStHxaiYLuXDaOMrMEgzYmSnKUbV8PI/view?usp=drive_link)

```
As stated in class, I'm linking a recorded lecture from a 
previous offering of CS 9. Please watch the before our next 
lecture on Tuesday 8/25.

You can ignore any announcements / reminders in the video. 
Reminders relevant for this course are:
* h04 due Tuesday 8/25 by 2PM PST
* lab04 due Tuesday 8/25 by 11:59PM PST
```

## LinkedList Implementation (Chapter 4.21)

```
# LinkedList.py
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

class LinkedList:
    def __init__(self):
        self.head = None

    def isEmpty(self):
        return self.head == None

    def addToFront(self, item):
        temp = Node(item)
        temp.setNext(self.head)
        self.head = temp

    def length(self):
        temp = self.head
        count = 0
        while temp != None:
            count = count + 1
            temp = temp.getNext()
        return count

    def search(self, item):
        temp = self.head
        found = False
        while temp != None and not found:
            if temp.getData() == item:
                found = True
            else:
                temp = temp.getNext()
        return found

    def remove(self, item):
        current = self.head
        
        if current == None: # empty list, nothing to do
            return

        previous = None
        found = False
        while not found: #Find the element
            if current == None:
                return
            if current.getData() == item:
                found = True
            else:
                previous = current
                current = current.getNext()

        # Case 1: remove 1st element
        if found == True and previous == None:
            self.head = current.getNext()
        
        # Case 2: remove not 1st element
        if found == True and previous != None:
            previous.setNext(current.getNext())
```

# Pytests for the Linked List Implementation

```
#LinkedListTest.py
from LinkedList import LinkedList, Node

def test_NodeCreation():
    n = Node(20)
    assert n.getData() == 20
    assert n.getNext() == None

def test_NodeSetData():
    n = Node(20)
    n.setData(200)
    assert n.getData() == 200

def test_NodeSetNext():
    n = Node(20)
    n2 = Node(10)
    n.setNext(n2)
    assert n.getNext() == n2

def test_createList():
    ll = LinkedList()
    assert ll.isEmpty() == True

def test_addingNodesToList():
    ll = LinkedList()
    assert ll.isEmpty() == True
    ll.addToFront(10)
    ll.addToFront("Gaucho")
    ll.addToFront(False)
    assert ll.isEmpty() == False
    assert ll.length() == 3
    assert ll.search(10) == True
    assert ll.search("Gaucho") == True
    assert ll.search(False) == True
    assert ll.search("CS9") == False

def test_removeNodesFromList():
    ll = LinkedList()
    ll.addToFront(10)
    ll.addToFront("Gaucho")
    ll.addToFront(False)
    ll.addToFront("CS9")
    assert ll.length() == 4
    assert ll.search(10) == True
    ll.remove(10)
    assert ll.search(10) == False
    assert ll.search("Gaucho") == True
    assert ll.search(False) == True
    assert ll.search("CS9") == True
    assert ll.length() == 3
    ll.remove(False)
    assert ll.search(False) == False
    assert ll.search("Gaucho") == True
    assert ll.search("CS9") == True
    assert ll.length() == 2
    assert ll.isEmpty() == False
    ll.remove("Gaucho")
    ll.remove("CS9")
    ll.isEmpty() == True
    ll.length() == 0
```

# Ordered Linked Lists
* We’ve discussed Unordered Linked Lists where the position of the nodes did not matter with respect to each other
* An Ordered Linked List is similar to an Unordered Linked List except (as you might guess) the nodes in the list are ordered with respect to each other
* The implementation of both are similar, except we have to maintain the ordered property of the nodes when we manage the list
    * Most methods can be the same, but adding the node requires us to put a node in the correct position (instead of simply adding to the front of the list)
    * Consider two cases:
        1. Adding to the front of the Linked List
        2. Adding to the middle / end of the Linked List

![AddLinkedList.png](AddLinkedList.png)
