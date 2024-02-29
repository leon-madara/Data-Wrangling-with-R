# Data-Wrangling-with-R
I will be performing data wrangling with R using the Diamonds dataset in the ggplot2 package. 

# Data Wrangling with R: Diamonds Dataset Analysis
## Project Overview
This project focuses on performing comprehensive data wrangling and analysis on the diamonds dataset, which is included in the ggplot2 package in R. Our objectives are to clean, transform, and analyze the dataset to uncover insights into the factors that influence diamond prices and other characteristics such as carat size, cut quality, color, and clarity.
## Dataset
The diamonds dataset contains 10 variables (carat, cut, color, clarity, depth, table, price, x, y, z) for 53,940 diamonds. The dataset is widely used for data analysis and visualization exercises and comes bundled with the ggplot2 package.
### Key Insights Aimed
 1.	**Price Distribution by Diamond Characteristics**: Analyzing how diamond prices vary with carat, cut, color, and clarity.
 2.	**Impact of Cut, Color, and Clarity on Price**: Determining which of these factors most significantly affects diamond prices.
 3.	**Relationship Between Carat and Dimensions**: Exploring how a diamond's weight correlates with its physical dimensions.
 4.	**Characteristics of Outliers**: Identifying and investigating outliers in price and other characteristics.
 5.	**Trends Over Different Qualities**: Examining how the distribution of prices or carat sizes varies across different qualities.
 6.	**Volume vs. Carat Size**: Calculating the volume of diamonds and analyzing its correlation with carat size.
 7.	**Price Per Carat Analysis**: Investigating the price per carat across different categories of diamonds.
## Tools and Technologies
  - R Language: For all data wrangling, analysis, and visualization tasks.
  - **ggplot2** Package: For data visualization.
  - **dplyr** Package: For data manipulation.
  - **tidyr** Package: For data tidying.
### **Installation and Setup**
Install required packages
- ```install.packages("ggplot2")```
- ```install.packages("dplyr")```
- ```install.packages("tidyr")```
      
Load required packages
- ```library(ggplot2)```
- ```library(dplyr)```
- ```library(tidyr)```
      
Load the dataset
```data(diamonds)```

