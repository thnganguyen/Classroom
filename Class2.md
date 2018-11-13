Class2
================

``` r
library(tidyverse)
```

    ## ── Attaching packages ───────────────────────────────────────────────────── tidyverse 1.2.1 ──

    ## ✔ ggplot2 3.0.0     ✔ purrr   0.2.5
    ## ✔ tibble  1.4.2     ✔ dplyr   0.7.6
    ## ✔ tidyr   0.8.1     ✔ stringr 1.3.1
    ## ✔ readr   1.1.1     ✔ forcats 0.3.0

    ## ── Conflicts ──────────────────────────────────────────────────────── tidyverse_conflicts() ──
    ## ✖ dplyr::filter() masks stats::filter()
    ## ✖ dplyr::lag()    masks stats::lag()

``` r
#library(tidyr)
library(ggplot2)
library(chron)
library(knitr)
```

``` r
source("Class_files/SR_music.R")
```

    ## 
    ## Attaching package: 'jsonlite'

    ## The following object is masked from 'package:purrr':
    ## 
    ##     flatten

``` r
music_day <- get_SR_music(channel = 164, date = "2017-12-31") %>%
  select(title, artist, start_time, stop_time)

days <- seq(as.Date("2018-01-01"), as.Date("2018-01-07"), "days")

music_days <- map_df(days, get_SR_music, channel = 164) %>%
  select(title, artist, start_time, stop_time)

by_title_day <- music_day %>%
  group_by(title) %>%
  summarise(numberViews = n()) %>%
  select(title, numberViews) %>%
  arrange(desc(numberViews))

mostViews <- by_title_day %>% slice(1:5) %>% select(title)
mostViews
```

    ## # A tibble: 5 x 1
    ##   title              
    ##   <chr>              
    ## 1 Beautiful          
    ## 2 Pari               
    ## 3 Too Good To Be True
    ## 4 All Falls Down     
    ## 5 Despacito (Remix)

``` r
by_title_days <- music_days %>%
  group_by(title) %>%
  summarise(numberViews = n()) %>%
  select(title, numberViews) %>%
  arrange(desc(numberViews))

mostViews <- by_title_days %>% slice(1:5) %>% select(title)
mostViews
```

    ## # A tibble: 5 x 1
    ##   title         
    ##   <chr>         
    ## 1 Beautiful     
    ## 2 Dreamer       
    ## 3 Getaway Car   
    ## 4 Only Human    
    ## 5 All Falls Down

``` r
by_artist <- music_days %>%
  group_by(artist) %>%
  summarise(numberSongs = n()) %>%
  select(artist, numberSongs) %>%
  arrange(desc(numberSongs))
mostSongs <- by_artist %>% slice(1) %>% select(artist)
mostSongs
```

    ## # A tibble: 1 x 1
    ##   artist      
    ##   <chr>       
    ## 1 Taylor Swift

``` r
music_days <- music_days %>%
  mutate(duration = stop_time-start_time)

ggplot(music_days, aes(x = duration)) + geom_histogram(bins = 100) + ggtitle("The distribution of song durations")
```

    ## Don't know how to automatically pick scale for object of type difftime. Defaulting to continuous.

![](Class2_files/figure-markdown_github/unnamed-chunk-1-1.png)

``` r
music_days <- music_days %>%
  mutate(time = as.numeric(chron::times(as.POSIXct(start_time) %>% format("%H:%M:%S"))))

by_date <- music_days %>%
  select(time)

ggplot(by_date, aes(x = time)) + geom_histogram() + 
  ggtitle("Distribution of start_time over the day")
```

    ## `stat_bin()` using `bins = 30`. Pick better value with `binwidth`.

![](Class2_files/figure-markdown_github/unnamed-chunk-1-2.png)

``` r
music_days <- music_days %>%
  mutate(date = as.POSIXct(start_time) %>% format("%Y-%m-%d")) %>%
  select(date, duration)
music_days_hour <- music_days %>%
  group_by(date) %>%
  mutate(nHour = sum(as.numeric(duration))/3600)

ggplot(music_days_hour, aes(x = date, y = nHour)) + geom_point() + coord_flip()
```

![](Class2_files/figure-markdown_github/unnamed-chunk-1-3.png)

``` r
claim_data <- read.csv("Class_files/claims.csv")

claim_by_year <- claim_data %>%
  mutate(year = as.numeric(as.POSIXct(Claim.date) %>% format("%Y")))

ggplot(claim_by_year, aes(x = year)) + geom_bar()
```

![](Class2_files/figure-markdown_github/Insurance%20claims%20from%20kammarkollegiet-1.png)

``` r
mytab <- claim_by_year %>% 
  filter(year>=2010) %>%
  mutate(closing.year = as.numeric(as.POSIXct(Closing.date) %>% format("%Y"))) %>%
  mutate(duration = (closing.year - year)) %>%
  group_by(duration, year) %>%
  summarize(loss = sum(Payment)) %>%
  filter(!is.na(duration)) %>%
  spread(duration, loss)

kable(mytab, format = "html")
```

