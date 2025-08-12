## The ggplot() function
It’s important to understand that any arguments defined inside `ggplot()` will also be inherited by other layers, including aesthetics.

## Data association
* Data is bound to a ggplot2 visualization by passing a data frame as the first argument in the `ggplot()` function call. You can include the named argument like `ggplot(data=df_variable)` or simply pass in the data frame like `ggplot(data frame)`.
* Because the data is bound at this step, this means that the rest of our layers, which are function calls we add with a `+` (plus sign), all have access to the data frame.


For example, assume we have a data frame `sales` with the columns `cost` and `profit`. In this example, we assign the data frame `sales` to the `ggplot()` object that is initailized:
```r
viz <- ggplot(data=sales) + 
geom_point(aes(x=cost, y=profit))
viz # renders plot
```

* The ggplot object or canvas was initialized with the data frame `sales` assigned to it
* The subsequent `geom_point` layer used the `cost` and `profit` columns to define the scales of the axes for that particular geom. Notice that it simply referred to those columns with their column names.
* We state the variable name of the visualization ggplot object so we can see the plot.

## Adding geoms

### Adding scatterplot geom and line-of-best-fit
```r
viz <- ggplot(data=df, aes(x=col1,y=col2)) +
geom_point() +
geom_smooth()
```

## Aesthetics
There are two ways to set aesthetics, by (1) manually specifying individual attributes or by (2) providing aesthetic mappings.

### Using inherited aesthetics (mapping)
```r
viz <- ggplot(data=airquality, aes(x=Ozone, y=Temp)) +
geom_point(aes(color=Month) + 
geom_smooth()
# mapping is wrapped in the aes() function as an additional argument to ggplot()
# subsequent geom layers, geom_point() and geom_smooth() use the scales defined inside the aesthetic mapping assigned at the canvas level.
```
* The code above would only change the **color of the point layer,** it would not affect the color of the smooth layer since the `aes()` aesthetic mapping is passed at the point layer.

### Manual alpha aesthetics
* Sometimes you want to change an aesthetic based on visual preference and not data.
* If you have a pre-determined value in mind, you provide a named aesthetic parameter and the value for that property without wrapping it in an `aes()`.
* To make all points on scatter plot red:
```r
viz <- ggplot(data=airquality, aes(x=Ozone, y=Temp)) +
geom_point(color="darkred")  
```

## Labels
Refer to the labels documentation here: <https://ggplot2.tidyverse.org/reference/labs.html>

An example:
```r
viz <- ggplot(data=movies, aes(x=imdbRating, y=nrOfWins)) +
geom_point(aes(color=nrOfGenre), alpha=0.5) + # Colors according to categorical_value, change transparency of points
labs(title="Movie Ratings Vs Award Wins", subtitle ="From IMDB dataset",x="Movie Rating",y="Number of Award Wins",color="Number of Genre")
# Adds labels
```

## The bar chart
Best used for visualisation of categorical data.

```r
#Create a bar chart
bar <- ggplot(data=mpg, aes(x=class)) + #ggplot automagically designates value/count to y axis
geom_bar(aes(fill=class)) + # Fills color of each bar according to categorical_value; geom_bar informs bar chart
labs(title="Types of Vehicles", subtitle="From fuel economy data for popular car models (1999-2008)") # Add labels
bar
```

## The histogram
Best used to visualise the distribution of continuous/numerical data.
* **Distribution, in particular**: shape, spread and frequency patterns
* **Continuous, numerical data**: While the bar chart is used for categorical data, histograms are good for quantitative data

```r
# create a histogram with data frame _songs_
hist <- qplot(songs,
              geom="histogram",
              main = 'Histogram of Song Lengths',
              xlab = 'Song Length (Seconds)',
              ylab = 'Count',
              fill=I("blue"),
              col=I("red"),
              alpha=I(.2)) +
 				geom_vline(aes(xintercept=364,
                       color=I("blue")),
                   linetype="solid",
                   size=1,
                   show.legend=T) +
				scale_colour_manual(name = "",
                            labels =c("Chicago"),
                            values=c("blue"))
```