### **File Descriptions**
*data_wrangling_script.R*
-	 Contains all R scripts used for data cleaning, transformation, and analysis.
-	*visualizations/*: Directory containing all generated plots and visual graphs.
## **Usage**
To replicate this analysis, run the data_wrangling_script.R script in R or RStudio. Ensure you have the required packages installed and loaded as described in the Installation and Setup section.

## **Data Analysis**
### Price Distribution by Diamond Characteristics
Price Distribution by Carat Size

```
ggplot(diamonds, aes(x = carat, y = price)) +
geom_point(aes(color = cut), alpha = 0.5) +
geom_smooth() +
theme_minimal() +
labs(title = "Price Distribution by Carat Size", x = "Carat", y = "Price")
```

Price Distribution by Cut Quality

```
ggplot(diamonds, aes(x = cut, y = price, fill = cut)) +
geom_boxplot() + theme_minimal() +
labs(title = "Price Distribution by Cut Quality", x = "Cut", y = "Price")
```

Price Distribution by Color

```
ggplot(diamonds, aes(x = color, y = price, fill = color)) +
geom_boxplot() + theme_minimal() +
labs(title = "Price Distribution by Color", x = "Color", y = "Price")
```

Price Distribution by Clarity

```
ggplot(diamonds, aes(x = clarity, y = price, fill = clarity)) +
geom_boxplot() + theme_minimal() +
labs(title = "Price Distribution by Clarity", x = "Clarity", y = "Price")
```

#### Fit a multiple regression model
-	```model <- lm(price ~ carat + cut + color + clarity, data = diamonds)```
-	Summary of the model to check coefficients and significance ```summary(model)```

### ***Visualizing Combined Effects with ggplot2***
```
ggplot(diamonds, aes(x = carat, y = price)) + 
geom_point(aes(color = clarity), alpha = 0.3) +
geom_smooth(se = FALSE) +
facet_wrap(~cut) +
theme_minimal() +
labs(title = "Price by Carat across Different Cuts", x = "Carat", y = "Price")
```

### ***Relationship Between Carat and Dimensions***
- To investigate this relationship, you can use both correlation analysis and visualization techniques.
- I conducted a **Correlation Analysis**, specifically, the *Pearson correlation coefficients* between carat and each of the dimensions **x, y, z**. This statistical measure is essential in quantifying the strength and direction of the *linear relationship* between carat weight and each dimension.

### Calculate correlation coefficients
```cor(diamonds$carat, diamonds$x, use = "complete.obs")```
The result: 0.9750942
```cor(diamonds$carat, diamonds$y, use = "complete.obs")```
The result: 0.9517222
```cor(diamonds$carat, diamonds$z, use = "complete.obs")```
The result: 0.9533874

ggplot(diamonds, aes(x = carat, y = x)) +
  geom_point(alpha = 0.5) +
  geom_smooth(method = "lm", color = "blue") +
  labs(title = "Relationship Between Carat and Length", x = "Carat", y = "Length (x)") +
  theme_minimal()

ggplot(diamonds, aes(x = carat, y = y)) +
  geom_point(alpha = 0.5) +
  geom_smooth(method = "lm", color = "blue") +
  labs(title = "Relationship Between Carat and Width", x = "Carat", y = "Width (y)") +
  theme_minimal()

ggplot(diamonds, aes(x = carat, y = z)) +
  geom_point(alpha = 0.5) +
  geom_smooth(method = "lm", color = "blue") +
  labs(title = "Relationship Between Carat and Depth", x = "Carat", y = "Depth (z)") +
  theme_minimal()


###***Characteristics of Outliers**
Identifying and investigating outliers in the **diamonds** dataset, particularly in price and other characteristics such as carat, cut, color, and clarity is a two-step process: **detection** and **analysis**. *Outliers can significantly impact the analysis, leading to skewed results*. They may represent *special cases*, such as **exceptionally large or high-quality diamonds**, or **data entry errors**.
###Detection of Outliers
To do this, we will use the *Boxplot Analysis* because it is a useful tool for visualizing the distribution of numerical data and detecting outliers based on the *interquartile range* **(IQR)**.
####Boxplot for price
ggplot(diamonds, aes(y = price)) +
  geom_boxplot() +
  labs(title = "Boxplot of Diamond Prices", y = "Price")

####Boxplot for carat
ggplot(diamonds, aes(y = carat)) +
  geom_boxplot() +
  labs(title = "Boxplot of Diamond Carat", y = "Carat")

##Another way of identifying outliers
**Statistical Detection**
Another way of identifying or detecting outliers is by *calculating the IQR* and identifying *data points* that *fall below* **Q1 - 1.5IQR** or *above* **Q3 + 1.5IQR** for each characteristic.
# Example for price
price_q1 <- quantile(diamonds$price, 0.25)
price_q3 <- quantile(diamonds$price, 0.75)
price_iqr <- price_q3 - price_q1
price_outliers <- diamonds %>% 
  filter(price < (price_q1 - 1.5 * price_iqr) | price > (price_q3 + 1.5 * price_iqr))
view(price_outliers)
##**Analysis of Outliers**
After identifying outliers, the next step is to investigate their characteristics to understand their nature and potential impact on the analysis.
Examining Outlier Characteristics
I analyzed the properties of the outliers, such as their cut, color, and clarity grades, to see if they share any common features using a simple code.
-	Summary statistics of outliers
-	summary(price_outliers)
-	Part of the analysis includes checking if high-price outliers tend to have higher carat values, better cut grades, or other distinguishing features.
###Visualing the outliers
Scatter plot of *price vs. carat* with **outliers** highlighted
ggplot(diamonds, aes(x = carat, y = price)) +
  geom_point(aes(color = (price < (price_q1 - 1.5 * price_iqr) | price > (price_q3 + 1.5 * price_iqr))), alpha = 0.5) +
  scale_color_manual(values = c("FALSE" = "grey", "TRUE" = "red")) +
  labs(title = "Diamond Price vs. Carat with Outliers Highlighted", x = "Carat", y = "Price") +
  theme_minimal()
##**Trends Over Different Qualities**
Examining how the distribution of prices or carat sizes varies across different qualities.
####**Price distribution by quality**
ggplot(diamonds, aes(x = cut, y = price)) +
  geom_boxplot(aes(fill = cut)) +
  labs(title = "Price Distribution by Cut", x = "Cut", y = "Price") +
  theme_minimal()

####**Price distribution by color**
ggplot(diamonds, aes(x = color, y = price)) +
  geom_boxplot(aes(fill = color)) +
  labs(title = "Price Distribution by Color", x = "Color", y = "Price") +
  theme_minimal()

####**Price distribution by clarity**
ggplot(diamonds, aes(x = clarity, y = price)) +
  geom_boxplot(aes(fill = clarity)) +
  labs(title = "Price Distribution by Clarity", x = "Clarity", y = "Price") +
  theme_minimal()

####*Carat Size Distribution by Quality*
ggplot(diamonds, aes(x = cut, y = carat)) +
  geom_boxplot(aes(fill = cut)) +
  labs(title = "Carat Size Distribution by Cut", x = "Cut", y = "Carat") +
  theme_minimal()

####*Carat Size Distribution by Color*
ggplot(diamonds, aes(x = color, y = carat)) +
  geom_boxplot(aes(fill = color)) +
  labs(title = "Carat Size Distribution by Color", x = "Color", y = "Carat") +
  theme_minimal()

####*Carat Size Distribution by Clarity*
ggplot(diamonds, aes(x = clarity, y = carat)) +
  geom_boxplot(aes(fill = clarity)) +
  labs(title = "Carat Size Distribution by Clarity", x = "Clarity", y = "Carat") +
  theme_minimal()

##**Volume vs. Carat Size**
Calculating the volume of diamonds and analyzing its correlation with carat size.
Analyzing the relationship between the volume of diamonds and their carat size involves calculating the volume based on the dimensions provided in the dataset, and then examining the correlation between the newly calculated volume and carat size. Given that the diamonds dataset contains the dimensions (length x, width y, and depth z) of each diamond, we can use these to calculate the volume.
###Diamond Volume
library(dplyr)

diamonds <- diamonds %>%
  mutate(volume = x * y * z)
 Then we examine the summary statistics
summary(diamonds$volume)
Then we analyze the correlation
cor(diamonds$carat, diamonds$volume, use = "complete.obs")
**0.9763084**
###Visualize the relationship
ggplot(diamonds, aes(x = carat, y = volume)) +
  geom_point(alpha = 0.1) +
  geom_smooth(method = "lm", color = "blue") +
  labs(title = "Relationship Between Diamond Volume and Carat Size",
       x = "Carat",
       y = "Volume (mm³)") +
  theme_minimal()

### **Price Per Carat Analysis**
Investigating the price per carat across different categories of diamonds.
Calculating the price per carat
diamonds <- diamonds %>%
  mutate(price_per_carat = price / carat)
The next step is to summarize **Price Per Carat by Category**
#### By cut
But first we create the column price_per_carat
library(dplyr)
- Assuming diamonds is already loaded
diamonds <- diamonds %>%
  mutate(price_per_carat = price / carat)
-Then we check if the column was created
str(diamonds$price_per_carat)

After a successful creation, we continue:
diamonds %>%
  group_by(cut) %>%
  summarise(avg_price_per_carat = mean(price_per_carat)) %>%
  arrange(desc(avg_price_per_carat))

#### By color
ggplot(diamonds, aes(x = reorder(color, -price_per_carat), y = price_per_carat, fill = color)) +
  geom_bar(stat = "summary", fun = "mean") +
  labs(title = "Average Price Per Carat by Color", x = "Color", y = "Average Price Per Carat") +
  theme_minimal()

#### By clarity
ggplot(diamonds, aes(x = reorder(clarity, -price_per_carat), y = price_per_carat, fill = clarity)) +
  geom_bar(stat = "summary", fun = "mean") +
  labs(title = "Average Price Per Carat by Clarity", x = "Clarity", y = "Average Price Per Carat") +
  theme_minimal()

##Findings and Conclusion
Findings:
1.	Price Per Carat by Color and Cut:
•	The average price per carat varies across color and cut categories.
•	The price per carat generally decreases from D (colorless) to J (slightly tinted) color categories.
•	Interestingly, not all higher quality cuts (like Ideal) command the highest price per carat, indicating that other factors like color and clarity may play a more significant role in pricing per carat.
2.	Carat Size Distribution:
•	The carat size has a wide distribution, with a median significantly lower than the mean, indicating a right-skewed distribution with outliers on the higher carat size.
•	Carat size varies by cut, color, and clarity, with some categories like higher clarity (IF, VVS1, VVS2) containing diamonds of larger carat sizes.
3.	Price Distribution:
•	Prices are widely distributed across all clarity grades, with a trend of increasing prices for higher clarity.
•	The variation in price by cut quality shows that better cuts (Ideal and Premium) do not always correspond to the highest prices, suggesting that buyers also place substantial value on color and clarity.
•	The distribution of prices by color shows a general decrease in median price from colorless to more tinted diamonds.
4.	Outliers in Price and Carat:
•	There are notable outliers in both price and carat size, with some diamonds exhibiting exceptionally high values in both, which may represent particularly large or high-quality stones.
5.	Relationship Between Carat and Dimensions:
•	There is a strong positive correlation between carat weight and the dimensions of diamonds (length, width, depth), as expected.
•	The scatter plots of carat against each dimension exhibit a linear relationship, with the spread increasing for larger diamonds, indicating variability in how carat weight translates to size.
6.	Volume vs. Carat Size:
•	The relationship between volume and carat is linear and positive, showing that volume is a reliable indicator of carat size. The volume calculation used (x * y * z) is therefore a reasonable approximation for physical size.
7.	Multiple Regression Analysis:
•	The multiple regression model indicates that carat size has the most substantial effect on price, followed by color and clarity.
•	The model has a high R-squared value (0.9159), showing that about 91.59% of the variability in price can be explained by the included variables (carat, cut, color, clarity).
Conclusions:
The analysis of the diamonds dataset provides insightful trends and relationships within the diamond market. Price per carat is not solely determined by cut quality but is influenced more significantly by color and clarity. Carat size shows a skewed distribution with outliers indicating the presence of exceptionally large diamonds. The multiple regression model effectively captures the combined effects of carat, cut, color, and clarity on price, with carat size being the dominant factor. The visualizations and statistical summaries support a robust understanding of the diamond valuation process, with implications for buyers, sellers, and enthusiasts looking to understand what drives diamond prices in the marketplace.
The findings also underscore the complexity of diamond pricing, where multiple factors interact in determining value. The linear relationships between carat and dimensions, as well as carat and volume, confirm the expected patterns that larger diamonds have greater physical dimensions and volume. The presence of outliers suggests that while most diamonds follow a general trend in pricing, there are exceptional cases where unique features may significantly increase a diamond's value.
##Future Work
I will use PowerBI, Tableau, or Metabase to create better visualizations and uncover additional key business insights.
##Contributors
•	Your Name
###License
This project is open-source and available under the MIT License.




