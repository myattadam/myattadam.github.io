# Dictionaries

### [Dictionary to JSON](https://github.com/myattadam/VBA-tools/blob/master/DictToJSON.bas)

[Example file](dictionary_example.xlsm)

This simple set of functions converts a nested dictionary structure to JSON format.
```VB
Sub saveJSON(filename As String, entity As Variant)
```
Saves the JSON file to the same location as the active workbook.

```VB
Function toJSON(entity As Variant) As String
```
A recursive function that converts a nested dictionary structure (and it's contents, whether they veriables, arrays, other dictionarys, etc.) to a JSON string.

A nested dictionary structure can be created as follows:
```VB
Dim lo As ListObject                  ' A ListObject is basically just an Excel table
Dim UID As String                     ' Our unique identifier

set record As Range                   ' Each record we're going to read is a row in our Excel table
Set data = New Dictionary             ' Where we're going to save our data

Set lo = Me.ListObjects(DATATABLE)    ' This example pulls records from a data table, but it could equally be via a query

For Each record In lo.DataBodyRange.Rows    ' Loops through 
    
    UID = Intersect(record, Me.Range(DATATABLE & "[UID]")).Value2
    
    If Not data.Exists(code) Then
        Set data(UID) = New Dictionary
            data(UID)("Code") = UID
            data(UID)("Client Name") = Intersect(record, Me.Range(DATATABLE & "[Client Name]")).Value2
        Set data(UID)("Transactions") = New Dictionary
            data(UID)("Total") = 0
    End If
                      
    period = Intersect(record, Me.Range(DATATABLE & "[Date]")).Value2
    
    data(code)("Transactions")(period) = Intersect(record, Me.Range(DATATABLE & "[Amount]")).Value2
    data(code)("Total") = data(code)("Total") + data(code)("Transactions")(period)
Next
```


### Excel notes
* When pulling data from a table in a dictionary, erroneous cell values will not be entered; you need to perform an ``IsError(value)`` check and apply an alternative if true.
