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
Play with the Format - Pane
