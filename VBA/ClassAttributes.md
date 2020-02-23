# Class attributes

The class will need to be exported to a .cls file, as `attributes` aren't visible in the VBA editor. You can only have a default property or a default procedure, not both.

### Default property of class
```basic
' myclass.cls
Property Get Value(index as variant) as variant
  Attribute Value.VB_UserMemId = 0
  Value = somearray(index)
End Property

' module.bas
Dim a = New MyClass()
a(1) = "Hello"
a(2) = "There"
```

### Default procedure of class
```basic
' myclass.cls
Function AddOne()
  Attribute Value.VB_UserMemId = 0
  number = number + 1
  Debug.Print "Number is now " & number
End Function
```
