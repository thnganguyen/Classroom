Class12
================

Thi Thuy Nga Nguyen

Solve exercises in R4DS chapter 21.5.3
======================================

Write code that uses one of the map functions to: Compute the mean of every column in mtcars.

``` r
map_dbl(mtcars, mean, na.rm = TRUE)
```

    ##        mpg        cyl       disp         hp       drat         wt 
    ##  20.090625   6.187500 230.721875 146.687500   3.596563   3.217250 
    ##       qsec         vs         am       gear       carb 
    ##  17.848750   0.437500   0.406250   3.687500   2.812500

Determine the type of each column in nycflights13::flights.

``` r
map_chr(nycflights13::flights, typeof)
```

    ##           year          month            day       dep_time sched_dep_time 
    ##      "integer"      "integer"      "integer"      "integer"      "integer" 
    ##      dep_delay       arr_time sched_arr_time      arr_delay        carrier 
    ##       "double"      "integer"      "integer"       "double"    "character" 
    ##         flight        tailnum         origin           dest       air_time 
    ##      "integer"    "character"    "character"    "character"       "double" 
    ##       distance           hour         minute      time_hour 
    ##       "double"       "double"       "double"       "double"

Compute the number of unique values in each column of iris.

``` r
iris %>%
  map(unique) %>%
  map_dbl(length)
```

    ## Sepal.Length  Sepal.Width Petal.Length  Petal.Width      Species 
    ##           35           23           43           22            3

Generate 10 random normals for each of *μ* = -10, 0, 10, 100

``` r
map(c(-10, 0, 10, 100), rnorm, n = 10)
```

    ## [[1]]
    ##  [1]  -9.102920  -8.944756  -8.747600  -8.044963  -8.565208  -8.032406
    ##  [7]  -9.445063 -10.218025  -7.829587 -11.784354
    ## 
    ## [[2]]
    ##  [1] -0.6288673  1.0512909 -0.9952339  2.0742747 -0.2241261  0.5861892
    ##  [7] -0.9238444  1.4445666  1.0490304 -0.1174851
    ## 
    ## [[3]]
    ##  [1]  9.302675  9.208012  8.991314  9.505799  8.931993 10.033765  9.003329
    ##  [8]  8.976184  9.154870 11.700636
    ## 
    ## [[4]]
    ##  [1]  99.34406 100.10441  99.81581  99.60798 100.31659  98.52780 101.04188
    ##  [8] 101.00043 100.04817  99.41434

How can you create a single vector that for each column in a data frame indicates whether or not it’s a factor?

``` r
map_lgl(iris, is.factor)
```

    ## Sepal.Length  Sepal.Width Petal.Length  Petal.Width      Species 
    ##        FALSE        FALSE        FALSE        FALSE         TRUE

``` r
map(1:5, runif)  # 1:5 -> n
```

    ## [[1]]
    ## [1] 0.8113796
    ## 
    ## [[2]]
    ## [1] 0.3220949 0.5264867
    ## 
    ## [[3]]
    ## [1] 0.3691964 0.4738856 0.0579287
    ## 
    ## [[4]]
    ## [1] 0.2844632 0.3005725 0.6613240 0.9840141
    ## 
    ## [[5]]
    ## [1] 0.4257593 0.9198061 0.4960224 0.8381823 0.4095948

``` r
map(-2:2, rnorm, n = 5)  # -2:2 -> mean
```

    ## [[1]]
    ## [1]  0.07573318 -2.35821325 -2.60850587 -1.20737828 -1.67495918
    ## 
    ## [[2]]
    ## [1] -1.0159602  0.2207095 -0.9831017 -1.0701597 -0.9115124
    ## 
    ## [[3]]
    ## [1] -0.66461397 -0.08503846 -0.17945283 -1.13547281  0.67767805
    ## 
    ## [[4]]
    ## [1] 0.752355 2.282006 1.258837 2.378956 1.950754
    ## 
    ## [[5]]
    ## [1] 3.081156 3.196589 2.398858 2.257814 3.703279

