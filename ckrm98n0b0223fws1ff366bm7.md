---
title: "Basic R"
datePublished: Fri Oct 23 2015 22:00:00 GMT+0000 (Coordinated Universal Time)
cuid: ckrm98n0b0223fws1ff366bm7
slug: basic-r
tags: r, beginner

---

### CALCULATOR

+, -, \*, /  
Use ^ for raise to a power, function sqrt() for square root, abs() for absolute value.

### VARIABLES

```sql
#assign value to variable:
x<-sqrt(4)

#you can use a variable to build a new variable: 
y<-x+5
```

### VECTORS (1)

Vector = elements of the same type  
List = elements also of different types

```sql
#function c() for vector construction, separator ","  
z<-c(x,y,3.2,5,6)

#you can use a vector to build a new vector:  
c(z,8,12.6,z,y)

#multiply by 2 every element of z vector.
z*2
```

If we have 2 vectors of the same length, R do an operation element by element  
If not, when finish the elements of the shorter one, R restart from the first

```sql
#this:
c(1,2,3,4)*c(3,2)
#is equal to this:
c(1,2,3,4)*c(3,2,3,2)
#both have this result: [3,4,9,8].
```

### LOGICAL VALUES

&lt;, &gt;, &gt;=, &lt;=, ==, !=  
OR = |, AND = &

```sql
TRUE & c(TRUE, FALSE, FALSE)  
#this compare TRUE whith each element
#the result is a vector: TRUE FALSE FALSE

TRUE && c(TRUE, FALSE, FALSE)
#this compare TRUE with only the first element
#the result is a value: TRUE
```

### FUNCTIONS (INTRO)

`functionname(param1, param2, ...)`  
`args(functionname)` to see the arguments of the function

```sql
#Function evaluation, !isTRUE() for the opposite.
isTRUE(4>3)   

xor(TRUE, TRUE)
#require 2 arguments, differs from "|" because returns TRUE
#only if one argument is TRUE and the other FALSE

any(c(-1,2,3)>0)
#TRUE if at least one element is > 0

all(c(-1,2,3)>1)
#TRUE if all are > 1

#paste strings
paste(Name, Surname, sep=" ")

#convert x variable to numeric
as.numeric(x)

#convert x to qualitative variable
as.factor(x)

#current date
Sys.Date()

#vector mean
mean(v)
```

### INTERACTION WITH THE WORLD

```sql
#see working directory
getwd()

#see files in WD
list.files()

#local functions and variables
ls()

#delete variable t
rm(t)

#delete all the local variables and functions
rm(list=ls())

#new folder in WD
dir.create("test")

#create folder (\\ only on Windows)
dir.create("C:\\test")

#change WD
setwd("C:\\test")

#create file
file.create("HelloWorld.R")

#TRUE if the file exists on WD
file.exists("HelloWorld.R")

#file info
file.info ("HelloWorld.R")

#rename file
file.rename("HelloWorld.R","Hello_World.R")

#copy file
file.copy("Hello_World.R","HelloWorld.R")

#delete folder and files in it 
#with recursive TRUE also delete subfolders
unlink("C:\\prova", recursive=TRUE)

#install a package
#CRAN = Comprehensive R Archive Network", contains approved packages
install.packages("dplyr")

#call an installed package
library("dplyr")
```

### NA AND VECTORS (2)

NA = Not Available (like NULL in SQL, or Nothing in VB) NaN = indetermined value (0/0).

```sql
is.na(x)
#vector with TRUE when NA
```

square roots to filter only some data in a vector

```sql
#take only missing/not missing elements
x[is.na(x)]
x[!is.na(x)]

#only positive elements
x[x>0]

#only not missing and positives
x[!is.na(x) & x>0]

#first and third element
x[c(1,3)]

#all except second and fourth element
x[-c(2,4)]

#give names to vector elements
y <- c(name1 = 1, name2 = -7, name3 = NA, name4 = 0)

#you can use these names to filter values
x[c("name", "name3")]

#see elements names
names(y)

#y elements used as names for vector x
names(x) <- y   # Gli elementi di y usati come nome degli elementi di x

identical(x,y)
#TRUE if x equal to y
```

