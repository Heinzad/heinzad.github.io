---
layout: post
tags: Algorithms
title: "01. Algorithm Analysis"
author: "Adam Heinz"
date: 2024-02-08 01:00:00
---
AIDE MEMOIRE 

Analysis 
======== 

Analysing Algorithms requires both a knowlege of logarithms and of Big-O notation. 

An algorithm is a precise set of instructions for solving a problem ( the term is derived from the name of the scholar al-Khwarizmi, the founder of algebra). 

We can pick and choose between alternative algorithms on offer if we can identify the bottlenecks in their performance: 
* running time  
* resources -- such as space or memory 

That analysis of the bottlenecks is called asymptotic analysis. 

# Asymptotic Analysis 

Finding the limits of a function when x is very large (i.e. asymptotic). 


## Big-O Notation 

Big O notation classifies algorithms by how their run time or resources used grow as x gets bigger. 

* find dominant function T(n) 
* find order of magnitude O(f(n)) -- the part of the function that increases the fastest as n increases 

Steps: 
1 count how often used 
2 take highest order only (e.g. 1, logn, n, n2, n3, 2n) 
3 remove constants 

### Functions in ascending order of magnitude  

| f(n) | Name | Remarks | 
| ---- | ---- | ------- | 
| 1 | Constant | unchanging | 
| log log n | Double Logarithmic | - | 
| log n | Logarithmic | slow increase | 
| n | Linear | proportional | 
| n log n | Log Linear | a little worse than n | 
| n2 | Quadratic | bad | 
| n3 | Cubic | very bad | 
| 2n | Exponential | explosive growth | 



### Results 

We return the results in terms of the best, worst, or average case:  
+ b.c. -- Best Case 
+ w.c. -- Worst Case 
+ a.c. -- Average Case 



## Logarithms 

Logarithms grow slowly. For example, it takes only 10 steps to reach 1,000, and only 20 steps to reach 1,000,000. 

| log2k | k | 
| ----- | --- | 
| 1 | 2 | 
| 2 | 4 | 
| 3 | 8 | 
| 4 | 16 | 
| 5 | 32 | 
| 6 | 64 | 
| 7 | 128 | 
| 8 | 256 | 
| 9 | 512 | 
| 10 | 1024 | 
| 20 | 1048576 | 
| 30 | 1073741824 |


# Common operations 

* O(1) hash sort 
* O(log n) binary search 
* O(n) sequential search 
* O(n log n) quicksort (b.c.)
* O(n2) quicksort on sorted list (w.c.) 
* O(n2) selection sort 
* O(n3) matrix multiplication 
* O(2n) Optimisation 





# References 

Brad Miller & David Ranum (2011). [Problem Solving with Algorithms and Data Structures using Python](https://runestone.academy/ns/books/published/pythonds/index.html)

Jeff Erikson (2019). [Algorithms](http://jeffe.cs.illinois.edu/teaching/algorithms/) 

Wikipedia [Big O Notation](https://en.wikipedia.org/wiki/Big_O_notation)
