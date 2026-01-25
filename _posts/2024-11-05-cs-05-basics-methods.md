---
layout: post
tags: C#
title: "05. C# Methods"
author: "Adam Heinz"
date: 2024-11-05 05:00:00
---

AIDE MEMOIRE

C# Methods
==========


# Void Method 

A void method returns nothing. 
Syntax: `{access modifier} {return type} {method name}( {parameters} ); `

For example:

```c#
private void DisplayMessage()
{
    MessageBox.Show("Display a Message");
}

// Method Call 
DisplayMessage();
```

## Value Methods

A value-returning method returns a value to the statement that called it. Syntax: 
```
{access modifier} {data type} {method name}( {parameters} )
{
    statement;
    return expression;
}
```

For example: 

```c#
private int summation (int val1, int val2, int defaultVal = 0) // multiple arguments, default
{
    return defaultVal + val1 + val2; // Named arguments
}
```


## Parameters

Parameters mostly act like constants within the scope of the method.

### Reference Parameters

Reference parameters act like variables, in that they can be modified within a method. 

```c#
private void zeros(ref int number)
{
    number = 0;
}
```

or 

```c#
int myNumber = 9;
private void zeroz(ref myNumber)
{
    myNumber -= 2;
}
```

### Output Parameter

An output parameter is like a reference parameter but returns the variable. 

```c#
private void zeroed(out int number)
{
    myNumber = 0;
}

int number;
zeroed(out number)
```







QED 

© Adam Heinz 

6 November 2024

