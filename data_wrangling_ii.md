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

## `filter`

Getting rid of rows

``` r
filter(litters_data, gd_of_birth == 20)
```

    ## # A tibble: 32 x 8
    ##   group litter_number gd0_weight gd18_weight gd_of_birth pups_born_alive
    ##   <chr> <chr>              <dbl>       <dbl>       <dbl>           <dbl>
    ## 1 Con7  #85                 19.7        34.7          20               3
    ## 2 Con7  #4/2/95/3-3         NA          NA            20               6
    ## 3 Con7  #2/2/95/3-2         NA          NA            20               6
    ## # ... with 29 more rows, and 2 more variables: pups_dead_birth <dbl>,
    ## #   pups_survive <dbl>

``` r
filter(litters_data, group == "Con7")
```

    ## # A tibble: 7 x 8
    ##   group litter_number   gd0_weight gd18_weight gd_of_birth pups_born_alive
    ##   <chr> <chr>                <dbl>       <dbl>       <dbl>           <dbl>
    ## 1 Con7  #85                   19.7        34.7          20               3
    ## 2 Con7  #1/2/95/2             27          42            19               8
    ## 3 Con7  #5/5/3/83/3-3         26          41.4          19               6
    ## 4 Con7  #5/4/2/95/2           28.5        44.1          19               5
    ## 5 Con7  #4/2/95/3-3           NA          NA            20               6
    ## 6 Con7  #2/2/95/3-2           NA          NA            20               6
    ## 7 Con7  #1/5/3/83/3-3/2       NA          NA            20               9
    ## # ... with 2 more variables: pups_dead_birth <dbl>, pups_survive <dbl>

``` r
filter(litters_data, gd0_weight < 23)
```

    ## # A tibble: 12 x 8
    ##    group litter_number gd0_weight gd18_weight gd_of_birth pups_born_alive
    ##    <chr> <chr>              <dbl>       <dbl>       <dbl>           <dbl>
    ##  1 Con7  #85                 19.7        34.7          20               3
    ##  2 Mod7  #59                 17          33.4          19               8
    ##  3 Mod7  #103                21.4        42.1          19               9
    ##  4 Mod7  #5/3/83/5-2         22.6        37            19               5
    ##  5 Mod7  #106                21.7        37.8          20               5
    ##  6 Mod7  #62                 19.5        35.9          19               7
    ##  7 Low7  #107                22.6        42.4          20               9
    ##  8 Low7  #85/2               22.2        38.5          20               8
    ##  9 Low7  #102                22.6        43.3          20              11
    ## 10 Low8  #53                 21.8        37.2          20               8
    ## 11 Low8  #100                20          39.2          20               8
    ## 12 Low8  #4/84               21.8        35.2          20               4
    ## # ... with 2 more variables: pups_dead_birth <dbl>, pups_survive <dbl>

``` r
filter(litters_data, pups_survive != 4)
```

    ## # A tibble: 44 x 8
    ##   group litter_number gd0_weight gd18_weight gd_of_birth pups_born_alive
    ##   <chr> <chr>              <dbl>       <dbl>       <dbl>           <dbl>
    ## 1 Con7  #85                 19.7        34.7          20               3
    ## 2 Con7  #1/2/95/2           27          42            19               8
    ## 3 Con7  #5/5/3/83/3-3       26          41.4          19               6
    ## # ... with 41 more rows, and 2 more variables: pups_dead_birth <dbl>,
    ## #   pups_survive <dbl>

``` r
filter(litters_data, !(group == "Con7"))
```

    ## # A tibble: 42 x 8
    ##   group litter_number gd0_weight gd18_weight gd_of_birth pups_born_alive
    ##   <chr> <chr>              <dbl>       <dbl>       <dbl>           <dbl>
    ## 1 Con8  #3/83/3-3           NA            NA          20               9
    ## 2 Con8  #2/95/3             NA            NA          20               8
    ## 3 Con8  #3/5/2/2/95         28.5          NA          20               8
    ## # ... with 39 more rows, and 2 more variables: pups_dead_birth <dbl>,
    ## #   pups_survive <dbl>

``` r
filter(litters_data, group %in% c("Con7", "COn8"))
```

    ## # A tibble: 7 x 8
    ##   group litter_number   gd0_weight gd18_weight gd_of_birth pups_born_alive
    ##   <chr> <chr>                <dbl>       <dbl>       <dbl>           <dbl>
    ## 1 Con7  #85                   19.7        34.7          20               3
    ## 2 Con7  #1/2/95/2             27          42            19               8
    ## 3 Con7  #5/5/3/83/3-3         26          41.4          19               6
    ## 4 Con7  #5/4/2/95/2           28.5        44.1          19               5
    ## 5 Con7  #4/2/95/3-3           NA          NA            20               6
    ## 6 Con7  #2/2/95/3-2           NA          NA            20               6
    ## 7 Con7  #1/5/3/83/3-3/2       NA          NA            20               9
    ## # ... with 2 more variables: pups_dead_birth <dbl>, pups_survive <dbl>

``` r
filter(litters_data, group == "Con7", gd_of_birth == 20)
```

    ## # A tibble: 4 x 8
    ##   group litter_number   gd0_weight gd18_weight gd_of_birth pups_born_alive
    ##   <chr> <chr>                <dbl>       <dbl>       <dbl>           <dbl>
    ## 1 Con7  #85                   19.7        34.7          20               3
    ## 2 Con7  #4/2/95/3-3           NA          NA            20               6
    ## 3 Con7  #2/2/95/3-2           NA          NA            20               6
    ## 4 Con7  #1/5/3/83/3-3/2       NA          NA            20               9
    ## # ... with 2 more variables: pups_dead_birth <dbl>, pups_survive <dbl>

