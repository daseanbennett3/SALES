STEP-BY-STEP Code for Future Refernce
--IN A NEW WORKSHEET, CREATE A CALCULATED FIELD NAMED "CY Sales"
IF YEAR([Order Date]) = [Select Year] THEN [Sales] END
--CREATE ANOTHER CALCULATED FIELD NAMED "PY Sales
IF YEAR([Order Date]) = [Select Year] - 1 THEN [Sales] END 

--DISPLAY A SUMMARY FOR THE TOTAL NUMBER OF CUSTOMERS, TOTAL SALES PER CUSTOMER, AND TOTAL NUMBER OF ORDERS FOR THE CURRENT AND PREVIOUS YEAR
Add YEAR(Order Date) as the Row
The Measured Values card should consist of SUM(sales), SUM(CY Sales[IF YEAR(Order Date) = 2023 THEN Sales END]), and SUM(PY Sales[IF YEAR(Order Date) = 2022 THEN Sales END])
Calculated Field[YEAR(Order Date)]

--ALLOW THE USER THE ABILITY TO SELECT THEIR DESIRED YEAR
Create a Parameter named Select Year with Data Type(Integer) -> Allowable Values -> Add Values From -> Display Format -> Okay -> select the parameter -> Show Parameter
Change the Calulated Parameter to the following for the current year and the previous year, respectively:
IF YEAR(Order Date) = Select Year THEN Sales END 
IF YEAR(Order Date) = Select Year - 1 THEN Sales END

Create a Calculated Field called % Diff Sales that shows the percentage difference of sales:
(SUM([CY Sales]) - SUM([PY Sales])) / SUM([PY Sales])
Change the Default Properties -> Number Format to Percentage
Remove the Year (Order Date) and add % Diff Sales to the MeasureD Values card

--CREATE THE KPI SALES WORKSHEET
Create a new worksheet named "KPI Sales"
Add SUM(CY Sales) & % Diff Sales to the Detail
Double click the title (which should be named "KPI Sales") and add a sheet title
Insert the two details above (% Diff Sales & SUM(CY Sales)) into it:
Total Sales
<SUM({SUM([CY Sales])})>
<SUM({[% Diff Sales]})>VS The Previous Year Sales
Double-click on "AGG(% Diff Sales)" OR Whatever value you used in the previous step/title card with the Format -> Pane (use ▲ 0.00%; ▼ -0.00%; as a custom format)

--PRESENT THE DATA AS A SparkLine Chart FOR EACH KPI ON A MONTHLY BASIS FOR THE CURRENT AND PREVIOUS YEAR
Add the Select Year parameter to the page if necessary (Show Parameter in the botton left of the wokrsheet)
Add Order Date to the Columns and switch from YEARS to Months with the drop down menu 
Add SUM(CY Sales) to the Row then change the Order Date from Discrete to Continuous
Add SUM(PY Sales) to the Y axis of the chart

--HIGHLIGHT THE HIGHEST AND LOWEST POINT ON THE SparkLine Chart
Create a Calculated Field called "MIN/MAX Sales":
IF SUM([CY Sales]) = WINDOW_MAX(SUM([CY Sales])) THEN SUM([CY Sales]) ELSEIF SUM([CY Sales]) = WINDOW_MIN(SUM([CY Sales])) THEN SUM([CY Sales]) END
Add the "MIN/MAX Sales" you just created to the Rows
Change the "MIN/MAX Sales" to a Circle and change the Measured Values to a Line instead of Automatic for both
Change "MIN/MAX Sales" in the Rows to Dual Axis AND Synchronize Axis if necessary

--FIX THE TITLE SO IT'S NOT A RANGE THAT REACTS TO WHAT IS BELOW (ALTERNATIVELY YOU COULD MAKE A SparkLine Chart ON A DIFFERENT WORKSHEET)
In the All card (Same place where you find the Measured Values card) add a "}" to the end and a "{" begining of both values (SUM of CY Sales and % Diff Sales):
Double click the title and re-Insert the two tabs that were just adjusted

--CHANGE THE COLORS OF THE DOTS (MIN AND MAX VALUES)
Control and drag the "MIN/MAX Sales" from the Rows to the Colors box in the All card (Same place where you find Measured Values).
Change Automatic drop down to "Diverging Colors" and change the "Stepped Colors" to "2 Steps"

