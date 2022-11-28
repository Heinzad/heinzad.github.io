Aide Memoire 

Read 'The Origins of Python' explained with bubble sort 
======================================================= 

Seen on [Hacker News](https://news.ycombinator.com/): 

Lambert Meertens, ["The Origins of Python"](https://inference-review.com/article/the-origins-of-python), Inference, vol 7 no 3 November 2022  

* A story of creating a user-friendly programming language (ABC) from design principles. Its adoption was constrained by copyright and few people used it. Python was inspired by the ABC design principles, but users adopted it enthusiastically and its open source license made it easy to share and build a community.  

* Features our old friend Bubble sort: 

"Bubble sort is a well-known but rather inefficient sorting algorithm, particularly for larger data sets. Yet, because of its simplicity, it is often used to illustrate elementary algorithmic concepts and to provide a first-glance comparison between programming languages" (Meertens 2022). 

**ABC:** 

``` 

FOR i IN {1..#a-1}:
  FOR j IN {1..#a-1}:
    IF a[j] > a[j+1]:
      PUT a[j+1], a[j] IN a[j], a[j+1] 

#(Meertens 2022)

``` 

**Python:**  

```python 

for i in range(len(a)-1):
  for j in range(len(a)-1):
    if a[j] > a[j+1]:
      a[j], a[j+1] = a[j+1], a[j] 

#(Meertens 2022) 

``` 

"Python’s ease of learning gave it a competitive advantage in a period when there was a perpetual need for more programmers. Its clean syntax and semantics make it easier to maintain a software base written in Python than other languages—an important consideration given that the cost of maintaining software dwarfs the cost of creating new software" (Meertens 2022). 

(https://inference-review.com/article/the-origins-of-python)  


Adam Heinz 
28 November 2022 