### DATE

Class date, YEAR-MONTH-DAY  
Internally is number of days from 1970-01-01

```sql
#string to date
as.Date("1987-05-26")

#system date
Sys.Date()

#day since 1970-01-01
unclass(VaiabileData)
```

Time classes: POSIXct e POSIXlt POSIXct: seconds from 1970-01-01 POSIXlt: list of values (sec, min, hour, mon, year, ecc.)

```sql
#POSIXct system datetime
Sys.time()   

#seconds since 1970-01-01
unclass(VariabileDateTimePOSIXct)

#POSIXlt datetime
as.POSIXlt(Sys.time())   

#see POSIXlt datetime
unclass(VariabileDateTimePOSIXlt)

#see POSIXlt datetime minute
VariabileDateTimePOSIXlt$min

weekdays(VariabileDateTimePOSIXlt)   # weekday
months(VariabileDateTimePOSIXlt)   # month
quarters(VariabileDateTimePOSIXlt)   # quarter

#strptime to convert a string not YYYY-MM-DD to date
strptime("26 may 1987", "%d %B %Y")

?strptime   #see strptime options

#we can use i.e. >,< for compare dates and - for subtract them
difftime(Date1, Date2, units = 'weeks')   #dates differences in weeks

#lubridate: great package for dates
library(lubridate)
```

### MATRICES AND DATA FRAMES

Matrix: table with only one data types  
Data Frames: also different data types in the same data frame

```sql
m <- matrix(1:20, 4, 5)
#m now is a 4x5 matrix with values from 1 to 20

#transform a matrix to a data frame
tab <- data.frame(v, m) 

#object type
class(tab)   

#give or change columns names
colnames(tab) <- c("a","b","c")   

#first 12 column names
names(tab[1:12])   

#only "col" column
tab$col  

#create Col column
tab$Col <- [vector or function]   

#delete Col column
tab$Col <- NULL   

#rows and columns count
dim(tab)   

#rows count
nrow(tab)   

#columns count
ncol(tab)   

#disk space occupied by tab data frame
object.size(tab)   

#table preview with first 6 rows
head(tab)   

#first 10 rows
head(tab, n=10)   

#last 10 rows
tail(tab, n=10)   

#data frame info
summary(tab)   

#columns info
str(tab)   

#max and min value of col column
range(tab$col)   

#column sums
colSums(tab)   

#rows sums
rowSums(tab)   

#columns means
colMeans(tab)   

#rows means
rowMeans(tab)   

#frequencies table col1 vs col2
fr <- table(tab$a, tab$b)   

#frequencies of col1 on col2
margin.table(fr, 1)   

#frequencies col2 on col1
margin.table(fr, 2)   

#cell percentages
prop.table(fr)   

#row percentages
prop.table(fr, 1)   

#column percentages
prop.table(fr, 2)
```

### CREATE FUNCTIONS

```sql
functionname <- function(arg1, arg2){
           #arguments
           #returned values
 }

#example (vector mean function):
meanf <- function(vec) {
  sum(vec)/length(vec)
 }

#call meanf function:
meanf(c(3,5,7))

#example with optional argument (sum function):
sumf <- function(num, incr = 1){
  num + incr
 }

#call incrementf function:
sumf(20)
sumf(20,16)

#example with n arguments
telegramf <- function(...){
  paste("START",...,"STOP")
}

#call telegramf function:
telegramf("hello","world","!")

#bynary functions can be used like +,-,*,/:
"%pastef%" <- function(left,right){ 
  paste(left,right)
}

#call pastef function:
"hello"%conc%"and"%conc%"goodbye"%conc%"world!"
```