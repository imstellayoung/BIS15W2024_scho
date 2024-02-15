---
title: "Homework 7"
author: "Stella Young Cho"
date: "2024-02-13"
output:
  html_document: 
    theme: spacelab
    keep_md: true
---



## Instructions
Answer the following questions and complete the exercises in RMarkdown. Please embed all of your code and push your final work to your repository. Your final lab report should be organized, clean, and run free from errors. Remember, you must remove the `#` for the included code chunks to run. Be sure to add your name to the author header above.  

Make sure to use the formatting conventions of RMarkdown to make your report neat and clean!  

## Load the libraries

```r
library(tidyverse)
library(janitor)
library(skimr)
library(naniar)
```

## Data
**1. For this homework, we will use two different data sets. Please load `amniota` and `amphibio`.**  

`amniota` data:  
Myhrvold N, Baldridge E, Chan B, Sivam D, Freeman DL, Ernest SKM (2015). “An amniote life-history
database to perform comparative analyses with birds, mammals, and reptiles.” _Ecology_, *96*, 3109.
doi: 10.1890/15-0846.1 (URL: https://doi.org/10.1890/15-0846.1).

```r
amniota <-  readr::read_csv(file = "data/amniota.csv")
```

```
## Rows: 21322 Columns: 36
## ── Column specification ────────────────────────────────────────────────────────
## Delimiter: ","
## chr  (6): class, order, family, genus, species, common_name
## dbl (30): subspecies, female_maturity_d, litter_or_clutch_size_n, litters_or...
## 
## ℹ Use `spec()` to retrieve the full column specification for this data.
## ℹ Specify the column types or set `show_col_types = FALSE` to quiet this message.
```

`amphibio` data:  
Oliveira BF, São-Pedro VA, Santos-Barrera G, Penone C, Costa GC (2017). “AmphiBIO, a global database
for amphibian ecological traits.” _Scientific Data_, *4*, 170123. doi: 10.1038/sdata.2017.123 (URL:
https://doi.org/10.1038/sdata.2017.123).

```r
amphibio <- readr::read_csv(file = "data/amphibio.csv")
```

```
## Rows: 6776 Columns: 38
## ── Column specification ────────────────────────────────────────────────────────
## Delimiter: ","
## chr  (6): id, Order, Family, Genus, Species, OBS
## dbl (31): Fos, Ter, Aqu, Arb, Leaves, Flowers, Seeds, Arthro, Vert, Diu, Noc...
## lgl  (1): Fruits
## 
## ℹ Use `spec()` to retrieve the full column specification for this data.
## ℹ Specify the column types or set `show_col_types = FALSE` to quiet this message.
```

## Questions  
**2. Do some exploratory analysis of the `amniota` data set. Use the function(s) of your choice. Try to get an idea of how NA's are represented in the data.**  

```r
glimpse(amniota)
```

```
## Rows: 21,322
## Columns: 36
## $ class                                 <chr> "Aves", "Aves", "Aves", "Aves", …
## $ order                                 <chr> "Accipitriformes", "Accipitrifor…
## $ family                                <chr> "Accipitridae", "Accipitridae", …
## $ genus                                 <chr> "Accipiter", "Accipiter", "Accip…
## $ species                               <chr> "albogularis", "badius", "bicolo…
## $ subspecies                            <dbl> -999, -999, -999, -999, -999, -9…
## $ common_name                           <chr> "Pied Goshawk", "Shikra", "Bicol…
## $ female_maturity_d                     <dbl> -999.000, 363.468, -999.000, -99…
## $ litter_or_clutch_size_n               <dbl> -999.000, 3.250, 2.700, -999.000…
## $ litters_or_clutches_per_y             <dbl> -999, 1, -999, -999, 1, -999, -9…
## $ adult_body_mass_g                     <dbl> 251.500, 140.000, 345.000, 142.0…
## $ maximum_longevity_y                   <dbl> -999.00000, -999.00000, -999.000…
## $ gestation_d                           <dbl> -999, -999, -999, -999, -999, -9…
## $ weaning_d                             <dbl> -999, -999, -999, -999, -999, -9…
## $ birth_or_hatching_weight_g            <dbl> -999, -999, -999, -999, -999, -9…
## $ weaning_weight_g                      <dbl> -999, -999, -999, -999, -999, -9…
## $ egg_mass_g                            <dbl> -999.00, 21.00, 32.00, -999.00, …
## $ incubation_d                          <dbl> -999.00, 30.00, -999.00, -999.00…
## $ fledging_age_d                        <dbl> -999.00, 32.00, -999.00, -999.00…
## $ longevity_y                           <dbl> -999.00000, -999.00000, -999.000…
## $ male_maturity_d                       <dbl> -999, -999, -999, -999, -999, -9…
## $ inter_litter_or_interbirth_interval_y <dbl> -999, -999, -999, -999, -999, -9…
## $ female_body_mass_g                    <dbl> 352.500, 168.500, 390.000, -999.…
## $ male_body_mass_g                      <dbl> 223.000, 125.000, 212.000, 142.0…
## $ no_sex_body_mass_g                    <dbl> -999.0, 123.0, -999.0, -999.0, -…
## $ egg_width_mm                          <dbl> -999, -999, -999, -999, -999, -9…
## $ egg_length_mm                         <dbl> -999, -999, -999, -999, -999, -9…
## $ fledging_mass_g                       <dbl> -999, -999, -999, -999, -999, -9…
## $ adult_svl_cm                          <dbl> -999.00, 30.00, 39.50, -999.00, …
## $ male_svl_cm                           <dbl> -999, -999, -999, -999, -999, -9…
## $ female_svl_cm                         <dbl> -999, -999, -999, -999, -999, -9…
## $ birth_or_hatching_svl_cm              <dbl> -999, -999, -999, -999, -999, -9…
## $ female_svl_at_maturity_cm             <dbl> -999, -999, -999, -999, -999, -9…
## $ female_body_mass_at_maturity_g        <dbl> -999, -999, -999, -999, -999, -9…
## $ no_sex_svl_cm                         <dbl> -999, -999, -999, -999, -999, -9…
## $ no_sex_maturity_d                     <dbl> -999, -999, -999, -999, -999, -9…
```

**3. Do some exploratory analysis of the `amphibio` data set. Use the function(s) of your choice. Try to get an idea of how NA's are represented in the data.**  

```r
str(amphibio)
```

```
## spc_tbl_ [6,776 × 38] (S3: spec_tbl_df/tbl_df/tbl/data.frame)
##  $ id                     : chr [1:6776] "Anf0001" "Anf0002" "Anf0003" "Anf0004" ...
##  $ Order                  : chr [1:6776] "Anura" "Anura" "Anura" "Anura" ...
##  $ Family                 : chr [1:6776] "Allophrynidae" "Alytidae" "Alytidae" "Alytidae" ...
##  $ Genus                  : chr [1:6776] "Allophryne" "Alytes" "Alytes" "Alytes" ...
##  $ Species                : chr [1:6776] "Allophryne ruthveni" "Alytes cisternasii" "Alytes dickhilleni" "Alytes maurus" ...
##  $ Fos                    : num [1:6776] NA NA NA NA NA 1 1 1 1 1 ...
##  $ Ter                    : num [1:6776] 1 1 1 1 1 1 1 1 1 1 ...
##  $ Aqu                    : num [1:6776] 1 1 1 1 NA 1 1 1 1 1 ...
##  $ Arb                    : num [1:6776] 1 1 1 1 1 1 NA NA NA NA ...
##  $ Leaves                 : num [1:6776] NA NA NA NA NA NA NA NA NA NA ...
##  $ Flowers                : num [1:6776] NA NA NA NA NA NA NA NA NA NA ...
##  $ Seeds                  : num [1:6776] NA NA NA NA NA NA NA NA NA NA ...
##  $ Fruits                 : logi [1:6776] NA NA NA NA NA NA ...
##  $ Arthro                 : num [1:6776] 1 1 1 NA 1 1 1 1 1 NA ...
##  $ Vert                   : num [1:6776] NA NA NA NA NA NA 1 NA NA NA ...
##  $ Diu                    : num [1:6776] 1 NA NA NA NA NA 1 1 1 NA ...
##  $ Noc                    : num [1:6776] 1 1 1 NA 1 1 1 1 1 NA ...
##  $ Crepu                  : num [1:6776] 1 NA NA NA NA 1 NA NA NA NA ...
##  $ Wet_warm               : num [1:6776] NA NA NA NA 1 1 NA NA NA NA ...
##  $ Wet_cold               : num [1:6776] 1 NA NA NA NA NA 1 NA NA NA ...
##  $ Dry_warm               : num [1:6776] NA NA NA NA NA NA NA NA NA NA ...
##  $ Dry_cold               : num [1:6776] NA NA NA NA NA NA NA NA NA NA ...
##  $ Body_mass_g            : num [1:6776] 31 6.1 NA NA 2.31 13.4 21.8 NA NA NA ...
##  $ Age_at_maturity_min_y  : num [1:6776] NA 2 2 NA 3 2 3 NA NA NA ...
##  $ Age_at_maturity_max_y  : num [1:6776] NA 2 2 NA 3 3 5 NA NA NA ...
##  $ Body_size_mm           : num [1:6776] 31 50 55 NA 40 55 80 60 65 NA ...
##  $ Size_at_maturity_min_mm: num [1:6776] NA 27 NA NA NA 35 NA NA NA NA ...
##  $ Size_at_maturity_max_mm: num [1:6776] NA 36 NA NA NA 40.5 NA NA NA NA ...
##  $ Longevity_max_y        : num [1:6776] NA 6 NA NA NA 7 9 NA NA NA ...
##  $ Litter_size_min_n      : num [1:6776] 300 60 40 NA 7 53 300 1500 1000 NA ...
##  $ Litter_size_max_n      : num [1:6776] 300 180 40 NA 20 171 1500 1500 1000 NA ...
##  $ Reproductive_output_y  : num [1:6776] 1 4 1 4 1 4 6 1 1 1 ...
##  $ Offspring_size_min_mm  : num [1:6776] NA 2.6 NA NA 5.4 2.6 1.5 NA 1.5 NA ...
##  $ Offspring_size_max_mm  : num [1:6776] NA 3.5 NA NA 7 5 2 NA 1.5 NA ...
##  $ Dir                    : num [1:6776] 0 0 0 0 0 0 0 0 0 0 ...
##  $ Lar                    : num [1:6776] 1 1 1 1 1 1 1 1 1 1 ...
##  $ Viv                    : num [1:6776] 0 0 0 0 0 0 0 0 0 0 ...
##  $ OBS                    : chr [1:6776] NA NA NA NA ...
##  - attr(*, "spec")=
##   .. cols(
##   ..   id = col_character(),
##   ..   Order = col_character(),
##   ..   Family = col_character(),
##   ..   Genus = col_character(),
##   ..   Species = col_character(),
##   ..   Fos = col_double(),
##   ..   Ter = col_double(),
##   ..   Aqu = col_double(),
##   ..   Arb = col_double(),
##   ..   Leaves = col_double(),
##   ..   Flowers = col_double(),
##   ..   Seeds = col_double(),
##   ..   Fruits = col_logical(),
##   ..   Arthro = col_double(),
##   ..   Vert = col_double(),
##   ..   Diu = col_double(),
##   ..   Noc = col_double(),
##   ..   Crepu = col_double(),
##   ..   Wet_warm = col_double(),
##   ..   Wet_cold = col_double(),
##   ..   Dry_warm = col_double(),
##   ..   Dry_cold = col_double(),
##   ..   Body_mass_g = col_double(),
##   ..   Age_at_maturity_min_y = col_double(),
##   ..   Age_at_maturity_max_y = col_double(),
##   ..   Body_size_mm = col_double(),
##   ..   Size_at_maturity_min_mm = col_double(),
##   ..   Size_at_maturity_max_mm = col_double(),
##   ..   Longevity_max_y = col_double(),
##   ..   Litter_size_min_n = col_double(),
##   ..   Litter_size_max_n = col_double(),
##   ..   Reproductive_output_y = col_double(),
##   ..   Offspring_size_min_mm = col_double(),
##   ..   Offspring_size_max_mm = col_double(),
##   ..   Dir = col_double(),
##   ..   Lar = col_double(),
##   ..   Viv = col_double(),
##   ..   OBS = col_character()
##   .. )
##  - attr(*, "problems")=<externalptr>
```

**4. How many total NA's are in each data set? Do these values make sense? Are NA's represented by values?**   

```r
amphibio %>% 
  map_df(~sum(is.na(.)))
```

```
## # A tibble: 1 × 38
##      id Order Family Genus Species   Fos   Ter   Aqu   Arb Leaves Flowers Seeds
##   <int> <int>  <int> <int>   <int> <int> <int> <int> <int>  <int>   <int> <int>
## 1     0     0      0     0       0  6053  1104  2810  4347   6752    6772  6772
## # ℹ 26 more variables: Fruits <int>, Arthro <int>, Vert <int>, Diu <int>,
## #   Noc <int>, Crepu <int>, Wet_warm <int>, Wet_cold <int>, Dry_warm <int>,
## #   Dry_cold <int>, Body_mass_g <int>, Age_at_maturity_min_y <int>,
## #   Age_at_maturity_max_y <int>, Body_size_mm <int>,
## #   Size_at_maturity_min_mm <int>, Size_at_maturity_max_mm <int>,
## #   Longevity_max_y <int>, Litter_size_min_n <int>, Litter_size_max_n <int>,
## #   Reproductive_output_y <int>, Offspring_size_min_mm <int>, …
```

**5. Make any necessary replacements in the data such that all NA's appear as "NA".**   

```r
amniota %>% 
  replace_with_na_all(condition = ~.x == -999.0)
```

```
## # A tibble: 21,322 × 36
##    class order     family genus species subspecies common_name female_maturity_d
##    <chr> <chr>     <chr>  <chr> <chr>        <dbl> <chr>                   <dbl>
##  1 Aves  Accipitr… Accip… Acci… albogu…         NA Pied Gosha…               NA 
##  2 Aves  Accipitr… Accip… Acci… badius          NA Shikra                   363.
##  3 Aves  Accipitr… Accip… Acci… bicolor         NA Bicolored …               NA 
##  4 Aves  Accipitr… Accip… Acci… brachy…         NA New Britai…               NA 
##  5 Aves  Accipitr… Accip… Acci… brevip…         NA Levant Spa…              363.
##  6 Aves  Accipitr… Accip… Acci… castan…         NA Chestnut-f…               NA 
##  7 Aves  Accipitr… Accip… Acci… chilen…         NA Chilean Ha…               NA 
##  8 Aves  Accipitr… Accip… Acci… chiono…         NA White-brea…              548.
##  9 Aves  Accipitr… Accip… Acci… cirroc…         NA Collared S…               NA 
## 10 Aves  Accipitr… Accip… Acci… cooper…         NA Cooper's H…              730 
## # ℹ 21,312 more rows
## # ℹ 28 more variables: litter_or_clutch_size_n <dbl>,
## #   litters_or_clutches_per_y <dbl>, adult_body_mass_g <dbl>,
## #   maximum_longevity_y <dbl>, gestation_d <dbl>, weaning_d <dbl>,
## #   birth_or_hatching_weight_g <dbl>, weaning_weight_g <dbl>, egg_mass_g <dbl>,
## #   incubation_d <dbl>, fledging_age_d <dbl>, longevity_y <dbl>,
## #   male_maturity_d <dbl>, inter_litter_or_interbirth_interval_y <dbl>, …
```

**6. Use the package `naniar` to produce a summary, including percentages, of missing data in each column for the `amniota` data.**  

```r
miss_var_summary(amniota)
```

```
## # A tibble: 36 × 3
##    variable                  n_miss pct_miss
##    <chr>                      <int>    <dbl>
##  1 class                          0        0
##  2 order                          0        0
##  3 family                         0        0
##  4 genus                          0        0
##  5 species                        0        0
##  6 subspecies                     0        0
##  7 common_name                    0        0
##  8 female_maturity_d              0        0
##  9 litter_or_clutch_size_n        0        0
## 10 litters_or_clutches_per_y      0        0
## # ℹ 26 more rows
```

**7. Use the package `naniar` to produce a summary, including percentages, of missing data in each column for the `amphibio` data.**

```r
miss_var_summary(amphibio)
```

```
## # A tibble: 38 × 3
##    variable n_miss pct_miss
##    <chr>     <int>    <dbl>
##  1 Fruits     6774    100. 
##  2 Flowers    6772     99.9
##  3 Seeds      6772     99.9
##  4 Leaves     6752     99.6
##  5 Dry_cold   6735     99.4
##  6 Vert       6657     98.2
##  7 OBS        6651     98.2
##  8 Wet_cold   6625     97.8
##  9 Crepu      6608     97.5
## 10 Dry_warm   6572     97.0
## # ℹ 28 more rows
```

**8. For the `amniota` data, calculate the number of NAs in the `egg_mass_g` column sorted by taxonomic class; i.e. how many NA's are present in the `egg_mass_g` column in birds, mammals, and reptiles? Does this results make sense biologically? How do these results affect your interpretation of NA's?**  

```r
amniota %>%
  group_by(class) %>%
  select(egg_mass_g, class) %>%
  miss_var_summary()
```

```
## # A tibble: 3 × 4
## # Groups:   class [3]
##   class    variable   n_miss pct_miss
##   <chr>    <chr>       <int>    <dbl>
## 1 Aves     egg_mass_g      0        0
## 2 Mammalia egg_mass_g      0        0
## 3 Reptilia egg_mass_g      0        0
```

**9. The `amphibio` data have variables that classify species as fossorial (burrowing), terrestrial, aquatic, or arboreal.Calculate the number of NA's in each of these variables. Do you think that the authors intend us to think that there are NA's in these columns or could they represent something else? Explain.**

```r
amphibio %>%
  select(Fos, Ter, Aqu, Arb) %>%
  miss_var_summary()
```

```
## # A tibble: 4 × 3
##   variable n_miss pct_miss
##   <chr>     <int>    <dbl>
## 1 Fos        6053     89.3
## 2 Arb        4347     64.2
## 3 Aqu        2810     41.5
## 4 Ter        1104     16.3
```

**10. Now that we know how NA's are represented in the `amniota` data, how would you load the data such that the values which represent NA's are automatically converted?**

```r
amniota <- read_csv("data/amniota.csv", na= c("NA", "-999"))
```

```
## Warning: One or more parsing issues, call `problems()` on your data frame for details,
## e.g.:
##   dat <- vroom(...)
##   problems(dat)
```

```
## Rows: 21322 Columns: 36
## ── Column specification ────────────────────────────────────────────────────────
## Delimiter: ","
## chr  (6): class, order, family, genus, species, common_name
## dbl (28): female_maturity_d, litter_or_clutch_size_n, litters_or_clutches_pe...
## lgl  (2): subspecies, female_body_mass_at_maturity_g
## 
## ℹ Use `spec()` to retrieve the full column specification for this data.
## ℹ Specify the column types or set `show_col_types = FALSE` to quiet this message.
```

## Push your final code to GitHub!
Please be sure that you check the `keep md` file in the knit preferences.  
