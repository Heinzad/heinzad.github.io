AIDE MEMOIRE

Algorithms: Stacks and Queues in Python 
========================================= 

Stacks and Queues are referred to as linear data structures as each item sits in an ordered position in a list. They have a top and bottom. 


## Stacks 

Like a stack of books or a stack of plates, where you access the top item, Stacks are a Last-In, First-Out (LIFO) abstract data type. 


### Stack ADT

The Stack Abstract Data Type has three operations: 
* __push__ - place a new value on the top of the stack 
* __pop__ - remove the top-most value and return
* __peek__ - preview the top-most value 


### Stack Implementation 

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


### Stack Analysis 

As implemented with python list functionality, the principal operations are O(1): 

| ADT | Python | Big-Oh | 
| --- | --------- | ------ |
| push | append() | O(1) |
| pop | pop() | O(1) | 
| peek | slice [:-1] | O(k) |


### Stack Applications

Stacks are used in a browser "back" button that accesses the last page visited, or the "undo" button in a document editor that takes you back to the last save point used. 


| infix | prefix | postfix | 
| ----- | ------ | ------- | 
| a + b | + a b  | a b +   |
  
  
One application of a stack is in converting infix (i.e. human-readable mathematical equations) to postfix (i.e. machine-readable mathematical equations)expressions. Another is calculating the result of a postfix expression. Both of the following examples rely on the Stack class (above) and a global variable to assign operator precedence.

_Global_Variable_:
```python
# Rank operator precedence using BODMAS 
BODMAS = {
    ")": 0, # right bracket 
    "/": 3, # division
    "*": 3, # multiplication 
    "+": 2, # addition 
    "-": 2, # subtraction
    "(": -1 # left bracket
}
```

#### _Converting infix expressions to postfix using Stacks_: 

_Algorithm_: 
```
for each symbol s  
if s is an operand  
    append s to outputs list
else if s is a left parenthesis  
    push s to operators stack 
else if s is a right parenthesis 
    pop each operator from stack to outputs until \
    left parenthesis is reached
else: # s is an operator 
    pop all equal or higher-precedence operators and \
    append to outputs
    push s to outputs stack
pop remaining operators and append to outputs
```

_Program_:
```python
def convert_infix_to_postfix(infix: str)-> str : 
    """Returns a postfix expression converted from a given infix expression. 
    -- assumes all numbers and operators are separated by a string

    >>> convert_infix_to_postfix('1 + 2')
    '1 2 +'
    >>> convert_infix_to_postfix('( A + B ) * ( C + D )')
    'A B + C D + *'
    """

    symbols = infix.split()
    outputs = []
    operators = Stack() 

    # for each symbol s 
    for symbol in symbols: 
        # if an operand, output 
        if symbol not in BODMAS: 
            outputs.append(symbol)
        
        else: # operators 
            # if a left parenthesis, push
            if symbol == "(": 
                operators.push(symbol) 
            # if a right parenthesis, pop outputs \
            # until corresponding left parenthesis is popped
            elif symbol == ")": 
                operator = operators.pop() 
                while operator != "(": 
                    outputs.append(operator)
                    operator = operators.pop() 
            # otherwise, pop all higher or equal operators to outputs \
            # then push
            else: 
                while not operators.is_empty() and \
                    BODMAS[operators.peek()] >= BODMAS[symbol]:
                    outputs.append(operators.pop())
                operators.push(symbol)

    # pop any remaining operators to outputs
    while not operators.is_empty():
        outputs.append(operators.pop())
    
    # results
    return " ".join(outputs)

```

#### _Calculating postfix expressions with Stacks_: 

_Algorithm_: 
```
for each symbol s: 
    if s is an operand: 
        push s to operands stack
    else # s is an operator
        pop two operands from stack 
        apply operator 
        push result to operands stack
pop result from stack 
```

_Program_:
```python
def calculate_postfix(postfix:str) -> float:
    """Returns the result of a given postfix expression
    -- assumes all numbers and operators are separated by a string

    >>> calculate_postfix('7 2 -')
    5.0
    >>> calculate_postfix('7 2 +')
    9.0
    >>> calculate_postfix('5 9 *')
    45.0
    >>> calculate_postfix('45 10 /')
    4.5
    >>> calculate_postfix('7 2 - 7 2 + * 10 /')
    4.5

    """
    symbols = postfix.split()
    operands = Stack()

    for symbol in symbols: 
        # if an operand, push to stack 
        if symbol not in BODMAS: 
            operands.push(float(symbol)) 
        else: # if an operator
            # pop top two operands
            ultimate_value = str(operands.pop())
            penultimate_value = str(operands.pop())
            # apply operator
            expression = f"{penultimate_value} {symbol} {ultimate_value} " 
            # push result to stack 
            operands.push(eval(expression))
    
    return operands.pop() 
```




## Queues 

Queues are First-In, First-Out (FIFO) abstract data type. 

### Queue ADT 

The Queue abstract data type has two principal operations: 
* __enqueue__ - Places a new value at the end of the queue
* __dequeue__ - Removes the value at the front of the queue


### Queue Implementation 
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

### Queue Analysis 

With the way that Python implements pop, a dequeue operation is O(n), while an enqueue is O(1) using append. 

| ADT | Python | Big-Oh | 
| --- | --------- | ------ |
| enqueue | append() | O(1) |
| dequeue | pop(i) | O(n) | 


## Deques

A Deque is a variation on a Queue where items can be added or removed from either the front or the rear. 

### Deque ADT 

The Deque has four principal operations: 
* enqueue_front
* enqueue_rear
* dequeue_front
* dequeue_rear

### Deque Implementation 

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

### Deque Analysis 

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

