01\_explore-libraries\_jenny.R
================
daniela
Wed Jan 31 14:05:44 2018

``` r
## how jenny might do this in a first exploration
## purposely leaving a few things to change later!
```

Which libraries does R search for packages?

``` r
.libPaths()
```

    ## [1] "/Library/Frameworks/R.framework/Versions/3.4/Resources/library"

``` r
## let's confirm the second element is, in fact, the default library
.Library
```

    ## [1] "/Library/Frameworks/R.framework/Resources/library"

``` r
library(fs)
path_real(.Library)   
```

    ## /Library/Frameworks/R.framework/Versions/3.4/Resources/library

Installed packages

``` r
library(tidyverse)
```

    ## ── Attaching packages ───────────────────────────────────────────────────────────────────────────────────────────────────────────────────────── tidyverse 1.2.1 ──

    ## ✔ ggplot2 2.2.1     ✔ purrr   0.2.4
    ## ✔ tibble  1.4.1     ✔ dplyr   0.7.4
    ## ✔ tidyr   0.7.2     ✔ stringr 1.2.0
    ## ✔ readr   1.1.1     ✔ forcats 0.2.0

    ## ── Conflicts ──────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────── tidyverse_conflicts() ──
    ## ✖ dplyr::filter() masks stats::filter()
    ## ✖ dplyr::lag()    masks stats::lag()

``` r
ipt <- installed.packages() %>%
  as_tibble()

## how many packages?
nrow(ipt)
```

    ## [1] 166

Exploring the packages

``` r
## count some things! inspiration
##   * tabulate by LibPath, Priority, or both
ipt %>%
  count(LibPath, Priority)
```

    ## # A tibble: 3 x 3
    ##   LibPath                                                 Priority       n
    ##   <chr>                                                   <chr>      <int>
    ## 1 /Library/Frameworks/R.framework/Versions/3.4/Resources… base          14
    ## 2 /Library/Frameworks/R.framework/Versions/3.4/Resources… recommend…    15
    ## 3 /Library/Frameworks/R.framework/Versions/3.4/Resources… <NA>         137

``` r
##   * what proportion need compilation?
ipt %>%
  count(NeedsCompilation) %>%
  mutate(prop = n / sum(n))
```

    ## # A tibble: 3 x 3
    ##   NeedsCompilation     n   prop
    ##   <chr>            <int>  <dbl>
    ## 1 no                  72 0.434 
    ## 2 yes                 85 0.512 
    ## 3 <NA>                 9 0.0542

``` r
##   * how break down re: version of R they were built on
ipt %>%
  count(Built) %>%
  mutate(prop = n / sum(n))
```

    ## # A tibble: 4 x 3
    ##   Built     n  prop
    ##   <chr> <int> <dbl>
    ## 1 3.4.0    60 0.361
    ## 2 3.4.1    18 0.108
    ## 3 3.4.2    23 0.139
    ## 4 3.4.3    65 0.392

Reflections

``` r
## reflect on ^^ and make a few notes to yourself; inspiration
##   * does the number of base + recommended packages make sense to you?
##   * how does the result of .libPaths() relate to the result of .Library?
```

Going further

``` r
## if you have time to do more ...

## is every package in .Library either base or recommended?
all_default_pkgs <- list.files(.Library)
all_br_pkgs <- ipt %>%
  filter(Priority %in% c("base", "recommended")) %>%
  pull(Package)
setdiff(all_default_pkgs, all_br_pkgs)
```

    ##   [1] "assertthat"    "backports"     "base64enc"     "bayesm"       
    ##   [5] "BH"            "bindr"         "bindrcpp"      "bit"          
    ##   [9] "bit64"         "bitops"        "brocks"        "broom"        
    ##  [13] "callr"         "caTools"       "cellranger"    "cli"          
    ##  [17] "clipr"         "clisymbols"    "colorspace"    "compositions" 
    ##  [21] "crayon"        "crosstalk"     "curl"          "DBI"          
    ##  [25] "dbplyr"        "debugme"       "DEoptimR"      "desc"         
    ##  [29] "devtools"      "dichromat"     "digest"        "dplyr"        
    ##  [33] "DT"            "emo"           "enc"           "energy"       
    ##  [37] "evaluate"      "forcats"       "fs"            "gdtools"      
    ##  [41] "ggforce"       "ggiraph"       "ggplot2"       "ggraph"       
    ##  [45] "ggrepel"       "ggtern"        "gh"            "git2r"        
    ##  [49] "glue"          "googlesheets"  "gridExtra"     "gtable"       
    ##  [53] "haven"         "here"          "highr"         "hms"          
    ##  [57] "htmltools"     "htmlwidgets"   "httpuv"        "httr"         
    ##  [61] "igraph"        "ini"           "irlba"         "jsonlite"     
    ##  [65] "knitr"         "labeling"      "latex2exp"     "lazyeval"     
    ##  [69] "lubridate"     "magrittr"      "markdown"      "memoise"      
    ##  [73] "mime"          "mnormt"        "modelr"        "munsell"      
    ##  [77] "officer"       "openssl"       "packrat"       "pillar"       
    ##  [81] "pkgconfig"     "plogr"         "plyr"          "png"          
    ##  [85] "praise"        "proto"         "psych"         "purrr"        
    ##  [89] "R.methodsS3"   "R.oo"          "R.utils"       "R6"           
    ##  [93] "RColorBrewer"  "Rcpp"          "RcppArmadillo" "readr"        
    ##  [97] "readxl"        "rematch"       "rematch2"      "reprex"       
    ## [101] "reshape2"      "rlang"         "rmarkdown"     "robustbase"   
    ## [105] "rprojroot"     "rstudioapi"    "rtweet"        "rvest"        
    ## [109] "rvg"           "scales"        "selectr"       "servr"        
    ## [113] "shiny"         "sourcetools"   "stringi"       "stringr"      
    ## [117] "styler"        "tensorA"       "testthat"      "tibble"       
    ## [121] "tidyr"         "tidyselect"    "tidyverse"     "translations" 
    ## [125] "tweenr"        "udunits2"      "units"         "usethis"      
    ## [129] "utf8"          "uuid"          "viridis"       "viridisLite"  
    ## [133] "whisker"       "withr"         "xml2"          "xtable"       
    ## [137] "yaml"          "zip"

``` r
## study package naming style (all lower case, contains '.', etc

## use `fields` argument to installed.packages() to get more info and use it!
ipt2 <- installed.packages(fields = "URL") %>%
  as_tibble()
ipt2 %>%
  mutate(github = grepl("github", URL)) %>%
  count(github) %>%
  mutate(prop = n / sum(n))
```

    ## # A tibble: 2 x 3
    ##   github     n  prop
    ##   <lgl>  <int> <dbl>
    ## 1 F         73 0.440
    ## 2 T         93 0.560
