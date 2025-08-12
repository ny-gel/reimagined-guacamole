## Matching rows across tables with inner_join()
Using the `inner_join()` function, it:
*  looks for columns that are common between two data frames
*  and then looks for rows where those columns’ values are the same
*  then **combines the matching rows** into a single row in a new table  

What `inner_join()` **keeps**:
* **All columns** from both data frames (with suffixes if there are name conflicts)
* **All `NA` values** in any column, as long as the row has a matching join key  

With the data frames `orders`, `customers` and `products`:
```r
joined_df <- orders %>%
  inner_join(customers) %>%
  inner_join(products) # to create combined df from 3 original df
```

**Renaming columns**
* There might not always be the same column name across data frames.
* To fix this, you will have to `rename` a column in one of the dfs, in order for the `inner_join()` function to work.
```r
customers <- customers %>%
  rename(customer_id = id) # to rename id into customer_id in customer.csv
inner_join(orders, customers) # both dfs to be added, as customers was piped into rename only before
```

## Joining specific columns
**Specifying join condition with `by`:** matches `id` column from `df_1` with `other_id` column from `df_2`. 

The general formula would look like:
```r
joined_dfs <- df_1 %>%
  inner_join(df_2,
             by = c('id' = 'other_id'))
```

Using a specific example, 
```markdown
**DF1:**
| id | name | age |
|----|------|-----|
| 1  | John | 20  |
| 2  | Jane | 25  |

**DF2:**
| student_id | grade |
|------------|-------|
| 1          | A     |
| 2          | B     |
```

In this example, 
```r
students %>%
  inner_join(grades, by = c("id" = "student_id"))
```

### Output
```markdown
| id | name | age | grade |
|----|------|-----|-------|
| 1  | John | 20  | A     |
| 2  | Jane | NA  | B     |
# Bob is removed (no matching grade)
```

**Handling name conflicts with `suffix` :**  
Another example:

```r
products_orders <- products %>%
  inner_join(orders,
            by = c('id' = 'product_id'), # specify join condition 
            suffix = c('_product','_order')) # handle name conflicts when both dfs have columns with same name
```
If both data frames have a `price` column, the result will have:
* `price_product` (price from products table)
* `price_order` (price from orders table)

## For data frames without (perfectly) matching rows
Using the `full_join()` function, for example:
```r
full_joined_dfs <- df_1 %>%
  full_join(df_2)
```
Will include all rows from both tables, even if they don’t match, and any missing values are filled in with `NA`.

## Left Join
Suppose we want to identify which customers are missing phone information. We would want a list of all customers who have `email`, but don’t have `phone`.  
We could get this by performing a Left Join, which includes all rows from the first (left) table, but only rows from the second (right) table that match the first table.
For this command, the order of the arguments matters. If the first data frame is `company_a` and we do a left join, we’ll only end up with rows that appear in `company_a`, and customers from Company B who **are also** members of A. 
```r
left_joined_df <- company_a %>%
  left_join(company_b)
```

## Right Join
* Is the exact opposite of a Left Join, so if we list `company_a` first, and `company_b` second, we will get only all customers from A **who are also** members in B.

```r
  right_joined_df <- company_a %>%
  right_join(company_b)
```
## Reconstructing data frames
We can use the dplyr `bind_rows()` method. This method only works if all of the columns are the same in all of the data frames.

```r
concatenated_dfs <- df1 %>%
  bind_rows(df2)
```
