

# R Programming Fundamentals

In this section, we outline the basic concepts of the R language (data classes, data structures, operators and functions), and demonstrate how to perform some common tasks.

## Classes of Data
The data you work with in R belong to one of several possible classes. The distinction between classes of data is crucial, because the class you instantiate your data with determines the set of possible operations you can perform on that data.

R has 6 "atomic" classes of data, but you will only encounter 4 of these in Psych 240 and 241. These classes are: Numeric, Integer, Logical, and Character. They are dubbed "atomic" classes because they are the fundamental units of the language (i.e., you cannot access a more basic type of object than these 6 classes) and they provide the building blocks for more complex classes and data structures. The 4 frequently used classes are described in more detail below.

### The Numeric Class
The numeric class is used to represent real numbers (i.e., numbers with a fractional component), for example, 6.23. The numeric class is also known as the "double" class, because internally it utilizes [double floating point precision](https://en.wikipedia.org/wiki/Double-precision_floating-point_format). The numeric class is the default class used to represent numbers in R. So, if you were to execute the command `2 + 2` in the R console, R would represent those 2's using the numeric class.

#### Special Values {-#special-values}
R has several special numeric values. `Inf` and `-Inf` are used to represent positive and negative Infinity, respectively. `NaN` is used when the results of a numeric computation produce[ "Not A Number".](https://en.wikipedia.org/wiki/NaN), such as when multiplying a number produces a value too small for the computer to represent. `NaN` is often abused to represent missing values, but the special value [NA](#missing) should be preferred.

### The Integer Class
Just as in mathematics, the integer class can only represent whole numbers, e.g. 6. However, if you type `6` into the R console, R will not use the integer class to represent the 6. As mentioned just above, R will use the numeric data type to represent the 6. If you want to *force* R to use the integer class, you must terminate the number with an upper-case L, e.g. `2L`. In practice, it usually makes little difference whether you use the integer class or the numeric class to represent whole numbers, so sticking with the default class of numeric is safe to do.

### The Logical Class {#logicals}
Logical data can only take on 2 possible values: TRUE (1) or FALSE (0). This type of datum is used to represent whether some state exists (is true) or does not exist (is false). TRUE and FALSE **must be uppercase**. They *can* can be abbreviated as T and F, but it is not recommended (The names `T` and `F` can be over-written with non-logical values, But `TRUE` and `FALSE` are reserved names that can never be overwritten. Such a restrictive class of data may not seem useful, but play an important role in data manipulation, as they can be used to indicate the presence (or absence) of other data values you are interested in.

You can change a `TRUE` to a `FALSE` (or a `FALSE` to a `TRUE`) by prefixing them with the `!` negation operator.


```r
TRUE
## [1] TRUE
!TRUE
## [1] FALSE
!FALSE
## [1] TRUE
```

The `!` operator isn't useful when you are typing out literal `TRUE` and `FALSES`, but is useful in situations where you have a function which produces one type of logical, and but you're interested in the opposite. We will look at such a case later in the [logical indexing](#indexing) section.

#### Special Values {-#missing}
R uses the special value `NA` to denote when a value is missing. `NA` is *technically* a logical value, but can used alongside other atomic classes of data without issue.

### The Character Class
The character data type (sometimes referred to as the "string" data type) is used for representing textual data.
To encode a string of text as character data in R, it must be wrapped in quotes (`" "` or `' '` are both acceptable). 

```r
a <- "foobar"
a
## [1] "foobar"
```

Without the quotes, R will interpret the text as the name of an R variable, and attempt to locate that variable. As demonstrated below, missing quotes is a common source of "object not found" errors.


```r
a <- foobar 
```

```{.Rerrormsg}
## Error in eval(expr, envir, enclos): object 'foobar' not found
```

Sometimes, a value can be represented using *either* a Character or Numeric class. For example, 4.2 could be represented as a numeric by typing `4.2` or a character by typing `"4.2"`. While these look similar, and print similarly, they behave *very* differently. Consider the following two operations.


```r
4.2 + 1
## [1] 5.2
"4.2" + 1
```

```{.Rerrormsg}
## Error in "4.2" + 1: non-numeric argument to binary operator
```

This illustrates the earlier point that different classes of data support different operations. Here, we can see that you cannot perform mathematical operations on Character data, even if the data **could** be interpreted numerically.

### Factors
Factors are not an atomic data class, but it is nearly impossible to use the R language and avoid encountering data with the Factor class. Factors are a hybrid class of data, in which values are printed out with character labels, but are represented internally with integers. Importantly, when a Factor variable is created, the range of possible new values it can take on is restricted - only values that belong to the set of initial values can be inserted or appended. Factors exists in order to have a data class that can represent categorical variables in statistical models such as ANOVA.

The reason Factors are unavoidable is because many functions in R *automatically* convert Character data into Factors. Two notorious culprits are the `data.frame` function, and the `read.csv` function (both of which we will encounter later). New programmers often use these functions, unaware of the implicit conversations they carry out behind the scenes, and are surprised later on when they find that cannot perform some operation (like appending new Character values to ones they already have). Luckily, the conversion-happy behavior of these functions can be stopped by changing one of R's global options, `stringsAsFactors`. To turn off this conversion, execute the command `options(stringsAsFactors = FALSE)` in your R Script or console. We **STRONGLY** recommend disabling this conversion.

### Class detection and conversion
Speaking of conversion, programmers commonly face the need to convert data of one class into data of another class. To determine which class of data you have, you can use the `class` function. For each atomic class, R provide a class detection function (starting with the prefix `is.`) which provides a `TRUE` or `FALSE` response, and a class conversion function (starting with the prefix `as.`). Consider the following examples:


```r
x <- 1
class(x) 
## [1] "numeric"
is.numeric(x)
## [1] TRUE
is.integer(x)
## [1] FALSE
x <- as.integer(x)
class(x)
## [1] "integer"
is.integer(x)
## [1] TRUE
x <- as.logical(x) # 1 converts to TRUE
x
## [1] TRUE
x <- as.character(x) # TRUE converts to "TRUE"
x
## [1] "TRUE"
is.character(x) 
## [1] TRUE
```

Class conversion is not always possible (e.g., `"Z"` cannot be converted into a number) or without loss of information (e.g., a numeric like `6.23` cannot be converted to an integer without losing the `.23`.).

## Data Structures
In order to introduce variables and classes, we have restricted our examples to using single, scalar values, like `6` and `"foobar"`. Of course, when analyzing data, it is necessary to group multiple values together. R provides several different types of **data structures** for this purpose, Think of data structures in R as big containers used for grouping together many values. After storing your data in these containers, you can reuse it multiple places (e.g. create an R object to store it) or access different subsets of it by position or name.

Each type of data structure focuses on representing different classes of data, and different relationships between the values in the data structure. In Psych 240 and 241, you will encountered 4 types of data structures: Vectors, Matrices, Data Frames, and Lists.

### Vectors
Vectors are 1 dimensional data structures which can hold values of a single atomic class. Classes of data *cannot* be mixed within a vector. In other words, a vector can hold either Integer, Numeric, Character, or Logical values, but not any combination of those values.


Vectors, no matter what type of data they hold, can be created by using the `c()` function in R, short for concatenate. Just place each value you want to be included in the vector inside the parenthesis, separated with a comma.

The individual values held in the vector are referred to as elements, and vectors have a length equal to the number of elements it contains. You can check how many elements there are in a vector using the `length()` function



```r
new_vector <- c(1, 10, 45, -1)
length(new_vector)
## [1] 4
char_vector <- c("foo", "bar", "herp", "derp")
length(char_vector)
## [1] 4
```

`c()` can also combine existing vectors into a single, larger vector

```r
big_vec <- c(new_vector, c(1, 2, 3, 4, 5))
big_vec
## [1]  1 10 45 -1  1  2  3  4  5
```

If you need to create a vector that contains a long one-by-one numeric sequence, there is a shortcut for typing out all the values you need - the `:` operator. Put the first number in the sequence before the colon and the final number in the sequence after the colon:


```r
5:50
##  [1]  5  6  7  8  9 10 11 12 13 14 15 16 17 18 19 20 21 22 23 24 25 26 27
## [24] 28 29 30 31 32 33 34 35 36 37 38 39 40 41 42 43 44 45 46 47 48 49 50
```

After you create a vector, you can give each elements a name, using the `names()` function and a character vector.


```r
names(new_vector) <- c("A", "B", "C", "D")
new_vector
##  A  B  C  D 
##  1 10 45 -1
```

Names can be useful when you want to select a subset of the values in a vector, which will come up in Section \@ref(subsetting).

### Matrices
Matrices in R are analogous to matrices from linear algebra, with the notable difference of being able to hold non-numeric types of data. They are a rectangular 2-D data structure, meaning that the number of elements in the matrix is be equal to the product of the number of rows and the number of columns.

- rows = dimension 1
- columns = dimension 2

Like vectors, data types may *not* be mixed in a matrix (e.g. you cannot have some elements be characters and other be numeric, etc.)

A matrix is created by feeding a vector into the `matrix()` function, and specifying either the number of rows *or* number of columns, *or* both.


```r
wide <- matrix(c(1:3, 99:101), ncol = 3)
wide
##      [,1] [,2] [,3]
## [1,]    1    3  100
## [2,]    2   99  101
long <- matrix(c(1:3, 99:101), nrow = 3)
long
##      [,1] [,2]
## [1,]    1   99
## [2,]    2  100
## [3,]    3  101
```

If you want a large matrix but with only a few unique values, take advantage of R's ability to **recycle** input.

```r
matrix(c(4), nrow = 3, ncol = 10)
##      [,1] [,2] [,3] [,4] [,5] [,6] [,7] [,8] [,9] [,10]
## [1,]    4    4    4    4    4    4    4    4    4     4
## [2,]    4    4    4    4    4    4    4    4    4     4
## [3,]    4    4    4    4    4    4    4    4    4     4
```

Note the matrix is filled up by column (i.e. first element goes to row 1 column 1, second element goes to row 2, column 1, etc.)

```r
matrix(c(4,0), nrow = 2, ncol = 10)
##      [,1] [,2] [,3] [,4] [,5] [,6] [,7] [,8] [,9] [,10]
## [1,]    4    4    4    4    4    4    4    4    4     4
## [2,]    0    0    0    0    0    0    0    0    0     0
```

### Data Frames
Like matrices, Data Frames are are 2D, rectangular data structures, but are more flexible because they allow for different data types to be stored in each column. A Data Frame is usually the best way to store and work with a dataset that mixes qualitative and quantitative variables. 

Data frames can be created by passing `name = value` pairs to the `data.frame()` function. The values should be vectors (of any type) and the names should be unquoted strings of text, which will be used to label each column. Importantly, all the vectors stored in the data frame must be of *identical length*.

Essentially, a data frame is a container that imposes a relational structure on a set of vectors. 

```r
df <- data.frame(x = c(1,4,4,2),
                 y = c(3,3,1,4),
                 month = c("Sep","Oct","Nov","Jan"),
                 stringsAsFactors = FALSE)
df
##   x y month
## 1 1 3   Sep
## 2 4 3   Oct
## 3 4 1   Nov
## 4 2 4   Jan
```

This example also shows an extra argument, `stringsAsFactors = FALSE`, that was **not** a variable to store in the data frame. This argument is another way to controls how R interprets character vectors (a.k.a. strings) when forming the data frame. Here, using `stringsAsFactors = TRUE` forces R to leave your character vectors as they are when creating the data frame.

### Lists
Lists are the most abstract and flexible data structure in R. Lists can hold any type of R object, but doesn't impose any relationship between them. You can have a list holding matrices, data frames, vectors, and even lists holding other lists!

Think of lists like a folder on your hard drive. You can stuff any kind of file you like in there and give it a name, but there is no relationship between them inside that folder, other than the order they are sorted in. Use a list when you need to group data structures of different sizes and types together. But carefully consider if there is another way, because the lack of structured relationships between the data in different list elements can make them tricky to work with

As you might have guessed, you can create lists of your own with the `list()` function. Like the `data.frame()` function, you pass in `name=value` pairs. But now, the values can be any R object, of any size, not just vectors with the same lengths.


```r
biglist <- list(first = -10:-15, second = data.frame(x=c("A","B"), y = 1:2))
biglist
## $first
## [1] -10 -11 -12 -13 -14 -15
## 
## $second
##   x y
## 1 A 1
## 2 B 2
```

## Operators
Creating data structures is great, but creating them is rarely ever the goal we have in mind - we always want to manipulate and perform computations using our data. Nearly every analysis you perform will make use of R's inline **operators**. We've seen these operators already when we did [arithmetic in the R console](#console). As a reminder, R has 5 basic arithmetic operators:

- `+` for addition
- `-` for subtraction
- `*` for multiplication
- `/` for division
- `^` for exponentiation

R also has two other types of operators: **Relational** operators and **Logical** operators. 

### Relational Operators
As the name implies, relational operators are used to examining the relationship between values. R has 6 relational operators:

- `<`: The "less than" operator
- `<=`: The "less than or equal to" operator
- `>`: The "greater than" operator
- `>=`: The "greater than or equal to" operator
- `==` The "equal to" operator
- `!=` The "not equal to" operator

All 6 of these operators can be used sensibly on Integer and Numeric data, while only the last two can be used sensibly on Logical and Character data.

Each of these operators returns it's answer in the form of a Logical value. Consider the following examples that demonstrate each operator:


```r
1 < 2
## [1] TRUE
1 > 2
## [1] FALSE
2 > 2
## [1] FALSE
2 >= 2
## [1] TRUE
2 == 2
## [1] TRUE
2 != 2
## [1] FALSE
1 != 2
## [1] TRUE

"hi" == "hello"
## [1] FALSE
"bye" == "bye"
## [1] TRUE
"bye" != "bye"
## [1] FALSE
```

These relational operators can be applied to matrices and vectors as well. When applied to matrices and vectors, they operate **element-wise**, meaning they operate on each element of the vector or matrix one at a time, and give you an answer for each individual element. So, if you apply the `==` operator to a vector with 10 values, you will get 10 `TRUE`/`FALSE` answers.


```r
c(10, 5, -10) > 0
## [1]  TRUE  TRUE FALSE
c(10, 5, -10) == 0
## [1] FALSE FALSE FALSE
c("foo", "bar", "herp", "derp") == "bar"
## [1] FALSE  TRUE FALSE FALSE
```

When you have a vector or matrix on *both* sides of the operator, it still operates element-wise. In this situation, it will match up the elements on each data structure by position (first with first, second with second, etc.).


```r
c(10, 5, -10) > c(20, -5, 0)
## [1] FALSE  TRUE FALSE
c(10, 5, -10) == c(20, -5, 0)
## [1] FALSE FALSE FALSE
```

When using relational operators with two vectors/matrices, make sure they both have the same number of elements. If not, R will recycle values from the beginning of the shorter vector in order to "pad" it's length. This is probably not what you want to happen, but R will **only** warn you if the length of the longer vector is not a multiple of the shorter vector. If it is a multiple, then R will recycle silently. Be vigilant! 


```r
c(10, 5) == c(20, -5, 0, 50) # No warning, 4 is a multiple of 2 !!!
## [1] FALSE FALSE FALSE FALSE
```


```r
c(10, 5) == c(20, -5, 0) # Recycling warning, 3 is not a multiple of 2
```

```{.Rerrormsg}
## Warning in c(10, 5) == c(20, -5, 0): longer object length is not a multiple
## of shorter object length
## [1] FALSE FALSE FALSE
```

### Logical Operators
R's logical operators are used for combining together multiple Logical values into a single Logical value. The 5 main logical operators are:

- `!`: The "negation" operator (mentioned in the [Logical Data](#logicals) section)
- `&`: The "element-wise and" operator
- `&&`: The "scalar and" operator
- `||`: The "element-wise and" operator
- `|`: The "scalar and" operator

We will not cover the use of these operators in the guide, as they go beyond the programming scope expected of Psych 240 and 241 students. Interested instructors are advised to look at examples here: 

- [Manipulating Data](https://wjhopper.github.io/psych640-labs/labs/ManipulatingData.html)
- [Logical Operators in R](https://www.youtube.com/watch?v=6PpQS-YLWDQ)

## Functions
We have used functions in several previous examples, without providing explanation of what an R function is, or how they are used generally. But understanding the basics of functions in R is important, because all of R's analysis and modeling tools are accessed by using function. In this section, we will describe what a function is generally, and explain how to learn what a function  does by reading documentation.

So, what is a function? A function is fixed piece of code that accepts input values, performs some operations or calculations on these values, and returns some results. The purpose of having functions in a programming language is to allow you to repeat an operation *without* have to repeat all of the code that defines the operation - the code is "bundled" into a function, and can be re-used infinitely without copying and pasting the code each time. A good way to start thinking about functions by analogy to equations. For instance, if we have the equation $y=\sqrt{x}$:

- $x$ is the input
- $\sqrt{}$ is the operation performed on the input
- $y$ is the output, in this case the result of taking the square root of $x$

When you use a function, you are performing a 'function call' (in computer science-ish terms). 
This leads to colloquialisms like "Call `mean` on that matrix" or "This code calls `diag` to extract the diagonal elements". 'Call' does not mean anything special to us. All using the word 'call' means in this context is 'use a function'.

The inputs to functions go by several names, but most often they are called "**arguments**" or "**parameters**". Calling a function with some specific input is often called "passing an argument". Don't confuse function parameters with population parameters from the statistics side of class

Perhaps the best way to understand the properties of R functions, and how they can be used, is to look the documentation of an R function. Here, we'll examine the documentation for a function we've used previously, the `matrix` function. We'll access it by executing the command `?matrix` in the console.


```r
?matrix
```

We'll focus on the Description, Usage, and Arguments section, shown below:
<div class="r-help-page">

<table width="100%" summary="page for matrix"><tr><td>matrix</td><td style="text-align: right;">R Documentation</td></tr></table>

<h2>Matrices</h2>

<h3>Description</h3>

<p><code>matrix</code> creates a matrix from the given set of values.
</p>
<p><code>as.matrix</code> attempts to turn its argument into a matrix.
</p>
<p><code>is.matrix</code> tests if its argument is a (strict) matrix.
</p>


<h3>Usage</h3>

<pre class="r">
matrix(data = NA, nrow = 1, ncol = 1, byrow = FALSE,
       dimnames = NULL)

as.matrix(x, ...)
## S3 method for class 'data.frame'
as.matrix(x, rownames.force = NA, ...)

is.matrix(x)
</pre>


<h3>Arguments</h3>

<table summary="R argblock">
<tr valign="top"><td><code>data</code></td>
<td>
<p>an optional data vector (including a list or
<code>expression</code> vector).  Non-atomic classed <span style="font-family: Courier New, Courier; color: #666666;"><b>R</b></span> objects are
coerced by <code>as.vector</code> and all attributes discarded.</p>
</td></tr>
<tr valign="top"><td><code>nrow</code></td>
<td>
<p>the desired number of rows.</p>
</td></tr>
<tr valign="top"><td><code>ncol</code></td>
<td>
<p>the desired number of columns.</p>
</td></tr>
<tr valign="top"><td><code>byrow</code></td>
<td>
<p>logical. If <code>FALSE</code> (the default) the matrix is
filled by columns, otherwise the matrix is filled by rows.</p>
</td></tr>
<tr valign="top"><td><code>dimnames</code></td>
<td>
<p>A <code>dimnames</code> attribute for the matrix:
<code>NULL</code> or a <code>list</code> of length 2 giving the row and column
names respectively.  An empty list is treated as <code>NULL</code>, and a
list of length one as row names.  The list can be named, and the
list names will be used as names for the dimensions.</p>
</td></tr>
<tr valign="top"><td><code>x</code></td>
<td>
<p>an <span style="font-family: Courier New, Courier; color: #666666;"><b>R</b></span> object.</p>
</td></tr>
<tr valign="top"><td><code>...</code></td>
<td>
<p>additional arguments to be passed to or from methods.</p>
</td></tr>
<tr valign="top"><td><code>rownames.force</code></td>
<td>
<p>logical indicating if the resulting matrix
should have character (rather than <code>NULL</code>)
<code>rownames</code>.  The default, <code>NA</code>, uses <code>NULL</code>
rownames if the data frame has &lsquo;automatic&rsquo; row.names or for a
zero-row data frame.</p>
</td></tr>
</table>


</div>

### Description
As you might expect, the Description section describes what the function is used for, and lists the functions that are documented in this page. Here, the `matrix`, `as.matrix` and `is.matrix` functions are documented.

### Usage
The usage section tells you:

  - The syntax for invoking the function
  - The names of the accepted arguments
  - The order of the arguments
  -  Which arguments are **required** and which are **optional**
      + Arguments with an `=` are optional
      + All others are required

We can see that the `matrix ` function has 5 arguments, `data`, `nrow`, `ncol`, `byrow` and `dimnames`, each with a default argument. Because all the arguments have defaults, we know that we can call the `matrix` function with no arguments and still get a result!


Lines saying "S3 Method for class ..." tell you about the functions' behavior when called on objects of a specific class. For example, this help page tells us that when the `as.matrix` function is called on a data.frame, there is an optional argument called `rownames.force` that isn't used when the input is some other data structure (like a vector). We can safely ignore the cryptic term "S3 Method". 

But, we will focus on other cryptic parts of the "Usage" section. But the usage section is also cryptic! What is `x`? What is `trim`? What do they do? For clarification, we must go the arguments sections.

### Arguments
The detailed descriptions in the arguments section tell us what types of values each argument is permitted to take on. It also tells us what aspect of the function's behavior each argument controls. For example, the `byrow` argument must be a logical value (i.e., `TRUE` or `FALSE`) and it controls whether the matrix is row-by-row, or column-by-column.

### Named vs. Unnamed Arguments
As we see in the "Usage" and "Arguments" sections, every function argument has a name (e.g., `x`, `na.rm`, `trim`). When you call a function, those names can be used in a `keyname = value` style of syntax, or they may be omitted in favor of just specifying the value. 

If you wish to omit the names of the arguments when calling a function **you must order your inputs in the exact same order as they appear in the `Usage` section!!!** If you specify arguments as `keyname=value` pairs, they may be passed in any order. If you mix and match named and unnamed, unnamed inputs that R encounters will be paired up with the unmatched arguments following their order in the Usage section.


```r
x <- c(4,10,3,33,2,NA,43,22,31,95)
matrix(data=x, byrow=TRUE, nrow=2, ncol=5) # named key/value style
##      [,1] [,2] [,3] [,4] [,5]
## [1,]    4   10    3   33    2
## [2,]   NA   43   22   31   95
matrix(x, 2, 5, TRUE)  # unnamed style, byrow goes last
##      [,1] [,2] [,3] [,4] [,5]
## [1,]    4   10    3   33    2
## [2,]   NA   43   22   31   95
```


```r
matrix(x, TRUE, 2, 5) #5 gets matched up with byrow, not ncol!
##      [,1] [,2]
## [1,]    4   10
```

### A word of advice
So, which style should you prefer when writing code in an R script: named or unnamed arguments? We recommend using named arguments for all arguments past the first. This strikes a balance between verbosity and clarity - it is often easy to remember what the first argument to a function does and what kind of values it should take on, but it is often difficult to remember the role and order of arguments beyond that. 

For example, you have just read the documentation for the `matrix` function - can you remember whether the `nrow` or `ncol` argument goes first? Are you confident enough to just write some code without looking it up. And are you confident that you'll still remember whether you're making a 10 by 50 or a 50 by 10 matrix when you re-read your code tomorrow?

We would venture to guess the answer to these questions is "No", which makes a strong case for naming your arguments when you write your code. Trust us, if you ever venture into a programming language without support for named arguments (I'm looking at you, MATLAB), you'll yearn for named arguments. 

In summary, name your arguments.

#### Special Arguments {-}
You may have noticed that in the "Usage" section, the `as.matrix` and the `is.matrix` functions have an argument called  `...`. In fact, many R functions have such an argument. A full discussion of the `...` construct is beyond the scope of this guide (or the ellipsis, if you're trying to Google it) is beyond the scope of this guide. For our purposes, we can understand it as a special "catch all" device for any parameters inputs that aren't otherwise explicitly declared. The `...` is used to enable argument passing between functions: it allows one function to capture arguments intended for another function, and send them directly to the other function, without ever know what the names of the arguments for the other function. Neat!

#### Examples {-}
The last section of the `matrix` help page we will look at  is "Examples" sections
<div class="r-help-page">

<table width="100%" summary="page for matrix"><tr><td>matrix</td><td style="text-align: right;">R Documentation</td></tr></table>

<h2>Matrices</h2>

<h3>Examples</h3>

<pre class="r">
is.matrix(as.matrix(1:10))
!is.matrix(warpbreaks)  # data.frame, NOT matrix!
warpbreaks[1:10,]
as.matrix(warpbreaks[1:10,])  # using as.matrix.data.frame(.) method

## Example of setting row and column names
mdat &lt;- matrix(c(1,2,3, 11,12,13), nrow = 2, ncol = 3, byrow = TRUE,
               dimnames = list(c("row1", "row2"),
                               c("C.1", "C.2", "C.3")))
mdat
</pre>


</div>

The examples sections demonstrate a simple application of the function. When using a function for the first time, or you find yourself confused by a part of the documentation, running and tweaking the examples you find here is a great way to get a concrete understanding of how the function behaves.

#### The Return Value {-}
The `matrix` function lacks one field in the help file that most R function have - the "Value" field. This section describes what the function outputs, i.e., what it "returns" to the caller. The `matrix` function can get away with omitting this section, because it's return value is fairly obvious - a matrix! But functions with more complicated outputs need to describe what they return in more detail, so the user can understand how to process the output in their own code.

## Data Manipulation
Data manipulations describes the processes of editing and re-organizing a set of observations in order to facilitate a subsequent analysis. In this guide, we will cover three main data manipulation processes: indexing, subsetting, and replacement. Since data manipulations typically begins by importing data from a file on your hard drive into R, we will begin this section describing how to import or "read in" 

### Tidy Data
It's worth beginning with an outline of a well-formatted data set. 

- The data is represented in a rectangular structure (table with rows and columns)
- Each column represents a specific variable, with a header signifying the name of this variable
- Each row is represents an observation
- Avoids names or values with blank spaces
- Avoids using names that contain symbols such as :, ;, ?, $, %, ^, &, *, (, ), -, #, ?, < , >, /, |, [, ], { and } 
 - Any missing values in your data set are indicated with `NA`
 
Adhering to these principles when you save new data, or manipulate data you have, will greatly simplify analysis performed in R.

### Importing Data

#### CSV files {-}
A CSV file is a type of plain-text document, and is indicated by the .csv file extension. Plain text files consist only of sequences of characters codes, including spaces, tabs, new lines and delimiters. They have no styling associated with them (e.g. no italics or bolding, no images). Files with extensions such as .txt, .R  and .html are plain-text files, while files such as .doc, .docx (Word documents) and .xlsx (Excel documents) are **not** plain text files. We recommend you use plain-text formats for sharing data, because they have the greatest deal of interoperability between computer operating systems and analysis programs.

In a CSV file, the content is arranged in a tabular format. Each new line in the file represents a row, and distinct values within each row are separated by commas to form the different columns. Below is an example of what a CSV file looks like before it is imported into R:


```r
commas <- read.csv("data/commas.csv")
write.csv(commas, file="", quote=FALSE, row.names=FALSE)
## condition,trial,rating
## a,1,3
## b,2,1
## c,3,11
```

We can see that the file has 3 columns, a header row, and 3 observation rows. Before we can import this file into R, we must know how to instruct R where to find the file on our computer, and . To do this, we must understand about file paths on our hard drive, and how R looks for files.

#### File Paths and Working Directories {-}
All the files stored on your computer's hard drive are associated with a named location in the file system's hierarchy. For example, Windows users are likely familiar storing files inside the "My Documents" folder (also known as a "directory").

Much like a file, the R session you have open is also associated with a directory on your hard drive. But, unlike a file, your R session can easily change it's current location without copying the session. The directory your R session currently inhabits is called the "Current Working Directory". You can see what this directory is by issuing the command `getwd()`.


```r
getwd()
## [1] "/Users/andrea/Documents/GitHub/PBS-R-Manual"
```

As you can see, the current working directory of my R session is the PBS-R-Manual folder. I can change this location using the `setwd()` command, and providing the name of another directory to move the R session to. The new directory I move to needs to be specified as a Character value (i.e., surrounded with quotation marks). However, I have to be very clear and explicit when describing the location of this directory. Specifically, I have describe this directories location using either a **relative** or an **absolute** path.

An absolute file path describes the location in relationship to the beginning of the entire file system, while a relative path describes the location in relationship to R's current working directory. This is important, because not every location on your hard drive is visible from R's current directory - R can only see files *below* it's current working directory in the file system hierarchy. 

If you need to access a file that *is not* below your current working directory, the best way to do this is with an absolute file path. On Windows, the start of each file system is given a letter prefix; the prefix of the file system holding the Window's installation is `C:\`. Directories are separated  with **backward** slashes (e.g. `C:\Users\will` is an absolute path). On Mac OSX and Linux, the start of the file system is `/` (read as "root"). Here, directories are separated  with **forward** slashes (e.g., `/Users/will` is an absolute path). But in R, you don't have to worry about forward slashes vs. backward slashes. **You can use forward slashes in your code, and it will work on either Mac or Windows**

If you need to access a file that *is* not below your current working directory, the best way to do this is with a relative file path. A relative file path doesn't need to begin with `C:\` or `"/"`, it can just begin with the name of the file or directory. Let's do this now with the CSV file we saw in the previous section.

#### read.csv {-}
The R function used for importing CSV files is called `read.csv`. It has one required argument, the file path describing the name and location of the CSV file to import. In this case, the CSV file is named `commas.csv` and it is stored in a directory named `data` that is in my current working directory. Let's import it now:


```r
commas <- read.csv("data/commas.csv")
class(commas)
## [1] "data.frame"
commas
##   condition trial rating
## 1         a     1      3
## 2         b     2      1
## 3         c     3     11
```

Note that I assigned the output of the `read.csv` function to variable named `commas`, and that the function imported the CSV file as a `data.frame` object. Also note that the values in the first row of the CSV file were used as names for each columns, rather than a row of data.

#### Importing Excel Files {-}
Excel files are ubiquitous, but because of their history as a proprietary format, R does not have native support for importing them. However, all is not lost: you can install the `readxl` package and use it's `read_excel` function to import .xls and .xlsx files into R as data frames.

### Subsetting
Subsetting describes the processes of "extracting" or "slicing out" a subset of the values from one data structure into another. In R, the processes of subsetting **does not** remove the values you subset from the original data structure. Rather, it creates a copy of the subset you ask for, and puts that copy into your new data structure. So, subsetting is a safe operation that will not result in any data loss.

Subsetting a data structure is performed in R using the `[]` operator, which are called square brackets. All R data structures can be subsetted using the `[]` operators. To subset a data structure, the `[]` operator immediately after the data structure. Between the two square brackets, you place what is known as an **index vector**. An index vector is a vector that describes which values in the data structure you want included in the subset. There are 3 types of index vectors:

- Numeric Index Vectors, which describe the *position* (e.g. first, third, or 19th) of the elements you want included in the subset.
- Character Index Vectors, which describe the *names* of the elements you want included in the subset (only useful when the elements have names).
- Logical Index Vectors, which specify for each element in the data structure whether it should be included, or excluded, from the subset.
 
We'll start with subsetting vectors with a numeric index vectors to get a feel for the general procedure.

#### Subsetting with a Numeric Index {-}


```r
alphabet <- c("a","b","c","d","e","f","g","h","i","j","k","l","m","n","o",
              "p","q","r","s","t","u","v","w","x","y","z") 
alphabet[c(1,26)] # Extract First and 26th element
## [1] "a" "z"
alphabet[10:20] # Extract tenth through 20th
##  [1] "j" "k" "l" "m" "n" "o" "p" "q" "r" "s" "t"
```

One of the most common mistake is including a value in your indexing vector which is greater than the length of the vector you are subsetting

```r
alphabet[100] # there are not 100 letters in the alphabet
## [1] NA
```
The `NA` means the value is missing. This is commonly referred to as an "index out of bounds" error, although R does not explicitly give you an error.

Another common mistake is forgetting to concatenate the values you want to use for the indexing vector (i.e. forgetting the `c()` function).

```r
alphabet[1,5]
```

```{.Rerrormsg}
## Error in alphabet[1, 5]: incorrect number of dimensions
```
This time, R does give us an error, letting us know that we've attempted to index a vector like a matrix. 

##### "Negative" Subsetting {-}
Instead of creating a vector of values you *do* want to pick out, it may be easier to come up with a vector of ones you *don't* want. We can use negative number's to specify which vector elements we don't want. 


```r
alphabet[c(-1,-26)] # Same as alphabet[2:24]
##  [1] "b" "c" "d" "e" "f" "g" "h" "i" "j" "k" "l" "m" "n" "o" "p" "q" "r"
## [18] "s" "t" "u" "v" "w" "x" "y"
alphabet[-1:-10]
##  [1] "k" "l" "m" "n" "o" "p" "q" "r" "s" "t" "u" "v" "w" "x" "y" "z"
```
Indexing with positive vectors is usually preferred, as the intent of the code is more clear, but sometimes this form can be clearer when constructing the "anti-set" is easier (e.g. when dropping the first value).

#### Subsetting with a Character Index {-}
If the elements of our vector have names, we can use those names instead of their positions. 

```r
x <- 1:5
names(x) <- c("A","B","C","D","F")
x
## A B C D F 
## 1 2 3 4 5
x[c("B","F")]
## B F 
## 2 5
```

#### Subsetting with a Logical Index {-}
When subsetting with a logical index vector, you supply a vector specifying whether to extract a specific element (with a `TRUE`) or to *not* extract a specific element (with a `FALSE`).

Let's revisit the example of selecting the first and last elements of the alphabet vector: We make a vector of logicals and stick it in the square brackets after your vector.


```r
alphabet[c(1,26)]
## [1] "a" "z"
alphabet[c(TRUE, FALSE, FALSE, FALSE, FALSE, FALSE, FALSE, FALSE, FALSE,
           FALSE, FALSE, FALSE, FALSE, FALSE, FALSE, FALSE, FALSE, FALSE,
           FALSE, FALSE, FALSE, FALSE, FALSE, FALSE, FALSE, TRUE)]
## [1] "a" "z"
```


But this specific example is not a good use case for logical vectors. Why?

1. Longer Code: length of the logical vector must match the length of the object its subsetting.
2. Duplicating work: If you already know the position of the elements you want, just put them into a vector and you're done!

The logical vector's utility comes into play when you *don't* know the numeric positions of the elements you are interested in. But, how can you determine which values you want to keep without knowing their position or their name? In these cases, we must *search* for values meeting a specific criteria. Searching for values within a data structure is a processes called **Indexing**.

### Indexing
Indexing a data structure in a search for specific values is a job for R's [relational operators](#relational-operators). Remember, relational operator are applied to all the elements of a data structure individually (i.e., "element-wise"). Thus, we can apply them to search for specific values, and use the Logical `TRUE`/`FALSE` values that result from this search as an index vector.


```r
x <- 2:11
print(x)
##  [1]  2  3  4  5  6  7  8  9 10 11
x <= 5 # Apply the less than or equals test
##  [1]  TRUE  TRUE  TRUE  TRUE FALSE FALSE FALSE FALSE FALSE FALSE
```

As you can see, values that meet the criteria (<= 5) return as `TRUE`. 

```r
x[x <= 5] # Index vector x with the results of the test. 
## [1] 2 3 4 5
```
When this logical vector is used to index the vector `x`, only the elements where the logical vector has value `TRUE` are returned.

We index character vectors using the `==` and `!=` operators, but not the greater/less than operators. Quantity makes no sense for characters!

```r
months <- c("January", "February", "March", "April", "May", "June", "July",
            "August", "September", "October", "November", "December")
months == "June" # The sixth element is TRUE
##  [1] FALSE FALSE FALSE FALSE FALSE  TRUE FALSE FALSE FALSE FALSE FALSE
## [12] FALSE
months[months == "June"]
## [1] "June"
months[months != "July"]
##  [1] "January"   "February"  "March"     "April"     "May"      
##  [6] "June"      "August"    "September" "October"   "November" 
## [11] "December"
```

#### Other Useful Tests: `is.na()` {-}
Unfortunately, we often have to deal with missing observations in real world data sets. R codes missing data as `NA` (or sometimes `NaN`). We can use the `is.na()` function to find any missing values in a vector. 

```r
missingno <- c(10,NA,1,4,2,NA,NA,99,NaN, NA)
is.na(missingno)
##  [1] FALSE  TRUE FALSE FALSE FALSE  TRUE  TRUE FALSE  TRUE  TRUE
missingno[!is.na(missingno)] # Select the *not* missing observations
## [1] 10  1  4  2 99
```

Removing missing values in a common step in data manipuation before an anlysis. For examples of removig missing values with other data structures, and more realistic data sets, the examples in the [Data Sets](#data-sets) section.

#### Converting a Logical to a Positional Index {-}
A useful function to know is `which()`. When used on a logical vector, it will return to your the position indices of the vector's `TRUE` element. It is useful when you want to know **where** in the vector your matches occur. 

```r
is.na(missingno)
##  [1] FALSE  TRUE FALSE FALSE FALSE  TRUE  TRUE FALSE  TRUE  TRUE
which(is.na(missingno))
## [1]  2  6  7  9 10
```

### Replacement
To replace the values in a vector (e.g., to replace empty characters with `NA` values), move the indexing and subsetting operation to the right-hand side of the assignment operator, and put the replacement value(s) on the left-hand side.

```r
song <- c("Happy", "Birthday", "", "You") 
song[song == ""] <- NA
song
## [1] "Happy"    "Birthday" NA         "You"
```

#### A word of advice {-#replacement-advice .advice}
Unlike subsetting, replacement *does* present the risk of data loss, because once a value is replaced, it can't be undone. So, when you are experimenting, we recommend you make a "backup" copy of your data structure before editing it. In the previous example, with the "Happy Birthday" lyrics, you might do the following.


```r
song <- c("Happy", "Birthday", "", "You")
song_original <- song
song[song == ""] <- NA
song
## [1] "Happy"    "Birthday" NA         "You"
song_original
## [1] "Happy"    "Birthday" ""         "You"
```

This way, you have a copy of the original data, in case there was a bug in your code, or you need the raw data later on for another operation.

### Matrices & Data Frames
Now we'll learn how to index data structures with more than one dimension, like matrices and data frames. Recall that matrices and data frames have both rows **and** columns,meaning that when we subset or index them, we must specify which rows and/or columns we would like out subset or search to apply to.

#### Matrices {-}
To index a matrix, all that is required is to have two vectors inside our square brackets, separated from each other by a comma. The template is: `OurBigMatrix[rowIndex, columnIndex]`

Like with vectors, the index vectors can be either:

- Numeric vectors specifying the position of the rows/columns we want to access
- Character vectors specifying the names of the rows/columns we want to access (if they have names)
- Logical vectors specifying for each column and row whether we want to access it (`TRUE`)  or ignore it (`FALSE`)


```r
dummy <- matrix(6:1, nrow = 2)
dummy
##      [,1] [,2] [,3]
## [1,]    6    4    2
## [2,]    5    3    1
dummy[1,2:3] # Row 1, Column 2 and 3. Output is a vector! 
## [1] 4 2
dummy[1:2,2:3] # Row 1 and 2, Column 2 and 3. Output is a matrix.
##      [,1] [,2]
## [1,]    4    2
## [2,]    3    1
```

If you want to select **all** of one dimension, (e.g., keep all rows or all columns) but  index the other dimension, provide the separating comma as usual, but don't give any indexing vector for the dimension you want to stay 100% intact.

```r
dummy[1,] # First Row, all columns
## [1] 6 4 2
dummy[1,1:3] # Same as previous
## [1] 6 4 2
dummy[,2] # All rows, second columns
## [1] 4 3
dummy[1:2,2] # Same as previous
## [1] 4 3
```

We can apply our relational operators to entire matrices in the same manner as vectors. The resulting logical matrix has the same dimensions as the one we apply the test to.

```r
dummy < 4 # 2 x 3
##       [,1]  [,2] [,3]
## [1,] FALSE FALSE TRUE
## [2,] FALSE  TRUE TRUE
```

We can also apply logical testing and logical indexing to specific dimensions of a matrix. This example here keeps all the columns of the matrix with a sum less than 8.

```r
colSums(dummy) # colSums() adds up each column
## [1] 11  7  3
colSums(dummy) < 8 # Does each column sum to less than 8?
## [1] FALSE  TRUE  TRUE
dummy[,colSums(dummy) < 8] # Select columns with a sum less than 8
##      [,1] [,2]
## [1,]    4    2
## [2,]    3    1
```

#### Data Frames {-}
To learn about data frames, we're going to use several data frames that come built-in with R as part of the `datasets` package. Try typing `InsectSprays`, `iris`, `airquality` and `mtcars` into the console to be sure they are loaded and available to you. Since they are included as part of a package, you will *not* see them listed in your environment pane. 

The `[row, column]` indexing style used with matrices also applies to data frames. However, data frames also support a different subsetting technique based on a special syntax that applies to it's column names. Subsetting or indexing a data frame using column names should be preferred to using column numbers, because that name is unlikely to change, while the row or column number is **very** likely to get changed throughout the course of an analysis. It's also much easier to remember the name of something than remember its position in a data frame!

##### Subsetting with $ syntax {-}

To subset a single column from a data frame, we can use that columns name, and the `$` operator. In this case, quotes around the column's name are not required. To demonstrate, we will subset the `mpg` column from the `mtcars` dataset.

```r
mtcars
##                      mpg cyl  disp  hp drat    wt  qsec vs am gear carb
## Mazda RX4           21.0   6 160.0 110 3.90 2.620 16.46  0  1    4    4
## Mazda RX4 Wag       21.0   6 160.0 110 3.90 2.875 17.02  0  1    4    4
## Datsun 710          22.8   4 108.0  93 3.85 2.320 18.61  1  1    4    1
## Hornet 4 Drive      21.4   6 258.0 110 3.08 3.215 19.44  1  0    3    1
## Hornet Sportabout   18.7   8 360.0 175 3.15 3.440 17.02  0  0    3    2
## Valiant             18.1   6 225.0 105 2.76 3.460 20.22  1  0    3    1
## Duster 360          14.3   8 360.0 245 3.21 3.570 15.84  0  0    3    4
## Merc 240D           24.4   4 146.7  62 3.69 3.190 20.00  1  0    4    2
## Merc 230            22.8   4 140.8  95 3.92 3.150 22.90  1  0    4    2
##  [ reached getOption("max.print") -- omitted 23 rows ]
mtcars$mpg
##  [1] 21.0 21.0 22.8 21.4 18.7 18.1 14.3 24.4 22.8 19.2 17.8 16.4 17.3 15.2
## [15] 10.4 10.4 14.7 32.4 30.4 33.9 21.5 15.5 15.2 13.3 19.2 27.3 26.0 30.4
## [29] 15.8 19.7 15.0 21.4
```
This `$` syntax *can not* be applied to rows.

##### Subsetting with [] syntax {-}
To subset *multiple* columns, or to subset specific rows, we need to use `[row,column]` style indexing (not the `$`).

But we're not forced to use numeric vectors just because we're using the `[` operator. We can select multiple columns by their names using a character vector that has the names of our desired columns as its elements.


```r
mtcars[,c("mpg","disp","gear")] # need  as well as the quotes here
##                      mpg  disp gear
## Mazda RX4           21.0 160.0    4
## Mazda RX4 Wag       21.0 160.0    4
## Datsun 710          22.8 108.0    4
## Hornet 4 Drive      21.4 258.0    3
## Hornet Sportabout   18.7 360.0    3
##  [ reached getOption("max.print") -- omitted 27 rows ]
```

One of the most common subsetting tasks with a data frame (or matrix) is the need to select values in one column where the values in another column meet a certain criteria. For example, you might want to select all the values in the column holding reaction times where participants were incorrect. There are 2 syntactic approaches to this, both of which use relational operators and logical indexing.

##### Method 1: Index the data frame itself {-}
We will use the `[row,column]` method to pick out the values of the `count` column in `InsectSprays` where spray A was used. First, we will build up a logical vector to index the correct rows by testing where the spray column has value 'A'

```r
InsectSprays$spray
##  [1] A A A A A A A A A A A A B B B B B B B B B B B B C C C C C C
##  [ reached getOption("max.print") -- omitted 42 entries ]
## Levels: A B C D E F
InsectSprays$spray=="A"
##  [1]  TRUE  TRUE  TRUE  TRUE  TRUE  TRUE  TRUE  TRUE  TRUE  TRUE  TRUE
## [12]  TRUE FALSE FALSE FALSE FALSE FALSE FALSE FALSE FALSE FALSE FALSE
## [23] FALSE FALSE FALSE FALSE FALSE FALSE FALSE FALSE
##  [ reached getOption("max.print") -- omitted 42 entries ]
```

Next, we combine this with a character vector of the column names we're interested in, and put it inside our `[]` brackets

```r
InsectSprays[InsectSprays$spray=="A",'count']
##  [1] 10  7 20 14 14 12 10 23 17 20 14 13
```
If we leave the column vector out, this statement will return a data frame. Can you guess how many unique values will be in the spray column in this case?

#### Method 2: Index a vector *from* the data frame {-}

Here, we will use the `$` operator to subset the `count` column from the `InsectSprays` data frame. Then, index this vector with the logical vector resulting from a relational test


```r
InsectSprays$count[InsectSprays$spray=="A"] # Same result as before
##  [1] 10  7 20 14 14 12 10 23 17 20 14 13
```

##### Errors when indexing by name {-}
If you try to subset a column of a data frame using the `$` operator, but the name of the column doesn't exist, `R` will return `NULL`

```r
InsectSprays$neeeeeeighhhhh
## NULL
```

But, if you use the `[row,column]` style of indexing and ask for a column that doesn't exist, you get a formal error. 


```r
InsectSprays[, 'neeeeeeighhhhh']
```

```{.Rerrormsg}
## Error in `[.data.frame`(InsectSprays, , "neeeeeeighhhhh"): undefined columns selected
```

Its also a common mistake to forget the quotes around names inside the `[]` brackets, which will throw an "object not found" error (unless the an object with the same name just happens to exist by coincidence, in which case you will probably get another type of error).


```r
InsectSprays[, spray]
```

```{.Rerrormsg}
## Error in `[.data.frame`(InsectSprays, , spray): object 'spray' not found
```

#### Reproducibility {-}
The R session information when compiling this book is shown below:

```r
library(printr)
sessionInfo()
## R version 3.5.1 (2018-07-02)
## Platform: x86_64-apple-darwin15.6.0 (64-bit)
## Running under: macOS  10.14.5
## 
## Matrix products: default
## BLAS: /Library/Frameworks/R.framework/Versions/3.5/Resources/lib/libRblas.0.dylib
## LAPACK: /Library/Frameworks/R.framework/Versions/3.5/Resources/lib/libRlapack.dylib
## 
## locale:
## [1] en_US.UTF-8/en_US.UTF-8/en_US.UTF-8/C/en_US.UTF-8/en_US.UTF-8
## 
## attached base packages:
## [1] stats     graphics  grDevices utils     datasets  methods   base     
## 
## other attached packages:
## [1] printr_0.1
## 
## loaded via a namespace (and not attached):
##  [1] compiler_3.5.1   magrittr_1.5     bookdown_0.11    htmltools_0.3.6 
##  [5] tools_3.5.1      Rcpp_1.0.1       codetools_0.2-15 stringi_1.4.3   
##  [9] rmarkdown_1.13   knitr_1.22       stringr_1.3.1    xfun_0.6        
## [13] digest_0.6.19    packrat_0.4.9-3  evaluate_0.14
```
