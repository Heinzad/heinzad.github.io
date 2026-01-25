---
title: "Using the VS Code PowerShell Terminal to Run Unit Tests with Python"
author: "Adam Heinz"
date: 2023-01-09T01:00:00-00:00
categories:
  - Python
---
How to solve the "ModuleNotFound" error when running Python's from terminal unittest within a project folder structure, according to Zed Shaw.  


# Overview 

The use of terminal and the project folder structure is advocated in Zed Shaw's [Learn Python 3 The Hard Way](https://learnpythonthehardway.org/python3/). 

However, this depended on using "nose" for unit testing, and at the end of chapter 46 "A Project Skeleton" is a message that might ruin your day: 

> "WARNING! At the time of publication I learned that the nose project has been abandoned and might not work very well. ... On Windows you may not have this problem, but using python -m "nose" will solve it if you do." 

The important bit is the "python -m" syntax, which allows [relative imports of test modules](https://peps.python.org/pep-0338/#import-statements-and-the-main-module). 

Without it, many attempts to run unit testing from the terminal will fail. 



## Project Directory Structure 


Zed Shaw advocates the following directory structure which we can apply to the automated testing example of exercise 48 (ex48): 

```
myprojects/ 
├── ex48/ 
|   ├── ex48/
|   |   ├── __ init __.py 
|   |   └── lexicon.py
|   ├── bin/ 
|   ├── docs/
|   ├── tests/
|       ├── __ init __.py
|       └── test_lexicon.py 
└── ...
```


### Lexicon Script 


The the lexicon.py file in exercise 48 can be scripted as follows: 

```python 

""" 
Title: Learn Python the Hard Way. Ex 48. Advanced User Input 
Author: Zed Shaw 
Script Name: lexicon.py
Script Developer: Heinzad 
Script Dated: 9 Jan 2023 
Script Description: Checks if user-supplied strings contain words 
accepted in a game's lexicon. 
""" 

# Build dictionary of words that are accepted in game answers 

lexica = {
    # directions 
    'north':'direction',
    'south':'direction',
    'east':'direction',
    'west':'direction',
    'down':'direction',
    'up':'direction',
    'left':'direction',
    'right':'direction',
    'back':'direction',
    
    # verbs 
    'go':'verb',
    'stop':'verb',
    'kill':'verb',
    'eat':'verb',
    
    # stops 
    'the':'stop',
    'in':'stop',
    'of':'stop',
    'at':'stop',
    'it':'stop', 
    
    # nouns 
    'door':'noun',
    'bear':'noun',
    'princess':'noun',
    'cabinet':'noun',
    } 


def convert_number(n): 
	""" Returns an integer or nothing if n is unconvertable """ 
    try: 
        return int(n) 
    except ValueError: 
        return None  

    
def dicta_scan(sentance): 
    """ Returns a (value, key) tuple from a dictionary 
	where a word in the sentance is in the game's lexicon. 
    >>> dicta_scan("north")
    ('direction', 'north')   
    >>> dicta_scan("go")
    ('verb', 'go')   
    >>> dicta_scan("the")
    ('stop', 'the')   
    >>> dicta_scan("bear")
    ('noun', 'bear')   
    >>> dicta_scan("1234")
    ('number', 1234)    
    >>> dicta_scan("ASDFADFASDF")
    ('error', 'ASDFADFASDF')   
    """ 
    
    words = sentance.split() 
    default = "___" 
    result = default 
    for word in words: 
        
        lex_key = lexica.get(word, default)  
        
        if lex_key != default: 
            lex_val = lexica[word] 
            return(lex_val, word) 
        
        elif word.isnumeric() and convert_number(word): 
            return('number', int(word)) 
        
        else: 
            return('error', word)    


if __name__ == "__main__":
    import doctest
    doctest.testmod() 

``` 


--- 
### Lexicon Test Script 

The the test_lexicon.py file in exercise 48 can be scripted using unittest as follows: 

```python 


""" 
Title: Learn Python the Hard Way. Ex 48. Advanced User Input 
Author: Zed Shaw 
Script Name: test_lexicon.py
Script Developer: Heinzad 
Script Dated: 9 Jan 2023 
Script Description: Runs unit testing on the lexicon module.  
""" 


import unittest
from ex48.lexicon import dicta_scan


class TestDicta(unittest.TestCase): 
    # tests the lexica scan  
    
    def test_direction_scan(self): 
        expected = ('direction', 'north') 
        actual = dicta_scan("north") 
        self.assertEqual(expected, actual, "'north' is a direction")
        
    def test_verb_scan(self): 
        expected = ('verb', 'go') 
        actual = dicta_scan("go") 
        self.assertEqual(expected, actual, "'go' is a verb")    
        
    def test_stops_scan(self): 
        expected = ('stop', 'the') 
        actual = dicta_scan("the") 
        self.assertEqual(expected, actual, "'the' is a stop")
        
    def test_nouns_scan(self): 
        expected = ('noun', 'bear') 
        actual = dicta_scan("bear") 
        self.assertEqual(expected, actual, "'bear' is a noun") 
        
    def test_number_scan(self): 
        expected = ('number', 1234) 
        actual = dicta_scan("1234") 
        self.assertEqual(expected, actual, "'1234' is a number")
        
    def test_error_scan(self): 
        expected = ('error', 'ASFADFASDF') 
        actual = dicta_scan("ASFADFASDF") 
        self.assertEqual(expected, actual, "'ASFADFASDF' is an error")
   

if __name__ == '__main__':
	unittest.main() 

```


--- 
### Terminal 


We are now in a position to run unittest from the PowerShell terminal using the -m flag: 

```powershell 

cd myprojects
cd ex48 
python -m unittest tests\test_lexicon.py 

``` 

With any luck we get an output of "......": 

> Ran 6 tests in 0.001s 





QED 

© Adam Heinz 

9 January 2023 
 
