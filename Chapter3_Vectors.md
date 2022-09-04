---
Title: "Chapter 3"
Author: "Batuhan T. Bozkurt"
Date: "2022-09-02"
---

# Quiz
1.  What are the four common types of atomic vectors? What are the two rare types?
	    Answer: I'd assume integer, string, boolean and maybe list vectors would be common. Rare, DB vectors? 
2.  What are attributes? How do you get them and set them?
    Dimension and class
3.  How is a list different from an atomic vector? How is a matrix different from a data frame?
    Atomic vectors only take one type of element
4.  Can you have a list that is a matrix? Can a data frame have a column that is a matrix?
    N/A
5.  How do tibbles behave differently from data frames?
	N/A
# Exercises
### 3.2.5
1. How do you create raw and complex scalars? (See ?raw and ?complex.)
	**Answer:** Raw vectors hold raw bytes. They are created with the ``raw()`` operator, where you put the desired length of the vector as the argument.
	
	Complex is used for complex arithmetic in R, such as real, imaginary, modulus. They are created with `complex()`, with arguments for its real and imaginary parts
```r
z <- complex(real = stats::rnorm(100), imaginary = stats::rnorm(100))
```

2. Test your knowledge of the vector coercion rules by predicting the output of the following uses of c():
```r
c(1, FALSE)
c("a", 1)
c(TRUE, 1L)
```

   **Answer:** Coercion order is character → double → integer → logical
   1,1
   "a", "1"
   1L, 1L (Will display 1, 1 but the contents will be integers)

3. Why is 1 == "1" true? Why is -1 < FALSE true? Why is "one" < 2 false?
	
	**Answer:** Double equals sign tells that if types are different, coerce and compare pure values. Triple === will escape this behavior.
	False will be coerced into 0, making the comparison true. 
	Third one will coerce both to characters. When comparing characters, R sorts alphabetically. Numbers come after letters.
	
4. Why is the default missing value, NA, a logical vector? What’s special about logical vectors? (Hint: think about c(FALSE, NA_character_).)
	 
	**Answer:** Logical vectors are at the very end of the coercion list, which might be helpful in not coercing the whole list into something else. The example given will chance FALSE into "FALSE"
	
5. Precisely what do is.atomic(), is.numeric(), and is.vector() test for?

	**Answer:** is.atomic test is the given argument is of an atomic type (TRUE for all six types, four basic and two complex)
	
	is.numeric tests "if an object can be interpreted as a number". It test AND coerces objects of type "numeric"
	
	is.vector only return TRUE if the tested vector is of the specified mode, and has no attributes other than names.

### 3.3.4
1.  How is `setNames()` implemented? How is `unname()` implemented? Read the source code.

	setNames simply writes the desired name to the names vector of the object
	
	Unname check if the names() vector for the argument is null. If false, it will rewrite it with NULL. If the additional boolean argument is given to the function, it will also NULL the dimension names

2.  What does `dim()` return when applied to a 1-dimensional vector? When might you use `NROW())` or `NCOL()`?
    
    If the vector was not given a dimension, it will return NULL. If it was assigned 1, it will return 1. NROW and NCOL are useful to create matrices
    
3.  How would you describe the following three objects? What makes them different from `1:5`?
    
    ```r
    x1 <- array(1:5, c(1, 1, 5))
    x2 <- array(1:5, c(1, 5, 1))
    x3 <- array(1:5, c(5, 1, 1))
    ```

	x  <- 1:5 would have a dimension of NULL. We can imagine these as 3D objects
	1. First will have x, y dimension as 1, z as 5. Printing it would create a large output, printing 5 1x1 matrices with as single number each.
	2. Second will print a row of 1:5
	3. Last will print a column of 1:5

4.  An early draft used this code to illustrate `structure()`:
    
    ```r
    structure(1:5, comment = "my attribute")
    #> [1] 1 2 3 4 5
    ```
    
    But when you print that object you don’t see the comment attribute. Why? Is the attribute missing, or is there something else special about it? (Hint: try using help.)

	Comments are not printed by structure



