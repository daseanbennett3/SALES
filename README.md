--Create CY Sales IF YEAR([Order Date]) = [Select Year] THEN [Sales] END
--Create PY Sales IF YEAR([Order Date]) = [Select Year] - 1 THEN [Sales] END 

--Display a summary of total number of customers , total sales per customer and total number of orders for the current year and the previous year:
YEAR(Order Date) as the Row
Measure Values of SUM(sales), SUM(Calculated Field[IF YEAR(Order Date) = 2023 THEN Sales END]), and SUM(Calcualted Field[IF YEAR(Order Date) = 2022 THEN Sales END])
Calculated Field[YEAR(Order Date)]
--The Dashboard should allow users to check historical data by offering them the flexibility to select any desired year.
Create Parameter named Select Year with Data Type(Integer) - Allowable Values - Add Values From - Display Format - Okay - select the parameter - Show Parameter
Change Calulated Parameter to [IF YEAR(Order Date) = Select Year THEN Sales END] for the current year and [IF YEAR(Order Date) = Select Year - 1 THEN Sales END] to get the previous year

--Build The KPI Sales Worksheet
Create a Calculated Field called "% Diff Sales" that shows the percentage difference of sales ((SUM([CY Sales]) - SUM([PY Sales])) / SUM([PY Sales]))
Change the Default Properties - Number Format to Percentage
Remove the Year (Order Date) and add % Diff Sales to the Measure Values

In a new worksheet, add SUM(CY Sales) & % Diff Sales to the Detail
Double click the title (KPI Sales) and add a sheet title and insert the two details above (% Diff Sales & SUM(CY Sales))
Double-click on "AGG(% Diff Sales)" with the Format - Pane (use ▲ 0.00%; ▼ -0.00%; as a custom format)

--Present the data for each KPI on a monthly basis for both the current year and the previous year. (SparkLine)
Add the Select Year parameter to the page if necessary (Show Parameter)
Add Order Date to the Columns and switch from YEARS to Months with drop down menu 
Add SUM(CY Sales) to the Row then change the Order Date from Discrete to continuous
Add SUM(PY Sales) to the Y axis of the chart

--Highlight the HIGHEST AND LOWEST point on the Sparkline Chart
Create a Calculated Field called "MIN/MAX Sales"(IF SUM([CY Sales]) = WINDOW_MAX(SUM([CY Sales])) THEN SUM([CY Sales]) ELSEIF SUM([CY Sales]) = WINDOW_MIN(SUM([CY Sales])) THEN SUM([CY Sales]) END)
Add the Mix/Max you just created to the Rows
Change the Min/Max to a Circle and change the Measured Values to a Line instead of Automatic for both
Change Min/Max in the Rows to Dual Axis AND Synchronize Axis if necessary

--Fix the title so it's not a range that reacts to what is below it (another way coould be to do the Sparkline on a different sheet)
In the All tab (Same place where you find Measured Values), add a "}" to the end and a "{" begining of both values (SUM of CY Sales and % Diff Sales)
Double click the title and re-Insert the two tabs that were just adjusted

--To change the dot colors
Control and drag the "Min/Max Sales" from the Rows to the Colors tab on the left side (Same place where you find Measured Values).
Change Automatic drop down to "Diverging Colors" and change the "Stepped Colors" to "2 Steps"

--Create a parameter for the current year and the previous year in order to use it in the Tool Tip (The Tool Tip is the dots on the grap you can hover over to see more information)
Create a Calculated Field called "Current Year" (SELECT (YEAR)) and another one called "Previous Year" (SELECT YEAR -1), then Convert them to Dimensions
Add the previous year, current year, current sales, previous sales, and the % Diff Sales to the Tool Tip
Double click Tool Tip and inseret which parameters you would like to display
Tool Tip should look like this {--Sales of<MONTH(Order Date)>, <ATTR(Current Year)>:<SUM(CY Sales)>
                                  Sales of<MONTH(Order Date)>,<ATTR(Previous Year)>:<SUM(PY Sales)>
                                  Sales DIfferences:<AGG(% Diff Sales)>
                                  Highest/Lowest Sales:<AGG(MIN/MAX Sales)>--}

Adjust the ToolTip to make it more reable with different coloring and BOLDing

--Subcategory Comparison Graph (New Worksheet)
Drag & drop the Sub-Category to the Rows
Drag & drop CY Sales and PY Sales to the Colummns
In the Marks card (Previously known as measured values) change the drop down menu from Automatic to Bar for both graphs
Right-click CY Sales in the Column and change to Dual Axis
Syncronize Axis on the CY Sales
Add CY Profit to the Columns
Hide CY Sales Header and Show Parameter Select Year
Adjust Grid Lines, Font of the axis' and Bar sizes
Control and Drag CY Profits to the Colors in the Marks card and do the 2 Step Color Divirging

Create a Calculated Field called KPI CY less than PY (IF SUM([CY Sales]) < SUM([PY Sales]) THEN '*' ELSE '$' END)
Drag the Calculated Field to the Rows
Right-click Sub-Category in the Columns, Sort By: Field - Descending - Field Name: CY Sales

In the Marks card, under All, add CY Profit, CY Sales, Current Year, PY Sales, Previous Year, % Diff Sales to the Tool Tip
ToolTip should look like this >>> Sub-Category:	<Sub-Category>
                                  Sales of <ATTR(Current Year)>: <SUM(CY Sales)>
                                  Sales of <ATTR(Previous Year)>: <SUM(PY Sales)>
                                  Sales Difference: <AGG(% Diff Sales)>
                                  Profit of <ATTR(Current Year)>: <SUM(CY Profit)>
                                  
Format the numbers in the Tool Tip by Right-clicking on the thing you wanna change in the Marks card under Numbers in the Default Pane

--Weekly Trends (New Worksheet)
Add Order Date to the Columns, change from YEAR to More -- Week Number and then make it continuous