Eliminate the anonymous function

``` r
mtcars %>% 
  split(.$cyl) %>% 
  map(function(df) lm(mpg ~ wt, data = df))
```

    ## $`4`
    ## 
    ## Call:
    ## lm(formula = mpg ~ wt, data = df)
    ## 
    ## Coefficients:
    ## (Intercept)           wt  
    ##      39.571       -5.647  
    ## 
    ## 
    ## $`6`
    ## 
    ## Call:
    ## lm(formula = mpg ~ wt, data = df)
    ## 
    ## Coefficients:
    ## (Intercept)           wt  
    ##       28.41        -2.78  
    ## 
    ## 
    ## $`8`
    ## 
    ## Call:
    ## lm(formula = mpg ~ wt, data = df)
    ## 
    ## Coefficients:
    ## (Intercept)           wt  
    ##      23.868       -2.192

``` r
mtcars %>% 
  split(.$cyl) %>%
  map(~lm(mpg ~ wt, data = .))
```

    ## $`4`
    ## 
    ## Call:
    ## lm(formula = mpg ~ wt, data = .)
    ## 
    ## Coefficients:
    ## (Intercept)           wt  
    ##      39.571       -5.647  
    ## 
    ## 
    ## $`6`
    ## 
    ## Call:
    ## lm(formula = mpg ~ wt, data = .)
    ## 
    ## Coefficients:
    ## (Intercept)           wt  
    ##       28.41        -2.78  
    ## 
    ## 
    ## $`8`
    ## 
    ## Call:
    ## lm(formula = mpg ~ wt, data = .)
    ## 
    ## Coefficients:
    ## (Intercept)           wt  
    ##      23.868       -2.192

A likelihood function
=====================

``` r
L_exp <- function(x, lambda) prod(dexp(x, lambda))
set.seed(1)
x <- rexp(100, 1)
L_exp(x, 1)
```

    ## [1] 1.730976e-45

``` r
lambda <- seq(0.5, 2, length.out = 10)
L_exp(x, lambda)
```

    ## [1] 7.026489e-50

Use map\_dbl to compute the vector (*L*(*λ*<sub>1</sub>),…,*L*(*λ*<sub>10</sub>))

``` r
map_dbl(lambda, L_exp, x = x)
```

    ##  [1] 3.282056e-53 3.545968e-48 6.031861e-46 1.730976e-45 2.969513e-46
    ##  [6] 6.480301e-48 2.927943e-50 3.819679e-53 1.823940e-56 3.798235e-60

More dramatic
=============

Modify the function repertoir from day 12 (Class11) such that it generates a suitable result if the API call fails (e.g. due to an empty repertoir). Use it in combination with e.g. map\_df in order to create a table of all sets from 1908-2017. Who is the most active director?

``` r
repertoire <- function(year){
  html <- read_html(paste0("https://www.dramaten.se/medverkande/rollboken/?category=date&query=", year))
  get_column <- function(css) {
    html %>%
      html_nodes(css) %>%
      html_text()
  }
  df <- data.frame(Play = get_column(".play-name")[-1], 
                   Opening_night = get_column(".first-appearance")[-1],
                   Director = get_column(".director")[-1], Stage = get_column(".stage")[-1])
  df$Director <- df$Director %>%
    str_extract_all("[A-ZÄÅÖ][a-ö-A-Ö]+ [A-ZÄÅÖ][a-ö-A-Ö]+") %>%
    lapply(function(x) if(identical(x,character(0))) "" else x) %>%
    unlist()
  df <- df %>%
    mutate(Play = as.character(Play), Opening_night = as.POSIXct(Opening_night), Stage = as.character(Stage))
  return(df)
}

safe_repertoire <- safely(repertoire)
years <- 1908:2017
data <- map(years, safe_repertoire)
dfs <- transpose(data)[["result"]]
errs <- transpose(data)[["error"]]
is_ok <- map_lgl(errs, is.null)
years[!is_ok]
```

    ## [1] 2003 2014