--CREATE A PARAMETER FOR THE CURRENT AND PREVIOUS YEAR TO USE IT IN THE Tooltip (THE TOOLTIP IS THE DOTS/MIN AND MAX ON THE GRAPH YOU CAN HOVER OVER FOR MORE INFORMATION)
Create a Calculated Field called "Current Year" and another one called "Previous Year," then Convert them both to Dimensions
SELECT YEAR
SELECT YEAR -1
Add the Previous Year, Current Year, CY Sales, PY Sales, and % Diff Sales to the Tooltip
Double click Tool Tip and inseret which parameters you would like to display:
Sales of <MONTH(Order Date)>, <ATTR(Current Year)>:  <SUM(CY Sales)>
Sales of <MONTH(Order Date)>, <ATTR(Previous Year)>: <SUM(PY Sales)>
Sales Differences:    <AGG(% Diff Sales)>
Highest/Lowest Sales: <AGG(MIN/MAX Sales)>

--DO THE EXACT SME THING FOR KPI Profit AND KPI Quantity

--CREATE A NEW WORKSHEET NAMED "Subcategory Comparison"
Drag & drop the Sub-Category to the Rows
Drag & drop CY Sales and PY Sales to the Colummns
In the Marks card, change the drop down menu from Automatic to Bar for both graphs
Right-click CY Sales in the Column and change to Dual Axis
Syncronize Axis on the CY Sales
Add CY Profit to the Columns
Hide CY Sales Header and show the Select Year parameter
Adjust Grid Lines, Font of the axis' and Bar sizes
Control and drag CY Profits to the Colors in the Marks card and then change the Automatic drop down to "Diverging Colors" and change the "Stepped Colors" to "2 Steps"

Create a Calculated Field called "KPI CY Less Than PY":
IF SUM([CY Sales]) < SUM([PY Sales]) THEN '*' ELSE '$' END
Drag the Calculated Field "KPI CY Less Than PY" to the Rows
Right-click Sub-Category in the Columns, Sort By: Field -> Descending -> Field Name: CY Sales

In the Marks card, under All, add CY Profit, CY Sales, Current Year, PY Sales, Previous Year, % Diff Sales to the Tool Tip:
Sub-Category:	                   <Sub-Category>
Sales of  <ATTR(Current Year)>:  <SUM(CY Sales)>
Sales of  <ATTR(Previous Year)>: <SUM(PY Sales)>
Sales Difference:                <AGG(% Diff Sales)>
Profit of <ATTR(Current Year)>:  <SUM(CY Profit)>
                                  
Format the numbers in the Tooltip by Right-clicking on the thing you wanna change in the Marks card under Numbers in the Default Pane

--Weekly Trends (New Worksheet)
Add Order Date to the Columns, change from YEAR to More -- Week Number and then make it continuous
Add CY Sales and CY Profit to the Rows 
Right-click on the top header and Add Reference Line -- Change Label: to Custom rather than Computation and name it "Avg." and Add "<Value>"
Do the same for the other graph
Edit the lines font, boldness, ect...

--Highlight the Above and Below averages
click on the SUM(CY Sales) in the Marks card and Control + Drag the CY Sales from Rows to the Colors
create a Calculated Field called "KPI Sales Average" that looks like this >>> IF SUM([CY Sales]) > WINDOW_AVG(SUM([CY Sales]))
                                                                              THEN 'above'
                                                                              ELSE 'below'
                                                                              END

Add KPI Sales Average to the Color under the CY Sales in the Marks card
Do The same thing from line 85 except with Profits

Go to the Tool Tip in the All section of the Marks card
Add Current Year to the Tool Tip and make it look like this >>  <WEEK(Order Date)>
                                                                Sales of <ATTR(Current Year)>: <SUM(CY Sales)>
                                                                <AGG(KPI Sales Average)> the average
                                                                Profit of <ATTR(Current Year)>: <SUM(CY Profit)>
                                                                <AGG(KPI Profit Average)> the average

--BUILDING DASHBOARDS
Create a Dashboard called "Sales Dashboard"
Adjust the size (from Ranged to Fixed: 1200 x 800) under the Dashboard tab on the left

--Create a Vertical/Hidden/Floatiing Filter Container
Drag and drop the KPI Sales worksheet to the middle on the dashboard
Hold shift and click and drag the filter anywhere
Remove the line graph
Play with the color of the floating container
Rename the container "Filter" in the Item Hierarchy -- Vertical Container

--Main Container
In the Dashboard tab, under Objects click and drag the Vertical Container in the middle of the dashboard
Adjust the line color and background color in the Layout tab
Rename the container in the Item Hierarchy (Main)
Add a Blank (under Objects)to the dashboard as a placeholder
Add (To the bottom of the container) a Horizontal Container (under Objects)to the dashboard and adjust the line border and the background color
Rename the Horizaontal Container in the Item Hierarchy (Title)

Add a Text to the Horizontal Container and type "Sales Dashboard"

