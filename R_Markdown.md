Housing Data - Learning Methods
================
Ryan Monks
20 January 2018

House Prices: Advanced Regression Techniques
============================================

This is a dataset found on Kaggle often used to learn new methods and techniques. The data is split into a test and a training set. This Ames dataset was compiled for use in data science education.

Initial Overview of dataset
---------------------------

There are 80 predictive variables in total and 1460 rows, only factors and integers. There are no numeric variables with decimal places.

``` r
train <- read.csv("C:/Users/Ryan/Desktop/R folder/House Prices/train.csv")
test <- read.csv("C:/Users/Ryan/Desktop/R folder/House Prices/test.csv")
library(ggplot2)
str(train)
```

    ## 'data.frame':    1460 obs. of  81 variables:
    ##  $ Id           : int  1 2 3 4 5 6 7 8 9 10 ...
    ##  $ MSSubClass   : int  60 20 60 70 60 50 20 60 50 190 ...
    ##  $ MSZoning     : Factor w/ 5 levels "C (all)","FV",..: 4 4 4 4 4 4 4 4 5 4 ...
    ##  $ LotFrontage  : int  65 80 68 60 84 85 75 NA 51 50 ...
    ##  $ LotArea      : int  8450 9600 11250 9550 14260 14115 10084 10382 6120 7420 ...
    ##  $ Street       : Factor w/ 2 levels "Grvl","Pave": 2 2 2 2 2 2 2 2 2 2 ...
    ##  $ Alley        : Factor w/ 2 levels "Grvl","Pave": NA NA NA NA NA NA NA NA NA NA ...
    ##  $ LotShape     : Factor w/ 4 levels "IR1","IR2","IR3",..: 4 4 1 1 1 1 4 1 4 4 ...
    ##  $ LandContour  : Factor w/ 4 levels "Bnk","HLS","Low",..: 4 4 4 4 4 4 4 4 4 4 ...
    ##  $ Utilities    : Factor w/ 2 levels "AllPub","NoSeWa": 1 1 1 1 1 1 1 1 1 1 ...
    ##  $ LotConfig    : Factor w/ 5 levels "Corner","CulDSac",..: 5 3 5 1 3 5 5 1 5 1 ...
    ##  $ LandSlope    : Factor w/ 3 levels "Gtl","Mod","Sev": 1 1 1 1 1 1 1 1 1 1 ...
    ##  $ Neighborhood : Factor w/ 25 levels "Blmngtn","Blueste",..: 6 25 6 7 14 12 21 17 18 4 ...
    ##  $ Condition1   : Factor w/ 9 levels "Artery","Feedr",..: 3 2 3 3 3 3 3 5 1 1 ...
    ##  $ Condition2   : Factor w/ 8 levels "Artery","Feedr",..: 3 3 3 3 3 3 3 3 3 1 ...
    ##  $ BldgType     : Factor w/ 5 levels "1Fam","2fmCon",..: 1 1 1 1 1 1 1 1 1 2 ...
    ##  $ HouseStyle   : Factor w/ 8 levels "1.5Fin","1.5Unf",..: 6 3 6 6 6 1 3 6 1 2 ...
    ##  $ OverallQual  : int  7 6 7 7 8 5 8 7 7 5 ...
    ##  $ OverallCond  : int  5 8 5 5 5 5 5 6 5 6 ...
    ##  $ YearBuilt    : int  2003 1976 2001 1915 2000 1993 2004 1973 1931 1939 ...
    ##  $ YearRemodAdd : int  2003 1976 2002 1970 2000 1995 2005 1973 1950 1950 ...
    ##  $ RoofStyle    : Factor w/ 6 levels "Flat","Gable",..: 2 2 2 2 2 2 2 2 2 2 ...
    ##  $ RoofMatl     : Factor w/ 8 levels "ClyTile","CompShg",..: 2 2 2 2 2 2 2 2 2 2 ...
    ##  $ Exterior1st  : Factor w/ 15 levels "AsbShng","AsphShn",..: 13 9 13 14 13 13 13 7 4 9 ...
    ##  $ Exterior2nd  : Factor w/ 16 levels "AsbShng","AsphShn",..: 14 9 14 16 14 14 14 7 16 9 ...
    ##  $ MasVnrType   : Factor w/ 4 levels "BrkCmn","BrkFace",..: 2 3 2 3 2 3 4 4 3 3 ...
    ##  $ MasVnrArea   : int  196 0 162 0 350 0 186 240 0 0 ...
    ##  $ ExterQual    : Factor w/ 4 levels "Ex","Fa","Gd",..: 3 4 3 4 3 4 3 4 4 4 ...
    ##  $ ExterCond    : Factor w/ 5 levels "Ex","Fa","Gd",..: 5 5 5 5 5 5 5 5 5 5 ...
    ##  $ Foundation   : Factor w/ 6 levels "BrkTil","CBlock",..: 3 2 3 1 3 6 3 2 1 1 ...
    ##  $ BsmtQual     : Factor w/ 4 levels "Ex","Fa","Gd",..: 3 3 3 4 3 3 1 3 4 4 ...
    ##  $ BsmtCond     : Factor w/ 4 levels "Fa","Gd","Po",..: 4 4 4 2 4 4 4 4 4 4 ...
    ##  $ BsmtExposure : Factor w/ 4 levels "Av","Gd","Mn",..: 4 2 3 4 1 4 1 3 4 4 ...
    ##  $ BsmtFinType1 : Factor w/ 6 levels "ALQ","BLQ","GLQ",..: 3 1 3 1 3 3 3 1 6 3 ...
    ##  $ BsmtFinSF1   : int  706 978 486 216 655 732 1369 859 0 851 ...
    ##  $ BsmtFinType2 : Factor w/ 6 levels "ALQ","BLQ","GLQ",..: 6 6 6 6 6 6 6 2 6 6 ...
    ##  $ BsmtFinSF2   : int  0 0 0 0 0 0 0 32 0 0 ...
    ##  $ BsmtUnfSF    : int  150 284 434 540 490 64 317 216 952 140 ...
    ##  $ TotalBsmtSF  : int  856 1262 920 756 1145 796 1686 1107 952 991 ...
    ##  $ Heating      : Factor w/ 6 levels "Floor","GasA",..: 2 2 2 2 2 2 2 2 2 2 ...
    ##  $ HeatingQC    : Factor w/ 5 levels "Ex","Fa","Gd",..: 1 1 1 3 1 1 1 1 3 1 ...
    ##  $ CentralAir   : Factor w/ 2 levels "N","Y": 2 2 2 2 2 2 2 2 2 2 ...
    ##  $ Electrical   : Factor w/ 5 levels "FuseA","FuseF",..: 5 5 5 5 5 5 5 5 2 5 ...
    ##  $ X1stFlrSF    : int  856 1262 920 961 1145 796 1694 1107 1022 1077 ...
    ##  $ X2ndFlrSF    : int  854 0 866 756 1053 566 0 983 752 0 ...
    ##  $ LowQualFinSF : int  0 0 0 0 0 0 0 0 0 0 ...
    ##  $ GrLivArea    : int  1710 1262 1786 1717 2198 1362 1694 2090 1774 1077 ...
    ##  $ BsmtFullBath : int  1 0 1 1 1 1 1 1 0 1 ...
    ##  $ BsmtHalfBath : int  0 1 0 0 0 0 0 0 0 0 ...
    ##  $ FullBath     : int  2 2 2 1 2 1 2 2 2 1 ...
    ##  $ HalfBath     : int  1 0 1 0 1 1 0 1 0 0 ...
    ##  $ BedroomAbvGr : int  3 3 3 3 4 1 3 3 2 2 ...
    ##  $ KitchenAbvGr : int  1 1 1 1 1 1 1 1 2 2 ...
    ##  $ KitchenQual  : Factor w/ 4 levels "Ex","Fa","Gd",..: 3 4 3 3 3 4 3 4 4 4 ...
    ##  $ TotRmsAbvGrd : int  8 6 6 7 9 5 7 7 8 5 ...
    ##  $ Functional   : Factor w/ 7 levels "Maj1","Maj2",..: 7 7 7 7 7 7 7 7 3 7 ...
    ##  $ Fireplaces   : int  0 1 1 1 1 0 1 2 2 2 ...
    ##  $ FireplaceQu  : Factor w/ 5 levels "Ex","Fa","Gd",..: NA 5 5 3 5 NA 3 5 5 5 ...
    ##  $ GarageType   : Factor w/ 6 levels "2Types","Attchd",..: 2 2 2 6 2 2 2 2 6 2 ...
    ##  $ GarageYrBlt  : int  2003 1976 2001 1998 2000 1993 2004 1973 1931 1939 ...
    ##  $ GarageFinish : Factor w/ 3 levels "Fin","RFn","Unf": 2 2 2 3 2 3 2 2 3 2 ...
    ##  $ GarageCars   : int  2 2 2 3 3 2 2 2 2 1 ...
    ##  $ GarageArea   : int  548 460 608 642 836 480 636 484 468 205 ...
    ##  $ GarageQual   : Factor w/ 5 levels "Ex","Fa","Gd",..: 5 5 5 5 5 5 5 5 2 3 ...
    ##  $ GarageCond   : Factor w/ 5 levels "Ex","Fa","Gd",..: 5 5 5 5 5 5 5 5 5 5 ...
    ##  $ PavedDrive   : Factor w/ 3 levels "N","P","Y": 3 3 3 3 3 3 3 3 3 3 ...
    ##  $ WoodDeckSF   : int  0 298 0 0 192 40 255 235 90 0 ...
    ##  $ OpenPorchSF  : int  61 0 42 35 84 30 57 204 0 4 ...
    ##  $ EnclosedPorch: int  0 0 0 272 0 0 0 228 205 0 ...
    ##  $ X3SsnPorch   : int  0 0 0 0 0 320 0 0 0 0 ...
    ##  $ ScreenPorch  : int  0 0 0 0 0 0 0 0 0 0 ...
    ##  $ PoolArea     : int  0 0 0 0 0 0 0 0 0 0 ...
    ##  $ PoolQC       : Factor w/ 3 levels "Ex","Fa","Gd": NA NA NA NA NA NA NA NA NA NA ...
    ##  $ Fence        : Factor w/ 4 levels "GdPrv","GdWo",..: NA NA NA NA NA 3 NA NA NA NA ...
    ##  $ MiscFeature  : Factor w/ 4 levels "Gar2","Othr",..: NA NA NA NA NA 3 NA 3 NA NA ...
    ##  $ MiscVal      : int  0 0 0 0 0 700 0 350 0 0 ...
    ##  $ MoSold       : int  2 5 9 2 12 10 8 11 4 1 ...
    ##  $ YrSold       : int  2008 2007 2008 2006 2008 2009 2007 2009 2008 2008 ...
    ##  $ SaleType     : Factor w/ 9 levels "COD","Con","ConLD",..: 9 9 9 9 9 9 9 9 9 9 ...
    ##  $ SaleCondition: Factor w/ 6 levels "Abnorml","AdjLand",..: 5 5 5 1 5 5 5 5 1 5 ...
    ##  $ SalePrice    : int  208500 181500 223500 140000 250000 143000 307000 200000 129900 118000 ...

