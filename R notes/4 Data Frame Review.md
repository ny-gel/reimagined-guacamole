## Mean, Median, Mode
* `median`: In R, the `median` of a vector is calculated using the median() function. The function accepts a vector as an input.
```r
a <- c(3,4,5,12)
median(a) # find median
```
* `Mode`: In R, the `mode` of a vector can be calculated using the Mode() function in the DescTools package. The function accepts a vector as an input and returns the most frequently occurring observation in the dataset.
```r
library(DescTools)
example_data <- c(24, 16, 12, 10, 12, 28, 38, 12, 28, 24)
example_mode <- Mode(example_data) # find Mode
```

* `mean`: In R, the mean of a vector is calculated using the mean() function. The function accepts a vector as input, and returns the average as a numeric.

## Quartiles, Quantiles and IQR
* Quantiles are the set of values/points that divides the dataset into groups of equal size.
* Quartiles are the three dividing points (or quantiles) that split data into four equally sized groups are called quartiles.

```r
vector_name <- c(-10, 30, 4, 5, 10, -20)

# Quartiles
quartiles <- quantile(vector_name,c(0.25, 0.5, 0.75), na.rm = TRUE)
Q1 <- quartiles[1]   # first quartile
Q2 <- quartiles[2]   # median
Q3 <- quartiles[3]   # third quartile

# Interquartile Range
iqr_value <- IQR(x, na.rm = TRUE)

Q1
Q2
Q3
iqr_value
```


### Explore the 1985 Cars Dataset
Luckily for you, we have a dataset that stores precisely that information. This dataset comes from the UCI Machine Learning Repository and is linked here. This dataset has been adapted so that the variable names are along the first rows.


# Task 1
```r
# load libraries
library(readr)
library(dplyr)
```

# Task 2
The file `cars85.csv` stores the data that comes from the UCI Machine Learning Repository. Load the file into a dataframe called `cars` to get started.
```r
# load data
cars <- read_csv('cars85.csv')
cars
```

# Task 3
Inspect cars with `head()` and `summary()`.
```r
# inspect data
head(cars)
summary(cars)
```

# Task 4
After inspecting the dataframe, you notice something odd about the `normalized_losses` column. This column has a lot of entries that are question marks (?). This variable is not worth looking at since we don’t have all the cars’ expected losses.

Let’s remove this column from the dataset. Select all columns from cars but `normalized_losses`. Save your new dataframe to `cars`.
```r
# select columns
cars <- cars %>%
  select(-normalized_losses)
```

# Task 5
```r
# view columns
names(cars)
```
# Task 6
Update that column name in cars as follows:
`symboling` -> `risk_factor`

```r
# rename column
cars <- cars %>%
  rename(risk_factor = symboling)
cars

```
# Task 7
```r
# define threshold
mpg_threshold <- 30
```
# Task 8
Add a new column to cars called `mpg_diff_from_threshold`. This will measure how far each car’s highway mpg is from 30 mpg. View the updated cars dataframe.
```r
# add column
cars <- cars %>%
  mutate(mpg_diff_from_threshold = mpg_threshold - highway_mpg)
cars


```
# Task 9
Filter the rows of cars to find all the cars where `mpg_diff_from_threshold` is greater than 0. Save this new dataframe to `mpg_exceeds_threshold` and view it.
```r
# filter rows
mpg_exceeds_threshold <- cars %>%
  filter(mpg_diff_from_threshold > 0)
mpg_exceeds_threshold
```

# Task 10
Which cars have the highest miles per gallon on the highways? To find this, `arrange` the rows of `mpg_exceeds_threshold` by `mpg_diff_from_threshold` descending. Save this new dataframe as `mpg_exceeds_threshold`.
```r
# arrange rows
mpg_exceeds_threshold <- mpg_exceeds_threshold %>%
  arrange(desc(mpg_diff_from_threshold))
mpg_exceeds_threshold

```
# Task 11
Order the rows of cars by `engine_size descending`. Save the new data frame to `ordered_by_engine_size`. View `ordered_by_engine_size`.
```r
# order rows by engine size
ordered_by_engine_size <- cars %>%
  arrange(desc(engine_size))
ordered_by_engine_size

```
# Task 12
Create a variable called `chosen_make` that contains the `make` you want to check.
```r
# choose make
chosen_make <- 'bmw'
```
# Task 13
Filter `cars` to only include rows where the `make` column is equal to `chosen_make`. Save the new dataframe to `chosen_make_details`.
```r
# filter rows by make
chosen_make_details <- cars %>%
  filter(make == chosen_make)
chosen_make_details
```
# Task 14
Order the rows of `chosen_make_details` by `engine_size` descending and save the new dataframe to `chosen_make_details`. View `chosen_make_details`.
```r
# order filtered rows by engine size
chosen_make_details <- chosen_make_details %>%
arrange(desc(engine_size))
chosen_make_details
```
