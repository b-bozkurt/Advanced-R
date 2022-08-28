---
Title: "Chapter 2"
Author: "Batuhan T. Bozkurt"
Date: "2022-08-28"
---

## Quiz
1. Given the following data frame, how do I create a new column called “3” that contains the sum of `1` and `2`? You may only use `[$]`, not `[[`. What makes `1`, `2`, and `3` challenging as variable names?
2. In the following code, how much memory does `y` occupy?
```R
x <- runif(1e6)
y <- list(x, x, x)
```
3. On which line does a get copied in the following example?
```
a <- c(1, 5, 3, 2)
b <- a
b[[1]] <- 10
```

## Exercises
#### 2.2.2 Binding
1.  Explain the relationship between `a`, `b`, `c` and `d` in the following code:
    
    ```
    a <- 1:10
    b <- a
    c <- b
    d <- 1:10
    ```

	**Answer:** a, b and c reference to one particular object that contains numbers between 1 to 10. d refers to another distinct object that refers to the same set of numbers.

2.  The following code accesses the mean function in multiple ways. Do they all point to the same underlying function object? Verify this with `lobstr::obj_addr()`
    
    ```
    mean
    base::mean
    get("mean")
    evalq(mean)
    match.fun("mean")
    ```

	**Answer:** These all refer to the same function.

3.  By default, base R data import functions, like `read.csv()`, will automatically convert non-syntactic names to syntactic ones. Why might this be problematic? What option allows you to suppress this behaviour?

	**Answer:** Not entirely sure about the first part - but converting non-syntactic names might cause incompatability issues with non-R code? The option check.names = FALSE will suppress this behavior.

4.  What rules does `make.names()` use to convert non-syntactic names into syntactic ones?

	**Answer:** Depends on the current locale. Names must start with a letter or dot (dot cannot be followed by a number). Can have _.

5.  I slightly simplified the rules that govern syntactic names. Why is `.123e1` not a syntactic name? Read `?make.names` for the full details.

	**Answer:** Names can start with a dot or underline, but only if the dot/underline is not followed by a number. 


#### 2.3.6 Copy-on-modify
1. Why is `tracemem(1:10)` not useful?

    **Answer:** Because the adress of the original object will always stay the same. If any modifications are made, they will be made on a copy.
    
2.  Explain why `tracemem()` shows two copies when you run this code. Hint: carefully look at the difference between this code and the code shown earlier in the section.

    ```
    x <- c(1L, 2L, 3L)
    tracemem(x)
    
    x[[3]] <- 4
    ```

	**Answer:** Likely because these are strings within the x variable. These are likely refering to the character vectors? Not 100% sure

3.  Sketch out the relationship between the following objects:
    
    ```
    a <- 1:10
    b <- list(a, a)
    c <- list(b, a, 1:10)
    ```

	**Answer:** If we were to call the first 1:10 object x, a refers to x. b is a list that refers to x twice. c is a list that refers to x twice, and a new object identical to x

4.  What happens when you run this code?
    
    ```
    x <- list(1:10)
    x[[2]] <- x
    ```
    
    **Answer:** On line two, a list identical to the 1:10 list in line 1 will be created. This new list will reference to the original list object in its second element. Because each name can only refer to one object, the original list object becomes unbound from x (but is still referred in the second element of the new list)

#### 2.4.1 Object size

1.  In the following example, why are `object.size(y)` and `obj_size(y)` so radically different? Consult the documentation of `object.size()`.
    
    ```
    y <- rep(list(runif(1e4)), 100)
    
    object.size(y)
    #> 8005648 bytes
    obj_size(y)
    #> 80,896 B
    ```
    
    **Answer:** Because object.size() provides an estimate while obj_size gives an exact value that is calculated depending on the current situation. Object.size() does not account for shared references.
    
2.  Take the following list. Why is its size somewhat misleading?
    
    ```
    funs <- list(mean, sd, var)
    obj_size(funs)
    #> 17,608 B
    ```
    
    **Answer:** We are only seeing the size of list elements. However its super high even for that?
    
3.  Predict the output of the following code:
    
    ```
    a <- runif(1e6)
    obj_size(a)
    
    b <- list(a, a)
    obj_size(b)
    obj_size(a, b)
    
    b[[1]][[1]] <- 10
    obj_size(b)
    obj_size(a, b)
    
    b[[2]][[1]] <- 10
    obj_size(b)
    obj_size(a, b)
    ```
    
    **Answer:** 
    It will all depend on the number of elements in each list.
    1.  A is a list of 1 million doubles. An empty list with 1 null element has a size of 48 b. 1 million doubles is 1 mb= 8,000,048 B
    2.  b will refer to the same object within a twice. It will be size of a + size of a list with two empty elements (which is 112)
    3.  Because there are shared elements, obj size will not change for obj_size(a,b)
    4.  Changing the first element of b modifies it to (10,a) and initiates copy protection. Instead of referencing to a, b[1][1] is now referencing to a new object containing 1 million of the same doubles. So double the size
        1.  ask about this one
    5.  b[2] is still referencing to a, so in obj_size(a,b), b has a in it, and size won’t change.
    6.  Chancing the second element of b will modify the adress again - now, we have a third object of 1 million doubles. Obj_size(b) is still 16 mb, because its now refering to the second and third million objects
    7.  Obj(a,b) changes to 24 mb, because it refers to three distinct 1 mils.

#### 2.5.3 Modify in Place
1.  Explain why the following code doesn’t create a circular list.
    
    ```
    x <- list()
    x[[1]] <- x
    ```
    
 **Answer:**    When we modify x by trying to add it to itself, a copy of x is created as part of copy-on-modify. Because this copy is a distinctly object separate from the original list, the code does not create a circular list.
    
2.  Wrap the two methods for subtracting medians into two functions, then use the ‘bench’ package17 to carefully compare their speeds. How does performance change as the number of columns increase?
    
**Answer:** No idea - apparently lists are faster because they are not copied over and over again. But converting a db to list takes processing power too, so the benefit only appears after 300+ columns in a db.
    
3.  What happens if you attempt to use `tracemem()` on an environment?

**Answer:** Environments are always modified in place, so tracemem does nothing.
