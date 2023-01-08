---
title: "Palmer Penguins Initial Analysis"
author: "Trevor Harrington"
format: html
editor: visual
keep-md: TRUE
---



## Palmer Penguins

Some Informative blurb


::: {.cell}

```{.r .cell-code}
library(tidyverse) #load the tidyverse
```

::: {.cell-output .cell-output-stderr}
```
── Attaching packages ─────────────────────────────────────── tidyverse 1.3.2 ──
✔ ggplot2 3.4.0      ✔ purrr   1.0.0 
✔ tibble  3.1.8      ✔ dplyr   1.0.10
✔ tidyr   1.2.1      ✔ stringr 1.5.0 
✔ readr   2.1.3      ✔ forcats 0.5.2 
── Conflicts ────────────────────────────────────────── tidyverse_conflicts() ──
✖ dplyr::filter() masks stats::filter()
✖ dplyr::lag()    masks stats::lag()
```
:::

```{.r .cell-code}
#Read the penguines_sampl data off GITHub

penguins<- read_csv("https://raw.githubusercontent.com/mcduryea/Intro-to-Bioinformatics/main/data/penguins_samp1.csv")
```

::: {.cell-output .cell-output-stderr}
```
Rows: 44 Columns: 8
── Column specification ────────────────────────────────────────────────────────
Delimiter: ","
chr (3): species, island, sex
dbl (5): bill_length_mm, bill_depth_mm, flipper_length_mm, body_mass_g, year

ℹ Use `spec()` to retrieve the full column specification for this data.
ℹ Specify the column types or set `show_col_types = FALSE` to quiet this message.
```
:::

```{.r .cell-code}
#See the first six rows of data added to notebook

penguins %>% head()
```

::: {.cell-output .cell-output-stdout}
```
# A tibble: 6 × 8
  species island bill_length_mm bill_depth_mm flipper_leng…¹ body_…² sex    year
  <chr>   <chr>           <dbl>         <dbl>          <dbl>   <dbl> <chr> <dbl>
1 Gentoo  Biscoe           59.6          17              230    6050 male   2007
2 Gentoo  Biscoe           48.6          16              230    5800 male   2008
3 Gentoo  Biscoe           52.1          17              230    5550 male   2009
4 Gentoo  Biscoe           51.5          16.3            230    5500 male   2009
5 Gentoo  Biscoe           55.1          16              230    5850 male   2009
6 Gentoo  Biscoe           49.8          15.9            229    5950 male   2009
# … with abbreviated variable names ¹​flipper_length_mm, ²​body_mass_g
```
:::
:::


A data set containing general characteristics from a sample of the Gentoo penguin population on Biscoe Island.

## Data Manipulation

This section covers techniques for filtering rows, subsetting columns, grouping data, and computing summary statistics in R. Filtering rows allows you to select only those rows that meet certain criteria, while subsetting columns allows you to select only a subset of the available columns. R allows for these functions, as well as dividing the data set into groups based on common characteristics that can create new ways of interpreting information. Using Computing to create summary statistics involves calculating statistical measures such as mean, median, and standard deviation for each group. Understanding how to use R to accomplish this analysis is crucial for grouping and summarizing large quantities of data, which can be combined to answer complex questions about the whole set.

**Questions to consider about Penguins**

-   How does the body mass of different penguin species compare?

-   What is the distribution of beak length and depth among penguin species?

-   How does flipper size vary among different penguin species?

-   Is there a correlation between flipper size and body mass in penguins?

-   How does flipper size vary within a single penguin species over time?

-   How does flipper size differ between male and female penguins of the same species?

-   How does flipper size relate to the preferred habitat of a penguin species (is it larger in species that inhabit colder environments)?

penguins %>%
count(species,island) %>%
pivot_wider(names_from = species, values_from = n,values_fill = 0)
