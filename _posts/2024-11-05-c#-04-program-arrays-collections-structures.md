AIDE MEMOIRE


Arrays, Collections and Structures with C\#
===========================================

Reference types differ from the value types previously discussed.
- *Value types* are initilised with one-part syntax `{type} {variableName};`, where the variable name points to a chunk of memory. 
- *Reference types* are initialised with two-part syntax `{ClassName} {variableName} = new {ClassName}();`, where the variable name references an object, and the `new` operator creates an object in memory and returns a reference to that object.



# Arrays

Arrays are reference type objects. They are indicated with square brackets. 

Indexing:
- elements can be accessed with the index number, beginning at zero.

Properties: 
- `.Length` returns the number of elements in the array. 


Consider this longform example which creates an array to hold 6 integers: 

```c#
// Initialise a variable of array type & Instantiate an array object associated it the variable
int[] numbers = new int[6];

double[] prices = new double[301];
decimal[] amounts = new decimal[25];
string[] names = new string[1500]
```

Once an array variable has been created, you can access its elements by index number, starting at zero:

```c#
const int MEDALS = 3;
string[] medallion = new string[MEDALS]; 

// Set values by index position 
medallion[0] = "Gold";
medallion[1] = "Silver";
medallion[2] = "Bronze";
```

The `.Length` property returns the number of elements in the array and can be used to loop through the array: 

```c#
double price; 
double[] prices = new double[25];

for (int index = 0; index < prices.Length; index ++)
{
    price = prices[index];
}
```

### foreach loop over array

If you just need to loop through every item in an array (i.e. without filtering criteria), 
```c#
double[] prices = new double[25];

foreach (int val in numbers)
{
    // Display in Message Box 
    ShowArray(var); 
}
```

## Two-Dimensional Arays

A two-dimensional array is indicated with a comma inside the square brackets `[,]` and requires row and column parameters: 

```c#
int rows = 2;
int cols = 3; 

int[,] items = new int[rows, cols];
```

The two-dimensional array has matching indexing matching rows and columns, e.g. `items[1, 2]`.

Use nested loops to iterate through all the row and column elements: 

```c#
const int SEARCH_ROWS = 2;
const int SEARCH_COLS = 3; 

int total = 0; // accumulator

// implicit sizing and initialization
int[,] items = {
    {0, 1, 2, 3}, 
    {4, 5, 6, 7},
    {8, 9, 10, 11} };

// sum the elements
for (int row = 0; row < SEARCH_ROWS; row++)
{
    for(int col = 0; col < SEARCH_COLS, col++)
    {
        total += items[row, col];
    }
}

// display the total
MessageBox.Show(total.ToString("c"));
```

# Lists

A list object automatically adjusts its size so you don't need to know how many elements you need to handle. 

The list declaration syntax uses angle brackets: `List<{type}> {variableName} = new List<{type}>();`. 

To initialise the list: 

```c#
List<int> myList = new List<int>() {1, 2, 3};
```

Indexing:
- items can be accessed with the index number, beginning at zero.

Properties: 
- `.Count` returns the number of items in the list.
- `.RemoveAt()` return success or failure in deleting an item at the given index position. 
- `.Clear()` truncates the list.
- `,IndexOf()` returns the index position of a search term, with optional start and end positions.


# Structures

Structures are declared by field: 
```c#
// Instantiate a Structure 
struct myVehicle
{
    public string make;
    public int year;
    public double mileage;
}

// Assign Values
myVehicle.make = "Ford Fiesta";

```

## Listing Structures

```c#
// Instantiate a structure object implicitly
Automobile sportsCar = new Automobile();

// Instantiate using `object initializer syntax`
Automobile truck = new Automobile() {
    make = "Dodge Ram",
    year = 1984,
    mileage = 9964.33};

// Store in list
List<Automobile> vehicles = New List<Automobile>();

// Add to list
vehicles.Add(sportsCar); 
vehicles.Add(truck);
```


# Enumerators

Note that these are constants, not strings.
```c#
enum moonPhase = {Waxing, Waning}; 

// declare a variable of that type
moonPhase mPhase;

// Assign a value to the variable
mPhase = moonPhase.Waking; 

// Get integer value (defaults to index)
int idx = int(moonPhase);
```


# Dictionaries

Access the dictionary class by `using Systesm.Collection.Generics;`.

Instantiate a dictionary with the syntax `Dictionary<KeyType, ValueType> dictName = new Dictionary<KeyType, ValueType>();`.

For example: 
```c#
Dictionary<int, string> customers = new Dictionary<int, string>() 
{
    {1, "Abermarle"}, 
    {2, "Beatrice"},
    {3, "Charleswood"}
};
```

or: 

```c#
Dictionary<int, string> customers = new Dictionary<int, string>(); 
customers[1] = "Abermarle"; 
customers[2] = "Beatrice";
customers[3] = "Charleswood";
```

Methods
- `.Add()`
- `.Remove()`
- `.ContainsKey()`
- `.ContainsValue()`
- `.Contains(KeyValuePair)`
- `.TryGetValue(Key, out myVariable)`
- `.Count`
- `.ElementAt(i)`
- `.Clear()`






QED 

© Adam Heinz 

6 November 2024