### 3.4.5
1.  What sort of object does `table()` return? What is its type? What attributes does it have? How does the dimensionality change as you tabulate more variables?

	Table() creates a `contigency table`, which displays the frequency of the objects it was given. It returns "integer". Attributes will be `dim` and `dimnames`. # of dimensions = the number of unique values (one to display each frequency)

2.  What happens to a factor when you modify its levels?
    
    ```r
    f1 <- factor(letters)
    levels(f1) <- rev(levels(f1))
    ```

	According to the example given in the book, the factor itself contains placeholders (1= first level, 2=  second level etc.). So, a 1,2,2,1 factor means put level one, two, two, then one.
	If we change a level, we will change what that factor prints. In this instance, reverse the levels

3.  What does this code do? How do `f2` and `f3` differ from `f1`?
    
    ```r
    f2 <- rev(factor(letters))
    
    f3 <- factor(letters, levels = rev(letters))
    ```
	rev reverses the order of items. f2 will reverse the order. "letters" is a-z alphabeth
	f1 - a-z
	f2- z-a
	In f3, the factor is correct, but levels are reversed. Like, the encoding is correct, but the key decypts it in reverse. It will print a-z (I was expecting z-a).
	So, it seems like reversing the key when creating the factor lets R assign the correct values. So, factor as integer is 26-1, because order or operations is first create the key, then encode it into integers.

### 3.5.3
1. List all the ways that a list differs from an atomic vector.
	Lists hold references to objects, while atomics hold the objects themselves.
	Lists can contain references to different types of objects, while atomics are limited to a single type.
	
2. Why do you need to use ``unlist()`` to convert a list to an atomic vector? Why doesn’t ``as.vector()`` work?

	Lists are considered vectors for as.vector.

3. Compare and contrast ``c()`` and ``unlist()`` when combining a date and date-time into a single vector.

	c() coerces types and strips time zones. This is due to how date and date-time is stored internally. Date-time counts from epoch (1970-01-01) in seconds, so when stripped of everything else, c() only combines a single large number - the amount of seconds passed since epoch.

### 3.6.8
1. Can you have a data frame with zero rows? What about zero columns?
	Yes, without any issues.
	
2. What happens if you attempt to set rownames that are not unique?
	Gives an error, does not execute.
	
3. If df is a data frame, what can you say about t(df), and t(t(df))? Perform some experiments, making sure to try different column types.
	t is the transpose function. Transposing once turns row names into column names, twice returns back. However, during the initial transposion, all my values got turned into strings. This is due to the df turning into a matrix, and matrices can only contain a single type of element.

4. What does as.matrix() do when applied to a data frame with columns of different types? How does it differ from data.matrix()?

	Coerced all elements to a single type, string. data.matrix() did the same, but this time coerced them all into integers. That is listed in the data.matrix() function (it coerces everything to numeric type)
	
	
# Notes
## Intro
1. Vectors come in two types
	1. Atomic vectors: All elements must be the same type
	2. Lists: Elements can be different types
2. Vectors have attributes
	1. Dimension: Turns vectors into matrices and arrays
	2. Class: Powers the S3 object system

## 3.2 Atomic vectors
1. Logical, integer, double and character vectors form the basics. 
2. Rare types are complex and raw vectors

#### 3.2.1 Scalars: How to add individual values to vectors
1. Logicals : T,F or TRUE FALSE
2. Doubles: Decimal or scientific. Three specials: Inf, -Inf, NaN(not a number)
3. Integers: Write as normal, follow by L. Cannot contain fractions.
4. Strings are surrounded by "" or ''

