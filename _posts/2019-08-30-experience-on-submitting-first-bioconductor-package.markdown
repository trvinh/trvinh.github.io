---
layout: post
comments: true
title: My experience on submitting First Bioconductor package
date: 2019-08-30
---

Table of Contents
=================

   * [My tips for better R coding](#my-tips-for-better-r-coding)
      * [Code format](#code-format)
      * [Best practices](#best-practices)
      * [Efficient code](#efficient-code)
   * [How to create an R package](#how-to-create-an-r-package)
   * [How to submit your package to Bioconductor](#how-to-submit-your-package-to-bioconductor)

# My tips for better R coding
## Code format
Coding style and format must be consistent throughout the package:
* *[camelCaps](https://en.wikipedia.org/wiki/Camel_case)* for `variable` and `function` names
* 4 spaces for [`indent`](https://en.wikipedia.org/wiki/Indentation_style) (no tabs!)
* no lines longer than 80 characters
* always use space after a comma
* use `<-` not `=` for assignment
* [...](http://bioconductor.org/developers/how-to/coding-style/)

*You should check the required format of the software repository, where you are planning to submit your program/package to.*

## Best practices
* Avoid repeated code! If you need to write a chunk of code in many places, better making a *helper* function
* Function length should have ≤ 50 lines
* Function and its arguments need to be well documented: their description, data type and format of input and output, example. Arguments should have default values.
* Avoid adding new dependent library/package. Try to utilize one package as much as possible. If the needed functions are available in [the R base package](https://stat.ethz.ch/R-manual/R-devel/library/base/html/00Index.html), there is no need to use other libraries if you don't have a convincing reason (e.g. higher speed). For example, here are [list of equivalent functions](https://stringr.tidyverse.org/articles/from-base.html) between `R base` and `stringr`. *Tip: check the barely used libraries and try to replace them.*
* Communicate with users using function `message("hello friend")` and give informative error message with `stop("hey, this is wrong input!")` instead of just return NULL `return()`
* Others:
  * Use `vapply()` instead of `sapply()` and use the various `apply` functions instead of `for` loops
  * Use `seq_len()` or `seq_along()` instead of `1:...`
  * Use `TRUE/FALSE` instead of `T/F`
  * Avoid `class()==` and `class()!=` instead use `is()`
  * Use `system2()` instead of `system`
  * Avoid the use of `<<-`
  * [...](https://www.bioconductor.org/developers/package-guidelines/#rcode)

## Efficient code
* Utilize [vectorized functions](http://alyssafrazee.com/2014/01/29/vectorization.html) and avoid loops (`for`, `while` or `apply` family). If loop is a must, keep it as simplest as you can.
* Avoid *copy-and-append* (e.g. `rbind`), use pre-allocatea-and-fill instead (e.g. `lapply()`)
* [...](https://bioconductor.org/developers/how-to/efficient-code/)

In the following example, I will use different methods to do the same thing (filter the input phylogenetic profile data and return only lines, where an orthologous protein is found) and return a new data frame as an output. The input data frame has 5 columns (“geneID”, “ncbiID”, “orthoID”, “Domain_similarity”, “traceability”) and 3234 lines. The output will contain 3 columns (“geneID”, “ncbiID”, “orthoID”) and 1366 lines.

1. Using `for` loop and the returned data frame is not preallocated:
```r
forloop <- function(df) {
    newDf <- data.frame()
    for (i in seq_len(nrow(df))) {
        if (!is.na(df$orthoID[i])) {
            newRow <- df[i, c("geneID", "ncbiID", "orthoID")]
            newDf <- rbind(newDf, newRow)
        }
    }
    return(newDf)
}
```
2. Using `for` loop and the returned data frame is preallocated:
```r
forloopPreallocate <- function(df) {
    newDf <- data.frame(
        geneID = character(nrow(df)),
        ncbiID = character(nrow(df)),
        orthoID = character(nrow(df)),
        stringsAsFactors = FALSE
    )
    j <- 1
    for (i in seq_len(nrow(df))) {
        if (!is.na(df$orthoID[i])) {
            newDf[j, ] <- df[i, c("geneID", "ncbiID", "orthoID")]
            j <- j + 1
        }
    }
    return(newDf)
}
```
3. Using `sapply()` function (this, or other members of the “apply” function like `apply()`, `lapply()`, `vapply()` are just alternative forms of “for-loop”, but they are more effective in some cases):
```r
sapplyfn <- function(df) {
    tmp <- sapply(
        seq_len(nrow(df)),
        function (i) {
            if (!is.na(df$orthoID[i])) {
                return(df[i, c("geneID", "ncbiID", "orthoID")])
            }
        }
    )
    return(do.call(rbind, tmp))
}
```
4. Using vectorized function `subset()`:
```r
vectorizefn <- function(df) {
    return(subset(df, !is.na(df$orthoID)))
}
```

And this is the benchmark of those 4 functions in term of the calculation speed:
```r
Unit: microseconds
               expr       min       mean      median         max
            forloop 305481.51 366958.9898 346929.2885 593712.576
 forloopPreallocate 170936.60 231358.6863 206390.8430 705593.905
           sapplyfn 126964.86 156118.3279 143331.4655 358841.424
        vectorizefn    182.09    309.3324    262.5635   2883.188
```

There are some more approaches to speed up your code, you can find them at:
* [Strategies to speed up r code](https://datascienceplus.com/strategies-to-speedup-r-code/)
* [Efficient R programming](https://csgillespie.github.io/efficientR/index.html)

# How to create an R package
This [book of Hadley Wickham](http://r-pkgs.had.co.nz) is an excellent resource for learning about creating an R package. It will guide you from explaining the package structure to describing each individual component of a package that one needs to build. Or this [post](https://masalmon.eu/2017/12/11/goodrpackages/) shows you how to develop a good R package. I suggest you should read it first before starting.

Some important things I have learned and experienced during developing my first [R package](https://bioc.ism.ac.jp/packages/devel/bioc/html/PhyloProfile.html) are:
* Write good documentation, including [man pages](http://r-pkgs.had.co.nz/man.html) for all the functions and [vignettes](http://r-pkgs.had.co.nz/vignettes.html) for the package. `man` file, or manual for a function, need to be detailed. Beside describing the purpose of the function and meaning of function's parameters (arguments), one needs to give also the data types of input and output, and an (should be runable) example showing the usage of that function. A `vignette` is, different from the `man` files, used for explaining the details of the whole package, as well as for demonstrating some *real* use-cases using the package.
* Test the functions with `testthat`
* Use [continuous integration](https://docs.travis-ci.com/user/for-beginners/) (such as [travis-ci](https://travis-ci.com/)) for automatically build and test code changes
* Write good documentation (yes, it need to be repeatedly emphasized!)

# How to submit your package to Bioconductor
To [submit an R package](https://www.bioconductor.org/developers/package-submission/) to Bioconductor, you need to follow their [package](https://www.bioconductor.org/developers/package-guidelines/#rcode) and [contribution](https://github.com/Bioconductor/Contributions) guidelines. I summarize here some main steps you need to do and what you should notice.
1. Store your R package in [GitHub](https://help.github.com/articles/create-a-repo/). The source code must be in the `master` branch of that GitHub repository.
2. [Add SSH keys](https://help.github.com/articles/connecting-to-github-with-ssh/) to your GitHub account.
    1. ssh-keygen -t rsa -b 4096 -C “your_email@example.com”
    2. copy and add the generated ssh public key to github under https://github.com/settings/keys
    3. add the private key file into ~/.ssh/config (change `id_rsa_private` by the file name of your private key)
```
host git.bioconductor.org
     HostName git.bioconductor.org
     IdentityFile ~/.ssh/id_rsa_private
     User github_user_name
```
3. [Open a new issue](https://github.com/Bioconductor/Contributions/issues/new) in Bioconductor contributions GitHub. Share the link to your package repository and use the `package name` as the title of that issue.
4. [Add a webhook](https://github.com/Bioconductor/Contributions#adding-a-web-hook) to your repository in order to automatically trigger a package build when you push a *valid commit* to the master branch.

A *valid commit* is recognized by the version of the package. For the first commit, start with `Version: 0.99.0`. Whenever you want to trigger a new package build and check on the Bioconductor server, you push a commit with a version bump to `Version: 0.99.1`. Read [this post](https://bioconductor.org/developers/how-to/version-numbering/) to understand how the version number in Bioconductor is controlled.

After being accepted, your package will be available in Bioconductor's git repository. The SSH key you created in step 2 will be used to maintain your package (e.g. bug fix, add new features,...). Make sure that *the email linked to your package* must be the same as the one being shown in your [BioC git profile](https://git.bioconductor.org/BiocCredentials) and it also must be present in your github account (can also be the alternative email).

In some cases, you want to submit a related package (such as an [experiment data package](https://bioconductor.org/packages/release/bioc/vignettes/ExperimentHub/inst/doc/CreateAnExperimentHubPackage.html)), that is located in another GitHub repository. To do that, you just need to post a comment in the current issue like `AdditionalPackage: https://github.com/username/repositoryname`. This must be posted by YOU, the same GitHub user that created the issue. And you also need to [add a webhook](https://github.com/Bioconductor/Contributions#adding-a-web-hook) to that related package, the same as you did for the main package.
