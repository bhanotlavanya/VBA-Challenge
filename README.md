# VBA-Challenge

Sub Stock_Data_Analysis()
First we have to declare all the variables which we are going to use
'(Ticker Symbol, Open Price, Closing Price, Percentage Change, Total Stock Volume,Quarterly Change, Greatest Total Volume)
Dim ticker As String
Dim open_price As Double
Dim closing_price As Double
Dim pc As Double
Dim tsv As Double
Dim qc As Double
Dim gtv As Double
'Other variables
Dim PreviousStockPrice As Long
Dim table_summary_row As Long
Dim greatest_increase As Double
Dim greatest_decrease As Double
'Declare Worksheet as "ws" and Loop through each worksheet
Dim ws As Worksheet
For Each ws In Worksheets
'Label Column Headers and Tables
ws.Range("P1").Value = "Ticker"
ws.Range("Q1").Value = "Value"
ws.Range("O2").Value = "Greatest % Increase"
ws.Range("O3").Value = "Greatest % Decrease"
ws.Range("O4").Value = "Greatest Total Volume"
ws.Range("I1").Value = "Ticker"
ws.Range("J1").Value = "Yearly Change"
ws.Range("K1").Value = "Percent Change"
ws.Range("L1").Value = "Total Stock Volume"

Here we will be finding Quarterly Change, Percent Change, and Total Stock Volume for each stock

and here we will be assigning variables for loop to start 
tsv = 0
table_summary_row = 2
PreviousStockPrice = 2

'Setting value of the last row for column A
EndRowA = ws.Cells(Rows.Count, 1).End(xlUp).Row

'Loop through first rows for stock info
For i = 2 To EndRowA

        'Finding the value of the Total Stock Volume
        tsv = tsv + ws.Cells(i, 7).Value
        
        'Executing to record for change in stock ticker in the summary table with ticker name and tsv and reset tsv back to zero
        If ws.Cells(i + 1, 1).Value <> ws.Cells(i, 1).Value Then
        
                ticker = ws.Cells(i, 1).Value
                
                ws.Range("I" & table_summary_row).Value = ticker
                
                ws.Range("L" & table_summary_row).Value = tsv
                
                tsv = 0
                
                'Here we will be finding the quarterly open and close price, quarterly change, and percentage change
                open_price = ws.Range("C" & PreviousStockPrice)
                
                close_price = ws.Range("F" & i)
                
                yc = close_price - open_price
                
                ws.Range("J" & table_summary_row).Value = yc
                
                'Starting another if statement to determine percent change
                If open_price = 0 Then
                
                    pc = 0
                    
                Else
                    open_pice = ws.Range("C" & PreviousStockPrice)
                
                    pc = yc / open_price
                    
                End If
                
                'Place value of percentage change in summary table using the % format
                ws.Range("K" & table_summary_row).Value = pc
                
                ws.Range("K" & table_summary_row).NumberFormat = "0.00%"
                
                'Make another if statement for conditional formating the cells of yearly change (green=positive/red=negative)
                If ws.Range("J" & table_summary_row).Value >= 0 Then
                    ws.Range("J" & table_summary_row).Interior.ColorIndex = 4
                    
                Else
                    ws.Range("J" & table_summary_row).Interior.ColorIndex = 3
                    
                End If
                
                'Initiate task to go to next row for summary table and previous stock price
                table_summary_row = table_summary_row + 1
                
                PreviousStockPrice = i + 1
                
            End If
            
            Next i

'Making another loop for Greatest % Increase, Greatest % Decrease, and Greatest Total Volume

'Assign values to variables for loop to start
gtv = 0
greatest_increase = 0
greatest_decrease = 0

'Set value of the last row for column K
EndRowK = ws.Cells(Rows.Count, 11).End(xlUp).Row

For i = 2 To EndRowK

    'First determine the Greatest Total Volume
    If ws.Range("L" & i).Value > gtv Then
       gtv = ws.Range("L" & i).Value
       ws.Range("Q4").Value = gtv
       ws.Range("P4").Value = ws.Range("I" & i).Value
       
    End If
    
    'Next determine Greatest % Increase
    If ws.Range("K" & i).Value > greatest_increase Then
        greatest_increase = ws.Range("K" & i).Value
        ws.Range("Q2").Value = greatest_increase
        ws.Range("P2").Value = ws.Range("I" & i).Value
        
    End If
    
    'Last determine Greatest % Decrease
    If ws.Range("K" & i).Value < greatest_decrease Then
        greatest_decrease = ws.Range("K" & i).Value
        ws.Range("Q3").Value = greatest_decrease
        ws.Range("P3").Value = ws.Range("I" & i).Value
        
    End If
    
    'Change format to "%" for Greatest % Increase and Decrease
    ws.Range("Q2").NumberFormat = "0.00%"
    
    ws.Range("Q3").NumberFormat = "0.00%"
    
Next i

'Then excute loops to next worksheet
Next ws

        
End Sub