#### 3.2.2 Making longer vectors with `c()`
```r
lgl_var <- c(TRUE, FALSE)
int_var <- c(1L, 6L, 10L)
dbl_var <- c(1, 2.5, 4.5)
chr_var <- c("these are", "some strings")
```
1. When the inputs are atomic vectors, c() always creates another atomic vector; i.e. it flattens:
```r
c(c(1, 2), c(3, 4))
#> [1] 1 2 3 4
```
2. Use `typeof()` for vector types and `length()` for length
3. Missing values are presented as NA
   
#### 3.2.3 Missing values
5. Use `is.na()` to figure if a vector has NA in it
```R
is.na(x)
#> [1]  TRUE FALSE  TRUE FALSE
```

#### 3.2.4 Testing and coercion
1. Generally, you can test if a vector is of a given type with an is.\*() function, but these functions need to be used with care. is.logical(), is.integer(), is.double(), and is.character()

```r
str(c("a", 1))
#>  chr [1:2] "a" "1"
```

2. Coercion often happens automatically. Most mathematical functions (+, log, abs, etc.) will coerce to numeric. This coercion is particularly useful for logical vectors because TRUE becomes 1 and FALSE becomes 0.
3. character → double → integer → logical

   ```r
x <- c(FALSE, FALSE, TRUE)
as.numeric(x)
#> [1] 0 0 1

# Total number of TRUEs
sum(x)
#> [1] 1

# Proportion that are TRUE
mean(x)
#> [1] 0.333
```

3. Generally, you can deliberately coerce by using an as.\*() function, like as.logical(), as.integer(), as.double(), or as.character(). Failed coercion of strings generates a warning and a missing value:

```r
as.integer(c("1", "1.5", "a"))
#> Warning: NAs introduced by coercion
#> [1]  1  1 NA
```
## 3.3 Attributes
### 3.3.1 Getting and setting
1. Attributes are name-value pairs that attach metadata to an object.
2. Individual atributes can be retrieved and modified with ``attr()``
3. Multiple attributes can be retreived with `attributes()` and set en masse with `structure()`

```r
a <- 1:3
attr(a, "x") <- "abcdef"
attr(a, "x")
#> [1] "abcdef"

attr(a, "y") <- 4:6
str(attributes(a))
#> List of 2
#>  $ x: chr "abcdef"
#>  $ y: int [1:3] 4 5 6

# Or equivalently
a <- structure(
  1:3, 
  x = "abcdef",
  y = 4:6
)
str(attributes(a))
#> List of 2
#>  $ x: chr "abcdef"
#>  $ y: int [1:3] 4 5 6
```

1. Attributes usually get lost during operations.
	1. Two attributes are routinely preserver: **names** and **dim** (dimensions)

### 3.3.2 Names
Naming vectors
```R
# When creating it: 
x <- c(a = 1, b = 2, c = 3)

# By assigning a character vector to names()
x <- 1:3
names(x) <- c("a", "b", "c")

# Inline, with setNames():
x <- setNames(1:3, c("a", "b", "c"))
```

* Remove names with unname()

### 3.3.3 Dimensions
* You can create matrices and arrays with matrix() and array(), or by using the assignment form of ``dim()``
  
```r
# Two scalar arguments specify row and column sizes
x <- matrix(1:6, nrow = 2, ncol = 3)
x
#>      [,1] [,2] [,3]
#> [1,]    1    3    5
#> [2,]    2    4    6

# One vector argument to describe all dimensions
y <- array(1:12, c(2, 3, 2))
y
#> , , 1
#> 
#>      [,1] [,2] [,3]
#> [1,]    1    3    5
#> [2,]    2    4    6
#> 
#> , , 2
#> 
#>      [,1] [,2] [,3]
#> [1,]    7    9   11
#> [2,]    8   10   12

# You can also modify an object in place by setting dim()
z <- 1:6
dim(z) <- c(3, 2)
z
#>      [,1] [,2]
#> [1,]    1    4
#> [2,]    2    5
#> [3,]    3    6
```

