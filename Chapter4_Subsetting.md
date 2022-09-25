---
Title: "Chapter 4 - Subsetting"
Author: "Batuhan T. Bozkurt"
Date: "2022-24-09"
---

## Quiz
1. What is the result of subsetting a vector with positive integers, negative integers, a logical vector, or a character vector?

2. What’s the difference between \[, \[\[, and $ when applied to a list?

3. When should you use drop = FALSE?

4. If x is a matrix, what does x\[] <- 0 do? How is it different from x <- 0?

5. How can you use a named vector to relabel categorical variables?

# Exercises
## 4.2.6 
1. Corrections
	1. First one requires double ``==`` for it to be a proper call.
	2. Negative and positive can't be together
	3. Not quite sure
	4. Repeat mtcars$cyl== for 6
2. Because of recycling. NA is a logical type, and logical vectors get recycled to fit with the subsetted vector.
3. upper.tri will return a reversed half-triangle of data:
	1 2 3
	4 5 6
	7 8 9
	:Upper.tri will return
	FALSE TRUE TRUE
	FALSE FALSE TRUE
	FALSE FALSE FALSE
	outer creates a matrix from two lists. 
	```r
	> x <- outer(1:5,1:5, FUN="*")
> x
     [,1] [,2] [,3] [,4] [,5]
[1,]    1    2    3    4    5
[2,]    2    4    6    8   10
[3,]    3    6    9   12   15
[4,]    4    8   12   16   20
[5,]    5   10   15   20   25
```

x[upper.tri(x)] return all the TRUE values. It will go column wise, in a vector
[1]  2  3  6  4  8 12  5 10 15 20

4. mtcars has 11 columns, so 20 will be out of bounds. 
5. 1.  Implement your own function that extracts the diagonal entries from a matrix (it should behave like `[diag(x)]` where `x` is a matrix).

Idea will be to create a logic matrix of TRUE's through the diagonal, and apply it to the matrix
```r
find_diag <- function(x)
{
	min <- min(nrow(x), ncol(x))
	y <- matrix(FALSE, nrow =min, ncol=min)
	co <- 1
	while (co<=min)
	{
		y[co,co] = TRUE
		co=co+1
	}
	x[y]
}
```

6. Would replace all NAs in the database with 0s


## 4.3.5
1. mtcars\[3,2], mtcars\[\[3,2]], mtcars$cyl\[3], 
2. No idea


## 4.5.9
1. We can use sample
   ```r
   # For column random
   x_rand <- x[, sample(ncol(x))] 
   #For both
   x_rand <- x[sample(nrow(x)), sample(ncol(x))] 
```

2. 
   ```r
   x_sample <- x[sample((nrow(x), m), ]

	#For contig:
	start <-
```

3. 
   ```r
   x[sort(names(x))]
```