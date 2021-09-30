Data wrangling iii: tidy data
================

``` r
library(tidyverse)
library(readxl)
library(haven)
```

## `pivot_longer`

``` r
pulse_df = 
  haven::read_sas("./data/public_pulse_data.sas7bdat") %>% janitor::clean_names()

pulse_df
```

    ## # A tibble: 1,087 x 7
    ##       id   age sex    bdi_score_bl bdi_score_01m bdi_score_06m bdi_score_12m
    ##    <dbl> <dbl> <chr>         <dbl>         <dbl>         <dbl>         <dbl>
    ##  1 10003  48.0 male              7             1             2             0
    ##  2 10015  72.5 male              6            NA            NA            NA
    ##  3 10022  58.5 male             14             3             8            NA
    ##  4 10026  72.7 male             20             6            18            16
    ##  5 10035  60.4 male              4             0             1             2
    ##  6 10050  84.7 male              2            10            12             8
    ##  7 10078  31.3 male              4             0            NA            NA
    ##  8 10088  56.9 male              5            NA             0             2
    ##  9 10091  76.0 male              0             3             4             0
    ## 10 10092  74.2 female           10             2            11             6
    ## # ... with 1,077 more rows

Let’s try to pivot (also remove prefix and mutate)

``` r
pulse_tidy = 
  pulse_df %>% 
  pivot_longer(
    bdi_score_bl:bdi_score_12m,
    names_to = "visit",
    names_prefix = "bdi_score_",
    values_to = "bdi"
  ) %>% 
  mutate(
    visit = replace(visit, visit == "bl", "00m")
  )

pulse_tidy
```

    ## # A tibble: 4,348 x 5
    ##       id   age sex   visit   bdi
    ##    <dbl> <dbl> <chr> <chr> <dbl>
    ##  1 10003  48.0 male  00m       7
    ##  2 10003  48.0 male  01m       1
    ##  3 10003  48.0 male  06m       2
    ##  4 10003  48.0 male  12m       0
    ##  5 10015  72.5 male  00m       6
    ##  6 10015  72.5 male  01m      NA
    ##  7 10015  72.5 male  06m      NA
    ##  8 10015  72.5 male  12m      NA
    ##  9 10022  58.5 male  00m      14
    ## 10 10022  58.5 male  01m       3
    ## # ... with 4,338 more rows

Always use `pivot_longer()` NOT `gather()`

## `pivot_wider`

lets make up a results data table

``` r
analysis_df = 
  tibble(
    group = c("treatment", "treatment", "control", "control"),
    time = c("a", "b", "a", "b"),
    group_mean = c(4, 8, 3, 6)
  )

analysis_df %>% 
  pivot_wider(
    names_from = "time",
    values_from = "group_mean"
  ) %>% 
  knitr::kable()
```

| group     |   a |   b |
|:----------|----:|----:|
| treatment |   4 |   8 |
| control   |   3 |   6 |

Don’t use `spread()` for this. `pivot_wider()` only

## `bind_rows`

Import LotR movies excel file

``` r
fellowship_df = 
  read_excel("data/LotR_Words.xlsx", range = "B3:D6") %>% 
  mutate(movie = "fellowship_rings")

two_towers_df = 
  read_excel("data/LotR_Words.xlsx", range = "F3:H6") %>% 
  mutate(movie = "two_towers")

return_df = 
  read_excel("data/LotR_Words.xlsx", range = "J3:L6") %>% 
  mutate(movie = "return_king")

lotr_df = 
  bind_rows(fellowship_df, two_towers_df, return_df) %>% 
  janitor::clean_names() %>% 
  pivot_longer(
    female:male,
    names_to = "sex",
    values_to = "words"
  ) %>% 
  relocate(movie)

lotr_df
```

    ## # A tibble: 18 x 4
    ##    movie            race   sex    words
    ##    <chr>            <chr>  <chr>  <dbl>
    ##  1 fellowship_rings Elf    female  1229
    ##  2 fellowship_rings Elf    male     971
    ##  3 fellowship_rings Hobbit female    14
    ##  4 fellowship_rings Hobbit male    3644
    ##  5 fellowship_rings Man    female     0
    ##  6 fellowship_rings Man    male    1995
    ##  7 two_towers       Elf    female   331
    ##  8 two_towers       Elf    male     513
    ##  9 two_towers       Hobbit female     0
    ## 10 two_towers       Hobbit male    2463
    ## 11 two_towers       Man    female   401
    ## 12 two_towers       Man    male    3589
    ## 13 return_king      Elf    female   183
    ## 14 return_king      Elf    male     510
    ## 15 return_king      Hobbit female     2
    ## 16 return_king      Hobbit male    2673
    ## 17 return_king      Man    female   268
    ## 18 return_king      Man    male    2459

`rbind()` is also a command that binds rows, but use `bind_rows()` (its
better)

## joins

Look at FAS data.This imports and cleans litters and pups

``` r
litters_df = 
  read_csv("data/FAS_litters.csv") %>% 
  janitor::clean_names() %>% 
  separate(group, into = c("dose", "day_of_tx"), 3) %>% 
  relocate(litter_number) %>% 
  mutate(dose = str_to_lower(dose))
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
pups_df = 
  read_csv("data/FAS_pups.csv") %>% 
  janitor::clean_names() %>% 
  mutate(sex = recode(sex, `1` = "male", `2` = "female"))
```

    ## Rows: 313 Columns: 6

    ## -- Column specification --------------------------------------------------------
    ## Delimiter: ","
    ## chr (1): Litter Number
    ## dbl (5): Sex, PD ears, PD eyes, PD pivot, PD walk

    ## 
    ## i Use `spec()` to retrieve the full column specification for this data.
    ## i Specify the column types or set `show_col_types = FALSE` to quiet this message.

Now we join them

``` r
fas_df = 
  left_join(pups_df, litters_df, by = "litter_number") %>% 
  relocate(litter_number, dose, day_of_tx)
```
