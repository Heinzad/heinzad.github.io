---
title: "03. Linked Lists"
author: "Adam Heinz"
date: 2024-02-08T03:00:00-00:00
categories:
  Algorithns
tags:
  - Python
---
How to analyse and implement linked lists in Python. 


# Overview

Lists are collection of nodes that are linked together by their references to each other. 
- An __unordered list__ is joined only by the sequence of the addition of items to the list. 
- An __ordered list__ refers to some property by which items are sequenced in the list.

Both unordered and ordered linked lists are discussed here in terms of:
- abstract data type
- implementation
- application


## Unordered Lists

### Unordered List ADT

The Unordered List Abstract Data Type has several key operations: 
* add - Adds a given item to the list 
* remove - Removes a given item to the list

It also requires a basic building block to hold the item and positional references: 
* Node

### Unordered List Analysis 

The complexity of a FIFO operation in a stack implemented as a linked list depends on whether or not it requires traversal: 

| ADT | Big-Oh | Remarks | 
| --- | ------ | ------- |
| is empty | O(1) | Checks head of list |
| length | O(n) | Traversed entire list to end |
| push | O(1) | Repoints head of list | 
| pop | O(1) | Repoints head of list | 
| peek | O(1) | Previews head of list |

The complexity of LIFO operations in a queue implemented as a simple linked list, however has a different profile: 

| ADT | Big-Oh | Remarks | 
| --- | ------ | ------- |
| is empty | O(1) | Checks head of list |
| length | O(n) | Traversed entire list to end |
| push | O(n) | Traverses list to remove final pointer | 
| pop | O(1) | Repoints head of list | 
| peek | O(1) | Traverses list to previews final item |

That performance penalty can be avoided by using another variation, the doubly-linked list, which has both head and tail pointers.


### Unordered List Implementation

We can build a stack in Python using a Linked List ADT. 
The core is a Node class that contains an item and a reference to the next node in the list. A new node is pushed to the top or head of the stack. The pop and peek operations also target the head of the stack. 

We can implement unordered lists as a: 
- stack, or 
- queue

Either method will stack or queue a node: 

```python
class Node:
    """A node in a linked list stores both data and a pointer to the next node

    >>> a = Node('alpha')
    >>> print(a.item)
    'alpha'
    >>> print(a.next_node)
    None
    >>> b = Node('bravo')
    >>> print(b.item)
    'bravo' 
    >>> print(b.next_node)
    None
    >>> a.next_node = b
    >>> print(a.next_node.item)
    'bravo'
    """

    def __init__(self, item): 
        """Initilises a node with given data and a blank pointer"""
        self.item = item
        self.next_node = None 
```

#### Implementing an Unordered List as a Stack:

```python 
class Stack:
    """Implements a stack using a Linked List Abstract Data Type 
    with push, peek, pop, is empty, and length operations.

    >>> s = Stack()
    >>> s.is_empty() 
    True
    >>> s.push('alpha')
    >>> s.push('bravo')
    >>> s.peek()
    'bravo'
    >>> s.pop()
    'bravo'
    >>> s.peek()
    'alpha'
    >>> s.pop()
    'alpha'
    """ 

    def __init__(): 
        """Initialises the stack"""
        self.head = None
    
    def push(self, item):
        """Adds a given item to the top of the stack"""
        if self.head is None: 
            self.head = Node(item)
        else:
            temp_node = self.head
            self.head = Node(item)
            self.head.next_node = temp_node

    def pop(self):
        """Removes and returns the item at the top of the stack"""
        if self.head is not None: 
            pop_item = self.head.item
            self.head = self.head.next_node
        return pop_item
    
    def peek(self):
        """Return the item at the top of the stack without removing it"""
        if self.head is not None:
            return self.head.item
    
    def is_empty(self): 
        """Returns True if the stack is empty"""
        return self.head is None
    
    def __len__(self): 
        """Returns the length of the stack"""
        counter = 0
        current_node = self.head
        while current_node is not None:
            counter += 1
            current_node = current_node.next_node
        return counter
```

#### Implementing an Unordered List as a Queue: 

```python
class Queue: 
    """Implements a queue using a Linked List Abstract Data Type 
    with enqueue, dequeue, is empty, and length operations.
    
    >>> q = Queue()
    >>> len(q)
    0
    >>> q.enqueue('alpha')
    >>> len(q)
    1
    >>> q.enqueue('bravo')
    >>> len(q)
    2
    >>> q.dequeue()
    'alpha'
    >>> len(q)
    1
    >>> q.dequeue()
    'bravo'
    >>> len(q)
    0
    """

    def __init__(self): 
        """Initialises the queue"""
        self.head = None 

    def enqueue(self, item): 
        """Adds an item to the tail of the queue"""
        if self.head is None: 
            self.head = Node(item)
        else: # traversal
            current_node = self.head
            while current_node.next_node is not None: 
                current_node = current_node.next_node
            current_node.next_node = Node(item) 
    
    def dequeue(self):
        """Removes and returns the item at the head of the queue"""
        if self.head is not None: 
            temp_node = self.head # result 
            self.head = self.head.next_node # removal 
        return temp_node 

    def is_empty(self):
        """Returns whether or not the queue is empty"""
        return self.head == None
    
    def __len__(self): 
        """Returns the length of the queue"""
        counter = 0 
        if self.head is not None: 
            current_node = self.head
            while current_node is not None: 
                counter += 1
                current_node = current_node.next_node 
        return counter
```

## Ordered List 

An ordered list holds a collection of items based on the value of some property of the items. 

### Ordered List ADT 

The ordered list abstract type has the same operations as the unordered list.
* add - Adds a given item to the list 
* remove - Removes a given item to the list
* length
* is empty
* search - Searches for a given item in the list

The complexity arises in their implementation, given the need to position the items in order of value. 

### Ordered List Analysis 

### Ordered List Implementation 



## References 

Brad Miller & David Ranum (2011). [Problem Solving with Algorithms and Data Structures using Python](https://runestone.academy/ns/books/published/pythonds/index.html)



QED 

© Adam Heinz 

9 February 2024 

