You’ve seen most of the functions we often use to diagnose a dataset for cleaning. Some of the most useful ones are:

`head()` — display the first 6 rows of the table  
`summary()` — display the summary statistics of the table  
`colnames()` — display the column names of the table  
`count()` - display counts of unique values in a column

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
### Dealing with duplicates
**Finding duplicates:**
```r
# finding duplicates
duplicates <- df %>%
  duplicates()
```
  
How many rows are duplicates? We can use the `table()` function, which returns a table with a count of each unique value in the object. Here, look for the number of FALSE & TRUE, where TRUE indicates duplicates.

```r
# count duplicated rows
duplicate_counts <- duplicates %>%
  table()
```

**Removing duplicates:** 

We can use the dplyr `distinct()` function to remove all rows of a data frame that are duplicates of another row.  

```r
`fruits %>% distinct()`
```
The `apple` row was deleted because it was exactly the same as another row. But the two `peach` rows remain because there is a difference in the price column.  
  

**To keep only the first occurrence of the duplicate:**
```r
fruits %>%
  distinct(item,.keep_all=TRUE)
# item refers to the col name
```

## Splitting by index
Let’s say we have a column `birthday` with data formatted in MMDDYYYY format. We want to split this data into day, month, and year so that we can use these columns as separate features.  Use `str_sub()` to do so.

```r
# A clean way to do all three at once
df %>%
  mutate(month = str_sub(birthday,1,2),
  day = str_sub(birthday,3,4),
  year = str_sub(birthday,5))

# drop unnecessary column
df %>%
  select(-birthday)

```
* first command takes the characters starting at index 1 and ending at index 2 of each value in the birthday column -> month column.  
* second command takes the characters starting at index 3 and ending at index 4 of each value in the birthday column -> day column.  
* third command takes the characters starting at index 5 and ending at the end of the value in the birthday column -> year column.  

**When characters are not constant**  
 The general formula for the `separate()` function is:
```r
df %>%
  separate(column_to_split, c('new_col1', 'new_col2', 'new_col3'), 'separator')
```  
e.g. `user_US` and `admin_Kenya`, we know we want to split across `_`, 
```r
# Create the 'user_type' and 'country' columns
df %>%
  separate(type,c('user_type','country'),'_')

# type is the pre-existing column to split
# c('user_type','country') is a vector with the names of the two new columns
# '_' is the character to split on
```
e.g. Now we want to split first and last names from an initial column called `full_names`.  
The `extra ='merge'` argument will ensure that two-word last names & middle names will end up in the `last_name` column.
```r
# separate the full_name column
students <- students %>%
  separate(full_name,c('first_name', 'last_name'),' ', extra ='merge')
```

## String parsing
Use the `gsub()` function to remove characters, and replace with other characters. Note how `` and "(space)" mean different things - the former means replace with nothing, and the latter means replace with a space.  

The general formula is:
```r
gsub(pattern, replacement, x)
```
* `pattern` = what you want to find/replace
* `replacement` = what you want to replace it with
* `x` = the text/vector where you're making changes  


In this example, we want to remove the % from the pre-existing `score` column and convert the  character class -> numerical class with `as.numeric()`.

```r
# remove % from score column
students <- students %>%
  mutate(score=gsub('\\%','',score))
head(students)

# change score column to numeric
students <- students %>%
  mutate(score = as.numeric(score))
students

# do both steps together
students <- students %>%
  mutate(score = as.numeric(gsub('\\%', '', score)))
```
