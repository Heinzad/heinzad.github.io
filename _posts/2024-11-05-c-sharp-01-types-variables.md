AIDE MEMOIRE

Data Types and Variables in C\#
===============================


C\# is a strongly typed language. 


# Variables

A variable is a symbolic name for a location in memory. It can hold only one value at a time.

Local variables are declared inside a method. Their scope and lifetime are limited to that method. 

Global variables are called **Field**s, which are declared at and scoped to the class level.

A constant is a field that cannot change in value. 

```c#
using System;
using System.Windows.Forms;

namespace My_Project
{
    public partial class Form1 : Form
    {
        // Globals
        private int days = 0;

        // Constants
        private const decimal INTEREST_RATE = 0.015m; // decimal (money)
    }
}
```


## Initializing Variables

Variables must be declared before they can be used. 

A variable declaration statement causes the variable to be created in memory. 
The variable declaration syntax is: `{ DataType } { VariableName }`. 

Multiple variables of the same data type can be declared in a single statement: 
```c#
string strExample1, strExample2, strExample3;
```

Variable assignments must be compatible with the data type of that variable. For example, you cannot put a text literal into a numeric variable.
```c#
int myInteger = "100m"; // fails
```

Assignments can be made when the variable is declared: 
```c#
string strExample = "Lorem Ipsum";
```

### Implicitly Typed Local Variables

Variables can also be declared without a type by using the `var` keyword: 
```c#
var myValue = 5;
```

## Naming Variables


# Primitive data types

A literal is a value used by a variable. 

## Character Type

- `char`: a single character only

Single-character literals are encased in single quotes: `'a'`

## String Type

- `string`: character sequences: 
```c#
string strExample = "Lorem Ipsum";
```

String Literals are encased in double quotes and may be proceeded with the at symbol:  `@"{my_string}"`.

### String Operators 

Strings can be concatenated with the `+` operator: 

```c#
string myMessage;
myMessage = "Hello" + " " + "World";
MessageBox.Show(myMessage);
```

## Boolean Type

- `bool`: 

Boolean literals are lower case `true` or `false` values only.

### Boolean Logical Operators

| symbol | name | description | remarks |
| ------ | ---- | ----------- | ------- |
| ! | NOT | Logical Negation | |
| & | AND | Logical AND | Returns `true` only if both operands are `true` | |
| && | AND | Conditional Logical AND |
| ^ | OR | Exclusive OR | |
| \| | OR | Logical OR | Returns `false` only if both its operands are `false`, otherwise returns `true`|
| \|\| | OR | Conditional Logical OR | 



## Integer Types

- `int`: a signed 32-bit integer that stores whole numbers from -2,147,483,648 to 2,147,483,647. 
- `long`: a signed 64-bit integer that stores whole numbers from -9,223,372,036,854,775,808 to 9,223,372,036,854,775,807.

## Floating Point Types

- `float`: fractional numbers with a precision of about 6-9 digits
- `double`: fractional numbers with a precision of about 15-17 digits
- `decimal`: fractional numbers with a precision of 28-29 digits

### Real Literals 

| symbol | description | 
| ------ | ----------- |
| f | float | 
| d | double |
| m | decimal (e.g. money) |


## Math Operators

| operator | description |
| ------ | ----------- | 
| + | Addition | 
| - | Subtraction | 
| * | Multiplication | 
| / | Division |
| % | Modulo - Remainder |


### Combined Assignment Operators

Combined Assignment Operators are a form of short hand that can be used when changing the value of a variable. 

| operator | usage | explication | 
| -------- | ----- | ----------- | 
| += | x += n; | x = x + n; |
| -= | x -= n; | x = x - n; |
| *= | x *= n; | x = x * n; | 
| /= | x /= n; | x = x / n; |
| %= | x %= n; | x = x % n; |



# Type Conversion 

## Implicit Type Conversion 

- some types will convert implicitly, such as int to Long, or float to double, because going from a smaller size to a larger size: 

```c#
// Implicit conversion
int myInteger = 41;
double myDouble = myInteger;
```

## Explicit Type Conversion

### Cast 

Casting explicity converts from one data type to another. Cast syntax is to bracket the required data type and prefix it to a variable: `(type)Variable`.

```c#
// Declare an integer variable
int myInteger;

// Declare a floating point variable
double myDouble = 3.1;

// Cast to integer
myInteger = (int)myDouble;
```


### To String

`ToString()` converts numbers to text: 
```c#
double realNumber = 11.1;
string txtNumber = realNumber.ToString(); 
```

#### Format Number as String:

Extend the ToString() syntax with a format argument:
```c#
// Display number in currency format
decimal salesAmount = 1048576.99;
string salesLabel = salesAmount.ToString("c");
``` 

Format strings can be written in upper or lower case. 

| format | description |
| ------ | ----------- |
| "n" | Number |
| "f" | Fixed-point scientific | 
| "e" | Exponential scientific | 
| "c" | Curency |
| "p" | Percent |

An optional numeric suffix indicates **precision**. For example: `"n4"` displays a number to four decimal places. 

Round down a number by displaying with a lower precision than it possesses. 

Pad the begining of a string with `"d"` to set the minimum width. For example, if the given integer has less than the minimum of digits, it will be given leading zeros so that 11 `ToString("d4")` returns "0011":

```c#
// Display string with leading zeros
int taxN = 11;
string taxNumber = taxN.ToString("d4");
```


### Parse

Convert text to a number using the syntax `{newType}.Parse({oldVariable})`: 
```c#
string txtNumber = "11.1"; 
double realNumber = double.Parse(txtNumber);
```

### Try Parse

`Parse` can fail if the given string cannot be coerced into the target format. 

Prevent exceptions with `TryParse` methods when converting a string to a number. Requres an output variable.
Works on `int`, `double` and `decimal` data types. The syntax is: `{type}.TryParse({"input string"}, out {output variable})`. 
```c#
// initialise variables
string srcText = "3"; 
decimal totNumbers = 0m;
int tgtInteger;
double tgtDouble;
decimal tgtDecimal;

// Accumulate total only if input string is a valid whole number
if (int.TryParse(srcText, out tgtInteger))
{
    totNumbers += tgtInteger
}
    // Accumulate total only if input string is a valid real number
    if (int.TryParse(srcText, out tgtDouble))
    {
        totNumbers += tgtDouble
    }
        // Accumulate total only if input string is a valid money
        if (int.TryParse(srcText, out tgtDecimal))
        {
            totNumbers += tgtDecimal
        }
        else
        {
            // Display error message
            MessageBox.Show("Not a Decimal")
        }
    else
    {
        // Display error message
        MessageBox.Show("Not a Double")
    }
else
{
    // Display error message
    MessageBox.Show("Not an Integer")
}
```





# Further Reading

Microsoft 
- (26 Jan 22). [bool (C# reference)](https://learn.microsoft.com/en-us/dotnet/csharp/language-reference/builtin-types/bool)
- (30 Sep 22). [Integral numeric types (C# reference)](https://learn.microsoft.com/en-us/dotnet/csharp/language-reference/builtin-types/integral-numeric-types)
- (30 Sep 22). [Floating-point numeric types (C# reference)](https://learn.microsoft.com/en-us/dotnet/csharp/language-reference/builtin-types/floating-point-numeric-types)



QED 

© Adam Heinz 

6 November 2024