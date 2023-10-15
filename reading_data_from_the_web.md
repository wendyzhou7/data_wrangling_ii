data_wrangling_ii
================
2023-10-15

``` r
library(tidyverse)
```

    ## ── Attaching core tidyverse packages ──────────────────────── tidyverse 2.0.0 ──
    ## ✔ dplyr     1.1.3     ✔ readr     2.1.4
    ## ✔ forcats   1.0.0     ✔ stringr   1.5.0
    ## ✔ ggplot2   3.4.3     ✔ tibble    3.2.1
    ## ✔ lubridate 1.9.3     ✔ tidyr     1.3.0
    ## ✔ purrr     1.0.2     
    ## ── Conflicts ────────────────────────────────────────── tidyverse_conflicts() ──
    ## ✖ dplyr::filter() masks stats::filter()
    ## ✖ dplyr::lag()    masks stats::lag()
    ## ℹ Use the conflicted package (<http://conflicted.r-lib.org/>) to force all conflicts to become errors

``` r
library(rvest)
```

    ## 
    ## Attaching package: 'rvest'
    ## 
    ## The following object is masked from 'package:readr':
    ## 
    ##     guess_encoding

``` r
library(httr)
```

Getting data from web to R: Import NSDUH data

``` r
nsduh_url = "http://samhda.s3-us-gov-west-1.amazonaws.com/s3fs-public/field-uploads/2k15StateFiles/NSDUHsaeShortTermCHG2015.htm"

nsduh_html = 
  read_html(nsduh_url)
```

``` r
marj_use_df = 
  nsduh_html |> 
  html_table() |> 
  first() |> #takes first element in a list of things (asks for first of 15 tables)
  slice(-1) #remove first row (note) that is not part of data
```

Import star wars …

``` r
swm_url = 
  "https://www.imdb.com/list/ls070150896/"

swm_html = 
  read_html(swm_url)
```

Using selector gadget…

``` r
swm_title_vec = 
  swm_html |> 
  html_elements(".lister-item-header a") |> 
  html_text() #takes the stuff out of the html

swm_gross_rev_vec = 
  swm_html |> 
  html_elements(".text-small:nth-child(7) span:nth-child(5)") |> 
  html_text()
 
# Create dataframe with what we extracted from html sites
swm_df = 
  tibble(title = swm_title_vec,
         gross_rev = swm_gross_rev_vec)
```

## APIs

Get water data from NYC.