There are too many columns to review plots inidividually.

Missing Data
------------

Understanding how many missing values is essential for prediction and for understanding the columns. Let's have a look at the number of missing values.

``` r
colSums(sapply(train,is.na))
```

    ##            Id    MSSubClass      MSZoning   LotFrontage       LotArea 
    ##             0             0             0           259             0 
    ##        Street         Alley      LotShape   LandContour     Utilities 
    ##             0          1369             0             0             0 
    ##     LotConfig     LandSlope  Neighborhood    Condition1    Condition2 
    ##             0             0             0             0             0 
    ##      BldgType    HouseStyle   OverallQual   OverallCond     YearBuilt 
    ##             0             0             0             0             0 
    ##  YearRemodAdd     RoofStyle      RoofMatl   Exterior1st   Exterior2nd 
    ##             0             0             0             0             0 
    ##    MasVnrType    MasVnrArea     ExterQual     ExterCond    Foundation 
    ##             8             8             0             0             0 
    ##      BsmtQual      BsmtCond  BsmtExposure  BsmtFinType1    BsmtFinSF1 
    ##            37            37            38            37             0 
    ##  BsmtFinType2    BsmtFinSF2     BsmtUnfSF   TotalBsmtSF       Heating 
    ##            38             0             0             0             0 
    ##     HeatingQC    CentralAir    Electrical     X1stFlrSF     X2ndFlrSF 
    ##             0             0             1             0             0 
    ##  LowQualFinSF     GrLivArea  BsmtFullBath  BsmtHalfBath      FullBath 
    ##             0             0             0             0             0 
    ##      HalfBath  BedroomAbvGr  KitchenAbvGr   KitchenQual  TotRmsAbvGrd 
    ##             0             0             0             0             0 
    ##    Functional    Fireplaces   FireplaceQu    GarageType   GarageYrBlt 
    ##             0             0           690            81            81 
    ##  GarageFinish    GarageCars    GarageArea    GarageQual    GarageCond 
    ##            81             0             0            81            81 
    ##    PavedDrive    WoodDeckSF   OpenPorchSF EnclosedPorch    X3SsnPorch 
    ##             0             0             0             0             0 
    ##   ScreenPorch      PoolArea        PoolQC         Fence   MiscFeature 
    ##             0             0          1453          1179          1406 
    ##       MiscVal        MoSold        YrSold      SaleType SaleCondition 
    ##             0             0             0             0             0 
    ##     SalePrice 
    ##             0