<table>
<thead>
<tr>
<th style="text-align:right;">
year
</th>
<th style="text-align:right;">
0
</th>
<th style="text-align:right;">
1
</th>
<th style="text-align:right;">
2
</th>
<th style="text-align:right;">
3
</th>
<th style="text-align:right;">
4
</th>
<th style="text-align:right;">
5
</th>
<th style="text-align:right;">
6
</th>
</tr>
</thead>
<tbody>
<tr>
<td style="text-align:right;">
2010
</td>
<td style="text-align:right;">
510018.0
</td>
<td style="text-align:right;">
3088112
</td>
<td style="text-align:right;">
4647158
</td>
<td style="text-align:right;">
1822604
</td>
<td style="text-align:right;">
2137421
</td>
<td style="text-align:right;">
758912.5
</td>
<td style="text-align:right;">
821161
</td>
</tr>
<tr>
<td style="text-align:right;">
2011
</td>
<td style="text-align:right;">
551435.2
</td>
<td style="text-align:right;">
2058350
</td>
<td style="text-align:right;">
2400339
</td>
<td style="text-align:right;">
1667244
</td>
<td style="text-align:right;">
1247299
</td>
<td style="text-align:right;">
814415.0
</td>
<td style="text-align:right;">
11900
</td>
</tr>
<tr>
<td style="text-align:right;">
2012
</td>
<td style="text-align:right;">
234650.0
</td>
<td style="text-align:right;">
1699957
</td>
<td style="text-align:right;">
4118429
</td>
<td style="text-align:right;">
2362584
</td>
<td style="text-align:right;">
1290159
</td>
<td style="text-align:right;">
NA
</td>
<td style="text-align:right;">
NA
</td>
</tr>
<tr>
<td style="text-align:right;">
2013
</td>
<td style="text-align:right;">
440172.0
</td>
<td style="text-align:right;">
2090456
</td>
<td style="text-align:right;">
2527246
</td>
<td style="text-align:right;">
2243057
</td>
<td style="text-align:right;">
548920
</td>
<td style="text-align:right;">
NA
</td>
<td style="text-align:right;">
NA
</td>
</tr>
<tr>
<td style="text-align:right;">
2014
</td>
<td style="text-align:right;">
294981.0
</td>
<td style="text-align:right;">
2150932
</td>
<td style="text-align:right;">
4212814
</td>
<td style="text-align:right;">
299683
</td>
<td style="text-align:right;">
NA
</td>
<td style="text-align:right;">
NA
</td>
<td style="text-align:right;">
NA
</td>
</tr>
<tr>
<td style="text-align:right;">
2015
</td>
<td style="text-align:right;">
407139.0
</td>
<td style="text-align:right;">
3313439
</td>
<td style="text-align:right;">
468328
</td>
<td style="text-align:right;">
NA
</td>
<td style="text-align:right;">
NA
</td>
<td style="text-align:right;">
NA
</td>
<td style="text-align:right;">
NA
</td>
</tr>
<tr>
<td style="text-align:right;">
2016
</td>
<td style="text-align:right;">
437061.0
</td>
<td style="text-align:right;">
31259
</td>
<td style="text-align:right;">
NA
</td>
<td style="text-align:right;">
NA
</td>
<td style="text-align:right;">
NA
</td>
<td style="text-align:right;">
NA
</td>
<td style="text-align:right;">
NA
</td>
</tr>
</tbody>
</table>
``` r
options(knitr.kable.NA = '')
```

``` r
parties_2018 <- read_csv2("https://data.val.se/val/val2018/valsedlar/partier/deltagande_partier.skv", locale = locale("sv", encoding = "ISO-8859-1"))
```

    ## Using ',' as decimal and '.' as grouping mark. Use read_delim() for more control.

    ## Parsed with column specification:
    ## cols(
    ##   VALTYP = col_character(),
    ##   VALOMRÅDESKOD = col_character(),
    ##   VALOMRÅDESNAMN = col_character(),
    ##   VALKRETSKOD = col_integer(),
    ##   VALKRETSNAMN = col_character(),
    ##   LÄNSKOD = col_character(),
    ##   LÄNSNAMN = col_character(),
    ##   PARTIBETECKNING = col_character(),
    ##   PARTIFÖRKORTNING = col_character(),
    ##   PARTIKOD = col_integer(),
    ##   ANMÄLNINGSDATUM = col_date(format = ""),
    ##   REGISTRERINGSDATUM = col_date(format = ""),
    ##   DIARIENUMMER = col_character(),
    ##   REGISTRERADPARTIBETECKNING = col_character(),
    ##   ANMÄLDAKANDIDATER = col_character(),
    ##   SYMBOL = col_character(),
    ##   DELTAGANDEGRUND = col_character()
    ## )

``` r
parties_2018 %>%
  group_by(VALTYP) %>%
  summarize(n = n_distinct(PARTIKOD))
```

    ## # A tibble: 3 x 2
    ##   VALTYP     n
    ##   <chr>  <int>
    ## 1 K        299
    ## 2 L        102
    ## 3 R         79

``` r
parties_2018 %>%
  filter(VALTYP == "K") %>%
  group_by(VALKRETSKOD) %>%
  summarize(n = n_distinct(PARTIKOD))
```

    ## # A tibble: 7 x 2
    ##   VALKRETSKOD     n
    ##         <int> <int>
    ## 1           0   281
    ## 2           1   102
    ## 3           2   102
    ## 4           3    87
    ## 5           4    81
    ## 6           5    81
    ## 7           6    81
