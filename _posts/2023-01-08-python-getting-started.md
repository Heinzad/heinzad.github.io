---
layout: post
tags: Python
title: "Getting Started with Python"
author: "Adam Heinz"
date: 2023-01-08 01:00:00
---
"All you need is a simple text editor, a shell, and Python" according to Zed Shaw.   


# Overview

Zed Shaw's [Learn Python 3 The Hard Way](https://learnpythonthehardway.org/python3/) recommends that beginners avoid using an IDE (Integrated Development Environment). However, his favourite text editor, atom, no longer exists. Notepad++ gives a minimalist text editor with few distractions. 

This is a setup that works on a windows machine running Windows10: 

* Notepad++  
* PowerShell 
* Python 3  


### Create a new directory 

You can make a new directory in the PowerShell terminal using the mkdir (Make Directory) command: 

```powershell 

mkdir mynewproject 

``` 



### Switch between programs using a mouse 

The other thing to learn is switching between open programs without using a mouse: 

``` 

CTRL+ALT+TAB 

``` 


### Create a new python file 

In Notepad, we can open a new document using: 

``` 

CTRL+N 

``` 

We can change the language to Python using: 

``` 

ALT+L  >  P  >  ↑ 

``` 

(The up arrow takes you from the top of the list to Python, which is at the bottom of the list). 


We can now save the python file with Notepad++ : 

``` 

CTRL+S  >  helloworld.py 

``` 


### Write some code 


We can now write some fairly sophisticated code in the text editor, but we can just demonstrate the classic "hello world" here:  

```python 

""" Demonstrate that our setup works by printing 'hello world' """ 

print("Hello World") 


``` 

We can now run our program in the terminal. 



### Navigate to project directory 

In the PowerShell terminal we navigate to the new directory using the cd (Change Directory) command: 

```powershell 

cd mynewproject 

``` 

### Run the script 

We can now run the script in the terminal: 

```powershell 

python helloworld.py 

``` 

This prints the message: 

``` 

Hello World 

``` 




QED 

© Adam Heinz 

9 January 2023 
 
 