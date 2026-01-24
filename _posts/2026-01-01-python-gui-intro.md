---
layout: post
tags: Python
author: "Adam Heinz"
date: 2026-01-01 10:00:00
---

AIDE MEMOIRE

Python GUI Introduction
=======================


Tkinter is part of the Python standard library. It is a wrapper around the Tcl/Tk GUI. Use it to build user interfaces.

Load the library:

```python
import tkinter as tk
```

Load themed widgets: 

```python
from tkinter import ttk
```


Window
------

### Begin by instantiating a Tk() class to create a main window: 

Parameters: 

- `screenName` : string (optional). 
    Nominate which screen to use.  
- `baseName` : string (optional).
    Application name (defaults to the name of the script).    

- `className` : string (optional). 
    Name of the window class to be used for stying and window manager settings.  

- `useTk` : boolean (optional). 
    Indicates whether or not to intitialise the Tk subsystem. (Defaults to 1).


Instantiate an object of the Tk class: 

```python
win = tk.Tk() 
```

### Add widgets to the window:

- `tk` creates a classic widget. 
- `ttk` creates a themed widget. 

```python
hello_world = ttk.Label(win, text='Hello, World!')
```

Display the label widget in the window:

- `pack()` resizes the window to the minimum bounds of the widget. 

```python
hello_world.pack()
```


### End with the event loop:  

```python
win.mainloop() 
```


Further Reading
---------------

- G4G (4 Aug 2025). ["Python Tkinter"](https://www.geeksforgeeks.org/python/python-gui-tkinter/), Geeks for Geeks. 

- David Amos. ["Python GUI Programming: Your Tkinter Tutorial"](https://realpython.com/python-gui-tkinter/), Real Python. 

- Python Tutorial. ["Ttk Widgets"](https://www.pythontutorial.net/tkinter/tkinter-ttk/).



  
QED 

© Adam Heinz 

1 January 2026
