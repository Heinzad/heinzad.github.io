---
title: "C# 03: Files Reading and Writing"
author: "Adam Heinz"
date: 2024-11-05T03:00:00-00:00
categories:
  C#
tags:
  - Language
---
How to read and write files with C\#.  


# Namespace

Using the `System.IO` namespace obtains several classes for reading and writing files:
- File system: `File` creation, copying, deletion, moving, opening of a file. 
- Read: `StreamReader` a TextReader that reads from a string.
- Write: `StreamWriter` a TextWriter that writes to a string.

## Location

The default location of a file is the currrent directory: `.\bin\Debug\
To open a file at a different location, prefix a path string with the @ symbol. 
`@"C:\Users\me\Documents\test.txt"`. 
(The **verbatim string literal**, which is prefixed with the @ symbol, handles escape characters without having to escape the escape character). 

## File 

- `.OpenText` opens a file.
- `.CreateText` writes a new file, or overwrites an existing file.
- `.AppendText` preserves an existing file while adding to its contents.


## Reading

The `StreamReader` class provides two methods to read data from file:
- line: `.ReadLine` returns a line of data as a string.
- string: `.Read` returns characters from data as a string.

```c#
try
    {
    // Initialise a variable to hold a line from the file
    string fileData; 
    
    // Instantiate a Stream Reader Object
    StreamReader fileReader;

    // Open the file 
    fileReader = File.OpenText("test.txt"); 

    // Keep reading lines until reaching the end of the text
    while (!fileReader.EndOfStream)
    {
        fileData = fileReader.ReadLine(); 

        // Do something 
    }

    // Close the connection after reaching the end of the text
    fileReader.Close();
}
catch (Exception exErr)
{
    // Display an error message
    MessageBox.Show(exErr.Message);
}
```

## Writing

Numeric data gets converted to a string.

```c#
// Directory locations
string filePath = @"target_file.txt";

// Initialise a new StreamWriter object to write to the file 
// Clean up memory with `using()` syntax
using (StreamWriter fileWriter = new StreamWriter(filePath))
{
    // Write a line to the file
    fileWriter.WriteLine("Hello World!");
}
```

### Append

```c#
try
{
    // Directory locations
    string filePath = @"target_file.txt";

    // Initialise a new StreamWriter object to append text to file
    StreamWriter fileAppender;
    fileAppender = File.AppendText(filePath);

    // Write a line to the file
    fileAppender.WriteLine("Append this text");
    fileAppender.Close()
}
catch (Exception exErr)
{
    // Display the error message
    MessageBox.Show(exErr.Message);
}

```



QED 

© Adam Heinz 

6 November 2024