``` r
dramatic <- map_dfr(dfs[is_ok], as.data.frame)

dramatic %>%
  group_by(Director) %>%
  summarize(number = n()) %>%
  arrange(desc(number)) %>%
  filter(Director != "") %>%
  head(n = 1)
```

    ## # A tibble: 1 x 2
    ##   Director    number
    ##   <chr>        <int>
    ## 1 Alf Sjöberg    140

``` r
dramatic %>%
  mutate(Year = format(Opening_night, "%Y")) %>%
  filter(Year %in% years[!is_ok])
```

    ## [1] Play          Opening_night Director      Stage         Year         
    ## <0 rows> (or 0-length row.names)

More SHL-players
================

``` r
get_player <- function(url){
  if (GET(paste0(url, "/statistics"))$status_code != 200) {
    stop("error in GET url")
  } else {
    response <- GET(paste0(url, "/statistics"))
  }
  player_xml <- content(response, "text") %>% 
    read_html(player_page)
  css <- list(".rmss_c-squad__player-header-name-h", # Namn
              ".rmss_c-squad__player-header-name-info-position", #Position
              ".rmss_c-squad__player-header-info-items-item-value") # Info
  map(css, html_nodes, x = player_xml) %>% 
    map(html_text) %>% 
    unlist() %>% 
    .[1:8] %>%
    set_names(c("Namn", "Position", "Födelsedatum", "Ålder", "Nationalitet", "Längd", "Vikt", "Skjuter")) %>%
    data.frame() %>%
    t() %>%
    data.frame() %>%
    mutate(Namn = as.character(Namn), Position = as.character(Position), 
           Födelsedatum = as.POSIXct(Födelsedatum), Ålder = as.numeric(Ålder),
           Nationalitet = as.character(Nationalitet), Skjuter = as.character(Skjuter))
}
get_player("http://www.shl.se/lag/087a-087aTQv9u__frolunda-hc/qQ9-a5b4QRqdS__ryan-lasch")
```

    ##         Namn Position Födelsedatum Ålder Nationalitet  Längd  Vikt Skjuter
    ## 1 Ryan Lasch  Forward   1987-01-22     1          USA 170 cm 71 kg   Höger

Construct a new function get\_team, that given an url of the form "<http://www.shl.se/lag/2459-2459QTs1f__djurgarden-hockey/roster>" generates a data.frame of players and informations from get\_player.

``` r
get_team <- function(url) {
  response <- GET(url)
  if (response$status_code != 200) {
    stop("link is not valid!!")
  }
  player_url <- content(response, "text") %>% 
    read_html() %>%
    html_nodes(".rmss_c-squad__team-cont-roster-group-item a") %>%
    html_attr("href") %>%
    map_chr(function(x) paste0("http://www.shl.se", x))
  map_dfr(player_url, get_player)
}
get_team("http://www.shl.se/lag/2459-2459QTs1f__djurgarden-hockey/roster") %>% 
  head()
```

    ##                Namn Position Födelsedatum Ålder Nationalitet  Längd  Vikt
    ## 1      Robin Jensen  Målvakt   1996-01-23     1      Sverige 189 cm 88 kg
    ## 2    Adam Reideborn  Målvakt   1992-01-18     1      Sverige 184 cm 86 kg
    ## 3   Simon Johansson     Back   1999-06-14     1      Sverige 183 cm 84 kg
    ## 4   Linus Hultström     Back   1992-12-09     1      Sverige 180 cm 89 kg
    ## 5 Jesper Pettersson     Back   1994-07-16     1      Sverige 175 cm 87 kg
    ## 6       Olle Alsing     Back   1996-05-01     1      Sverige 181 cm 77 kg
    ##   Skjuter
    ## 1 Vänster
    ## 2 Vänster
    ## 3   Höger
    ## 4   Höger
    ## 5   Höger
    ## 6       -
