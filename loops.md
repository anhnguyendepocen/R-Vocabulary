# Loops

[R Vocab Topics](index) &#187; [Functions](functions_loops) &#187; Loops


* <a href="#if">`if` statement for conditional programming</a>
* <a href="#ifelse">`if...else` statement for conditional programming</a>
* <a href="#for">`for` loop to iterate over a fixed number of iterations</a>
* <a href="#while">`while` loop to iterate until a logical statement returns FALSE</a>
* <a href="#repeat">`repeat` loop to execute until told to break</a>
* <a href="#break">`break` function to exit a loop</a>
* <a href="#next">`next` function to skip an interation of a loop</a>

<br>

<a name="if"></a>

# `if` Statement
The conditional `if` statement is used to test an expression.  If the `test_expression` is `TRUE`, the `statement` gets executed. But if it's `FALSE`, nothing happens. 


```r
# syntax of if statement
if (test_expression) {
        statement
}
```

The following is an example that tests if any values in a vector are negative.  Notice there are two ways to write this `if` statement; since the body of the statement is only one line you can write it with or without curly braces.  I recommend getting in the habit of using curly braces, that way if you build onto if statements with additional functions in the body or add an `else` statement later you will not run into issues with unexpected code procedures.

```r
x <- c(8, 3, -2, 5)

# without curly braces
if(any(x < 0)) print("x contains negative numbers")
## [1] "x contains negative numbers"

# with curly braces produces same result
if(any(x < 0)){
        print("x contains negative numbers")
}
## [1] "x contains negative numbers"

# an if statement in which the test expression is FALSE
# does not produce any output
y <- c(8, 3, 2, 5)

if(any(y < 0)){
        print("y contains negative numbers")
}
```
<small><a href="#">Go to top</a></small>

<br>

<a name="ifelse"></a>

# `if...else` Statement
The conditional `if...else` statement is used to test an expression similar to the `if` statement.  However, rather than nothing happening if the `test_expression` is `FALSE`, the `else` part of the function will be evaluated. 


```r
# syntax of if...else statement
if (test_expression) {
        statement 1
} else {
        statement 2
}
```