There are a significant number of missing values for:

*PoolQC *Fence *MiscFeature *GarageQual *GarageCond *Alley *LotFrontage *FireplaceQu

Summary can show us the number of NAS

``` r
summary(train$PoolQC)
```

    ##   Ex   Fa   Gd NA's 
    ##    2    2    3 1453

``` r
summary(train$Fence)
```

    ## GdPrv  GdWo MnPrv  MnWw  NA's 
    ##    59    54   157    11  1179

However, one Kaggle guide has shown a very good method of showing NAs: <https://www.kaggle.com/notaapple/detailed-exploratory-data-analysis-using-r>

``` r
plot_Missing <- function(data_in, title = NULL){
  temp_df <- as.data.frame(ifelse(is.na(data_in), 0, 1))
  temp_df <- temp_df[,order(colSums(temp_df))]
  data_temp <- expand.grid(list(x = 1:nrow(temp_df), y = colnames(temp_df)))
  data_temp$m <- as.vector(as.matrix(temp_df))
  data_temp <- data.frame(x = unlist(data_temp$x), y = unlist(data_temp$y), m = unlist(data_temp$m))
  ggplot(data_temp) + geom_tile(aes(x=x, y=y, fill=factor(m))) + scale_fill_manual(values=c("white", "black"), name="Missing\n(0=Yes, 1=No)") + theme_light() + ylab("") + xlab("") + ggtitle(title)
}
plot_Missing(train[,colSums(is.na(train)) > 0])
```

