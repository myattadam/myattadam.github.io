# VBA tools and macro development

## General VBA

[Microsoft VBA overview](https://docs.microsoft.com/en-us/office/vba/api/overview/)




### Console output
Spacing out results with ``Tab()`` and ``Spc()``:
```basic
Debug.Print "ABC"; Tab(20); "DEF"; Tab(25); "GHI"
Debug.Print "ABC"; Spc(20); "DEF"; Spc(25); "GHI"
```
Outputs:
```basic
ABC                  DEF  GHI
ABC                    DEF                         GHI
```

### Dates
* To get the current date, use `Date`, to get the current date and time, use `Now`
* Dates as variables can be entered in the following format: ``#yyyy/mm/dd#`` or ``#mm/dd/yyyy#``. Regardless of what method is used, VBA will automatically default to ``#mm/dd/yyyy#``.



### Subroutines and Functions
#### Parameter arrays
```basic
Sub AnyNumberArgs(strName As String, ParamArray intScores() As Variant) 
 Dim intI As Integer 
 
 Debug.Print strName; " Scores" 
 ' Use UBound function to determine upper limit of array. 
 For intI = 0 To UBound(intScores()) 
 Debug.Print " "; intScores(intI) 
 Next intI 
End Sub

AnyNumberArgs "Jamie", 10, 26, 32, 15, 22, 24, 16 
AnyNumberArgs "Kelly", "High", "Low", "Average", "High"
```

### Miscellaneous
Remember when using the ``IIF`` function, that it will evaluate both parts of the _true_ and _false_ arguements. If there's a possiblity of returning an error from either of these, don't use this function.

#### The Static keyword
The ``Static`` keyword on a function variable 'remembers' what it contains even after exiting a function:

```vbscript
Function Records() as Dictionary
  Static data As Dictionary

  If data Is Nothing Then
    Set data = New Dictionary
    ' Read data
  Else
    ' Update data
  End If

  Set Records = data
End Function
```
On first calling `Records()`, the function first checks to see if there's anything assigned to `data` and if not, creates and assigns a dictionary object. On exiting the function, it returns the `data` object. Calling `Records()` a second time, the function remembers that `data` has already been assigned and updates and returns `data` instead.

You can use `Static` to create psuedo-objects. This function lets you keep track of the maximum values that gets passed to it. If `index` is -1, then it assumes you want to reset the lists. Otherwise, if `value` is `Empty`, then it returns whatever's at that index as an array of the code and index, otherwise it compares the value to what's in the array at index, and replaces if the new value is higher.
```basic
Private Function MaxValues(Optional index As Long = -1, Optional value As Variant = Empty, Optional code As String = "") As Variant
    Static values() As Variant
    Static codes() As Variant
    
    If index = -1 Then
        ReDim values(1 To QUARTERS)
        ReDim codes(1 To QUARTERS)
    Else
    
        If IsEmpty(value) Then
            MaxValues = Array(codes(index), values(index))
        Else
            If value > values(index) Then
                values(index) = value
                codes(index) = code
            End If
        End If
    End If
End Function

MaxValues                 ' Clears the data
MaxValues(1)(1)           ' Returns the code of whatever is at index 1
MaxValues 1, 5.7, "ABC"   ' Compares whats at index 1 with 5.7, and replaces if the value is higher
```

## Excel

### Data Tables
Reference|Range|Table
--|--|--
Relative|`A1`|`[FIELD]`
Absolute (column & row)|`$A$1`|`[[FIELD]:[FIELD]]`
Absolute (column)|$A1|
Absolute (row)|A$1|



## Excel VBA
When plotting series, use a ``variant`` array to set the values. For missing data, use an ``Empty`` value and Excel will ignore plotting the point (Do not use ``Null`` as this can cause type mismatch errors).




### [datepicker.xlsm](https://github.com/myattadam/VBA-tools/blob/master/datepicker.xlsm)
I built this as a 'native' date picking tool as Excel 2010 doesn't come with a standard form control.
To use it in a workbook, copy the form and class to the workbooks project tree. To open the form, call _frmDatePicker.showForm Range("A1")_, replacing A1 with the destination cell the date needs to be written to.

_Note: Still needs a bit of work. I want to remove the hard coding for some of the values that set the colour of the buttons, etc._

### [shape_tools.ppam](https://github.com/myattadam/VBA-tools/blob/master/shape_tools.ppam)
This add-in currently contains a tool for copying and pasting just the size and position of a shape or range of shapes. The order in which you select the shapes to copy (shift click a shape to select more than one at a time) determines the order in which the size and positions are applied. Use this tool to quickly grab a layout of a slide and reapply it with a bunch of other shapes (including tables).

