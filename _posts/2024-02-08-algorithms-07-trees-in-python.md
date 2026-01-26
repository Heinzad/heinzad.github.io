---
title: "Algorithms 07: Trees"
author: "Adam Heinz"
date: 2024-02-08T07:00:00-00:00
categories:
  Algorithns
tags:
  - Python
---
How to analyse and implement binary search trees and traverse them in Python.  


# Overview

Trees consist of a root and subtrees. 

* Parse trees can store mathematical expressions as a tree. 
* Binary trees have a maximum of two child nodes. 
* Binary Search Trees (BST) are defined as having a left branch holding lower values than the right branch. 

## Tree Traversal 

Pre-, In-, and Post-order traversals are classified by whether the root is visited first, betwen, or last among the sub-trees. 

_Pre-Order Traversal_: 
* visit root
* visit left subtree 
* visit right subtree

_ In-Order Traversal_: 
* visit left subtree 
* visit root
* visit right subtree

_ Post-Order Traversal_: 
* visit left subtree 
* visit right subtree
* visit root



## References 

Brad Miller & David Ranum (2011). [Problem Solving with Algorithms and Data Structures using Python](https://runestone.academy/ns/books/published/pythonds/index.html)



QED 

© Adam Heinz 

9 February 2024 