Add two Navigations (under Objects) to the Horizontal Container (Must show a shadow on the far right side of the container to signify its the horizonal Sales Dashboard container)

--Horizontal Container for the KPIs
Add a Horizontal Container to the botton Horizontal Container (Edit the border and background color then name it KPIs)
Add a Blank to the container you just edited
Add 2 more Blanks to the other one (There should be 3 in the 1 container) ) (Look for the horizontal shadow on the far right)

--Horizontal Container for the Charts
Add a Horizontal Container under the other two to the dashboard (Name it Charts)
Add two blanks to the new container

--BUILDING DASHBOARD (Put all Charts together)
From Sheets, Add KPI Sales to the far left, KPI Profit to the middle, and KPI Quantity to the right (Make sure it shows up correctly in the Item Hierarchy)
From Sheets, Add Subcategory Comparison to the far left and Weekly Trends to the middle in the botton Horizontal Container

--BUILDING DASHBOARD (Format)
Click the chosen horizontal container from the Item Hierarchy, click the drop-down arrow and hit "Distribute Contents Evenly"
Edit the title by adding the Select Year Parameter

--Edit the two navigations at the top so the Contents can be Evenly Distributed
Add a Horizontal Container to the far right
Drag both Navigation buttons to the new Horizontal Container 
Check the Item Hierarchy to make sure the two Navigation buttons are in the correct space (Rename it Buttons)
Click the button -- drop down menu -- Edit Button -- Navigate To: Sales Dashboard -- Title: Sales Dashboard -- Tooltip: Go To Sales Dashboard

Click on the Vertical Container (Filter) -- Drop down menu -- Add Show/Hide Button -- Drow down menu -- Hide Button

Click on each chart and make sure it says Entire View and not Standard at the very top

--ADD A LEGEND TO THE CHARTS (Create a Chart for the Legend)
Create a new sheet named Subcategory Legend
Add Current Year and Previous Year to the Text
The Text should be >> <Current Year> Sales VS. <Previous Year> Sales
Go from Standard to Entire View at the top

Add a Vertical Container next to the Horizontal Container that has the Subcategory Comparison
Add the Subctegory Comparison in the Horizontal Container and add it to the Vertical Container
Rename it Chart 1
Add a Text to the top of the Vertical Container and name it Sales & Profit by Subcategory
Hide the Subcategory Comparison title
Add a Horizontal Container under the Title in the Vertical Container
Add Subcategory Legend from Sheets to the new Horizontal Container (Then Hide Title)
Add Text to the right side of the "2023 Sales vs 2022 Sales" Horizontal Container
The Text should be >> <Parameters.Select Year> ⬤Profit ⬤Loss

Add a Vertical Container to the middle of the bottom container
Drag the Weekly Trends container to the newly created Vertical Container (Name it Chart 2 in Item Hierarchy)
Add a Text to the top of the newly created Vertical Container (It should read >> Sales & Profits Over Time)
Hide the "Weekly Trends" title
Add a Text between the graphs and the title (It should read >> )

--CREATE A LEGEND FOR THE 3 CHARTS
Duplicate the Subcategory Legend and renane the duplicate KPI Legend
The Text should be >> <Current Year>  VS. <Previous Year>  ⬤ Highest Month ⬤ Lowest Month
Go back to the Sales Dashboard and add the newly created KPI Legend (in the Sheets area) right below the title and above the 3 horizontal containers
Hide the title of the KPI Legend to show the Text
Adjust the border colors and padding (8 everywhere except for the top)

--DYNAMIC FILTERS
Go to KPI Sales sheet, select Category, right-click and select "Show Filter" (Do the same for Subcategory, Region, State, and City)
Click each filter in the Filters tab -- right click -- Apply to Worksheets -- All Using This Datasource
Go back to Sales Dashboard
Show the filter and make it bigger
Delete everything in the filter except the Select Year Parameter
Click on the Total Sales -- Drop down arrow -- Filters -- click on Category, Sub-Category, Region, State, and City
For each filter in the Filters tab -- Drop down menu -- Multiple Values (dropdown)
Add a Text right under Select Year Parameter and below Sub-Category that say "Products" and "Location" respectively
Add a Blank to make more room
Add another text at the top as a title named "Filters"
Adjust the inner padding if desired
Select the Use as Filter button on the two graphs in the bottom container

--ADD IMAGES AS ICON
Click the drop-down menu on the filter -- Edit Button... -- Item Hidden -- Choose -- Pick any image
Do the same for the Item Shown (Add a tooltip that says what the filter does)
Drag and drop an Image to the far left of the title -- Choose -- Pick the Icon of the project -- Options: check both Fit Image and Center Image
