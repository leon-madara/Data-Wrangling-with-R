# Data-Wrangling-with-R
I will be performing data wrangling with R using the Diamonds dataset in the ggplot2 package. 

### Wrangling Processes
An R project that showcases the use of R packages. Specifically, by utilizing the inbuilt tidyverse library.

### Data Sources
R's inbuilt dataset (diamond): A dataset about diamonds' cut, color, clarity, price, etc.

### Tools
 - R Studio

### Data Cleaning
  - Searching for missing values
      -  ```.na```


### Data Wrangling
  1.  Filter row
  2.  Reorder rows
  3.  Add/modify columns with mutate()
  4.  Add summaries with group by() and summarize()

### Exploratory Data Analysis (EDA)
EDA involved exploring the elements that impacted the diamond price
  -  Average price of diamonds by cut, color, clarity, depth, etc.
  -  Characteristics of the top 10 highest and lowest selling diamond

### Data Analysis
  ```R
?diamonds
> view(diamonds)
> diamonds_ideal <- filter(diamonds, cut == "Ideal")
> view(diamonds_ideal)
```
The first code ```?diamonds``` opens information about the diamond library, providing the description, usage, format, and other vital data.
The second code ```diamonds_ideal <- filter(diamonds, cut == "Ideal")``` filters the diamond dataset and assigns the output to a temporary dataset diamonds_ideal
The third code ```view(diamonds_ideal)``` is a basic code to view the filtered dataset

```diamonds_sm <- select(diamonds, cut, color)```
```view(diamonds_sm)```
The first code assigns the filtered dataset that contains only two columns - cut and color and then the second views the dataset.


