# R Coding

Archived: Yes
Created: August 18, 2022 10:13 AM
Materials: https://campus.datacamp.com/courses/free-introduction-to-r/chapter-1-intro-to-basics-1?ex=3
Reviewed: Yes

- Basics
    
    Aritmetics are conducted in line without any issues
    
    %% for modulo
    
    Variables are assigned with < - 
    
    ![Untitled](Chapter2NamesAndValues/Untitled.png)
    
    Booleans are CASE SENSITIVE
    
    Check class of variables with class()
    
    Create vectors with the combine function c(),
    
    example : 
    
    vector_x ← c(”a”, “b”, “c”)
    
    names() function can be used to assign names to the variables within a vector. 
    
    ```
    some_vector <- c("John Doe", "poker player")
    names(some_vector) <- c("Name", "Profession")
    ```
    
    Adding two vectors aritmethically takes the element-wise sum (first element is added with first, so on and so forth).
    
    Vector index does not start from 0, it starts from 1
    
    sum() adds up all elements within an array
    
    ```r
    # Construct a matrix with 3 rows that contain the numbers 1 up to 9
    matrix(1:9,byrow=TRUE, nrow=3)
    ```
    
    cbind() - combines column of two matrices together
    
    rbind() - same for rows
    
    Factors : With the use of factor() function, one can group responses into levels. These levels are initially typed out in alphebetical order. With the use of levels() function, one can rename the levels.
    
    Datasets:
    
    Can contain multiple types of variables
    
    head() and tail() for top/bottom 10
    
    str() for info about the dataset
    
    data.frame() to add vectors into a dataset
    
    For both vectors and datasets
    
    [1,2]- single element, 1st row 2nd column
    
    [1,] - entirety of the first row
    
    [,1]- entirety of the first column
    
    [1:3,2:4] - everything betwene rows 1,2,3; columns 2,3,4
    
    subset(dataset, subset = condition) - extracts data from a dataset that fullfills a boolean condition
    
    example: subset(dataset_x, subset= diameter < 1) Note that you write the conditions column name without “”
    
    list() - can store datasets, and everything else really
    
