# Data Visualization with tidyverse in R

To visualize data, I'm using the `tidyverse` extension for R. You must run `library(tidyverse)` each time you want to run the program.

### Loading the Starwars Dataset
To load the `starwars` dataset, you can use view or load it from the csv.

```r
view(starwars)
```

If you are loading data from a csv, load the data frame `df` by
```r
starwars <- read_csv('starwars.csv')
```

### Using csv files
The first argument of the `read_csv()` is the file to be read.  
Tibbles in R can be exported to csv files using the `write_csv()` function.   
The first argument of `write_csv()` is the tibble to be exported.

### Finding the class of a column
Specifically for the `name` column in the `starwars` dataset:
```r
print(class(starwars$name))
```

More commonly, you will want to obtain the structure of the entire data frame. Instead of printing individual classes of columns, use the `str()` function.


### Adding a column using `mutate()`, and the pipe operator `%>%`
Let's try to convert from metric to imperial.  
We will need to define a new variable and use the `mutate` operator, to add a new column to the data frame.

```r
imp_height <- starwars %>%
  mutate(height = 0.0328084 * height)
```

`%>%` essentially chains the first function to the next; It takes the output of one statement and makes it the input of the next statement, allowing for two-step actions to be performed. You can think of it as a "THEN".  

### Calculating mean of a column 
The general code for obtaining `mean` of column with dplyr:
```r
df %>%
  summarise(mean_col = mean(column_name))
```

```r
# Group data by `eye_color` and then calculate the `mean` mass of each group
starwars %>%
  group_by(eye_color) %>%
  summarise(mean(mass))
```


### Removing NA values in summarised values  
1. Converting invalid values into `NA`  
2. Filter out `NA` values  

`is.na(x)` function checks if any of the elements of (x) are `NA`.  
`is.na(x)` returns a logical vector of the same length of (x), where each element is `TRUE` if the corresponding element in (x) is `NA` and `FALSE` if there is a numerical number.

For instance:
```r
x <- c(1, 2, NA, 3, NA)
is.na(x) 
[1] FALSE, FALSE, TRUE, FALSE, TRUE
```
This means that the **3rd and 5th elements** in the vector are `NA`.

`!is.na(x)` is the logical negation operator. It reverses (negates) the logical values.  
When applied to the above example, the 3rd and 5th elements become `FALSE` when they are `NA`.  
In the `starwars` data set, using `!is.na(x)` will select the values that is NOT `NA` / have a numerical number.  

```r
starwars %>%
  filter(!is.na(mass))
```
This is very useful when you want to clean your data or remove missing values before performing operations like aggregation, summaries, or plotting. **For duplicates, see (5) Data cleaning.**


## Filtering Rows with Logic From Codecademy 

### Loading data frames
```r
# load libraries
library(readr)
library(dplyr)
```

```r
# load data frame
artists <- read_csv('artists.csv')
```

### Filter function by rows
Say I want to filter data within the `df` by **height**.  
I'll call the individuals above 100cm as `tall`.

```r
# define the new variable
# filter sorts rows, select sorts columns
tall<- filter(starwars, height = >100)

# cleaner way to do it
tall <- starwars %>%
  filter(height = >100)
```

### Select function by columns
Allows us to select columns of data except specified ones.    
```r
# Add the - operator before the name of the columns before passing them as arguments to select().
select(-genre, -spotify_monthly_listeners, -year_founded)
```

In this case, the weather data frame is piped into the select function that would select the first two columns of the weather data frame.
```r
weather %>% select(1:2) 
```

### Dplyr's filter (or, not)
```r
# filter rows with or
korea_or_before_2000 <- artists %>%
  filter(country == 'South Korea' | year_founded < 2000)
```

```r
# filter rows with not !
not_rock_groups <- artists %>%
  filter (!(genre == 'Rock'))
```
For instance: What if you want to find all orders where shoes in any color but red were purchased. Using the not or bang operator (!), 

```r
orders %>%
  filter(!(shoe_color == 'red'))
```

1. orders is again piped into `filter()`  
2. the condition that should not be met is wrapped in parentheses, preceded by `!`, and given as an argument to `filter()`  
3. a new data frame is returned containing only rows where shoe_color is not 'red'  

### Dplyr's rename
The `rename()` function of dplyr package can be used to change the column names of a data frame.

```r
#Renaming convention
rename(new_col_name = old_col_name)
```

On the other hand to rename multiple columns based on logical criteria, the `rename()` function has variants such as `rename_if()`, `rename_at()` and `rename_all()`.


### Final review
Using the dplyr lesson,
 
```r
# filter rows
popular_not_hip_hop <- chosen_cols %>% 
  filter(spotify_monthly_listeners > 20000000, genre != 'Hip Hop') 
head(popular_not_hip_hop)
```

```r
# arrange rows
youtube_desc <- popular_not_hip_hop %>% 
  arrange(desc(youtube_subscribers))
head(youtube_desc)
```

```r
# select columns, filter and arrange rows
artists <- artists %>%
  select(-country,-year_founded,-albums) %>%
filter(spotify_monthly_listeners > 20000000, genre !='Hip Hop') %>%
  arrange(desc(youtube_subscribers))
head(artists) # displays the final data frame
```

Using the transmute function, which also takes name-value pairs like `mutate()`. `transmute()` however only returns the data frame with only the new columns.
```r
# To add `sales_tax` and `profit` columns while dropping all other columns from the data frame:
df %>%
  transmute(sales_tax = price * 0.075,
            profit = price - cost_to_manufacture)
```
```r
# Add breed back into the dogs data frame
breed = breed
```

1. We learnt to add new columns to a data frame using mutate()
2. Add new columns to a data frame and drop existing columns using transmute()
3. Change the column names of a data frame using rename()
