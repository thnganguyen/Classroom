Class8
================

``` r
knitr::opts_chunk$set(echo = TRUE, warning = FALSE, message = FALSE)
library(tidyverse)
```

    ## ── Attaching packages ────────────────────────────────────────────────────────────────── tidyverse 1.2.1 ──

    ## ✔ ggplot2 3.0.0     ✔ purrr   0.2.5
    ## ✔ tibble  1.4.2     ✔ dplyr   0.7.6
    ## ✔ tidyr   0.8.1     ✔ stringr 1.3.1
    ## ✔ readr   1.1.1     ✔ forcats 0.3.0

    ## ── Conflicts ───────────────────────────────────────────────────────────────────── tidyverse_conflicts() ──
    ## ✖ dplyr::filter() masks stats::filter()
    ## ✖ dplyr::lag()    masks stats::lag()

``` r
library(stringr)
```

A csv-reader
============

Don't get the same dimension with using read\_csv

``` r
tab <- readLines(con <- file("Class_files/mot-2014-2017.csv", encoding = "UTF-8"))
close(con)
my_tab <- tab %>% 
  str_split(pattern = fixed(","), simplify = TRUE)

dim(my_tab)
```

    ## [1] 14387    22

``` r
my_tab[1, ]
```

    ##  [1] "\"3056965\""                                                                             
    ##  [2] "\"H2021\""                                                                               
    ##  [3] "\"2014/15\""                                                                             
    ##  [4] "\"1\""                                                                                   
    ##  [5] "\"mot\""                                                                                 
    ##  [6] "\"Enskild motion\""                                                                      
    ##  [7] "\"mot\""                                                                                 
    ##  [8] "\"C300\""                                                                                
    ##  [9] "\"NU\""                                                                                  
    ## [10] "\"\""                                                                                    
    ## [11] "\"1\""                                                                                   
    ## [12] "\"2014-09-29 00:00:00\""                                                                 
    ## [13] "\"2018-06-04 09:09:07\""                                                                 
    ## [14] "\"Utveckla en nationell strategi för tillväxt och grundläggande service i hela landet \""
    ## [15] "\"av Anders Ahlgren och Anders Åkesson (C)\""                                            
    ## [16] "\"Klar\""                                                                                
    ## [17] "\"\""                                                                                    
    ## [18] ""                                                                                        
    ## [19] ""                                                                                        
    ## [20] ""                                                                                        
    ## [21] ""                                                                                        
    ## [22] ""

``` r
tab1 <- read_csv("Class_files/mot-2014-2017.csv")
glimpse(tab1)
```

    ## Observations: 14,386
    ## Variables: 17
    ## $ `3056965`                                                                             <int> ...
    ## $ H2021                                                                                 <chr> ...
    ## $ `2014/15`                                                                             <chr> ...
    ## $ `1`                                                                                   <int> ...
    ## $ mot                                                                                   <chr> ...
    ## $ `Enskild motion`                                                                      <chr> ...
    ## $ mot_1                                                                                 <chr> ...
    ## $ C300                                                                                  <chr> ...
    ## $ NU                                                                                    <chr> ...
    ## $ X10                                                                                   <chr> ...
    ## $ `1_1`                                                                                 <int> ...
    ## $ `2014-09-29 00:00:00`                                                                 <dttm> ...
    ## $ `2018-06-04 09:09:07`                                                                 <dttm> ...
    ## $ `Utveckla en nationell strategi för tillväxt och grundläggande service i hela landet` <chr> ...
    ## $ `av Anders Ahlgren och Anders Åkesson (C)`                                            <chr> ...
    ## $ Klar                                                                                  <chr> ...
    ## $ X17                                                                                   <chr> ...

HTML tables
===========

Extract table into vector using regex

``` r
table <- "<table>
  <tr>
<th>Förnamn</th>
<th>Efternamn</th> 
<th>Ålder</th>
</tr>
<tr>
<td>Kalle</td>
<td>Karlsson</td> 
<td>25</td>
</tr>
<tr>
<td>Lisa</td>
<td>Larsson</td> 
<td>17</td>
</tr>
</table>"

pattern1 <- regex("\\\n?\\<.*?\\>\\\n?", multiline = TRUE, dotall = TRUE)
table %>%
  str_replace_all(pattern1, " ") %>%
  str_squish() %>%
  str_split(" ", simplify = TRUE)
```

    ##      [,1]      [,2]        [,3]    [,4]    [,5]       [,6] [,7]  
    ## [1,] "Förnamn" "Efternamn" "Ålder" "Kalle" "Karlsson" "25" "Lisa"
    ##      [,8]      [,9]
    ## [1,] "Larsson" "17"

``` r
pattern <- regex("\\>[\\w\\d]+\\<", multiline = TRUE)
table %>%
  str_extract_all(pattern, simplify = TRUE) %>%
   str_remove_all("[><]")
```

    ## [1] "Förnamn"   "Efternamn" "Ålder"     "Kalle"     "Karlsson"  "25"       
    ## [7] "Lisa"      "Larsson"   "17"

Motions of the Riksdag
======================

``` r
motions <- read_csv("Class_files/mot-2014-2017.csv", 
                    col_names = c("hangar_id", "dok_id", "rm", "beteckning", 
                                  "typ", "subtyp", "doktyp", "dokumentnamn",  "organ", 
                                  "mottagare", "nummer", "datum", "systemdatum", 
                                  "titel", "subtitel", "status"))
glimpse(motions)
```

    ## Observations: 14,387
    ## Variables: 16
    ## $ hangar_id    <int> 3056965, 3058019, 3056966, 3110999, 3111000, 3111...
    ## $ dok_id       <chr> "H2021", "H20210", "H202100", "H2021000", "H20210...
    ## $ rm           <chr> "2014/15", "2014/15", "2014/15", "2014/15", "2014...
    ## $ beteckning   <int> 1, 10, 100, 1000, 1001, 1002, 1003, 1004, 1005, 1...
    ## $ typ          <chr> "mot", "mot", "mot", "mot", "mot", "mot", "mot", ...
    ## $ subtyp       <chr> "Enskild motion", "Enskild motion", "Enskild moti...
    ## $ doktyp       <chr> "mot", "mot", "mot", "mot", "mot", "mot", "mot", ...
    ## $ dokumentnamn <chr> "C300", "C303", "M1273", "C414", "C416", "C473", ...
    ## $ organ        <chr> "NU", "UbU", "TU", "MJU", "UbU", "TU", "SoU", "TU...
    ## $ mottagare    <chr> NA, NA, NA, NA, NA, NA, NA, NA, NA, NA, NA, NA, N...
    ## $ nummer       <int> 1, 10, 100, 1000, 1001, 1002, 1003, 1004, 1005, 1...
    ## $ datum        <dttm> 2014-09-29, 2014-10-02, 2014-10-29, 2014-11-06, ...
    ## $ systemdatum  <dttm> 2018-06-04 09:09:07, 2018-06-07 14:05:04, 2016-0...
    ## $ titel        <chr> "Utveckla en nationell strategi för tillväxt och ...
    ## $ subtitel     <chr> "av Anders Ahlgren och Anders Åkesson (C)", "av K...
    ## $ status       <chr> "Klar", "Klar", "Klar", "Klar", "Klar", "Klar", "...
