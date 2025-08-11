You’ve seen most of the functions we often use to diagnose a dataset for cleaning. Some of the most useful ones are:

`head()` — display the first 6 rows of the table
`summary()` — display the summary statistics of the table
`colnames()` — display the column names of the table

## Data transformation
### Gather function
`gather()` takes a data frame and the columns to unpack.
```r
df %>%
  gather('Checking','Savings',key='Account Type',value='Amount')
```
```r
df # the data frame you want to gather, which can be piped into `gather()`
Checking, Savings # the columns of the old data frame that you want to turn into variables
key # what to call the column of the new data frame that stores the variables
value # what to call the column of the new data frame that stores the values
````
