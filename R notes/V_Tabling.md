# Data Visualization with tidyverse in R

To visualize data, I'm using the `tidyverse` extension for R. You must run `library(tidyverse)` each time you want to run the program.

## Loading the Starwars Dataset

To load the `starwars` dataset:

```r
view(starwars)
```

## Finding the class of a column
You can use:
```r
print(class(df$name))
```

Specifically for the `name` column in the `starwars` dataset:
```r
print(class(starwars$name))
```

## Filtering data

### By height
I'll call the individuals above 100cm as `tall`.
```r
#Define the new variable
tall<- filter(starwars, height = >100)
```

### Converting data
Let's try to convert from metric to imperial.
We will need to define a new variable and use the `mutate` operator.
```r
imp_height <- mutate(starwars, height = 0.0328084 * height)
```

A cleaner way to do this is by using the `%>%` pipe operator. Words flow from left to right, so reading it might be simpler.
```r
imp_height <- starwars %>%
  mutate(height = 0.0328084 * height)
```

The `%>%` pipe operator essentially chains the first function to the next. It takes the output of one statement and makes it the input of the next statement. When describing it, you can think of it as a "THEN".

### Performing more actions on grouped data
A two-step action can then be performed using the pipe operator. See how we group data by `eye_color` and then calculate the `mean` mass of each group:
```r
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
[1] F, F, TRUE, F, TRUE
```
This means that the 3rd and 5th elements in the vector are `NA`.

`!is.na(x)` is the logical negation operator. It reverses (negates) the logical values.
When applied to the above example, the 3rd and 5th elements become `FALSE` when they are `NA`.
In the `starwars` data set, using `!is.na(x)` will select the values that is NOT `NA` / have a numerical number.

```r
starwars %>%
  filter(!is.na(mass))
```
This is very useful when you want to clean your data or remove missing values before performing operations like aggregation, summaries, or plotting.



