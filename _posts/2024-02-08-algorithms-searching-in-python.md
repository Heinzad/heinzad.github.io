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





## References 

Brad Miller & David Ranum (2011). [Problem Solving with Algorithms and Data Structures using Python](https://runestone.academy/ns/books/published/pythonds/index.html)



QED 

© Adam Heinz 

9 February 2024 