The following extends the [previous example](#if) in which the `if` statement tests if any values in a vector are negative; if `TRUE` it produces one output and if `FALSE` it produces the `else` output.  


```r
# this test results in statement 1 being executed
x <- c(8, 3, -2, 5)

if(any(x < 0)){
        print("x contains negative numbers")
} else{
        print("x contains all positive numbers")
}
## [1] "x contains negative numbers"

# this test results in statement 2 (or the else statement) being executed
y <- c(8, 3, 2, 5)

if(any(y < 0)){
        print("y contains negative numbers")
} else{
        print("y contains all positive numbers")
}
## [1] "y contains all positive numbers"
```

Simple `if...else` statements, as above, in which only one line of code is being executed in the statements can be written in a simplified alternative manner.  These alternatives are only recommended for very short `if...else` code:

```r
x <- c(8, 3, 2, 5)

# alternative 1
if(any(x < 0)) print("x contains negative numbers") else print("x contains all positive numbers")
## [1] "x contains all positive numbers"

# alternative 2 using the ifelse function
ifelse(any(x < 0), "x contains negative numbers", "x contains all positive numbers")
## [1] "x contains all positive numbers"
```

We can also nest as many `if...else` statements as required (or desired).  For example:

```r
# this test results in statement 1 being executed
x <- 7

if(x >= 10){
        print("x exceeds acceptable tolerance levels")
} else if(x >= 0 & x < 10){
        print("x is within acceptable tolerance levels")
} else {
         print("x is negative")
}
## [1] "x is within acceptable tolerance levels"
```
<small><a href="#">Go to top</a></small>

<br>

<a name="for"></a>

# `for` Loop
The `for` loop is used to execute repetitive code statements for a particular number of times.  The general syntax is provided below where `i` is the counter and as `i` assumes each sequential value defined (1 through 100 in this example) the code in the body will be performed for that i<sup>th</sup> value.


```r
# syntax of for loop
for(i in 1:100) {
        <do stuff here with i>
}
```

An important lesson to learn is that R is not efficient at *growing* data objects.  As a result, it is more efficient to create an empty data object and *fill* it with the `for` loop outputs.  For example, if you want to create a vector in which 5 values are randomly drawn from a poisson distribution with mean 5, it is less efficient to perform the first example in the following code chunk than to perform the second example.  Although this inefficiency is not noticed in this small example, when you perform larger repetitions it will become noticable so you might as well get in the habit of *filling* rather than *growing*. 


```r
# not advised
for(i in 5){
        x <- rpois(5, lambda = 5)
        print(x)
}
## [1] 3 5 3 6 3

# advised
x <- vector(mode = "numeric", length = 5)

for(i in 5){
        x <- rpois(5, lambda = 5)
        print(x)
}
## [1] 7 3 6 5 2
```

Another example in which we create an empty matrix with 5 rows and 5 columns.  The `for` loop then iterates over each column (note how *i* takes on the values 1 through the number of columns in the `my.mat` matrix) and takes a random draw of 5 values from a poisson distribution with mean *i* in column *i*:

```r
my.mat <- matrix(NA, nrow = 5, ncol = 5)

for(i in 1:ncol(my.mat)){
        my.mat[, i] <- rpois(5, lambda = i)
}
my.mat
##      [,1] [,2] [,3] [,4] [,5]
## [1,]    0    2    7    5   13
## [2,]    0    1    3    6    6
## [3,]    1    2    2    3    4
## [4,]    1    3    2    3    8
## [5,]    1    3    3    1    6
```
<small><a href="#">Go to top</a></small>

<br>

<a name="while"></a>

# `while` Loop
While loops begin by testing a condition. If it is true, then they execute the statement. Once the statement is executed, the condition is tested again, and so forth, until the condition is false, after which the loop exits.  It's considered a best practice to include a counter object to keep track of total iterations


```r
# syntax of while loop
counter <- 1

while(test_expression) {
        statement
        counter <- counter + 1
}
```

`while` loops can potentially result in infinite loops if not written properly; therefore, you must use them with care.  To provide a simple example to illustrate how similiar `for` and `while` loops are: 


```r
counter <- 1

while(counter <= 10) {
        print(counter)
        counter <- counter + 1
}

# this for loop provides the same output
counter <- vector(mode = "numeric", length = 10)

for(i in 1:length(counter)) {
        print(i)
}
```

The primary difference between a `for` loop and a `while` loop is:  a `for` loop is used when the number of iterations a code should be run is known where a `while` loop is used when the number of iterations is not known.  For instance, the following takes value `x` and adds or subtracts 1 from the value randomly until `x` exceeds the values in the test expression.  The output illustrates that the code runs 14 times until x exceeded the threshold with the value 9.


```r
counter <- 1
x <- 5
set.seed(3)

while(x >= 3 && x <= 8 ) {
        coin <- rbinom(1, 1, 0.5)
        
        if(coin == 1) { ## random walk
                x <- x + 1
        } else {
                x <- x - 1
        }
        cat("On iteration", counter, ", x =", x, '\n')
        counter <- counter + 1
}
## On iteration 1 , x = 4 
## On iteration 2 , x = 5 
## On iteration 3 , x = 4 
## On iteration 4 , x = 3 
## On iteration 5 , x = 4 
## On iteration 6 , x = 5 
## On iteration 7 , x = 4 
## On iteration 8 , x = 3 
## On iteration 9 , x = 4 
## On iteration 10 , x = 5 
## On iteration 11 , x = 6 
## On iteration 12 , x = 7 
## On iteration 13 , x = 8 
## On iteration 14 , x = 9
```
<small><a href="#">Go to top</a></small>

<br>

<a name="repeat"></a>

# `repeat` Loop
A `repeat` loop is used to iterate over a block of code multiple number of times. There is test expression in a repeat loop to end or exit the loop. Rather, we must put a condition statement explicitly inside the body of the loop and use the `break` function to exit the loop. Failing to do so will result into an infinite loop.


```r
# syntax of repeat loop
counter <- 1

repeat {
        statement
        
        if(test_expression){
                break
        }
        counter <- counter + 1
}
```

For example ,say we want to randomly draw values from a uniform distribution between 1 and 25.  Furthermore, we want to continue to draw values randomly until our sample contains at least each integer value between 1 and 25; however, we do not care if we've drawn a particular value multiple times.  The following code repeats the random draws of values between 1 and 25 (in which we round).  We then include an `if` statement to check if all values between 1 and 25 are present in our sample.  If so, we use the [`break`](#break) statement to exit the loop.  If not, we add to our counter and let the loop repeat until the conditional `if` statement is found to be true.  We can then check the `counter` object to assess how many iterations were required to reach our conditional requirement.  

```r
counter <- 1
x <- NULL

repeat {
        x <- c(x, round(runif(1, min = 1, max = 25)))
        
        if(all(1:25 %in% x)){
                break
        }
                
        counter <- counter + 1
}

counter
## [1] 75
```
<small><a href="#">Go to top</a></small>

<br>

<a name="break"></a>

# `break` Function to Exit a Loop
The `break` function is used to exit a loop immediately, regardless of what iteration the loop may be on.  `break` functions are typically embedded in an `if` statement in which a condition is assessed, if TRUE `break` out of the loop, if FALSE continue on with the loop.  In a nested looping situation, where there is a loop inside another loop, this statement exits from the innermost loop that is being evaluated.


```r
x <- 1:5

for (i in x) {
        if (i == 3){
                break
                }
        print(i)
}
## [1] 1
## [1] 2
```
<small><a href="#">Go to top</a></small>

<br>

<a name="next"></a>

# `next` Function to Skip an Iteration in a Loop
The `next` statement is useful when we want to skip the current iteration of a loop without terminating it. On encountering next, the R parser skips further evaluation and starts next iteration of the loop.

```r
x <- 1:5

for (i in x) {
        if (i == 3){
                next
                }
        print(i)
}
## [1] 1
## [1] 2
## [1] 4
## [1] 5
```
<small><a href="#">Go to top</a></small>