``` r
filter(litters_data, group == "Con7" | gd_of_birth == 20)
```

    ## # A tibble: 35 x 8
    ##   group litter_number gd0_weight gd18_weight gd_of_birth pups_born_alive
    ##   <chr> <chr>              <dbl>       <dbl>       <dbl>           <dbl>
    ## 1 Con7  #85                 19.7        34.7          20               3
    ## 2 Con7  #1/2/95/2           27          42            19               8
    ## 3 Con7  #5/5/3/83/3-3       26          41.4          19               6
    ## # ... with 32 more rows, and 2 more variables: pups_dead_birth <dbl>,
    ## #   pups_survive <dbl>

``` r
drop_na(litters_data)
```

    ## # A tibble: 31 x 8
    ##   group litter_number gd0_weight gd18_weight gd_of_birth pups_born_alive
    ##   <chr> <chr>              <dbl>       <dbl>       <dbl>           <dbl>
    ## 1 Con7  #85                 19.7        34.7          20               3
    ## 2 Con7  #1/2/95/2           27          42            19               8
    ## 3 Con7  #5/5/3/83/3-3       26          41.4          19               6
    ## # ... with 28 more rows, and 2 more variables: pups_dead_birth <dbl>,
    ## #   pups_survive <dbl>

``` r
drop_na(litters_data, gd0_weight)
```

    ## # A tibble: 34 x 8
    ##   group litter_number gd0_weight gd18_weight gd_of_birth pups_born_alive
    ##   <chr> <chr>              <dbl>       <dbl>       <dbl>           <dbl>
    ## 1 Con7  #85                 19.7        34.7          20               3
    ## 2 Con7  #1/2/95/2           27          42            19               8
    ## 3 Con7  #5/5/3/83/3-3       26          41.4          19               6
    ## # ... with 31 more rows, and 2 more variables: pups_dead_birth <dbl>,
    ## #   pups_survive <dbl>

## `mutate`

Adding/changing columns

``` r
mutate(litters_data, weight_change = gd18_weight - gd0_weight)
```

    ## # A tibble: 49 x 9
    ##   group litter_number gd0_weight gd18_weight gd_of_birth pups_born_alive
    ##   <chr> <chr>              <dbl>       <dbl>       <dbl>           <dbl>
    ## 1 Con7  #85                 19.7        34.7          20               3
    ## 2 Con7  #1/2/95/2           27          42            19               8
    ## 3 Con7  #5/5/3/83/3-3       26          41.4          19               6
    ## # ... with 46 more rows, and 3 more variables: pups_dead_birth <dbl>,
    ## #   pups_survive <dbl>, weight_change <dbl>

``` r
mutate(
  litters_data,
  weight_change = gd18_weight - gd0_weight,
  group = str_to_lower(group)
)
```

    ## # A tibble: 49 x 9
    ##   group litter_number gd0_weight gd18_weight gd_of_birth pups_born_alive
    ##   <chr> <chr>              <dbl>       <dbl>       <dbl>           <dbl>
    ## 1 con7  #85                 19.7        34.7          20               3
    ## 2 con7  #1/2/95/2           27          42            19               8
    ## 3 con7  #5/5/3/83/3-3       26          41.4          19               6
    ## # ... with 46 more rows, and 3 more variables: pups_dead_birth <dbl>,
    ## #   pups_survive <dbl>, weight_change <dbl>

## `arrange`

rearrangement

``` r
arrange(litters_data, gd_of_birth, gd0_weight)
```

    ## # A tibble: 49 x 8
    ##   group litter_number gd0_weight gd18_weight gd_of_birth pups_born_alive
    ##   <chr> <chr>              <dbl>       <dbl>       <dbl>           <dbl>
    ## 1 Mod7  #59                 17          33.4          19               8
    ## 2 Mod7  #62                 19.5        35.9          19               7
    ## 3 Mod7  #103                21.4        42.1          19               9
    ## # ... with 46 more rows, and 2 more variables: pups_dead_birth <dbl>,
    ## #   pups_survive <dbl>

## Pipes

``` r
litters_df = 
  read_csv("data/FAS_litters.csv") %>%
  janitor::clean_names() %>%
  select(group, pups_survive) %>%
  filter(group == "Con7")
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
litters_df = 
  read_csv("data/FAS_litters.csv") %>% 
  janitor::clean_names() %>% 
  select(-pups_survive) %>% 
  mutate(
    weight_change = gd18_weight - gd0_weight,
    group = str_to_lower(group)
  ) %>% 
  drop_na(weight_change) %>% 
  filter(group %in% c("Con7", "COn8")) %>% 
  select(litter_number, group, weight_change, everything())
```

    ## Rows: 49 Columns: 8

    ## -- Column specification --------------------------------------------------------
    ## Delimiter: ","
    ## chr (2): Group, Litter Number
    ## dbl (6): GD0 weight, GD18 weight, GD of Birth, Pups born alive, Pups dead @ ...

    ## 
    ## i Use `spec()` to retrieve the full column specification for this data.
    ## i Specify the column types or set `show_col_types = FALSE` to quiet this message.

Shortcut for %&gt;% is shift+ctrl+M
