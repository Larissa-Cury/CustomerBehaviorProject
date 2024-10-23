## CustomerBehaviorProject
This is the repository that describes my Power Bi project which aims to analyse customer behavior


## Transforming the data

### Creating the `dim_customers` table 

- Return Unique Customers Only
   - I removed the duplicated rows in Customer Id to return only unique customers
  
- Creating the "Marital Status" Column
  
   - I added a new custom column by
     [x] `Add Columns` > `Add Custom Column`
   - I used the formula below to create the column:
     - It first creates a random number between 0 and 1 `Number.RandomBetween(0,1)`;
     - Then `Number.Round` rounds this number to either 0 or 1;
     - Then the IF statement addresses "Single" to "1" and "Married" to 0
   
```
if Number.Round(Number.RandomBetween(0,1)) = 1 then "Single" else "Married"
```

- Creating the "Region" Column
  
  - I first added a new temporary column called `randomNumber` by
    [x] `Add Columns` > `Add Custom Column`
  - I created the column based on the formula below;
    - The column stores values from 1 to 6 following the same rationale described for the `Marital Status` column
 
```
Number.Round(Number.RandomBetween(1,6)))
```

  - I first added a new column by [x] `Add Columns` > `Add Custom Column`;
  - I created the column based on the formula below:
    - The IF statement attributes either Northeast, Southeast, Midwest, Southwest, West, Pacific according to the numbers originated in `RandomNumbers`;
  - Then I deleted the temporary column

```
if [randomNumber] = 1 then "Northeast"
else if [randomNumber] = 2 then "Southeast"
else if [randomNumber] = 3 then "Midwest"
else if [randomNumber] = 4 then "Southwest"
else if [randomNumber] = 5 then "West"
else "Pacific"
```

### Creating the `fact_sales` table 

- Given that the column `Purchase Date` provides not only the dates but also the timestamps of each purchase, I used it to identify unique orders. I applied `remove duplicates` in this column.
   - I removed the following columns: `Customer Age`, `Returns`, `Customer Name`, `Gender`
  
- Creating the "Order ID" Column
 
  - I first created an Index Column by
    [x] `Add Columns` > `Add Index Column` > `Start From 1`
  - Then I created a custom column using the formula:  `[Index] + 1000`
  - Then I transformed `Order ID` from whole numbers to Text;
  - Then I deleted the `Index` column.

### Creating the `dim_calendar` table

- Step 1: I first created an empty query to obtain the min and max dates from `fact_sales`;
   - In the advanced editor, I used the following code:
   
     ```
     = let
       Source = fact_sales,
       MinDate = List.Min(Source[Purchase Date]), // Extract Min date from Purchase Date
       MaxDate = List.Max(Source[Purchase Date])  // Extract Maxc date from Purchase Date
   in
       [MinDate = MinDate, MaxDate = MaxDate]
     ```
- Step 2: I created 2 new date parameters called `minDate` and `maxDate` containing the min and max dates from the dataset

- Step 3: I created a list of dates
   - I created a new query in which I created `MinDate` and `MaxDate` steps by attributing their equivalent date parameters I created in the previous step; 
   - I created a new step called `Quant Days" by ```= Duration.Days(MaxDate - MinDate) + 1```





   









