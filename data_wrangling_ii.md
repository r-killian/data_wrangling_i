data wrangling ii
================

## Load library

``` r
library(tidyverse)
```

    ## -- Attaching packages --------------------------------------- tidyverse 1.3.1 --

    ## v ggplot2 3.3.5     v purrr   0.3.4
    ## v tibble  3.1.4     v dplyr   1.0.7
    ## v tidyr   1.1.3     v stringr 1.4.0
    ## v readr   2.0.1     v forcats 0.5.1

    ## -- Conflicts ------------------------------------------ tidyverse_conflicts() --
    ## x dplyr::filter() masks stats::filter()
    ## x dplyr::lag()    masks stats::lag()

## Load data and clean column names

``` r
options(tibble.print_min = 3)

litters_data = read_csv("./data/FAS_litters.csv")
```

    ## Rows: 49 Columns: 8

    ## -- Column specification --------------------------------------------------------
    ## Delimiter: ","
    ## chr (2): Group, Litter Number
    ## dbl (6): GD0 weight, GD18 weight, GD of Birth, Pups born alive, Pups dead @ ...

    ## 
    ## i Use `spec()` to retrieve the full column specification for this data.
    ## i Specify the column types or set `show_col_types = FALSE` to quiet this message.

``` r
litters_data = janitor::clean_names(litters_data)

pups_data = read_csv("./data/FAS_pups.csv")
```

    ## Rows: 313 Columns: 6

    ## -- Column specification --------------------------------------------------------
    ## Delimiter: ","
    ## chr (1): Litter Number
    ## dbl (5): Sex, PD ears, PD eyes, PD pivot, PD walk

    ## 
    ## i Use `spec()` to retrieve the full column specification for this data.
    ## i Specify the column types or set `show_col_types = FALSE` to quiet this message.

``` r
pups_data = janitor::clean_names(pups_data)
```

## `select`

lets select some columns.

``` r
select(litters_data, group, litter_number)
```

    ## # A tibble: 49 x 2
    ##   group litter_number
    ##   <chr> <chr>        
    ## 1 Con7  #85          
    ## 2 Con7  #1/2/95/2    
    ## 3 Con7  #5/5/3/83/3-3
    ## # ... with 46 more rows

``` r
select(litters_data, group, gd0_weight, gd18_weight)
```

    ## # A tibble: 49 x 3
    ##   group gd0_weight gd18_weight
    ##   <chr>      <dbl>       <dbl>
    ## 1 Con7        19.7        34.7
    ## 2 Con7        27          42  
    ## 3 Con7        26          41.4
    ## # ... with 46 more rows

``` r
select(litters_data, group, gd0_weight:gd_of_birth)
```

    ## # A tibble: 49 x 4
    ##   group gd0_weight gd18_weight gd_of_birth
    ##   <chr>      <dbl>       <dbl>       <dbl>
    ## 1 Con7        19.7        34.7          20
    ## 2 Con7        27          42            19
    ## 3 Con7        26          41.4          19
    ## # ... with 46 more rows

``` r
select(litters_data, group, starts_with("pups"))
```

    ## # A tibble: 49 x 4
    ##   group pups_born_alive pups_dead_birth pups_survive
    ##   <chr>           <dbl>           <dbl>        <dbl>
    ## 1 Con7                3               4            3
    ## 2 Con7                8               0            7
    ## 3 Con7                6               0            5
    ## # ... with 46 more rows

``` r
select(litters_data, -litter_number)
```

    ## # A tibble: 49 x 7
    ##   group gd0_weight gd18_weight gd_of_birth pups_born_alive pups_dead_birth
    ##   <chr>      <dbl>       <dbl>       <dbl>           <dbl>           <dbl>
    ## 1 Con7        19.7        34.7          20               3               4
    ## 2 Con7        27          42            19               8               0
    ## 3 Con7        26          41.4          19               6               0
    ## # ... with 46 more rows, and 1 more variable: pups_survive <dbl>

``` r
select(litters_data, GROUP = group, litter_number)
```

    ## # A tibble: 49 x 2
    ##   GROUP litter_number
    ##   <chr> <chr>        
    ## 1 Con7  #85          
    ## 2 Con7  #1/2/95/2    
    ## 3 Con7  #5/5/3/83/3-3
    ## # ... with 46 more rows

``` r
rename(litters_data, GROUP = group)
```

    ## # A tibble: 49 x 8
    ##   GROUP litter_number gd0_weight gd18_weight gd_of_birth pups_born_alive
    ##   <chr> <chr>              <dbl>       <dbl>       <dbl>           <dbl>
    ## 1 Con7  #85                 19.7        34.7          20               3
    ## 2 Con7  #1/2/95/2           27          42            19               8
    ## 3 Con7  #5/5/3/83/3-3       26          41.4          19               6
    ## # ... with 46 more rows, and 2 more variables: pups_dead_birth <dbl>,
    ## #   pups_survive <dbl>

``` r
select(litters_data, litter_number, everything())
```

    ## # A tibble: 49 x 8
    ##   litter_number group gd0_weight gd18_weight gd_of_birth pups_born_alive
    ##   <chr>         <chr>      <dbl>       <dbl>       <dbl>           <dbl>
    ## 1 #85           Con7        19.7        34.7          20               3
    ## 2 #1/2/95/2     Con7        27          42            19               8
    ## 3 #5/5/3/83/3-3 Con7        26          41.4          19               6
    ## # ... with 46 more rows, and 2 more variables: pups_dead_birth <dbl>,
    ## #   pups_survive <dbl>

``` r
relocate(litters_data, litter_number)
```

    ## # A tibble: 49 x 8
    ##   litter_number group gd0_weight gd18_weight gd_of_birth pups_born_alive
    ##   <chr>         <chr>      <dbl>       <dbl>       <dbl>           <dbl>
    ## 1 #85           Con7        19.7        34.7          20               3
    ## 2 #1/2/95/2     Con7        27          42            19               8
    ## 3 #5/5/3/83/3-3 Con7        26          41.4          19               6
    ## # ... with 46 more rows, and 2 more variables: pups_dead_birth <dbl>,
    ## #   pups_survive <dbl>

## Learning Assessment

Learning Assessment: In the pups data, select the columns containing
litter number, sex, and PD ears

``` r
select(pups_data, litter_number, sex, pd_ears)
```

    ## # A tibble: 313 x 3
    ##   litter_number   sex pd_ears
    ##   <chr>         <dbl>   <dbl>
    ## 1 #85               1       4
    ## 2 #85               1       4
    ## 3 #1/2/95/2         1       5
    ## # ... with 310 more rows
