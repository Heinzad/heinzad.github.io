AIDE MEMOIRE

Algorithms: Searching in Python
===============================

Searching whether or not an item is present in a collection of items is commonly done using sequential search, binary search, or hashing approaches. 


## Sequential Search 

Sequential search is also known as linear search. 
Take an unordered list, for example. To find an item in an unordered list, the operation must move from node to node in sequence until either the item is found or the end of the list is reached. 

### Analysis

The analysis of sequential or linear search depends on whether or not the item is actually present in list, and if the list is ordered or unordered. 
For an unordered list where the item is present, on average it will be found in the middle of the list
  
_Present_: 
  
| case | description | Big-Oh | 
| ---- | ----------- | ------ |
| best | item at head of list | O(1) |
| worst | item at tail of list | O(n) |
| average | item in middle of list | O(n/2) |  
  
  
_Absent_: 
  
| case | description | Big-Oh | 
| ---- | ----------- | ------ |
| best case | traverse entire list | O(n) |
| worst case | traverse entire list | O(n) |
| average case | traverse entire list | O(n) |



## Binary Search 

Binary search takes advantage of the properties of an unordered list. It begins by comparing an item the mid-point of a list to determine if the item resides in the upper or lower half of the list. It then works recursively by bisecting the relevant half of the bisected list until the item is found. 

### Analysis 

A binary search is logarithmic as the bisection halves the dataset with each operation. 

| case | description | Big-Oh | 
| ---- | ----------- | ------ |
| best worse average | bisection | O(log n) |



## Hashing 

Hashing uses a hash function to allocate items to a slot in a hash table. A hash returns an integer value for a given item. 
Slotting is determined by the modulo of the hash and the number of slots in the hash table: 
```
    hash % slots
``` 
The load factor is the number of used slots by the number of slots in the hash table: 
```
    theta = slotted / slots
``` 

A collision occurs when two or more items are hashed to the same slot. Collision resolution strategies are classified as open or closed addressing. 

Closed addressing takes the slots as given: 
* Chaining -- uses the slot as a pointer to a linked list holding all the items with that hash. 

Open addressing tries to find an unused slot: 
* Linear probing -- iterates through the slots until an empty slot is found.
* Rehashing -- generates a new hash 
* Quadratic probing -- takes the hash and adds the square of the numbers one (1) to four (4) to search for empty slots. 


### Analysis 

With open addressing, the best case of a successful search is O(n). 

With closed addressing, the worst is O(n) where searching traverses the whole of the linked list. 


## References 

Brad Miller & David Ranum (2011). [Problem Solving with Algorithms and Data Structures using Python](https://runestone.academy/ns/books/published/pythonds/index.html)



QED 

© Adam Heinz 

9 February 2024 
