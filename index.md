--- 
title: "UMass PBS Instructors R Manual"
author: Andrea Cataldo & Will Hopper
date: "Jun 13, 2019"
site: bookdown::bookdown_site
output:
  bookdown::gitbook:
    config:
      toc_depth: 2
      toc:
        collapse: none
        before: |
          <li><a href="index.html">UMass PBS Instructors R Manual</a></li>
      download: ["pdf"]
      sharing:
        github: yes
        facebook: no
        twitter: no
        all: no
  bookdown::pdf_book:
    latex_engine: xelatex
urlcolor: blue
font-family: "DejaVu Sans"
mainfont: "DejaVu Sans"
documentclass: book
css: ["css/custom.css"]
github-repo: wjhopper/PBS-R-Manual
description: "A guide for instructors teaching statistics and research methods with R in the UMass Amherst Psychological & Brain Sciences Department."
---



# Preface {-}



This book serves as a guide for instructors teaching statistics and research methods with R in the UMass Amherst Psychological & Brain Sciences Department. The book is organized into three main sections:

1. An introduction to the R language and the RStudio development environment.
2. A reference for performing and interpreting common statistical tests in R.
3. Recommended best practices for instructors.

Section 1 is primarily intended for instructors who are unfamiliar with the R language and its ecosystem. However, we recommend even experienced R users read it, as it is useful to remind yourself of the perspective the majority of your students will be approaching R from, and to get "on the same page" as your fellow instructors.

Much of the information in this guide can be found in any "Introduction to R" book or resource. So, why write another one? There are two main reasons:

1. To provide you with the information you need specifically for Psych 240 and 241, and none of the information you don't.
2. To provide opinionated guidance and reference material for the instructor specifically, not just the "average" R user.

We hope that you find this guide useful as you embark on teaching a new class!

## Getting Started {-}
To follow along with the examples in this book, it is recommended that you install the latest version of R and RStudio to your personal computer.

### The R Language {-}

R is a complete programming language and computing environment designed to enable statistical modeling, data manipulation, and reporting. Despite its focus on statistics, it is still very feature rich - we can use it to perform mathematical calculations, create data visualizations, connect to remote databases, save results as files on your computer's hard drive, and more. In fact, this entire manual was created in R!

To get started, download and install the most current version of R for your operating system:

- Windows: https://cran.r-project.org/bin/windows/base/
- Mac OS X: https://cran.r-project.org/bin/macosx/
- Linux: https://cran.r-project.org/bin/Linux/ (as always, see instructions for your specific Linux distribution)

### RStudio {-}

RStudio is a graphical program that simplifies common R-related tasks, organizes information about your current R session, and generally makes it easier to use R. This type of program is called an **I**ntegrated **D**evelopment **E**nvironment, or IDE for short.

We **strongly** recommend using R within RStudio. However, it is important to understand that they are separate and distinct programs, and R can freely be used outside of RStudio. 

To help your students understand their relationship, you could offer the following analogy: RStudio is like a workbench and toolbox, where R is like a hammer. You do not *need* a workbench and toolbox to build something with your hammer - but having a workbench to rest your project on while you work, and a toolbox where all your nails are organized is very convenient, and will probably help you do your work faster and more accurately.

The company which produces the RStudio software has several different products with "RStudio" in the name. You and your students will want to install the "RStudio Desktop - Open Source License" version (i.e., the free version):

- RStudio Desktop for Windows: http://rstudio.org/download/latest/stable/desktop/windows/RStudio-latest.exe
- RStudio Desktop for Mac OS X: http://rstudio.org/download/latest/stable/desktop/mac/RStudio-latest.dmg
- RStudio Desktop for Linux: [Get .deb and .rpm packages here](https://www.rstudio.com/products/rstudio/download/#download)


## Formatting Conventions {-}
R source code (i.e., code run in the R console or from an R script) and output are presented in monospace font in regions with a light gray background. We **do not** include command line prompts (`>` and `+`, like you would see in a real R console) in the R source code. This is to allow you to conveniently copy and run the code without having to delete the leading prompt symbols. All text output from executing R commands is denoted with two preceeding hashes (`##`). So, any time you see `##` following a block of R code, you are looking at the output of the preeceeding command.

## Additional Materials {-}
As alluded to earlier, this is far from the only "Intro to R" material in existence, though this guide has been developed with PSYCH 240 and 241 specifically in mind. But we do encourage you to supplement your knowlege with outside sources. We recommend the following resources to learning about R programming:

- [R in a Nutshell](http://silk.library.umass.edu/login?url=https://search.ebscohost.com/login.aspx?direct=true&db=cat06087a&AN=umass.016355822&site=eds-live&scope=site) by Joseph Adler
- [The Art of R Programming](http://silk.library.umass.edu/login?url=https://search.ebscohost.com/login.aspx?direct=true&db=cat06087a&AN=umass.016669840&site=eds-live&scope=site) by Norman Matloff
- [R for Data Science](https://r4ds.had.co.nz/) by Garrett Grolemund & Hadley Wickham
- [Advanced R](https://adv-r.hadley.nz/) by Hadley Wickham

On campus, there are two great consulting resources for getting help with statistical analysis in R:

- [The Institute for Social Science Research](https://www.umass.edu/issr/what-we-do/consultation)
- [The Center for Research on Famlies](https://www.umass.edu/family/methodology-department)

#### Reproducibility {-}
The R session information when compiling this book is shown below:

```r
sessionInfo()
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
##  [1] compiler_3.5.1   magrittr_1.5     bookdown_0.11    htmltools_0.3.6 
##  [5] tools_3.5.1      yaml_2.2.0       Rcpp_1.0.1       codetools_0.2-15
##  [9] stringi_1.4.3    rmarkdown_1.13   knitr_1.22       stringr_1.3.1   
## [13] xfun_0.6         digest_0.6.19    packrat_0.4.9-3  evaluate_0.14
```
