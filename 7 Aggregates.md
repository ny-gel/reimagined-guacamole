## Calculating column statistics
Using dplyr's `summarize()`, you can combine all values from a column into a single value returning a new data frame containing the desired calculation.  

The **general syntax** for `summarize()` is
```r
df %>%
  summarize(var_name = command(column_name))
```
* `var_name` is the name you assign to the column that stores the results of the summary function in the returned data frame
* `command` is the summary function that is applied to the column by summarize()
* `column_name` is the pre-existing name of the column of df that is being summarized


For instance, finding the `median` `age` of `customers`:
```r
customers %>%
  select(age)
# c(23, 25, 31, 35, 35, 46, 62)
customers %>%
  summarize(median_age = median(age))
# 35
```

Or `shipments` contains address information for all shipments that you’ve sent out in the past year. You want to know how many different `states` you have shipped to.
```r
shipments %>%
  select(states)
# c('CA', 'CA', 'CA', 'CA', 'NY', 'NY', 'NJ', 'NJ', 'NJ', 'NJ', 'NJ', 'NJ', 'NJ')
shipments %>%
  summarize(n_distinct_states = n_distinct(states))
# 3
```

Or `inventory` contains a list of types of t-shirts that your company makes. You want to know the `sd` of the `price` of your inventory.
```r
inventory %>%
  select(price)
# c(31, 23, 30, 27, 30, 22, 27, 22, 39, 27, 36)	
inventory %>% 
  summarize(sd_price = sd(price))
# 5.465595
```

To determine the average price of goods sold:
```
# in context of average price
average_price <- orders %>%
  summarize(average_price = mean(price, na.rm = TRUE))
```

### Commands
* `mean()` Average of all values in column
* `median()` Median value in column
* `sd()` Standard deviation of values in column
* `var()`
* `min()`
* `max()`
* `IQR()`
* `n_distinct()` Number of unique values in column
* `n` Count of rows within a group - does not require column as an argument
* `sum()`


## group_by() for aggregates
Suppose we have a grade book with columns `student`, `assignment_name`, and `grade`  
And we want to get an average grade for each student. What we can use is the group_by() function.
```r
df %>%
  group_by(column_1) %>%
  summarize(aggregate_name = command(column_2))

# in context of grades
grades <- df %>%
  group_by(student) %>%
  summarize(mean_grade = mean(grade))

```


## Combining grouping with filter
Rather than filtering rows by the individual column values, the rows will be filtered by their group value since a summary function is used.You want to identify all the enrollments in difficult courses, which you define as courses with an average `quiz_score` less than `80`. To filter the data frame to just these rows:
```r
enrollments %>%
  group_by(course) %>%
  filter(mean(quiz_score) < 80)
```
The average `quiz_score` for the `learn-python` course is 75, so all the rows of enrollments with a value of `learn-python` in the course column remain - even if their individual column data might exceed 80.


## Combining grouping with mutate
`group_by()` can also be used with the dplyr function `mutate()` to add columns to a data frame that involve per-group metrics.
You want to add a new column to the data frame that stores the difference between a row’s `quiz_score` and the average `quiz_score` for that row’s `course`. To add the column:
```r
enrollments %>% 
  group_by(course) %>% 
  mutate(diff_from_course_mean = quiz_score - mean(quiz_score))
```

## Task 1
Our finance department wants to know the price of the most expensive pair of shoes purchased. Save your answer to the variable `most_expensive`.
```r
# define most_expensive here:
most_expensive <- orders %>%
  summarize(price=max(price, na.rm = TRUE)) #na.rm = TRUE removes all missing values before calculating max value
```

## Task 2
The team now wants to know the number of distinct shoe colors that was sold.
```r
# define num_colors here:
num_colors <- orders %>%
  summarize(num_colors=n_distinct(shoe_color))
```

## Task 3
Now, they want to know the price of the **most expensive shoe** for each `shoe_type`. Name the column that shows the most expensive shoe prices `max_price`.
Save your answer to the variable `pricey_shoes`, and view it.
```r
# define pricey_shoes
max_price <- orders %>%
  group_by(shoe_type) %>%
  summarize(max_price = max(price, na.rm = TRUE))
pricey_shoes <- max_price
pricey_shoes
```

## Task 4
The inventory team wants to know how many of each `shoe_type` has been sold so they can forecast inventory for the future.
Save your answer to the variable `shoes_sold`, and view it.
```r
# define shoes_sold
shoes_sold <- orders %>%
    group_by(shoe_type) %>%
    summarize(count = n())
shoes_sold
```

They also want to find out how many visits came from each source and `month`, which can be found in the `page_visits` data frame.
```r
# define click_source here:
click_source <- page_visits %>%
  group_by(utm_source, month) %>%
  summarize(count = n())
```

## Task 5
At ShoeFly.com, our Purchasing team thinks that certain `shoe_type`/`shoe_color` combinations are particularly popular this year.Find the total number of shoes of each `shoe_type`/`shoe_color` combination purchased. Name the aggregate count column `count`. Save your result to the variable `shoe_counts`, and view it.
```r
# define shoe_counts here:
shoe_counts <- orders %>%
  group_by(shoe_type,shoe_color) %>%
  summarize (count=n())
shoe_counts
```

## Task 6
Find the mean price of each `shoe_type`/`shoe_material` combination purchased.Assign the name `mean_price` to the calculated aggregate. Save your result to the variable `shoe_prices`, and view it.
```r
# define shoe_prices here:
shoe_prices <- orders %>%
  group_by(shoe_type, shoe_material) %>%
  summarize(mean_price=mean(price, na.rm = TRUE))
shoe_prices
```

## Task 7
Your boss at ShoeFly.com wants to gain a better insight into the orders of the most popular `shoe_types`.
Group orders by `shoe_type` and filter to only include orders with a `shoe_type` that has been ordered more than 16 times. Save the result to `most_pop_orders`, and view it.
```r
most_pop_orders <- orders %>%
  group_by(shoe_type) %>%
  filter(n() > 16)
```

## Task 8
You want to be able to tell how expensive each order is compared to the average price of orders with the same `shoe_type`.
Group orders by `shoe_type` and create a new column named `diff_from_shoe_type_mean` that stores the difference in price between an orders price and the average price of orders with the same `shoe_type`. Save the result to `diff_from_mean`, and view it.
```r
# define diff_from_mean here:
diff_from_mean <- orders %>%
  group_by(shoe_type) %>%
  mutate(diff_from_shoe_type_mean = price - mean(price, na.rm = TRUE))
diff_from_mean
```
