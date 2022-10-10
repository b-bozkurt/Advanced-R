---
Title: "Chapter 6"
Author: "Batuhan T. Bozkurt"
Date: "2022-10-09"
---

# Quiz
1.  What are the three components of a function?
    Name, input, output
    
2.  What does the following code return?
    
    ```
    x <- 10
    f1 <- function(x) {
      function() {
        x + 10
      }
    }
    f1(1)()
    ```

	Looks like there are two functions conconated within one another. Would still call 20?
	Actual result is 11. No idea why.
1.  How would you usually write this code?
    
    ```
    `+`(1, `*`(2, 3))
    ```

	1+2*3

4.  How could you make this call easier to read?
    
    ```
    mean(, TRUE, x = c(1:10, NA))
    ```

	mean (1:10) ?

5.  Does the following code throw an error when executed? Why or why not?
    
    ```
    f2 <- function(a, b) {
      a * 10
    }
    f2(10, stop("This is an error!"))
    ```
    
    Don't think so. I believe we only initilize an error message for the function IF it would have an error.
    
6.  What is an infix function? How do you write it? What’s a replacement function? How do you write it?
    
    Not sure.
    
7.  How do you ensure that cleanup action occurs regardless of how a function exits?
   
	   Again, not quite sure.
   
# Exercises
## 6.2.5
1. Because names in R are just references - functions are R objects, and R objects can be referred by multiple names. 
2. The second one with parantheses. 
3. Well, I don't have any R code to review. Furthermore, this feels like bad coding - a simple name, even if it will be used only a few times, should make code more readable.
4. ``is.function`` and `is.primitive`
5. The given code results in two lists of about 1300 elements. 
	1. I believe we need to check the ``formals`` for this one. 
	2. Not quite sure what is special about these, but we would need to count for `formals =0` in this case.
	3. Check with `is.primitive()==TRUE`
6. Arguments, body and environment.
7. Primitive functions, and functions in the global environment.




## 6.4.5
1. First c is a function, second will be a name, and third will be equal to 10
2. Name masking, functions vs. variables, a new environment being created whenever you run a function, dynamic lookup
3. I'm expecting this to go from innter to outer - so 10^2 +1 times 2 - 202. Got it right.

## 6.5.4
1. ?
2. Lazy evaluation - we wil start with x = z, setting our z to 100 before x inside the function runs. Returns 100
3. Name masking : when we access the x inside c(), the x={y <- 1;2} is evaluated in the function. This will bind y to 1. Because y has been evaluated, the y=0 will not run. The final result is c(2,1)
4. ?
5. ?
6. No arguments necessary, according to ?library
# Notes

## 6.2.1 Function components
1. Three components of functions: arguments, body, and environment.
2. Three pats of functions:

-   The `[formals()]`, the list of arguments that control how you call the function.
    
-   The `[body()]`, the code inside the function.
    
-   The `[environment()]`, the data structure that determines how the function finds the values associated with the names.

## 6.3 Function composition
1. The magrittr package35 provides a third option: the binary operator `[%>%]`, which is called the pipe and is pronounced as “and then”. 
	1. This allows functions to run one after another with each others output.
```r
 library(magrittr)

x %>%
  deviation() %>%
  square() %>%
  mean() %>%
  sqrt()
#> [1] 0.274
```

