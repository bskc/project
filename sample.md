Dplyr Practice
karthik
December 16, 2016
This is short tutorial on data wrangling with the help of dplyr package which is a part of tidyverse family.
There are basically 5 core 'verbs' that constitute the entire dplyr package and along with the other tools, can pretty much take care of about most of the data manipulation work.
The scope of this document is however not to discuss all of the five 'verbs' but look at one of the 'verbs' select() briefly. The select() is used for making changes column wise.
With the help of select(), variable(s) can be either be retained or dropped,re-ordered and renamed. Each of these will be discussed briefly along with examples.
Firstly load the dplyr package.
library(dplyr)
Now, let's import a data set 'flights' from the working directory
airfly <- tbl_df(read.csv("flights.csv"))
As this is a huge data set, let's create another data set from 'flights' containing 100 obs.
airtraffic <- airfly[sample(nrow(airfly),100),]
Time to explore select()
How to rename a column name?
select(airtraffic,  arrival = arr, departure = dep)
## # A tibble: 100 × 2
##    arrival departure
##      <int>     <int>
## 1     1722      1630
## 2      922       558
## 3       NA        NA
## 4     1341      1246
## 5     2023      1753
## 6     1402      1040
## 7     2002      1654
## 8     1236      1122
## 9     2245      1857
## 10    1252      1205
## # ... with 90 more rows
#Here, arr and dep names have been to changed to 'arrival' and 'departure' respectively
How to drop a column from the dataset
select(airtraffic, -hour,-minute)
## # A tibble: 100 × 12
##                   date   dep   arr dep_delay arr_delay carrier flight
##                 <fctr> <int> <int>     <int>     <int>  <fctr>  <int>
## 1  2011-12-01 12:00:00  1630  1722         0        -8      WN     42
## 2  2011-08-13 12:00:00   558   922        -2        -3      AA    466
## 3  2011-02-03 12:00:00    NA    NA        NA        NA      XE   2944
## 4  2011-08-30 12:00:00  1246  1341        -4        -6      XE   2223
## 5  2011-11-22 12:00:00  1753  2023        83        75      CO   1503
## 6  2011-02-10 12:00:00  1040  1402         0        -7      XE   2446
## 7  2011-06-13 12:00:00  1654  2002        14        22      XE   2728
## 8  2011-06-10 12:00:00  1122  1236        -3        -7      XE   2429
## 9  2011-06-18 12:00:00  1857  2245        -8        -9      CO   1417
## 10 2011-08-04 12:00:00  1205  1252       -10       -19      XE   2407
## # ... with 90 more rows, and 5 more variables: dest <fctr>, plane <fctr>,
## #   cancelled <int>, time <int>, dist <int>
#Here both hour and minute varibles have been dropped from the datset
How to Re-order the column names
select(airtraffic, plane, everything())
## # A tibble: 100 × 14
##     plane                date  hour minute   dep   arr dep_delay arr_delay
##    <fctr>              <fctr> <int>  <int> <int> <int>     <int>     <int>
## 1  N660SW 2011-12-01 12:00:00    16     30  1630  1722         0        -8
## 2  N3GTAA 2011-08-13 12:00:00     5     58   558   922        -2        -3
## 3  N15941 2011-02-03 12:00:00    NA     NA    NA    NA        NA        NA
## 4  N14573 2011-08-30 12:00:00    12     46  1246  1341        -4        -6
## 5  N13248 2011-11-22 12:00:00    17     53  1753  2023        83        75
## 6  N24103 2011-02-10 12:00:00    10     40  1040  1402         0        -7
## 7  N14143 2011-06-13 12:00:00    16     54  1654  2002        14        22
## 8  N14940 2011-06-10 12:00:00    11     22  1122  1236        -3        -7
## 9  N24633 2011-06-18 12:00:00    18     57  1857  2245        -8        -9
## 10 N13929 2011-08-04 12:00:00    12      5  1205  1252       -10       -19
## # ... with 90 more rows, and 6 more variables: carrier <fctr>,
## #   flight <int>, dest <fctr>, cancelled <int>, time <int>, dist <int>
#Here, the plane column is pushed to the first column position in the table
How to select a variable from a dataset
This can be achieved mainly by four ways.
1.	Using contains argument
select(airtraffic, contains("de"))
## # A tibble: 100 × 4
##      dep dep_delay arr_delay   dest
##    <int>     <int>     <int> <fctr>
## 1   1630         0        -8    DAL
## 2    558        -2        -3    MIA
## 3     NA        NA        NA    MEM
## 4   1246        -4        -6    CRP
## 5   1753        83        75    ORD
## 6   1040         0        -7    RDU
## 7   1654        14        22    TYS
## 8   1122        -3        -7    MAF
## 9   1857        -8        -9    PIT
## 10  1205       -10       -19    LFT
## # ... with 90 more rows
#Four varaibles whose names contain 'de' are selected
2.	Using ends_with argument
select(airtraffic, ends_with("dep"))
## # A tibble: 100 × 1
##      dep
##    <int>
## 1   1630
## 2    558
## 3     NA
## 4   1246
## 5   1753
## 6   1040
## 7   1654
## 8   1122
## 9   1857
## 10  1205
## # ... with 90 more rows
#Only one variable, that is dep, ending with 'dep' is selected
3.Using starts_with argument
select(airtraffic, starts_with("fli"))
## # A tibble: 100 × 1
##    flight
##     <int>
## 1      42
## 2     466
## 3    2944
## 4    2223
## 5    1503
## 6    2446
## 7    2728
## 8    2429
## 9    1417
## 10   2407
## # ... with 90 more rows
#Only one variable, that is flight, startng with 'fli' is selected
4.Using Matches argument
select(airtraffic, matches(".o."))
## # A tibble: 100 × 1
##     hour
##    <int>
## 1     16
## 2      5
## 3     NA
## 4     12
## 5     17
## 6     10
## 7     16
## 8     11
## 9     18
## 10    12
## # ... with 90 more rows
#Only one variable, that is hour, that has an 'o' is selected
A quick summary.
You can rename, re-order, drop, select variable(s) from a datset using select() along with the supporting arguments.
