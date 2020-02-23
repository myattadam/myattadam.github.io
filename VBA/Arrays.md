# Arrays
You can use the ``Array`` keyword for prefilling arrays, for example ``Array("A", "B", "C")`` to create a one-dimensional array, or ``Array(Array(1, 2, 3), Array(4, 5, 6))`` to create a staggered array (an array of arrays).

You can also use the ``Evaluate`` keyword to create an array, ``Evaluate("{1, 2, 3; 4, 5, 6}")`` or the shorthand alternative, ``[{1, 2, 3; 4, 5, 6}]``, using a semicolon to create a multidimensional array. Note that when creating multidimensional array with this method, the array needs be balanced.

The evaluate keyword, when used to create an array, returns an array object so the returned value can be accessed like an array, like this: ``Evaluate("{1, 2, 3}")(1) = 2`` but you can't do the same using the shorthand method as the array is not created via a function.


Multidimensional arrays built using the evaluate function can be iterated over using a `For Each ... Next` loop:
```VB
a = Evaluate("{1, 2, 3; 4, 5, 6}") ' OR [{1, 2, 3; 4, 5, 6}]

For Each i In a
    Debug.Print i
Next

' Outputs:
' a(1, 1) = 1
' a(2, 1) = 4
' a(1, 2) = 2
' a(2, 2) = 5
' a(1, 3) = 3
' a(2, 3) = 6
```

Using ``Transpose`` on an array flips the array X/Y:
```VB
a = Application.Transpose(a)

For Each i In a
    Debug.Print i
Next

' Outputs:
' a(1, 1) = 1
' a(2, 1) = 2
' a(3, 1) = 3
' a(1, 2) = 4
' a(2, 2) = 5
' a(3, 2) = 6
```
Using a ``For ... Next`` loop doesn't work the same way because of the multiple dimensions; you need a loop for each dimension of the array.


the following using the ``Array()`` function however _does not_ work, because the Array keyword creates a staggered array `(1)(1)` rather than a multidimensional array `(1,1)`:
```    
  a = Array(Array(1, 2, 3), Array(4, 5, 6))
  
  For Each i In a
      Debug.Print i
  Next
```

#### Sorting arrays
This function takes an one-dimensional or an staggered array, sorts them ascendingly, and passes back the sorted array. If you are passing a one-dimensional array, there's no need to specify the column to sort by (`byColumn`).

```basic
Function QuickSort(arr As Variant, Optional byColumn As Long = -1) As Variant
    Dim left As Variant
    Dim right As Variant
    Dim pivot As Variant
    
    Dim i As Long
    
    If Arrays.Length(arr) > 1 Then
    
        pivot = arr(UBound(arr))
        
        For i = LBound(arr) To UBound(arr) - 1
            If byColumn = -1 Then
        
                If arr(i) <= pivot Then
                    Arrays.Append left, arr(i)
                Else
                    Arrays.Append right, arr(i)
                End If
                
            Else
            
                If arr(i)(byColumn) <= pivot(byColumn) Then
                    Arrays.Append left, arr(i)
                Else
                    Arrays.Append right, arr(i)
                End If
                
            End If
        Next
        
        QuickSort left, byColumn
        QuickSort right, byColumn
    
        arr = Empty
        
        If Not IsEmpty(left) Then
            For i = LBound(left) To UBound(left)
                Arrays.Append arr, left(i)
            Next
        End If
        
        Arrays.Append arr, pivot
        
        If Not IsEmpty(right) Then
            For i = LBound(right) To UBound(right)
                Arrays.Append arr, right(i)
            Next
        End If
        
    End If
    
    QuickSort = arr
End Function
```

Example:
```basic
QuickSort arr, 1  ' By letter

'Input:  [[99,"D"],[1,"S"],[10,"P"],[79,"D"],[4,"H"],[38,"I"],[94,"I"],[40,"Z"],[16,"H"],[64,"E"],[41,"L"],[32,"T"],[20,"Q"],[58,"F"],[45,"C"],[26,"Y"],[37,"U"],[91,"I"],[62,"Q"],[9,"L"]]
'Output: [[45,"C"],[99,"D"],[79,"D"],[64,"E"],[58,"F"],[4,"H"],[16,"H"],[38,"I"],[94,"I"],[91,"I"],[41,"L"],[9,"L"],[10,"P"],[20,"Q"],[62,"Q"],[1,"S"],[32,"T"],[37,"U"],[26,"Y"],[40,"Z"]]


QuickSort arr, 0 ' By number
'Input: [[10,"C"],[12,"J"],[53,"A"],[54,"R"],[8,"W"],[67,"F"],[35,"M"],[70,"E"],[53,"Y"],[75,"C"],[46,"K"],[20,"N"],[9,"J"],[16,"P"],[9,"Y"],[27,"M"],[75,"X"],[67,"H"],[8,"H"],[32,"B"]]
'Output: [[8,"W"],[8,"H"],[9,"J"],[9,"Y"],[10,"C"],[12,"J"],[16,"P"],[20,"N"],[27,"M"],[32,"B"],[35,"M"],[46,"K"],[53,"A"],[53,"Y"],[54,"R"],[67,"F"],[67,"H"],[70,"E"],[75,"C"],[75,"X"]]
```
