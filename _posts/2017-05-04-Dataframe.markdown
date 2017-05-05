---
layout: post
comments: true
title: Data frame
date: 2017-05-04
excerpt_separator: <!--more-->
---

A data frame in R can be used to store a table (two dimension) data structure. Unlike matrix, different columns of a data frame can have different data type (like numeric, character, factor, etc.)

<a name="go_to_top"></a>
# Content
1. [Create an empty data frame](#create_empty_dataframe)
2. [Create a data frame with data](#create_dataframe)
3. [Rename a data frame](#rename_dataframe)

<!--more-->

<a name="create_empty_dataframe"></a>
## Create an empty data frame
```{r}
> df <- data.frame("GeneID"=character(), "Species"=character(), "TaxonID"=character(), stringsAsFactors=FALSE)
```
and use `for` loop to add values into it
```{r}
> GeneID = c("g01","g02","g03")
> Species = c("Human","Mouse","Cat")
> TaxonID = c(9606,10090,9685)
> for(i in 1:length()){
+ 	df[i,] <- c(paste0("GENE_",GeneID[i]),paste("Species",Species[i]),TaxonID[i])
+ }
> df
    GeneID       Species TaxonID
1 GENE_g01 Species Human    9606
2 GENE_g02 Species Mouse   10090
3 GENE_g03   Species Cat    9685
```
*Note the difference between paste0() and paste() above!!!*

[Back to Contents](#go_to_top)

<a name="create_dataframe"></a>
## Create a data frame with data
```{r}
> df <- data.frame("GeneID" = c("g01","g02","g03"), "Species" = c("Human","Mouse","Cat"), "TaxonID" = c(9606,10090,9685))
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
> df <- data.frame("GeneID" = c("g01","g02","g03"), "Species" = c("Human","Mouse","Cat"), "TaxonID" = c(9606,10090,9685), stringsAsFactors=FALSE)
> str(df)
'data.frame':	3 obs. of  3 variables:
 $ GeneID : chr  "g01" "g02" "g03"
 $ Species: chr  "Human" "Mouse" "Cat"
 $ TaxonID: num  9606 10090 9685
```
[Back to Contents](#go_to_top)

<a name="rename_dataframe"></a>
## Rename a data frame

```{r}
> assign("newDf",df)
> newDf
GeneID       Species TaxonID
1 GENE_g01 Species Human    9606
2 GENE_g02 Species Mouse   10090
3 GENE_g03   Species Cat    9685
```

It can be used for dynamically generating data frames
```{r}
> nameDf <- c("df1","df2","df3")
> for(i in 1:length(nameDf)){
+ 	x <- ... # create a data frame x
+ 	assign(nameDf[i],x) # rename x to value of nameDf[i]
+ }
```
[Back to Contents](#go_to_top)
