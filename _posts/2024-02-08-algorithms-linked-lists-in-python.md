AIDE MEMOIRE

Algorithms: Linked Lists in Python
==================================

Lists are collection of nodes that are joined by their references to each other. An unordered list is joined only by the sequence of the addition of items to the list. An ordered list refers to some property by which items are sequenced in the list. 

## Unordered Lists

### ADT

The Unordered List Abstract Data Type has several key operations: 
* List - Creates a pointer to the start of the list
* add - Adds a given item to the list 
* remove - Removes a given item to the list
* search - Searches for a given item in the list

It also requires a basic building block to hold data and positional references: 
* Node

### Implementation

We can build a stack in Python using a Linked List ADT. 

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


class Stack:
    """Implements a stack using a Linked List Abstract Data Type with push, peek, pop, is empty, and length operations.

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
        """ initialises a linked list as a pointer to the head of the list"""
        self.head = None
    
    def push(self, item):
        """Adds a given item to the head of the list"""
        if self.head is None: 
            self.head = Node(item)
        else:
            shunt_node = self.head
            self.head = Node(item)
            self.head.next_node = shunt_node

    def pop(self):
        """Removes and returns the item at the head of a list"""
        if self.head is not None: 
            pop_item = self.head.item
            self.head = self.head.next_node
        return pop_item
    
    def peek(self):
        """Return the item at the head of a list without removing it"""
        if self.head is not None:
            return self.head.item
    
    def is_empty(self): 
        """Returns True if the list is empty"""
        return self.head is None
    
    def __len__(self): 
        """Returns the length of the list"""
        counter = 0
        current_node = self.head
        while current_node is not None:
            counter += 1
            current_node = current_node.next_node
        return counter

```

### Analysis 

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


## References 

Brad Miller & David Ranum (2011). [Problem Solving with Algorithms and Data Structures using Python](https://runestone.academy/ns/books/published/pythonds/index.html)



QED 

© Adam Heinz 

9 February 2024 

