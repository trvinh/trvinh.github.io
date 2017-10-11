---
layout: post
comments: true
title: Data frame
date: 2017-05-04
excerpt_separator: <!--more-->
---

A data frame in R can be used to store a table (two dimension) data structure. Unlike matrix, different columns of a data frame can have different data type (like numeric, character, factor, etc.)

<a name="go_to_top"></a>

Table of Contents
=================

* [Create an empty data frame](#create-an-empty-data-frame)
* [Create a data frame with data](#create-a-data-frame-with-data)
* [How to work with a data frame](#how-to-work-with-a-data-frame)

<!--more-->

# Create an empty data frame
```r
> df <- data.frame("GeneID"=character(), "Species"=character(),
"TaxonID"=character(), stringsAsFactors=FALSE)
```
and use `for` loop to add values into it
```r
> GeneID = c("g01","g02","g03")
> Species = c("Human","Mouse","Cat")
> TaxonID = c(9606,10090,9685)
> for(i in 1:length()){
+ 	df[i,] <- c(paste0("GENE_",GeneID[i]),
paste("Species",Species[i]),TaxonID[i])
+ }
> df
GeneID       Species TaxonID
1 GENE_g01 Species Human    9606
2 GENE_g02 Species Mouse   10090
3 GENE_g03   Species Cat    9685
```
*Note the difference between paste0() and paste() above!!!*
[Back to Contents](#go_to_top)

# Create a data frame with data
```r
> df <- data.frame("GeneID" = c("g01","g02","g03"),
"Species" = c("Human","Mouse","Cat"),
"TaxonID" = c(9606,10090,9685))
> df
GeneID Species TaxonID
1    g01   Human    9606
2    g02   Mouse   10090
3    g03     Cat    9685
```
or
```{r}
> GeneID = c("g01","g02","g03")
> Species = c("Human","Mouse","Cat")
> TaxonID = c(9606,10090,9685)
> df <- data.frame(GeneID,Species,TaxonID)
> df
GeneID Species TaxonID
1    g01   Human    9606
2    g02   Mouse   10090
3    g03     Cat    9685
```

To check data types in a data frame, use `str(dataframe)`
```{r}
> str(df)
'data.frame':	3 obs. of  3 variables:
$ GeneID : Factor w/ 3 levels "g01","g02","g03": 1 2 3
$ Species: Factor w/ 3 levels "Cat","Human",..: 2 3 1
$ TaxonID: num  9606 10090 9685
```

Variables `GeneID` and `Species` here have data type of *factor*, instead of *characters*. To prevent **data.frame()** from automatically converting character vector to factor, use `stringsAsFactors=FALSE`
```{r}
> df <- data.frame("GeneID" = c("g01","g02","g03"),
"Species" = c("Human","Mouse","Cat"),
"TaxonID" = c(9606,10090,9685),
stringsAsFactors=FALSE)
> str(df)
'data.frame':	3 obs. of  3 variables:
$ GeneID : chr  "g01" "g02" "g03"
$ Species: chr  "Human" "Mouse" "Cat"
$ TaxonID: num  9606 10090 9685
```
[Back to Contents](#go_to_top)

# How to work with a data frame
In [next post]({% post_url 2017-10-10-Working-With-Dataframe %}) I write about how to work with a data frame, such as: subset data frame, remove rows and columns, replace values in data frame, etc.
