---
title: "C# 02: Program Flow"
author: "Adam Heinz"
date: 2024-11-05T02:00:00-00:00
categories:
  C#
tags:
  - Language
---
How to use program flow with C\#.  



# Control Structures

There are three types of *control structure* that control the order in which statements execute: Sequential, Decision, and Iteration. 

## Sequential 

A script executes in the order in which it is written: 

```c#
// Initialise Variables
decimal salesTotal = 0m; 
decimal discountPercent = 0.01;
decimal discountAmount = 0m;
decimal grandTotal = 0m;

salesTotal = 100.00;
discountAmount = salesTotal * discountPercent;
grandTotal = salesTotal + discountAmount;
```

## Decision

A decision structure only performs actions if a specified condition is met. 

### Relational Operators

Relational operators return a boolean value when used in an assertion: 

| symbol | usage | description |
| ------ | ----- | ----------- |
| > | x > y | x is greater than y |
| < | x < y | x is less than y |
| >= | x >= y | x is greater than or equal to y |
| <= | x <= y | x is less than or equal to y |
| == | x == y | x is equal to y |
| != | x != y | x is not equal to y |


### if Statement 

The If statement provides control-of-flow logic. 

```
flowchart LR
    A((begin))
    B{evaluate}
    C(statement)
    D((end))
    A --- B
    B-- true --C
    C --- D
```

A simple if statement can be written with the syntax: 
`if ({expression}) {statement};`.


### if-else-if Statement


```
flowchart TD
    A((begin))
    B{evaluate}
    C(statement)
    D(otherwise)
    E((end))
    A --- B
    B-- true --C
    B-- false --D
    C --- E
    D --- E
```

```c#
// Only go out when it is dry outside, otherwise take evasive action
bool isRaining = true;
string msgText;

if (isRaining)
{
    msgText = "Close Windows";
}
else
{
    msgText = "Go Out";
}
MessageBox.Show(msgText);
```

The if-else statement can be nested (in indented blocks), or serial, with else if statements (same indentation).

```
flowchart TD
    A((begin))
    B{evaluate1}
    C(otherwise1)
    D{evaluate2}
    E(otherwise2)
    F(statement)
    G((end))
    A --- B
    B-- true --C
    B-- false --D
    D-- true --E
    D-- false --F
    C --- E
    F --- E
```

if-else-if statements can be written in consecutive blocks

Boolean logic can be used to create a compound expression with multiple criteria: 

```c#
// Only go out when it is warm, dry and sunny outside, otherwise take evasive action
bool isSunny = false;
bool isRaining = true;
bool isCold = false;
string msgText;

if (isSunny && !isRaining && !isCold)
{
    msgText = "Go Out";
}
else if (isRaining)
{
    msgText = "Close Windows";
}
else if (isCold)
{
    msgText = "Light Fire";
}
else
{
    msgText = "Wear Hat";
}
MessageBox.Show(msgText);
```

### Switch Statement 

A switch statement is an alternative to the if-else-if statement. It can be easer to read than an if-else-if statement. 

A switch statement uses a test expression. Each subsection begins with a case statement and ends with a break. An optional default statement can address what else to do if the test expression is not met.  

```c#
switch ({expression})
{
    case {criteria}: 
        statements;
        break;
    
    default:
        statements;
        break;
}
```


Take calendar months, for example: 
```c#
// Display a month name for given month numbers in the first quarter only
int month = 2;
string msgText;

switch(month)
{
    case 1:
    msgText = "January";
    break;

    case 2:
    msgText = "February";
    break;

    case 3:
    msgText = "March";
    break;

    default:
    msgText = "Invalid Month";
    break;
}
```





## Iteration 

### Operators

We can increment or decrement a variable with operators: 

| symbol | description | 
| ------ | ----------- |
| ++ | increase by 1 |
| -- | decrease by 1 |


### While Loop

A while loop repeats for as long a condition remains true. The basic syntax is: 
`while ({boolean expression}) {statement};`. While is a pre-test loop in that the condition is given first 

Note that an infinite loop can occur if the check condition is written in such a way that it is never met.

```
flowchart TD
    A((begin))
    B[/.../]
    C{evaluate}
    D((end))
    A --- B
    B --- C
    C-- true --B
    C-- false --D
```

For example, we might use this with a counter: 

```c#
// Initialise variable to count iterations
int counter = 0;
// Display message five times over
while (counter < 5)
{
    // Display message
    MessageBox.Show("Hello World");

    // increase the count
    counter ++;
}
```

### Do Whle Loop

Do-While is a post-test loop in that it performs the check after the first iteration

```
flowchart TD
    A((begin))
    B[/statement/]
    C{evaluate}
    D((end))
    A --- B
    B --- C
    C-- true --B
    C-- false --D
    D --- E
```

```c#
// Check the weather ten times
int countRows = 10;
int countLoop = 1; 

do
{
    // Check the weather
    MessageBox.Show("Look out the window");   
    // Increment the counter
    countLoop ++;
} while (countLoop < contRows);
```


## For Loop

The for loop uses a counter variable to control the number of iterations. The basic syntax is: 
`for ({initExpression}; {testExpression}; {updateExpression};) statement;`. You do not modify the counter variable in the body of the for loop. 

```
flowchart TD
    A((begin))
    B[/initialize/]
    C{evaluate}
    D(statement)
    E(update)
    F((end))
    A --- B
    B --- C
    C-- true --D
    D --- E
    E --- C
    C-- false --F
```

```c#
// Loop five times
for(int count = 0; count < 5; count ++;)
{
    MessageBox.Show("Start at Zero; Stop at 5 iterations; Increment on each iteration;");
}
```


# Further Reading

Microsoft (29 Apr 23). [Selection statements - if, if-else, and switch](https://learn.microsoft.com/en-us/dotnet/csharp/language-reference/statements/selection-statements)



QED 

© Adam Heinz 

6 November 2024
