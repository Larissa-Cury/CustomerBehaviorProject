# CustomerBehaviorProject
This is the repository that describes my Power Bi project which aims to analyse customer behavior


# Transforming the data


- Creating the "Marital Status Column"
 - The formula: first creates a random number between 0 and 1 `Number.RandomBetween(0,1)`;
 - Then Number.Round rounds this number to either 0 or 1;
 - Then the IF statement addresses "Single" to "1" and "Married" to 0
   
```
if Number.Round(Number.RandomBetween(0,1)) = 1 then "Single" else "Married"
```

- Creating the "Region" Column

- The IF statement attributes either Northeast, Southeast, Midwest, Southwest, West, Pacific according to the numbers originated in `RandomNumbers`
```
if [randomNumber] = 1 then "Northeast"
else if [randomNumber] = 2 then "Southeast"
else if [randomNumber] = 3 then "Midwest"
else if [randomNumber] = 4 then "Southwest"
else if [randomNumber] = 5 then "West"
else "Pacific"
```