![](R_Markdown_files/figure-markdown_github-ascii_identifiers/unnamed-chunk-4-1.png)

For some reason, the with= parameter is not needed anymore that is in the link. This code is creating a function plot\_Missing that has parameter of the dataset, in our case those that are not NA.

Observations: \* A large proportion of houses miss alleys, pools, fences, fireplaces and elevators. This is consistent with what one may expect. \* The fireplace and fireplacequ variables could be combined into one.

Cleaning Data
-------------

One good idea is to convert some character variables to numeric variables to allow for quicker assignment for the model. Initially I started by assigning categorical levels with a high frequency in one bucket into binary indicators.

``` r
train$paved[train$Street == "Pave"] <-1 
train$paved[train$Street != "Pave"] <-0
table(train$Street)
```

    ## 
    ## Grvl Pave 
    ##    6 1454

``` r
table(train$LotShape)
```

    ## 
    ## IR1 IR2 IR3 Reg 
    ## 484  41  10 925

``` r
train$regshape[train$LotShape == "Reg"] <-1
train$regshape[train$LotShape != "Reg"] <-0
table(train$LotShape)
```

    ## 
    ## IR1 IR2 IR3 Reg 
    ## 484  41  10 925

``` r
train$regshape[train$LandContour == "Reg"] <-1
train$regshape[train$LandContour != "Reg"] <-0
table(train$LandContour)
```

    ## 
    ##  Bnk  HLS  Low  Lvl 
    ##   63   50   36 1311

Upon looking at Garagetype as a variable to consider, it became apparent that some are more indicative of value than others as shown below. Therefore it's important to use this information.

    ## 
    ## Attaching package: 'dplyr'

    ## The following objects are masked from 'package:stats':
    ## 
    ##     filter, lag

    ## The following objects are masked from 'package:base':
    ## 
    ##     intersect, setdiff, setequal, union

| GarageType |  mean(SalePrice, na.rm = T)|
|:-----------|---------------------------:|
| 2Types     |                    151283.3|
| Attchd     |                    202892.7|
| Basment    |                    160570.7|
| BuiltIn    |                    254751.7|
| CarPort    |                    109962.1|
| Detchd     |                    134091.2|
| NA         |                    103317.3|

