--Display a summary of total number of customers , total sales per customer and total number of orders for the current year and the previous year:
YEAR(Order Date) as the Row
Measure Values of SUM(sales), SUM(Calculated Field[IF YEAR(Order Date) = 2023 THEN Sales END]), and SUM(Calcualted Field[IF YEAR(Order Date) = 2022 THEN Sales END])
Calculated Field[YEAR(Order Date)]
--The Dashboard should allow users to check historical data by offering them the flexibility to select any desired year.
Create Parameter named Select Year with Data Type(Integer) - Allowable Values - Add Values From - Display Format - Okay - select the parameter - Show Parameter
Change Calulated Parameter to [IF YEAR(Order Date) = Select Year THEN Sales END] for the current year and [IF YEAR(Order Date) = Select Year - 1 THEN Sales END] to get the previous year

Create a parameter that shows the percentage difference of sales ((SUM([CY Sales]) - SUM([PY Sales])) / SUM([PY Sales]))
Change the Default Properties - Number Format to Percentage
Remove the Year (Order Date) and add % Diff Sales to the Measure Values

In a new worksheet, add SUM(CY Sales) & % Diff Sales to the details
Double click the title (KPI Sales) and add a sheet title and insert the two details above
Play with the Format - Pane (use ▲ 0.00%; ▼ -0.00%; as a custom format)

--Present the data for each KPI on a monthly basis for both the current year and the previous year. (SparkLine)
Add Order Date to the Columns and switch from YEARS to Months with drop down menu (Change Discrete to continuous)
Add SUM(CY Sales) to the Row
Add SUM(PY Sales) to the Y axis of the chart

--Highlight the HIGHEST AND LOWEST point on the Sparkline Chart
Create a Calculated Field (IF SUM([CY Sales]) = WINDOW_MAX(SUM([CY Sales])) THEN SUM([CY Sales]) ELSEIF SUM([CY Sales]) = WINDOW_MIN(SUM([CY Sales])) THEN SUM([CY Sales]) END)
Add the Mix/Max you jst created to the Rows
Chain the Min/Max to a Circle and change the Measured Values to a Line instead of Automatic for both
Change Min/Max in the Rows to Dual Axis

--Fix the title so its not a range that reacts to what is below it (another way coould be to do the Sparkline on a different sheet)
In the Measured Values, add a "}" to the end and a "{" begining of both values (SUM of CY Sales and % DIff Sales)
Double click the title and re-Insert the two tabs that were just adjusted

--To change the dot colors
Control and drag the min/max sales to the Colors tab on the left side. Chnage Automatic drop down to Diviring Colors and change the Stepped Colors to 2 Steps
