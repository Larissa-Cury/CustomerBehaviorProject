## CustomerBehaviorProject
This is the repository that describes my Power Bi project which aims to analyse customer behavior


## Transforming the data

### Creating the `dim_customers` table 

* The `dim_calendar` table was creating following the steps described below the picture.

 <div align="center">
  <img width="900" height="420" 
       src="https://drive.google.com/uc?id=173CEUJ6Zt85daKprqmL85BDIqei9YwTV">
</div>

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

### Creating the `dim_calendar` table

* The _dim_calendar_ table was creating following the steps described below the picture.

 <div align="center">
  <img width="500" height="320" 
       src="https://drive.google.com/uc?id=1la9wOyJQV0O54ZIBDyO8wL6sfrmYOQ3v">
</div>
 
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
   - I created a new query in which I created `MinDate` and `MaxDate` steps by attributing their value to the equivalent date parameters I created in the previous step; 
   - I created a new step called `Quant Days" by ```= Duration.Days(MaxDate - MinDate) + 1```
   - I then added a new step called `XXX` and attributed the value `List.Dates(MinDate, daysQuant, #duration(1,0,0,0))` to it
       - `List.Dates()` creates a list of dates starting at `MinDate` plus `daysQuant` number of days incrementing by 1 day `#duration(1,0,0,0))`
       - _Note._ 1: Represents 1 day, 0: Represents 0 hours, 0: Represents 0 minutes and 0: Represents 0 seconds.
   - Finally, I transformed the list to a table by right-clicking on the list and choosing `to table` option

 <div align="center">
  <img width="500" height="320" 
       src="https://drive.google.com/uc?id=1raln5QNU5lBfqINJPBeJATmHEaWpeS2m">
</div>

- Step 4: Transforming the column into a date format
   - I renamed the table as  `dim_calendar` and also the column as `Date`
   - I changed the cell value's format to `Date` format
 
- Step 5: Creating new columns based on the `Date` Column
   - With the `Date` column selected, I went to [x] `Add Columns` > `Date` and created the following columns:
      - Year
      - Month
      - Name of the Month
      - Quarter
      - Day
      - Name of the Day

- Step 6: Adding Custom Columns based on `dim_calendar` columns
      - `Short Month` was created using the formula `Text.Start([Month Name],3)`
      - `Year Month` was created using the formula `Date.ToText([Date], "yyyy MMM")`
      - `Quarter Text` was created using the formula `"Q" & Number.ToText([Quarter])`
  
    _Note._ I right-clicked the text columns to put their first letter in capitals by clicking on `Transform`
    _Note 2._ If your system is not in English, then you'll need to add the English names manually by custom columns such as `Date.ToText([Date], "MMMM", "en-US")`

- Step 7: Creating a relantionship between `Purchase Date` from `fact_sales`  and `Date` from `Dim_Calendar`
- Step 8: I sorted the both `Month Eng` and `Short Month Eng` by the `Month Column`
  
### Creating the `fact_sales` table 

* The `fact_sales` table was creating following the steps described below the picture.
  -_Note._ The OG dataset does not provide a `Product Id` key, this is why I didn't break the table down into a `dim_product` one

<div align="center">
  <img width="500" height="320" 
       src="https://drive.google.com/uc?id=1v__rLH6k_msWIM8zXmQxl52GAMAg_7zN">
</div>

- Given that the column `Purchase Date` provides not only the dates but also the timestamps of each purchase, I used it to identify unique orders. I applied `remove duplicates` in this column.
   - I removed the following columns: `Customer Age`, `Returns`, `Customer Name`, `Gender`
  
- Creating the "Order ID" Column
 
  - I first created an Index Column by
    [x] `Add Columns` > `Add Index Column` > `Start From 1`
  - Then I created a custom column using the formula:  `[Index] + 1000`
  - Then I transformed `Order ID` from whole numbers to Text;
  - Then I deleted the `Index` column.






   









