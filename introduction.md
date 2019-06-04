# Introducing R and RStudio



![](images/rstudio.png)

## Pane #1: The R Console

The R console allows you to execute R code (often called expressions, or commands) and see the results printed out. To execute R commands, place your cursor at the prompt (the `>` symbol), type in your code, and press <kbd>Enter</kbd>.

A simple and easy way to demonstrate using R console is to perform some basic arithmetic, just like a calculator. R has 5 basic arithmetic operators:

- `+` for addition
- `-` for subtraction
- `*` for multiplication
- `/` for division
- `^` for exponentiation


```r
2 + 2
## [1] 4
```

R respects the [order of operations](https://en.wikipedia.org/wiki/Order_of_operations) (i.e. PEMDAS) by default, so if you want to for a specific operation to be executed fist, you need to surround it with parenthesis. To see how grouping with parenthesis affects arithmetic operations, compare the following two examples:

```r
2*10 + 3/10
## [1] 20.3
2*(10 + 3)/10
## [1] 2.6
```

The R console is the best place to start immersing yourself in the language by experimentation. It allows you to "code as you go"; run one command, see what you get, adjust it and test it again. However, the interactive nature of using the R console makes it a poor choice for saving your work to use again later, and for complicated, multi-step operations.

When you know you need to repeat the same step again in the future (i.e. re-use code), or you have a task that requires intermediate steps, you should organize all your code into an R script. RStudio allows you to easily write and interactively test out your R scripts in the **Editor Pane**, which is introduced below.

## Pane #2: The Editor
The editor pane is where you can create, modify, and save plain text documents, and is designed to help you create and execute R scripts. An R script is just a text file that contains valid R code (the same kind of commands you would enter into the console), and commonly carries the `.R` file extension. You can think of an R script like a stand-alone computer program, but instead of double-clicking to run it, you run it via the R interpreter.

You can create a new, blank R script by going to the File Menu ➡ New File ➡ R Script.

Rstudio provides several methods for executing the code you've written. You can:

- Run a single line of code by placing your cursor on that line and pressing <kbd>Ctrl</kbd> + <kbd>Enter</kbd> (<kbd>⌘ Command</kbd> + <kbd>Enter</kbd> on Mac).
- Run multiple lines of code by selecting all the lines your want to run with your cursor and pressing  <kbd>Ctrl</kbd> + <kbd>Enter</kbd> (<kbd>⌘ Command</kbd> + <kbd>Enter</kbd> on Mac). 
- Run the entire R script by pressing the "Source" button in the top-right corner of the editor pane, or using the <kbd>Ctrl</kbd> + <kbd>Shift</kbd> + <kbd>S</kbd> keyboard shortcut (<kbd>⌘ Command</kbd>  + <kbd>Shift</kbd> + <kbd>S</kbd> on Mac)

No matter which method you use, the code you choose to run will be executed in the R console below, and you will see any results printed out there as well.

Another reason to write your code in an R script is to keep a short explanation of what you are doing (and why!) together with your code. These short explanations are called *comments* In R, you write a comment by prefixing the comment's text with the pound sign `#`. Comments can go on their own line, or at the end of an R expression.


```r
# Hi I am a comment. 
# The R interprer ignores me!
2*10 + 3/10 # comments can go here too!
## [1] 20.3
```

## Pane #3
### Variables and the Environment
As your R scripts grow beyond adding and subtracting a few numbers, you will often want to save the results of your computations (like datasets, or the results of statistical models) to use multiple times, or to increase the clarity of your code. In R, you can save a value for later use by assigning it to a *variable* name.

You can create a new variable using the assignment operator `<-`, using the syntax `name <- value` where `name` is a syntactically valid name, and `value` is the value you wish to assign. To demonstrate creating a variable, consider the code below, where I create a variable named `x`, assign it a value of 1, and then print out its value by typing `x` into the console and pressing enter.


```r
x <- 1
x
## [1] 1
```

If you execute the first command `x <- 1` in your own console, you should see it appear in the "Environment" pane of the RStudio window. The environment pane is helpful for keeping track of all the variables you've created. If you ever want to clear your Environment (i.e., delete all the variables you've created), you can press the ![](images/clear.png) button.

Variable names are mostly arbitrary (we could have used `elephant` or `jumpluff` instead of `x`), but there are some restrictions on variable names, the most important being:

1. Spaces are not allowed
2. The name cannot begin with a number.

See the "Details" section of the [make.names](https://www.rdocumentation.org/packages/base/versions/3.6.0/topics/make.names) help page, and the [list of reserved keywords](https://www.rdocumentation.org/packages/base/versions/3.6.0/topics/Reserved) for a complete set of naming rules.

### History
