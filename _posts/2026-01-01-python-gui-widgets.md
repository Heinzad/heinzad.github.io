---
layout: post
tags: Python
title: "GUI Widgets"
author: "Adam Heinz"
date: 2026-01-01 11:00:00
---
How to use Tkinter widgets to create a GUI for a Python application.  


# Overview

Widgets are the building blocks of user interaction in Tkinter. 
Each widget has a class. 


### Basic Widgets: 

1. Frame
2. Label
3. Button
4. Entry
5. Text 


### Preparatory: 

Load the library:

```python
import tkinter as tk
```

Load themed widgets: 

```python
from tkinter import ttk
```

Frame
-----

Displays a rectangle. Contains child widgets. 

```python
myInterface = tk.Tk()
myInterface.title('User Interface')

myFrame = tk.Frame(myInterface)
otherFrame = tk.Frame(myInterface)

myFrame.pack()
otherFrame.pack()
```

*The order in which the frames are packed is also the order in which they are displayed.*  


Label
-----

Displays text and images. Options include font, background, foreground, etc. 


```python
backgroundImage = PhotoImage(file="the-background.png")

myBackground_Label = tk.Label(
    master=myFrame,
    image=backgroundImage,
    bg="#525561"
)
myBackground_Label.pack()

myHeader_Label = tk.Label(
    master=myBackground_Label,
    text="Caption",
    fg="#FFFFFF",
    font=("yu gothic ui bold", -20),
    bg = "#272A37"
)
myHeader_Label.pack()
```


Button
------

Contains text and performs actions when clicked.

```python
myAction_Button = tk.Button(
    master=myBackground_Label,
    text="Activate", 
    foreground="#206DB4",
    font=("yu gothic ui bold", -13),
    background="#272A37",
    borderwidth=0,
    cursor="hand2",
    activebackground="#272A37",
    activeforeground="#FFFFFF",
    command=myInterface.destroy
)
myAction_Button.pack()
```

*If the command is to execute a function with parameters then use `command=lambda: aFunc(args)`. Otherwise use `command=bFunc()`.*  

Enable or disable a button by updating its dictionary. 

Set the disabled flag:

```python
myAction_Button.state(['disabled'])
```

Clear the disabled flag: 

```python
myAction_Button.state(['!disabled'])
```



Entry
-----

Data entry for a single line of text. 

```python
my_name = StringVar()

myName_Entry = Entry(
    master=myBackground,
    bd=0,
    bg="#3D404B",
    highlightthickness=0,
    font=("yu gothic ui semibold", -16),
    textvariable=my_name
)
myName_Entry.pack()

print(myName_Entry.get())
```

Use a grid layout to place textboxes beside their labels: 

```python
alpha_Label = ttk.Label(myInterface, text='First Name')
beta_Label = ttk.Label(myInterface, text='Last Name')

alpha_Entry = ttk.Entry(myInterface)
beta_Entry = ttk.Entry(myInterface)

alpha_Label.grid(row=0, column=0)
alpha_Entry.grid(row=0, column=1)
beta_Label.grid(row=1, column=0)
beta_Entry.grid(row=1, column=1)
```

Text
----

Data entry for multi-line text. 

```python
loremIpsum_Text = tk.Text(master=myFrame)
loremIpsum_Text.pack()
```

Insert: 

Use the keyword `END` in the positional parameter to append a new line of text: 

```python
loremIpsum.insert(tk.END, "Lorem ipsum dolor sit amet,\nconsectetur adipiscing elit")
```


Further Reading
---------------

- G4G (4 Aug 2025). ["Python Tkinter"](https://www.geeksforgeeks.org/python/python-gui-tkinter/), Geeks for Geeks. 

- David Amos. ["Python GUI Programming: Your Tkinter Tutorial"](https://realpython.com/python-gui-tkinter/), Real Python. 

- Python Tutorial. ["Ttk Widgets"](https://www.pythontutorial.net/tkinter/tkinter-ttk/).

- Mark Roseman. ["Basic Widgets"](https://tkdocs.com/tutorial/widgets.html), tkdocs. 

- Mark Roseman. ["More Widgets"](https://tkdocs.com/tutorial/morewidgets.html), tkdocs. 



  
QED 

© Adam Heinz 

1 January 2026
