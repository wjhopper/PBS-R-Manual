# Statistics



## The Data

We rely on the `sat.act` data set from the `psych` package for all demonstrations in this chapter. 

To make the data set available on your own computer, run the following code:


```r
install.packages('psych')
library(psych)
```

You should now be able to access the `sat.act` data set:


```r
summary(sat.act)
```

```
##      gender        education          age             ACT       
##  Min.   :1.000   Min.   :0.000   Min.   :13.00   Min.   : 3.00  
##  1st Qu.:1.000   1st Qu.:3.000   1st Qu.:19.00   1st Qu.:25.00  
##  Median :2.000   Median :3.000   Median :22.00   Median :29.00  
##  Mean   :1.647   Mean   :3.164   Mean   :25.59   Mean   :28.55  
##  3rd Qu.:2.000   3rd Qu.:4.000   3rd Qu.:29.00   3rd Qu.:32.00  
##  Max.   :2.000   Max.   :5.000   Max.   :65.00   Max.   :36.00  
##                                                                 
##       SATV            SATQ      
##  Min.   :200.0   Min.   :200.0  
##  1st Qu.:550.0   1st Qu.:530.0  
##  Median :620.0   Median :620.0  
##  Mean   :612.2   Mean   :610.2  
##  3rd Qu.:700.0   3rd Qu.:700.0  
##  Max.   :800.0   Max.   :800.0  
##                  NA's   :13
```

As you can see, this data set includes self-reported SAT and ACT scores along with demographic information from 700 respondents. It therefore contains both categorical and continuous variables that are appropriate for both within- and between-subjects analyses. For more information, refer to the help documentation by running the command `?sat.act`.

## Formula Objects

Formulas are special objects in R that are used in a range of functions discussed in this chapter, notably `plot()`, `aov()`, and `lm()`. The use of formulas in these specific functions will be discussed in more detail below, but we first introduce their general properties here. 

The basic structure is as follows:

```y ~ x```

The `y` variable is the dependent variable, the `x` variable is the independent variable, and the `~` operator can be taken to mean "as a function of". Thus, the above example is a formula for "y as a function fo x".

For a more concrete example, consider the effect of the independent variable age on the dependent variable ACT scores:

```sat.act$ACT ~ sat.act$age```

We can have multiple independent variables in the formula. For instance, if we wanted to define ACT scores as a function of age and education, we would do so as follows:

```sat.act$ACT ~ sat.act$age + sat.act$education```

The above formula is an example of an "additive" model, in which ACT scores are a function of the additive effects of age and education. To define the interactive model, we would separate age and education by an asterisk instead of a plus sign:

```sat.act$ACT ~ sat.act$age*sat.act$education```

Note that the above formula defines the *full* interactive model, so ACT scores are defined as a function of (1) the main effect of age, (2) the main effect of education, and (3) the interaction of age and education. This same model can be implemented by definind each components separately in the formula object, like this:

```sat.act$ACT ~ sat.act$age + sat.act$education + sat.act$age:sat.act$education```

By defining the components separately, you can pick and choose which to keep. For instance, you could specify only the main effect of age and the interaction of age and education, as follows:

```sat.act$ACT ~ sat.act$age + sat.act$age:sat.act$education```

Formulas can get much more complex than the examples here, which only extend as far necessary for the functions and tests that you are likely to use as an instructor in undergraduate psychology program. A comprehensive tutorial can be found [here](https://www.datacamp.com/community/tutorials/r-formula-tutorial). 

## Descriptive Statistics

### Mean



### Median



### Variance



### Standard Deviation



### Standard Error



### Correlation



### Summary Functions



## Plotting

### Scatter & Line Plots



### Barplots



### Histograms



### Boxplots



### Configuring Plots



### What about ggplot?



## Inferential Statistics

### Distribution Functions

The sections below introduce functions for executing common statistical tests. However, R's family of distribution functions also make it possible to explore the underlying probability distributions for each test, and to carry out each test by hand. We therefore begin our demonstration of inferential statistics by giving an overview of these powerful intructional tools. 

Each distribution can be accessed by a set of four functions, each labelled with the name of the distribution and a prefix:

Prefix | Task | Example
:-------|:------------------------------------------------------|:-------
p | Get the probability of a value above or below a point on the x-axis. | `pnorm()`
d | Get the probability density associated with a point on the x-axis. | `dnorm()`
q | Get the x-axis value marking a cumulative probability. | `qnorm()`
r | Get a random sample from the distribution. | `rnorm()`

### T-Tests



### ANOVA



### Correlation



### Binomial Tests



### Chi-Square



### Linear Models










