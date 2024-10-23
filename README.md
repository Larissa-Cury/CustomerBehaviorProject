## CustomerBehaviorProject
This is the repository that describes my Power Bi project which aims to analyse customer behavior


## Transforming the data

### Creating the `dim_customers` table 

- Return Unique Customers Only
   - I removed the duplicated rows in Customer Id to return only unique customers
  
- Creating the "Marital Status" Column
  
   - [x] `Add Columns` > `Add Custom Column`
   - I used the formula below to create the column.
   - It first creates a random number between 0 and 1 `Number.RandomBetween(0,1)`;
   - Then `Number.Round` rounds this number to either 0 or 1;
   - Then the IF statement addresses "Single" to "1" and "Married" to 0
   
```
if Number.Round(Number.RandomBetween(0,1)) = 1 then "Single" else "Married"
```

- Creating the "Region" Column
  
  - [x] `Add Columns` > `Add Custom Column`
  - I first created a temporary column called `randomNumber`, using the formula below, to store values from 1 to 6 following the same rationale described for the `Marital Status` column
 
```
Number.Round(Number.RandomBetween(1,6)))
```

  - [x] `Add Columns` > `Add Custom Column`
  -  I created the column based on the formula below.
  - The IF statement attributes either Northeast, Southeast, Midwest, Southwest, West, Pacific according to the numbers originated in `RandomNumbers`

```
if [randomNumber] = 1 then "Northeast"
else if [randomNumber] = 2 then "Southeast"
else if [randomNumber] = 3 then "Midwest"
else if [randomNumber] = 4 then "Southwest"
else if [randomNumber] = 5 then "West"
else "Pacific"
```
