### Explore the 1985 Cars Dataset
Luckily for you, we have a dataset that stores precisely that information. This dataset comes from the UCI Machine Learning Repository and is linked here. This dataset has been adapted so that the variable names are along the first rows.


# Task 1
```r
# load libraries
library(readr)
library(dplyr)
```

# Task 2
```r
# load data
cars <- read_csv('cars85.csv')
cars
```

# Task 3
```r
# inspect data
head(cars)
summary(cars)
```

# Task 4
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
```r
# add column
cars <- cars %>%
  mutate(mpg_diff_from_threshold = mpg_threshold - highway_mpg)
cars


```
# Task 9
```r
# filter rows
mpg_exceeds_threshold <- cars %>%
  filter(mpg_diff_from_threshold > 0)
mpg_exceeds_threshold
```

# Task 10
```r
# arrange rows
mpg_exceeds_threshold <- mpg_exceeds_threshold %>%
  arrange(desc(mpg_diff_from_threshold))
mpg_exceeds_threshold

```
# Task 11
```r
# order rows by engine size
ordered_by_engine_size <- cars %>%
  arrange(desc(engine_size))
ordered_by_engine_size

```
# Task 12
```r
# choose make
chosen_make <- 'bmw'
```
# Task 13
```r
# filter rows by make
chosen_make_details <- cars %>%
  filter(make == chosen_make)
chosen_make_details
```
# Task 14
```r
# order filtered rows by engine size
chosen_make_details <- chosen_make_details %>%
arrange(desc(engine_size))
chosen_make_details
```
