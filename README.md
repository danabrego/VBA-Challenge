First thing first need to set variables (qtr change, %change, total volumes, ticker, market open, market close and need a counter for volume as i go through each ticker)

add column fields (table 1) to summarize the stocks
 -need to add each day in the qtr for stock to get total volume
 - get market open and market close for 1st and last day
 - format new columns to display as percentage "0.00%"
 - calculate % change


add column fields (table 2) for greatest increase, greatest decrease, and overall highest volume
  - format fields for percentage "0.00%
  - find biggest increase, biggest decrease, and biggest over all volume

conditional format for anything over 0 change in green and anything less than 0 in red

run the script through each worksheet (ws)

screenshot of final result
![image](https://github.com/danabrego/VBA-Challenge/assets/162066812/06436e67-7d2b-4560-8c86-7ade470ba110)

Resource:
Turor training was scheduled with brandon who helped with my test file 
Visual Code searched vba scripts from previous lessons
google
stackoverflow

I was not able to save as a VBS file so placed the VBA script here

Sub Stock_data()

'variable list

Dim Ticker As String

Dim Marketopen As Double

Dim Marketclose As Double

Dim QtrChange As Double

Dim TotalVolume As Double

Dim PChange As Double

Dim datapoint As Integer

Dim ws As Worksheet

For Each ws In Worksheets

 'table 1 layout

    ws.Range("I1").Value = "Ticker"
    ws.Range("J1").Value = "Qtr Change"
    ws.Range("K1").Value = "% Change"
    ws.Range("L1").Value = "Total Volume"

    'counter start
    datapoint = 2
    previous_i = 1
    TotalVolume = 0

    LastRow = ws.Cells(Rows.Count, "A").End(xlUp).Row

        For i = 2 To LastRow

            If ws.Cells(i + 1, 1).Value <> ws.Cells(i, 1).Value Then

            Ticker = ws.Cells(i, 1).Value

            previous_i = previous_i + 1

            Marketopen = ws.Cells(previous_i, 3).Value
            Marketclose = ws.Cells(i, 6).Value

        
            For j = previous_i To i

                TotalVolume = TotalVolume + ws.Cells(j, 7).Value

            Next j

            If Marketopen = 0 Then

                PChange = Marketclose

            Else
                QtrChange = Marketclose - Marketopen

                PChange = QtrChange / Marketopen

            End If

            ''values
            
            ws.Cells(datapoint, 11).NumberFormat = "0.00%"
            ws.Cells(datapoint, 12).Value = TotalVolume
            ws.Cells(datapoint, 9).Value = Ticker
            ws.Cells(datapoint, 10).Value = QtrChange
            ws.Cells(datapoint, 11).Value = PChange


            datapoint = datapoint + 1

        'reset counter
            TotalVolume = 0
            QtrChange = 0
            PChange = 0

            previous_i = i

        End If

    Next i

    lastRow2 = ws.Cells(Rows.Count, "K").End(xlUp).Row

    Increase = 0
    Decrease = 0
    Greatest = 0

        For k = 3 To lastRow2

  
            last_k = k - 1

            currentchange = ws.Cells(k, 11).Value

            previouschange = ws.Cells(last_k, 11).Value

            volume = ws.Cells(k, 12).Value

            prevvol = ws.Cells(last_k, 12).Value

' calculate decrease, increase and volume

            If Increase > currentchange And Increase > previouschange Then

                Increase = Increase

            ElseIf currentchange > Increase And currentchange > previouschange Then

                Increase = currentchange

                increase_name = ws.Cells(k, 9).Value

            ElseIf previouschange > Increase And previouschange > currenchange Then

                Increase = previouschange

                increase_name = ws.Cells(last_k, 9).Value

            End If

            If Decrease < currentchange And Decrease < previouschange Then

                Decrease = Decrease


            ElseIf currentchange < Increase And currentchange < previouschange Then

                Decrease = currentchange


                decrease_name = ws.Cells(k, 9).Value

            ElseIf previouschange < Increase And previouschange < currentchange Then

                Decrease = previouschange

                decrease_name = ws.Cells(last_k, 9).Value

            End If

            If Greatest > volume And Greatest > prevvol Then

                Greatest = Greatest


            ElseIf volume > Greatest And volume > prevvol Then

                Greatest = volume

                greatest_name = ws.Cells(k, 9).Value

            ElseIf prevvol > Greatest And prevvol > volume Then

                Greatest = prevvol

                greatest_name = ws.Cells(last_k, 9).Value

            End If

        Next k

'table 2

    ws.Range("N2").Value = "Greatest % Increase"
    ws.Range("N3").Value = "Greatest % Decrease"
    ws.Range("N4").Value = "Greatest Total Volume"
    ws.Range("O1").Value = "Ticker"
    ws.Range("P1").Value = "Value"

    ws.Range("O2").Value = increase_name
    ws.Range("O3").Value = decrease_name
    ws.Range("O4").Value = greatest_name
    ws.Range("P2").Value = Increase
    ws.Range("P3").Value = Decrease
    ws.Range("P4").Value = Greatest

    ws.Range("P2").NumberFormat = "0.00%"
    ws.Range("P3").NumberFormat = "0.00%"


    lastRow3 = ws.Cells(Rows.Count, "J").End(xlUp).Row


        For j = 2 To lastRow3

            If ws.Cells(j, 10) > 0 Then

                ws.Cells(j, 10).Interior.ColorIndex = 4

            Else

                ws.Cells(j, 10).Interior.ColorIndex = 3
            End If

        Next j

Next ws
End Sub

