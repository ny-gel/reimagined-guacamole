# R

R is a programming language and software environment for statistical computing and graphics. It is widely used for data analysis, visualization, and predictive modeling.

### Basics of variables
```r
# Define a and b
a <- 5
b <- 10
```

To add variables, simply write them out:

```r
a + b
```

Using if, else and print functions:
```r
a + b
  if (a > b) {
    print ("Variable a is greater than b")
  } else if (a < b) {
    print ("Variable a is less than b")
  } else if (a == b) {
    print ("Variable a is equal to b")
  }
```

## Conditionals - `for`, `if`, `else`
In R, we will often perform tasks based on a condition. We can perform a task by using an `if` statement. 

- if keyword followed by parentheses `()`, which is followed by a code block, or block statement, indicated by a set of curly braces `[]`
- inside the parentheses, a condition is provided which is either `TRUE` or `FALSE`
- if the condition is TRUE, the code inside the curly braces runs; if FALSE, it is not executed.
- the `else` keyword **follows** the code block of an `if` statement, and has code block wrapped by a set of curly braces
- the code block inside the `else` statement code will execute when the if statement condition evaluates to FALSE.


### Skip loops

```r
for (i in 1:10) {
  if (i == 3) {
    next
  }
  print (i)
}
```

### Break loops
```r
for (i in 1:5) {
  if (i == 3) {
    cat("breaking at 3")
    break
  }
}
```
It's important that `break` is only used after `cat()` as the program exits the loop immediately and will not print the statement you want.

  
## Vectors

Vectors are one-dimensional arrays of data in R. They are a list-like structure that contain items of the **same data type**.

### Creating a vector:
```r
vector1 <- c(1, 2, 3, 4)
```
`c()` is used to concatenate values into a vector (numbers, strings) and keeps them separate.
`paste()` is used to join into a <u> single <u> string.


### Accessing elements of a vector:
If we wanted to access the second element of `vector(1)`,
```r
vector1[2] # returns 2, see above vector
```


### Operations for vectors
* `sum()`
* `mean()`
* `paste()`
* 
```r
sum(vector1) # returns 10
mean(vector1) # returns 2.5
paste ("Hello", vector1) # returns Hello 1, Hello 2, etc.
```

**Sum values in a vector:**
```r
x <- c(5, 8, 12, 105, 7)
total <- 0
for (val in x) {
  if (val > 100) {
    cat("Value greater than 100\n")
    break
  }
  total <- total + val
}
```

**Skipping missing values in a vector:**
```r
names <- c("John", "Jane", NA, "Doe", "Alice", NA)
for (name in names) {
  if (is.na(name)) {
    next
  }
  print(name)
}
```

**Checking length of a vector:**
`length(vector_name)`


## Matrices

Matrices are two-dimensional arrays of data in R.

```r
matrix1 <- matrix(1:6, ncol = 2) # Create a matrix
matrix1[2, 2] # returns 4, access element
colSums(matrix1) # returns the sum of each column
rowMeans(matrix1) # returns the mean of each row
```


## Data Frames

Data frames are two-dimensional arrays with named columns, similar to tables in a relational database.

```r
# Create a data frame
data.frame1 <- data.frame(Name = c('John', 'Jane', 'Jim'),
                         Age = c(30, 28, 35),
                         Gender = c('Male', 'Female', 'Male'))

# Access elements of a data frame
data.frame1$Name # returns the Name column

# Perform operations on a data frame
aggregate(Age ~ Gender, data = data.frame1, mean) # returns the mean Age by Gender
```


