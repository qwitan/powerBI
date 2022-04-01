# powerBI
exploratory and assessment related projects

https://app.powerbi.com/groups/me/reports/28c56f3f-bfa3-468c-b306-767301a99612/ReportSection9b085f85e671a44b1295
_________________________________________________________________________________________________________________________________________________________

# Data Source: Yahoo Finance

-----------------------------

# For the Candle Sticks: 
process: search for any ticker -> click on 'Historical Data' -> filter based on your needs -> right click download and copy link address.
https://query1.finance.yahoo.com/v7/finance/download/NFLX?period1=1633046400&period2=1648771200&interval=1wk&events=history&includeAdjustedClose=true
a) in PowerBI go to 'Get Data' and click on 'Web' to insert link
b) to make the link dynamic
- (StockQuote as text) as table => 
- replace ticker with "&StockQuote&"
c) make excel file/table with tickers and invoke customer function 
d) update datatypes

------------

# For the Tickers
a) create a parameter: In the power query window, under the Home ribbon, Click the Manage Parameters button. From the drop down Select the option New Parameter.   
- Name section: 'ticker' and change the Type field to Text. 
b) making the query: 

      1	https://query1.finance.yahoo.com/v8/finance/chart/
      2	{StockSymbol}  (Change the abc symbol to a parameter)
      3	?range=5y&interval=1d

c) Making the Query Branch
     
      Meta: info about the stock, as well as the timeframe and granularity
      Timestamp: a list of the dates in the range
      Indicators: the price information of stock

- create a Query Branch. The branch will split our query at this step into two paths. One will retrieve the dates, the other the prices. Then we will merge these branches back together to get the dates and prices in the same table. To start this branch Right Click on the Navigation Step, then Select the option in the drop-down menu Insert Step After. This will reference the previous step and show the same data. Our newly created set is the start of the branch. Rename StartBranch.

d) Branch 1: Dates
Click on timestamp: List.
- First convert the list to a table so we can perform transformations. Click on Transform ribbon. Select the button To Table. Next, under the Add Column ribbon Click the button Custom Column. Change the name to Date and use the following formula in the formula window:
      
      25569 + ( [Column1]/60/60/24 )
      
- Select the Date column. Click the Transform ribbon. Under the Data section, Select the Date format. Note: do not select the Date/Time.      
- Go to the Add Column ribbon, Click the little Drop down on the right half of the Index Column button. Select the option From 0 from the drop down menu. Remove the column labeled Column1, as it is not needed anymore. To do this, Right Click on Column1 and select the option Remove from the drop down menu. 

e) Branch 2: Prices
- Right Click EndDateBranch and Click the option Insert Step After to add the start of the branch
- Change the value in the formula bar from = EndBranchDate to = StartBranch
     
      Indicators: Record
      adjclose: List
      1: Record
      adjclose: List
 
 - Covert our list to a Table. see above for this step. Here we have the list of prices and again need to preserve the order with an index column. Go to the ribbon labeled Add Column. Click the Index Column and select From 0 in the drop down. Rename it EndBranchPrice

f) Merging the Branches
- On the Home Ribbon, Click the drop down arrow on the Merge Queries button. Then Select the option Merge Queries. This brings up the merge screen. Merge the query with itself. On the bottom half of the merge, Select StockValue (current). Click on the Index column for both top and bottom.

      = Table.NestedJoin(EndBranchPrice, {"Index"}, EndBranchPrice, {"Index"}, "EndBranchPrice", JoinKind.Inner)
      
-----------------------------

------------

#Link to Dashboard

      https://app.powerbi.com/groups/me/reports/28c56f3f-bfa3-468c-b306-767301a99612/ReportSection9b085f85e671a44b1295

------------
