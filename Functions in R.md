##Functions in R

The function in R consists of three separate components:

```r
function_name <- function(parameters){
  function body
}
```

##Function Name
Basically what is used to call the function.

## Function parameters aka formal arguments
Are the variables that are placed inside the parentheses, and separated with a comma that will be set to actual values (called arguments).
For instance,

```r
# To find the circumference of a circle
circumference <- function(r){
  2*pi*r
}
print(circumference(2))
```
[1] 12.56637

The parameters can also be set to specific values. Using the same example,
```r
# To find the circumference of a circle with r = 1
circumference <- function(r=1){
  2*pi*r
}
print(circumference())
```
[1] 6.283185
If we call the function with no argument passed, it will calculate the circumference of a unit circle. Otherwise, it will calculate with the provided radius 'r'.

##Function Body
Is the set of commands inside the curly braces that run in a predefined manner everytime we call the function.

```r
#Sum of two numbers in a function
sum_two <- function(x,y) {
  x + y
}
print(sum_two(1,2))
```
[1] 3
This allows us to call the function by position by following the same sequence of arguments as in the function.
i.e. x = 1 and y = 2, and not vice versa.

If we pass the arguments by name (by explicitly stating what value each parameter is defined in the function), the order of arguments does not matter.

```r
subtract_two_nums <- function(x, y){
    x - y
}
print(subtract_two_nums(x=3, y=1))
print(subtract_two_nums(y=1, x=3))
```

It's also possible to mix position and name-based matching of the arguments when calling the function. Using the example of calculating BMR in a 30y F:

```r
calculate_calories_women <- function(weight, height, age=30){
    (10 * weight) + (6.25 * height) - (5 * age) - 161
}
print(calculate_calories_women(60, 165))
```
Since one of the parameters has a defined assigned value to it, when we pass two arguments to the function, R interprets the third missing argument should be set to its default value. 


###Joining functions
1. Passing the output from calling one function directly to anther
```r
radius_from_diameter <- function(d){
    d/2
}
circumference <- function(r){
    2*pi*r
}
print(circumference(radius_from_diameter(4)))
```

##Nested functions
Occurs when we define a new function inside another function. For instance, we can sum up the area of three circles:

```r
sum_circle_ares <- function(r1, r2, r3){
    circle_area <- function(r){
        pi*r^2
    }
    circle_area(r1) + circle_area(r2) + circle_area(r3)
}
print(sum_circle_ares(1, 2, 3))
```

Here, we defined `circle_area` function inside the `sum_circle_ares` function. We then called that inner function three times, (r1/2/3) inside the outer function to calculate the area for each circle, before summing them up.
If we try to call `circle_area` outside of the function, it throws an error because the inner function can only work inside the function it was called.