- Chapter 2
    
    The ← code “binds” an object to a reference. So, in x <- c(1, 2, 3), you don’t create an object named x with values 1,2,3 but instead reference an object of 1,2,3 with x.
    
    ![Untitled](R%20Coding%20b9798d0f9b8546efbeb0e1e379a4322d/Untitled%201.png)
    
    - Exercise 2.2.2
        
        ![Untitled](R%20Coding%20b9798d0f9b8546efbeb0e1e379a4322d/Untitled%202.png)
        
        Answer: a, b and c reference to one particular object that contains numbers between 1 to 10. d refers to another distinct object that refers to the same set of numbers.
        
        ![Untitled](R%20Coding%20b9798d0f9b8546efbeb0e1e379a4322d/Untitled%203.png)
        
        Answer: These all refer to the same function
        
        1. Not entirely sure about the first part - but converting non-syntactic names might cause incompatability issues with non-R code? The option check.names = FALSE will suppress this behavior.
        2. Depends on the current locale. Names must start with a letter or dot (dot cannot be followed by a number). Can have _. 
        3. Names can start with a dot or underline, but only if the dot/underline is not followed by a number.
        
    - 2.3 Copy-on-modify
        
        Turns out, if you modify a reference of a reference, that will not alter the original object. Instead, R will create a copy of the original object and alter that instead.
        
        x <- c(1, 2, 3)
        y <- x
        
        y[[3]] <- 4
        x
        #> [1] 1 2 3
        
        tracemem() ile bir objeyi sürekli olarak takip edebiliyoruz. İlk adımda tracemem() yaptıktan sonra her değişiklikte terminale print ediliyor. untracemem() kapatıyor
        
        x <- c(1, 2, 3)
        cat(tracemem(x), "\n")
        #> <0x7f80c0e0ffc8>
        
        y <- x
        y[[3]] <- 4L
        #> tracemem[0x7f80c0e0ffc8 -> 0x7f80c4427f40]:
        
        2.3.2 Function calls
        
        Kabaca, fonksiyonlar kopyalama gibi çalışıyor. 
        
        f <- function(a) {
        a
        }
        
        x <- c(1, 2, 3)
        cat(tracemem(x), "\n")
        #> <0x7fe1121693a8>
        
        z <- f(x)
        
        #Does not create a new copy of object a
        
        untracemem(x)
        
        Eğer fonksiyon objede oynama yapsaydı x’in refere ettiği obje bir önceki örnekteki y gibi kopyalanacaktı
        
        2.3.3 Lists
        
        Listeler, içerisindeki elementlerin referanslarını taşıyor. Copy yaparsan, sadece modify edilen referans değişiyor.
        
        ![Untitled](R%20Coding%20b9798d0f9b8546efbeb0e1e379a4322d/Untitled%204.png)
        
        2.3.4 Data Frames:
        
        Kolon değiştirirsen sadece o kolonu kopyalıyor. Satır değiştirirsen tüm sütunlar değiştiği için hepsini kopyalıyor
        
        2.3.5 Character vectors:
        
        Karakter vektörleri bir global string pool’u refere ediyormuş.
        
        ![Untitled](R%20Coding%20b9798d0f9b8546efbeb0e1e379a4322d/Untitled%205.png)
        
        2.3.6 Exercises:
        
        1. Why is tracemem(1:10) not useful: Because the adress of the original object will always stay the same. If any modifications are made, they will be made on a copy.
        
        ![Untitled](R%20Coding%20b9798d0f9b8546efbeb0e1e379a4322d/Untitled%206.png)
        
        Likely because these are strings within the x variable. These are likely refering to the character vectors? Not 100% sure
        
        1. If we were to call the first 1:10 object x, a refers to x. b is a list that refers to x twice. c is a list that refers to x twice, and a new object identical to x
        2. It will create a new list. The first element of this new list will refer to the object refered in the original x list. The second element will be a new object that is identical to the original object.
        
        When x is assigned to an element of itself, the list referenced by x is copied to a new adress in the memory.
        
        The original list object bound to x is now referenced in the newly created list object, and is no longer bound to a name.
        
    - 2.4 Object size
        
        Because R works with references, object sizes tends to be much smaller than one might expect. 
        
        Repeating a string twice will not double the object size - they’ll refer to the same string in the global string pool
        
        When you represent a range with x:y, only the first and last values will be stored.
        
        These are all pulled with lobstr:obj_size()
        
        2.4.1 Exercise- 
        
        1. Because object.size() provides an estimate while obj_size gives an exact value that is calculated depending on the current situation.  Object.size() does not account for shared references.
        2. We are only seeing the size of list elements. However its super high even for that?,
        3. It will all depend on the number of elements in each list. 
            1. A is a list of 1 million doubles. An empty list with 1 null element has a size of 48 b. 1 million doubles is 1 mb= 8,000,048 B
            2. b will refer to the same object within a twice. It will be size of a + size of a list with two empty elements (which is 112)
            3. Because there are shared elements, obj size will not change for obj_size(a,b)
            4. Changing the first element of b modifies it to (10,a) and initiates copy protection. Instead of referencing to a, b[1][1] is now referencing to a new object containing 1 million of the same doubles. So double the size
                1. ask about this one
            5. b[2] is still referencing to a, so in obj_size(a,b), b has a in it, and size won’t change.
            6. Chancing the second element of b will modify the adress again - now, we have a third object of 1 million doubles. Obj_size(b) is still 16 mb, because its now refering to the second and third million objects
            7. Obj(a,b) changes to 24 mb, because it refers to three distinct 1 mils.
            
            2.5 Modify-in-place
            
            If an object is only bound to a single name, modifying the object will not create a copy. It will simply modify it in the same memory adress.
            
            However, most R functions will create a reference for objects when they work with them. This is a problem, because when an object is referenced for more than once in R, there is no way for R to revert back to modify-in-place behavior, even if the previous reference is deleted. Due to this, it’s best to do tracemem() and test manually for optimizations.
            
            Loops in R with dataframesare notoriously slow because of this, as they create copies of themselves in each iteration. On the other hand, lists use old C code, and only a single copy is enough
            
            2.5.2 Environments
            
            Environments are always modified-in-place
            
            2.5.3 Exercises:
            
            1. When we modify x by trying to add it to itself, a copy of x is created as part of copy-on-modify. Because this copy is a distinctly object separate from the original list, the code does not create a circular list.
            2. No idea - apparently lists are faster because they are not copied over and over again. But converting a db to list takes processing power too, so the benefit only appears after 300+ columns in a db.
            3. Environments are always modified in place, so tracemem does nothing.
    
    2.6 Unbinding and the garbage collector
