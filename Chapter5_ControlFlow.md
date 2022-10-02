---
Title: "Chapter 5"
Author: "Batuhan T. Bozkurt"
Date: "2022-02-10"
---

# Quiz
1. What is the difference between if and ifelse()?
	An if statement will occur if its TRUE, or else skip. An ifelse statement will do one thing if TRUE, and another if FALSE
	I know about else if, but not ifelse.

2. In the following code, what will the value of y be if x is TRUE? What if x is FALSE? What if x is NA?
	```r
	y <- if (x) 3
	```

	y will be set to 3 if x is TRUE. Not sure about NA - likely an error.

3. What does switch("x", x = , y = 2, z = 3) return?

	Switch statements work as if they are nested if statements. Not quite sure of this one would work with x not having anything set to. 

# Exercises
## 5.2.4
1. What type of vector does each of the following calls to ifelse() return?
   ```r
	ifelse(TRUE, 1, "no")
	ifelse(FALSE, 1, "no")
	ifelse(NA, 1, "no")
```

	Double, character and logical. The output type depends on the test itself - it will start with logical, then coerce according to the type of the yes answer, then according to the type of the no answer. For instance, if we change the test into a "1", the output will be a logical output.

2. Why does the following code work
```r
x <- 1:10
if (length(x)) "not empty" else "empty"
#> [1] "not empty"

x <- numeric()
if (length(x)) "not empty" else "empty"
#> [1] "empty"
```

Length of x is >0, which acts as TRUE? On the second run, x is set to an empty numeric vector, and the 0 length acts as a FALSE.


## 5.3.3
1. Why does it work?
   ```r
x <- numeric()
out <- vector("list", length(x))
for (i in 1:length(x)) {
  out[i] <- x[i] ^ 2
}
out
```

Because : can also run decreasing sequences. In this case, i starts as 1, then loops to 0. Square of 0 returns NA, and because we have reached the end of the sequence, the for loop breaks off.

2. When the following code is evaluated, what can you say about the vector being iterated?

```r
xs <- c(1, 2, 3)
for (x in xs) {
  xs <- c(xs, x * 2)
}
xs
#> [1] 1 2 3 2 4 6`
```

Each loop will multiply each element in the vector, and then combine the original vector with the new multiplied one.

3.  What does the following code tell you about when the index is updated?
    
    ```r
    for (i in 1:3) {
      i <- i * 2
      print(i) 
    }
    #> [1] 2
    #> [1] 4
    #> [1] 6
    ```

Each line that is printed will be one update of the index?

# Notes
1. When switch statements have emptly "answers", the output will be copied from the next switch with actual output
   ```r
   legs <- function(x) {
  switch(x,
    cow = ,
    horse = ,
    dog = 4,
    human = ,
    chicken = 2,
    plant = 0,
    stop("Unknown input")
  )
}
legs("cow")
#> [1] 4
legs("dog")
#> [1] 4
```

2. When running `for` loops, make sure to initilize the output outside of the loop - speeds up loops.
	```r
means <- c(1, 50, 20)
out <- vector("list", length(means))
for (i in 1:length(means)) {
  out[[i]] <- rnorm(10, means[[i]])
}
```
