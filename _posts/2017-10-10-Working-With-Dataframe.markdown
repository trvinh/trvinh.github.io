---
layout: post
comments: true
title: Working with Data frame
date: 2017-10-10
excerpt_separator: <!--more-->
---

<a name="go_to_top"></a>

Table of Contents
=================

   * [Rename a data frame](#rename-a-data-frame)
   * [Convert a list into one COLUMN of a data frame](#convert-a-list-into-one-column-of-a-data-frame)
   * [Convert a list into one ROW of a data frame](#convert-a-list-into-one-row-of-a-data-frame)
   * [Remove column(s) from a data frame](#remove-columns-from-a-data-frame)
   * [Remove rows that contain a specific value from a data frame](#remove-rows-that-contain-a-specific-value-from-a-data-frame)
   * [Remove unused levels from factor](#remove-unused-levels-from-factor)
   * [Replace values of one column by another column](#replace-values-of-one-column-by-another-column)
   * [Remove rows that contain NA value](#remove-rows-that-contain-na-value)
   * [Reorder levels of factor](#reorder-levels-of-factor)
<!--more-->

# Rename a data frame

```{r}
> df <- data.frame("GeneID" = c("g01","g02","g03"),
		"Species" = c("Human","Mouse","Cat"),
		"TaxonID" = c(9606,10090,9685),
		stringsAsFactors=FALSE)
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

# Convert a list into one COLUMN of a data frame
```{r}
> ll <- c("H.sapiens",9606,2996,80000)
> list2col <- data.frame(ll)
> list2col
         ll
1 H.sapiens
2      9606
3      2996
4     80000
```
[Back to Contents](#go_to_top)

# Convert a list into one ROW of a data frame

```{r}
> ll <- c("H.sapiens",9606,2996,80000)
> list2row <- data.frame(matrix(ll, nrow=1, byrow=TRUE))
> colnames(list2row) <- c("Species","TaxonID","Length(MB)","Proteins")
> list2row
    Species TaxonID Length(MB) Proteins
1 H.sapiens    9606   2996    80000
```
[Back to Contents](#go_to_top)

# Remove column(s) from a data frame

```{r}
> df <- data.frame("GeneID" = c("g01","g02","g03"),
		"Species" = c("Human","Mouse","Cat"),
		"TaxonID" = c(9606,10090,9685),
		stringsAsFactors=FALSE)
> subset(df, select = -c(Species))
  GeneID TaxonID
1    g01    9606
2    g02   10090
3    g03    9685
```
or
```{r}
> subset(df, select = c(GeneID,TaxonID))
  GeneID TaxonID
1    g01    9606
2    g02   10090
3    g03    9685
```
[Back to Contents](#go_to_top)

# Remove rows that contain a specific value from a data frame

```{r}
> df[df$GeneID != "g02",]
  GeneID Species TaxonID
1    g01   Human    9606
3    g03     Cat    9685
```
or
```{r}
> df[df$GeneID %in% c("g01","g03"),]
  GeneID Species TaxonID
1    g01   Human    9606
3    g03     Cat    9685
```
or
```{r}
> speciesList <- c("Human","Cat")
> df[match(speciesList, df$Species),]
  GeneID Species TaxonID
1    g01   Human    9606
3    g03     Cat    9685
```
or
```{r}
> subset(df, Species %in% speciesList)
  GeneID Species TaxonID
1    g01   Human    9606
3    g03     Cat    968
```
_**NOTE:**_
In case `GeneID` has type of *factor* instead of *chr* :
```{r}
> df$GeneID <- as.factor(df$GeneID)
> str(df)
'data.frame':	3 obs. of  3 variables:
 $ GeneID : Factor w/ 3 levels "g01","g02","g03": 1 2 3
 $ Species: chr  "Human" "Mouse" "Cat"
 $ TaxonID: num  9606 10090 9685
```
when we remove row of `GeneID == "g02"`
```{r}
> df2 <- df[df$GeneID != "g02",]
> df2
  GeneID Species TaxonID
1    g01   Human    9606
3    g03     Cat    9685
```
`df2` still has 3 levels (g01, g02 and g03) for `GeneID`
```{r}
> str(df2)
'data.frame':	2 obs. of  3 variables:
 $ GeneID : Factor w/ 3 levels "g01","g02","g03": 1 3
 $ Species: chr  "Human" "Cat"
 $ TaxonID: num  9606 9685
```
[Back to Contents](#go_to_top)

# Remove unused levels from factor
```{r}
> df2 <- droplevels(df2)
> str(df2)
'data.frame':	2 obs. of  3 variables:
 $ GeneID : Factor w/ 2 levels "g01","g03": 1 2
 $ Species: chr  "Human" "Cat"
 $ TaxonID: num  9606 9685
```
[Back to Contents](#go_to_top)

# Replace values of one column by another column
```{r}
> df[df$TaxonID < 10000,]$Species <- NA
> df
  GeneID Species TaxonID
1    g01    <NA>    9606
2    g02   Mouse   10090
3    g03    <NA>    9685
```
[Back to Contents](#go_to_top)

# Remove rows that contain NA value
```{r}
> df[complete.cases(df),]
  GeneID Species TaxonID
2    g02   Mouse   10090
```
[Back to Contents](#go_to_top)

# Reorder levels of factor
```{r}
> df$GeneID <- factor(df$GeneID, levels = c("g03","g01","g02"))
> str(df)
'data.frame':	3 obs. of  3 variables:
 $ GeneID : Factor w/ 3 levels "g03","g01","g02": 2 3 1
 $ Species: chr  "Human" "Mouse" "Cat"
 $ TaxonID: num  9606 10090 9685
```
[Back to Contents](#go_to_top)
