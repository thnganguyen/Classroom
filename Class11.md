Class11
================

``` r
rescale01 <- function(x) {
  rng <- range(x, na.rm = FALSE)
  (x - rng[1]) / (rng[2] - rng[1])
}

compute_variance <- function(x) {  # unbiased estimate
  n <- length(x)
  return(sum((x - mean(x))^2)/(n-1))
}

compute_skew <- function(x) {  # biased estimate
  n <- length(x)
  x_bar <- sum(x)/n
  return(sum((x - x_bar)^3)/n/((sum((x - x_bar)^2)/(n-1))^(3/2)))
}

z <- runif(10, min = 0, max = 2)
compute_variance(z)
```

    ## [1] 0.4162566

``` r
var(z)
```

    ## [1] 0.4162566

``` r
compute_skew(z)
```

    ## [1] 0.846077

``` r
is_directory <- function(x) file.info(x)$isdir
is_readable <- function(x) file.access(x, 4) == 0

is_directory("Class_files")
```

    ## [1] TRUE

``` r
is_readable("Class_files")
```

    ## Class_files 
    ##        TRUE

``` r
temp = 15
if (temp <= 0) {
  "freezing"
} else if (temp <= 10) {
  "cold"
} else if (temp <= 20) {
  "cool"
} else if (temp <= 30) {
  "warm"
} else {
  "hot"
}
```

    ## [1] "cool"

``` r
cut(50, breaks = seq(-10, 40, 10),
    right = FALSE,
    labels = c("freezing", "cold", "cool", "warm", "hot"))
```

    ## [1] <NA>
    ## Levels: freezing cold cool warm hot

``` r
commas <- function(...) stringr::str_c(..., collapse = ", ")
# commas(letters, collapse = "-")  # get an error for collapse 

rule <- function(..., pad = "-") {
  title <- paste0(...)
  width <- getOption("width") - nchar(title) - 5
  pad_char <- nchar(pad)
  cat(title, " ", stringr::str_dup(pad, width/pad_char), "\n", sep = "")
}
rule("Title", pad = "-+")
```

    ## Title -+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+

Monte-Carlo integration
=======================

Write a function MC\_int that takes f, a, b, N as inputs and returners I.

``` r
MC_int <- function(f, a, b, N, ...) {
  x <- runif(N, a, b)
  I <- (b-a)*sum(f(x, ...))/N
  return(I)
}

MC_int(dnorm, 0, 1, 10000, mean = 1, sd = 2)
```

    ## [1] 0.1914271

Dramatic
========

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
                   Direction = get_column(".director")[-1], Stage = get_column(".stage")[-1])
  df$Direction <- df$Direction %>%
    str_extract_all("[A-ZÄÅÖ][a-ö-A-Ö]+ [A-ZÄÅÖ][a-ö-A-Ö]+") %>%
    lapply(function(x) if(identical(x,character(0))) "" else x) %>%
    unlist()
  df <- df %>%
    mutate(Play = as.character(Play), Opening_night = as.POSIXct(Opening_night), Stage = as.character(Stage))
  return(df)
}

glimpse(repertoire(2001))
```

    ## Observations: 21
    ## Variables: 4
    ## $ Play          <chr> "Verandan 10: Jättehemligt", "Se dig om i vrede"...
    ## $ Opening_night <dttm> 2001-01-27, 2001-01-27, 2001-02-02, 2001-02-03,...
    ## $ Direction     <chr> "Anna Björk", "Christian Tomner", "Eva Dahlman",...
    ## $ Stage         <chr> "Lejonkulans foajé", "Målarsalen", "Elverket 1",...
