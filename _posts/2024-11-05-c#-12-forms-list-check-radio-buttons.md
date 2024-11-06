AIDE MEMOIRE

List, Check, and Radio Buttons in C# Forms
==========================================

## List Box

List Box Properties:
- .Text 
- .Items
- .SelectedItem
- .SelectedIndex

A List Box as a `.SelectedItem` property that is indexed. The index defaults to -1, representing an unselected state. 

```c#
// Perform an action when item is picked
if (myListBox.SelectedIndex != -1)
{
    // do something 
    mySelection = myListBox.SelectedItem.ToString();
}
else
{
    // do something else
}
```



## Radio Button 

A Radio button control has a `.Checked` property that returns true or false.

```c#
// Perform an action when button is clicked
if (myRadioButton.Checked)
{
    // do something 
}
else
{
    // do something else
}
```

## Check Box

A Check Box control has a `.Checked` property that returns true or false. 

```c#
// Perform an action when box is ticked
if (myCheckBox.Checked)
{
    // do something 
}
else
{
    // do something else
}
```

## Check Changed

A Checked Changed event is raised whenever a Radio Button or Check Box control changes. 



QED 

© Adam Heinz 

6 November 2024
