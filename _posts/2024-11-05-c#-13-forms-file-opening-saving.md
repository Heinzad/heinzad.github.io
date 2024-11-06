AIDE MEMOIRE

Opening and Saving Files with C\#
=================================

The applicaion displays standard Windows dialog boxes for opening and saving files when using: 
- OpenFileDialog
- SaveFileDialog

## Displaying a Dialog control
On adding an OpenFileDialog or SaveFileDialog control to a form, it appears in the component tray at the bottom of the designer. 

Display the dialog box in your code with `.ShowDialog(this);`

## Detect Selection

Which button the user clicked is found with: 
- Open Button: `DialogResult.OK`
- Cancel Button: `DialogResult.Cancel`

```c#
if (openFile.ShowDialog() == DialogResult.OK)
{
    // Do Something
    MessageBox.Show("User clicked the open button")
}
else if (openFile.ShowDialog() == DialogResult.Cancel)
{
    // Do Something
    MessageBox.Show("User clicked the cancel button")
}
else 
{
    // Do Something
    MessageBox.Show("Nothing happened")
}
```

## Initial Directory

Display a directory with the property `.InitialDirector`

QED 

© Adam Heinz 

6 November 2024
