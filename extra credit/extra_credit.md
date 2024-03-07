---
title: "Extra Credit W24"
author: "Stella Young Cho"
date: "2024-03-07"
output:
  html_document: 
    keep_md: true
  pdf_document: default
---

## Instructions
Answer the following questions and complete the exercises in RMarkdown. Please embed all of your code and push your final work to your repository. Your code must be organized, clean, and run free from errors. Remember, you must remove the `#` for any included code chunks to run. Be sure to add your name to the author header above. 

Your code must knit in order to be considered. If you are stuck and cannot answer a question, then comment out your code and knit the document.  

Don't forget to answer any questions that are asked in the prompt. Some questions will require a plot, but others do not- make sure to read each question carefully.  

For the questions that require a plot, make sure to have clearly labeled axes and a title. Keep your plots clean and professional-looking, but you are free to add color and other aesthetics.  

Be sure to follow the directions and push your code to your repository.

## Background
In the `data` folder, you will find data about global shark attacks. The data are updated continuously, and are taken from [opendatasoft](https://public.opendatasoft.com/explore/dataset/global-shark-attack/table/?flg=en-us&disjunctive.country&disjunctive.area&disjunctive.activity).  

## Load the libraries

```r
library("tidyverse")
library("janitor")
library("naniar")
```

## Load the data
Run the following code chunk to import the data.

```r
global_sharks <- read_csv("data/global-shark-attack.csv") %>% clean_names()
```

## Questions
1. (2 points) Start by doing some data exploration using your preferred function(s). What is the structure of the data? Where are the missing values and how are they represented?  

```r
glimpse(global_sharks)
```

```
## Rows: 6,890
## Columns: 21
## $ date                   <date> 2023-07-29, 2023-04-22, 2023-03-02, 2023-02-18…
## $ year                   <dbl> 2023, 2023, 2023, 2023, 2022, 2022, 2021, 2021,…
## $ type                   <chr> "Unprovoked", "Unprovoked", "Unprovoked", "Ques…
## $ country                <chr> "USA", "AUSTRALIA", "SEYCHELLES", "ARGENTINA", …
## $ area                   <chr> "Florida", "Western Australia", "Praslin Island…
## $ location               <chr> "Tampa Bay", "Lucy's Beach", NA, "Chubut Provin…
## $ activity               <chr> "Swimming", "Surfing", "Snorkeling", NA, "Snork…
## $ name                   <chr> "Natalie Branda", "Max Marsden", "Arthur \xc9",…
## $ sex                    <chr> "F", "M", "M", "M", "F", "M", "M", "M", "M", "M…
## $ age                    <chr> "26", "30", "6", "32", NA, "21.0", "15.0", "73.…
## $ injury                 <chr> "Superficial injuries to abomen and thighs", "B…
## $ fatal_y_n              <chr> "N", "N", "UNKNOWN", "UNKNOWN", "N", "N", "N", …
## $ time                   <chr> "20h00", "07h15", "Afternoon", NA, "12h30", "15…
## $ species                <chr> NA, "Bronze whaler shark, 1.5 m", "Lemon shark"…
## $ investigator_or_source <chr> "Fox12, 8/1/2023", "The West Australian, 4/22/2…
## $ pdf                    <chr> NA, NA, NA, NA, "2022.07.28-Cornwall.pdf", "202…
## $ href_formula           <chr> NA, NA, NA, NA, "http://sharkattackfile.net/spr…
## $ href                   <chr> NA, NA, NA, NA, "http://sharkattackfile.net/spr…
## $ case_number_19         <chr> NA, NA, NA, NA, "2022.07.28", "2022.03.09", "20…
## $ case_number_20         <chr> NA, NA, NA, NA, "2022.7.28", "2022.03.09", "202…
## $ original_order         <dbl> NA, NA, NA, NA, 6792, 6743, 6720, 6626, 6618, 6…
```


```r
str(global_sharks)
```

```
## spc_tbl_ [6,890 × 21] (S3: spec_tbl_df/tbl_df/tbl/data.frame)
##  $ date                  : Date[1:6890], format: "2023-07-29" "2023-04-22" ...
##  $ year                  : num [1:6890] 2023 2023 2023 2023 2022 ...
##  $ type                  : chr [1:6890] "Unprovoked" "Unprovoked" "Unprovoked" "Questionable" ...
##  $ country               : chr [1:6890] "USA" "AUSTRALIA" "SEYCHELLES" "ARGENTINA" ...
##  $ area                  : chr [1:6890] "Florida" "Western Australia" "Praslin Island" "Patagonia" ...
##  $ location              : chr [1:6890] "Tampa Bay" "Lucy's Beach" NA "Chubut Province" ...
##  $ activity              : chr [1:6890] "Swimming" "Surfing" "Snorkeling" NA ...
##  $ name                  : chr [1:6890] "Natalie Branda" "Max Marsden" "Arthur \xc9" "Diego Barr\x92a" ...
##  $ sex                   : chr [1:6890] "F" "M" "M" "M" ...
##  $ age                   : chr [1:6890] "26" "30" "6" "32" ...
##  $ injury                : chr [1:6890] "Superficial injuries to abomen and thighs" "Bite to right arm" "Left foot bitten" "Death by misadventure" ...
##  $ fatal_y_n             : chr [1:6890] "N" "N" "UNKNOWN" "UNKNOWN" ...
##  $ time                  : chr [1:6890] "20h00" "07h15" "Afternoon" NA ...
##  $ species               : chr [1:6890] NA "Bronze whaler shark, 1.5 m" "Lemon shark" NA ...
##  $ investigator_or_source: chr [1:6890] "Fox12, 8/1/2023" "The West Australian, 4/22/2023" "Midlibre, 3/18/2023" "El Pais,  2/27/2023" ...
##  $ pdf                   : chr [1:6890] NA NA NA NA ...
##  $ href_formula          : chr [1:6890] NA NA NA NA ...
##  $ href                  : chr [1:6890] NA NA NA NA ...
##  $ case_number_19        : chr [1:6890] NA NA NA NA ...
##  $ case_number_20        : chr [1:6890] NA NA NA NA ...
##  $ original_order        : num [1:6890] NA NA NA NA 6792 ...
##  - attr(*, "spec")=
##   .. cols(
##   ..   Date = col_date(format = ""),
##   ..   Year = col_double(),
##   ..   Type = col_character(),
##   ..   Country = col_character(),
##   ..   Area = col_character(),
##   ..   Location = col_character(),
##   ..   Activity = col_character(),
##   ..   Name = col_character(),
##   ..   Sex = col_character(),
##   ..   Age = col_character(),
##   ..   Injury = col_character(),
##   ..   `Fatal (Y/N)` = col_character(),
##   ..   Time = col_character(),
##   ..   Species = col_character(),
##   ..   `Investigator or Source` = col_character(),
##   ..   pdf = col_character(),
##   ..   `href formula` = col_character(),
##   ..   href = col_character(),
##   ..   `Case Number...19` = col_character(),
##   ..   `Case Number...20` = col_character(),
##   ..   `original order` = col_double()
##   .. )
##  - attr(*, "problems")=<externalptr>
```

2. (3 points) Are there any "hotspots" for shark incidents? Make a plot that shows the total number of incidents for the top 10 countries? Which country has the highest number of incidents?

```r
global_sharks %>% 
  count(country) %>% 
  top_n(10, n) %>% 
  ggplot(aes(x=reorder(country, n), y=n)) +
  geom_col(color="black", fill="pink", alpha=0.5) +
  coord_flip() +
  labs(title="Shark Incidents by Country",
       x=NULL,
       y="# of Incidents") +
  theme(plot.title=element_text(size=12, face="bold"),
        axis.title.x=element_text(size=10),
        axis.title.y=element_text(size=10))
```

![](extra_credit_files/figure-html/unnamed-chunk-5-1.png)<!-- -->


3. (3 points) Are there months of the year when incidents are more likely to occur? Make a plot that shows the total number of incidents by month. Which month has the highest number of incidents?

```r
global_sharks %>% 
  separate(date, into = c("yr", "month", "day"), sep = "-") %>%
  filter(month != "NA") %>% 
  count(month) %>% 
  ggplot(aes(x=factor(month), y=n)) +
  geom_col(color="black", fill="pink", alpha=0.5) +
  labs(title="Number of Shark Incidents by Month",
       x="Month",
       y="# of Incidents") +
  theme(plot.title=element_text(size=12, face="bold"),
        axis.title.x=element_text(size=10),
        axis.title.y=element_text(size=10))
```

![](extra_credit_files/figure-html/unnamed-chunk-6-1.png)<!-- -->


4. (3 points) Which activity is associated with the highest number of incidents? Make a plot that compares the top 5 riskiest activities. "NA" should not be classified as an activity.

```r
global_sharks %>% 
  filter(activity != "NA") %>% 
  count(activity) %>% 
  top_n(5, n) %>% 
  ggplot(aes(x=reorder(activity, n), y=n)) +
  geom_col(color="black", fill="pink", alpha=0.5) +
  labs(title="Number of Shark Incidents by Top Riskiest Activities",
       x="Activity",
       y="# of Incidents") +
  theme(plot.title=element_text(size=12, face="bold"),
        axis.title.x=element_text(size=10),
        axis.title.y=element_text(size=10))
```

![](extra_credit_files/figure-html/unnamed-chunk-7-1.png)<!-- -->


5. (3 points) The data include information on who was attacked. Make a plot that shows the total number of fatalities by sex- are males or females more likely to be killed by sharks?

```r
global_sharks %>% 
  filter(fatal_y_n == "Y") %>% 
  filter(sex == "F" | sex == "M") %>% 
  count(sex) %>% 
  ggplot(aes(x=sex, y=n, fill=sex)) +
  geom_col(color="black", alpha=0.5) +
  labs(title="Fatal Shark Incidents by Sex",
       x=NULL,
       y="# of Incidents",
       fill="Sex") +
  theme(plot.title=element_text(size=12, face="bold"),
        axis.title.x=element_text(size=10),
        axis.title.y=element_text(size=10))
```

![](extra_credit_files/figure-html/unnamed-chunk-8-1.png)<!-- -->


6. (3 points) Make a plot that shows the range of age for the individuals that are attacked. Make sure to restrict sex to M or F (some of the codes used are not clear). You will also need to find a way to manage the messy age column.

```r
sharks_age <- global_sharks %>% 
  filter(sex == "F" | sex == "M") %>% 
  filter(!str_detect(age, "[ABCDEFGHIJKLMNOPQRSTUVWXYZabcdefghijklmnopqrstuvwxyz&'_]")) %>% mutate(age=as.numeric(age))
```

```
## Warning: There was 1 warning in `mutate()`.
## ℹ In argument: `age = as.numeric(age)`.
## Caused by warning:
## ! NAs introduced by coercion
```


```r
sharks_age %>% 
  ggplot(aes(x=sex, y=age, fill=sex)) + 
  geom_boxplot(alpha=0.5) +
  labs(title="Age of Shark Attack Victims",
       x=NULL,
       y="Age",
       fill="Sex") +
  theme(plot.title=element_text(size=12, face="bold"),
        axis.title.x=element_text(size=10),
        axis.title.y=element_text(size=10))
```

```
## Warning: Removed 5 rows containing non-finite values (`stat_boxplot()`).
```

![](extra_credit_files/figure-html/unnamed-chunk-10-1.png)<!-- -->


7. (3 points) In the United States, what are the top 5 states where shark attacks have been recorded since 1950? Make a plot that compares the number of incidents for these 5 states.

```r
global_sharks %>% 
  filter(country == "USA") %>% 
  count(area) %>% 
  top_n(5, n) %>% 
  ggplot(aes(x=reorder(area, n), y=n)) +
  geom_col(color="black", fill="pink", alpha=0.5) +
  labs(title="Number of Shark Incidents by Top 5 States Since 1950",
       x="State",
       y="# of Incidents") +
  theme(plot.title=element_text(size=12, face="bold"),
        axis.title.x=element_text(size=10),
        axis.text.x = element_text(angle = 60, hjust = 1),
        axis.title.y=element_text(size=10))
```

![](extra_credit_files/figure-html/unnamed-chunk-11-1.png)<!-- -->


8. (3 points) Make a new object that limits the data to only include attacks attributed to Great White Sharks. This is trickier than it sounds, you should end up with 494 observations. Look online and adapt code involving `str_detect`. Which country has the highest number of Great White Shark attacks?
USA has the greatest number of White Shark attacks.

```r
global_sharks %>% 
  filter(str_detect(species, "White")) %>% 
  count(country) %>%
  arrange(desc(n))
```

```
## # A tibble: 36 × 2
##    country          n
##    <chr>        <int>
##  1 USA            151
##  2 AUSTRALIA      135
##  3 SOUTH AFRICA   124
##  4 NEW ZEALAND     17
##  5 ITALY           16
##  6 CROATIA          9
##  7 GREECE           4
##  8 CHILE            3
##  9 BAHAMAS          2
## 10 CANADA           2
## # ℹ 26 more rows
```


9. (4 points) Use faceting to compare the number of fatal and non-fatal attacks for the top 5 countries with the highest number of Great White Shark attacks.

```r
global_sharks %>% 
  filter(str_detect(species, "White")) %>% 
  filter(fatal_y_n == "Y" | fatal_y_n == "N") %>% 
  filter(country == "USA" | country == "AUSTRALIA" | country == "SOUTH AFRICA" | country == "NEW ZEALAND" | country == "ITALY") %>% 
  ggplot(aes(x=country, fill=country)) +
  geom_bar(color="black", alpha=0.5) +
  coord_flip() +
  facet_grid(fatal_y_n~.) +
  labs(title="Non-Fatal & Fatal Great Shark Attacks by Top 5 Countries",
       x=NULL,
       y="# of Incidents",
       fill="Country") +
  theme(plot.title=element_text(size=12, face="bold"),
        axis.title.x=element_text(size=10),
        axis.title.y=element_text(size=10))
```

![](extra_credit_files/figure-html/unnamed-chunk-13-1.png)<!-- -->


10. (3 points) Using the `global_sharks` data, what is one question that you are interested in exploring? Write the question and answer it using a plot or table. 
What are the top 10 states where shark attacks have been recorded since 1950?

```r
global_sharks %>% 
  filter(country == "USA") %>% 
  count(area) %>% 
  top_n(10, n) %>% 
  ggplot(aes(x=reorder(area, n), y=n)) +
  geom_col(color="black", fill="pink", alpha=0.5) +
  labs(title="Number of Shark Incidents by Top 10 States",
       x="State",
       y="# of Incidents") +
  theme(plot.title=element_text(size=12, face="bold"),
        axis.title.x=element_text(size=10),
        axis.text.x = element_text(angle = 60, hjust = 1),
        axis.title.y=element_text(size=10))
```

![](extra_credit_files/figure-html/unnamed-chunk-14-1.png)<!-- -->