The above table shows that BuiltIn and Attached garages correlate to a significantly higher sale value than the other components. Therefore it's appropriate to combine these into one.

``` r
train$GarageType1[train$GarageType %in% c("Attchd","BuiltIn")] <- 1
train$GarageType1[!train$GarageType %in% c("Attchd","BuiltIn")] <- 0
```

Repeating this for GarageFinish and GarageQual, noting that garage quality had a significant deviation between each various level.

``` r
train$garagequal[train$GarageQual =="Ex"] <-1
train$garagequal[train$GarageQual =="Fa"] <-2
train$garagequal[train$GarageQual =="Gd"] <-3
train$garagequal[train$GarageQual =="TA"] <-4
train$garagequal[train$GarageQual == "Po" | is.na(train$GarageQual)] <- 5

train$garagefinish[train$GarageFinish %in% c("Fin", "RFn")] <- 1
train$garagefinish[!train$GarageFinish %in% c("Fin","RFn")] <- 0
```

Correlations
------------

``` r
library(corrplot)
```

    ## corrplot 0.84 loaded

``` r
correlations<-cor(train[,c("SalePrice","OverallQual","OverallCond")])
corrplot(correlations, method="circle", type="lower",  sig.level = 0.01, insig = "blank")
```

![](R_Markdown_files/figure-markdown_github-ascii_identifiers/unnamed-chunk-9-1.png)

Plotting data
-------------

TBC

Estimation methods
------------------

R Tools learnt along the way:
-----------------------------

### Subsetting

This is the action of subsetting vectors for the desired entries. There are apparantely 5 main ways of doing so:

The method used was using the letters vector

``` r
letters
```

    ##  [1] "a" "b" "c" "d" "e" "f" "g" "h" "i" "j" "k" "l" "m" "n" "o" "p" "q"
    ## [18] "r" "s" "t" "u" "v" "w" "x" "y" "z"

#### Method 1 & 2 By position and by exclusion

The simplest is to identify by the relative position in the vector

``` r
letters[c(1,13)]
```

    ## [1] "a" "m"

Note we have to use a column vector as simply writing 1, 13 doesn't make any sense.

``` r
letters[-c(1,13)]
```

    ##  [1] "b" "c" "d" "e" "f" "g" "h" "i" "j" "k" "l" "n" "o" "p" "q" "r" "s"
    ## [18] "t" "u" "v" "w" "x" "y" "z"

Negative numbers mean extract all but those figures. Note that you cannot mix positive and negative.

Pros: Easy

Cons: Must know position of the entry wanted

#### Method 3 - By Name

``` r
ip <- c("Ryan"=1,"Monks"=3,"Edward"=5)
ip[c("Ryan","Edward")]
```

    ##   Ryan Edward 
    ##      1      5

#### Method 4 - By logicals

Like the names example, this one is used if the position isn't known but we sort of know what we are looking for.

This method uses the \[ \] operator and returns any subset of the vector containing the elements corresponding to TRUE values in our logical 'indexer'.

``` r
x = c("a", "b", "c", "b")
x[c(TRUE, FALSE, TRUE, FALSE)]
```

    ## [1] "a" "c"

This shows that it only pulls out elements 1 and 3 because they are considered true by the indexer.

``` r
x = c("a", "b", "c", "b")
x[x=="b"]
```

    ## [1] "b" "b"

Or we could find all the positions of the vector

``` r
x = c("a", "b", "c", "b")
x[(1:length(x))[x=="b"]] 
```

    ## [1] "b" "b"

What this does is produces a vector of true false for x=="b" and then that is applied to the vector (1:length(x)). This produces the same results but in a different method, albeit.

Here's another more technical example:

``` r
x = rnorm(100, mean=4, sd=1)
x[x<1 & x>6] 
```

    ## numeric(0)

We can also use logicals to include or exclude values, using !. This inverts the logical vector.

``` r
x = c("ryan", "edward", "monks")
x[!(x=="edward")] 
```

    ## [1] "ryan"  "monks"

#### Method 5 - The degenerate case

This is simply doing x\[ \] which returns the vector itself. This is useful if we wish to re assign the values of every entry of the vector.

URL found here: <https://www.stat.berkeley.edu/~nolan/stat133/Fall05/notes/RSubset.html>
