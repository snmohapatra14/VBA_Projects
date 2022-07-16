# VBA_Projects
# VBA code to merge data of all sheets in an excel file with under a common header 
Sub MERGEDATAEXP_SN()

Dim myheaders As Range
'myheadrs is the header that needs to be selected by the user after the msgbox pops up

Set myheaders = Application.InputBox(prompt:="Please provide the header that is common to each sheet", Type:=8)
'The input for the inputbox should be the header range selected by the user when the message pops up

Dim strowcell As Variant
strowcell = InputBox("Provide the starting cell name from where the data will be extracted for merging" & vbNewLine & "for ex: B3", vbInformation)
'The strowcell means starting row cell i.e., the cell name needs to be defined, starting from which data will be extracted, cell name needs to be defined in R1C1 style

Dim stcontcell As String
stcontcell = InputBox("Provide the starting cell name of any input column that doesn't have any blank cell" & vbNewLine & "for ex: C3")
'The stcontcell means starting continuous cell i.e., the cell name in any column which can act as aprent column or any column which has no blank cell in the data, cell name needs to be defined in R1C1 style

Worksheets.Add after:=Worksheets(Worksheets.Count)
ActiveSheet.name = "Merged"

Dim destinationheader As Range
Set destinationheader = Worksheets("Merged").Range("A1")
'Here the default destination range is chosen as A1. if it needs to be changed to any other cell, the same needs to be changed in subsequent codes. In case the user wants to paste all the data in cell B1 of merged sheet, then B1 needs to be written as "Set destinationheader = Worksheets("Merged").Range("B1")"

myheaders.Copy destinationheader
'The code is to copy the selected header to the destination range in the "Merged" worksheet

Dim n As Integer
Dim m As Range
Dim endcell As String
'endcell represents the last assumed cell number for this project and here it is taken upto column zz

Dim h As Integer
h = 0
'H is used as a helper to define the starting row at which the next sheet data needs to be pasted after each for loop

Dim stmergecell As String
Dim myws As Worksheet

For Each myws In Worksheets
If myws.name = "Merged" Then Exit For
n = myws.Range(stcontcell).End(xlDown).Row
endcell = "ZZ" & n
'endcell means expected last cell in any input sheet, if there are more columns in each sheet with data, then "zz" can be extended as per need

Set m = myws.Range(strowcell, endcell)
'm is the range in any input sheet which will be copied to merged sheet

stmergecell = "A" & myheaders.Rows.Count + h+ 1
'Stmergecell indicates the cell at which the next sheet data needs to pasted after each for loop. If destinationheader cell has been changed from A1 to B1(Suppose), then the above code should be changed to (stmergecell= "B" & myheaders.Rows.Count + 1)

m.Copy Worksheets("merged").Range(stmergecell)
h = m.Rows.Count + h

Next myws

End Sub