* A vector without a `dim` attribute set is often thought of as 1-dimensional, but actually has `NULL` dimensions.

## 3.4 S3 atomic vectors
1. Having the `class` attribute will change the vector into an S3 object, making it behave differently if passed to a generic vector
2. In this section, we’ll discuss four important S3 vectors used in base R:
	-   Categorical data, where values come from a fixed set of levels recorded in **factor** vectors.
	-   Dates (with day resolution), which are recorded in **Date** vectors.
	-   Date-times (with second or sub-second resolution), which are stored in **POSIXct** vectors.
	-   Durations, which are stored in **difftime** vectors.
### 3.4.1 Factors
1. Factors are vectors that can only contain predefined values. Each "category" is labeled as a level.
```r
x <- factor(c("a", "b", "b", "a"))
x
#> [1] a b b a
#> Levels: a b

typeof(x)
#> [1] "integer"
attributes(x)
#> $levels
#> [1] "a" "b"
#> 
#> $class
#> [1] "factor"
```

1. Ordered factors are similar to factors, buth the levels have predefined relationships to one another.
```r
grade <- ordered(c("b", "b", "a", "c"), levels = c("c", "b", "a"))
grade
#> [1] b b a c
#> Levels: c < b < a
```

### 3.4.2 Dates
1. They are built on top of double vectors. Class "Date", no other attributes.
```r
today <- Sys.Date()

typeof(today)
#> [1] "double"
attributes(today)
#> $class
#> [1] "Date"
```

2. Value of the double counts from the number of days since 1970-01-01

### Date-time
```r
now_ct <- as.POSIXct("2018-08-01 22:00", tz = "UTC")
now_ct
#> [1] "2018-08-01 22:00:00 UTC"

typeof(now_ct)
#> [1] "double"
attributes(now_ct)
#> $class
#> [1] "POSIXct" "POSIXt" 
#> 
#> $tzone
#> [1] "UTC"
```
You can edit the tzone attribute to format the printed time zone.

### Durations
```r
one_week_1 <- as.difftime(1, units = "weeks")
one_week_1
#> Time difference of 1 weeks

typeof(one_week_1)
#> [1] "double"
attributes(one_week_1)
#> $class
#> [1] "difftime"
#> 
#> $units
#> [1] "weeks"

one_week_2 <- as.difftime(7, units = "days")
one_week_2
#> Time difference of 7 days

typeof(one_week_2)
#> [1] "double"
attributes(one_week_2)
#> $class
#> [1] "difftime"
#> 
#> $units
#> [1] "days"
```


## 3.5 Lists
1. Lists can have leements with different types. The actual elements of the list are actually of the same type, because they are all references to other objects.
2. Can be recursive (contain other lists).
4. c() combines several lists into one. c will also coerce lists into one.


## 3.6 Data frames and tibbles
1. In contrast to lists, data frames must be rectangular - length of each of its vectors must be the same.
2. names() are the column names, length() is the number of columns.
3. Tibbles were created to fix some issues with dataframes. They come with the tibble package.
4. Creating a tibble is similar. Here are the differences:
	1. Tibbles do not coerce their input
	2. Instead of converting non-syntactic names, tibbles only add backticks \`\`. 
	3. Length of each vector still needs to be the same. Tibbles only fix this if the mismatched vector has a length of 1 (in which case it will repeat that vector as many times it needs to fit)
	4. Tibble also allows you to refer to variables created during construction. Inputs are evaluated left to right.,
### 3.6.4 Subsetting
1. Tibbles tweak these behaviours so that a ``[`` always returns a tibble, and a ``$`` doesn’t do partial matching and warns if it can’t find a variable (this is what makes tibbles surly).
2. A tibble’s insistence on returning a data frame from ``[`` can cause problems with legacy code, which often uses ``df[, "col"] ``to extract a single column. If you want a single column, I recommend using ``df[["col"]]``. This clearly communicates your intent, and works with both data frames and tibbles.

