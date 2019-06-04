--- 
title: "UMass PBS Instructors R Manual"
author:
  - name: Will Hopper
  - name: Andrea Cataldo
date: "Jun 4, 2019"
site: bookdown::bookdown_site
output: bookdown::gitbook
documentclass: book
github-repo: wjhopper/PBS-R-Manual
description: "A guide for instructors teaching statistics and research methods with R in the UMass Amherst Psychological & Brain Sciences Department."
---



# Preface
This book serves as a guide for instructors teaching statistics and research methods with R in the UMass Amherst Psychological & Brain Sciences Department. The book is organized into three main sections:

1. An introduction to the R language and the Rstudio development environment.
2. A reference for performing and interpreting common statistical tests in R.
3. Recommended Best practices for instructors.

Section 1 is primarily intended for instructors who are unfamiliar with the R language and it's ecosystem. However, we recommended even experienced R users read it, as it is useful to remind yourself of the perspective the majority of your students will be approaching R from, and to get "on the same page" as your fellow instructors.

## Getting Started
To follow along with the examples in this book, it is recommended that you install the latest version of R and RStudio to your personal computer.

### The R Language

R is a complete programming language and computing environment designed to enable statistical modeling, data manipulation, and reporting. Despite its focus on statistics, it is still very feature rich - we can use it to perform mathematical calculations, create data visualizations, connect to remote databases, save results as files on your computers hard drive, and more. In fact, this entire manual was created in R!

To get started, download and install the most current version of R for your operating system:

- Windows: https://cran.r-project.org/bin/windows/base/
- Mac OS X: https://cran.r-project.org/bin/macosx/
- Linux: https://cran.r-project.org/bin/Linux/ (as always, see instructions for your specific Linux distribution)

### RStudio

RStudio is a graphical program that simplifies common R related tasks, organizes information about your current R session, and generally makes it easier to use R. This type of program is called an **I**ntegrated **D**evelopment **E**nvironment, or IDE for short.

We **strongly** recommend using R within RStudio. However, it is important to understand that they are separate and distinct programs, and R can freely be used outside of RStudio. 

To help your students understand their relationship, you could offer the following analogy: RStudio is like a workbench and toolbox, where R is like a hammer. You do not *need* a workbench and toolbox to build something with your hammer - but having a workbench to rest your project on while you work, and a toolbox where all your nails are organized is very convenient, and will probably help you do your work faster and more accurately.

The company which produces the RStudio software has several different products with "RStudio" in the name. You and your students will want to install the "RStudio Desktop - Open Source License" version (i.e., the free version):

- RStudio Desktop for Windows: http://rstudio.org/download/latest/stable/desktop/windows/RStudio-latest.exe
- RStudio Desktop for Mac OS X: http://rstudio.org/download/latest/stable/desktop/mac/RStudio-latest.dmg
- RStudio Desktop for Linux: [Get .deb and .rpm packages here](https://www.rstudio.com/products/rstudio/download/#download)


## Reproducibility
The R session information when compiling this book is shown below:

```r
> sessionInfo()
```

```
## R version 3.5.1 (2018-07-02)
## Platform: x86_64-w64-mingw32/x64 (64-bit)
## Running under: Windows 7 x64 (build 7601) Service Pack 1
## 
## Matrix products: default
## 
## locale:
## [1] LC_COLLATE=English_United States.1252 
## [2] LC_CTYPE=English_United States.1252   
## [3] LC_MONETARY=English_United States.1252
## [4] LC_NUMERIC=C                          
## [5] LC_TIME=English_United States.1252    
## 
## attached base packages:
## [1] stats     graphics  grDevices utils     datasets  methods   base     
## 
## loaded via a namespace (and not attached):
##  [1] compiler_3.5.1  magrittr_1.5    bookdown_0.11   htmltools_0.3.6
##  [5] tools_3.5.1     yaml_2.2.0      Rcpp_1.0.1      stringi_1.4.3  
##  [9] rmarkdown_1.13  knitr_1.22      stringr_1.3.1   xfun_0.6       
## [13] digest_0.6.19   packrat_0.4.9-3 evaluate_0.14
```

