AIDE MEMOIRE

Algorithms: Stacks and Queues in Python 
========================================= 

Stacks and Queues are referred to as linear data structures as each item sits in an ordered position in a list. They have front and rear ends. 


## Stacks 

Stacks are a Last-In, First-Out (LIFO) abstract data type. 

### ADT

The Stack Abstract Data Type has three operations: 
* push
* pop
* peek

### Implementation 

In Python, the stack can be implemented using the existing list functionality. 

```python 

class Stack(object): 
    """Implements a stack using python list functionality
    
    >>> s = Stack() 
    >>> s.push('alpha') 
    >>> s.peek()
    'alpha'
    >>> s.pop()
    'alpha'
    """

    def __init__(self): 
        """Initialises a stack as a list"""
        self._stack = []

    def push(self, item): 
        """Adds an item onto the stack"""
        self._stack.append(item) 

    def pop(self): 
        """Removes and Returns the last item from the stack"""
        if len(self._stack) > 0: 
            return self._stack.pop() 

    def peek(self): 
        """Returns the last item from the stack without removing it"""
        if len(self._stack) > 0: 
            return self._stack[-1] 

```

### Analysis 

As implemented with python list functionality, the principal operations are O(1): 

| ADT | Python | Big-Oh | 
| --- | --------- | ------ |
| push | append() | O(1) |
| pop | pop() | O(1) | 
| peek | slice [:1] | O(k) |


## Queue 

Queues are First-In, First-Out (FIFO) abstract data type. 

### ADT 

The Queue abstract data type has two principal operations: 
* enqueue
* dequeue


### Implementation 
Queues can be implemented in Python using list functionality. 

```python

class Queue(object): 
    """Implements a queue using Python list functionality. 

    >>> q = Queue()
    >>> q.enqueue('alpha')
    >>> q.dequeue()
    'alpha'
    """

    def __init__(self):
        """Initialises a queue as a list"""
        self._queue = []

    def enqueue(self, item): 
        """Adds an item at the rear of a queue"""
        self._queue.append(item)

    def dequeue(self):
        """Removes and returns an item from the front of a queue"""
        if len(self._queue) > 0:
            return self._queue.pop(0) 

```

### Analysis 

With the way that Python implements pop, a dequeue operation is O(n), while an enqueue is O(1) using append. 

| ADT | Python | Big-Oh | 
| --- | --------- | ------ |
| enqueue | append() | O(1) |
| dequeue | pop(i) | O(n) | 


## Deque

A Deque is a variation on a Queue where items can be added or removed from either the front or the rear. 

### ADT 

The Deque has four principal operations: 
* enqueue_front
* enqueue_rear
* dequeue_front
* dequeue_rear

### Implementation 

The Deque can be implemented as a list in Python. 

```python

class Deque(object): 
    """Implemements a Deque using Python list functionality.

    d = Deque()
    >>> d.enqueue_front('alpha')
    >>> d.enqueue_rear('bravo') 
    >>> d.enqueue_rear('charlie')
    >>> d.dequeue_front()
    'alpha'
    >>> d.dequeue_rear()
    'charlie'
    >>> d.dequeue_front()
    'bravo'
    """

    def __init__(self):
        """Initilises a deque as a list""" 
        self._deque = []
    
    def enqueue_front(self, item): 
        """Adds an item at the front of a deque"""
        self._deque.insert(0, item)
    
    def enqueue_rear(self, item): 
        """Adds an item to the rear of a deque"""
        self._deque.append(item)
    
    def dequeue_front(self):
        """Removes and returns an item from the front of a deque"""
        if len(self._deque) > 0: 
            return self.pop(0)
    
    def dequeue_rear(self): 
        """Removes and returns an item from the rear of a deque"""
        if len(self._deque) > 0: 
            return self.pop()

```

### Analysis 

Using Python list functionality for a deque, front-of-deque operations are O(n), while rear-of-deque operations are O(1).  

| ADT | Python | Big-Oh | 
| --- | --------- | ------ |
| enqueue_front | insert(i, item) | O(n) |
| enqueue_rear | append() | O(1) |
| dequeue_front | pop(i) | O(n) |
| dequeue_rear | pop() | O(1) | 



## References 

Brad Miller & David Ranum (2011). [Problem Solving with Algorithms and Data Structures using Python](https://runestone.academy/ns/books/published/pythonds/index.html)



QED 

© Adam Heinz 

9 February 2024 

