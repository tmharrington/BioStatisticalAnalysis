---
title: "Indoor Air Analysis"
author: "Trevor Harrington"
format: html
editor: visual
keep-md: TRUE
prefer-html: true
---



# Air Pollution and Global GDP/Climate trends

## Introduction

## determining hypothesis with testing data 


::: {.cell}

```{.r .cell-code}
library(tidyverse)
```

::: {.cell-output .cell-output-stderr}
```
── Attaching packages ─────────────────────────────────────── tidyverse 1.3.2 ──
✔ ggplot2 3.4.0     ✔ purrr   1.0.1
✔ tibble  3.1.8     ✔ dplyr   1.1.0
✔ tidyr   1.3.0     ✔ stringr 1.5.0
✔ readr   2.1.3     ✔ forcats 1.0.0
── Conflicts ────────────────────────────────────────── tidyverse_conflicts() ──
✖ dplyr::filter() masks stats::filter()
✖ dplyr::lag()    masks stats::lag()
```
:::

```{.r .cell-code}
library(kableExtra)
```

::: {.cell-output .cell-output-stderr}
```

Attaching package: 'kableExtra'

The following object is masked from 'package:dplyr':

    group_rows
```
:::

```{.r .cell-code}
library(ggplot2)
library(tidymodels)
```

::: {.cell-output .cell-output-stderr}
```
── Attaching packages ────────────────────────────────────── tidymodels 1.0.0 ──
✔ broom        1.0.3     ✔ rsample      1.1.1
✔ dials        1.1.0     ✔ tune         1.0.1
✔ infer        1.0.4     ✔ workflows    1.1.2
✔ modeldata    1.1.0     ✔ workflowsets 1.0.0
✔ parsnip      1.0.3     ✔ yardstick    1.1.0
✔ recipes      1.0.4     
── Conflicts ───────────────────────────────────────── tidymodels_conflicts() ──
✖ scales::discard()        masks purrr::discard()
✖ dplyr::filter()          masks stats::filter()
✖ recipes::fixed()         masks stringr::fixed()
✖ kableExtra::group_rows() masks dplyr::group_rows()
✖ dplyr::lag()             masks stats::lag()
✖ yardstick::spec()        masks readr::spec()
✖ recipes::step()          masks stats::step()
• Search for functions across packages at https://www.tidymodels.org/find/
```
:::

```{.r .cell-code}
library(janitor)
```

::: {.cell-output .cell-output-stderr}
```

Attaching package: 'janitor'

The following objects are masked from 'package:stats':

    chisq.test, fisher.test
```
:::

```{.r .cell-code}
library(magrittr)
```

::: {.cell-output .cell-output-stderr}
```

Attaching package: 'magrittr'

The following object is masked from 'package:purrr':

    set_names

The following object is masked from 'package:tidyr':

    extract
```
:::

```{.r .cell-code}
 library(dplyr)

indoor_pollution <- readr::read_csv('https://raw.githubusercontent.com/rfordatascience/tidytuesday/master/data/2022/2022-04-12/indoor_pollution.csv') %>%

clean_names()
```

::: {.cell-output .cell-output-stderr}
```
Rows: 8010 Columns: 4
── Column specification ────────────────────────────────────────────────────────
Delimiter: ","
chr (2): Entity, Code
dbl (2): Year, Deaths - Cause: All causes - Risk: Household air pollution fr...

ℹ Use `spec()` to retrieve the full column specification for this data.
ℹ Specify the column types or set `show_col_types = FALSE` to quiet this message.
```
:::
:::

::: {.cell}

```{.r .cell-code}
 iap_splits <- initial_split(indoor_pollution, prop = 0.15,
                                   pool = 1) 

exploratory_data <- training(iap_splits)
test_data <- testing(iap_splits)

head(exploratory_data) %>%
  
  rename(percent_deaths_by_household_pollution = deaths_cause_all_causes_risk_household_air_pollution_from_solid_fuels_sex_both_age_age_standardized_percent) %>% #shorten the long name 
  
kable(digits = c(1,0,0,4)) %>%
    kable_styling(bootstrap_options = c("hover", "striped"))
```

::: {.cell-output-display}

`````{=html}
<table class="table table-hover table-striped" style="margin-left: auto; margin-right: auto;">
 <thead>
  <tr>
   <th style="text-align:left;"> entity </th>
   <th style="text-align:left;"> code </th>
   <th style="text-align:right;"> year </th>
   <th style="text-align:right;"> percent_deaths_by_household_pollution </th>
  </tr>
 </thead>
<tbody>
  <tr>
   <td style="text-align:left;"> African Region </td>
   <td style="text-align:left;"> NA </td>
   <td style="text-align:right;"> 2007 </td>
   <td style="text-align:right;"> 9.6302 </td>
  </tr>
  <tr>
   <td style="text-align:left;"> Ecuador </td>
   <td style="text-align:left;"> ECU </td>
   <td style="text-align:right;"> 1995 </td>
   <td style="text-align:right;"> 2.7964 </td>
  </tr>
  <tr>
   <td style="text-align:left;"> Mozambique </td>
   <td style="text-align:left;"> MOZ </td>
   <td style="text-align:right;"> 2011 </td>
   <td style="text-align:right;"> 11.3990 </td>
  </tr>
  <tr>
   <td style="text-align:left;"> Sudan </td>
   <td style="text-align:left;"> SDN </td>
   <td style="text-align:right;"> 2001 </td>
   <td style="text-align:right;"> 14.8504 </td>
  </tr>
  <tr>
   <td style="text-align:left;"> Uruguay </td>
   <td style="text-align:left;"> URY </td>
   <td style="text-align:right;"> 1998 </td>
   <td style="text-align:right;"> 0.6097 </td>
  </tr>
  <tr>
   <td style="text-align:left;"> South Africa </td>
   <td style="text-align:left;"> ZAF </td>
   <td style="text-align:right;"> 2010 </td>
   <td style="text-align:right;"> 1.7999 </td>
  </tr>
</tbody>
</table>

`````

:::
:::


::: callout-note
**Observation:**

This data set contains variables gathered across multiple years, listing the percent of deaths caused by indoor air pollutants created from indoor combustion. Given the amount of data present in this dataset, several potential hypotheses could likely be considered.

The data, listed by country and region, are mixed in the dataset and will likely require some cleaning to separate. categorical data and contains variables that were collected over many years.

-   Depending on the scope of research, it may be more useful to focus on the dataset containing regional data rather than country-specific to highlight what role climate plays in the effect of indoor air pollution.

    -   In temperate climates, it is more likely to use biofuels for heating as well as cooking. Alongside the availability of insulation material for homes that reduced the ability for indoor and outdoor atmosphere gas exchange, this may be an essential factor that could be compared with this data.

    -   Using the temporal variable that is available in this data, some conclusions could be made about the

-   focusing on the countries rather than regions in this data could be used to show how countries that have invested in new technologies or better homes could correlate with either improved or worsened indoor air quality. Suppose homes are made more insulated but old gas stoves and wood fires are built into them. In that case, it is possible that the data could show up to a point a worsened impact on indoor air quality is associated with a country's growth in economic opportunity.
:::


::: {.cell}

```{.r .cell-code}
exploratory_data %>% 
  
  summarize(
  "Lowest percentage of deaths" = min(deaths_cause_all_causes_risk_household_air_pollution_from_solid_fuels_sex_both_age_age_standardized_percent),
  
  "Highest percentage of deaths"= max(deaths_cause_all_causes_risk_household_air_pollution_from_solid_fuels_sex_both_age_age_standardized_percent),
  "average percentage of deaths" =mean(deaths_cause_all_causes_risk_household_air_pollution_from_solid_fuels_sex_both_age_age_standardized_percent),
  "Standard deviation" = sd(deaths_cause_all_causes_risk_household_air_pollution_from_solid_fuels_sex_both_age_age_standardized_percent)
) %>%
  pivot_longer(cols = 1:4) %>%
  
  kable(digits = c(1,3)) %>%
    kable_styling(bootstrap_options = c("hover", "striped"))
```

::: {.cell-output-display}

`````{=html}
<table class="table table-hover table-striped" style="margin-left: auto; margin-right: auto;">
 <thead>
  <tr>
   <th style="text-align:left;"> name </th>
   <th style="text-align:right;"> value </th>
  </tr>
 </thead>
<tbody>
  <tr>
   <td style="text-align:left;"> Lowest percentage of deaths </td>
   <td style="text-align:right;"> 0.001 </td>
  </tr>
  <tr>
   <td style="text-align:left;"> Highest percentage of deaths </td>
   <td style="text-align:right;"> 23.205 </td>
  </tr>
  <tr>
   <td style="text-align:left;"> average percentage of deaths </td>
   <td style="text-align:right;"> 5.117 </td>
  </tr>
  <tr>
   <td style="text-align:left;"> Standard deviation </td>
   <td style="text-align:right;"> 5.436 </td>
  </tr>
</tbody>
</table>

`````

:::
:::


::: callout-note
**Observations:**

-   0.0016 % deaths is extremely low, especially compared to the maximum at 23%, and even the average of 5.57 shows a significant spread across datapoints this broad range suggests

    -   This data does not provide particularly useful information since the data is not group by year. Potentially plotting the mean percentage across time could provide a useful visual to see how this data changes.

-   the maximum deaths at 23.04% is extremely high, and I am very curious at which country/region was measured this high-- potentially an outlier?

-   5.77 standard deviation seems very large for the size of the data set being considered. This will be interesting to reference when seperating by country, region or year.
:::


::: {.cell}

```{.r .cell-code}
library(countrycode)

exploratory_data %>% 
  
   rename(percent_deaths_by_household_pollution = deaths_cause_all_causes_risk_household_air_pollution_from_solid_fuels_sex_both_age_age_standardized_percent) %>% #shorten the long name 
  
   mutate(country_region = countrycode(entity, origin = "country.name", destination = "region")) %>%
  
  mutate(continent = countrycode (entity, origin = "country.name", destination = "continent"))%>%
  
   filter(continent!= 0) %>%
  
  kable() %>%
    kable_styling(bootstrap_options = c("hover", "striped"))
```

::: {.cell-output .cell-output-stderr}
```
Warning: There was 1 warning in `mutate()`.
ℹ In argument: `country_region = countrycode(entity, origin = "country.name",
  destination = "region")`.
Caused by warning in `countrycode_convert()`:
! Some values were not matched unambiguously: Africa, African Region, African Union, America, Andean Latin America, Asia, Australasia, Caribbean, Central Asia, Central Europe, Central Europe, Eastern Europe, and Central Asia, Central Latin America, Central sub-Saharan Africa, Commonwealth, Commonwealth High Income, Commonwealth Low Income, Commonwealth Middle Income, East Asia, East Asia & Pacific - World Bank region, Eastern Europe, Eastern Mediterranean Region, Eastern sub-Saharan Africa, England, Europe, Europe & Central Asia - World Bank region, European Region, European Union, G20, High-income, High-income Asia Pacific, High-income North America, High-middle SDI, High SDI, Latin America & Caribbean - World Bank region, Low-middle SDI, Low SDI, Micronesia (country), Middle East & North Africa, Middle SDI, Nordic Region, North Africa and Middle East, North America, Northern Ireland, Oceania, OECD Countries, Region of the Americas, Scotland, South-East Asia Region, South Asia - World Bank region, Southeast Asia, Southeast Asia, East Asia, and Oceania, Southern Latin America, Southern sub-Saharan Africa, Sub-Saharan Africa - World Bank region, Timor, Tropical Latin America, Wales, Western Europe, Western Pacific Region, Western sub-Saharan Africa, World, World Bank High Income, World Bank Low Income, World Bank Lower Middle Income, World Bank Upper Middle Income
```
:::

::: {.cell-output .cell-output-stderr}
```
Warning: There was 1 warning in `mutate()`.
ℹ In argument: `continent = countrycode(entity, origin = "country.name",
  destination = "continent")`.
Caused by warning in `countrycode_convert()`:
! Some values were not matched unambiguously: Africa, African Region, African Union, America, Andean Latin America, Asia, Australasia, Caribbean, Central Asia, Central Europe, Central Europe, Eastern Europe, and Central Asia, Central Latin America, Central sub-Saharan Africa, Commonwealth, Commonwealth High Income, Commonwealth Low Income, Commonwealth Middle Income, East Asia, East Asia & Pacific - World Bank region, Eastern Europe, Eastern Mediterranean Region, Eastern sub-Saharan Africa, England, Europe, Europe & Central Asia - World Bank region, European Region, European Union, G20, High-income, High-income Asia Pacific, High-income North America, High-middle SDI, High SDI, Latin America & Caribbean - World Bank region, Low-middle SDI, Low SDI, Micronesia (country), Middle East & North Africa, Middle SDI, Nordic Region, North Africa and Middle East, North America, Northern Ireland, Oceania, OECD Countries, Region of the Americas, Scotland, South-East Asia Region, South Asia - World Bank region, Southeast Asia, Southeast Asia, East Asia, and Oceania, Southern Latin America, Southern sub-Saharan Africa, Sub-Saharan Africa - World Bank region, Timor, Tropical Latin America, Wales, Western Europe, Western Pacific Region, Western sub-Saharan Africa, World, World Bank High Income, World Bank Low Income, World Bank Lower Middle Income, World Bank Upper Middle Income
```
:::

::: {.cell-output-display}

`````{=html}
<table class="table table-hover table-striped" style="margin-left: auto; margin-right: auto;">
 <thead>
  <tr>
   <th style="text-align:left;"> entity </th>
   <th style="text-align:left;"> code </th>
   <th style="text-align:right;"> year </th>
   <th style="text-align:right;"> percent_deaths_by_household_pollution </th>
   <th style="text-align:left;"> country_region </th>
   <th style="text-align:left;"> continent </th>
  </tr>
 </thead>
<tbody>
  <tr>
   <td style="text-align:left;"> Ecuador </td>
   <td style="text-align:left;"> ECU </td>
   <td style="text-align:right;"> 1995 </td>
   <td style="text-align:right;"> 2.7963905 </td>
   <td style="text-align:left;"> Latin America &amp; Caribbean </td>
   <td style="text-align:left;"> Americas </td>
  </tr>
  <tr>
   <td style="text-align:left;"> Mozambique </td>
   <td style="text-align:left;"> MOZ </td>
   <td style="text-align:right;"> 2011 </td>
   <td style="text-align:right;"> 11.3990155 </td>
   <td style="text-align:left;"> Sub-Saharan Africa </td>
   <td style="text-align:left;"> Africa </td>
  </tr>
  <tr>
   <td style="text-align:left;"> Sudan </td>
   <td style="text-align:left;"> SDN </td>
   <td style="text-align:right;"> 2001 </td>
   <td style="text-align:right;"> 14.8504188 </td>
   <td style="text-align:left;"> Sub-Saharan Africa </td>
   <td style="text-align:left;"> Africa </td>
  </tr>
  <tr>
   <td style="text-align:left;"> Uruguay </td>
   <td style="text-align:left;"> URY </td>
   <td style="text-align:right;"> 1998 </td>
   <td style="text-align:right;"> 0.6097369 </td>
   <td style="text-align:left;"> Latin America &amp; Caribbean </td>
   <td style="text-align:left;"> Americas </td>
  </tr>
  <tr>
   <td style="text-align:left;"> South Africa </td>
   <td style="text-align:left;"> ZAF </td>
   <td style="text-align:right;"> 2010 </td>
   <td style="text-align:right;"> 1.7998612 </td>
   <td style="text-align:left;"> Sub-Saharan Africa </td>
   <td style="text-align:left;"> Africa </td>
  </tr>
  <tr>
   <td style="text-align:left;"> Bhutan </td>
   <td style="text-align:left;"> BTN </td>
   <td style="text-align:right;"> 2002 </td>
   <td style="text-align:right;"> 15.7426039 </td>
   <td style="text-align:left;"> South Asia </td>
   <td style="text-align:left;"> Asia </td>
  </tr>
  <tr>
   <td style="text-align:left;"> Liberia </td>
   <td style="text-align:left;"> LBR </td>
   <td style="text-align:right;"> 2010 </td>
   <td style="text-align:right;"> 12.0870666 </td>
   <td style="text-align:left;"> Sub-Saharan Africa </td>
   <td style="text-align:left;"> Africa </td>
  </tr>
  <tr>
   <td style="text-align:left;"> Ireland </td>
   <td style="text-align:left;"> IRL </td>
   <td style="text-align:right;"> 2002 </td>
   <td style="text-align:right;"> 0.0258116 </td>
   <td style="text-align:left;"> Europe &amp; Central Asia </td>
   <td style="text-align:left;"> Europe </td>
  </tr>
  <tr>
   <td style="text-align:left;"> Saint Lucia </td>
   <td style="text-align:left;"> LCA </td>
   <td style="text-align:right;"> 2003 </td>
   <td style="text-align:right;"> 1.5223194 </td>
   <td style="text-align:left;"> Latin America &amp; Caribbean </td>
   <td style="text-align:left;"> Americas </td>
  </tr>
  <tr>
   <td style="text-align:left;"> South Africa </td>
   <td style="text-align:left;"> ZAF </td>
   <td style="text-align:right;"> 2006 </td>
   <td style="text-align:right;"> 2.2728153 </td>
   <td style="text-align:left;"> Sub-Saharan Africa </td>
   <td style="text-align:left;"> Africa </td>
  </tr>
  <tr>
   <td style="text-align:left;"> Guinea-Bissau </td>
   <td style="text-align:left;"> GNB </td>
   <td style="text-align:right;"> 2019 </td>
   <td style="text-align:right;"> 13.3988124 </td>
   <td style="text-align:left;"> Sub-Saharan Africa </td>
   <td style="text-align:left;"> Africa </td>
  </tr>
  <tr>
   <td style="text-align:left;"> Lebanon </td>
   <td style="text-align:left;"> LBN </td>
   <td style="text-align:right;"> 2013 </td>
   <td style="text-align:right;"> 0.0643805 </td>
   <td style="text-align:left;"> Middle East &amp; North Africa </td>
   <td style="text-align:left;"> Asia </td>
  </tr>
  <tr>
   <td style="text-align:left;"> Puerto Rico </td>
   <td style="text-align:left;"> PRI </td>
   <td style="text-align:right;"> 2010 </td>
   <td style="text-align:right;"> 0.0031596 </td>
   <td style="text-align:left;"> Latin America &amp; Caribbean </td>
   <td style="text-align:left;"> Americas </td>
  </tr>
  <tr>
   <td style="text-align:left;"> Egypt </td>
   <td style="text-align:left;"> EGY </td>
   <td style="text-align:right;"> 2014 </td>
   <td style="text-align:right;"> 0.0278133 </td>
   <td style="text-align:left;"> Middle East &amp; North Africa </td>
   <td style="text-align:left;"> Africa </td>
  </tr>
  <tr>
   <td style="text-align:left;"> Vanuatu </td>
   <td style="text-align:left;"> VUT </td>
   <td style="text-align:right;"> 2007 </td>
   <td style="text-align:right;"> 16.7207276 </td>
   <td style="text-align:left;"> East Asia &amp; Pacific </td>
   <td style="text-align:left;"> Oceania </td>
  </tr>
  <tr>
   <td style="text-align:left;"> San Marino </td>
   <td style="text-align:left;"> SMR </td>
   <td style="text-align:right;"> 1997 </td>
   <td style="text-align:right;"> 0.0176664 </td>
   <td style="text-align:left;"> Europe &amp; Central Asia </td>
   <td style="text-align:left;"> Europe </td>
  </tr>
  <tr>
   <td style="text-align:left;"> Armenia </td>
   <td style="text-align:left;"> ARM </td>
   <td style="text-align:right;"> 2016 </td>
   <td style="text-align:right;"> 0.3337881 </td>
   <td style="text-align:left;"> Europe &amp; Central Asia </td>
   <td style="text-align:left;"> Asia </td>
  </tr>
  <tr>
   <td style="text-align:left;"> Thailand </td>
   <td style="text-align:left;"> THA </td>
   <td style="text-align:right;"> 2007 </td>
   <td style="text-align:right;"> 3.1333328 </td>
   <td style="text-align:left;"> East Asia &amp; Pacific </td>
   <td style="text-align:left;"> Asia </td>
  </tr>
  <tr>
   <td style="text-align:left;"> Philippines </td>
   <td style="text-align:left;"> PHL </td>
   <td style="text-align:right;"> 2011 </td>
   <td style="text-align:right;"> 10.2440497 </td>
   <td style="text-align:left;"> East Asia &amp; Pacific </td>
   <td style="text-align:left;"> Asia </td>
  </tr>
  <tr>
   <td style="text-align:left;"> Uzbekistan </td>
   <td style="text-align:left;"> UZB </td>
   <td style="text-align:right;"> 1996 </td>
   <td style="text-align:right;"> 5.9958917 </td>
   <td style="text-align:left;"> Europe &amp; Central Asia </td>
   <td style="text-align:left;"> Asia </td>
  </tr>
  <tr>
   <td style="text-align:left;"> Gambia </td>
   <td style="text-align:left;"> GMB </td>
   <td style="text-align:right;"> 2019 </td>
   <td style="text-align:right;"> 12.3082649 </td>
   <td style="text-align:left;"> Sub-Saharan Africa </td>
   <td style="text-align:left;"> Africa </td>
  </tr>
  <tr>
   <td style="text-align:left;"> Denmark </td>
   <td style="text-align:left;"> DNK </td>
   <td style="text-align:right;"> 1997 </td>
   <td style="text-align:right;"> 0.0136523 </td>
   <td style="text-align:left;"> Europe &amp; Central Asia </td>
   <td style="text-align:left;"> Europe </td>
  </tr>
  <tr>
   <td style="text-align:left;"> Fiji </td>
   <td style="text-align:left;"> FJI </td>
   <td style="text-align:right;"> 1997 </td>
   <td style="text-align:right;"> 8.7399938 </td>
   <td style="text-align:left;"> East Asia &amp; Pacific </td>
   <td style="text-align:left;"> Oceania </td>
  </tr>
  <tr>
   <td style="text-align:left;"> Saint Lucia </td>
   <td style="text-align:left;"> LCA </td>
   <td style="text-align:right;"> 2009 </td>
   <td style="text-align:right;"> 0.7296562 </td>
   <td style="text-align:left;"> Latin America &amp; Caribbean </td>
   <td style="text-align:left;"> Americas </td>
  </tr>
  <tr>
   <td style="text-align:left;"> Kuwait </td>
   <td style="text-align:left;"> KWT </td>
   <td style="text-align:right;"> 2006 </td>
   <td style="text-align:right;"> 0.0252485 </td>
   <td style="text-align:left;"> Middle East &amp; North Africa </td>
   <td style="text-align:left;"> Asia </td>
  </tr>
  <tr>
   <td style="text-align:left;"> Lithuania </td>
   <td style="text-align:left;"> LTU </td>
   <td style="text-align:right;"> 2007 </td>
   <td style="text-align:right;"> 0.3431521 </td>
   <td style="text-align:left;"> Europe &amp; Central Asia </td>
   <td style="text-align:left;"> Europe </td>
  </tr>
  <tr>
   <td style="text-align:left;"> Philippines </td>
   <td style="text-align:left;"> PHL </td>
   <td style="text-align:right;"> 2018 </td>
   <td style="text-align:right;"> 7.3628956 </td>
   <td style="text-align:left;"> East Asia &amp; Pacific </td>
   <td style="text-align:left;"> Asia </td>
  </tr>
  <tr>
   <td style="text-align:left;"> Denmark </td>
   <td style="text-align:left;"> DNK </td>
   <td style="text-align:right;"> 2019 </td>
   <td style="text-align:right;"> 0.0036142 </td>
   <td style="text-align:left;"> Europe &amp; Central Asia </td>
   <td style="text-align:left;"> Europe </td>
  </tr>
  <tr>
   <td style="text-align:left;"> Argentina </td>
   <td style="text-align:left;"> ARG </td>
   <td style="text-align:right;"> 2013 </td>
   <td style="text-align:right;"> 0.2891292 </td>
   <td style="text-align:left;"> Latin America &amp; Caribbean </td>
   <td style="text-align:left;"> Americas </td>
  </tr>
  <tr>
   <td style="text-align:left;"> Comoros </td>
   <td style="text-align:left;"> COM </td>
   <td style="text-align:right;"> 2019 </td>
   <td style="text-align:right;"> 11.2698873 </td>
   <td style="text-align:left;"> Sub-Saharan Africa </td>
   <td style="text-align:left;"> Africa </td>
  </tr>
  <tr>
   <td style="text-align:left;"> Egypt </td>
   <td style="text-align:left;"> EGY </td>
   <td style="text-align:right;"> 2008 </td>
   <td style="text-align:right;"> 0.0701894 </td>
   <td style="text-align:left;"> Middle East &amp; North Africa </td>
   <td style="text-align:left;"> Africa </td>
  </tr>
  <tr>
   <td style="text-align:left;"> Belize </td>
   <td style="text-align:left;"> BLZ </td>
   <td style="text-align:right;"> 2012 </td>
   <td style="text-align:right;"> 2.3020779 </td>
   <td style="text-align:left;"> Latin America &amp; Caribbean </td>
   <td style="text-align:left;"> Americas </td>
  </tr>
  <tr>
   <td style="text-align:left;"> Japan </td>
   <td style="text-align:left;"> JPN </td>
   <td style="text-align:right;"> 2015 </td>
   <td style="text-align:right;"> 0.0081105 </td>
   <td style="text-align:left;"> East Asia &amp; Pacific </td>
   <td style="text-align:left;"> Asia </td>
  </tr>
  <tr>
   <td style="text-align:left;"> Canada </td>
   <td style="text-align:left;"> CAN </td>
   <td style="text-align:right;"> 2013 </td>
   <td style="text-align:right;"> 0.0038294 </td>
   <td style="text-align:left;"> North America </td>
   <td style="text-align:left;"> Americas </td>
  </tr>
  <tr>
   <td style="text-align:left;"> Niger </td>
   <td style="text-align:left;"> NER </td>
   <td style="text-align:right;"> 2016 </td>
   <td style="text-align:right;"> 15.7382443 </td>
   <td style="text-align:left;"> Sub-Saharan Africa </td>
   <td style="text-align:left;"> Africa </td>
  </tr>
  <tr>
   <td style="text-align:left;"> Mauritania </td>
   <td style="text-align:left;"> MRT </td>
   <td style="text-align:right;"> 2014 </td>
   <td style="text-align:right;"> 7.4563630 </td>
   <td style="text-align:left;"> Sub-Saharan Africa </td>
   <td style="text-align:left;"> Africa </td>
  </tr>
  <tr>
   <td style="text-align:left;"> Mozambique </td>
   <td style="text-align:left;"> MOZ </td>
   <td style="text-align:right;"> 2000 </td>
   <td style="text-align:right;"> 11.8076598 </td>
   <td style="text-align:left;"> Sub-Saharan Africa </td>
   <td style="text-align:left;"> Africa </td>
  </tr>
  <tr>
   <td style="text-align:left;"> Peru </td>
   <td style="text-align:left;"> PER </td>
   <td style="text-align:right;"> 2006 </td>
   <td style="text-align:right;"> 4.1324464 </td>
   <td style="text-align:left;"> Latin America &amp; Caribbean </td>
   <td style="text-align:left;"> Americas </td>
  </tr>
  <tr>
   <td style="text-align:left;"> Iceland </td>
   <td style="text-align:left;"> ISL </td>
   <td style="text-align:right;"> 2012 </td>
   <td style="text-align:right;"> 0.0053915 </td>
   <td style="text-align:left;"> Europe &amp; Central Asia </td>
   <td style="text-align:left;"> Europe </td>
  </tr>
  <tr>
   <td style="text-align:left;"> Israel </td>
   <td style="text-align:left;"> ISR </td>
   <td style="text-align:right;"> 2007 </td>
   <td style="text-align:right;"> 0.0138260 </td>
   <td style="text-align:left;"> Middle East &amp; North Africa </td>
   <td style="text-align:left;"> Asia </td>
  </tr>
  <tr>
   <td style="text-align:left;"> France </td>
   <td style="text-align:left;"> FRA </td>
   <td style="text-align:right;"> 1990 </td>
   <td style="text-align:right;"> 0.0401628 </td>
   <td style="text-align:left;"> Europe &amp; Central Asia </td>
   <td style="text-align:left;"> Europe </td>
  </tr>
  <tr>
   <td style="text-align:left;"> Saint Lucia </td>
   <td style="text-align:left;"> LCA </td>
   <td style="text-align:right;"> 1999 </td>
   <td style="text-align:right;"> 2.2239770 </td>
   <td style="text-align:left;"> Latin America &amp; Caribbean </td>
   <td style="text-align:left;"> Americas </td>
  </tr>
  <tr>
   <td style="text-align:left;"> Libya </td>
   <td style="text-align:left;"> LBY </td>
   <td style="text-align:right;"> 1999 </td>
   <td style="text-align:right;"> 0.5720146 </td>
   <td style="text-align:left;"> Middle East &amp; North Africa </td>
   <td style="text-align:left;"> Africa </td>
  </tr>
  <tr>
   <td style="text-align:left;"> Antigua and Barbuda </td>
   <td style="text-align:left;"> ATG </td>
   <td style="text-align:right;"> 1997 </td>
   <td style="text-align:right;"> 0.3356916 </td>
   <td style="text-align:left;"> Latin America &amp; Caribbean </td>
   <td style="text-align:left;"> Americas </td>
  </tr>
  <tr>
   <td style="text-align:left;"> Serbia </td>
   <td style="text-align:left;"> SRB </td>
   <td style="text-align:right;"> 2018 </td>
   <td style="text-align:right;"> 1.6376230 </td>
   <td style="text-align:left;"> Europe &amp; Central Asia </td>
   <td style="text-align:left;"> Europe </td>
  </tr>
  <tr>
   <td style="text-align:left;"> Nauru </td>
   <td style="text-align:left;"> NRU </td>
   <td style="text-align:right;"> 2009 </td>
   <td style="text-align:right;"> 1.5659575 </td>
   <td style="text-align:left;"> East Asia &amp; Pacific </td>
   <td style="text-align:left;"> Oceania </td>
  </tr>
  <tr>
   <td style="text-align:left;"> Equatorial Guinea </td>
   <td style="text-align:left;"> GNQ </td>
   <td style="text-align:right;"> 2009 </td>
   <td style="text-align:right;"> 4.8877087 </td>
   <td style="text-align:left;"> Sub-Saharan Africa </td>
   <td style="text-align:left;"> Africa </td>
  </tr>
  <tr>
   <td style="text-align:left;"> Bahamas </td>
   <td style="text-align:left;"> BHS </td>
   <td style="text-align:right;"> 2019 </td>
   <td style="text-align:right;"> 0.0772799 </td>
   <td style="text-align:left;"> Latin America &amp; Caribbean </td>
   <td style="text-align:left;"> Americas </td>
  </tr>
  <tr>
   <td style="text-align:left;"> Slovenia </td>
   <td style="text-align:left;"> SVN </td>
   <td style="text-align:right;"> 1993 </td>
   <td style="text-align:right;"> 1.2396426 </td>
   <td style="text-align:left;"> Europe &amp; Central Asia </td>
   <td style="text-align:left;"> Europe </td>
  </tr>
  <tr>
   <td style="text-align:left;"> Ethiopia </td>
   <td style="text-align:left;"> ETH </td>
   <td style="text-align:right;"> 2016 </td>
   <td style="text-align:right;"> 13.2988524 </td>
   <td style="text-align:left;"> Sub-Saharan Africa </td>
   <td style="text-align:left;"> Africa </td>
  </tr>
  <tr>
   <td style="text-align:left;"> Austria </td>
   <td style="text-align:left;"> AUT </td>
   <td style="text-align:right;"> 1994 </td>
   <td style="text-align:right;"> 0.0471325 </td>
   <td style="text-align:left;"> Europe &amp; Central Asia </td>
   <td style="text-align:left;"> Europe </td>
  </tr>
  <tr>
   <td style="text-align:left;"> Estonia </td>
   <td style="text-align:left;"> EST </td>
   <td style="text-align:right;"> 2010 </td>
   <td style="text-align:right;"> 0.5045086 </td>
   <td style="text-align:left;"> Europe &amp; Central Asia </td>
   <td style="text-align:left;"> Europe </td>
  </tr>
  <tr>
   <td style="text-align:left;"> Denmark </td>
   <td style="text-align:left;"> DNK </td>
   <td style="text-align:right;"> 2017 </td>
   <td style="text-align:right;"> 0.0040826 </td>
   <td style="text-align:left;"> Europe &amp; Central Asia </td>
   <td style="text-align:left;"> Europe </td>
  </tr>
  <tr>
   <td style="text-align:left;"> Costa Rica </td>
   <td style="text-align:left;"> CRI </td>
   <td style="text-align:right;"> 2000 </td>
   <td style="text-align:right;"> 1.6165632 </td>
   <td style="text-align:left;"> Latin America &amp; Caribbean </td>
   <td style="text-align:left;"> Americas </td>
  </tr>
  <tr>
   <td style="text-align:left;"> Lithuania </td>
   <td style="text-align:left;"> LTU </td>
   <td style="text-align:right;"> 1998 </td>
   <td style="text-align:right;"> 0.6909350 </td>
   <td style="text-align:left;"> Europe &amp; Central Asia </td>
   <td style="text-align:left;"> Europe </td>
  </tr>
  <tr>
   <td style="text-align:left;"> Puerto Rico </td>
   <td style="text-align:left;"> PRI </td>
   <td style="text-align:right;"> 2012 </td>
   <td style="text-align:right;"> 0.0028757 </td>
   <td style="text-align:left;"> Latin America &amp; Caribbean </td>
   <td style="text-align:left;"> Americas </td>
  </tr>
  <tr>
   <td style="text-align:left;"> Equatorial Guinea </td>
   <td style="text-align:left;"> GNQ </td>
   <td style="text-align:right;"> 2014 </td>
   <td style="text-align:right;"> 2.5494728 </td>
   <td style="text-align:left;"> Sub-Saharan Africa </td>
   <td style="text-align:left;"> Africa </td>
  </tr>
  <tr>
   <td style="text-align:left;"> Grenada </td>
   <td style="text-align:left;"> GRD </td>
   <td style="text-align:right;"> 1999 </td>
   <td style="text-align:right;"> 1.5332128 </td>
   <td style="text-align:left;"> Latin America &amp; Caribbean </td>
   <td style="text-align:left;"> Americas </td>
  </tr>
  <tr>
   <td style="text-align:left;"> Cyprus </td>
   <td style="text-align:left;"> CYP </td>
   <td style="text-align:right;"> 2005 </td>
   <td style="text-align:right;"> 0.0256534 </td>
   <td style="text-align:left;"> Europe &amp; Central Asia </td>
   <td style="text-align:left;"> Asia </td>
  </tr>
  <tr>
   <td style="text-align:left;"> Tokelau </td>
   <td style="text-align:left;"> TKL </td>
   <td style="text-align:right;"> 1993 </td>
   <td style="text-align:right;"> 0.0642746 </td>
   <td style="text-align:left;"> East Asia &amp; Pacific </td>
   <td style="text-align:left;"> Oceania </td>
  </tr>
  <tr>
   <td style="text-align:left;"> Croatia </td>
   <td style="text-align:left;"> HRV </td>
   <td style="text-align:right;"> 1995 </td>
   <td style="text-align:right;"> 1.3090589 </td>
   <td style="text-align:left;"> Europe &amp; Central Asia </td>
   <td style="text-align:left;"> Europe </td>
  </tr>
  <tr>
   <td style="text-align:left;"> Mali </td>
   <td style="text-align:left;"> MLI </td>
   <td style="text-align:right;"> 2001 </td>
   <td style="text-align:right;"> 11.9292149 </td>
   <td style="text-align:left;"> Sub-Saharan Africa </td>
   <td style="text-align:left;"> Africa </td>
  </tr>
  <tr>
   <td style="text-align:left;"> Thailand </td>
   <td style="text-align:left;"> THA </td>
   <td style="text-align:right;"> 2003 </td>
   <td style="text-align:right;"> 3.6432231 </td>
   <td style="text-align:left;"> East Asia &amp; Pacific </td>
   <td style="text-align:left;"> Asia </td>
  </tr>
  <tr>
   <td style="text-align:left;"> Madagascar </td>
   <td style="text-align:left;"> MDG </td>
   <td style="text-align:right;"> 1996 </td>
   <td style="text-align:right;"> 16.5189092 </td>
   <td style="text-align:left;"> Sub-Saharan Africa </td>
   <td style="text-align:left;"> Africa </td>
  </tr>
  <tr>
   <td style="text-align:left;"> Luxembourg </td>
   <td style="text-align:left;"> LUX </td>
   <td style="text-align:right;"> 2019 </td>
   <td style="text-align:right;"> 0.0049119 </td>
   <td style="text-align:left;"> Europe &amp; Central Asia </td>
   <td style="text-align:left;"> Europe </td>
  </tr>
  <tr>
   <td style="text-align:left;"> Sao Tome and Principe </td>
   <td style="text-align:left;"> STP </td>
   <td style="text-align:right;"> 1996 </td>
   <td style="text-align:right;"> 15.8957191 </td>
   <td style="text-align:left;"> Sub-Saharan Africa </td>
   <td style="text-align:left;"> Africa </td>
  </tr>
  <tr>
   <td style="text-align:left;"> Bahamas </td>
   <td style="text-align:left;"> BHS </td>
   <td style="text-align:right;"> 2000 </td>
   <td style="text-align:right;"> 0.2593684 </td>
   <td style="text-align:left;"> Latin America &amp; Caribbean </td>
   <td style="text-align:left;"> Americas </td>
  </tr>
  <tr>
   <td style="text-align:left;"> Hungary </td>
   <td style="text-align:left;"> HUN </td>
   <td style="text-align:right;"> 2006 </td>
   <td style="text-align:right;"> 1.5233016 </td>
   <td style="text-align:left;"> Europe &amp; Central Asia </td>
   <td style="text-align:left;"> Europe </td>
  </tr>
  <tr>
   <td style="text-align:left;"> Croatia </td>
   <td style="text-align:left;"> HRV </td>
   <td style="text-align:right;"> 1990 </td>
   <td style="text-align:right;"> 1.7001570 </td>
   <td style="text-align:left;"> Europe &amp; Central Asia </td>
   <td style="text-align:left;"> Europe </td>
  </tr>
  <tr>
   <td style="text-align:left;"> Turkmenistan </td>
   <td style="text-align:left;"> TKM </td>
   <td style="text-align:right;"> 2006 </td>
   <td style="text-align:right;"> 0.0764796 </td>
   <td style="text-align:left;"> Europe &amp; Central Asia </td>
   <td style="text-align:left;"> Asia </td>
  </tr>
  <tr>
   <td style="text-align:left;"> Samoa </td>
   <td style="text-align:left;"> WSM </td>
   <td style="text-align:right;"> 1992 </td>
   <td style="text-align:right;"> 13.2417568 </td>
   <td style="text-align:left;"> East Asia &amp; Pacific </td>
   <td style="text-align:left;"> Oceania </td>
  </tr>
  <tr>
   <td style="text-align:left;"> Myanmar </td>
   <td style="text-align:left;"> MMR </td>
   <td style="text-align:right;"> 2010 </td>
   <td style="text-align:right;"> 13.7823404 </td>
   <td style="text-align:left;"> East Asia &amp; Pacific </td>
   <td style="text-align:left;"> Asia </td>
  </tr>
  <tr>
   <td style="text-align:left;"> Poland </td>
   <td style="text-align:left;"> POL </td>
   <td style="text-align:right;"> 2006 </td>
   <td style="text-align:right;"> 1.3145120 </td>
   <td style="text-align:left;"> Europe &amp; Central Asia </td>
   <td style="text-align:left;"> Europe </td>
  </tr>
  <tr>
   <td style="text-align:left;"> India </td>
   <td style="text-align:left;"> IND </td>
   <td style="text-align:right;"> 1999 </td>
   <td style="text-align:right;"> 12.4925432 </td>
   <td style="text-align:left;"> South Asia </td>
   <td style="text-align:left;"> Asia </td>
  </tr>
  <tr>
   <td style="text-align:left;"> Vanuatu </td>
   <td style="text-align:left;"> VUT </td>
   <td style="text-align:right;"> 2019 </td>
   <td style="text-align:right;"> 16.3333186 </td>
   <td style="text-align:left;"> East Asia &amp; Pacific </td>
   <td style="text-align:left;"> Oceania </td>
  </tr>
  <tr>
   <td style="text-align:left;"> Mexico </td>
   <td style="text-align:left;"> MEX </td>
   <td style="text-align:right;"> 1991 </td>
   <td style="text-align:right;"> 3.8408931 </td>
   <td style="text-align:left;"> Latin America &amp; Caribbean </td>
   <td style="text-align:left;"> Americas </td>
  </tr>
  <tr>
   <td style="text-align:left;"> Japan </td>
   <td style="text-align:left;"> JPN </td>
   <td style="text-align:right;"> 2009 </td>
   <td style="text-align:right;"> 0.0101385 </td>
   <td style="text-align:left;"> East Asia &amp; Pacific </td>
   <td style="text-align:left;"> Asia </td>
  </tr>
  <tr>
   <td style="text-align:left;"> United Kingdom </td>
   <td style="text-align:left;"> GBR </td>
   <td style="text-align:right;"> 1990 </td>
   <td style="text-align:right;"> 0.0121910 </td>
   <td style="text-align:left;"> Europe &amp; Central Asia </td>
   <td style="text-align:left;"> Europe </td>
  </tr>
  <tr>
   <td style="text-align:left;"> Czechia </td>
   <td style="text-align:left;"> CZE </td>
   <td style="text-align:right;"> 2008 </td>
   <td style="text-align:right;"> 0.1073660 </td>
   <td style="text-align:left;"> Europe &amp; Central Asia </td>
   <td style="text-align:left;"> Europe </td>
  </tr>
  <tr>
   <td style="text-align:left;"> Kazakhstan </td>
   <td style="text-align:left;"> KAZ </td>
   <td style="text-align:right;"> 2011 </td>
   <td style="text-align:right;"> 1.7001212 </td>
   <td style="text-align:left;"> Europe &amp; Central Asia </td>
   <td style="text-align:left;"> Asia </td>
  </tr>
  <tr>
   <td style="text-align:left;"> Sweden </td>
   <td style="text-align:left;"> SWE </td>
   <td style="text-align:right;"> 2005 </td>
   <td style="text-align:right;"> 0.0091248 </td>
   <td style="text-align:left;"> Europe &amp; Central Asia </td>
   <td style="text-align:left;"> Europe </td>
  </tr>
  <tr>
   <td style="text-align:left;"> Moldova </td>
   <td style="text-align:left;"> MDA </td>
   <td style="text-align:right;"> 2009 </td>
   <td style="text-align:right;"> 1.5713703 </td>
   <td style="text-align:left;"> Europe &amp; Central Asia </td>
   <td style="text-align:left;"> Europe </td>
  </tr>
  <tr>
   <td style="text-align:left;"> United Arab Emirates </td>
   <td style="text-align:left;"> ARE </td>
   <td style="text-align:right;"> 2008 </td>
   <td style="text-align:right;"> 0.0016334 </td>
   <td style="text-align:left;"> Middle East &amp; North Africa </td>
   <td style="text-align:left;"> Asia </td>
  </tr>
  <tr>
   <td style="text-align:left;"> Iran </td>
   <td style="text-align:left;"> IRN </td>
   <td style="text-align:right;"> 1993 </td>
   <td style="text-align:right;"> 1.4896693 </td>
   <td style="text-align:left;"> Middle East &amp; North Africa </td>
   <td style="text-align:left;"> Asia </td>
  </tr>
  <tr>
   <td style="text-align:left;"> Liberia </td>
   <td style="text-align:left;"> LBR </td>
   <td style="text-align:right;"> 2012 </td>
   <td style="text-align:right;"> 11.9645461 </td>
   <td style="text-align:left;"> Sub-Saharan Africa </td>
   <td style="text-align:left;"> Africa </td>
  </tr>
  <tr>
   <td style="text-align:left;"> Bahamas </td>
   <td style="text-align:left;"> BHS </td>
   <td style="text-align:right;"> 1990 </td>
   <td style="text-align:right;"> 0.5883986 </td>
   <td style="text-align:left;"> Latin America &amp; Caribbean </td>
   <td style="text-align:left;"> Americas </td>
  </tr>
  <tr>
   <td style="text-align:left;"> Cuba </td>
   <td style="text-align:left;"> CUB </td>
   <td style="text-align:right;"> 2000 </td>
   <td style="text-align:right;"> 0.6644410 </td>
   <td style="text-align:left;"> Latin America &amp; Caribbean </td>
   <td style="text-align:left;"> Americas </td>
  </tr>
  <tr>
   <td style="text-align:left;"> Papua New Guinea </td>
   <td style="text-align:left;"> PNG </td>
   <td style="text-align:right;"> 2007 </td>
   <td style="text-align:right;"> 19.5081069 </td>
   <td style="text-align:left;"> East Asia &amp; Pacific </td>
   <td style="text-align:left;"> Oceania </td>
  </tr>
  <tr>
   <td style="text-align:left;"> United States </td>
   <td style="text-align:left;"> USA </td>
   <td style="text-align:right;"> 2015 </td>
   <td style="text-align:right;"> 0.0058794 </td>
   <td style="text-align:left;"> North America </td>
   <td style="text-align:left;"> Americas </td>
  </tr>
  <tr>
   <td style="text-align:left;"> Luxembourg </td>
   <td style="text-align:left;"> LUX </td>
   <td style="text-align:right;"> 2011 </td>
   <td style="text-align:right;"> 0.0082670 </td>
   <td style="text-align:left;"> Europe &amp; Central Asia </td>
   <td style="text-align:left;"> Europe </td>
  </tr>
  <tr>
   <td style="text-align:left;"> Bolivia </td>
   <td style="text-align:left;"> BOL </td>
   <td style="text-align:right;"> 1994 </td>
   <td style="text-align:right;"> 7.1299788 </td>
   <td style="text-align:left;"> Latin America &amp; Caribbean </td>
   <td style="text-align:left;"> Americas </td>
  </tr>
  <tr>
   <td style="text-align:left;"> Costa Rica </td>
   <td style="text-align:left;"> CRI </td>
   <td style="text-align:right;"> 2005 </td>
   <td style="text-align:right;"> 1.0294493 </td>
   <td style="text-align:left;"> Latin America &amp; Caribbean </td>
   <td style="text-align:left;"> Americas </td>
  </tr>
  <tr>
   <td style="text-align:left;"> Lebanon </td>
   <td style="text-align:left;"> LBN </td>
   <td style="text-align:right;"> 2006 </td>
   <td style="text-align:right;"> 0.2156280 </td>
   <td style="text-align:left;"> Middle East &amp; North Africa </td>
   <td style="text-align:left;"> Asia </td>
  </tr>
  <tr>
   <td style="text-align:left;"> Germany </td>
   <td style="text-align:left;"> DEU </td>
   <td style="text-align:right;"> 1997 </td>
   <td style="text-align:right;"> 0.0092219 </td>
   <td style="text-align:left;"> Europe &amp; Central Asia </td>
   <td style="text-align:left;"> Europe </td>
  </tr>
  <tr>
   <td style="text-align:left;"> Austria </td>
   <td style="text-align:left;"> AUT </td>
   <td style="text-align:right;"> 1998 </td>
   <td style="text-align:right;"> 0.0383683 </td>
   <td style="text-align:left;"> Europe &amp; Central Asia </td>
   <td style="text-align:left;"> Europe </td>
  </tr>
  <tr>
   <td style="text-align:left;"> Nicaragua </td>
   <td style="text-align:left;"> NIC </td>
   <td style="text-align:right;"> 2004 </td>
   <td style="text-align:right;"> 10.1355355 </td>
   <td style="text-align:left;"> Latin America &amp; Caribbean </td>
   <td style="text-align:left;"> Americas </td>
  </tr>
  <tr>
   <td style="text-align:left;"> Rwanda </td>
   <td style="text-align:left;"> RWA </td>
   <td style="text-align:right;"> 2008 </td>
   <td style="text-align:right;"> 12.7516739 </td>
   <td style="text-align:left;"> Sub-Saharan Africa </td>
   <td style="text-align:left;"> Africa </td>
  </tr>
  <tr>
   <td style="text-align:left;"> Palestine </td>
   <td style="text-align:left;"> PSE </td>
   <td style="text-align:right;"> 1992 </td>
   <td style="text-align:right;"> 5.4816027 </td>
   <td style="text-align:left;"> Middle East &amp; North Africa </td>
   <td style="text-align:left;"> Asia </td>
  </tr>
  <tr>
   <td style="text-align:left;"> Cape Verde </td>
   <td style="text-align:left;"> CPV </td>
   <td style="text-align:right;"> 2011 </td>
   <td style="text-align:right;"> 5.2084591 </td>
   <td style="text-align:left;"> Sub-Saharan Africa </td>
   <td style="text-align:left;"> Africa </td>
  </tr>
  <tr>
   <td style="text-align:left;"> Armenia </td>
   <td style="text-align:left;"> ARM </td>
   <td style="text-align:right;"> 2008 </td>
   <td style="text-align:right;"> 1.0514637 </td>
   <td style="text-align:left;"> Europe &amp; Central Asia </td>
   <td style="text-align:left;"> Asia </td>
  </tr>
  <tr>
   <td style="text-align:left;"> Maldives </td>
   <td style="text-align:left;"> MDV </td>
   <td style="text-align:right;"> 2012 </td>
   <td style="text-align:right;"> 2.8102106 </td>
   <td style="text-align:left;"> South Asia </td>
   <td style="text-align:left;"> Asia </td>
  </tr>
  <tr>
   <td style="text-align:left;"> Israel </td>
   <td style="text-align:left;"> ISR </td>
   <td style="text-align:right;"> 2006 </td>
   <td style="text-align:right;"> 0.0147671 </td>
   <td style="text-align:left;"> Middle East &amp; North Africa </td>
   <td style="text-align:left;"> Asia </td>
  </tr>
  <tr>
   <td style="text-align:left;"> Angola </td>
   <td style="text-align:left;"> AGO </td>
   <td style="text-align:right;"> 1991 </td>
   <td style="text-align:right;"> 12.5965142 </td>
   <td style="text-align:left;"> Sub-Saharan Africa </td>
   <td style="text-align:left;"> Africa </td>
  </tr>
  <tr>
   <td style="text-align:left;"> Mali </td>
   <td style="text-align:left;"> MLI </td>
   <td style="text-align:right;"> 2005 </td>
   <td style="text-align:right;"> 12.1540321 </td>
   <td style="text-align:left;"> Sub-Saharan Africa </td>
   <td style="text-align:left;"> Africa </td>
  </tr>
  <tr>
   <td style="text-align:left;"> Brazil </td>
   <td style="text-align:left;"> BRA </td>
   <td style="text-align:right;"> 2012 </td>
   <td style="text-align:right;"> 1.6214570 </td>
   <td style="text-align:left;"> Latin America &amp; Caribbean </td>
   <td style="text-align:left;"> Americas </td>
  </tr>
  <tr>
   <td style="text-align:left;"> Angola </td>
   <td style="text-align:left;"> AGO </td>
   <td style="text-align:right;"> 1993 </td>
   <td style="text-align:right;"> 11.9140975 </td>
   <td style="text-align:left;"> Sub-Saharan Africa </td>
   <td style="text-align:left;"> Africa </td>
  </tr>
  <tr>
   <td style="text-align:left;"> Latvia </td>
   <td style="text-align:left;"> LVA </td>
   <td style="text-align:right;"> 2015 </td>
   <td style="text-align:right;"> 0.4215370 </td>
   <td style="text-align:left;"> Europe &amp; Central Asia </td>
   <td style="text-align:left;"> Europe </td>
  </tr>
  <tr>
   <td style="text-align:left;"> Indonesia </td>
   <td style="text-align:left;"> IDN </td>
   <td style="text-align:right;"> 2001 </td>
   <td style="text-align:right;"> 7.7689270 </td>
   <td style="text-align:left;"> East Asia &amp; Pacific </td>
   <td style="text-align:left;"> Asia </td>
  </tr>
  <tr>
   <td style="text-align:left;"> Bosnia and Herzegovina </td>
   <td style="text-align:left;"> BIH </td>
   <td style="text-align:right;"> 1993 </td>
   <td style="text-align:right;"> 9.4455416 </td>
   <td style="text-align:left;"> Europe &amp; Central Asia </td>
   <td style="text-align:left;"> Europe </td>
  </tr>
  <tr>
   <td style="text-align:left;"> Germany </td>
   <td style="text-align:left;"> DEU </td>
   <td style="text-align:right;"> 2014 </td>
   <td style="text-align:right;"> 0.0037945 </td>
   <td style="text-align:left;"> Europe &amp; Central Asia </td>
   <td style="text-align:left;"> Europe </td>
  </tr>
  <tr>
   <td style="text-align:left;"> Jamaica </td>
   <td style="text-align:left;"> JAM </td>
   <td style="text-align:right;"> 2013 </td>
   <td style="text-align:right;"> 1.5024576 </td>
   <td style="text-align:left;"> Latin America &amp; Caribbean </td>
   <td style="text-align:left;"> Americas </td>
  </tr>
  <tr>
   <td style="text-align:left;"> Colombia </td>
   <td style="text-align:left;"> COL </td>
   <td style="text-align:right;"> 2018 </td>
   <td style="text-align:right;"> 1.0737497 </td>
   <td style="text-align:left;"> Latin America &amp; Caribbean </td>
   <td style="text-align:left;"> Americas </td>
  </tr>
  <tr>
   <td style="text-align:left;"> Bolivia </td>
   <td style="text-align:left;"> BOL </td>
   <td style="text-align:right;"> 2005 </td>
   <td style="text-align:right;"> 5.2120353 </td>
   <td style="text-align:left;"> Latin America &amp; Caribbean </td>
   <td style="text-align:left;"> Americas </td>
  </tr>
  <tr>
   <td style="text-align:left;"> India </td>
   <td style="text-align:left;"> IND </td>
   <td style="text-align:right;"> 2013 </td>
   <td style="text-align:right;"> 8.5536003 </td>
   <td style="text-align:left;"> South Asia </td>
   <td style="text-align:left;"> Asia </td>
  </tr>
  <tr>
   <td style="text-align:left;"> Morocco </td>
   <td style="text-align:left;"> MAR </td>
   <td style="text-align:right;"> 2011 </td>
   <td style="text-align:right;"> 2.0292960 </td>
   <td style="text-align:left;"> Middle East &amp; North Africa </td>
   <td style="text-align:left;"> Africa </td>
  </tr>
  <tr>
   <td style="text-align:left;"> Poland </td>
   <td style="text-align:left;"> POL </td>
   <td style="text-align:right;"> 2018 </td>
   <td style="text-align:right;"> 0.7069132 </td>
   <td style="text-align:left;"> Europe &amp; Central Asia </td>
   <td style="text-align:left;"> Europe </td>
  </tr>
  <tr>
   <td style="text-align:left;"> Lithuania </td>
   <td style="text-align:left;"> LTU </td>
   <td style="text-align:right;"> 2018 </td>
   <td style="text-align:right;"> 0.1552845 </td>
   <td style="text-align:left;"> Europe &amp; Central Asia </td>
   <td style="text-align:left;"> Europe </td>
  </tr>
  <tr>
   <td style="text-align:left;"> Panama </td>
   <td style="text-align:left;"> PAN </td>
   <td style="text-align:right;"> 2005 </td>
   <td style="text-align:right;"> 2.3679592 </td>
   <td style="text-align:left;"> Latin America &amp; Caribbean </td>
   <td style="text-align:left;"> Americas </td>
  </tr>
  <tr>
   <td style="text-align:left;"> Benin </td>
   <td style="text-align:left;"> BEN </td>
   <td style="text-align:right;"> 2018 </td>
   <td style="text-align:right;"> 12.3010879 </td>
   <td style="text-align:left;"> Sub-Saharan Africa </td>
   <td style="text-align:left;"> Africa </td>
  </tr>
  <tr>
   <td style="text-align:left;"> China </td>
   <td style="text-align:left;"> CHN </td>
   <td style="text-align:right;"> 2002 </td>
   <td style="text-align:right;"> 10.6826624 </td>
   <td style="text-align:left;"> East Asia &amp; Pacific </td>
   <td style="text-align:left;"> Asia </td>
  </tr>
  <tr>
   <td style="text-align:left;"> Indonesia </td>
   <td style="text-align:left;"> IDN </td>
   <td style="text-align:right;"> 2017 </td>
   <td style="text-align:right;"> 4.9492931 </td>
   <td style="text-align:left;"> East Asia &amp; Pacific </td>
   <td style="text-align:left;"> Asia </td>
  </tr>
  <tr>
   <td style="text-align:left;"> Cuba </td>
   <td style="text-align:left;"> CUB </td>
   <td style="text-align:right;"> 1993 </td>
   <td style="text-align:right;"> 0.8895589 </td>
   <td style="text-align:left;"> Latin America &amp; Caribbean </td>
   <td style="text-align:left;"> Americas </td>
  </tr>
  <tr>
   <td style="text-align:left;"> Mozambique </td>
   <td style="text-align:left;"> MOZ </td>
   <td style="text-align:right;"> 2017 </td>
   <td style="text-align:right;"> 11.6481113 </td>
   <td style="text-align:left;"> Sub-Saharan Africa </td>
   <td style="text-align:left;"> Africa </td>
  </tr>
  <tr>
   <td style="text-align:left;"> Cote d'Ivoire </td>
   <td style="text-align:left;"> CIV </td>
   <td style="text-align:right;"> 2017 </td>
   <td style="text-align:right;"> 9.9960043 </td>
   <td style="text-align:left;"> Sub-Saharan Africa </td>
   <td style="text-align:left;"> Africa </td>
  </tr>
  <tr>
   <td style="text-align:left;"> Tajikistan </td>
   <td style="text-align:left;"> TJK </td>
   <td style="text-align:right;"> 1999 </td>
   <td style="text-align:right;"> 13.0578051 </td>
   <td style="text-align:left;"> Europe &amp; Central Asia </td>
   <td style="text-align:left;"> Asia </td>
  </tr>
  <tr>
   <td style="text-align:left;"> Haiti </td>
   <td style="text-align:left;"> HTI </td>
   <td style="text-align:right;"> 1991 </td>
   <td style="text-align:right;"> 15.9986193 </td>
   <td style="text-align:left;"> Latin America &amp; Caribbean </td>
   <td style="text-align:left;"> Americas </td>
  </tr>
  <tr>
   <td style="text-align:left;"> Finland </td>
   <td style="text-align:left;"> FIN </td>
   <td style="text-align:right;"> 2013 </td>
   <td style="text-align:right;"> 0.0062007 </td>
   <td style="text-align:left;"> Europe &amp; Central Asia </td>
   <td style="text-align:left;"> Europe </td>
  </tr>
  <tr>
   <td style="text-align:left;"> Laos </td>
   <td style="text-align:left;"> LAO </td>
   <td style="text-align:right;"> 2011 </td>
   <td style="text-align:right;"> 16.2729534 </td>
   <td style="text-align:left;"> East Asia &amp; Pacific </td>
   <td style="text-align:left;"> Asia </td>
  </tr>
  <tr>
   <td style="text-align:left;"> Morocco </td>
   <td style="text-align:left;"> MAR </td>
   <td style="text-align:right;"> 2008 </td>
   <td style="text-align:right;"> 2.7287742 </td>
   <td style="text-align:left;"> Middle East &amp; North Africa </td>
   <td style="text-align:left;"> Africa </td>
  </tr>
  <tr>
   <td style="text-align:left;"> Botswana </td>
   <td style="text-align:left;"> BWA </td>
   <td style="text-align:right;"> 2005 </td>
   <td style="text-align:right;"> 4.5732766 </td>
   <td style="text-align:left;"> Sub-Saharan Africa </td>
   <td style="text-align:left;"> Africa </td>
  </tr>
  <tr>
   <td style="text-align:left;"> Liberia </td>
   <td style="text-align:left;"> LBR </td>
   <td style="text-align:right;"> 2011 </td>
   <td style="text-align:right;"> 11.9950037 </td>
   <td style="text-align:left;"> Sub-Saharan Africa </td>
   <td style="text-align:left;"> Africa </td>
  </tr>
  <tr>
   <td style="text-align:left;"> Latvia </td>
   <td style="text-align:left;"> LVA </td>
   <td style="text-align:right;"> 2016 </td>
   <td style="text-align:right;"> 0.4031621 </td>
   <td style="text-align:left;"> Europe &amp; Central Asia </td>
   <td style="text-align:left;"> Europe </td>
  </tr>
  <tr>
   <td style="text-align:left;"> Saint Kitts and Nevis </td>
   <td style="text-align:left;"> KNA </td>
   <td style="text-align:right;"> 2012 </td>
   <td style="text-align:right;"> 0.2831855 </td>
   <td style="text-align:left;"> Latin America &amp; Caribbean </td>
   <td style="text-align:left;"> Americas </td>
  </tr>
  <tr>
   <td style="text-align:left;"> Equatorial Guinea </td>
   <td style="text-align:left;"> GNQ </td>
   <td style="text-align:right;"> 2007 </td>
   <td style="text-align:right;"> 6.0304468 </td>
   <td style="text-align:left;"> Sub-Saharan Africa </td>
   <td style="text-align:left;"> Africa </td>
  </tr>
  <tr>
   <td style="text-align:left;"> Nicaragua </td>
   <td style="text-align:left;"> NIC </td>
   <td style="text-align:right;"> 1999 </td>
   <td style="text-align:right;"> 11.0709031 </td>
   <td style="text-align:left;"> Latin America &amp; Caribbean </td>
   <td style="text-align:left;"> Americas </td>
  </tr>
  <tr>
   <td style="text-align:left;"> Northern Mariana Islands </td>
   <td style="text-align:left;"> MNP </td>
   <td style="text-align:right;"> 2011 </td>
   <td style="text-align:right;"> 1.4482527 </td>
   <td style="text-align:left;"> East Asia &amp; Pacific </td>
   <td style="text-align:left;"> Oceania </td>
  </tr>
  <tr>
   <td style="text-align:left;"> Bahamas </td>
   <td style="text-align:left;"> BHS </td>
   <td style="text-align:right;"> 1997 </td>
   <td style="text-align:right;"> 0.3704869 </td>
   <td style="text-align:left;"> Latin America &amp; Caribbean </td>
   <td style="text-align:left;"> Americas </td>
  </tr>
  <tr>
   <td style="text-align:left;"> Puerto Rico </td>
   <td style="text-align:left;"> PRI </td>
   <td style="text-align:right;"> 2005 </td>
   <td style="text-align:right;"> 0.0038577 </td>
   <td style="text-align:left;"> Latin America &amp; Caribbean </td>
   <td style="text-align:left;"> Americas </td>
  </tr>
  <tr>
   <td style="text-align:left;"> Israel </td>
   <td style="text-align:left;"> ISR </td>
   <td style="text-align:right;"> 1992 </td>
   <td style="text-align:right;"> 0.0441180 </td>
   <td style="text-align:left;"> Middle East &amp; North Africa </td>
   <td style="text-align:left;"> Asia </td>
  </tr>
  <tr>
   <td style="text-align:left;"> Paraguay </td>
   <td style="text-align:left;"> PRY </td>
   <td style="text-align:right;"> 2001 </td>
   <td style="text-align:right;"> 8.1523830 </td>
   <td style="text-align:left;"> Latin America &amp; Caribbean </td>
   <td style="text-align:left;"> Americas </td>
  </tr>
  <tr>
   <td style="text-align:left;"> Venezuela </td>
   <td style="text-align:left;"> VEN </td>
   <td style="text-align:right;"> 2010 </td>
   <td style="text-align:right;"> 0.0931878 </td>
   <td style="text-align:left;"> Latin America &amp; Caribbean </td>
   <td style="text-align:left;"> Americas </td>
  </tr>
  <tr>
   <td style="text-align:left;"> Mozambique </td>
   <td style="text-align:left;"> MOZ </td>
   <td style="text-align:right;"> 2013 </td>
   <td style="text-align:right;"> 11.2846063 </td>
   <td style="text-align:left;"> Sub-Saharan Africa </td>
   <td style="text-align:left;"> Africa </td>
  </tr>
  <tr>
   <td style="text-align:left;"> Denmark </td>
   <td style="text-align:left;"> DNK </td>
   <td style="text-align:right;"> 1990 </td>
   <td style="text-align:right;"> 0.0228328 </td>
   <td style="text-align:left;"> Europe &amp; Central Asia </td>
   <td style="text-align:left;"> Europe </td>
  </tr>
  <tr>
   <td style="text-align:left;"> Saudi Arabia </td>
   <td style="text-align:left;"> SAU </td>
   <td style="text-align:right;"> 2005 </td>
   <td style="text-align:right;"> 0.3837821 </td>
   <td style="text-align:left;"> Middle East &amp; North Africa </td>
   <td style="text-align:left;"> Asia </td>
  </tr>
  <tr>
   <td style="text-align:left;"> Djibouti </td>
   <td style="text-align:left;"> DJI </td>
   <td style="text-align:right;"> 2001 </td>
   <td style="text-align:right;"> 6.2929158 </td>
   <td style="text-align:left;"> Middle East &amp; North Africa </td>
   <td style="text-align:left;"> Africa </td>
  </tr>
  <tr>
   <td style="text-align:left;"> Latvia </td>
   <td style="text-align:left;"> LVA </td>
   <td style="text-align:right;"> 2000 </td>
   <td style="text-align:right;"> 0.9319204 </td>
   <td style="text-align:left;"> Europe &amp; Central Asia </td>
   <td style="text-align:left;"> Europe </td>
  </tr>
  <tr>
   <td style="text-align:left;"> Mexico </td>
   <td style="text-align:left;"> MEX </td>
   <td style="text-align:right;"> 1992 </td>
   <td style="text-align:right;"> 3.7274224 </td>
   <td style="text-align:left;"> Latin America &amp; Caribbean </td>
   <td style="text-align:left;"> Americas </td>
  </tr>
  <tr>
   <td style="text-align:left;"> Monaco </td>
   <td style="text-align:left;"> MCO </td>
   <td style="text-align:right;"> 2017 </td>
   <td style="text-align:right;"> 0.0028420 </td>
   <td style="text-align:left;"> Europe &amp; Central Asia </td>
   <td style="text-align:left;"> Europe </td>
  </tr>
  <tr>
   <td style="text-align:left;"> North Korea </td>
   <td style="text-align:left;"> PRK </td>
   <td style="text-align:right;"> 1990 </td>
   <td style="text-align:right;"> 20.3195029 </td>
   <td style="text-align:left;"> East Asia &amp; Pacific </td>
   <td style="text-align:left;"> Asia </td>
  </tr>
  <tr>
   <td style="text-align:left;"> San Marino </td>
   <td style="text-align:left;"> SMR </td>
   <td style="text-align:right;"> 2010 </td>
   <td style="text-align:right;"> 0.0070857 </td>
   <td style="text-align:left;"> Europe &amp; Central Asia </td>
   <td style="text-align:left;"> Europe </td>
  </tr>
  <tr>
   <td style="text-align:left;"> Latvia </td>
   <td style="text-align:left;"> LVA </td>
   <td style="text-align:right;"> 2001 </td>
   <td style="text-align:right;"> 0.9063388 </td>
   <td style="text-align:left;"> Europe &amp; Central Asia </td>
   <td style="text-align:left;"> Europe </td>
  </tr>
  <tr>
   <td style="text-align:left;"> Slovenia </td>
   <td style="text-align:left;"> SVN </td>
   <td style="text-align:right;"> 1998 </td>
   <td style="text-align:right;"> 0.8881027 </td>
   <td style="text-align:left;"> Europe &amp; Central Asia </td>
   <td style="text-align:left;"> Europe </td>
  </tr>
  <tr>
   <td style="text-align:left;"> Slovenia </td>
   <td style="text-align:left;"> SVN </td>
   <td style="text-align:right;"> 2000 </td>
   <td style="text-align:right;"> 0.7608949 </td>
   <td style="text-align:left;"> Europe &amp; Central Asia </td>
   <td style="text-align:left;"> Europe </td>
  </tr>
  <tr>
   <td style="text-align:left;"> Chile </td>
   <td style="text-align:left;"> CHL </td>
   <td style="text-align:right;"> 2008 </td>
   <td style="text-align:right;"> 0.6365674 </td>
   <td style="text-align:left;"> Latin America &amp; Caribbean </td>
   <td style="text-align:left;"> Americas </td>
  </tr>
  <tr>
   <td style="text-align:left;"> Azerbaijan </td>
   <td style="text-align:left;"> AZE </td>
   <td style="text-align:right;"> 2001 </td>
   <td style="text-align:right;"> 4.9788456 </td>
   <td style="text-align:left;"> Europe &amp; Central Asia </td>
   <td style="text-align:left;"> Asia </td>
  </tr>
  <tr>
   <td style="text-align:left;"> Tuvalu </td>
   <td style="text-align:left;"> TUV </td>
   <td style="text-align:right;"> 2008 </td>
   <td style="text-align:right;"> 3.7063418 </td>
   <td style="text-align:left;"> East Asia &amp; Pacific </td>
   <td style="text-align:left;"> Oceania </td>
  </tr>
  <tr>
   <td style="text-align:left;"> Botswana </td>
   <td style="text-align:left;"> BWA </td>
   <td style="text-align:right;"> 2010 </td>
   <td style="text-align:right;"> 4.4528485 </td>
   <td style="text-align:left;"> Sub-Saharan Africa </td>
   <td style="text-align:left;"> Africa </td>
  </tr>
  <tr>
   <td style="text-align:left;"> Greece </td>
   <td style="text-align:left;"> GRC </td>
   <td style="text-align:right;"> 2010 </td>
   <td style="text-align:right;"> 0.0513926 </td>
   <td style="text-align:left;"> Europe &amp; Central Asia </td>
   <td style="text-align:left;"> Europe </td>
  </tr>
  <tr>
   <td style="text-align:left;"> Ireland </td>
   <td style="text-align:left;"> IRL </td>
   <td style="text-align:right;"> 1996 </td>
   <td style="text-align:right;"> 0.0465626 </td>
   <td style="text-align:left;"> Europe &amp; Central Asia </td>
   <td style="text-align:left;"> Europe </td>
  </tr>
  <tr>
   <td style="text-align:left;"> Greenland </td>
   <td style="text-align:left;"> GRL </td>
   <td style="text-align:right;"> 2008 </td>
   <td style="text-align:right;"> 0.1343891 </td>
   <td style="text-align:left;"> Europe &amp; Central Asia </td>
   <td style="text-align:left;"> Americas </td>
  </tr>
  <tr>
   <td style="text-align:left;"> Vanuatu </td>
   <td style="text-align:left;"> VUT </td>
   <td style="text-align:right;"> 1991 </td>
   <td style="text-align:right;"> 18.2713208 </td>
   <td style="text-align:left;"> East Asia &amp; Pacific </td>
   <td style="text-align:left;"> Oceania </td>
  </tr>
  <tr>
   <td style="text-align:left;"> Chile </td>
   <td style="text-align:left;"> CHL </td>
   <td style="text-align:right;"> 2005 </td>
   <td style="text-align:right;"> 0.7850238 </td>
   <td style="text-align:left;"> Latin America &amp; Caribbean </td>
   <td style="text-align:left;"> Americas </td>
  </tr>
  <tr>
   <td style="text-align:left;"> Madagascar </td>
   <td style="text-align:left;"> MDG </td>
   <td style="text-align:right;"> 2001 </td>
   <td style="text-align:right;"> 16.3874236 </td>
   <td style="text-align:left;"> Sub-Saharan Africa </td>
   <td style="text-align:left;"> Africa </td>
  </tr>
  <tr>
   <td style="text-align:left;"> Cook Islands </td>
   <td style="text-align:left;"> COK </td>
   <td style="text-align:right;"> 1998 </td>
   <td style="text-align:right;"> 1.1380640 </td>
   <td style="text-align:left;"> East Asia &amp; Pacific </td>
   <td style="text-align:left;"> Oceania </td>
  </tr>
  <tr>
   <td style="text-align:left;"> Switzerland </td>
   <td style="text-align:left;"> CHE </td>
   <td style="text-align:right;"> 2016 </td>
   <td style="text-align:right;"> 0.0022276 </td>
   <td style="text-align:left;"> Europe &amp; Central Asia </td>
   <td style="text-align:left;"> Europe </td>
  </tr>
  <tr>
   <td style="text-align:left;"> Kazakhstan </td>
   <td style="text-align:left;"> KAZ </td>
   <td style="text-align:right;"> 2003 </td>
   <td style="text-align:right;"> 3.3491985 </td>
   <td style="text-align:left;"> Europe &amp; Central Asia </td>
   <td style="text-align:left;"> Asia </td>
  </tr>
  <tr>
   <td style="text-align:left;"> Niger </td>
   <td style="text-align:left;"> NER </td>
   <td style="text-align:right;"> 2005 </td>
   <td style="text-align:right;"> 15.2339324 </td>
   <td style="text-align:left;"> Sub-Saharan Africa </td>
   <td style="text-align:left;"> Africa </td>
  </tr>
  <tr>
   <td style="text-align:left;"> Benin </td>
   <td style="text-align:left;"> BEN </td>
   <td style="text-align:right;"> 2019 </td>
   <td style="text-align:right;"> 12.1171025 </td>
   <td style="text-align:left;"> Sub-Saharan Africa </td>
   <td style="text-align:left;"> Africa </td>
  </tr>
  <tr>
   <td style="text-align:left;"> Slovakia </td>
   <td style="text-align:left;"> SVK </td>
   <td style="text-align:right;"> 2001 </td>
   <td style="text-align:right;"> 0.1607904 </td>
   <td style="text-align:left;"> Europe &amp; Central Asia </td>
   <td style="text-align:left;"> Europe </td>
  </tr>
  <tr>
   <td style="text-align:left;"> Ethiopia </td>
   <td style="text-align:left;"> ETH </td>
   <td style="text-align:right;"> 1995 </td>
   <td style="text-align:right;"> 13.6553821 </td>
   <td style="text-align:left;"> Sub-Saharan Africa </td>
   <td style="text-align:left;"> Africa </td>
  </tr>
  <tr>
   <td style="text-align:left;"> Vietnam </td>
   <td style="text-align:left;"> VNM </td>
   <td style="text-align:right;"> 2018 </td>
   <td style="text-align:right;"> 5.5723214 </td>
   <td style="text-align:left;"> East Asia &amp; Pacific </td>
   <td style="text-align:left;"> Asia </td>
  </tr>
  <tr>
   <td style="text-align:left;"> Paraguay </td>
   <td style="text-align:left;"> PRY </td>
   <td style="text-align:right;"> 2003 </td>
   <td style="text-align:right;"> 7.7428688 </td>
   <td style="text-align:left;"> Latin America &amp; Caribbean </td>
   <td style="text-align:left;"> Americas </td>
  </tr>
  <tr>
   <td style="text-align:left;"> Tokelau </td>
   <td style="text-align:left;"> TKL </td>
   <td style="text-align:right;"> 2009 </td>
   <td style="text-align:right;"> 0.0132162 </td>
   <td style="text-align:left;"> East Asia &amp; Pacific </td>
   <td style="text-align:left;"> Oceania </td>
  </tr>
  <tr>
   <td style="text-align:left;"> Cook Islands </td>
   <td style="text-align:left;"> COK </td>
   <td style="text-align:right;"> 2009 </td>
   <td style="text-align:right;"> 0.5530641 </td>
   <td style="text-align:left;"> East Asia &amp; Pacific </td>
   <td style="text-align:left;"> Oceania </td>
  </tr>
  <tr>
   <td style="text-align:left;"> Netherlands </td>
   <td style="text-align:left;"> NLD </td>
   <td style="text-align:right;"> 1999 </td>
   <td style="text-align:right;"> 0.0158779 </td>
   <td style="text-align:left;"> Europe &amp; Central Asia </td>
   <td style="text-align:left;"> Europe </td>
  </tr>
  <tr>
   <td style="text-align:left;"> Morocco </td>
   <td style="text-align:left;"> MAR </td>
   <td style="text-align:right;"> 2003 </td>
   <td style="text-align:right;"> 3.9552153 </td>
   <td style="text-align:left;"> Middle East &amp; North Africa </td>
   <td style="text-align:left;"> Africa </td>
  </tr>
  <tr>
   <td style="text-align:left;"> Netherlands </td>
   <td style="text-align:left;"> NLD </td>
   <td style="text-align:right;"> 1993 </td>
   <td style="text-align:right;"> 0.0249564 </td>
   <td style="text-align:left;"> Europe &amp; Central Asia </td>
   <td style="text-align:left;"> Europe </td>
  </tr>
  <tr>
   <td style="text-align:left;"> Guyana </td>
   <td style="text-align:left;"> GUY </td>
   <td style="text-align:right;"> 2008 </td>
   <td style="text-align:right;"> 2.1802187 </td>
   <td style="text-align:left;"> Latin America &amp; Caribbean </td>
   <td style="text-align:left;"> Americas </td>
  </tr>
  <tr>
   <td style="text-align:left;"> Pakistan </td>
   <td style="text-align:left;"> PAK </td>
   <td style="text-align:right;"> 2011 </td>
   <td style="text-align:right;"> 10.7529108 </td>
   <td style="text-align:left;"> South Asia </td>
   <td style="text-align:left;"> Asia </td>
  </tr>
  <tr>
   <td style="text-align:left;"> Mexico </td>
   <td style="text-align:left;"> MEX </td>
   <td style="text-align:right;"> 2005 </td>
   <td style="text-align:right;"> 2.2879495 </td>
   <td style="text-align:left;"> Latin America &amp; Caribbean </td>
   <td style="text-align:left;"> Americas </td>
  </tr>
  <tr>
   <td style="text-align:left;"> Cyprus </td>
   <td style="text-align:left;"> CYP </td>
   <td style="text-align:right;"> 2010 </td>
   <td style="text-align:right;"> 0.0148718 </td>
   <td style="text-align:left;"> Europe &amp; Central Asia </td>
   <td style="text-align:left;"> Asia </td>
  </tr>
  <tr>
   <td style="text-align:left;"> Oman </td>
   <td style="text-align:left;"> OMN </td>
   <td style="text-align:right;"> 1997 </td>
   <td style="text-align:right;"> 2.6148362 </td>
   <td style="text-align:left;"> Middle East &amp; North Africa </td>
   <td style="text-align:left;"> Asia </td>
  </tr>
  <tr>
   <td style="text-align:left;"> Palestine </td>
   <td style="text-align:left;"> PSE </td>
   <td style="text-align:right;"> 1994 </td>
   <td style="text-align:right;"> 4.4090738 </td>
   <td style="text-align:left;"> Middle East &amp; North Africa </td>
   <td style="text-align:left;"> Asia </td>
  </tr>
  <tr>
   <td style="text-align:left;"> Haiti </td>
   <td style="text-align:left;"> HTI </td>
   <td style="text-align:right;"> 2007 </td>
   <td style="text-align:right;"> 14.2018764 </td>
   <td style="text-align:left;"> Latin America &amp; Caribbean </td>
   <td style="text-align:left;"> Americas </td>
  </tr>
  <tr>
   <td style="text-align:left;"> Congo </td>
   <td style="text-align:left;"> COG </td>
   <td style="text-align:right;"> 2004 </td>
   <td style="text-align:right;"> 8.3928389 </td>
   <td style="text-align:left;"> Sub-Saharan Africa </td>
   <td style="text-align:left;"> Africa </td>
  </tr>
  <tr>
   <td style="text-align:left;"> Malta </td>
   <td style="text-align:left;"> MLT </td>
   <td style="text-align:right;"> 2014 </td>
   <td style="text-align:right;"> 0.0151035 </td>
   <td style="text-align:left;"> Middle East &amp; North Africa </td>
   <td style="text-align:left;"> Europe </td>
  </tr>
  <tr>
   <td style="text-align:left;"> Eritrea </td>
   <td style="text-align:left;"> ERI </td>
   <td style="text-align:right;"> 1997 </td>
   <td style="text-align:right;"> 11.2878163 </td>
   <td style="text-align:left;"> Sub-Saharan Africa </td>
   <td style="text-align:left;"> Africa </td>
  </tr>
  <tr>
   <td style="text-align:left;"> Switzerland </td>
   <td style="text-align:left;"> CHE </td>
   <td style="text-align:right;"> 1990 </td>
   <td style="text-align:right;"> 0.0091008 </td>
   <td style="text-align:left;"> Europe &amp; Central Asia </td>
   <td style="text-align:left;"> Europe </td>
  </tr>
  <tr>
   <td style="text-align:left;"> Malawi </td>
   <td style="text-align:left;"> MWI </td>
   <td style="text-align:right;"> 1995 </td>
   <td style="text-align:right;"> 10.0860940 </td>
   <td style="text-align:left;"> Sub-Saharan Africa </td>
   <td style="text-align:left;"> Africa </td>
  </tr>
  <tr>
   <td style="text-align:left;"> Turkey </td>
   <td style="text-align:left;"> TUR </td>
   <td style="text-align:right;"> 2019 </td>
   <td style="text-align:right;"> 0.0508833 </td>
   <td style="text-align:left;"> Europe &amp; Central Asia </td>
   <td style="text-align:left;"> Asia </td>
  </tr>
  <tr>
   <td style="text-align:left;"> Chile </td>
   <td style="text-align:left;"> CHL </td>
   <td style="text-align:right;"> 2015 </td>
   <td style="text-align:right;"> 0.3647605 </td>
   <td style="text-align:left;"> Latin America &amp; Caribbean </td>
   <td style="text-align:left;"> Americas </td>
  </tr>
  <tr>
   <td style="text-align:left;"> Grenada </td>
   <td style="text-align:left;"> GRD </td>
   <td style="text-align:right;"> 2011 </td>
   <td style="text-align:right;"> 0.4904653 </td>
   <td style="text-align:left;"> Latin America &amp; Caribbean </td>
   <td style="text-align:left;"> Americas </td>
  </tr>
  <tr>
   <td style="text-align:left;"> Nauru </td>
   <td style="text-align:left;"> NRU </td>
   <td style="text-align:right;"> 2014 </td>
   <td style="text-align:right;"> 0.9607399 </td>
   <td style="text-align:left;"> East Asia &amp; Pacific </td>
   <td style="text-align:left;"> Oceania </td>
  </tr>
  <tr>
   <td style="text-align:left;"> Venezuela </td>
   <td style="text-align:left;"> VEN </td>
   <td style="text-align:right;"> 1999 </td>
   <td style="text-align:right;"> 0.2037482 </td>
   <td style="text-align:left;"> Latin America &amp; Caribbean </td>
   <td style="text-align:left;"> Americas </td>
  </tr>
  <tr>
   <td style="text-align:left;"> North Macedonia </td>
   <td style="text-align:left;"> MKD </td>
   <td style="text-align:right;"> 2000 </td>
   <td style="text-align:right;"> 4.7697229 </td>
   <td style="text-align:left;"> Europe &amp; Central Asia </td>
   <td style="text-align:left;"> Europe </td>
  </tr>
  <tr>
   <td style="text-align:left;"> Togo </td>
   <td style="text-align:left;"> TGO </td>
   <td style="text-align:right;"> 1994 </td>
   <td style="text-align:right;"> 13.2028908 </td>
   <td style="text-align:left;"> Sub-Saharan Africa </td>
   <td style="text-align:left;"> Africa </td>
  </tr>
  <tr>
   <td style="text-align:left;"> United Arab Emirates </td>
   <td style="text-align:left;"> ARE </td>
   <td style="text-align:right;"> 2000 </td>
   <td style="text-align:right;"> 0.0086346 </td>
   <td style="text-align:left;"> Middle East &amp; North Africa </td>
   <td style="text-align:left;"> Asia </td>
  </tr>
  <tr>
   <td style="text-align:left;"> Sweden </td>
   <td style="text-align:left;"> SWE </td>
   <td style="text-align:right;"> 2012 </td>
   <td style="text-align:right;"> 0.0054739 </td>
   <td style="text-align:left;"> Europe &amp; Central Asia </td>
   <td style="text-align:left;"> Europe </td>
  </tr>
  <tr>
   <td style="text-align:left;"> Hungary </td>
   <td style="text-align:left;"> HUN </td>
   <td style="text-align:right;"> 2011 </td>
   <td style="text-align:right;"> 1.3359497 </td>
   <td style="text-align:left;"> Europe &amp; Central Asia </td>
   <td style="text-align:left;"> Europe </td>
  </tr>
  <tr>
   <td style="text-align:left;"> Morocco </td>
   <td style="text-align:left;"> MAR </td>
   <td style="text-align:right;"> 2015 </td>
   <td style="text-align:right;"> 1.2651961 </td>
   <td style="text-align:left;"> Middle East &amp; North Africa </td>
   <td style="text-align:left;"> Africa </td>
  </tr>
  <tr>
   <td style="text-align:left;"> Portugal </td>
   <td style="text-align:left;"> PRT </td>
   <td style="text-align:right;"> 1993 </td>
   <td style="text-align:right;"> 0.4130288 </td>
   <td style="text-align:left;"> Europe &amp; Central Asia </td>
   <td style="text-align:left;"> Europe </td>
  </tr>
  <tr>
   <td style="text-align:left;"> Afghanistan </td>
   <td style="text-align:left;"> AFG </td>
   <td style="text-align:right;"> 2013 </td>
   <td style="text-align:right;"> 15.8105333 </td>
   <td style="text-align:left;"> South Asia </td>
   <td style="text-align:left;"> Asia </td>
  </tr>
  <tr>
   <td style="text-align:left;"> Poland </td>
   <td style="text-align:left;"> POL </td>
   <td style="text-align:right;"> 2016 </td>
   <td style="text-align:right;"> 0.7824107 </td>
   <td style="text-align:left;"> Europe &amp; Central Asia </td>
   <td style="text-align:left;"> Europe </td>
  </tr>
  <tr>
   <td style="text-align:left;"> Norway </td>
   <td style="text-align:left;"> NOR </td>
   <td style="text-align:right;"> 1995 </td>
   <td style="text-align:right;"> 0.0154846 </td>
   <td style="text-align:left;"> Europe &amp; Central Asia </td>
   <td style="text-align:left;"> Europe </td>
  </tr>
  <tr>
   <td style="text-align:left;"> Cyprus </td>
   <td style="text-align:left;"> CYP </td>
   <td style="text-align:right;"> 2011 </td>
   <td style="text-align:right;"> 0.0137861 </td>
   <td style="text-align:left;"> Europe &amp; Central Asia </td>
   <td style="text-align:left;"> Asia </td>
  </tr>
  <tr>
   <td style="text-align:left;"> Equatorial Guinea </td>
   <td style="text-align:left;"> GNQ </td>
   <td style="text-align:right;"> 2004 </td>
   <td style="text-align:right;"> 7.7939131 </td>
   <td style="text-align:left;"> Sub-Saharan Africa </td>
   <td style="text-align:left;"> Africa </td>
  </tr>
  <tr>
   <td style="text-align:left;"> Eritrea </td>
   <td style="text-align:left;"> ERI </td>
   <td style="text-align:right;"> 2015 </td>
   <td style="text-align:right;"> 10.9686155 </td>
   <td style="text-align:left;"> Sub-Saharan Africa </td>
   <td style="text-align:left;"> Africa </td>
  </tr>
  <tr>
   <td style="text-align:left;"> Nepal </td>
   <td style="text-align:left;"> NPL </td>
   <td style="text-align:right;"> 2009 </td>
   <td style="text-align:right;"> 16.3618971 </td>
   <td style="text-align:left;"> South Asia </td>
   <td style="text-align:left;"> Asia </td>
  </tr>
  <tr>
   <td style="text-align:left;"> Slovakia </td>
   <td style="text-align:left;"> SVK </td>
   <td style="text-align:right;"> 1996 </td>
   <td style="text-align:right;"> 0.2267970 </td>
   <td style="text-align:left;"> Europe &amp; Central Asia </td>
   <td style="text-align:left;"> Europe </td>
  </tr>
  <tr>
   <td style="text-align:left;"> Gambia </td>
   <td style="text-align:left;"> GMB </td>
   <td style="text-align:right;"> 2007 </td>
   <td style="text-align:right;"> 13.1392506 </td>
   <td style="text-align:left;"> Sub-Saharan Africa </td>
   <td style="text-align:left;"> Africa </td>
  </tr>
  <tr>
   <td style="text-align:left;"> Angola </td>
   <td style="text-align:left;"> AGO </td>
   <td style="text-align:right;"> 2013 </td>
   <td style="text-align:right;"> 8.3056871 </td>
   <td style="text-align:left;"> Sub-Saharan Africa </td>
   <td style="text-align:left;"> Africa </td>
  </tr>
  <tr>
   <td style="text-align:left;"> Cameroon </td>
   <td style="text-align:left;"> CMR </td>
   <td style="text-align:right;"> 2015 </td>
   <td style="text-align:right;"> 7.2319903 </td>
   <td style="text-align:left;"> Sub-Saharan Africa </td>
   <td style="text-align:left;"> Africa </td>
  </tr>
  <tr>
   <td style="text-align:left;"> Serbia </td>
   <td style="text-align:left;"> SRB </td>
   <td style="text-align:right;"> 2001 </td>
   <td style="text-align:right;"> 4.5010029 </td>
   <td style="text-align:left;"> Europe &amp; Central Asia </td>
   <td style="text-align:left;"> Europe </td>
  </tr>
  <tr>
   <td style="text-align:left;"> Austria </td>
   <td style="text-align:left;"> AUT </td>
   <td style="text-align:right;"> 2015 </td>
   <td style="text-align:right;"> 0.0129256 </td>
   <td style="text-align:left;"> Europe &amp; Central Asia </td>
   <td style="text-align:left;"> Europe </td>
  </tr>
  <tr>
   <td style="text-align:left;"> Saudi Arabia </td>
   <td style="text-align:left;"> SAU </td>
   <td style="text-align:right;"> 1992 </td>
   <td style="text-align:right;"> 5.2105199 </td>
   <td style="text-align:left;"> Middle East &amp; North Africa </td>
   <td style="text-align:left;"> Asia </td>
  </tr>
  <tr>
   <td style="text-align:left;"> Chile </td>
   <td style="text-align:left;"> CHL </td>
   <td style="text-align:right;"> 1992 </td>
   <td style="text-align:right;"> 2.4568508 </td>
   <td style="text-align:left;"> Latin America &amp; Caribbean </td>
   <td style="text-align:left;"> Americas </td>
  </tr>
  <tr>
   <td style="text-align:left;"> South Sudan </td>
   <td style="text-align:left;"> SSD </td>
   <td style="text-align:right;"> 1990 </td>
   <td style="text-align:right;"> 13.2033478 </td>
   <td style="text-align:left;"> Sub-Saharan Africa </td>
   <td style="text-align:left;"> Africa </td>
  </tr>
  <tr>
   <td style="text-align:left;"> Algeria </td>
   <td style="text-align:left;"> DZA </td>
   <td style="text-align:right;"> 1990 </td>
   <td style="text-align:right;"> 2.3047240 </td>
   <td style="text-align:left;"> Middle East &amp; North Africa </td>
   <td style="text-align:left;"> Africa </td>
  </tr>
  <tr>
   <td style="text-align:left;"> Guam </td>
   <td style="text-align:left;"> GUM </td>
   <td style="text-align:right;"> 1999 </td>
   <td style="text-align:right;"> 0.4643495 </td>
   <td style="text-align:left;"> East Asia &amp; Pacific </td>
   <td style="text-align:left;"> Oceania </td>
  </tr>
  <tr>
   <td style="text-align:left;"> Bahrain </td>
   <td style="text-align:left;"> BHR </td>
   <td style="text-align:right;"> 2001 </td>
   <td style="text-align:right;"> 0.3519369 </td>
   <td style="text-align:left;"> Middle East &amp; North Africa </td>
   <td style="text-align:left;"> Asia </td>
  </tr>
  <tr>
   <td style="text-align:left;"> South Korea </td>
   <td style="text-align:left;"> KOR </td>
   <td style="text-align:right;"> 2007 </td>
   <td style="text-align:right;"> 0.0131833 </td>
   <td style="text-align:left;"> East Asia &amp; Pacific </td>
   <td style="text-align:left;"> Asia </td>
  </tr>
  <tr>
   <td style="text-align:left;"> Northern Mariana Islands </td>
   <td style="text-align:left;"> MNP </td>
   <td style="text-align:right;"> 2008 </td>
   <td style="text-align:right;"> 1.3109977 </td>
   <td style="text-align:left;"> East Asia &amp; Pacific </td>
   <td style="text-align:left;"> Oceania </td>
  </tr>
  <tr>
   <td style="text-align:left;"> Finland </td>
   <td style="text-align:left;"> FIN </td>
   <td style="text-align:right;"> 2001 </td>
   <td style="text-align:right;"> 0.0187513 </td>
   <td style="text-align:left;"> Europe &amp; Central Asia </td>
   <td style="text-align:left;"> Europe </td>
  </tr>
  <tr>
   <td style="text-align:left;"> Poland </td>
   <td style="text-align:left;"> POL </td>
   <td style="text-align:right;"> 2004 </td>
   <td style="text-align:right;"> 1.4518677 </td>
   <td style="text-align:left;"> Europe &amp; Central Asia </td>
   <td style="text-align:left;"> Europe </td>
  </tr>
  <tr>
   <td style="text-align:left;"> Bahamas </td>
   <td style="text-align:left;"> BHS </td>
   <td style="text-align:right;"> 2013 </td>
   <td style="text-align:right;"> 0.1067973 </td>
   <td style="text-align:left;"> Latin America &amp; Caribbean </td>
   <td style="text-align:left;"> Americas </td>
  </tr>
  <tr>
   <td style="text-align:left;"> Senegal </td>
   <td style="text-align:left;"> SEN </td>
   <td style="text-align:right;"> 2015 </td>
   <td style="text-align:right;"> 11.0460317 </td>
   <td style="text-align:left;"> Sub-Saharan Africa </td>
   <td style="text-align:left;"> Africa </td>
  </tr>
  <tr>
   <td style="text-align:left;"> Nigeria </td>
   <td style="text-align:left;"> NGA </td>
   <td style="text-align:right;"> 2007 </td>
   <td style="text-align:right;"> 9.6985109 </td>
   <td style="text-align:left;"> Sub-Saharan Africa </td>
   <td style="text-align:left;"> Africa </td>
  </tr>
  <tr>
   <td style="text-align:left;"> Estonia </td>
   <td style="text-align:left;"> EST </td>
   <td style="text-align:right;"> 2015 </td>
   <td style="text-align:right;"> 0.2934446 </td>
   <td style="text-align:left;"> Europe &amp; Central Asia </td>
   <td style="text-align:left;"> Europe </td>
  </tr>
  <tr>
   <td style="text-align:left;"> New Zealand </td>
   <td style="text-align:left;"> NZL </td>
   <td style="text-align:right;"> 2012 </td>
   <td style="text-align:right;"> 0.0107646 </td>
   <td style="text-align:left;"> East Asia &amp; Pacific </td>
   <td style="text-align:left;"> Oceania </td>
  </tr>
  <tr>
   <td style="text-align:left;"> Mozambique </td>
   <td style="text-align:left;"> MOZ </td>
   <td style="text-align:right;"> 2009 </td>
   <td style="text-align:right;"> 11.5026051 </td>
   <td style="text-align:left;"> Sub-Saharan Africa </td>
   <td style="text-align:left;"> Africa </td>
  </tr>
  <tr>
   <td style="text-align:left;"> Sudan </td>
   <td style="text-align:left;"> SDN </td>
   <td style="text-align:right;"> 2014 </td>
   <td style="text-align:right;"> 9.1565292 </td>
   <td style="text-align:left;"> Sub-Saharan Africa </td>
   <td style="text-align:left;"> Africa </td>
  </tr>
  <tr>
   <td style="text-align:left;"> Cook Islands </td>
   <td style="text-align:left;"> COK </td>
   <td style="text-align:right;"> 2001 </td>
   <td style="text-align:right;"> 0.9131933 </td>
   <td style="text-align:left;"> East Asia &amp; Pacific </td>
   <td style="text-align:left;"> Oceania </td>
  </tr>
  <tr>
   <td style="text-align:left;"> Lebanon </td>
   <td style="text-align:left;"> LBN </td>
   <td style="text-align:right;"> 2007 </td>
   <td style="text-align:right;"> 0.1865496 </td>
   <td style="text-align:left;"> Middle East &amp; North Africa </td>
   <td style="text-align:left;"> Asia </td>
  </tr>
  <tr>
   <td style="text-align:left;"> United States Virgin Islands </td>
   <td style="text-align:left;"> VIR </td>
   <td style="text-align:right;"> 1990 </td>
   <td style="text-align:right;"> 0.4319072 </td>
   <td style="text-align:left;"> Latin America &amp; Caribbean </td>
   <td style="text-align:left;"> Americas </td>
  </tr>
  <tr>
   <td style="text-align:left;"> Bosnia and Herzegovina </td>
   <td style="text-align:left;"> BIH </td>
   <td style="text-align:right;"> 1998 </td>
   <td style="text-align:right;"> 8.8127849 </td>
   <td style="text-align:left;"> Europe &amp; Central Asia </td>
   <td style="text-align:left;"> Europe </td>
  </tr>
  <tr>
   <td style="text-align:left;"> Estonia </td>
   <td style="text-align:left;"> EST </td>
   <td style="text-align:right;"> 2009 </td>
   <td style="text-align:right;"> 0.5605938 </td>
   <td style="text-align:left;"> Europe &amp; Central Asia </td>
   <td style="text-align:left;"> Europe </td>
  </tr>
  <tr>
   <td style="text-align:left;"> Greenland </td>
   <td style="text-align:left;"> GRL </td>
   <td style="text-align:right;"> 2011 </td>
   <td style="text-align:right;"> 0.1003583 </td>
   <td style="text-align:left;"> Europe &amp; Central Asia </td>
   <td style="text-align:left;"> Americas </td>
  </tr>
  <tr>
   <td style="text-align:left;"> Senegal </td>
   <td style="text-align:left;"> SEN </td>
   <td style="text-align:right;"> 2017 </td>
   <td style="text-align:right;"> 10.8681935 </td>
   <td style="text-align:left;"> Sub-Saharan Africa </td>
   <td style="text-align:left;"> Africa </td>
  </tr>
  <tr>
   <td style="text-align:left;"> Gambia </td>
   <td style="text-align:left;"> GMB </td>
   <td style="text-align:right;"> 1996 </td>
   <td style="text-align:right;"> 13.7428207 </td>
   <td style="text-align:left;"> Sub-Saharan Africa </td>
   <td style="text-align:left;"> Africa </td>
  </tr>
  <tr>
   <td style="text-align:left;"> Zimbabwe </td>
   <td style="text-align:left;"> ZWE </td>
   <td style="text-align:right;"> 1992 </td>
   <td style="text-align:right;"> 9.1460869 </td>
   <td style="text-align:left;"> Sub-Saharan Africa </td>
   <td style="text-align:left;"> Africa </td>
  </tr>
  <tr>
   <td style="text-align:left;"> Tunisia </td>
   <td style="text-align:left;"> TUN </td>
   <td style="text-align:right;"> 2012 </td>
   <td style="text-align:right;"> 0.0733820 </td>
   <td style="text-align:left;"> Middle East &amp; North Africa </td>
   <td style="text-align:left;"> Africa </td>
  </tr>
  <tr>
   <td style="text-align:left;"> Ghana </td>
   <td style="text-align:left;"> GHA </td>
   <td style="text-align:right;"> 2008 </td>
   <td style="text-align:right;"> 8.6931552 </td>
   <td style="text-align:left;"> Sub-Saharan Africa </td>
   <td style="text-align:left;"> Africa </td>
  </tr>
  <tr>
   <td style="text-align:left;"> Panama </td>
   <td style="text-align:left;"> PAN </td>
   <td style="text-align:right;"> 1993 </td>
   <td style="text-align:right;"> 4.2928954 </td>
   <td style="text-align:left;"> Latin America &amp; Caribbean </td>
   <td style="text-align:left;"> Americas </td>
  </tr>
  <tr>
   <td style="text-align:left;"> Trinidad and Tobago </td>
   <td style="text-align:left;"> TTO </td>
   <td style="text-align:right;"> 2015 </td>
   <td style="text-align:right;"> 0.0139163 </td>
   <td style="text-align:left;"> Latin America &amp; Caribbean </td>
   <td style="text-align:left;"> Americas </td>
  </tr>
  <tr>
   <td style="text-align:left;"> North Macedonia </td>
   <td style="text-align:left;"> MKD </td>
   <td style="text-align:right;"> 2015 </td>
   <td style="text-align:right;"> 2.4448995 </td>
   <td style="text-align:left;"> Europe &amp; Central Asia </td>
   <td style="text-align:left;"> Europe </td>
  </tr>
  <tr>
   <td style="text-align:left;"> Malawi </td>
   <td style="text-align:left;"> MWI </td>
   <td style="text-align:right;"> 2005 </td>
   <td style="text-align:right;"> 9.0748751 </td>
   <td style="text-align:left;"> Sub-Saharan Africa </td>
   <td style="text-align:left;"> Africa </td>
  </tr>
  <tr>
   <td style="text-align:left;"> Canada </td>
   <td style="text-align:left;"> CAN </td>
   <td style="text-align:right;"> 2007 </td>
   <td style="text-align:right;"> 0.0089040 </td>
   <td style="text-align:left;"> North America </td>
   <td style="text-align:left;"> Americas </td>
  </tr>
  <tr>
   <td style="text-align:left;"> Luxembourg </td>
   <td style="text-align:left;"> LUX </td>
   <td style="text-align:right;"> 2012 </td>
   <td style="text-align:right;"> 0.0076711 </td>
   <td style="text-align:left;"> Europe &amp; Central Asia </td>
   <td style="text-align:left;"> Europe </td>
  </tr>
  <tr>
   <td style="text-align:left;"> Kenya </td>
   <td style="text-align:left;"> KEN </td>
   <td style="text-align:right;"> 2007 </td>
   <td style="text-align:right;"> 8.7403164 </td>
   <td style="text-align:left;"> Sub-Saharan Africa </td>
   <td style="text-align:left;"> Africa </td>
  </tr>
  <tr>
   <td style="text-align:left;"> Bosnia and Herzegovina </td>
   <td style="text-align:left;"> BIH </td>
   <td style="text-align:right;"> 1990 </td>
   <td style="text-align:right;"> 10.7870048 </td>
   <td style="text-align:left;"> Europe &amp; Central Asia </td>
   <td style="text-align:left;"> Europe </td>
  </tr>
  <tr>
   <td style="text-align:left;"> Finland </td>
   <td style="text-align:left;"> FIN </td>
   <td style="text-align:right;"> 1998 </td>
   <td style="text-align:right;"> 0.0244447 </td>
   <td style="text-align:left;"> Europe &amp; Central Asia </td>
   <td style="text-align:left;"> Europe </td>
  </tr>
  <tr>
   <td style="text-align:left;"> Nauru </td>
   <td style="text-align:left;"> NRU </td>
   <td style="text-align:right;"> 1996 </td>
   <td style="text-align:right;"> 3.0516638 </td>
   <td style="text-align:left;"> East Asia &amp; Pacific </td>
   <td style="text-align:left;"> Oceania </td>
  </tr>
  <tr>
   <td style="text-align:left;"> El Salvador </td>
   <td style="text-align:left;"> SLV </td>
   <td style="text-align:right;"> 2017 </td>
   <td style="text-align:right;"> 1.9378980 </td>
   <td style="text-align:left;"> Latin America &amp; Caribbean </td>
   <td style="text-align:left;"> Americas </td>
  </tr>
  <tr>
   <td style="text-align:left;"> Mongolia </td>
   <td style="text-align:left;"> MNG </td>
   <td style="text-align:right;"> 2009 </td>
   <td style="text-align:right;"> 7.5528064 </td>
   <td style="text-align:left;"> East Asia &amp; Pacific </td>
   <td style="text-align:left;"> Asia </td>
  </tr>
  <tr>
   <td style="text-align:left;"> Azerbaijan </td>
   <td style="text-align:left;"> AZE </td>
   <td style="text-align:right;"> 2015 </td>
   <td style="text-align:right;"> 1.0812200 </td>
   <td style="text-align:left;"> Europe &amp; Central Asia </td>
   <td style="text-align:left;"> Asia </td>
  </tr>
  <tr>
   <td style="text-align:left;"> Afghanistan </td>
   <td style="text-align:left;"> AFG </td>
   <td style="text-align:right;"> 1998 </td>
   <td style="text-align:right;"> 19.0267890 </td>
   <td style="text-align:left;"> South Asia </td>
   <td style="text-align:left;"> Asia </td>
  </tr>
  <tr>
   <td style="text-align:left;"> Guatemala </td>
   <td style="text-align:left;"> GTM </td>
   <td style="text-align:right;"> 1991 </td>
   <td style="text-align:right;"> 12.3197442 </td>
   <td style="text-align:left;"> Latin America &amp; Caribbean </td>
   <td style="text-align:left;"> Americas </td>
  </tr>
  <tr>
   <td style="text-align:left;"> Croatia </td>
   <td style="text-align:left;"> HRV </td>
   <td style="text-align:right;"> 2015 </td>
   <td style="text-align:right;"> 0.3806202 </td>
   <td style="text-align:left;"> Europe &amp; Central Asia </td>
   <td style="text-align:left;"> Europe </td>
  </tr>
  <tr>
   <td style="text-align:left;"> Philippines </td>
   <td style="text-align:left;"> PHL </td>
   <td style="text-align:right;"> 2000 </td>
   <td style="text-align:right;"> 8.6057457 </td>
   <td style="text-align:left;"> East Asia &amp; Pacific </td>
   <td style="text-align:left;"> Asia </td>
  </tr>
  <tr>
   <td style="text-align:left;"> Moldova </td>
   <td style="text-align:left;"> MDA </td>
   <td style="text-align:right;"> 1992 </td>
   <td style="text-align:right;"> 7.3808254 </td>
   <td style="text-align:left;"> Europe &amp; Central Asia </td>
   <td style="text-align:left;"> Europe </td>
  </tr>
  <tr>
   <td style="text-align:left;"> Peru </td>
   <td style="text-align:left;"> PER </td>
   <td style="text-align:right;"> 2018 </td>
   <td style="text-align:right;"> 1.7546741 </td>
   <td style="text-align:left;"> Latin America &amp; Caribbean </td>
   <td style="text-align:left;"> Americas </td>
  </tr>
  <tr>
   <td style="text-align:left;"> Libya </td>
   <td style="text-align:left;"> LBY </td>
   <td style="text-align:right;"> 2011 </td>
   <td style="text-align:right;"> 0.0474830 </td>
   <td style="text-align:left;"> Middle East &amp; North Africa </td>
   <td style="text-align:left;"> Africa </td>
  </tr>
  <tr>
   <td style="text-align:left;"> Malta </td>
   <td style="text-align:left;"> MLT </td>
   <td style="text-align:right;"> 2000 </td>
   <td style="text-align:right;"> 0.0567653 </td>
   <td style="text-align:left;"> Middle East &amp; North Africa </td>
   <td style="text-align:left;"> Europe </td>
  </tr>
  <tr>
   <td style="text-align:left;"> Chile </td>
   <td style="text-align:left;"> CHL </td>
   <td style="text-align:right;"> 1998 </td>
   <td style="text-align:right;"> 1.4891698 </td>
   <td style="text-align:left;"> Latin America &amp; Caribbean </td>
   <td style="text-align:left;"> Americas </td>
  </tr>
  <tr>
   <td style="text-align:left;"> Nepal </td>
   <td style="text-align:left;"> NPL </td>
   <td style="text-align:right;"> 1991 </td>
   <td style="text-align:right;"> 19.1749025 </td>
   <td style="text-align:left;"> South Asia </td>
   <td style="text-align:left;"> Asia </td>
  </tr>
  <tr>
   <td style="text-align:left;"> Estonia </td>
   <td style="text-align:left;"> EST </td>
   <td style="text-align:right;"> 2018 </td>
   <td style="text-align:right;"> 0.2434099 </td>
   <td style="text-align:left;"> Europe &amp; Central Asia </td>
   <td style="text-align:left;"> Europe </td>
  </tr>
  <tr>
   <td style="text-align:left;"> Mauritania </td>
   <td style="text-align:left;"> MRT </td>
   <td style="text-align:right;"> 2001 </td>
   <td style="text-align:right;"> 9.7639395 </td>
   <td style="text-align:left;"> Sub-Saharan Africa </td>
   <td style="text-align:left;"> Africa </td>
  </tr>
  <tr>
   <td style="text-align:left;"> Brunei </td>
   <td style="text-align:left;"> BRN </td>
   <td style="text-align:right;"> 1995 </td>
   <td style="text-align:right;"> 0.5496835 </td>
   <td style="text-align:left;"> East Asia &amp; Pacific </td>
   <td style="text-align:left;"> Asia </td>
  </tr>
  <tr>
   <td style="text-align:left;"> Guatemala </td>
   <td style="text-align:left;"> GTM </td>
   <td style="text-align:right;"> 2001 </td>
   <td style="text-align:right;"> 10.8159063 </td>
   <td style="text-align:left;"> Latin America &amp; Caribbean </td>
   <td style="text-align:left;"> Americas </td>
  </tr>
  <tr>
   <td style="text-align:left;"> Luxembourg </td>
   <td style="text-align:left;"> LUX </td>
   <td style="text-align:right;"> 2000 </td>
   <td style="text-align:right;"> 0.0184923 </td>
   <td style="text-align:left;"> Europe &amp; Central Asia </td>
   <td style="text-align:left;"> Europe </td>
  </tr>
  <tr>
   <td style="text-align:left;"> Ireland </td>
   <td style="text-align:left;"> IRL </td>
   <td style="text-align:right;"> 2015 </td>
   <td style="text-align:right;"> 0.0079483 </td>
   <td style="text-align:left;"> Europe &amp; Central Asia </td>
   <td style="text-align:left;"> Europe </td>
  </tr>
  <tr>
   <td style="text-align:left;"> Russia </td>
   <td style="text-align:left;"> RUS </td>
   <td style="text-align:right;"> 1993 </td>
   <td style="text-align:right;"> 0.6755564 </td>
   <td style="text-align:left;"> Europe &amp; Central Asia </td>
   <td style="text-align:left;"> Europe </td>
  </tr>
  <tr>
   <td style="text-align:left;"> Yemen </td>
   <td style="text-align:left;"> YEM </td>
   <td style="text-align:right;"> 1999 </td>
   <td style="text-align:right;"> 13.7826099 </td>
   <td style="text-align:left;"> Middle East &amp; North Africa </td>
   <td style="text-align:left;"> Asia </td>
  </tr>
  <tr>
   <td style="text-align:left;"> Laos </td>
   <td style="text-align:left;"> LAO </td>
   <td style="text-align:right;"> 2005 </td>
   <td style="text-align:right;"> 17.3603887 </td>
   <td style="text-align:left;"> East Asia &amp; Pacific </td>
   <td style="text-align:left;"> Asia </td>
  </tr>
  <tr>
   <td style="text-align:left;"> Cook Islands </td>
   <td style="text-align:left;"> COK </td>
   <td style="text-align:right;"> 2012 </td>
   <td style="text-align:right;"> 0.5291710 </td>
   <td style="text-align:left;"> East Asia &amp; Pacific </td>
   <td style="text-align:left;"> Oceania </td>
  </tr>
  <tr>
   <td style="text-align:left;"> Chad </td>
   <td style="text-align:left;"> TCD </td>
   <td style="text-align:right;"> 1999 </td>
   <td style="text-align:right;"> 14.2308599 </td>
   <td style="text-align:left;"> Sub-Saharan Africa </td>
   <td style="text-align:left;"> Africa </td>
  </tr>
  <tr>
   <td style="text-align:left;"> Saint Vincent and the Grenadines </td>
   <td style="text-align:left;"> VCT </td>
   <td style="text-align:right;"> 2018 </td>
   <td style="text-align:right;"> 0.4460764 </td>
   <td style="text-align:left;"> Latin America &amp; Caribbean </td>
   <td style="text-align:left;"> Americas </td>
  </tr>
  <tr>
   <td style="text-align:left;"> Central African Republic </td>
   <td style="text-align:left;"> CAF </td>
   <td style="text-align:right;"> 2013 </td>
   <td style="text-align:right;"> 11.1805850 </td>
   <td style="text-align:left;"> Sub-Saharan Africa </td>
   <td style="text-align:left;"> Africa </td>
  </tr>
  <tr>
   <td style="text-align:left;"> Bosnia and Herzegovina </td>
   <td style="text-align:left;"> BIH </td>
   <td style="text-align:right;"> 2005 </td>
   <td style="text-align:right;"> 5.3704883 </td>
   <td style="text-align:left;"> Europe &amp; Central Asia </td>
   <td style="text-align:left;"> Europe </td>
  </tr>
  <tr>
   <td style="text-align:left;"> Costa Rica </td>
   <td style="text-align:left;"> CRI </td>
   <td style="text-align:right;"> 1992 </td>
   <td style="text-align:right;"> 3.6478186 </td>
   <td style="text-align:left;"> Latin America &amp; Caribbean </td>
   <td style="text-align:left;"> Americas </td>
  </tr>
  <tr>
   <td style="text-align:left;"> Russia </td>
   <td style="text-align:left;"> RUS </td>
   <td style="text-align:right;"> 2015 </td>
   <td style="text-align:right;"> 0.1580474 </td>
   <td style="text-align:left;"> Europe &amp; Central Asia </td>
   <td style="text-align:left;"> Europe </td>
  </tr>
  <tr>
   <td style="text-align:left;"> Burundi </td>
   <td style="text-align:left;"> BDI </td>
   <td style="text-align:right;"> 2018 </td>
   <td style="text-align:right;"> 14.4220801 </td>
   <td style="text-align:left;"> Sub-Saharan Africa </td>
   <td style="text-align:left;"> Africa </td>
  </tr>
  <tr>
   <td style="text-align:left;"> Italy </td>
   <td style="text-align:left;"> ITA </td>
   <td style="text-align:right;"> 1992 </td>
   <td style="text-align:right;"> 0.1223881 </td>
   <td style="text-align:left;"> Europe &amp; Central Asia </td>
   <td style="text-align:left;"> Europe </td>
  </tr>
  <tr>
   <td style="text-align:left;"> Italy </td>
   <td style="text-align:left;"> ITA </td>
   <td style="text-align:right;"> 2008 </td>
   <td style="text-align:right;"> 0.0342351 </td>
   <td style="text-align:left;"> Europe &amp; Central Asia </td>
   <td style="text-align:left;"> Europe </td>
  </tr>
  <tr>
   <td style="text-align:left;"> Mauritania </td>
   <td style="text-align:left;"> MRT </td>
   <td style="text-align:right;"> 2015 </td>
   <td style="text-align:right;"> 7.1764645 </td>
   <td style="text-align:left;"> Sub-Saharan Africa </td>
   <td style="text-align:left;"> Africa </td>
  </tr>
  <tr>
   <td style="text-align:left;"> Greenland </td>
   <td style="text-align:left;"> GRL </td>
   <td style="text-align:right;"> 1991 </td>
   <td style="text-align:right;"> 0.5564985 </td>
   <td style="text-align:left;"> Europe &amp; Central Asia </td>
   <td style="text-align:left;"> Americas </td>
  </tr>
  <tr>
   <td style="text-align:left;"> Cook Islands </td>
   <td style="text-align:left;"> COK </td>
   <td style="text-align:right;"> 1994 </td>
   <td style="text-align:right;"> 1.4951386 </td>
   <td style="text-align:left;"> East Asia &amp; Pacific </td>
   <td style="text-align:left;"> Oceania </td>
  </tr>
  <tr>
   <td style="text-align:left;"> United States </td>
   <td style="text-align:left;"> USA </td>
   <td style="text-align:right;"> 1991 </td>
   <td style="text-align:right;"> 0.0162699 </td>
   <td style="text-align:left;"> North America </td>
   <td style="text-align:left;"> Americas </td>
  </tr>
  <tr>
   <td style="text-align:left;"> Zambia </td>
   <td style="text-align:left;"> ZMB </td>
   <td style="text-align:right;"> 2013 </td>
   <td style="text-align:right;"> 8.9439838 </td>
   <td style="text-align:left;"> Sub-Saharan Africa </td>
   <td style="text-align:left;"> Africa </td>
  </tr>
  <tr>
   <td style="text-align:left;"> Andorra </td>
   <td style="text-align:left;"> AND </td>
   <td style="text-align:right;"> 1996 </td>
   <td style="text-align:right;"> 0.0143778 </td>
   <td style="text-align:left;"> Europe &amp; Central Asia </td>
   <td style="text-align:left;"> Europe </td>
  </tr>
  <tr>
   <td style="text-align:left;"> Colombia </td>
   <td style="text-align:left;"> COL </td>
   <td style="text-align:right;"> 2009 </td>
   <td style="text-align:right;"> 2.2387384 </td>
   <td style="text-align:left;"> Latin America &amp; Caribbean </td>
   <td style="text-align:left;"> Americas </td>
  </tr>
  <tr>
   <td style="text-align:left;"> Ethiopia </td>
   <td style="text-align:left;"> ETH </td>
   <td style="text-align:right;"> 2012 </td>
   <td style="text-align:right;"> 13.7125493 </td>
   <td style="text-align:left;"> Sub-Saharan Africa </td>
   <td style="text-align:left;"> Africa </td>
  </tr>
  <tr>
   <td style="text-align:left;"> Egypt </td>
   <td style="text-align:left;"> EGY </td>
   <td style="text-align:right;"> 2003 </td>
   <td style="text-align:right;"> 0.1976754 </td>
   <td style="text-align:left;"> Middle East &amp; North Africa </td>
   <td style="text-align:left;"> Africa </td>
  </tr>
  <tr>
   <td style="text-align:left;"> Belarus </td>
   <td style="text-align:left;"> BLR </td>
   <td style="text-align:right;"> 1998 </td>
   <td style="text-align:right;"> 0.5201996 </td>
   <td style="text-align:left;"> Europe &amp; Central Asia </td>
   <td style="text-align:left;"> Europe </td>
  </tr>
  <tr>
   <td style="text-align:left;"> Croatia </td>
   <td style="text-align:left;"> HRV </td>
   <td style="text-align:right;"> 1996 </td>
   <td style="text-align:right;"> 1.2612863 </td>
   <td style="text-align:left;"> Europe &amp; Central Asia </td>
   <td style="text-align:left;"> Europe </td>
  </tr>
  <tr>
   <td style="text-align:left;"> Singapore </td>
   <td style="text-align:left;"> SGP </td>
   <td style="text-align:right;"> 2002 </td>
   <td style="text-align:right;"> 0.0710783 </td>
   <td style="text-align:left;"> East Asia &amp; Pacific </td>
   <td style="text-align:left;"> Asia </td>
  </tr>
  <tr>
   <td style="text-align:left;"> Australia </td>
   <td style="text-align:left;"> AUS </td>
   <td style="text-align:right;"> 2019 </td>
   <td style="text-align:right;"> 0.0078252 </td>
   <td style="text-align:left;"> East Asia &amp; Pacific </td>
   <td style="text-align:left;"> Oceania </td>
  </tr>
  <tr>
   <td style="text-align:left;"> Botswana </td>
   <td style="text-align:left;"> BWA </td>
   <td style="text-align:right;"> 2006 </td>
   <td style="text-align:right;"> 4.5672332 </td>
   <td style="text-align:left;"> Sub-Saharan Africa </td>
   <td style="text-align:left;"> Africa </td>
  </tr>
  <tr>
   <td style="text-align:left;"> Greenland </td>
   <td style="text-align:left;"> GRL </td>
   <td style="text-align:right;"> 2010 </td>
   <td style="text-align:right;"> 0.1075665 </td>
   <td style="text-align:left;"> Europe &amp; Central Asia </td>
   <td style="text-align:left;"> Americas </td>
  </tr>
  <tr>
   <td style="text-align:left;"> Kiribati </td>
   <td style="text-align:left;"> KIR </td>
   <td style="text-align:right;"> 1999 </td>
   <td style="text-align:right;"> 13.6793235 </td>
   <td style="text-align:left;"> East Asia &amp; Pacific </td>
   <td style="text-align:left;"> Oceania </td>
  </tr>
  <tr>
   <td style="text-align:left;"> Ireland </td>
   <td style="text-align:left;"> IRL </td>
   <td style="text-align:right;"> 2012 </td>
   <td style="text-align:right;"> 0.0099438 </td>
   <td style="text-align:left;"> Europe &amp; Central Asia </td>
   <td style="text-align:left;"> Europe </td>
  </tr>
  <tr>
   <td style="text-align:left;"> Australia </td>
   <td style="text-align:left;"> AUS </td>
   <td style="text-align:right;"> 2010 </td>
   <td style="text-align:right;"> 0.0139809 </td>
   <td style="text-align:left;"> East Asia &amp; Pacific </td>
   <td style="text-align:left;"> Oceania </td>
  </tr>
  <tr>
   <td style="text-align:left;"> Central African Republic </td>
   <td style="text-align:left;"> CAF </td>
   <td style="text-align:right;"> 1997 </td>
   <td style="text-align:right;"> 11.2138393 </td>
   <td style="text-align:left;"> Sub-Saharan Africa </td>
   <td style="text-align:left;"> Africa </td>
  </tr>
  <tr>
   <td style="text-align:left;"> Qatar </td>
   <td style="text-align:left;"> QAT </td>
   <td style="text-align:right;"> 2010 </td>
   <td style="text-align:right;"> 0.0029019 </td>
   <td style="text-align:left;"> Middle East &amp; North Africa </td>
   <td style="text-align:left;"> Asia </td>
  </tr>
  <tr>
   <td style="text-align:left;"> South Africa </td>
   <td style="text-align:left;"> ZAF </td>
   <td style="text-align:right;"> 2001 </td>
   <td style="text-align:right;"> 3.0682570 </td>
   <td style="text-align:left;"> Sub-Saharan Africa </td>
   <td style="text-align:left;"> Africa </td>
  </tr>
  <tr>
   <td style="text-align:left;"> Suriname </td>
   <td style="text-align:left;"> SUR </td>
   <td style="text-align:right;"> 2003 </td>
   <td style="text-align:right;"> 2.8710795 </td>
   <td style="text-align:left;"> Latin America &amp; Caribbean </td>
   <td style="text-align:left;"> Americas </td>
  </tr>
  <tr>
   <td style="text-align:left;"> India </td>
   <td style="text-align:left;"> IND </td>
   <td style="text-align:right;"> 2011 </td>
   <td style="text-align:right;"> 9.3898966 </td>
   <td style="text-align:left;"> South Asia </td>
   <td style="text-align:left;"> Asia </td>
  </tr>
  <tr>
   <td style="text-align:left;"> Finland </td>
   <td style="text-align:left;"> FIN </td>
   <td style="text-align:right;"> 2016 </td>
   <td style="text-align:right;"> 0.0042083 </td>
   <td style="text-align:left;"> Europe &amp; Central Asia </td>
   <td style="text-align:left;"> Europe </td>
  </tr>
  <tr>
   <td style="text-align:left;"> Vanuatu </td>
   <td style="text-align:left;"> VUT </td>
   <td style="text-align:right;"> 2006 </td>
   <td style="text-align:right;"> 16.5392168 </td>
   <td style="text-align:left;"> East Asia &amp; Pacific </td>
   <td style="text-align:left;"> Oceania </td>
  </tr>
  <tr>
   <td style="text-align:left;"> Djibouti </td>
   <td style="text-align:left;"> DJI </td>
   <td style="text-align:right;"> 2018 </td>
   <td style="text-align:right;"> 3.1192126 </td>
   <td style="text-align:left;"> Middle East &amp; North Africa </td>
   <td style="text-align:left;"> Africa </td>
  </tr>
  <tr>
   <td style="text-align:left;"> Germany </td>
   <td style="text-align:left;"> DEU </td>
   <td style="text-align:right;"> 2004 </td>
   <td style="text-align:right;"> 0.0064865 </td>
   <td style="text-align:left;"> Europe &amp; Central Asia </td>
   <td style="text-align:left;"> Europe </td>
  </tr>
  <tr>
   <td style="text-align:left;"> Jamaica </td>
   <td style="text-align:left;"> JAM </td>
   <td style="text-align:right;"> 2012 </td>
   <td style="text-align:right;"> 1.5660270 </td>
   <td style="text-align:left;"> Latin America &amp; Caribbean </td>
   <td style="text-align:left;"> Americas </td>
  </tr>
  <tr>
   <td style="text-align:left;"> Sudan </td>
   <td style="text-align:left;"> SDN </td>
   <td style="text-align:right;"> 2006 </td>
   <td style="text-align:right;"> 13.0647204 </td>
   <td style="text-align:left;"> Sub-Saharan Africa </td>
   <td style="text-align:left;"> Africa </td>
  </tr>
  <tr>
   <td style="text-align:left;"> Guyana </td>
   <td style="text-align:left;"> GUY </td>
   <td style="text-align:right;"> 1995 </td>
   <td style="text-align:right;"> 3.8490914 </td>
   <td style="text-align:left;"> Latin America &amp; Caribbean </td>
   <td style="text-align:left;"> Americas </td>
  </tr>
  <tr>
   <td style="text-align:left;"> American Samoa </td>
   <td style="text-align:left;"> ASM </td>
   <td style="text-align:right;"> 1992 </td>
   <td style="text-align:right;"> 4.2303077 </td>
   <td style="text-align:left;"> East Asia &amp; Pacific </td>
   <td style="text-align:left;"> Oceania </td>
  </tr>
  <tr>
   <td style="text-align:left;"> Palau </td>
   <td style="text-align:left;"> PLW </td>
   <td style="text-align:right;"> 2001 </td>
   <td style="text-align:right;"> 0.0075242 </td>
   <td style="text-align:left;"> East Asia &amp; Pacific </td>
   <td style="text-align:left;"> Oceania </td>
  </tr>
  <tr>
   <td style="text-align:left;"> Paraguay </td>
   <td style="text-align:left;"> PRY </td>
   <td style="text-align:right;"> 1992 </td>
   <td style="text-align:right;"> 10.8018359 </td>
   <td style="text-align:left;"> Latin America &amp; Caribbean </td>
   <td style="text-align:left;"> Americas </td>
  </tr>
  <tr>
   <td style="text-align:left;"> Netherlands </td>
   <td style="text-align:left;"> NLD </td>
   <td style="text-align:right;"> 1998 </td>
   <td style="text-align:right;"> 0.0170386 </td>
   <td style="text-align:left;"> Europe &amp; Central Asia </td>
   <td style="text-align:left;"> Europe </td>
  </tr>
  <tr>
   <td style="text-align:left;"> Guatemala </td>
   <td style="text-align:left;"> GTM </td>
   <td style="text-align:right;"> 2003 </td>
   <td style="text-align:right;"> 10.1803154 </td>
   <td style="text-align:left;"> Latin America &amp; Caribbean </td>
   <td style="text-align:left;"> Americas </td>
  </tr>
  <tr>
   <td style="text-align:left;"> Seychelles </td>
   <td style="text-align:left;"> SYC </td>
   <td style="text-align:right;"> 2000 </td>
   <td style="text-align:right;"> 0.2588378 </td>
   <td style="text-align:left;"> Sub-Saharan Africa </td>
   <td style="text-align:left;"> Africa </td>
  </tr>
  <tr>
   <td style="text-align:left;"> Indonesia </td>
   <td style="text-align:left;"> IDN </td>
   <td style="text-align:right;"> 1994 </td>
   <td style="text-align:right;"> 9.2113799 </td>
   <td style="text-align:left;"> East Asia &amp; Pacific </td>
   <td style="text-align:left;"> Asia </td>
  </tr>
  <tr>
   <td style="text-align:left;"> Uruguay </td>
   <td style="text-align:left;"> URY </td>
   <td style="text-align:right;"> 2006 </td>
   <td style="text-align:right;"> 0.3653479 </td>
   <td style="text-align:left;"> Latin America &amp; Caribbean </td>
   <td style="text-align:left;"> Americas </td>
  </tr>
  <tr>
   <td style="text-align:left;"> Iran </td>
   <td style="text-align:left;"> IRN </td>
   <td style="text-align:right;"> 2018 </td>
   <td style="text-align:right;"> 0.0281230 </td>
   <td style="text-align:left;"> Middle East &amp; North Africa </td>
   <td style="text-align:left;"> Asia </td>
  </tr>
  <tr>
   <td style="text-align:left;"> Armenia </td>
   <td style="text-align:left;"> ARM </td>
   <td style="text-align:right;"> 2010 </td>
   <td style="text-align:right;"> 0.6908603 </td>
   <td style="text-align:left;"> Europe &amp; Central Asia </td>
   <td style="text-align:left;"> Asia </td>
  </tr>
  <tr>
   <td style="text-align:left;"> Lesotho </td>
   <td style="text-align:left;"> LSO </td>
   <td style="text-align:right;"> 2007 </td>
   <td style="text-align:right;"> 6.5778506 </td>
   <td style="text-align:left;"> Sub-Saharan Africa </td>
   <td style="text-align:left;"> Africa </td>
  </tr>
  <tr>
   <td style="text-align:left;"> South Sudan </td>
   <td style="text-align:left;"> SSD </td>
   <td style="text-align:right;"> 2017 </td>
   <td style="text-align:right;"> 10.8756470 </td>
   <td style="text-align:left;"> Sub-Saharan Africa </td>
   <td style="text-align:left;"> Africa </td>
  </tr>
  <tr>
   <td style="text-align:left;"> United States Virgin Islands </td>
   <td style="text-align:left;"> VIR </td>
   <td style="text-align:right;"> 2012 </td>
   <td style="text-align:right;"> 0.0794829 </td>
   <td style="text-align:left;"> Latin America &amp; Caribbean </td>
   <td style="text-align:left;"> Americas </td>
  </tr>
  <tr>
   <td style="text-align:left;"> Ireland </td>
   <td style="text-align:left;"> IRL </td>
   <td style="text-align:right;"> 2005 </td>
   <td style="text-align:right;"> 0.0183959 </td>
   <td style="text-align:left;"> Europe &amp; Central Asia </td>
   <td style="text-align:left;"> Europe </td>
  </tr>
  <tr>
   <td style="text-align:left;"> Pakistan </td>
   <td style="text-align:left;"> PAK </td>
   <td style="text-align:right;"> 2018 </td>
   <td style="text-align:right;"> 8.4835111 </td>
   <td style="text-align:left;"> South Asia </td>
   <td style="text-align:left;"> Asia </td>
  </tr>
  <tr>
   <td style="text-align:left;"> Nigeria </td>
   <td style="text-align:left;"> NGA </td>
   <td style="text-align:right;"> 2000 </td>
   <td style="text-align:right;"> 11.2497211 </td>
   <td style="text-align:left;"> Sub-Saharan Africa </td>
   <td style="text-align:left;"> Africa </td>
  </tr>
  <tr>
   <td style="text-align:left;"> Burundi </td>
   <td style="text-align:left;"> BDI </td>
   <td style="text-align:right;"> 2000 </td>
   <td style="text-align:right;"> 10.5143818 </td>
   <td style="text-align:left;"> Sub-Saharan Africa </td>
   <td style="text-align:left;"> Africa </td>
  </tr>
  <tr>
   <td style="text-align:left;"> Mexico </td>
   <td style="text-align:left;"> MEX </td>
   <td style="text-align:right;"> 2019 </td>
   <td style="text-align:right;"> 1.3642024 </td>
   <td style="text-align:left;"> Latin America &amp; Caribbean </td>
   <td style="text-align:left;"> Americas </td>
  </tr>
  <tr>
   <td style="text-align:left;"> Thailand </td>
   <td style="text-align:left;"> THA </td>
   <td style="text-align:right;"> 2017 </td>
   <td style="text-align:right;"> 1.6921479 </td>
   <td style="text-align:left;"> East Asia &amp; Pacific </td>
   <td style="text-align:left;"> Asia </td>
  </tr>
  <tr>
   <td style="text-align:left;"> Nauru </td>
   <td style="text-align:left;"> NRU </td>
   <td style="text-align:right;"> 1992 </td>
   <td style="text-align:right;"> 3.6092332 </td>
   <td style="text-align:left;"> East Asia &amp; Pacific </td>
   <td style="text-align:left;"> Oceania </td>
  </tr>
  <tr>
   <td style="text-align:left;"> Gambia </td>
   <td style="text-align:left;"> GMB </td>
   <td style="text-align:right;"> 1992 </td>
   <td style="text-align:right;"> 14.7750616 </td>
   <td style="text-align:left;"> Sub-Saharan Africa </td>
   <td style="text-align:left;"> Africa </td>
  </tr>
  <tr>
   <td style="text-align:left;"> Brazil </td>
   <td style="text-align:left;"> BRA </td>
   <td style="text-align:right;"> 2004 </td>
   <td style="text-align:right;"> 2.6693090 </td>
   <td style="text-align:left;"> Latin America &amp; Caribbean </td>
   <td style="text-align:left;"> Americas </td>
  </tr>
  <tr>
   <td style="text-align:left;"> Netherlands </td>
   <td style="text-align:left;"> NLD </td>
   <td style="text-align:right;"> 2003 </td>
   <td style="text-align:right;"> 0.0113799 </td>
   <td style="text-align:left;"> Europe &amp; Central Asia </td>
   <td style="text-align:left;"> Europe </td>
  </tr>
  <tr>
   <td style="text-align:left;"> Austria </td>
   <td style="text-align:left;"> AUT </td>
   <td style="text-align:right;"> 1996 </td>
   <td style="text-align:right;"> 0.0426625 </td>
   <td style="text-align:left;"> Europe &amp; Central Asia </td>
   <td style="text-align:left;"> Europe </td>
  </tr>
  <tr>
   <td style="text-align:left;"> Bolivia </td>
   <td style="text-align:left;"> BOL </td>
   <td style="text-align:right;"> 1997 </td>
   <td style="text-align:right;"> 6.5391058 </td>
   <td style="text-align:left;"> Latin America &amp; Caribbean </td>
   <td style="text-align:left;"> Americas </td>
  </tr>
  <tr>
   <td style="text-align:left;"> Slovakia </td>
   <td style="text-align:left;"> SVK </td>
   <td style="text-align:right;"> 2017 </td>
   <td style="text-align:right;"> 0.0669592 </td>
   <td style="text-align:left;"> Europe &amp; Central Asia </td>
   <td style="text-align:left;"> Europe </td>
  </tr>
  <tr>
   <td style="text-align:left;"> Mauritania </td>
   <td style="text-align:left;"> MRT </td>
   <td style="text-align:right;"> 2018 </td>
   <td style="text-align:right;"> 6.2685292 </td>
   <td style="text-align:left;"> Sub-Saharan Africa </td>
   <td style="text-align:left;"> Africa </td>
  </tr>
  <tr>
   <td style="text-align:left;"> Sierra Leone </td>
   <td style="text-align:left;"> SLE </td>
   <td style="text-align:right;"> 2014 </td>
   <td style="text-align:right;"> 12.0775612 </td>
   <td style="text-align:left;"> Sub-Saharan Africa </td>
   <td style="text-align:left;"> Africa </td>
  </tr>
  <tr>
   <td style="text-align:left;"> Cook Islands </td>
   <td style="text-align:left;"> COK </td>
   <td style="text-align:right;"> 1995 </td>
   <td style="text-align:right;"> 1.3896208 </td>
   <td style="text-align:left;"> East Asia &amp; Pacific </td>
   <td style="text-align:left;"> Oceania </td>
  </tr>
  <tr>
   <td style="text-align:left;"> Zambia </td>
   <td style="text-align:left;"> ZMB </td>
   <td style="text-align:right;"> 2012 </td>
   <td style="text-align:right;"> 9.0153351 </td>
   <td style="text-align:left;"> Sub-Saharan Africa </td>
   <td style="text-align:left;"> Africa </td>
  </tr>
  <tr>
   <td style="text-align:left;"> Haiti </td>
   <td style="text-align:left;"> HTI </td>
   <td style="text-align:right;"> 1994 </td>
   <td style="text-align:right;"> 15.1013937 </td>
   <td style="text-align:left;"> Latin America &amp; Caribbean </td>
   <td style="text-align:left;"> Americas </td>
  </tr>
  <tr>
   <td style="text-align:left;"> Comoros </td>
   <td style="text-align:left;"> COM </td>
   <td style="text-align:right;"> 2014 </td>
   <td style="text-align:right;"> 12.2528049 </td>
   <td style="text-align:left;"> Sub-Saharan Africa </td>
   <td style="text-align:left;"> Africa </td>
  </tr>
  <tr>
   <td style="text-align:left;"> South Korea </td>
   <td style="text-align:left;"> KOR </td>
   <td style="text-align:right;"> 1996 </td>
   <td style="text-align:right;"> 0.0754712 </td>
   <td style="text-align:left;"> East Asia &amp; Pacific </td>
   <td style="text-align:left;"> Asia </td>
  </tr>
  <tr>
   <td style="text-align:left;"> Argentina </td>
   <td style="text-align:left;"> ARG </td>
   <td style="text-align:right;"> 2018 </td>
   <td style="text-align:right;"> 0.2123095 </td>
   <td style="text-align:left;"> Latin America &amp; Caribbean </td>
   <td style="text-align:left;"> Americas </td>
  </tr>
  <tr>
   <td style="text-align:left;"> Malta </td>
   <td style="text-align:left;"> MLT </td>
   <td style="text-align:right;"> 2018 </td>
   <td style="text-align:right;"> 0.0111830 </td>
   <td style="text-align:left;"> Middle East &amp; North Africa </td>
   <td style="text-align:left;"> Europe </td>
  </tr>
  <tr>
   <td style="text-align:left;"> Netherlands </td>
   <td style="text-align:left;"> NLD </td>
   <td style="text-align:right;"> 1996 </td>
   <td style="text-align:right;"> 0.0196343 </td>
   <td style="text-align:left;"> Europe &amp; Central Asia </td>
   <td style="text-align:left;"> Europe </td>
  </tr>
  <tr>
   <td style="text-align:left;"> Kuwait </td>
   <td style="text-align:left;"> KWT </td>
   <td style="text-align:right;"> 2005 </td>
   <td style="text-align:right;"> 0.0287309 </td>
   <td style="text-align:left;"> Middle East &amp; North Africa </td>
   <td style="text-align:left;"> Asia </td>
  </tr>
  <tr>
   <td style="text-align:left;"> Niger </td>
   <td style="text-align:left;"> NER </td>
   <td style="text-align:right;"> 2007 </td>
   <td style="text-align:right;"> 15.2492386 </td>
   <td style="text-align:left;"> Sub-Saharan Africa </td>
   <td style="text-align:left;"> Africa </td>
  </tr>
  <tr>
   <td style="text-align:left;"> Spain </td>
   <td style="text-align:left;"> ESP </td>
   <td style="text-align:right;"> 2000 </td>
   <td style="text-align:right;"> 0.1723371 </td>
   <td style="text-align:left;"> Europe &amp; Central Asia </td>
   <td style="text-align:left;"> Europe </td>
  </tr>
  <tr>
   <td style="text-align:left;"> Kyrgyzstan </td>
   <td style="text-align:left;"> KGZ </td>
   <td style="text-align:right;"> 2006 </td>
   <td style="text-align:right;"> 8.3646495 </td>
   <td style="text-align:left;"> Europe &amp; Central Asia </td>
   <td style="text-align:left;"> Asia </td>
  </tr>
  <tr>
   <td style="text-align:left;"> Bulgaria </td>
   <td style="text-align:left;"> BGR </td>
   <td style="text-align:right;"> 1994 </td>
   <td style="text-align:right;"> 2.9284739 </td>
   <td style="text-align:left;"> Europe &amp; Central Asia </td>
   <td style="text-align:left;"> Europe </td>
  </tr>
  <tr>
   <td style="text-align:left;"> Mali </td>
   <td style="text-align:left;"> MLI </td>
   <td style="text-align:right;"> 2003 </td>
   <td style="text-align:right;"> 12.0562145 </td>
   <td style="text-align:left;"> Sub-Saharan Africa </td>
   <td style="text-align:left;"> Africa </td>
  </tr>
  <tr>
   <td style="text-align:left;"> Greenland </td>
   <td style="text-align:left;"> GRL </td>
   <td style="text-align:right;"> 1990 </td>
   <td style="text-align:right;"> 0.5601366 </td>
   <td style="text-align:left;"> Europe &amp; Central Asia </td>
   <td style="text-align:left;"> Americas </td>
  </tr>
  <tr>
   <td style="text-align:left;"> Bangladesh </td>
   <td style="text-align:left;"> BGD </td>
   <td style="text-align:right;"> 1998 </td>
   <td style="text-align:right;"> 16.2293767 </td>
   <td style="text-align:left;"> South Asia </td>
   <td style="text-align:left;"> Asia </td>
  </tr>
  <tr>
   <td style="text-align:left;"> Guinea </td>
   <td style="text-align:left;"> GIN </td>
   <td style="text-align:right;"> 2001 </td>
   <td style="text-align:right;"> 13.6495325 </td>
   <td style="text-align:left;"> Sub-Saharan Africa </td>
   <td style="text-align:left;"> Africa </td>
  </tr>
  <tr>
   <td style="text-align:left;"> Indonesia </td>
   <td style="text-align:left;"> IDN </td>
   <td style="text-align:right;"> 2000 </td>
   <td style="text-align:right;"> 7.8739882 </td>
   <td style="text-align:left;"> East Asia &amp; Pacific </td>
   <td style="text-align:left;"> Asia </td>
  </tr>
  <tr>
   <td style="text-align:left;"> Algeria </td>
   <td style="text-align:left;"> DZA </td>
   <td style="text-align:right;"> 2006 </td>
   <td style="text-align:right;"> 0.2516670 </td>
   <td style="text-align:left;"> Middle East &amp; North Africa </td>
   <td style="text-align:left;"> Africa </td>
  </tr>
  <tr>
   <td style="text-align:left;"> Kuwait </td>
   <td style="text-align:left;"> KWT </td>
   <td style="text-align:right;"> 2002 </td>
   <td style="text-align:right;"> 0.0409664 </td>
   <td style="text-align:left;"> Middle East &amp; North Africa </td>
   <td style="text-align:left;"> Asia </td>
  </tr>
  <tr>
   <td style="text-align:left;"> Syria </td>
   <td style="text-align:left;"> SYR </td>
   <td style="text-align:right;"> 2005 </td>
   <td style="text-align:right;"> 0.1458000 </td>
   <td style="text-align:left;"> Middle East &amp; North Africa </td>
   <td style="text-align:left;"> Asia </td>
  </tr>
  <tr>
   <td style="text-align:left;"> Laos </td>
   <td style="text-align:left;"> LAO </td>
   <td style="text-align:right;"> 2001 </td>
   <td style="text-align:right;"> 17.6510981 </td>
   <td style="text-align:left;"> East Asia &amp; Pacific </td>
   <td style="text-align:left;"> Asia </td>
  </tr>
  <tr>
   <td style="text-align:left;"> Sweden </td>
   <td style="text-align:left;"> SWE </td>
   <td style="text-align:right;"> 2001 </td>
   <td style="text-align:right;"> 0.0128307 </td>
   <td style="text-align:left;"> Europe &amp; Central Asia </td>
   <td style="text-align:left;"> Europe </td>
  </tr>
  <tr>
   <td style="text-align:left;"> Dominican Republic </td>
   <td style="text-align:left;"> DOM </td>
   <td style="text-align:right;"> 2019 </td>
   <td style="text-align:right;"> 1.3402564 </td>
   <td style="text-align:left;"> Latin America &amp; Caribbean </td>
   <td style="text-align:left;"> Americas </td>
  </tr>
  <tr>
   <td style="text-align:left;"> Dominica </td>
   <td style="text-align:left;"> DMA </td>
   <td style="text-align:right;"> 2006 </td>
   <td style="text-align:right;"> 1.1229164 </td>
   <td style="text-align:left;"> Latin America &amp; Caribbean </td>
   <td style="text-align:left;"> Americas </td>
  </tr>
  <tr>
   <td style="text-align:left;"> Bolivia </td>
   <td style="text-align:left;"> BOL </td>
   <td style="text-align:right;"> 2011 </td>
   <td style="text-align:right;"> 4.4992946 </td>
   <td style="text-align:left;"> Latin America &amp; Caribbean </td>
   <td style="text-align:left;"> Americas </td>
  </tr>
  <tr>
   <td style="text-align:left;"> Monaco </td>
   <td style="text-align:left;"> MCO </td>
   <td style="text-align:right;"> 2015 </td>
   <td style="text-align:right;"> 0.0032023 </td>
   <td style="text-align:left;"> Europe &amp; Central Asia </td>
   <td style="text-align:left;"> Europe </td>
  </tr>
  <tr>
   <td style="text-align:left;"> Togo </td>
   <td style="text-align:left;"> TGO </td>
   <td style="text-align:right;"> 2002 </td>
   <td style="text-align:right;"> 11.3269916 </td>
   <td style="text-align:left;"> Sub-Saharan Africa </td>
   <td style="text-align:left;"> Africa </td>
  </tr>
  <tr>
   <td style="text-align:left;"> Eritrea </td>
   <td style="text-align:left;"> ERI </td>
   <td style="text-align:right;"> 1999 </td>
   <td style="text-align:right;"> 8.7232291 </td>
   <td style="text-align:left;"> Sub-Saharan Africa </td>
   <td style="text-align:left;"> Africa </td>
  </tr>
  <tr>
   <td style="text-align:left;"> Zambia </td>
   <td style="text-align:left;"> ZMB </td>
   <td style="text-align:right;"> 1999 </td>
   <td style="text-align:right;"> 7.8706533 </td>
   <td style="text-align:left;"> Sub-Saharan Africa </td>
   <td style="text-align:left;"> Africa </td>
  </tr>
  <tr>
   <td style="text-align:left;"> Cook Islands </td>
   <td style="text-align:left;"> COK </td>
   <td style="text-align:right;"> 2018 </td>
   <td style="text-align:right;"> 0.4507596 </td>
   <td style="text-align:left;"> East Asia &amp; Pacific </td>
   <td style="text-align:left;"> Oceania </td>
  </tr>
  <tr>
   <td style="text-align:left;"> Mauritania </td>
   <td style="text-align:left;"> MRT </td>
   <td style="text-align:right;"> 2010 </td>
   <td style="text-align:right;"> 8.6557928 </td>
   <td style="text-align:left;"> Sub-Saharan Africa </td>
   <td style="text-align:left;"> Africa </td>
  </tr>
  <tr>
   <td style="text-align:left;"> Nigeria </td>
   <td style="text-align:left;"> NGA </td>
   <td style="text-align:right;"> 1999 </td>
   <td style="text-align:right;"> 11.4066836 </td>
   <td style="text-align:left;"> Sub-Saharan Africa </td>
   <td style="text-align:left;"> Africa </td>
  </tr>
  <tr>
   <td style="text-align:left;"> Democratic Republic of Congo </td>
   <td style="text-align:left;"> COD </td>
   <td style="text-align:right;"> 2000 </td>
   <td style="text-align:right;"> 13.2330455 </td>
   <td style="text-align:left;"> Sub-Saharan Africa </td>
   <td style="text-align:left;"> Africa </td>
  </tr>
  <tr>
   <td style="text-align:left;"> Gambia </td>
   <td style="text-align:left;"> GMB </td>
   <td style="text-align:right;"> 2017 </td>
   <td style="text-align:right;"> 12.4347909 </td>
   <td style="text-align:left;"> Sub-Saharan Africa </td>
   <td style="text-align:left;"> Africa </td>
  </tr>
  <tr>
   <td style="text-align:left;"> Laos </td>
   <td style="text-align:left;"> LAO </td>
   <td style="text-align:right;"> 2003 </td>
   <td style="text-align:right;"> 17.6034108 </td>
   <td style="text-align:left;"> East Asia &amp; Pacific </td>
   <td style="text-align:left;"> Asia </td>
  </tr>
  <tr>
   <td style="text-align:left;"> Bosnia and Herzegovina </td>
   <td style="text-align:left;"> BIH </td>
   <td style="text-align:right;"> 2014 </td>
   <td style="text-align:right;"> 3.4562693 </td>
   <td style="text-align:left;"> Europe &amp; Central Asia </td>
   <td style="text-align:left;"> Europe </td>
  </tr>
  <tr>
   <td style="text-align:left;"> Armenia </td>
   <td style="text-align:left;"> ARM </td>
   <td style="text-align:right;"> 1994 </td>
   <td style="text-align:right;"> 4.0610477 </td>
   <td style="text-align:left;"> Europe &amp; Central Asia </td>
   <td style="text-align:left;"> Asia </td>
  </tr>
  <tr>
   <td style="text-align:left;"> Pakistan </td>
   <td style="text-align:left;"> PAK </td>
   <td style="text-align:right;"> 2006 </td>
   <td style="text-align:right;"> 12.3296793 </td>
   <td style="text-align:left;"> South Asia </td>
   <td style="text-align:left;"> Asia </td>
  </tr>
  <tr>
   <td style="text-align:left;"> Cape Verde </td>
   <td style="text-align:left;"> CPV </td>
   <td style="text-align:right;"> 2008 </td>
   <td style="text-align:right;"> 6.2016812 </td>
   <td style="text-align:left;"> Sub-Saharan Africa </td>
   <td style="text-align:left;"> Africa </td>
  </tr>
  <tr>
   <td style="text-align:left;"> Cook Islands </td>
   <td style="text-align:left;"> COK </td>
   <td style="text-align:right;"> 2004 </td>
   <td style="text-align:right;"> 0.6953622 </td>
   <td style="text-align:left;"> East Asia &amp; Pacific </td>
   <td style="text-align:left;"> Oceania </td>
  </tr>
  <tr>
   <td style="text-align:left;"> Djibouti </td>
   <td style="text-align:left;"> DJI </td>
   <td style="text-align:right;"> 2011 </td>
   <td style="text-align:right;"> 3.7781850 </td>
   <td style="text-align:left;"> Middle East &amp; North Africa </td>
   <td style="text-align:left;"> Africa </td>
  </tr>
  <tr>
   <td style="text-align:left;"> Netherlands </td>
   <td style="text-align:left;"> NLD </td>
   <td style="text-align:right;"> 2006 </td>
   <td style="text-align:right;"> 0.0085465 </td>
   <td style="text-align:left;"> Europe &amp; Central Asia </td>
   <td style="text-align:left;"> Europe </td>
  </tr>
  <tr>
   <td style="text-align:left;"> Haiti </td>
   <td style="text-align:left;"> HTI </td>
   <td style="text-align:right;"> 2013 </td>
   <td style="text-align:right;"> 14.1148572 </td>
   <td style="text-align:left;"> Latin America &amp; Caribbean </td>
   <td style="text-align:left;"> Americas </td>
  </tr>
  <tr>
   <td style="text-align:left;"> Bolivia </td>
   <td style="text-align:left;"> BOL </td>
   <td style="text-align:right;"> 1995 </td>
   <td style="text-align:right;"> 6.8946627 </td>
   <td style="text-align:left;"> Latin America &amp; Caribbean </td>
   <td style="text-align:left;"> Americas </td>
  </tr>
  <tr>
   <td style="text-align:left;"> Madagascar </td>
   <td style="text-align:left;"> MDG </td>
   <td style="text-align:right;"> 1997 </td>
   <td style="text-align:right;"> 16.5287524 </td>
   <td style="text-align:left;"> Sub-Saharan Africa </td>
   <td style="text-align:left;"> Africa </td>
  </tr>
  <tr>
   <td style="text-align:left;"> Belize </td>
   <td style="text-align:left;"> BLZ </td>
   <td style="text-align:right;"> 2005 </td>
   <td style="text-align:right;"> 3.1448595 </td>
   <td style="text-align:left;"> Latin America &amp; Caribbean </td>
   <td style="text-align:left;"> Americas </td>
  </tr>
  <tr>
   <td style="text-align:left;"> Greece </td>
   <td style="text-align:left;"> GRC </td>
   <td style="text-align:right;"> 1998 </td>
   <td style="text-align:right;"> 0.1438124 </td>
   <td style="text-align:left;"> Europe &amp; Central Asia </td>
   <td style="text-align:left;"> Europe </td>
  </tr>
  <tr>
   <td style="text-align:left;"> Marshall Islands </td>
   <td style="text-align:left;"> MHL </td>
   <td style="text-align:right;"> 1992 </td>
   <td style="text-align:right;"> 10.3359536 </td>
   <td style="text-align:left;"> East Asia &amp; Pacific </td>
   <td style="text-align:left;"> Oceania </td>
  </tr>
  <tr>
   <td style="text-align:left;"> Bermuda </td>
   <td style="text-align:left;"> BMU </td>
   <td style="text-align:right;"> 2012 </td>
   <td style="text-align:right;"> 0.1868545 </td>
   <td style="text-align:left;"> North America </td>
   <td style="text-align:left;"> Americas </td>
  </tr>
  <tr>
   <td style="text-align:left;"> South Korea </td>
   <td style="text-align:left;"> KOR </td>
   <td style="text-align:right;"> 2001 </td>
   <td style="text-align:right;"> 0.0333202 </td>
   <td style="text-align:left;"> East Asia &amp; Pacific </td>
   <td style="text-align:left;"> Asia </td>
  </tr>
  <tr>
   <td style="text-align:left;"> Cote d'Ivoire </td>
   <td style="text-align:left;"> CIV </td>
   <td style="text-align:right;"> 2015 </td>
   <td style="text-align:right;"> 10.1675605 </td>
   <td style="text-align:left;"> Sub-Saharan Africa </td>
   <td style="text-align:left;"> Africa </td>
  </tr>
  <tr>
   <td style="text-align:left;"> South Africa </td>
   <td style="text-align:left;"> ZAF </td>
   <td style="text-align:right;"> 1993 </td>
   <td style="text-align:right;"> 4.2848489 </td>
   <td style="text-align:left;"> Sub-Saharan Africa </td>
   <td style="text-align:left;"> Africa </td>
  </tr>
  <tr>
   <td style="text-align:left;"> South Sudan </td>
   <td style="text-align:left;"> SSD </td>
   <td style="text-align:right;"> 2004 </td>
   <td style="text-align:right;"> 11.6792902 </td>
   <td style="text-align:left;"> Sub-Saharan Africa </td>
   <td style="text-align:left;"> Africa </td>
  </tr>
  <tr>
   <td style="text-align:left;"> Philippines </td>
   <td style="text-align:left;"> PHL </td>
   <td style="text-align:right;"> 1999 </td>
   <td style="text-align:right;"> 8.4344036 </td>
   <td style="text-align:left;"> East Asia &amp; Pacific </td>
   <td style="text-align:left;"> Asia </td>
  </tr>
  <tr>
   <td style="text-align:left;"> Zambia </td>
   <td style="text-align:left;"> ZMB </td>
   <td style="text-align:right;"> 1996 </td>
   <td style="text-align:right;"> 8.5546781 </td>
   <td style="text-align:left;"> Sub-Saharan Africa </td>
   <td style="text-align:left;"> Africa </td>
  </tr>
  <tr>
   <td style="text-align:left;"> Norway </td>
   <td style="text-align:left;"> NOR </td>
   <td style="text-align:right;"> 2006 </td>
   <td style="text-align:right;"> 0.0061874 </td>
   <td style="text-align:left;"> Europe &amp; Central Asia </td>
   <td style="text-align:left;"> Europe </td>
  </tr>
  <tr>
   <td style="text-align:left;"> Mali </td>
   <td style="text-align:left;"> MLI </td>
   <td style="text-align:right;"> 1999 </td>
   <td style="text-align:right;"> 12.0392938 </td>
   <td style="text-align:left;"> Sub-Saharan Africa </td>
   <td style="text-align:left;"> Africa </td>
  </tr>
  <tr>
   <td style="text-align:left;"> Tuvalu </td>
   <td style="text-align:left;"> TUV </td>
   <td style="text-align:right;"> 1995 </td>
   <td style="text-align:right;"> 8.5480982 </td>
   <td style="text-align:left;"> East Asia &amp; Pacific </td>
   <td style="text-align:left;"> Oceania </td>
  </tr>
  <tr>
   <td style="text-align:left;"> Serbia </td>
   <td style="text-align:left;"> SRB </td>
   <td style="text-align:right;"> 2002 </td>
   <td style="text-align:right;"> 4.1986320 </td>
   <td style="text-align:left;"> Europe &amp; Central Asia </td>
   <td style="text-align:left;"> Europe </td>
  </tr>
  <tr>
   <td style="text-align:left;"> Mongolia </td>
   <td style="text-align:left;"> MNG </td>
   <td style="text-align:right;"> 1990 </td>
   <td style="text-align:right;"> 12.5047587 </td>
   <td style="text-align:left;"> East Asia &amp; Pacific </td>
   <td style="text-align:left;"> Asia </td>
  </tr>
  <tr>
   <td style="text-align:left;"> Russia </td>
   <td style="text-align:left;"> RUS </td>
   <td style="text-align:right;"> 2017 </td>
   <td style="text-align:right;"> 0.1390252 </td>
   <td style="text-align:left;"> Europe &amp; Central Asia </td>
   <td style="text-align:left;"> Europe </td>
  </tr>
  <tr>
   <td style="text-align:left;"> Saint Kitts and Nevis </td>
   <td style="text-align:left;"> KNA </td>
   <td style="text-align:right;"> 1990 </td>
   <td style="text-align:right;"> 1.9966096 </td>
   <td style="text-align:left;"> Latin America &amp; Caribbean </td>
   <td style="text-align:left;"> Americas </td>
  </tr>
  <tr>
   <td style="text-align:left;"> Guatemala </td>
   <td style="text-align:left;"> GTM </td>
   <td style="text-align:right;"> 2006 </td>
   <td style="text-align:right;"> 9.3331039 </td>
   <td style="text-align:left;"> Latin America &amp; Caribbean </td>
   <td style="text-align:left;"> Americas </td>
  </tr>
  <tr>
   <td style="text-align:left;"> Ecuador </td>
   <td style="text-align:left;"> ECU </td>
   <td style="text-align:right;"> 1991 </td>
   <td style="text-align:right;"> 3.8545001 </td>
   <td style="text-align:left;"> Latin America &amp; Caribbean </td>
   <td style="text-align:left;"> Americas </td>
  </tr>
  <tr>
   <td style="text-align:left;"> Slovakia </td>
   <td style="text-align:left;"> SVK </td>
   <td style="text-align:right;"> 2018 </td>
   <td style="text-align:right;"> 0.0638687 </td>
   <td style="text-align:left;"> Europe &amp; Central Asia </td>
   <td style="text-align:left;"> Europe </td>
  </tr>
  <tr>
   <td style="text-align:left;"> Tunisia </td>
   <td style="text-align:left;"> TUN </td>
   <td style="text-align:right;"> 2014 </td>
   <td style="text-align:right;"> 0.0572086 </td>
   <td style="text-align:left;"> Middle East &amp; North Africa </td>
   <td style="text-align:left;"> Africa </td>
  </tr>
  <tr>
   <td style="text-align:left;"> Japan </td>
   <td style="text-align:left;"> JPN </td>
   <td style="text-align:right;"> 1998 </td>
   <td style="text-align:right;"> 0.0286840 </td>
   <td style="text-align:left;"> East Asia &amp; Pacific </td>
   <td style="text-align:left;"> Asia </td>
  </tr>
  <tr>
   <td style="text-align:left;"> Sao Tome and Principe </td>
   <td style="text-align:left;"> STP </td>
   <td style="text-align:right;"> 2002 </td>
   <td style="text-align:right;"> 14.9816768 </td>
   <td style="text-align:left;"> Sub-Saharan Africa </td>
   <td style="text-align:left;"> Africa </td>
  </tr>
  <tr>
   <td style="text-align:left;"> Seychelles </td>
   <td style="text-align:left;"> SYC </td>
   <td style="text-align:right;"> 2006 </td>
   <td style="text-align:right;"> 0.1407163 </td>
   <td style="text-align:left;"> Sub-Saharan Africa </td>
   <td style="text-align:left;"> Africa </td>
  </tr>
  <tr>
   <td style="text-align:left;"> Netherlands </td>
   <td style="text-align:left;"> NLD </td>
   <td style="text-align:right;"> 2005 </td>
   <td style="text-align:right;"> 0.0094143 </td>
   <td style="text-align:left;"> Europe &amp; Central Asia </td>
   <td style="text-align:left;"> Europe </td>
  </tr>
  <tr>
   <td style="text-align:left;"> Chad </td>
   <td style="text-align:left;"> TCD </td>
   <td style="text-align:right;"> 2015 </td>
   <td style="text-align:right;"> 14.1732720 </td>
   <td style="text-align:left;"> Sub-Saharan Africa </td>
   <td style="text-align:left;"> Africa </td>
  </tr>
  <tr>
   <td style="text-align:left;"> Samoa </td>
   <td style="text-align:left;"> WSM </td>
   <td style="text-align:right;"> 1996 </td>
   <td style="text-align:right;"> 12.8659457 </td>
   <td style="text-align:left;"> East Asia &amp; Pacific </td>
   <td style="text-align:left;"> Oceania </td>
  </tr>
  <tr>
   <td style="text-align:left;"> Gabon </td>
   <td style="text-align:left;"> GAB </td>
   <td style="text-align:right;"> 2012 </td>
   <td style="text-align:right;"> 1.5630691 </td>
   <td style="text-align:left;"> Sub-Saharan Africa </td>
   <td style="text-align:left;"> Africa </td>
  </tr>
  <tr>
   <td style="text-align:left;"> Equatorial Guinea </td>
   <td style="text-align:left;"> GNQ </td>
   <td style="text-align:right;"> 2017 </td>
   <td style="text-align:right;"> 1.8405095 </td>
   <td style="text-align:left;"> Sub-Saharan Africa </td>
   <td style="text-align:left;"> Africa </td>
  </tr>
  <tr>
   <td style="text-align:left;"> Moldova </td>
   <td style="text-align:left;"> MDA </td>
   <td style="text-align:right;"> 2013 </td>
   <td style="text-align:right;"> 0.9861998 </td>
   <td style="text-align:left;"> Europe &amp; Central Asia </td>
   <td style="text-align:left;"> Europe </td>
  </tr>
  <tr>
   <td style="text-align:left;"> Cuba </td>
   <td style="text-align:left;"> CUB </td>
   <td style="text-align:right;"> 2009 </td>
   <td style="text-align:right;"> 0.4077594 </td>
   <td style="text-align:left;"> Latin America &amp; Caribbean </td>
   <td style="text-align:left;"> Americas </td>
  </tr>
  <tr>
   <td style="text-align:left;"> Northern Mariana Islands </td>
   <td style="text-align:left;"> MNP </td>
   <td style="text-align:right;"> 1992 </td>
   <td style="text-align:right;"> 2.3399964 </td>
   <td style="text-align:left;"> East Asia &amp; Pacific </td>
   <td style="text-align:left;"> Oceania </td>
  </tr>
  <tr>
   <td style="text-align:left;"> Antigua and Barbuda </td>
   <td style="text-align:left;"> ATG </td>
   <td style="text-align:right;"> 2012 </td>
   <td style="text-align:right;"> 0.1374306 </td>
   <td style="text-align:left;"> Latin America &amp; Caribbean </td>
   <td style="text-align:left;"> Americas </td>
  </tr>
  <tr>
   <td style="text-align:left;"> South Africa </td>
   <td style="text-align:left;"> ZAF </td>
   <td style="text-align:right;"> 2015 </td>
   <td style="text-align:right;"> 1.3762538 </td>
   <td style="text-align:left;"> Sub-Saharan Africa </td>
   <td style="text-align:left;"> Africa </td>
  </tr>
  <tr>
   <td style="text-align:left;"> Tokelau </td>
   <td style="text-align:left;"> TKL </td>
   <td style="text-align:right;"> 2002 </td>
   <td style="text-align:right;"> 0.0242587 </td>
   <td style="text-align:left;"> East Asia &amp; Pacific </td>
   <td style="text-align:left;"> Oceania </td>
  </tr>
  <tr>
   <td style="text-align:left;"> Uzbekistan </td>
   <td style="text-align:left;"> UZB </td>
   <td style="text-align:right;"> 2013 </td>
   <td style="text-align:right;"> 2.5005690 </td>
   <td style="text-align:left;"> Europe &amp; Central Asia </td>
   <td style="text-align:left;"> Asia </td>
  </tr>
  <tr>
   <td style="text-align:left;"> Grenada </td>
   <td style="text-align:left;"> GRD </td>
   <td style="text-align:right;"> 1996 </td>
   <td style="text-align:right;"> 2.5815078 </td>
   <td style="text-align:left;"> Latin America &amp; Caribbean </td>
   <td style="text-align:left;"> Americas </td>
  </tr>
  <tr>
   <td style="text-align:left;"> Kenya </td>
   <td style="text-align:left;"> KEN </td>
   <td style="text-align:right;"> 1996 </td>
   <td style="text-align:right;"> 8.5528262 </td>
   <td style="text-align:left;"> Sub-Saharan Africa </td>
   <td style="text-align:left;"> Africa </td>
  </tr>
  <tr>
   <td style="text-align:left;"> El Salvador </td>
   <td style="text-align:left;"> SLV </td>
   <td style="text-align:right;"> 2003 </td>
   <td style="text-align:right;"> 5.2831212 </td>
   <td style="text-align:left;"> Latin America &amp; Caribbean </td>
   <td style="text-align:left;"> Americas </td>
  </tr>
  <tr>
   <td style="text-align:left;"> Cameroon </td>
   <td style="text-align:left;"> CMR </td>
   <td style="text-align:right;"> 1991 </td>
   <td style="text-align:right;"> 10.6498631 </td>
   <td style="text-align:left;"> Sub-Saharan Africa </td>
   <td style="text-align:left;"> Africa </td>
  </tr>
  <tr>
   <td style="text-align:left;"> Ireland </td>
   <td style="text-align:left;"> IRL </td>
   <td style="text-align:right;"> 2009 </td>
   <td style="text-align:right;"> 0.0126069 </td>
   <td style="text-align:left;"> Europe &amp; Central Asia </td>
   <td style="text-align:left;"> Europe </td>
  </tr>
  <tr>
   <td style="text-align:left;"> Ukraine </td>
   <td style="text-align:left;"> UKR </td>
   <td style="text-align:right;"> 1993 </td>
   <td style="text-align:right;"> 1.1785335 </td>
   <td style="text-align:left;"> Europe &amp; Central Asia </td>
   <td style="text-align:left;"> Europe </td>
  </tr>
  <tr>
   <td style="text-align:left;"> Trinidad and Tobago </td>
   <td style="text-align:left;"> TTO </td>
   <td style="text-align:right;"> 2000 </td>
   <td style="text-align:right;"> 0.0627119 </td>
   <td style="text-align:left;"> Latin America &amp; Caribbean </td>
   <td style="text-align:left;"> Americas </td>
  </tr>
  <tr>
   <td style="text-align:left;"> Democratic Republic of Congo </td>
   <td style="text-align:left;"> COD </td>
   <td style="text-align:right;"> 2014 </td>
   <td style="text-align:right;"> 13.4677102 </td>
   <td style="text-align:left;"> Sub-Saharan Africa </td>
   <td style="text-align:left;"> Africa </td>
  </tr>
  <tr>
   <td style="text-align:left;"> Peru </td>
   <td style="text-align:left;"> PER </td>
   <td style="text-align:right;"> 1995 </td>
   <td style="text-align:right;"> 5.6786745 </td>
   <td style="text-align:left;"> Latin America &amp; Caribbean </td>
   <td style="text-align:left;"> Americas </td>
  </tr>
  <tr>
   <td style="text-align:left;"> Uganda </td>
   <td style="text-align:left;"> UGA </td>
   <td style="text-align:right;"> 1993 </td>
   <td style="text-align:right;"> 9.6376538 </td>
   <td style="text-align:left;"> Sub-Saharan Africa </td>
   <td style="text-align:left;"> Africa </td>
  </tr>
  <tr>
   <td style="text-align:left;"> Suriname </td>
   <td style="text-align:left;"> SUR </td>
   <td style="text-align:right;"> 1993 </td>
   <td style="text-align:right;"> 5.0592460 </td>
   <td style="text-align:left;"> Latin America &amp; Caribbean </td>
   <td style="text-align:left;"> Americas </td>
  </tr>
  <tr>
   <td style="text-align:left;"> Monaco </td>
   <td style="text-align:left;"> MCO </td>
   <td style="text-align:right;"> 2002 </td>
   <td style="text-align:right;"> 0.0063614 </td>
   <td style="text-align:left;"> Europe &amp; Central Asia </td>
   <td style="text-align:left;"> Europe </td>
  </tr>
  <tr>
   <td style="text-align:left;"> Botswana </td>
   <td style="text-align:left;"> BWA </td>
   <td style="text-align:right;"> 2002 </td>
   <td style="text-align:right;"> 4.5647080 </td>
   <td style="text-align:left;"> Sub-Saharan Africa </td>
   <td style="text-align:left;"> Africa </td>
  </tr>
  <tr>
   <td style="text-align:left;"> Uzbekistan </td>
   <td style="text-align:left;"> UZB </td>
   <td style="text-align:right;"> 2008 </td>
   <td style="text-align:right;"> 3.5587525 </td>
   <td style="text-align:left;"> Europe &amp; Central Asia </td>
   <td style="text-align:left;"> Asia </td>
  </tr>
  <tr>
   <td style="text-align:left;"> Ethiopia </td>
   <td style="text-align:left;"> ETH </td>
   <td style="text-align:right;"> 1999 </td>
   <td style="text-align:right;"> 13.0592962 </td>
   <td style="text-align:left;"> Sub-Saharan Africa </td>
   <td style="text-align:left;"> Africa </td>
  </tr>
  <tr>
   <td style="text-align:left;"> Canada </td>
   <td style="text-align:left;"> CAN </td>
   <td style="text-align:right;"> 2011 </td>
   <td style="text-align:right;"> 0.0046283 </td>
   <td style="text-align:left;"> North America </td>
   <td style="text-align:left;"> Americas </td>
  </tr>
  <tr>
   <td style="text-align:left;"> Belize </td>
   <td style="text-align:left;"> BLZ </td>
   <td style="text-align:right;"> 1990 </td>
   <td style="text-align:right;"> 6.7903008 </td>
   <td style="text-align:left;"> Latin America &amp; Caribbean </td>
   <td style="text-align:left;"> Americas </td>
  </tr>
  <tr>
   <td style="text-align:left;"> Poland </td>
   <td style="text-align:left;"> POL </td>
   <td style="text-align:right;"> 1994 </td>
   <td style="text-align:right;"> 2.6805378 </td>
   <td style="text-align:left;"> Europe &amp; Central Asia </td>
   <td style="text-align:left;"> Europe </td>
  </tr>
  <tr>
   <td style="text-align:left;"> Marshall Islands </td>
   <td style="text-align:left;"> MHL </td>
   <td style="text-align:right;"> 1996 </td>
   <td style="text-align:right;"> 8.9452135 </td>
   <td style="text-align:left;"> East Asia &amp; Pacific </td>
   <td style="text-align:left;"> Oceania </td>
  </tr>
  <tr>
   <td style="text-align:left;"> Mauritius </td>
   <td style="text-align:left;"> MUS </td>
   <td style="text-align:right;"> 1996 </td>
   <td style="text-align:right;"> 1.2861462 </td>
   <td style="text-align:left;"> Sub-Saharan Africa </td>
   <td style="text-align:left;"> Africa </td>
  </tr>
  <tr>
   <td style="text-align:left;"> Croatia </td>
   <td style="text-align:left;"> HRV </td>
   <td style="text-align:right;"> 1992 </td>
   <td style="text-align:right;"> 1.4907045 </td>
   <td style="text-align:left;"> Europe &amp; Central Asia </td>
   <td style="text-align:left;"> Europe </td>
  </tr>
  <tr>
   <td style="text-align:left;"> Uzbekistan </td>
   <td style="text-align:left;"> UZB </td>
   <td style="text-align:right;"> 2019 </td>
   <td style="text-align:right;"> 1.5565890 </td>
   <td style="text-align:left;"> Europe &amp; Central Asia </td>
   <td style="text-align:left;"> Asia </td>
  </tr>
  <tr>
   <td style="text-align:left;"> Bosnia and Herzegovina </td>
   <td style="text-align:left;"> BIH </td>
   <td style="text-align:right;"> 2017 </td>
   <td style="text-align:right;"> 3.1731687 </td>
   <td style="text-align:left;"> Europe &amp; Central Asia </td>
   <td style="text-align:left;"> Europe </td>
  </tr>
  <tr>
   <td style="text-align:left;"> Egypt </td>
   <td style="text-align:left;"> EGY </td>
   <td style="text-align:right;"> 2006 </td>
   <td style="text-align:right;"> 0.1011435 </td>
   <td style="text-align:left;"> Middle East &amp; North Africa </td>
   <td style="text-align:left;"> Africa </td>
  </tr>
  <tr>
   <td style="text-align:left;"> Saint Vincent and the Grenadines </td>
   <td style="text-align:left;"> VCT </td>
   <td style="text-align:right;"> 1998 </td>
   <td style="text-align:right;"> 2.2713615 </td>
   <td style="text-align:left;"> Latin America &amp; Caribbean </td>
   <td style="text-align:left;"> Americas </td>
  </tr>
  <tr>
   <td style="text-align:left;"> Jamaica </td>
   <td style="text-align:left;"> JAM </td>
   <td style="text-align:right;"> 1990 </td>
   <td style="text-align:right;"> 8.0193904 </td>
   <td style="text-align:left;"> Latin America &amp; Caribbean </td>
   <td style="text-align:left;"> Americas </td>
  </tr>
  <tr>
   <td style="text-align:left;"> Serbia </td>
   <td style="text-align:left;"> SRB </td>
   <td style="text-align:right;"> 1994 </td>
   <td style="text-align:right;"> 6.0909041 </td>
   <td style="text-align:left;"> Europe &amp; Central Asia </td>
   <td style="text-align:left;"> Europe </td>
  </tr>
  <tr>
   <td style="text-align:left;"> Jamaica </td>
   <td style="text-align:left;"> JAM </td>
   <td style="text-align:right;"> 1997 </td>
   <td style="text-align:right;"> 3.9009168 </td>
   <td style="text-align:left;"> Latin America &amp; Caribbean </td>
   <td style="text-align:left;"> Americas </td>
  </tr>
  <tr>
   <td style="text-align:left;"> Somalia </td>
   <td style="text-align:left;"> SOM </td>
   <td style="text-align:right;"> 2003 </td>
   <td style="text-align:right;"> 15.0599281 </td>
   <td style="text-align:left;"> Sub-Saharan Africa </td>
   <td style="text-align:left;"> Africa </td>
  </tr>
  <tr>
   <td style="text-align:left;"> China </td>
   <td style="text-align:left;"> CHN </td>
   <td style="text-align:right;"> 2006 </td>
   <td style="text-align:right;"> 8.5611853 </td>
   <td style="text-align:left;"> East Asia &amp; Pacific </td>
   <td style="text-align:left;"> Asia </td>
  </tr>
  <tr>
   <td style="text-align:left;"> Sri Lanka </td>
   <td style="text-align:left;"> LKA </td>
   <td style="text-align:right;"> 1998 </td>
   <td style="text-align:right;"> 8.3239769 </td>
   <td style="text-align:left;"> South Asia </td>
   <td style="text-align:left;"> Asia </td>
  </tr>
  <tr>
   <td style="text-align:left;"> Kazakhstan </td>
   <td style="text-align:left;"> KAZ </td>
   <td style="text-align:right;"> 2013 </td>
   <td style="text-align:right;"> 1.4957238 </td>
   <td style="text-align:left;"> Europe &amp; Central Asia </td>
   <td style="text-align:left;"> Asia </td>
  </tr>
  <tr>
   <td style="text-align:left;"> Tonga </td>
   <td style="text-align:left;"> TON </td>
   <td style="text-align:right;"> 1999 </td>
   <td style="text-align:right;"> 8.4226348 </td>
   <td style="text-align:left;"> East Asia &amp; Pacific </td>
   <td style="text-align:left;"> Oceania </td>
  </tr>
  <tr>
   <td style="text-align:left;"> Morocco </td>
   <td style="text-align:left;"> MAR </td>
   <td style="text-align:right;"> 2014 </td>
   <td style="text-align:right;"> 1.4367081 </td>
   <td style="text-align:left;"> Middle East &amp; North Africa </td>
   <td style="text-align:left;"> Africa </td>
  </tr>
  <tr>
   <td style="text-align:left;"> Norway </td>
   <td style="text-align:left;"> NOR </td>
   <td style="text-align:right;"> 2011 </td>
   <td style="text-align:right;"> 0.0044716 </td>
   <td style="text-align:left;"> Europe &amp; Central Asia </td>
   <td style="text-align:left;"> Europe </td>
  </tr>
  <tr>
   <td style="text-align:left;"> Italy </td>
   <td style="text-align:left;"> ITA </td>
   <td style="text-align:right;"> 1998 </td>
   <td style="text-align:right;"> 0.0742200 </td>
   <td style="text-align:left;"> Europe &amp; Central Asia </td>
   <td style="text-align:left;"> Europe </td>
  </tr>
  <tr>
   <td style="text-align:left;"> Ecuador </td>
   <td style="text-align:left;"> ECU </td>
   <td style="text-align:right;"> 2014 </td>
   <td style="text-align:right;"> 0.7463073 </td>
   <td style="text-align:left;"> Latin America &amp; Caribbean </td>
   <td style="text-align:left;"> Americas </td>
  </tr>
  <tr>
   <td style="text-align:left;"> Sri Lanka </td>
   <td style="text-align:left;"> LKA </td>
   <td style="text-align:right;"> 1992 </td>
   <td style="text-align:right;"> 9.7970156 </td>
   <td style="text-align:left;"> South Asia </td>
   <td style="text-align:left;"> Asia </td>
  </tr>
  <tr>
   <td style="text-align:left;"> Russia </td>
   <td style="text-align:left;"> RUS </td>
   <td style="text-align:right;"> 1992 </td>
   <td style="text-align:right;"> 0.7196468 </td>
   <td style="text-align:left;"> Europe &amp; Central Asia </td>
   <td style="text-align:left;"> Europe </td>
  </tr>
  <tr>
   <td style="text-align:left;"> Tonga </td>
   <td style="text-align:left;"> TON </td>
   <td style="text-align:right;"> 1995 </td>
   <td style="text-align:right;"> 9.4940891 </td>
   <td style="text-align:left;"> East Asia &amp; Pacific </td>
   <td style="text-align:left;"> Oceania </td>
  </tr>
  <tr>
   <td style="text-align:left;"> Saint Vincent and the Grenadines </td>
   <td style="text-align:left;"> VCT </td>
   <td style="text-align:right;"> 2001 </td>
   <td style="text-align:right;"> 1.5877067 </td>
   <td style="text-align:left;"> Latin America &amp; Caribbean </td>
   <td style="text-align:left;"> Americas </td>
  </tr>
  <tr>
   <td style="text-align:left;"> Guinea </td>
   <td style="text-align:left;"> GIN </td>
   <td style="text-align:right;"> 1998 </td>
   <td style="text-align:right;"> 14.2286043 </td>
   <td style="text-align:left;"> Sub-Saharan Africa </td>
   <td style="text-align:left;"> Africa </td>
  </tr>
  <tr>
   <td style="text-align:left;"> Sweden </td>
   <td style="text-align:left;"> SWE </td>
   <td style="text-align:right;"> 1996 </td>
   <td style="text-align:right;"> 0.0183005 </td>
   <td style="text-align:left;"> Europe &amp; Central Asia </td>
   <td style="text-align:left;"> Europe </td>
  </tr>
  <tr>
   <td style="text-align:left;"> Djibouti </td>
   <td style="text-align:left;"> DJI </td>
   <td style="text-align:right;"> 2002 </td>
   <td style="text-align:right;"> 5.6562794 </td>
   <td style="text-align:left;"> Middle East &amp; North Africa </td>
   <td style="text-align:left;"> Africa </td>
  </tr>
  <tr>
   <td style="text-align:left;"> Guam </td>
   <td style="text-align:left;"> GUM </td>
   <td style="text-align:right;"> 2001 </td>
   <td style="text-align:right;"> 0.4362919 </td>
   <td style="text-align:left;"> East Asia &amp; Pacific </td>
   <td style="text-align:left;"> Oceania </td>
  </tr>
  <tr>
   <td style="text-align:left;"> Israel </td>
   <td style="text-align:left;"> ISR </td>
   <td style="text-align:right;"> 1999 </td>
   <td style="text-align:right;"> 0.0264065 </td>
   <td style="text-align:left;"> Middle East &amp; North Africa </td>
   <td style="text-align:left;"> Asia </td>
  </tr>
  <tr>
   <td style="text-align:left;"> Montenegro </td>
   <td style="text-align:left;"> MNE </td>
   <td style="text-align:right;"> 2001 </td>
   <td style="text-align:right;"> 3.8724071 </td>
   <td style="text-align:left;"> Europe &amp; Central Asia </td>
   <td style="text-align:left;"> Europe </td>
  </tr>
  <tr>
   <td style="text-align:left;"> Serbia </td>
   <td style="text-align:left;"> SRB </td>
   <td style="text-align:right;"> 1995 </td>
   <td style="text-align:right;"> 5.9198645 </td>
   <td style="text-align:left;"> Europe &amp; Central Asia </td>
   <td style="text-align:left;"> Europe </td>
  </tr>
  <tr>
   <td style="text-align:left;"> India </td>
   <td style="text-align:left;"> IND </td>
   <td style="text-align:right;"> 1992 </td>
   <td style="text-align:right;"> 13.2429023 </td>
   <td style="text-align:left;"> South Asia </td>
   <td style="text-align:left;"> Asia </td>
  </tr>
  <tr>
   <td style="text-align:left;"> Solomon Islands </td>
   <td style="text-align:left;"> SLB </td>
   <td style="text-align:right;"> 1998 </td>
   <td style="text-align:right;"> 22.9691325 </td>
   <td style="text-align:left;"> East Asia &amp; Pacific </td>
   <td style="text-align:left;"> Oceania </td>
  </tr>
  <tr>
   <td style="text-align:left;"> Congo </td>
   <td style="text-align:left;"> COG </td>
   <td style="text-align:right;"> 1991 </td>
   <td style="text-align:right;"> 10.7580464 </td>
   <td style="text-align:left;"> Sub-Saharan Africa </td>
   <td style="text-align:left;"> Africa </td>
  </tr>
  <tr>
   <td style="text-align:left;"> Mali </td>
   <td style="text-align:left;"> MLI </td>
   <td style="text-align:right;"> 1996 </td>
   <td style="text-align:right;"> 12.2585756 </td>
   <td style="text-align:left;"> Sub-Saharan Africa </td>
   <td style="text-align:left;"> Africa </td>
  </tr>
  <tr>
   <td style="text-align:left;"> Syria </td>
   <td style="text-align:left;"> SYR </td>
   <td style="text-align:right;"> 2018 </td>
   <td style="text-align:right;"> 0.0213544 </td>
   <td style="text-align:left;"> Middle East &amp; North Africa </td>
   <td style="text-align:left;"> Asia </td>
  </tr>
  <tr>
   <td style="text-align:left;"> Ecuador </td>
   <td style="text-align:left;"> ECU </td>
   <td style="text-align:right;"> 1994 </td>
   <td style="text-align:right;"> 3.0111500 </td>
   <td style="text-align:left;"> Latin America &amp; Caribbean </td>
   <td style="text-align:left;"> Americas </td>
  </tr>
  <tr>
   <td style="text-align:left;"> Venezuela </td>
   <td style="text-align:left;"> VEN </td>
   <td style="text-align:right;"> 2006 </td>
   <td style="text-align:right;"> 0.1596374 </td>
   <td style="text-align:left;"> Latin America &amp; Caribbean </td>
   <td style="text-align:left;"> Americas </td>
  </tr>
  <tr>
   <td style="text-align:left;"> San Marino </td>
   <td style="text-align:left;"> SMR </td>
   <td style="text-align:right;"> 2005 </td>
   <td style="text-align:right;"> 0.0103866 </td>
   <td style="text-align:left;"> Europe &amp; Central Asia </td>
   <td style="text-align:left;"> Europe </td>
  </tr>
  <tr>
   <td style="text-align:left;"> Democratic Republic of Congo </td>
   <td style="text-align:left;"> COD </td>
   <td style="text-align:right;"> 2008 </td>
   <td style="text-align:right;"> 13.9673960 </td>
   <td style="text-align:left;"> Sub-Saharan Africa </td>
   <td style="text-align:left;"> Africa </td>
  </tr>
  <tr>
   <td style="text-align:left;"> Belgium </td>
   <td style="text-align:left;"> BEL </td>
   <td style="text-align:right;"> 1998 </td>
   <td style="text-align:right;"> 0.0227294 </td>
   <td style="text-align:left;"> Europe &amp; Central Asia </td>
   <td style="text-align:left;"> Europe </td>
  </tr>
  <tr>
   <td style="text-align:left;"> Guinea </td>
   <td style="text-align:left;"> GIN </td>
   <td style="text-align:right;"> 2004 </td>
   <td style="text-align:right;"> 13.7803404 </td>
   <td style="text-align:left;"> Sub-Saharan Africa </td>
   <td style="text-align:left;"> Africa </td>
  </tr>
  <tr>
   <td style="text-align:left;"> United Arab Emirates </td>
   <td style="text-align:left;"> ARE </td>
   <td style="text-align:right;"> 2014 </td>
   <td style="text-align:right;"> 0.0007332 </td>
   <td style="text-align:left;"> Middle East &amp; North Africa </td>
   <td style="text-align:left;"> Asia </td>
  </tr>
  <tr>
   <td style="text-align:left;"> Ireland </td>
   <td style="text-align:left;"> IRL </td>
   <td style="text-align:right;"> 1995 </td>
   <td style="text-align:right;"> 0.0508563 </td>
   <td style="text-align:left;"> Europe &amp; Central Asia </td>
   <td style="text-align:left;"> Europe </td>
  </tr>
  <tr>
   <td style="text-align:left;"> Czechia </td>
   <td style="text-align:left;"> CZE </td>
   <td style="text-align:right;"> 1990 </td>
   <td style="text-align:right;"> 0.3434426 </td>
   <td style="text-align:left;"> Europe &amp; Central Asia </td>
   <td style="text-align:left;"> Europe </td>
  </tr>
  <tr>
   <td style="text-align:left;"> Uzbekistan </td>
   <td style="text-align:left;"> UZB </td>
   <td style="text-align:right;"> 1997 </td>
   <td style="text-align:right;"> 5.6939443 </td>
   <td style="text-align:left;"> Europe &amp; Central Asia </td>
   <td style="text-align:left;"> Asia </td>
  </tr>
  <tr>
   <td style="text-align:left;"> Turkey </td>
   <td style="text-align:left;"> TUR </td>
   <td style="text-align:right;"> 2003 </td>
   <td style="text-align:right;"> 0.4050764 </td>
   <td style="text-align:left;"> Europe &amp; Central Asia </td>
   <td style="text-align:left;"> Asia </td>
  </tr>
  <tr>
   <td style="text-align:left;"> Benin </td>
   <td style="text-align:left;"> BEN </td>
   <td style="text-align:right;"> 1999 </td>
   <td style="text-align:right;"> 13.8314990 </td>
   <td style="text-align:left;"> Sub-Saharan Africa </td>
   <td style="text-align:left;"> Africa </td>
  </tr>
  <tr>
   <td style="text-align:left;"> Belgium </td>
   <td style="text-align:left;"> BEL </td>
   <td style="text-align:right;"> 2011 </td>
   <td style="text-align:right;"> 0.0079514 </td>
   <td style="text-align:left;"> Europe &amp; Central Asia </td>
   <td style="text-align:left;"> Europe </td>
  </tr>
  <tr>
   <td style="text-align:left;"> Peru </td>
   <td style="text-align:left;"> PER </td>
   <td style="text-align:right;"> 1994 </td>
   <td style="text-align:right;"> 5.9114607 </td>
   <td style="text-align:left;"> Latin America &amp; Caribbean </td>
   <td style="text-align:left;"> Americas </td>
  </tr>
  <tr>
   <td style="text-align:left;"> Chile </td>
   <td style="text-align:left;"> CHL </td>
   <td style="text-align:right;"> 1990 </td>
   <td style="text-align:right;"> 2.7866860 </td>
   <td style="text-align:left;"> Latin America &amp; Caribbean </td>
   <td style="text-align:left;"> Americas </td>
  </tr>
  <tr>
   <td style="text-align:left;"> Malta </td>
   <td style="text-align:left;"> MLT </td>
   <td style="text-align:right;"> 2016 </td>
   <td style="text-align:right;"> 0.0130177 </td>
   <td style="text-align:left;"> Middle East &amp; North Africa </td>
   <td style="text-align:left;"> Europe </td>
  </tr>
  <tr>
   <td style="text-align:left;"> Tajikistan </td>
   <td style="text-align:left;"> TJK </td>
   <td style="text-align:right;"> 2009 </td>
   <td style="text-align:right;"> 9.0937435 </td>
   <td style="text-align:left;"> Europe &amp; Central Asia </td>
   <td style="text-align:left;"> Asia </td>
  </tr>
  <tr>
   <td style="text-align:left;"> Nauru </td>
   <td style="text-align:left;"> NRU </td>
   <td style="text-align:right;"> 2007 </td>
   <td style="text-align:right;"> 1.8127641 </td>
   <td style="text-align:left;"> East Asia &amp; Pacific </td>
   <td style="text-align:left;"> Oceania </td>
  </tr>
  <tr>
   <td style="text-align:left;"> Ireland </td>
   <td style="text-align:left;"> IRL </td>
   <td style="text-align:right;"> 2008 </td>
   <td style="text-align:right;"> 0.0137819 </td>
   <td style="text-align:left;"> Europe &amp; Central Asia </td>
   <td style="text-align:left;"> Europe </td>
  </tr>
  <tr>
   <td style="text-align:left;"> Czechia </td>
   <td style="text-align:left;"> CZE </td>
   <td style="text-align:right;"> 1996 </td>
   <td style="text-align:right;"> 0.2035119 </td>
   <td style="text-align:left;"> Europe &amp; Central Asia </td>
   <td style="text-align:left;"> Europe </td>
  </tr>
  <tr>
   <td style="text-align:left;"> Cuba </td>
   <td style="text-align:left;"> CUB </td>
   <td style="text-align:right;"> 2007 </td>
   <td style="text-align:right;"> 0.4445820 </td>
   <td style="text-align:left;"> Latin America &amp; Caribbean </td>
   <td style="text-align:left;"> Americas </td>
  </tr>
  <tr>
   <td style="text-align:left;"> India </td>
   <td style="text-align:left;"> IND </td>
   <td style="text-align:right;"> 2002 </td>
   <td style="text-align:right;"> 12.0442394 </td>
   <td style="text-align:left;"> South Asia </td>
   <td style="text-align:left;"> Asia </td>
  </tr>
  <tr>
   <td style="text-align:left;"> Congo </td>
   <td style="text-align:left;"> COG </td>
   <td style="text-align:right;"> 2017 </td>
   <td style="text-align:right;"> 5.7949508 </td>
   <td style="text-align:left;"> Sub-Saharan Africa </td>
   <td style="text-align:left;"> Africa </td>
  </tr>
  <tr>
   <td style="text-align:left;"> Switzerland </td>
   <td style="text-align:left;"> CHE </td>
   <td style="text-align:right;"> 1999 </td>
   <td style="text-align:right;"> 0.0060547 </td>
   <td style="text-align:left;"> Europe &amp; Central Asia </td>
   <td style="text-align:left;"> Europe </td>
  </tr>
  <tr>
   <td style="text-align:left;"> Madagascar </td>
   <td style="text-align:left;"> MDG </td>
   <td style="text-align:right;"> 2006 </td>
   <td style="text-align:right;"> 15.9465524 </td>
   <td style="text-align:left;"> Sub-Saharan Africa </td>
   <td style="text-align:left;"> Africa </td>
  </tr>
  <tr>
   <td style="text-align:left;"> Namibia </td>
   <td style="text-align:left;"> NAM </td>
   <td style="text-align:right;"> 2001 </td>
   <td style="text-align:right;"> 6.7380208 </td>
   <td style="text-align:left;"> Sub-Saharan Africa </td>
   <td style="text-align:left;"> Africa </td>
  </tr>
  <tr>
   <td style="text-align:left;"> Austria </td>
   <td style="text-align:left;"> AUT </td>
   <td style="text-align:right;"> 2012 </td>
   <td style="text-align:right;"> 0.0147937 </td>
   <td style="text-align:left;"> Europe &amp; Central Asia </td>
   <td style="text-align:left;"> Europe </td>
  </tr>
  <tr>
   <td style="text-align:left;"> Australia </td>
   <td style="text-align:left;"> AUS </td>
   <td style="text-align:right;"> 1998 </td>
   <td style="text-align:right;"> 0.0544288 </td>
   <td style="text-align:left;"> East Asia &amp; Pacific </td>
   <td style="text-align:left;"> Oceania </td>
  </tr>
  <tr>
   <td style="text-align:left;"> Mauritius </td>
   <td style="text-align:left;"> MUS </td>
   <td style="text-align:right;"> 2009 </td>
   <td style="text-align:right;"> 0.3510428 </td>
   <td style="text-align:left;"> Sub-Saharan Africa </td>
   <td style="text-align:left;"> Africa </td>
  </tr>
  <tr>
   <td style="text-align:left;"> Guinea-Bissau </td>
   <td style="text-align:left;"> GNB </td>
   <td style="text-align:right;"> 1990 </td>
   <td style="text-align:right;"> 14.4419249 </td>
   <td style="text-align:left;"> Sub-Saharan Africa </td>
   <td style="text-align:left;"> Africa </td>
  </tr>
  <tr>
   <td style="text-align:left;"> Northern Mariana Islands </td>
   <td style="text-align:left;"> MNP </td>
   <td style="text-align:right;"> 1996 </td>
   <td style="text-align:right;"> 1.6666338 </td>
   <td style="text-align:left;"> East Asia &amp; Pacific </td>
   <td style="text-align:left;"> Oceania </td>
  </tr>
  <tr>
   <td style="text-align:left;"> Ireland </td>
   <td style="text-align:left;"> IRL </td>
   <td style="text-align:right;"> 1998 </td>
   <td style="text-align:right;"> 0.0389680 </td>
   <td style="text-align:left;"> Europe &amp; Central Asia </td>
   <td style="text-align:left;"> Europe </td>
  </tr>
  <tr>
   <td style="text-align:left;"> Oman </td>
   <td style="text-align:left;"> OMN </td>
   <td style="text-align:right;"> 2007 </td>
   <td style="text-align:right;"> 0.3285890 </td>
   <td style="text-align:left;"> Middle East &amp; North Africa </td>
   <td style="text-align:left;"> Asia </td>
  </tr>
  <tr>
   <td style="text-align:left;"> Ghana </td>
   <td style="text-align:left;"> GHA </td>
   <td style="text-align:right;"> 1993 </td>
   <td style="text-align:right;"> 11.1188243 </td>
   <td style="text-align:left;"> Sub-Saharan Africa </td>
   <td style="text-align:left;"> Africa </td>
  </tr>
  <tr>
   <td style="text-align:left;"> North Macedonia </td>
   <td style="text-align:left;"> MKD </td>
   <td style="text-align:right;"> 2004 </td>
   <td style="text-align:right;"> 3.8730152 </td>
   <td style="text-align:left;"> Europe &amp; Central Asia </td>
   <td style="text-align:left;"> Europe </td>
  </tr>
  <tr>
   <td style="text-align:left;"> Belize </td>
   <td style="text-align:left;"> BLZ </td>
   <td style="text-align:right;"> 2007 </td>
   <td style="text-align:right;"> 2.9204376 </td>
   <td style="text-align:left;"> Latin America &amp; Caribbean </td>
   <td style="text-align:left;"> Americas </td>
  </tr>
  <tr>
   <td style="text-align:left;"> Germany </td>
   <td style="text-align:left;"> DEU </td>
   <td style="text-align:right;"> 1996 </td>
   <td style="text-align:right;"> 0.0095455 </td>
   <td style="text-align:left;"> Europe &amp; Central Asia </td>
   <td style="text-align:left;"> Europe </td>
  </tr>
  <tr>
   <td style="text-align:left;"> Burundi </td>
   <td style="text-align:left;"> BDI </td>
   <td style="text-align:right;"> 2010 </td>
   <td style="text-align:right;"> 13.9319962 </td>
   <td style="text-align:left;"> Sub-Saharan Africa </td>
   <td style="text-align:left;"> Africa </td>
  </tr>
  <tr>
   <td style="text-align:left;"> Cote d'Ivoire </td>
   <td style="text-align:left;"> CIV </td>
   <td style="text-align:right;"> 1993 </td>
   <td style="text-align:right;"> 11.9782038 </td>
   <td style="text-align:left;"> Sub-Saharan Africa </td>
   <td style="text-align:left;"> Africa </td>
  </tr>
  <tr>
   <td style="text-align:left;"> Slovakia </td>
   <td style="text-align:left;"> SVK </td>
   <td style="text-align:right;"> 1998 </td>
   <td style="text-align:right;"> 0.1956476 </td>
   <td style="text-align:left;"> Europe &amp; Central Asia </td>
   <td style="text-align:left;"> Europe </td>
  </tr>
  <tr>
   <td style="text-align:left;"> Uganda </td>
   <td style="text-align:left;"> UGA </td>
   <td style="text-align:right;"> 2004 </td>
   <td style="text-align:right;"> 9.9735659 </td>
   <td style="text-align:left;"> Sub-Saharan Africa </td>
   <td style="text-align:left;"> Africa </td>
  </tr>
  <tr>
   <td style="text-align:left;"> Nepal </td>
   <td style="text-align:left;"> NPL </td>
   <td style="text-align:right;"> 2001 </td>
   <td style="text-align:right;"> 18.6253288 </td>
   <td style="text-align:left;"> South Asia </td>
   <td style="text-align:left;"> Asia </td>
  </tr>
  <tr>
   <td style="text-align:left;"> Brazil </td>
   <td style="text-align:left;"> BRA </td>
   <td style="text-align:right;"> 1999 </td>
   <td style="text-align:right;"> 3.4532619 </td>
   <td style="text-align:left;"> Latin America &amp; Caribbean </td>
   <td style="text-align:left;"> Americas </td>
  </tr>
  <tr>
   <td style="text-align:left;"> Italy </td>
   <td style="text-align:left;"> ITA </td>
   <td style="text-align:right;"> 2017 </td>
   <td style="text-align:right;"> 0.0206421 </td>
   <td style="text-align:left;"> Europe &amp; Central Asia </td>
   <td style="text-align:left;"> Europe </td>
  </tr>
  <tr>
   <td style="text-align:left;"> Cuba </td>
   <td style="text-align:left;"> CUB </td>
   <td style="text-align:right;"> 1995 </td>
   <td style="text-align:right;"> 0.8079552 </td>
   <td style="text-align:left;"> Latin America &amp; Caribbean </td>
   <td style="text-align:left;"> Americas </td>
  </tr>
  <tr>
   <td style="text-align:left;"> American Samoa </td>
   <td style="text-align:left;"> ASM </td>
   <td style="text-align:right;"> 1993 </td>
   <td style="text-align:right;"> 3.9010772 </td>
   <td style="text-align:left;"> East Asia &amp; Pacific </td>
   <td style="text-align:left;"> Oceania </td>
  </tr>
  <tr>
   <td style="text-align:left;"> Honduras </td>
   <td style="text-align:left;"> HND </td>
   <td style="text-align:right;"> 1997 </td>
   <td style="text-align:right;"> 11.3956418 </td>
   <td style="text-align:left;"> Latin America &amp; Caribbean </td>
   <td style="text-align:left;"> Americas </td>
  </tr>
  <tr>
   <td style="text-align:left;"> American Samoa </td>
   <td style="text-align:left;"> ASM </td>
   <td style="text-align:right;"> 1990 </td>
   <td style="text-align:right;"> 5.0087732 </td>
   <td style="text-align:left;"> East Asia &amp; Pacific </td>
   <td style="text-align:left;"> Oceania </td>
  </tr>
  <tr>
   <td style="text-align:left;"> Democratic Republic of Congo </td>
   <td style="text-align:left;"> COD </td>
   <td style="text-align:right;"> 2005 </td>
   <td style="text-align:right;"> 13.8866046 </td>
   <td style="text-align:left;"> Sub-Saharan Africa </td>
   <td style="text-align:left;"> Africa </td>
  </tr>
  <tr>
   <td style="text-align:left;"> Gabon </td>
   <td style="text-align:left;"> GAB </td>
   <td style="text-align:right;"> 2000 </td>
   <td style="text-align:right;"> 4.2435362 </td>
   <td style="text-align:left;"> Sub-Saharan Africa </td>
   <td style="text-align:left;"> Africa </td>
  </tr>
  <tr>
   <td style="text-align:left;"> South Sudan </td>
   <td style="text-align:left;"> SSD </td>
   <td style="text-align:right;"> 2006 </td>
   <td style="text-align:right;"> 11.6691633 </td>
   <td style="text-align:left;"> Sub-Saharan Africa </td>
   <td style="text-align:left;"> Africa </td>
  </tr>
  <tr>
   <td style="text-align:left;"> South Africa </td>
   <td style="text-align:left;"> ZAF </td>
   <td style="text-align:right;"> 2003 </td>
   <td style="text-align:right;"> 2.6828270 </td>
   <td style="text-align:left;"> Sub-Saharan Africa </td>
   <td style="text-align:left;"> Africa </td>
  </tr>
  <tr>
   <td style="text-align:left;"> Mauritania </td>
   <td style="text-align:left;"> MRT </td>
   <td style="text-align:right;"> 2007 </td>
   <td style="text-align:right;"> 9.2800702 </td>
   <td style="text-align:left;"> Sub-Saharan Africa </td>
   <td style="text-align:left;"> Africa </td>
  </tr>
  <tr>
   <td style="text-align:left;"> United States Virgin Islands </td>
   <td style="text-align:left;"> VIR </td>
   <td style="text-align:right;"> 2014 </td>
   <td style="text-align:right;"> 0.0714985 </td>
   <td style="text-align:left;"> Latin America &amp; Caribbean </td>
   <td style="text-align:left;"> Americas </td>
  </tr>
  <tr>
   <td style="text-align:left;"> Bermuda </td>
   <td style="text-align:left;"> BMU </td>
   <td style="text-align:right;"> 1998 </td>
   <td style="text-align:right;"> 0.6743851 </td>
   <td style="text-align:left;"> North America </td>
   <td style="text-align:left;"> Americas </td>
  </tr>
  <tr>
   <td style="text-align:left;"> Belarus </td>
   <td style="text-align:left;"> BLR </td>
   <td style="text-align:right;"> 2012 </td>
   <td style="text-align:right;"> 0.1318992 </td>
   <td style="text-align:left;"> Europe &amp; Central Asia </td>
   <td style="text-align:left;"> Europe </td>
  </tr>
  <tr>
   <td style="text-align:left;"> Sri Lanka </td>
   <td style="text-align:left;"> LKA </td>
   <td style="text-align:right;"> 2008 </td>
   <td style="text-align:right;"> 6.7478651 </td>
   <td style="text-align:left;"> South Asia </td>
   <td style="text-align:left;"> Asia </td>
  </tr>
  <tr>
   <td style="text-align:left;"> Zambia </td>
   <td style="text-align:left;"> ZMB </td>
   <td style="text-align:right;"> 2019 </td>
   <td style="text-align:right;"> 8.2834331 </td>
   <td style="text-align:left;"> Sub-Saharan Africa </td>
   <td style="text-align:left;"> Africa </td>
  </tr>
  <tr>
   <td style="text-align:left;"> Dominica </td>
   <td style="text-align:left;"> DMA </td>
   <td style="text-align:right;"> 2018 </td>
   <td style="text-align:right;"> 0.5828765 </td>
   <td style="text-align:left;"> Latin America &amp; Caribbean </td>
   <td style="text-align:left;"> Americas </td>
  </tr>
  <tr>
   <td style="text-align:left;"> Chile </td>
   <td style="text-align:left;"> CHL </td>
   <td style="text-align:right;"> 1996 </td>
   <td style="text-align:right;"> 1.8040128 </td>
   <td style="text-align:left;"> Latin America &amp; Caribbean </td>
   <td style="text-align:left;"> Americas </td>
  </tr>
  <tr>
   <td style="text-align:left;"> Barbados </td>
   <td style="text-align:left;"> BRB </td>
   <td style="text-align:right;"> 2018 </td>
   <td style="text-align:right;"> 0.0069034 </td>
   <td style="text-align:left;"> Latin America &amp; Caribbean </td>
   <td style="text-align:left;"> Americas </td>
  </tr>
  <tr>
   <td style="text-align:left;"> Syria </td>
   <td style="text-align:left;"> SYR </td>
   <td style="text-align:right;"> 1998 </td>
   <td style="text-align:right;"> 0.5076598 </td>
   <td style="text-align:left;"> Middle East &amp; North Africa </td>
   <td style="text-align:left;"> Asia </td>
  </tr>
  <tr>
   <td style="text-align:left;"> Liberia </td>
   <td style="text-align:left;"> LBR </td>
   <td style="text-align:right;"> 2009 </td>
   <td style="text-align:right;"> 12.0158710 </td>
   <td style="text-align:left;"> Sub-Saharan Africa </td>
   <td style="text-align:left;"> Africa </td>
  </tr>
  <tr>
   <td style="text-align:left;"> Norway </td>
   <td style="text-align:left;"> NOR </td>
   <td style="text-align:right;"> 2018 </td>
   <td style="text-align:right;"> 0.0026485 </td>
   <td style="text-align:left;"> Europe &amp; Central Asia </td>
   <td style="text-align:left;"> Europe </td>
  </tr>
  <tr>
   <td style="text-align:left;"> Sierra Leone </td>
   <td style="text-align:left;"> SLE </td>
   <td style="text-align:right;"> 1996 </td>
   <td style="text-align:right;"> 13.7545387 </td>
   <td style="text-align:left;"> Sub-Saharan Africa </td>
   <td style="text-align:left;"> Africa </td>
  </tr>
  <tr>
   <td style="text-align:left;"> Bermuda </td>
   <td style="text-align:left;"> BMU </td>
   <td style="text-align:right;"> 2005 </td>
   <td style="text-align:right;"> 0.3709298 </td>
   <td style="text-align:left;"> North America </td>
   <td style="text-align:left;"> Americas </td>
  </tr>
  <tr>
   <td style="text-align:left;"> Iran </td>
   <td style="text-align:left;"> IRN </td>
   <td style="text-align:right;"> 1990 </td>
   <td style="text-align:right;"> 1.8996349 </td>
   <td style="text-align:left;"> Middle East &amp; North Africa </td>
   <td style="text-align:left;"> Asia </td>
  </tr>
  <tr>
   <td style="text-align:left;"> Mozambique </td>
   <td style="text-align:left;"> MOZ </td>
   <td style="text-align:right;"> 2002 </td>
   <td style="text-align:right;"> 11.3068093 </td>
   <td style="text-align:left;"> Sub-Saharan Africa </td>
   <td style="text-align:left;"> Africa </td>
  </tr>
  <tr>
   <td style="text-align:left;"> Benin </td>
   <td style="text-align:left;"> BEN </td>
   <td style="text-align:right;"> 2002 </td>
   <td style="text-align:right;"> 13.5195420 </td>
   <td style="text-align:left;"> Sub-Saharan Africa </td>
   <td style="text-align:left;"> Africa </td>
  </tr>
  <tr>
   <td style="text-align:left;"> Nauru </td>
   <td style="text-align:left;"> NRU </td>
   <td style="text-align:right;"> 2016 </td>
   <td style="text-align:right;"> 0.7844455 </td>
   <td style="text-align:left;"> East Asia &amp; Pacific </td>
   <td style="text-align:left;"> Oceania </td>
  </tr>
  <tr>
   <td style="text-align:left;"> Oman </td>
   <td style="text-align:left;"> OMN </td>
   <td style="text-align:right;"> 2003 </td>
   <td style="text-align:right;"> 0.6891378 </td>
   <td style="text-align:left;"> Middle East &amp; North Africa </td>
   <td style="text-align:left;"> Asia </td>
  </tr>
  <tr>
   <td style="text-align:left;"> San Marino </td>
   <td style="text-align:left;"> SMR </td>
   <td style="text-align:right;"> 2008 </td>
   <td style="text-align:right;"> 0.0080771 </td>
   <td style="text-align:left;"> Europe &amp; Central Asia </td>
   <td style="text-align:left;"> Europe </td>
  </tr>
  <tr>
   <td style="text-align:left;"> Canada </td>
   <td style="text-align:left;"> CAN </td>
   <td style="text-align:right;"> 1992 </td>
   <td style="text-align:right;"> 0.0459742 </td>
   <td style="text-align:left;"> North America </td>
   <td style="text-align:left;"> Americas </td>
  </tr>
  <tr>
   <td style="text-align:left;"> Cote d'Ivoire </td>
   <td style="text-align:left;"> CIV </td>
   <td style="text-align:right;"> 2007 </td>
   <td style="text-align:right;"> 10.2253493 </td>
   <td style="text-align:left;"> Sub-Saharan Africa </td>
   <td style="text-align:left;"> Africa </td>
  </tr>
  <tr>
   <td style="text-align:left;"> Netherlands </td>
   <td style="text-align:left;"> NLD </td>
   <td style="text-align:right;"> 2009 </td>
   <td style="text-align:right;"> 0.0064798 </td>
   <td style="text-align:left;"> Europe &amp; Central Asia </td>
   <td style="text-align:left;"> Europe </td>
  </tr>
  <tr>
   <td style="text-align:left;"> Kiribati </td>
   <td style="text-align:left;"> KIR </td>
   <td style="text-align:right;"> 1990 </td>
   <td style="text-align:right;"> 13.9719748 </td>
   <td style="text-align:left;"> East Asia &amp; Pacific </td>
   <td style="text-align:left;"> Oceania </td>
  </tr>
  <tr>
   <td style="text-align:left;"> Cook Islands </td>
   <td style="text-align:left;"> COK </td>
   <td style="text-align:right;"> 2017 </td>
   <td style="text-align:right;"> 0.4774644 </td>
   <td style="text-align:left;"> East Asia &amp; Pacific </td>
   <td style="text-align:left;"> Oceania </td>
  </tr>
  <tr>
   <td style="text-align:left;"> Cyprus </td>
   <td style="text-align:left;"> CYP </td>
   <td style="text-align:right;"> 2015 </td>
   <td style="text-align:right;"> 0.0110377 </td>
   <td style="text-align:left;"> Europe &amp; Central Asia </td>
   <td style="text-align:left;"> Asia </td>
  </tr>
  <tr>
   <td style="text-align:left;"> Botswana </td>
   <td style="text-align:left;"> BWA </td>
   <td style="text-align:right;"> 2000 </td>
   <td style="text-align:right;"> 4.9907577 </td>
   <td style="text-align:left;"> Sub-Saharan Africa </td>
   <td style="text-align:left;"> Africa </td>
  </tr>
  <tr>
   <td style="text-align:left;"> Samoa </td>
   <td style="text-align:left;"> WSM </td>
   <td style="text-align:right;"> 2009 </td>
   <td style="text-align:right;"> 11.9213950 </td>
   <td style="text-align:left;"> East Asia &amp; Pacific </td>
   <td style="text-align:left;"> Oceania </td>
  </tr>
  <tr>
   <td style="text-align:left;"> Denmark </td>
   <td style="text-align:left;"> DNK </td>
   <td style="text-align:right;"> 1996 </td>
   <td style="text-align:right;"> 0.0147314 </td>
   <td style="text-align:left;"> Europe &amp; Central Asia </td>
   <td style="text-align:left;"> Europe </td>
  </tr>
  <tr>
   <td style="text-align:left;"> Singapore </td>
   <td style="text-align:left;"> SGP </td>
   <td style="text-align:right;"> 2001 </td>
   <td style="text-align:right;"> 0.0839700 </td>
   <td style="text-align:left;"> East Asia &amp; Pacific </td>
   <td style="text-align:left;"> Asia </td>
  </tr>
  <tr>
   <td style="text-align:left;"> Australia </td>
   <td style="text-align:left;"> AUS </td>
   <td style="text-align:right;"> 1992 </td>
   <td style="text-align:right;"> 0.0992922 </td>
   <td style="text-align:left;"> East Asia &amp; Pacific </td>
   <td style="text-align:left;"> Oceania </td>
  </tr>
  <tr>
   <td style="text-align:left;"> Finland </td>
   <td style="text-align:left;"> FIN </td>
   <td style="text-align:right;"> 1990 </td>
   <td style="text-align:right;"> 0.0492674 </td>
   <td style="text-align:left;"> Europe &amp; Central Asia </td>
   <td style="text-align:left;"> Europe </td>
  </tr>
  <tr>
   <td style="text-align:left;"> Mali </td>
   <td style="text-align:left;"> MLI </td>
   <td style="text-align:right;"> 1995 </td>
   <td style="text-align:right;"> 12.2778708 </td>
   <td style="text-align:left;"> Sub-Saharan Africa </td>
   <td style="text-align:left;"> Africa </td>
  </tr>
  <tr>
   <td style="text-align:left;"> Syria </td>
   <td style="text-align:left;"> SYR </td>
   <td style="text-align:right;"> 2000 </td>
   <td style="text-align:right;"> 0.3571362 </td>
   <td style="text-align:left;"> Middle East &amp; North Africa </td>
   <td style="text-align:left;"> Asia </td>
  </tr>
  <tr>
   <td style="text-align:left;"> Sudan </td>
   <td style="text-align:left;"> SDN </td>
   <td style="text-align:right;"> 1990 </td>
   <td style="text-align:right;"> 17.3383988 </td>
   <td style="text-align:left;"> Sub-Saharan Africa </td>
   <td style="text-align:left;"> Africa </td>
  </tr>
  <tr>
   <td style="text-align:left;"> Bhutan </td>
   <td style="text-align:left;"> BTN </td>
   <td style="text-align:right;"> 2013 </td>
   <td style="text-align:right;"> 9.9622425 </td>
   <td style="text-align:left;"> South Asia </td>
   <td style="text-align:left;"> Asia </td>
  </tr>
  <tr>
   <td style="text-align:left;"> Nauru </td>
   <td style="text-align:left;"> NRU </td>
   <td style="text-align:right;"> 2000 </td>
   <td style="text-align:right;"> 2.5886187 </td>
   <td style="text-align:left;"> East Asia &amp; Pacific </td>
   <td style="text-align:left;"> Oceania </td>
  </tr>
  <tr>
   <td style="text-align:left;"> Mauritania </td>
   <td style="text-align:left;"> MRT </td>
   <td style="text-align:right;"> 2013 </td>
   <td style="text-align:right;"> 7.9350951 </td>
   <td style="text-align:left;"> Sub-Saharan Africa </td>
   <td style="text-align:left;"> Africa </td>
  </tr>
  <tr>
   <td style="text-align:left;"> Brazil </td>
   <td style="text-align:left;"> BRA </td>
   <td style="text-align:right;"> 2003 </td>
   <td style="text-align:right;"> 2.8176703 </td>
   <td style="text-align:left;"> Latin America &amp; Caribbean </td>
   <td style="text-align:left;"> Americas </td>
  </tr>
  <tr>
   <td style="text-align:left;"> Northern Mariana Islands </td>
   <td style="text-align:left;"> MNP </td>
   <td style="text-align:right;"> 2002 </td>
   <td style="text-align:right;"> 1.3375452 </td>
   <td style="text-align:left;"> East Asia &amp; Pacific </td>
   <td style="text-align:left;"> Oceania </td>
  </tr>
  <tr>
   <td style="text-align:left;"> United Kingdom </td>
   <td style="text-align:left;"> GBR </td>
   <td style="text-align:right;"> 2010 </td>
   <td style="text-align:right;"> 0.0032262 </td>
   <td style="text-align:left;"> Europe &amp; Central Asia </td>
   <td style="text-align:left;"> Europe </td>
  </tr>
  <tr>
   <td style="text-align:left;"> Tajikistan </td>
   <td style="text-align:left;"> TJK </td>
   <td style="text-align:right;"> 2013 </td>
   <td style="text-align:right;"> 7.3593963 </td>
   <td style="text-align:left;"> Europe &amp; Central Asia </td>
   <td style="text-align:left;"> Asia </td>
  </tr>
  <tr>
   <td style="text-align:left;"> Guatemala </td>
   <td style="text-align:left;"> GTM </td>
   <td style="text-align:right;"> 2009 </td>
   <td style="text-align:right;"> 8.4837610 </td>
   <td style="text-align:left;"> Latin America &amp; Caribbean </td>
   <td style="text-align:left;"> Americas </td>
  </tr>
  <tr>
   <td style="text-align:left;"> Djibouti </td>
   <td style="text-align:left;"> DJI </td>
   <td style="text-align:right;"> 2000 </td>
   <td style="text-align:right;"> 6.8644776 </td>
   <td style="text-align:left;"> Middle East &amp; North Africa </td>
   <td style="text-align:left;"> Africa </td>
  </tr>
  <tr>
   <td style="text-align:left;"> Andorra </td>
   <td style="text-align:left;"> AND </td>
   <td style="text-align:right;"> 1992 </td>
   <td style="text-align:right;"> 0.0205019 </td>
   <td style="text-align:left;"> Europe &amp; Central Asia </td>
   <td style="text-align:left;"> Europe </td>
  </tr>
  <tr>
   <td style="text-align:left;"> Turkey </td>
   <td style="text-align:left;"> TUR </td>
   <td style="text-align:right;"> 1992 </td>
   <td style="text-align:right;"> 1.9969325 </td>
   <td style="text-align:left;"> Europe &amp; Central Asia </td>
   <td style="text-align:left;"> Asia </td>
  </tr>
  <tr>
   <td style="text-align:left;"> Botswana </td>
   <td style="text-align:left;"> BWA </td>
   <td style="text-align:right;"> 1997 </td>
   <td style="text-align:right;"> 6.1373951 </td>
   <td style="text-align:left;"> Sub-Saharan Africa </td>
   <td style="text-align:left;"> Africa </td>
  </tr>
  <tr>
   <td style="text-align:left;"> United States Virgin Islands </td>
   <td style="text-align:left;"> VIR </td>
   <td style="text-align:right;"> 2010 </td>
   <td style="text-align:right;"> 0.0894555 </td>
   <td style="text-align:left;"> Latin America &amp; Caribbean </td>
   <td style="text-align:left;"> Americas </td>
  </tr>
  <tr>
   <td style="text-align:left;"> Namibia </td>
   <td style="text-align:left;"> NAM </td>
   <td style="text-align:right;"> 2009 </td>
   <td style="text-align:right;"> 6.3053788 </td>
   <td style="text-align:left;"> Sub-Saharan Africa </td>
   <td style="text-align:left;"> Africa </td>
  </tr>
  <tr>
   <td style="text-align:left;"> Japan </td>
   <td style="text-align:left;"> JPN </td>
   <td style="text-align:right;"> 1994 </td>
   <td style="text-align:right;"> 0.0434370 </td>
   <td style="text-align:left;"> East Asia &amp; Pacific </td>
   <td style="text-align:left;"> Asia </td>
  </tr>
  <tr>
   <td style="text-align:left;"> Brazil </td>
   <td style="text-align:left;"> BRA </td>
   <td style="text-align:right;"> 2008 </td>
   <td style="text-align:right;"> 2.1047274 </td>
   <td style="text-align:left;"> Latin America &amp; Caribbean </td>
   <td style="text-align:left;"> Americas </td>
  </tr>
  <tr>
   <td style="text-align:left;"> Jamaica </td>
   <td style="text-align:left;"> JAM </td>
   <td style="text-align:right;"> 2018 </td>
   <td style="text-align:right;"> 1.1177069 </td>
   <td style="text-align:left;"> Latin America &amp; Caribbean </td>
   <td style="text-align:left;"> Americas </td>
  </tr>
  <tr>
   <td style="text-align:left;"> Eritrea </td>
   <td style="text-align:left;"> ERI </td>
   <td style="text-align:right;"> 2002 </td>
   <td style="text-align:right;"> 10.8572316 </td>
   <td style="text-align:left;"> Sub-Saharan Africa </td>
   <td style="text-align:left;"> Africa </td>
  </tr>
  <tr>
   <td style="text-align:left;"> Madagascar </td>
   <td style="text-align:left;"> MDG </td>
   <td style="text-align:right;"> 2014 </td>
   <td style="text-align:right;"> 15.4199146 </td>
   <td style="text-align:left;"> Sub-Saharan Africa </td>
   <td style="text-align:left;"> Africa </td>
  </tr>
  <tr>
   <td style="text-align:left;"> Netherlands </td>
   <td style="text-align:left;"> NLD </td>
   <td style="text-align:right;"> 2014 </td>
   <td style="text-align:right;"> 0.0045415 </td>
   <td style="text-align:left;"> Europe &amp; Central Asia </td>
   <td style="text-align:left;"> Europe </td>
  </tr>
  <tr>
   <td style="text-align:left;"> Burkina Faso </td>
   <td style="text-align:left;"> BFA </td>
   <td style="text-align:right;"> 2003 </td>
   <td style="text-align:right;"> 12.2371718 </td>
   <td style="text-align:left;"> Sub-Saharan Africa </td>
   <td style="text-align:left;"> Africa </td>
  </tr>
  <tr>
   <td style="text-align:left;"> Kazakhstan </td>
   <td style="text-align:left;"> KAZ </td>
   <td style="text-align:right;"> 2019 </td>
   <td style="text-align:right;"> 1.0410325 </td>
   <td style="text-align:left;"> Europe &amp; Central Asia </td>
   <td style="text-align:left;"> Asia </td>
  </tr>
  <tr>
   <td style="text-align:left;"> Kuwait </td>
   <td style="text-align:left;"> KWT </td>
   <td style="text-align:right;"> 2010 </td>
   <td style="text-align:right;"> 0.0144424 </td>
   <td style="text-align:left;"> Middle East &amp; North Africa </td>
   <td style="text-align:left;"> Asia </td>
  </tr>
  <tr>
   <td style="text-align:left;"> Botswana </td>
   <td style="text-align:left;"> BWA </td>
   <td style="text-align:right;"> 2015 </td>
   <td style="text-align:right;"> 4.0675290 </td>
   <td style="text-align:left;"> Sub-Saharan Africa </td>
   <td style="text-align:left;"> Africa </td>
  </tr>
  <tr>
   <td style="text-align:left;"> Guinea-Bissau </td>
   <td style="text-align:left;"> GNB </td>
   <td style="text-align:right;"> 2004 </td>
   <td style="text-align:right;"> 13.5277897 </td>
   <td style="text-align:left;"> Sub-Saharan Africa </td>
   <td style="text-align:left;"> Africa </td>
  </tr>
  <tr>
   <td style="text-align:left;"> Finland </td>
   <td style="text-align:left;"> FIN </td>
   <td style="text-align:right;"> 2017 </td>
   <td style="text-align:right;"> 0.0039138 </td>
   <td style="text-align:left;"> Europe &amp; Central Asia </td>
   <td style="text-align:left;"> Europe </td>
  </tr>
  <tr>
   <td style="text-align:left;"> Malaysia </td>
   <td style="text-align:left;"> MYS </td>
   <td style="text-align:right;"> 2002 </td>
   <td style="text-align:right;"> 0.2353539 </td>
   <td style="text-align:left;"> East Asia &amp; Pacific </td>
   <td style="text-align:left;"> Asia </td>
  </tr>
  <tr>
   <td style="text-align:left;"> Marshall Islands </td>
   <td style="text-align:left;"> MHL </td>
   <td style="text-align:right;"> 2011 </td>
   <td style="text-align:right;"> 7.2826866 </td>
   <td style="text-align:left;"> East Asia &amp; Pacific </td>
   <td style="text-align:left;"> Oceania </td>
  </tr>
  <tr>
   <td style="text-align:left;"> Tonga </td>
   <td style="text-align:left;"> TON </td>
   <td style="text-align:right;"> 1998 </td>
   <td style="text-align:right;"> 8.6517626 </td>
   <td style="text-align:left;"> East Asia &amp; Pacific </td>
   <td style="text-align:left;"> Oceania </td>
  </tr>
  <tr>
   <td style="text-align:left;"> Sierra Leone </td>
   <td style="text-align:left;"> SLE </td>
   <td style="text-align:right;"> 2013 </td>
   <td style="text-align:right;"> 13.1772563 </td>
   <td style="text-align:left;"> Sub-Saharan Africa </td>
   <td style="text-align:left;"> Africa </td>
  </tr>
  <tr>
   <td style="text-align:left;"> Albania </td>
   <td style="text-align:left;"> ALB </td>
   <td style="text-align:right;"> 2014 </td>
   <td style="text-align:right;"> 3.7595581 </td>
   <td style="text-align:left;"> Europe &amp; Central Asia </td>
   <td style="text-align:left;"> Europe </td>
  </tr>
  <tr>
   <td style="text-align:left;"> Albania </td>
   <td style="text-align:left;"> ALB </td>
   <td style="text-align:right;"> 2019 </td>
   <td style="text-align:right;"> 3.0463064 </td>
   <td style="text-align:left;"> Europe &amp; Central Asia </td>
   <td style="text-align:left;"> Europe </td>
  </tr>
  <tr>
   <td style="text-align:left;"> Sierra Leone </td>
   <td style="text-align:left;"> SLE </td>
   <td style="text-align:right;"> 2001 </td>
   <td style="text-align:right;"> 13.4842975 </td>
   <td style="text-align:left;"> Sub-Saharan Africa </td>
   <td style="text-align:left;"> Africa </td>
  </tr>
  <tr>
   <td style="text-align:left;"> North Korea </td>
   <td style="text-align:left;"> PRK </td>
   <td style="text-align:right;"> 2011 </td>
   <td style="text-align:right;"> 16.2269399 </td>
   <td style="text-align:left;"> East Asia &amp; Pacific </td>
   <td style="text-align:left;"> Asia </td>
  </tr>
  <tr>
   <td style="text-align:left;"> Brunei </td>
   <td style="text-align:left;"> BRN </td>
   <td style="text-align:right;"> 2005 </td>
   <td style="text-align:right;"> 0.1254024 </td>
   <td style="text-align:left;"> East Asia &amp; Pacific </td>
   <td style="text-align:left;"> Asia </td>
  </tr>
  <tr>
   <td style="text-align:left;"> Luxembourg </td>
   <td style="text-align:left;"> LUX </td>
   <td style="text-align:right;"> 1995 </td>
   <td style="text-align:right;"> 0.0246205 </td>
   <td style="text-align:left;"> Europe &amp; Central Asia </td>
   <td style="text-align:left;"> Europe </td>
  </tr>
  <tr>
   <td style="text-align:left;"> Yemen </td>
   <td style="text-align:left;"> YEM </td>
   <td style="text-align:right;"> 2015 </td>
   <td style="text-align:right;"> 7.7362054 </td>
   <td style="text-align:left;"> Middle East &amp; North Africa </td>
   <td style="text-align:left;"> Asia </td>
  </tr>
  <tr>
   <td style="text-align:left;"> San Marino </td>
   <td style="text-align:left;"> SMR </td>
   <td style="text-align:right;"> 2011 </td>
   <td style="text-align:right;"> 0.0068590 </td>
   <td style="text-align:left;"> Europe &amp; Central Asia </td>
   <td style="text-align:left;"> Europe </td>
  </tr>
  <tr>
   <td style="text-align:left;"> Equatorial Guinea </td>
   <td style="text-align:left;"> GNQ </td>
   <td style="text-align:right;"> 1990 </td>
   <td style="text-align:right;"> 13.5882288 </td>
   <td style="text-align:left;"> Sub-Saharan Africa </td>
   <td style="text-align:left;"> Africa </td>
  </tr>
  <tr>
   <td style="text-align:left;"> Panama </td>
   <td style="text-align:left;"> PAN </td>
   <td style="text-align:right;"> 1992 </td>
   <td style="text-align:right;"> 4.4780690 </td>
   <td style="text-align:left;"> Latin America &amp; Caribbean </td>
   <td style="text-align:left;"> Americas </td>
  </tr>
  <tr>
   <td style="text-align:left;"> Andorra </td>
   <td style="text-align:left;"> AND </td>
   <td style="text-align:right;"> 2000 </td>
   <td style="text-align:right;"> 0.0106414 </td>
   <td style="text-align:left;"> Europe &amp; Central Asia </td>
   <td style="text-align:left;"> Europe </td>
  </tr>
  <tr>
   <td style="text-align:left;"> United States </td>
   <td style="text-align:left;"> USA </td>
   <td style="text-align:right;"> 2017 </td>
   <td style="text-align:right;"> 0.0052448 </td>
   <td style="text-align:left;"> North America </td>
   <td style="text-align:left;"> Americas </td>
  </tr>
  <tr>
   <td style="text-align:left;"> South Korea </td>
   <td style="text-align:left;"> KOR </td>
   <td style="text-align:right;"> 2008 </td>
   <td style="text-align:right;"> 0.0115634 </td>
   <td style="text-align:left;"> East Asia &amp; Pacific </td>
   <td style="text-align:left;"> Asia </td>
  </tr>
  <tr>
   <td style="text-align:left;"> Fiji </td>
   <td style="text-align:left;"> FJI </td>
   <td style="text-align:right;"> 2002 </td>
   <td style="text-align:right;"> 7.4271874 </td>
   <td style="text-align:left;"> East Asia &amp; Pacific </td>
   <td style="text-align:left;"> Oceania </td>
  </tr>
  <tr>
   <td style="text-align:left;"> Libya </td>
   <td style="text-align:left;"> LBY </td>
   <td style="text-align:right;"> 2019 </td>
   <td style="text-align:right;"> 0.0249859 </td>
   <td style="text-align:left;"> Middle East &amp; North Africa </td>
   <td style="text-align:left;"> Africa </td>
  </tr>
  <tr>
   <td style="text-align:left;"> Rwanda </td>
   <td style="text-align:left;"> RWA </td>
   <td style="text-align:right;"> 1990 </td>
   <td style="text-align:right;"> 13.1010180 </td>
   <td style="text-align:left;"> Sub-Saharan Africa </td>
   <td style="text-align:left;"> Africa </td>
  </tr>
  <tr>
   <td style="text-align:left;"> Trinidad and Tobago </td>
   <td style="text-align:left;"> TTO </td>
   <td style="text-align:right;"> 2009 </td>
   <td style="text-align:right;"> 0.0213992 </td>
   <td style="text-align:left;"> Latin America &amp; Caribbean </td>
   <td style="text-align:left;"> Americas </td>
  </tr>
  <tr>
   <td style="text-align:left;"> United Arab Emirates </td>
   <td style="text-align:left;"> ARE </td>
   <td style="text-align:right;"> 1997 </td>
   <td style="text-align:right;"> 0.0173407 </td>
   <td style="text-align:left;"> Middle East &amp; North Africa </td>
   <td style="text-align:left;"> Asia </td>
  </tr>
  <tr>
   <td style="text-align:left;"> Northern Mariana Islands </td>
   <td style="text-align:left;"> MNP </td>
   <td style="text-align:right;"> 2014 </td>
   <td style="text-align:right;"> 1.6636452 </td>
   <td style="text-align:left;"> East Asia &amp; Pacific </td>
   <td style="text-align:left;"> Oceania </td>
  </tr>
  <tr>
   <td style="text-align:left;"> Angola </td>
   <td style="text-align:left;"> AGO </td>
   <td style="text-align:right;"> 1990 </td>
   <td style="text-align:right;"> 12.5035325 </td>
   <td style="text-align:left;"> Sub-Saharan Africa </td>
   <td style="text-align:left;"> Africa </td>
  </tr>
  <tr>
   <td style="text-align:left;"> Eswatini </td>
   <td style="text-align:left;"> SWZ </td>
   <td style="text-align:right;"> 2016 </td>
   <td style="text-align:right;"> 5.3827972 </td>
   <td style="text-align:left;"> Sub-Saharan Africa </td>
   <td style="text-align:left;"> Africa </td>
  </tr>
  <tr>
   <td style="text-align:left;"> Sri Lanka </td>
   <td style="text-align:left;"> LKA </td>
   <td style="text-align:right;"> 2016 </td>
   <td style="text-align:right;"> 5.5914185 </td>
   <td style="text-align:left;"> South Asia </td>
   <td style="text-align:left;"> Asia </td>
  </tr>
  <tr>
   <td style="text-align:left;"> Benin </td>
   <td style="text-align:left;"> BEN </td>
   <td style="text-align:right;"> 2010 </td>
   <td style="text-align:right;"> 13.3493004 </td>
   <td style="text-align:left;"> Sub-Saharan Africa </td>
   <td style="text-align:left;"> Africa </td>
  </tr>
  <tr>
   <td style="text-align:left;"> Jamaica </td>
   <td style="text-align:left;"> JAM </td>
   <td style="text-align:right;"> 2019 </td>
   <td style="text-align:right;"> 1.0322508 </td>
   <td style="text-align:left;"> Latin America &amp; Caribbean </td>
   <td style="text-align:left;"> Americas </td>
  </tr>
  <tr>
   <td style="text-align:left;"> Guatemala </td>
   <td style="text-align:left;"> GTM </td>
   <td style="text-align:right;"> 2017 </td>
   <td style="text-align:right;"> 6.6026659 </td>
   <td style="text-align:left;"> Latin America &amp; Caribbean </td>
   <td style="text-align:left;"> Americas </td>
  </tr>
  <tr>
   <td style="text-align:left;"> Palestine </td>
   <td style="text-align:left;"> PSE </td>
   <td style="text-align:right;"> 1998 </td>
   <td style="text-align:right;"> 2.4785974 </td>
   <td style="text-align:left;"> Middle East &amp; North Africa </td>
   <td style="text-align:left;"> Asia </td>
  </tr>
  <tr>
   <td style="text-align:left;"> Vanuatu </td>
   <td style="text-align:left;"> VUT </td>
   <td style="text-align:right;"> 2008 </td>
   <td style="text-align:right;"> 16.7992885 </td>
   <td style="text-align:left;"> East Asia &amp; Pacific </td>
   <td style="text-align:left;"> Oceania </td>
  </tr>
  <tr>
   <td style="text-align:left;"> Poland </td>
   <td style="text-align:left;"> POL </td>
   <td style="text-align:right;"> 2017 </td>
   <td style="text-align:right;"> 0.7442846 </td>
   <td style="text-align:left;"> Europe &amp; Central Asia </td>
   <td style="text-align:left;"> Europe </td>
  </tr>
  <tr>
   <td style="text-align:left;"> Fiji </td>
   <td style="text-align:left;"> FJI </td>
   <td style="text-align:right;"> 1998 </td>
   <td style="text-align:right;"> 8.3778539 </td>
   <td style="text-align:left;"> East Asia &amp; Pacific </td>
   <td style="text-align:left;"> Oceania </td>
  </tr>
  <tr>
   <td style="text-align:left;"> Kenya </td>
   <td style="text-align:left;"> KEN </td>
   <td style="text-align:right;"> 2008 </td>
   <td style="text-align:right;"> 8.8055856 </td>
   <td style="text-align:left;"> Sub-Saharan Africa </td>
   <td style="text-align:left;"> Africa </td>
  </tr>
  <tr>
   <td style="text-align:left;"> Malaysia </td>
   <td style="text-align:left;"> MYS </td>
   <td style="text-align:right;"> 2008 </td>
   <td style="text-align:right;"> 0.1468362 </td>
   <td style="text-align:left;"> East Asia &amp; Pacific </td>
   <td style="text-align:left;"> Asia </td>
  </tr>
  <tr>
   <td style="text-align:left;"> Cuba </td>
   <td style="text-align:left;"> CUB </td>
   <td style="text-align:right;"> 1994 </td>
   <td style="text-align:right;"> 0.8425642 </td>
   <td style="text-align:left;"> Latin America &amp; Caribbean </td>
   <td style="text-align:left;"> Americas </td>
  </tr>
  <tr>
   <td style="text-align:left;"> Cyprus </td>
   <td style="text-align:left;"> CYP </td>
   <td style="text-align:right;"> 2000 </td>
   <td style="text-align:right;"> 0.0476587 </td>
   <td style="text-align:left;"> Europe &amp; Central Asia </td>
   <td style="text-align:left;"> Asia </td>
  </tr>
  <tr>
   <td style="text-align:left;"> Spain </td>
   <td style="text-align:left;"> ESP </td>
   <td style="text-align:right;"> 2004 </td>
   <td style="text-align:right;"> 0.1224925 </td>
   <td style="text-align:left;"> Europe &amp; Central Asia </td>
   <td style="text-align:left;"> Europe </td>
  </tr>
  <tr>
   <td style="text-align:left;"> Serbia </td>
   <td style="text-align:left;"> SRB </td>
   <td style="text-align:right;"> 2010 </td>
   <td style="text-align:right;"> 2.5015739 </td>
   <td style="text-align:left;"> Europe &amp; Central Asia </td>
   <td style="text-align:left;"> Europe </td>
  </tr>
  <tr>
   <td style="text-align:left;"> Papua New Guinea </td>
   <td style="text-align:left;"> PNG </td>
   <td style="text-align:right;"> 2014 </td>
   <td style="text-align:right;"> 18.6973951 </td>
   <td style="text-align:left;"> East Asia &amp; Pacific </td>
   <td style="text-align:left;"> Oceania </td>
  </tr>
  <tr>
   <td style="text-align:left;"> Botswana </td>
   <td style="text-align:left;"> BWA </td>
   <td style="text-align:right;"> 2003 </td>
   <td style="text-align:right;"> 4.4691688 </td>
   <td style="text-align:left;"> Sub-Saharan Africa </td>
   <td style="text-align:left;"> Africa </td>
  </tr>
  <tr>
   <td style="text-align:left;"> Tajikistan </td>
   <td style="text-align:left;"> TJK </td>
   <td style="text-align:right;"> 1997 </td>
   <td style="text-align:right;"> 13.0355552 </td>
   <td style="text-align:left;"> Europe &amp; Central Asia </td>
   <td style="text-align:left;"> Asia </td>
  </tr>
  <tr>
   <td style="text-align:left;"> Cote d'Ivoire </td>
   <td style="text-align:left;"> CIV </td>
   <td style="text-align:right;"> 2010 </td>
   <td style="text-align:right;"> 10.4970759 </td>
   <td style="text-align:left;"> Sub-Saharan Africa </td>
   <td style="text-align:left;"> Africa </td>
  </tr>
  <tr>
   <td style="text-align:left;"> United States </td>
   <td style="text-align:left;"> USA </td>
   <td style="text-align:right;"> 2014 </td>
   <td style="text-align:right;"> 0.0062478 </td>
   <td style="text-align:left;"> North America </td>
   <td style="text-align:left;"> Americas </td>
  </tr>
  <tr>
   <td style="text-align:left;"> Iraq </td>
   <td style="text-align:left;"> IRQ </td>
   <td style="text-align:right;"> 2017 </td>
   <td style="text-align:right;"> 0.0498369 </td>
   <td style="text-align:left;"> Middle East &amp; North Africa </td>
   <td style="text-align:left;"> Asia </td>
  </tr>
  <tr>
   <td style="text-align:left;"> Northern Mariana Islands </td>
   <td style="text-align:left;"> MNP </td>
   <td style="text-align:right;"> 2003 </td>
   <td style="text-align:right;"> 1.2943637 </td>
   <td style="text-align:left;"> East Asia &amp; Pacific </td>
   <td style="text-align:left;"> Oceania </td>
  </tr>
  <tr>
   <td style="text-align:left;"> Georgia </td>
   <td style="text-align:left;"> GEO </td>
   <td style="text-align:right;"> 2012 </td>
   <td style="text-align:right;"> 4.5711953 </td>
   <td style="text-align:left;"> Europe &amp; Central Asia </td>
   <td style="text-align:left;"> Asia </td>
  </tr>
  <tr>
   <td style="text-align:left;"> Guam </td>
   <td style="text-align:left;"> GUM </td>
   <td style="text-align:right;"> 2005 </td>
   <td style="text-align:right;"> 0.3940051 </td>
   <td style="text-align:left;"> East Asia &amp; Pacific </td>
   <td style="text-align:left;"> Oceania </td>
  </tr>
  <tr>
   <td style="text-align:left;"> Saudi Arabia </td>
   <td style="text-align:left;"> SAU </td>
   <td style="text-align:right;"> 2012 </td>
   <td style="text-align:right;"> 0.0967188 </td>
   <td style="text-align:left;"> Middle East &amp; North Africa </td>
   <td style="text-align:left;"> Asia </td>
  </tr>
  <tr>
   <td style="text-align:left;"> Guinea-Bissau </td>
   <td style="text-align:left;"> GNB </td>
   <td style="text-align:right;"> 1992 </td>
   <td style="text-align:right;"> 14.3167230 </td>
   <td style="text-align:left;"> Sub-Saharan Africa </td>
   <td style="text-align:left;"> Africa </td>
  </tr>
  <tr>
   <td style="text-align:left;"> Poland </td>
   <td style="text-align:left;"> POL </td>
   <td style="text-align:right;"> 2003 </td>
   <td style="text-align:right;"> 1.5260644 </td>
   <td style="text-align:left;"> Europe &amp; Central Asia </td>
   <td style="text-align:left;"> Europe </td>
  </tr>
  <tr>
   <td style="text-align:left;"> Benin </td>
   <td style="text-align:left;"> BEN </td>
   <td style="text-align:right;"> 1992 </td>
   <td style="text-align:right;"> 15.1697999 </td>
   <td style="text-align:left;"> Sub-Saharan Africa </td>
   <td style="text-align:left;"> Africa </td>
  </tr>
  <tr>
   <td style="text-align:left;"> Tunisia </td>
   <td style="text-align:left;"> TUN </td>
   <td style="text-align:right;"> 2013 </td>
   <td style="text-align:right;"> 0.0647057 </td>
   <td style="text-align:left;"> Middle East &amp; North Africa </td>
   <td style="text-align:left;"> Africa </td>
  </tr>
  <tr>
   <td style="text-align:left;"> Brunei </td>
   <td style="text-align:left;"> BRN </td>
   <td style="text-align:right;"> 2008 </td>
   <td style="text-align:right;"> 0.0794717 </td>
   <td style="text-align:left;"> East Asia &amp; Pacific </td>
   <td style="text-align:left;"> Asia </td>
  </tr>
  <tr>
   <td style="text-align:left;"> Hungary </td>
   <td style="text-align:left;"> HUN </td>
   <td style="text-align:right;"> 2010 </td>
   <td style="text-align:right;"> 1.3625677 </td>
   <td style="text-align:left;"> Europe &amp; Central Asia </td>
   <td style="text-align:left;"> Europe </td>
  </tr>
  <tr>
   <td style="text-align:left;"> Cote d'Ivoire </td>
   <td style="text-align:left;"> CIV </td>
   <td style="text-align:right;"> 2005 </td>
   <td style="text-align:right;"> 9.9177577 </td>
   <td style="text-align:left;"> Sub-Saharan Africa </td>
   <td style="text-align:left;"> Africa </td>
  </tr>
  <tr>
   <td style="text-align:left;"> Namibia </td>
   <td style="text-align:left;"> NAM </td>
   <td style="text-align:right;"> 2016 </td>
   <td style="text-align:right;"> 5.0355685 </td>
   <td style="text-align:left;"> Sub-Saharan Africa </td>
   <td style="text-align:left;"> Africa </td>
  </tr>
  <tr>
   <td style="text-align:left;"> Uruguay </td>
   <td style="text-align:left;"> URY </td>
   <td style="text-align:right;"> 1996 </td>
   <td style="text-align:right;"> 0.7447248 </td>
   <td style="text-align:left;"> Latin America &amp; Caribbean </td>
   <td style="text-align:left;"> Americas </td>
  </tr>
  <tr>
   <td style="text-align:left;"> Mali </td>
   <td style="text-align:left;"> MLI </td>
   <td style="text-align:right;"> 2016 </td>
   <td style="text-align:right;"> 12.2627504 </td>
   <td style="text-align:left;"> Sub-Saharan Africa </td>
   <td style="text-align:left;"> Africa </td>
  </tr>
  <tr>
   <td style="text-align:left;"> Niue </td>
   <td style="text-align:left;"> NIU </td>
   <td style="text-align:right;"> 1997 </td>
   <td style="text-align:right;"> 3.2346882 </td>
   <td style="text-align:left;"> East Asia &amp; Pacific </td>
   <td style="text-align:left;"> Oceania </td>
  </tr>
  <tr>
   <td style="text-align:left;"> Switzerland </td>
   <td style="text-align:left;"> CHE </td>
   <td style="text-align:right;"> 2012 </td>
   <td style="text-align:right;"> 0.0028005 </td>
   <td style="text-align:left;"> Europe &amp; Central Asia </td>
   <td style="text-align:left;"> Europe </td>
  </tr>
  <tr>
   <td style="text-align:left;"> Tunisia </td>
   <td style="text-align:left;"> TUN </td>
   <td style="text-align:right;"> 2019 </td>
   <td style="text-align:right;"> 0.0302474 </td>
   <td style="text-align:left;"> Middle East &amp; North Africa </td>
   <td style="text-align:left;"> Africa </td>
  </tr>
  <tr>
   <td style="text-align:left;"> Sierra Leone </td>
   <td style="text-align:left;"> SLE </td>
   <td style="text-align:right;"> 2010 </td>
   <td style="text-align:right;"> 13.5336590 </td>
   <td style="text-align:left;"> Sub-Saharan Africa </td>
   <td style="text-align:left;"> Africa </td>
  </tr>
  <tr>
   <td style="text-align:left;"> Philippines </td>
   <td style="text-align:left;"> PHL </td>
   <td style="text-align:right;"> 1993 </td>
   <td style="text-align:right;"> 8.7930198 </td>
   <td style="text-align:left;"> East Asia &amp; Pacific </td>
   <td style="text-align:left;"> Asia </td>
  </tr>
  <tr>
   <td style="text-align:left;"> Yemen </td>
   <td style="text-align:left;"> YEM </td>
   <td style="text-align:right;"> 1997 </td>
   <td style="text-align:right;"> 14.8006987 </td>
   <td style="text-align:left;"> Middle East &amp; North Africa </td>
   <td style="text-align:left;"> Asia </td>
  </tr>
  <tr>
   <td style="text-align:left;"> Paraguay </td>
   <td style="text-align:left;"> PRY </td>
   <td style="text-align:right;"> 2006 </td>
   <td style="text-align:right;"> 7.2374351 </td>
   <td style="text-align:left;"> Latin America &amp; Caribbean </td>
   <td style="text-align:left;"> Americas </td>
  </tr>
  <tr>
   <td style="text-align:left;"> Hungary </td>
   <td style="text-align:left;"> HUN </td>
   <td style="text-align:right;"> 1995 </td>
   <td style="text-align:right;"> 2.3572403 </td>
   <td style="text-align:left;"> Europe &amp; Central Asia </td>
   <td style="text-align:left;"> Europe </td>
  </tr>
  <tr>
   <td style="text-align:left;"> Cook Islands </td>
   <td style="text-align:left;"> COK </td>
   <td style="text-align:right;"> 2011 </td>
   <td style="text-align:right;"> 0.5347384 </td>
   <td style="text-align:left;"> East Asia &amp; Pacific </td>
   <td style="text-align:left;"> Oceania </td>
  </tr>
  <tr>
   <td style="text-align:left;"> Djibouti </td>
   <td style="text-align:left;"> DJI </td>
   <td style="text-align:right;"> 1992 </td>
   <td style="text-align:right;"> 9.3563910 </td>
   <td style="text-align:left;"> Middle East &amp; North Africa </td>
   <td style="text-align:left;"> Africa </td>
  </tr>
  <tr>
   <td style="text-align:left;"> Singapore </td>
   <td style="text-align:left;"> SGP </td>
   <td style="text-align:right;"> 2016 </td>
   <td style="text-align:right;"> 0.0111886 </td>
   <td style="text-align:left;"> East Asia &amp; Pacific </td>
   <td style="text-align:left;"> Asia </td>
  </tr>
  <tr>
   <td style="text-align:left;"> South Korea </td>
   <td style="text-align:left;"> KOR </td>
   <td style="text-align:right;"> 2012 </td>
   <td style="text-align:right;"> 0.0077837 </td>
   <td style="text-align:left;"> East Asia &amp; Pacific </td>
   <td style="text-align:left;"> Asia </td>
  </tr>
  <tr>
   <td style="text-align:left;"> Slovenia </td>
   <td style="text-align:left;"> SVN </td>
   <td style="text-align:right;"> 2005 </td>
   <td style="text-align:right;"> 0.5590575 </td>
   <td style="text-align:left;"> Europe &amp; Central Asia </td>
   <td style="text-align:left;"> Europe </td>
  </tr>
  <tr>
   <td style="text-align:left;"> Moldova </td>
   <td style="text-align:left;"> MDA </td>
   <td style="text-align:right;"> 2017 </td>
   <td style="text-align:right;"> 0.7243144 </td>
   <td style="text-align:left;"> Europe &amp; Central Asia </td>
   <td style="text-align:left;"> Europe </td>
  </tr>
  <tr>
   <td style="text-align:left;"> Lesotho </td>
   <td style="text-align:left;"> LSO </td>
   <td style="text-align:right;"> 1992 </td>
   <td style="text-align:right;"> 11.7222435 </td>
   <td style="text-align:left;"> Sub-Saharan Africa </td>
   <td style="text-align:left;"> Africa </td>
  </tr>
  <tr>
   <td style="text-align:left;"> Gabon </td>
   <td style="text-align:left;"> GAB </td>
   <td style="text-align:right;"> 2010 </td>
   <td style="text-align:right;"> 1.9761133 </td>
   <td style="text-align:left;"> Sub-Saharan Africa </td>
   <td style="text-align:left;"> Africa </td>
  </tr>
  <tr>
   <td style="text-align:left;"> Ethiopia </td>
   <td style="text-align:left;"> ETH </td>
   <td style="text-align:right;"> 2010 </td>
   <td style="text-align:right;"> 13.7306352 </td>
   <td style="text-align:left;"> Sub-Saharan Africa </td>
   <td style="text-align:left;"> Africa </td>
  </tr>
  <tr>
   <td style="text-align:left;"> Sudan </td>
   <td style="text-align:left;"> SDN </td>
   <td style="text-align:right;"> 2017 </td>
   <td style="text-align:right;"> 7.4586169 </td>
   <td style="text-align:left;"> Sub-Saharan Africa </td>
   <td style="text-align:left;"> Africa </td>
  </tr>
  <tr>
   <td style="text-align:left;"> Grenada </td>
   <td style="text-align:left;"> GRD </td>
   <td style="text-align:right;"> 2012 </td>
   <td style="text-align:right;"> 0.4705969 </td>
   <td style="text-align:left;"> Latin America &amp; Caribbean </td>
   <td style="text-align:left;"> Americas </td>
  </tr>
  <tr>
   <td style="text-align:left;"> Cameroon </td>
   <td style="text-align:left;"> CMR </td>
   <td style="text-align:right;"> 2001 </td>
   <td style="text-align:right;"> 8.6795002 </td>
   <td style="text-align:left;"> Sub-Saharan Africa </td>
   <td style="text-align:left;"> Africa </td>
  </tr>
  <tr>
   <td style="text-align:left;"> Colombia </td>
   <td style="text-align:left;"> COL </td>
   <td style="text-align:right;"> 1991 </td>
   <td style="text-align:right;"> 5.3008888 </td>
   <td style="text-align:left;"> Latin America &amp; Caribbean </td>
   <td style="text-align:left;"> Americas </td>
  </tr>
  <tr>
   <td style="text-align:left;"> Antigua and Barbuda </td>
   <td style="text-align:left;"> ATG </td>
   <td style="text-align:right;"> 2004 </td>
   <td style="text-align:right;"> 0.2243155 </td>
   <td style="text-align:left;"> Latin America &amp; Caribbean </td>
   <td style="text-align:left;"> Americas </td>
  </tr>
  <tr>
   <td style="text-align:left;"> Bulgaria </td>
   <td style="text-align:left;"> BGR </td>
   <td style="text-align:right;"> 2012 </td>
   <td style="text-align:right;"> 1.7013950 </td>
   <td style="text-align:left;"> Europe &amp; Central Asia </td>
   <td style="text-align:left;"> Europe </td>
  </tr>
  <tr>
   <td style="text-align:left;"> Cape Verde </td>
   <td style="text-align:left;"> CPV </td>
   <td style="text-align:right;"> 1996 </td>
   <td style="text-align:right;"> 10.7794554 </td>
   <td style="text-align:left;"> Sub-Saharan Africa </td>
   <td style="text-align:left;"> Africa </td>
  </tr>
  <tr>
   <td style="text-align:left;"> Costa Rica </td>
   <td style="text-align:left;"> CRI </td>
   <td style="text-align:right;"> 2014 </td>
   <td style="text-align:right;"> 0.5709923 </td>
   <td style="text-align:left;"> Latin America &amp; Caribbean </td>
   <td style="text-align:left;"> Americas </td>
  </tr>
  <tr>
   <td style="text-align:left;"> Guinea </td>
   <td style="text-align:left;"> GIN </td>
   <td style="text-align:right;"> 2009 </td>
   <td style="text-align:right;"> 13.7167296 </td>
   <td style="text-align:left;"> Sub-Saharan Africa </td>
   <td style="text-align:left;"> Africa </td>
  </tr>
  <tr>
   <td style="text-align:left;"> Palau </td>
   <td style="text-align:left;"> PLW </td>
   <td style="text-align:right;"> 2007 </td>
   <td style="text-align:right;"> 0.0072145 </td>
   <td style="text-align:left;"> East Asia &amp; Pacific </td>
   <td style="text-align:left;"> Oceania </td>
  </tr>
  <tr>
   <td style="text-align:left;"> Zambia </td>
   <td style="text-align:left;"> ZMB </td>
   <td style="text-align:right;"> 2001 </td>
   <td style="text-align:right;"> 7.5972692 </td>
   <td style="text-align:left;"> Sub-Saharan Africa </td>
   <td style="text-align:left;"> Africa </td>
  </tr>
  <tr>
   <td style="text-align:left;"> Jordan </td>
   <td style="text-align:left;"> JOR </td>
   <td style="text-align:right;"> 2019 </td>
   <td style="text-align:right;"> 0.0047295 </td>
   <td style="text-align:left;"> Middle East &amp; North Africa </td>
   <td style="text-align:left;"> Asia </td>
  </tr>
  <tr>
   <td style="text-align:left;"> United States </td>
   <td style="text-align:left;"> USA </td>
   <td style="text-align:right;"> 2013 </td>
   <td style="text-align:right;"> 0.0066159 </td>
   <td style="text-align:left;"> North America </td>
   <td style="text-align:left;"> Americas </td>
  </tr>
  <tr>
   <td style="text-align:left;"> Dominica </td>
   <td style="text-align:left;"> DMA </td>
   <td style="text-align:right;"> 2012 </td>
   <td style="text-align:right;"> 0.7910477 </td>
   <td style="text-align:left;"> Latin America &amp; Caribbean </td>
   <td style="text-align:left;"> Americas </td>
  </tr>
  <tr>
   <td style="text-align:left;"> Seychelles </td>
   <td style="text-align:left;"> SYC </td>
   <td style="text-align:right;"> 1999 </td>
   <td style="text-align:right;"> 0.2922330 </td>
   <td style="text-align:left;"> Sub-Saharan Africa </td>
   <td style="text-align:left;"> Africa </td>
  </tr>
  <tr>
   <td style="text-align:left;"> Bosnia and Herzegovina </td>
   <td style="text-align:left;"> BIH </td>
   <td style="text-align:right;"> 2004 </td>
   <td style="text-align:right;"> 5.7031104 </td>
   <td style="text-align:left;"> Europe &amp; Central Asia </td>
   <td style="text-align:left;"> Europe </td>
  </tr>
  <tr>
   <td style="text-align:left;"> Sao Tome and Principe </td>
   <td style="text-align:left;"> STP </td>
   <td style="text-align:right;"> 2018 </td>
   <td style="text-align:right;"> 9.7554050 </td>
   <td style="text-align:left;"> Sub-Saharan Africa </td>
   <td style="text-align:left;"> Africa </td>
  </tr>
  <tr>
   <td style="text-align:left;"> Slovakia </td>
   <td style="text-align:left;"> SVK </td>
   <td style="text-align:right;"> 2009 </td>
   <td style="text-align:right;"> 0.1025045 </td>
   <td style="text-align:left;"> Europe &amp; Central Asia </td>
   <td style="text-align:left;"> Europe </td>
  </tr>
  <tr>
   <td style="text-align:left;"> Croatia </td>
   <td style="text-align:left;"> HRV </td>
   <td style="text-align:right;"> 1997 </td>
   <td style="text-align:right;"> 1.1867904 </td>
   <td style="text-align:left;"> Europe &amp; Central Asia </td>
   <td style="text-align:left;"> Europe </td>
  </tr>
  <tr>
   <td style="text-align:left;"> Bhutan </td>
   <td style="text-align:left;"> BTN </td>
   <td style="text-align:right;"> 1997 </td>
   <td style="text-align:right;"> 16.9179945 </td>
   <td style="text-align:left;"> South Asia </td>
   <td style="text-align:left;"> Asia </td>
  </tr>
  <tr>
   <td style="text-align:left;"> American Samoa </td>
   <td style="text-align:left;"> ASM </td>
   <td style="text-align:right;"> 1999 </td>
   <td style="text-align:right;"> 3.0234405 </td>
   <td style="text-align:left;"> East Asia &amp; Pacific </td>
   <td style="text-align:left;"> Oceania </td>
  </tr>
  <tr>
   <td style="text-align:left;"> Saint Vincent and the Grenadines </td>
   <td style="text-align:left;"> VCT </td>
   <td style="text-align:right;"> 2002 </td>
   <td style="text-align:right;"> 1.3912171 </td>
   <td style="text-align:left;"> Latin America &amp; Caribbean </td>
   <td style="text-align:left;"> Americas </td>
  </tr>
  <tr>
   <td style="text-align:left;"> Bangladesh </td>
   <td style="text-align:left;"> BGD </td>
   <td style="text-align:right;"> 1997 </td>
   <td style="text-align:right;"> 16.2799248 </td>
   <td style="text-align:left;"> South Asia </td>
   <td style="text-align:left;"> Asia </td>
  </tr>
  <tr>
   <td style="text-align:left;"> Niger </td>
   <td style="text-align:left;"> NER </td>
   <td style="text-align:right;"> 2013 </td>
   <td style="text-align:right;"> 15.3008260 </td>
   <td style="text-align:left;"> Sub-Saharan Africa </td>
   <td style="text-align:left;"> Africa </td>
  </tr>
  <tr>
   <td style="text-align:left;"> San Marino </td>
   <td style="text-align:left;"> SMR </td>
   <td style="text-align:right;"> 2019 </td>
   <td style="text-align:right;"> 0.0053417 </td>
   <td style="text-align:left;"> Europe &amp; Central Asia </td>
   <td style="text-align:left;"> Europe </td>
  </tr>
  <tr>
   <td style="text-align:left;"> North Korea </td>
   <td style="text-align:left;"> PRK </td>
   <td style="text-align:right;"> 2002 </td>
   <td style="text-align:right;"> 13.7928696 </td>
   <td style="text-align:left;"> East Asia &amp; Pacific </td>
   <td style="text-align:left;"> Asia </td>
  </tr>
  <tr>
   <td style="text-align:left;"> Vietnam </td>
   <td style="text-align:left;"> VNM </td>
   <td style="text-align:right;"> 2012 </td>
   <td style="text-align:right;"> 7.4696574 </td>
   <td style="text-align:left;"> East Asia &amp; Pacific </td>
   <td style="text-align:left;"> Asia </td>
  </tr>
  <tr>
   <td style="text-align:left;"> Sierra Leone </td>
   <td style="text-align:left;"> SLE </td>
   <td style="text-align:right;"> 2004 </td>
   <td style="text-align:right;"> 13.5206344 </td>
   <td style="text-align:left;"> Sub-Saharan Africa </td>
   <td style="text-align:left;"> Africa </td>
  </tr>
  <tr>
   <td style="text-align:left;"> Zambia </td>
   <td style="text-align:left;"> ZMB </td>
   <td style="text-align:right;"> 2004 </td>
   <td style="text-align:right;"> 7.9461389 </td>
   <td style="text-align:left;"> Sub-Saharan Africa </td>
   <td style="text-align:left;"> Africa </td>
  </tr>
  <tr>
   <td style="text-align:left;"> Belize </td>
   <td style="text-align:left;"> BLZ </td>
   <td style="text-align:right;"> 2015 </td>
   <td style="text-align:right;"> 1.9918647 </td>
   <td style="text-align:left;"> Latin America &amp; Caribbean </td>
   <td style="text-align:left;"> Americas </td>
  </tr>
  <tr>
   <td style="text-align:left;"> Azerbaijan </td>
   <td style="text-align:left;"> AZE </td>
   <td style="text-align:right;"> 2019 </td>
   <td style="text-align:right;"> 0.8517431 </td>
   <td style="text-align:left;"> Europe &amp; Central Asia </td>
   <td style="text-align:left;"> Asia </td>
  </tr>
  <tr>
   <td style="text-align:left;"> Tunisia </td>
   <td style="text-align:left;"> TUN </td>
   <td style="text-align:right;"> 1990 </td>
   <td style="text-align:right;"> 2.5720265 </td>
   <td style="text-align:left;"> Middle East &amp; North Africa </td>
   <td style="text-align:left;"> Africa </td>
  </tr>
  <tr>
   <td style="text-align:left;"> El Salvador </td>
   <td style="text-align:left;"> SLV </td>
   <td style="text-align:right;"> 2015 </td>
   <td style="text-align:right;"> 2.3433598 </td>
   <td style="text-align:left;"> Latin America &amp; Caribbean </td>
   <td style="text-align:left;"> Americas </td>
  </tr>
  <tr>
   <td style="text-align:left;"> Gambia </td>
   <td style="text-align:left;"> GMB </td>
   <td style="text-align:right;"> 2013 </td>
   <td style="text-align:right;"> 12.0156471 </td>
   <td style="text-align:left;"> Sub-Saharan Africa </td>
   <td style="text-align:left;"> Africa </td>
  </tr>
  <tr>
   <td style="text-align:left;"> Barbados </td>
   <td style="text-align:left;"> BRB </td>
   <td style="text-align:right;"> 1995 </td>
   <td style="text-align:right;"> 0.0254471 </td>
   <td style="text-align:left;"> Latin America &amp; Caribbean </td>
   <td style="text-align:left;"> Americas </td>
  </tr>
  <tr>
   <td style="text-align:left;"> Namibia </td>
   <td style="text-align:left;"> NAM </td>
   <td style="text-align:right;"> 1991 </td>
   <td style="text-align:right;"> 11.5529705 </td>
   <td style="text-align:left;"> Sub-Saharan Africa </td>
   <td style="text-align:left;"> Africa </td>
  </tr>
  <tr>
   <td style="text-align:left;"> Namibia </td>
   <td style="text-align:left;"> NAM </td>
   <td style="text-align:right;"> 2012 </td>
   <td style="text-align:right;"> 5.9346397 </td>
   <td style="text-align:left;"> Sub-Saharan Africa </td>
   <td style="text-align:left;"> Africa </td>
  </tr>
  <tr>
   <td style="text-align:left;"> Equatorial Guinea </td>
   <td style="text-align:left;"> GNQ </td>
   <td style="text-align:right;"> 1998 </td>
   <td style="text-align:right;"> 11.5005657 </td>
   <td style="text-align:left;"> Sub-Saharan Africa </td>
   <td style="text-align:left;"> Africa </td>
  </tr>
  <tr>
   <td style="text-align:left;"> Tokelau </td>
   <td style="text-align:left;"> TKL </td>
   <td style="text-align:right;"> 2004 </td>
   <td style="text-align:right;"> 0.0207262 </td>
   <td style="text-align:left;"> East Asia &amp; Pacific </td>
   <td style="text-align:left;"> Oceania </td>
  </tr>
  <tr>
   <td style="text-align:left;"> Ethiopia </td>
   <td style="text-align:left;"> ETH </td>
   <td style="text-align:right;"> 2000 </td>
   <td style="text-align:right;"> 12.8014256 </td>
   <td style="text-align:left;"> Sub-Saharan Africa </td>
   <td style="text-align:left;"> Africa </td>
  </tr>
  <tr>
   <td style="text-align:left;"> Thailand </td>
   <td style="text-align:left;"> THA </td>
   <td style="text-align:right;"> 2013 </td>
   <td style="text-align:right;"> 2.1627760 </td>
   <td style="text-align:left;"> East Asia &amp; Pacific </td>
   <td style="text-align:left;"> Asia </td>
  </tr>
  <tr>
   <td style="text-align:left;"> Italy </td>
   <td style="text-align:left;"> ITA </td>
   <td style="text-align:right;"> 2015 </td>
   <td style="text-align:right;"> 0.0233611 </td>
   <td style="text-align:left;"> Europe &amp; Central Asia </td>
   <td style="text-align:left;"> Europe </td>
  </tr>
  <tr>
   <td style="text-align:left;"> Togo </td>
   <td style="text-align:left;"> TGO </td>
   <td style="text-align:right;"> 1991 </td>
   <td style="text-align:right;"> 13.7867098 </td>
   <td style="text-align:left;"> Sub-Saharan Africa </td>
   <td style="text-align:left;"> Africa </td>
  </tr>
  <tr>
   <td style="text-align:left;"> India </td>
   <td style="text-align:left;"> IND </td>
   <td style="text-align:right;"> 1997 </td>
   <td style="text-align:right;"> 13.1115332 </td>
   <td style="text-align:left;"> South Asia </td>
   <td style="text-align:left;"> Asia </td>
  </tr>
  <tr>
   <td style="text-align:left;"> Gambia </td>
   <td style="text-align:left;"> GMB </td>
   <td style="text-align:right;"> 2012 </td>
   <td style="text-align:right;"> 12.0493126 </td>
   <td style="text-align:left;"> Sub-Saharan Africa </td>
   <td style="text-align:left;"> Africa </td>
  </tr>
  <tr>
   <td style="text-align:left;"> Honduras </td>
   <td style="text-align:left;"> HND </td>
   <td style="text-align:right;"> 2001 </td>
   <td style="text-align:right;"> 10.3026372 </td>
   <td style="text-align:left;"> Latin America &amp; Caribbean </td>
   <td style="text-align:left;"> Americas </td>
  </tr>
  <tr>
   <td style="text-align:left;"> Kenya </td>
   <td style="text-align:left;"> KEN </td>
   <td style="text-align:right;"> 2011 </td>
   <td style="text-align:right;"> 9.0187468 </td>
   <td style="text-align:left;"> Sub-Saharan Africa </td>
   <td style="text-align:left;"> Africa </td>
  </tr>
  <tr>
   <td style="text-align:left;"> Mexico </td>
   <td style="text-align:left;"> MEX </td>
   <td style="text-align:right;"> 2000 </td>
   <td style="text-align:right;"> 2.5868930 </td>
   <td style="text-align:left;"> Latin America &amp; Caribbean </td>
   <td style="text-align:left;"> Americas </td>
  </tr>
  <tr>
   <td style="text-align:left;"> Latvia </td>
   <td style="text-align:left;"> LVA </td>
   <td style="text-align:right;"> 1993 </td>
   <td style="text-align:right;"> 1.2105553 </td>
   <td style="text-align:left;"> Europe &amp; Central Asia </td>
   <td style="text-align:left;"> Europe </td>
  </tr>
  <tr>
   <td style="text-align:left;"> Croatia </td>
   <td style="text-align:left;"> HRV </td>
   <td style="text-align:right;"> 2004 </td>
   <td style="text-align:right;"> 0.7301670 </td>
   <td style="text-align:left;"> Europe &amp; Central Asia </td>
   <td style="text-align:left;"> Europe </td>
  </tr>
  <tr>
   <td style="text-align:left;"> Italy </td>
   <td style="text-align:left;"> ITA </td>
   <td style="text-align:right;"> 2013 </td>
   <td style="text-align:right;"> 0.0256126 </td>
   <td style="text-align:left;"> Europe &amp; Central Asia </td>
   <td style="text-align:left;"> Europe </td>
  </tr>
  <tr>
   <td style="text-align:left;"> New Zealand </td>
   <td style="text-align:left;"> NZL </td>
   <td style="text-align:right;"> 2007 </td>
   <td style="text-align:right;"> 0.0149759 </td>
   <td style="text-align:left;"> East Asia &amp; Pacific </td>
   <td style="text-align:left;"> Oceania </td>
  </tr>
  <tr>
   <td style="text-align:left;"> Guyana </td>
   <td style="text-align:left;"> GUY </td>
   <td style="text-align:right;"> 2003 </td>
   <td style="text-align:right;"> 2.7886983 </td>
   <td style="text-align:left;"> Latin America &amp; Caribbean </td>
   <td style="text-align:left;"> Americas </td>
  </tr>
  <tr>
   <td style="text-align:left;"> Andorra </td>
   <td style="text-align:left;"> AND </td>
   <td style="text-align:right;"> 2015 </td>
   <td style="text-align:right;"> 0.0045892 </td>
   <td style="text-align:left;"> Europe &amp; Central Asia </td>
   <td style="text-align:left;"> Europe </td>
  </tr>
  <tr>
   <td style="text-align:left;"> Laos </td>
   <td style="text-align:left;"> LAO </td>
   <td style="text-align:right;"> 2019 </td>
   <td style="text-align:right;"> 14.9863711 </td>
   <td style="text-align:left;"> East Asia &amp; Pacific </td>
   <td style="text-align:left;"> Asia </td>
  </tr>
  <tr>
   <td style="text-align:left;"> Nepal </td>
   <td style="text-align:left;"> NPL </td>
   <td style="text-align:right;"> 1999 </td>
   <td style="text-align:right;"> 18.9154135 </td>
   <td style="text-align:left;"> South Asia </td>
   <td style="text-align:left;"> Asia </td>
  </tr>
  <tr>
   <td style="text-align:left;"> Guinea </td>
   <td style="text-align:left;"> GIN </td>
   <td style="text-align:right;"> 1999 </td>
   <td style="text-align:right;"> 14.0514274 </td>
   <td style="text-align:left;"> Sub-Saharan Africa </td>
   <td style="text-align:left;"> Africa </td>
  </tr>
  <tr>
   <td style="text-align:left;"> Croatia </td>
   <td style="text-align:left;"> HRV </td>
   <td style="text-align:right;"> 2007 </td>
   <td style="text-align:right;"> 0.6053568 </td>
   <td style="text-align:left;"> Europe &amp; Central Asia </td>
   <td style="text-align:left;"> Europe </td>
  </tr>
  <tr>
   <td style="text-align:left;"> South Sudan </td>
   <td style="text-align:left;"> SSD </td>
   <td style="text-align:right;"> 1995 </td>
   <td style="text-align:right;"> 12.9509081 </td>
   <td style="text-align:left;"> Sub-Saharan Africa </td>
   <td style="text-align:left;"> Africa </td>
  </tr>
  <tr>
   <td style="text-align:left;"> Bahamas </td>
   <td style="text-align:left;"> BHS </td>
   <td style="text-align:right;"> 2011 </td>
   <td style="text-align:right;"> 0.1181729 </td>
   <td style="text-align:left;"> Latin America &amp; Caribbean </td>
   <td style="text-align:left;"> Americas </td>
  </tr>
  <tr>
   <td style="text-align:left;"> Democratic Republic of Congo </td>
   <td style="text-align:left;"> COD </td>
   <td style="text-align:right;"> 2013 </td>
   <td style="text-align:right;"> 13.6680958 </td>
   <td style="text-align:left;"> Sub-Saharan Africa </td>
   <td style="text-align:left;"> Africa </td>
  </tr>
  <tr>
   <td style="text-align:left;"> Brunei </td>
   <td style="text-align:left;"> BRN </td>
   <td style="text-align:right;"> 1993 </td>
   <td style="text-align:right;"> 0.7343526 </td>
   <td style="text-align:left;"> East Asia &amp; Pacific </td>
   <td style="text-align:left;"> Asia </td>
  </tr>
  <tr>
   <td style="text-align:left;"> Switzerland </td>
   <td style="text-align:left;"> CHE </td>
   <td style="text-align:right;"> 1994 </td>
   <td style="text-align:right;"> 0.0075259 </td>
   <td style="text-align:left;"> Europe &amp; Central Asia </td>
   <td style="text-align:left;"> Europe </td>
  </tr>
  <tr>
   <td style="text-align:left;"> Netherlands </td>
   <td style="text-align:left;"> NLD </td>
   <td style="text-align:right;"> 2007 </td>
   <td style="text-align:right;"> 0.0077642 </td>
   <td style="text-align:left;"> Europe &amp; Central Asia </td>
   <td style="text-align:left;"> Europe </td>
  </tr>
  <tr>
   <td style="text-align:left;"> Mauritius </td>
   <td style="text-align:left;"> MUS </td>
   <td style="text-align:right;"> 2013 </td>
   <td style="text-align:right;"> 0.2320786 </td>
   <td style="text-align:left;"> Sub-Saharan Africa </td>
   <td style="text-align:left;"> Africa </td>
  </tr>
  <tr>
   <td style="text-align:left;"> Cape Verde </td>
   <td style="text-align:left;"> CPV </td>
   <td style="text-align:right;"> 1992 </td>
   <td style="text-align:right;"> 12.0539379 </td>
   <td style="text-align:left;"> Sub-Saharan Africa </td>
   <td style="text-align:left;"> Africa </td>
  </tr>
  <tr>
   <td style="text-align:left;"> New Zealand </td>
   <td style="text-align:left;"> NZL </td>
   <td style="text-align:right;"> 2005 </td>
   <td style="text-align:right;"> 0.0173005 </td>
   <td style="text-align:left;"> East Asia &amp; Pacific </td>
   <td style="text-align:left;"> Oceania </td>
  </tr>
  <tr>
   <td style="text-align:left;"> Seychelles </td>
   <td style="text-align:left;"> SYC </td>
   <td style="text-align:right;"> 2016 </td>
   <td style="text-align:right;"> 0.0607888 </td>
   <td style="text-align:left;"> Sub-Saharan Africa </td>
   <td style="text-align:left;"> Africa </td>
  </tr>
  <tr>
   <td style="text-align:left;"> Puerto Rico </td>
   <td style="text-align:left;"> PRI </td>
   <td style="text-align:right;"> 1990 </td>
   <td style="text-align:right;"> 0.0093775 </td>
   <td style="text-align:left;"> Latin America &amp; Caribbean </td>
   <td style="text-align:left;"> Americas </td>
  </tr>
  <tr>
   <td style="text-align:left;"> Chile </td>
   <td style="text-align:left;"> CHL </td>
   <td style="text-align:right;"> 2016 </td>
   <td style="text-align:right;"> 0.3415360 </td>
   <td style="text-align:left;"> Latin America &amp; Caribbean </td>
   <td style="text-align:left;"> Americas </td>
  </tr>
  <tr>
   <td style="text-align:left;"> Egypt </td>
   <td style="text-align:left;"> EGY </td>
   <td style="text-align:right;"> 2007 </td>
   <td style="text-align:right;"> 0.0841014 </td>
   <td style="text-align:left;"> Middle East &amp; North Africa </td>
   <td style="text-align:left;"> Africa </td>
  </tr>
  <tr>
   <td style="text-align:left;"> Benin </td>
   <td style="text-align:left;"> BEN </td>
   <td style="text-align:right;"> 1996 </td>
   <td style="text-align:right;"> 14.5255622 </td>
   <td style="text-align:left;"> Sub-Saharan Africa </td>
   <td style="text-align:left;"> Africa </td>
  </tr>
  <tr>
   <td style="text-align:left;"> Brunei </td>
   <td style="text-align:left;"> BRN </td>
   <td style="text-align:right;"> 2006 </td>
   <td style="text-align:right;"> 0.1087136 </td>
   <td style="text-align:left;"> East Asia &amp; Pacific </td>
   <td style="text-align:left;"> Asia </td>
  </tr>
  <tr>
   <td style="text-align:left;"> North Macedonia </td>
   <td style="text-align:left;"> MKD </td>
   <td style="text-align:right;"> 2011 </td>
   <td style="text-align:right;"> 2.8849773 </td>
   <td style="text-align:left;"> Europe &amp; Central Asia </td>
   <td style="text-align:left;"> Europe </td>
  </tr>
  <tr>
   <td style="text-align:left;"> Latvia </td>
   <td style="text-align:left;"> LVA </td>
   <td style="text-align:right;"> 2004 </td>
   <td style="text-align:right;"> 0.7893972 </td>
   <td style="text-align:left;"> Europe &amp; Central Asia </td>
   <td style="text-align:left;"> Europe </td>
  </tr>
  <tr>
   <td style="text-align:left;"> Bangladesh </td>
   <td style="text-align:left;"> BGD </td>
   <td style="text-align:right;"> 1994 </td>
   <td style="text-align:right;"> 16.3624720 </td>
   <td style="text-align:left;"> South Asia </td>
   <td style="text-align:left;"> Asia </td>
  </tr>
  <tr>
   <td style="text-align:left;"> Indonesia </td>
   <td style="text-align:left;"> IDN </td>
   <td style="text-align:right;"> 1993 </td>
   <td style="text-align:right;"> 9.4838628 </td>
   <td style="text-align:left;"> East Asia &amp; Pacific </td>
   <td style="text-align:left;"> Asia </td>
  </tr>
  <tr>
   <td style="text-align:left;"> Latvia </td>
   <td style="text-align:left;"> LVA </td>
   <td style="text-align:right;"> 1998 </td>
   <td style="text-align:right;"> 0.9927360 </td>
   <td style="text-align:left;"> Europe &amp; Central Asia </td>
   <td style="text-align:left;"> Europe </td>
  </tr>
  <tr>
   <td style="text-align:left;"> Estonia </td>
   <td style="text-align:left;"> EST </td>
   <td style="text-align:right;"> 1999 </td>
   <td style="text-align:right;"> 1.1707756 </td>
   <td style="text-align:left;"> Europe &amp; Central Asia </td>
   <td style="text-align:left;"> Europe </td>
  </tr>
  <tr>
   <td style="text-align:left;"> Russia </td>
   <td style="text-align:left;"> RUS </td>
   <td style="text-align:right;"> 2007 </td>
   <td style="text-align:right;"> 0.2965896 </td>
   <td style="text-align:left;"> Europe &amp; Central Asia </td>
   <td style="text-align:left;"> Europe </td>
  </tr>
  <tr>
   <td style="text-align:left;"> United Kingdom </td>
   <td style="text-align:left;"> GBR </td>
   <td style="text-align:right;"> 1995 </td>
   <td style="text-align:right;"> 0.0084516 </td>
   <td style="text-align:left;"> Europe &amp; Central Asia </td>
   <td style="text-align:left;"> Europe </td>
  </tr>
  <tr>
   <td style="text-align:left;"> Croatia </td>
   <td style="text-align:left;"> HRV </td>
   <td style="text-align:right;"> 2016 </td>
   <td style="text-align:right;"> 0.3586848 </td>
   <td style="text-align:left;"> Europe &amp; Central Asia </td>
   <td style="text-align:left;"> Europe </td>
  </tr>
  <tr>
   <td style="text-align:left;"> Jordan </td>
   <td style="text-align:left;"> JOR </td>
   <td style="text-align:right;"> 1996 </td>
   <td style="text-align:right;"> 0.1121437 </td>
   <td style="text-align:left;"> Middle East &amp; North Africa </td>
   <td style="text-align:left;"> Asia </td>
  </tr>
  <tr>
   <td style="text-align:left;"> Greenland </td>
   <td style="text-align:left;"> GRL </td>
   <td style="text-align:right;"> 1995 </td>
   <td style="text-align:right;"> 0.5235664 </td>
   <td style="text-align:left;"> Europe &amp; Central Asia </td>
   <td style="text-align:left;"> Americas </td>
  </tr>
  <tr>
   <td style="text-align:left;"> Brazil </td>
   <td style="text-align:left;"> BRA </td>
   <td style="text-align:right;"> 1992 </td>
   <td style="text-align:right;"> 5.3935195 </td>
   <td style="text-align:left;"> Latin America &amp; Caribbean </td>
   <td style="text-align:left;"> Americas </td>
  </tr>
  <tr>
   <td style="text-align:left;"> South Sudan </td>
   <td style="text-align:left;"> SSD </td>
   <td style="text-align:right;"> 2008 </td>
   <td style="text-align:right;"> 11.7375758 </td>
   <td style="text-align:left;"> Sub-Saharan Africa </td>
   <td style="text-align:left;"> Africa </td>
  </tr>
  <tr>
   <td style="text-align:left;"> Vietnam </td>
   <td style="text-align:left;"> VNM </td>
   <td style="text-align:right;"> 2004 </td>
   <td style="text-align:right;"> 10.7854647 </td>
   <td style="text-align:left;"> East Asia &amp; Pacific </td>
   <td style="text-align:left;"> Asia </td>
  </tr>
  <tr>
   <td style="text-align:left;"> Tanzania </td>
   <td style="text-align:left;"> TZA </td>
   <td style="text-align:right;"> 2008 </td>
   <td style="text-align:right;"> 10.8187853 </td>
   <td style="text-align:left;"> Sub-Saharan Africa </td>
   <td style="text-align:left;"> Africa </td>
  </tr>
  <tr>
   <td style="text-align:left;"> South Korea </td>
   <td style="text-align:left;"> KOR </td>
   <td style="text-align:right;"> 2011 </td>
   <td style="text-align:right;"> 0.0084527 </td>
   <td style="text-align:left;"> East Asia &amp; Pacific </td>
   <td style="text-align:left;"> Asia </td>
  </tr>
  <tr>
   <td style="text-align:left;"> Latvia </td>
   <td style="text-align:left;"> LVA </td>
   <td style="text-align:right;"> 2005 </td>
   <td style="text-align:right;"> 0.7451950 </td>
   <td style="text-align:left;"> Europe &amp; Central Asia </td>
   <td style="text-align:left;"> Europe </td>
  </tr>
  <tr>
   <td style="text-align:left;"> Turkmenistan </td>
   <td style="text-align:left;"> TKM </td>
   <td style="text-align:right;"> 2011 </td>
   <td style="text-align:right;"> 0.0426774 </td>
   <td style="text-align:left;"> Europe &amp; Central Asia </td>
   <td style="text-align:left;"> Asia </td>
  </tr>
  <tr>
   <td style="text-align:left;"> Dominica </td>
   <td style="text-align:left;"> DMA </td>
   <td style="text-align:right;"> 2007 </td>
   <td style="text-align:right;"> 1.0499529 </td>
   <td style="text-align:left;"> Latin America &amp; Caribbean </td>
   <td style="text-align:left;"> Americas </td>
  </tr>
  <tr>
   <td style="text-align:left;"> Latvia </td>
   <td style="text-align:left;"> LVA </td>
   <td style="text-align:right;"> 2012 </td>
   <td style="text-align:right;"> 0.4746578 </td>
   <td style="text-align:left;"> Europe &amp; Central Asia </td>
   <td style="text-align:left;"> Europe </td>
  </tr>
  <tr>
   <td style="text-align:left;"> Suriname </td>
   <td style="text-align:left;"> SUR </td>
   <td style="text-align:right;"> 1990 </td>
   <td style="text-align:right;"> 5.5306475 </td>
   <td style="text-align:left;"> Latin America &amp; Caribbean </td>
   <td style="text-align:left;"> Americas </td>
  </tr>
  <tr>
   <td style="text-align:left;"> Belarus </td>
   <td style="text-align:left;"> BLR </td>
   <td style="text-align:right;"> 2000 </td>
   <td style="text-align:right;"> 0.4713192 </td>
   <td style="text-align:left;"> Europe &amp; Central Asia </td>
   <td style="text-align:left;"> Europe </td>
  </tr>
  <tr>
   <td style="text-align:left;"> Barbados </td>
   <td style="text-align:left;"> BRB </td>
   <td style="text-align:right;"> 2006 </td>
   <td style="text-align:right;"> 0.0136079 </td>
   <td style="text-align:left;"> Latin America &amp; Caribbean </td>
   <td style="text-align:left;"> Americas </td>
  </tr>
  <tr>
   <td style="text-align:left;"> Belarus </td>
   <td style="text-align:left;"> BLR </td>
   <td style="text-align:right;"> 1992 </td>
   <td style="text-align:right;"> 0.7170302 </td>
   <td style="text-align:left;"> Europe &amp; Central Asia </td>
   <td style="text-align:left;"> Europe </td>
  </tr>
  <tr>
   <td style="text-align:left;"> Samoa </td>
   <td style="text-align:left;"> WSM </td>
   <td style="text-align:right;"> 2001 </td>
   <td style="text-align:right;"> 12.7592066 </td>
   <td style="text-align:left;"> East Asia &amp; Pacific </td>
   <td style="text-align:left;"> Oceania </td>
  </tr>
  <tr>
   <td style="text-align:left;"> Iran </td>
   <td style="text-align:left;"> IRN </td>
   <td style="text-align:right;"> 1998 </td>
   <td style="text-align:right;"> 0.7799006 </td>
   <td style="text-align:left;"> Middle East &amp; North Africa </td>
   <td style="text-align:left;"> Asia </td>
  </tr>
  <tr>
   <td style="text-align:left;"> Comoros </td>
   <td style="text-align:left;"> COM </td>
   <td style="text-align:right;"> 1990 </td>
   <td style="text-align:right;"> 14.5814713 </td>
   <td style="text-align:left;"> Sub-Saharan Africa </td>
   <td style="text-align:left;"> Africa </td>
  </tr>
  <tr>
   <td style="text-align:left;"> Turkmenistan </td>
   <td style="text-align:left;"> TKM </td>
   <td style="text-align:right;"> 2010 </td>
   <td style="text-align:right;"> 0.0483905 </td>
   <td style="text-align:left;"> Europe &amp; Central Asia </td>
   <td style="text-align:left;"> Asia </td>
  </tr>
  <tr>
   <td style="text-align:left;"> Comoros </td>
   <td style="text-align:left;"> COM </td>
   <td style="text-align:right;"> 2003 </td>
   <td style="text-align:right;"> 12.9840219 </td>
   <td style="text-align:left;"> Sub-Saharan Africa </td>
   <td style="text-align:left;"> Africa </td>
  </tr>
  <tr>
   <td style="text-align:left;"> Central African Republic </td>
   <td style="text-align:left;"> CAF </td>
   <td style="text-align:right;"> 2003 </td>
   <td style="text-align:right;"> 10.9775956 </td>
   <td style="text-align:left;"> Sub-Saharan Africa </td>
   <td style="text-align:left;"> Africa </td>
  </tr>
  <tr>
   <td style="text-align:left;"> Nauru </td>
   <td style="text-align:left;"> NRU </td>
   <td style="text-align:right;"> 2003 </td>
   <td style="text-align:right;"> 2.2650905 </td>
   <td style="text-align:left;"> East Asia &amp; Pacific </td>
   <td style="text-align:left;"> Oceania </td>
  </tr>
  <tr>
   <td style="text-align:left;"> Bermuda </td>
   <td style="text-align:left;"> BMU </td>
   <td style="text-align:right;"> 2017 </td>
   <td style="text-align:right;"> 0.1405873 </td>
   <td style="text-align:left;"> North America </td>
   <td style="text-align:left;"> Americas </td>
  </tr>
  <tr>
   <td style="text-align:left;"> Costa Rica </td>
   <td style="text-align:left;"> CRI </td>
   <td style="text-align:right;"> 1996 </td>
   <td style="text-align:right;"> 2.4865158 </td>
   <td style="text-align:left;"> Latin America &amp; Caribbean </td>
   <td style="text-align:left;"> Americas </td>
  </tr>
  <tr>
   <td style="text-align:left;"> Bangladesh </td>
   <td style="text-align:left;"> BGD </td>
   <td style="text-align:right;"> 2005 </td>
   <td style="text-align:right;"> 16.9978811 </td>
   <td style="text-align:left;"> South Asia </td>
   <td style="text-align:left;"> Asia </td>
  </tr>
  <tr>
   <td style="text-align:left;"> Mauritius </td>
   <td style="text-align:left;"> MUS </td>
   <td style="text-align:right;"> 2006 </td>
   <td style="text-align:right;"> 0.4622050 </td>
   <td style="text-align:left;"> Sub-Saharan Africa </td>
   <td style="text-align:left;"> Africa </td>
  </tr>
  <tr>
   <td style="text-align:left;"> Mauritania </td>
   <td style="text-align:left;"> MRT </td>
   <td style="text-align:right;"> 1990 </td>
   <td style="text-align:right;"> 12.0011284 </td>
   <td style="text-align:left;"> Sub-Saharan Africa </td>
   <td style="text-align:left;"> Africa </td>
  </tr>
  <tr>
   <td style="text-align:left;"> Tokelau </td>
   <td style="text-align:left;"> TKL </td>
   <td style="text-align:right;"> 1998 </td>
   <td style="text-align:right;"> 0.0371379 </td>
   <td style="text-align:left;"> East Asia &amp; Pacific </td>
   <td style="text-align:left;"> Oceania </td>
  </tr>
  <tr>
   <td style="text-align:left;"> Romania </td>
   <td style="text-align:left;"> ROU </td>
   <td style="text-align:right;"> 2011 </td>
   <td style="text-align:right;"> 1.3471102 </td>
   <td style="text-align:left;"> Europe &amp; Central Asia </td>
   <td style="text-align:left;"> Europe </td>
  </tr>
  <tr>
   <td style="text-align:left;"> Belarus </td>
   <td style="text-align:left;"> BLR </td>
   <td style="text-align:right;"> 2001 </td>
   <td style="text-align:right;"> 0.4459390 </td>
   <td style="text-align:left;"> Europe &amp; Central Asia </td>
   <td style="text-align:left;"> Europe </td>
  </tr>
  <tr>
   <td style="text-align:left;"> North Macedonia </td>
   <td style="text-align:left;"> MKD </td>
   <td style="text-align:right;"> 1992 </td>
   <td style="text-align:right;"> 6.1240094 </td>
   <td style="text-align:left;"> Europe &amp; Central Asia </td>
   <td style="text-align:left;"> Europe </td>
  </tr>
  <tr>
   <td style="text-align:left;"> Lebanon </td>
   <td style="text-align:left;"> LBN </td>
   <td style="text-align:right;"> 1998 </td>
   <td style="text-align:right;"> 0.7153674 </td>
   <td style="text-align:left;"> Middle East &amp; North Africa </td>
   <td style="text-align:left;"> Asia </td>
  </tr>
  <tr>
   <td style="text-align:left;"> El Salvador </td>
   <td style="text-align:left;"> SLV </td>
   <td style="text-align:right;"> 2016 </td>
   <td style="text-align:right;"> 2.2087833 </td>
   <td style="text-align:left;"> Latin America &amp; Caribbean </td>
   <td style="text-align:left;"> Americas </td>
  </tr>
  <tr>
   <td style="text-align:left;"> Belgium </td>
   <td style="text-align:left;"> BEL </td>
   <td style="text-align:right;"> 1993 </td>
   <td style="text-align:right;"> 0.0321984 </td>
   <td style="text-align:left;"> Europe &amp; Central Asia </td>
   <td style="text-align:left;"> Europe </td>
  </tr>
  <tr>
   <td style="text-align:left;"> Solomon Islands </td>
   <td style="text-align:left;"> SLB </td>
   <td style="text-align:right;"> 1993 </td>
   <td style="text-align:right;"> 23.2054417 </td>
   <td style="text-align:left;"> East Asia &amp; Pacific </td>
   <td style="text-align:left;"> Oceania </td>
  </tr>
  <tr>
   <td style="text-align:left;"> Luxembourg </td>
   <td style="text-align:left;"> LUX </td>
   <td style="text-align:right;"> 2006 </td>
   <td style="text-align:right;"> 0.0123779 </td>
   <td style="text-align:left;"> Europe &amp; Central Asia </td>
   <td style="text-align:left;"> Europe </td>
  </tr>
  <tr>
   <td style="text-align:left;"> Uganda </td>
   <td style="text-align:left;"> UGA </td>
   <td style="text-align:right;"> 2010 </td>
   <td style="text-align:right;"> 10.5000034 </td>
   <td style="text-align:left;"> Sub-Saharan Africa </td>
   <td style="text-align:left;"> Africa </td>
  </tr>
  <tr>
   <td style="text-align:left;"> Panama </td>
   <td style="text-align:left;"> PAN </td>
   <td style="text-align:right;"> 1990 </td>
   <td style="text-align:right;"> 4.7903589 </td>
   <td style="text-align:left;"> Latin America &amp; Caribbean </td>
   <td style="text-align:left;"> Americas </td>
  </tr>
  <tr>
   <td style="text-align:left;"> Vanuatu </td>
   <td style="text-align:left;"> VUT </td>
   <td style="text-align:right;"> 2015 </td>
   <td style="text-align:right;"> 16.8414685 </td>
   <td style="text-align:left;"> East Asia &amp; Pacific </td>
   <td style="text-align:left;"> Oceania </td>
  </tr>
  <tr>
   <td style="text-align:left;"> Colombia </td>
   <td style="text-align:left;"> COL </td>
   <td style="text-align:right;"> 2003 </td>
   <td style="text-align:right;"> 3.0849318 </td>
   <td style="text-align:left;"> Latin America &amp; Caribbean </td>
   <td style="text-align:left;"> Americas </td>
  </tr>
  <tr>
   <td style="text-align:left;"> Samoa </td>
   <td style="text-align:left;"> WSM </td>
   <td style="text-align:right;"> 2011 </td>
   <td style="text-align:right;"> 12.8261521 </td>
   <td style="text-align:left;"> East Asia &amp; Pacific </td>
   <td style="text-align:left;"> Oceania </td>
  </tr>
  <tr>
   <td style="text-align:left;"> El Salvador </td>
   <td style="text-align:left;"> SLV </td>
   <td style="text-align:right;"> 2007 </td>
   <td style="text-align:right;"> 4.5066621 </td>
   <td style="text-align:left;"> Latin America &amp; Caribbean </td>
   <td style="text-align:left;"> Americas </td>
  </tr>
  <tr>
   <td style="text-align:left;"> Sierra Leone </td>
   <td style="text-align:left;"> SLE </td>
   <td style="text-align:right;"> 2005 </td>
   <td style="text-align:right;"> 13.5943415 </td>
   <td style="text-align:left;"> Sub-Saharan Africa </td>
   <td style="text-align:left;"> Africa </td>
  </tr>
  <tr>
   <td style="text-align:left;"> Slovenia </td>
   <td style="text-align:left;"> SVN </td>
   <td style="text-align:right;"> 2017 </td>
   <td style="text-align:right;"> 0.3200488 </td>
   <td style="text-align:left;"> Europe &amp; Central Asia </td>
   <td style="text-align:left;"> Europe </td>
  </tr>
  <tr>
   <td style="text-align:left;"> Guatemala </td>
   <td style="text-align:left;"> GTM </td>
   <td style="text-align:right;"> 2018 </td>
   <td style="text-align:right;"> 6.3227482 </td>
   <td style="text-align:left;"> Latin America &amp; Caribbean </td>
   <td style="text-align:left;"> Americas </td>
  </tr>
  <tr>
   <td style="text-align:left;"> Uganda </td>
   <td style="text-align:left;"> UGA </td>
   <td style="text-align:right;"> 2015 </td>
   <td style="text-align:right;"> 11.0921403 </td>
   <td style="text-align:left;"> Sub-Saharan Africa </td>
   <td style="text-align:left;"> Africa </td>
  </tr>
  <tr>
   <td style="text-align:left;"> Rwanda </td>
   <td style="text-align:left;"> RWA </td>
   <td style="text-align:right;"> 2004 </td>
   <td style="text-align:right;"> 11.9874199 </td>
   <td style="text-align:left;"> Sub-Saharan Africa </td>
   <td style="text-align:left;"> Africa </td>
  </tr>
  <tr>
   <td style="text-align:left;"> Eritrea </td>
   <td style="text-align:left;"> ERI </td>
   <td style="text-align:right;"> 2014 </td>
   <td style="text-align:right;"> 11.0750372 </td>
   <td style="text-align:left;"> Sub-Saharan Africa </td>
   <td style="text-align:left;"> Africa </td>
  </tr>
  <tr>
   <td style="text-align:left;"> Belarus </td>
   <td style="text-align:left;"> BLR </td>
   <td style="text-align:right;"> 2014 </td>
   <td style="text-align:right;"> 0.1092656 </td>
   <td style="text-align:left;"> Europe &amp; Central Asia </td>
   <td style="text-align:left;"> Europe </td>
  </tr>
  <tr>
   <td style="text-align:left;"> Kiribati </td>
   <td style="text-align:left;"> KIR </td>
   <td style="text-align:right;"> 2004 </td>
   <td style="text-align:right;"> 13.2642037 </td>
   <td style="text-align:left;"> East Asia &amp; Pacific </td>
   <td style="text-align:left;"> Oceania </td>
  </tr>
  <tr>
   <td style="text-align:left;"> Georgia </td>
   <td style="text-align:left;"> GEO </td>
   <td style="text-align:right;"> 1991 </td>
   <td style="text-align:right;"> 7.7094074 </td>
   <td style="text-align:left;"> Europe &amp; Central Asia </td>
   <td style="text-align:left;"> Asia </td>
  </tr>
  <tr>
   <td style="text-align:left;"> Comoros </td>
   <td style="text-align:left;"> COM </td>
   <td style="text-align:right;"> 2007 </td>
   <td style="text-align:right;"> 12.4511768 </td>
   <td style="text-align:left;"> Sub-Saharan Africa </td>
   <td style="text-align:left;"> Africa </td>
  </tr>
  <tr>
   <td style="text-align:left;"> Brunei </td>
   <td style="text-align:left;"> BRN </td>
   <td style="text-align:right;"> 2001 </td>
   <td style="text-align:right;"> 0.2332388 </td>
   <td style="text-align:left;"> East Asia &amp; Pacific </td>
   <td style="text-align:left;"> Asia </td>
  </tr>
  <tr>
   <td style="text-align:left;"> Ethiopia </td>
   <td style="text-align:left;"> ETH </td>
   <td style="text-align:right;"> 2004 </td>
   <td style="text-align:right;"> 13.2997674 </td>
   <td style="text-align:left;"> Sub-Saharan Africa </td>
   <td style="text-align:left;"> Africa </td>
  </tr>
  <tr>
   <td style="text-align:left;"> Rwanda </td>
   <td style="text-align:left;"> RWA </td>
   <td style="text-align:right;"> 2014 </td>
   <td style="text-align:right;"> 12.4223385 </td>
   <td style="text-align:left;"> Sub-Saharan Africa </td>
   <td style="text-align:left;"> Africa </td>
  </tr>
  <tr>
   <td style="text-align:left;"> Bhutan </td>
   <td style="text-align:left;"> BTN </td>
   <td style="text-align:right;"> 2017 </td>
   <td style="text-align:right;"> 9.0538443 </td>
   <td style="text-align:left;"> South Asia </td>
   <td style="text-align:left;"> Asia </td>
  </tr>
  <tr>
   <td style="text-align:left;"> Saint Lucia </td>
   <td style="text-align:left;"> LCA </td>
   <td style="text-align:right;"> 2011 </td>
   <td style="text-align:right;"> 0.5739066 </td>
   <td style="text-align:left;"> Latin America &amp; Caribbean </td>
   <td style="text-align:left;"> Americas </td>
  </tr>
  <tr>
   <td style="text-align:left;"> Cook Islands </td>
   <td style="text-align:left;"> COK </td>
   <td style="text-align:right;"> 1992 </td>
   <td style="text-align:right;"> 1.7473516 </td>
   <td style="text-align:left;"> East Asia &amp; Pacific </td>
   <td style="text-align:left;"> Oceania </td>
  </tr>
  <tr>
   <td style="text-align:left;"> Mali </td>
   <td style="text-align:left;"> MLI </td>
   <td style="text-align:right;"> 2012 </td>
   <td style="text-align:right;"> 12.2027954 </td>
   <td style="text-align:left;"> Sub-Saharan Africa </td>
   <td style="text-align:left;"> Africa </td>
  </tr>
  <tr>
   <td style="text-align:left;"> Albania </td>
   <td style="text-align:left;"> ALB </td>
   <td style="text-align:right;"> 2011 </td>
   <td style="text-align:right;"> 4.3409999 </td>
   <td style="text-align:left;"> Europe &amp; Central Asia </td>
   <td style="text-align:left;"> Europe </td>
  </tr>
  <tr>
   <td style="text-align:left;"> Finland </td>
   <td style="text-align:left;"> FIN </td>
   <td style="text-align:right;"> 2003 </td>
   <td style="text-align:right;"> 0.0154542 </td>
   <td style="text-align:left;"> Europe &amp; Central Asia </td>
   <td style="text-align:left;"> Europe </td>
  </tr>
  <tr>
   <td style="text-align:left;"> Bahamas </td>
   <td style="text-align:left;"> BHS </td>
   <td style="text-align:right;"> 2015 </td>
   <td style="text-align:right;"> 0.0962533 </td>
   <td style="text-align:left;"> Latin America &amp; Caribbean </td>
   <td style="text-align:left;"> Americas </td>
  </tr>
  <tr>
   <td style="text-align:left;"> Zimbabwe </td>
   <td style="text-align:left;"> ZWE </td>
   <td style="text-align:right;"> 2016 </td>
   <td style="text-align:right;"> 8.6568174 </td>
   <td style="text-align:left;"> Sub-Saharan Africa </td>
   <td style="text-align:left;"> Africa </td>
  </tr>
  <tr>
   <td style="text-align:left;"> Australia </td>
   <td style="text-align:left;"> AUS </td>
   <td style="text-align:right;"> 2008 </td>
   <td style="text-align:right;"> 0.0167765 </td>
   <td style="text-align:left;"> East Asia &amp; Pacific </td>
   <td style="text-align:left;"> Oceania </td>
  </tr>
  <tr>
   <td style="text-align:left;"> Spain </td>
   <td style="text-align:left;"> ESP </td>
   <td style="text-align:right;"> 1994 </td>
   <td style="text-align:right;"> 0.2962129 </td>
   <td style="text-align:left;"> Europe &amp; Central Asia </td>
   <td style="text-align:left;"> Europe </td>
  </tr>
  <tr>
   <td style="text-align:left;"> Chad </td>
   <td style="text-align:left;"> TCD </td>
   <td style="text-align:right;"> 1997 </td>
   <td style="text-align:right;"> 14.5322433 </td>
   <td style="text-align:left;"> Sub-Saharan Africa </td>
   <td style="text-align:left;"> Africa </td>
  </tr>
  <tr>
   <td style="text-align:left;"> Marshall Islands </td>
   <td style="text-align:left;"> MHL </td>
   <td style="text-align:right;"> 2003 </td>
   <td style="text-align:right;"> 7.2681305 </td>
   <td style="text-align:left;"> East Asia &amp; Pacific </td>
   <td style="text-align:left;"> Oceania </td>
  </tr>
  <tr>
   <td style="text-align:left;"> Brazil </td>
   <td style="text-align:left;"> BRA </td>
   <td style="text-align:right;"> 2014 </td>
   <td style="text-align:right;"> 1.3876265 </td>
   <td style="text-align:left;"> Latin America &amp; Caribbean </td>
   <td style="text-align:left;"> Americas </td>
  </tr>
  <tr>
   <td style="text-align:left;"> Japan </td>
   <td style="text-align:left;"> JPN </td>
   <td style="text-align:right;"> 1995 </td>
   <td style="text-align:right;"> 0.0387259 </td>
   <td style="text-align:left;"> East Asia &amp; Pacific </td>
   <td style="text-align:left;"> Asia </td>
  </tr>
  <tr>
   <td style="text-align:left;"> Turkmenistan </td>
   <td style="text-align:left;"> TKM </td>
   <td style="text-align:right;"> 2009 </td>
   <td style="text-align:right;"> 0.0549753 </td>
   <td style="text-align:left;"> Europe &amp; Central Asia </td>
   <td style="text-align:left;"> Asia </td>
  </tr>
  <tr>
   <td style="text-align:left;"> Bhutan </td>
   <td style="text-align:left;"> BTN </td>
   <td style="text-align:right;"> 2009 </td>
   <td style="text-align:right;"> 11.5951814 </td>
   <td style="text-align:left;"> South Asia </td>
   <td style="text-align:left;"> Asia </td>
  </tr>
  <tr>
   <td style="text-align:left;"> Indonesia </td>
   <td style="text-align:left;"> IDN </td>
   <td style="text-align:right;"> 1992 </td>
   <td style="text-align:right;"> 9.7611786 </td>
   <td style="text-align:left;"> East Asia &amp; Pacific </td>
   <td style="text-align:left;"> Asia </td>
  </tr>
  <tr>
   <td style="text-align:left;"> Mauritania </td>
   <td style="text-align:left;"> MRT </td>
   <td style="text-align:right;"> 1999 </td>
   <td style="text-align:right;"> 9.8084289 </td>
   <td style="text-align:left;"> Sub-Saharan Africa </td>
   <td style="text-align:left;"> Africa </td>
  </tr>
  <tr>
   <td style="text-align:left;"> Tajikistan </td>
   <td style="text-align:left;"> TJK </td>
   <td style="text-align:right;"> 2008 </td>
   <td style="text-align:right;"> 9.4014500 </td>
   <td style="text-align:left;"> Europe &amp; Central Asia </td>
   <td style="text-align:left;"> Asia </td>
  </tr>
  <tr>
   <td style="text-align:left;"> Italy </td>
   <td style="text-align:left;"> ITA </td>
   <td style="text-align:right;"> 2007 </td>
   <td style="text-align:right;"> 0.0369913 </td>
   <td style="text-align:left;"> Europe &amp; Central Asia </td>
   <td style="text-align:left;"> Europe </td>
  </tr>
  <tr>
   <td style="text-align:left;"> Myanmar </td>
   <td style="text-align:left;"> MMR </td>
   <td style="text-align:right;"> 2017 </td>
   <td style="text-align:right;"> 12.4578100 </td>
   <td style="text-align:left;"> East Asia &amp; Pacific </td>
   <td style="text-align:left;"> Asia </td>
  </tr>
  <tr>
   <td style="text-align:left;"> Saudi Arabia </td>
   <td style="text-align:left;"> SAU </td>
   <td style="text-align:right;"> 1995 </td>
   <td style="text-align:right;"> 3.0067568 </td>
   <td style="text-align:left;"> Middle East &amp; North Africa </td>
   <td style="text-align:left;"> Asia </td>
  </tr>
  <tr>
   <td style="text-align:left;"> Montenegro </td>
   <td style="text-align:left;"> MNE </td>
   <td style="text-align:right;"> 2010 </td>
   <td style="text-align:right;"> 2.9028185 </td>
   <td style="text-align:left;"> Europe &amp; Central Asia </td>
   <td style="text-align:left;"> Europe </td>
  </tr>
  <tr>
   <td style="text-align:left;"> Uruguay </td>
   <td style="text-align:left;"> URY </td>
   <td style="text-align:right;"> 2005 </td>
   <td style="text-align:right;"> 0.3896433 </td>
   <td style="text-align:left;"> Latin America &amp; Caribbean </td>
   <td style="text-align:left;"> Americas </td>
  </tr>
  <tr>
   <td style="text-align:left;"> Trinidad and Tobago </td>
   <td style="text-align:left;"> TTO </td>
   <td style="text-align:right;"> 2017 </td>
   <td style="text-align:right;"> 0.0125572 </td>
   <td style="text-align:left;"> Latin America &amp; Caribbean </td>
   <td style="text-align:left;"> Americas </td>
  </tr>
  <tr>
   <td style="text-align:left;"> Turkmenistan </td>
   <td style="text-align:left;"> TKM </td>
   <td style="text-align:right;"> 1998 </td>
   <td style="text-align:right;"> 0.1220276 </td>
   <td style="text-align:left;"> Europe &amp; Central Asia </td>
   <td style="text-align:left;"> Asia </td>
  </tr>
  <tr>
   <td style="text-align:left;"> Armenia </td>
   <td style="text-align:left;"> ARM </td>
   <td style="text-align:right;"> 1995 </td>
   <td style="text-align:right;"> 4.0777037 </td>
   <td style="text-align:left;"> Europe &amp; Central Asia </td>
   <td style="text-align:left;"> Asia </td>
  </tr>
  <tr>
   <td style="text-align:left;"> France </td>
   <td style="text-align:left;"> FRA </td>
   <td style="text-align:right;"> 2001 </td>
   <td style="text-align:right;"> 0.0190436 </td>
   <td style="text-align:left;"> Europe &amp; Central Asia </td>
   <td style="text-align:left;"> Europe </td>
  </tr>
  <tr>
   <td style="text-align:left;"> Barbados </td>
   <td style="text-align:left;"> BRB </td>
   <td style="text-align:right;"> 1999 </td>
   <td style="text-align:right;"> 0.0209001 </td>
   <td style="text-align:left;"> Latin America &amp; Caribbean </td>
   <td style="text-align:left;"> Americas </td>
  </tr>
  <tr>
   <td style="text-align:left;"> Belarus </td>
   <td style="text-align:left;"> BLR </td>
   <td style="text-align:right;"> 2015 </td>
   <td style="text-align:right;"> 0.1001278 </td>
   <td style="text-align:left;"> Europe &amp; Central Asia </td>
   <td style="text-align:left;"> Europe </td>
  </tr>
  <tr>
   <td style="text-align:left;"> Gambia </td>
   <td style="text-align:left;"> GMB </td>
   <td style="text-align:right;"> 1997 </td>
   <td style="text-align:right;"> 13.3604815 </td>
   <td style="text-align:left;"> Sub-Saharan Africa </td>
   <td style="text-align:left;"> Africa </td>
  </tr>
  <tr>
   <td style="text-align:left;"> Uganda </td>
   <td style="text-align:left;"> UGA </td>
   <td style="text-align:right;"> 2019 </td>
   <td style="text-align:right;"> 11.1763657 </td>
   <td style="text-align:left;"> Sub-Saharan Africa </td>
   <td style="text-align:left;"> Africa </td>
  </tr>
  <tr>
   <td style="text-align:left;"> Luxembourg </td>
   <td style="text-align:left;"> LUX </td>
   <td style="text-align:right;"> 2018 </td>
   <td style="text-align:right;"> 0.0050964 </td>
   <td style="text-align:left;"> Europe &amp; Central Asia </td>
   <td style="text-align:left;"> Europe </td>
  </tr>
  <tr>
   <td style="text-align:left;"> Bolivia </td>
   <td style="text-align:left;"> BOL </td>
   <td style="text-align:right;"> 2012 </td>
   <td style="text-align:right;"> 4.4212486 </td>
   <td style="text-align:left;"> Latin America &amp; Caribbean </td>
   <td style="text-align:left;"> Americas </td>
  </tr>
  <tr>
   <td style="text-align:left;"> Serbia </td>
   <td style="text-align:left;"> SRB </td>
   <td style="text-align:right;"> 2015 </td>
   <td style="text-align:right;"> 1.8118834 </td>
   <td style="text-align:left;"> Europe &amp; Central Asia </td>
   <td style="text-align:left;"> Europe </td>
  </tr>
  <tr>
   <td style="text-align:left;"> Bangladesh </td>
   <td style="text-align:left;"> BGD </td>
   <td style="text-align:right;"> 2007 </td>
   <td style="text-align:right;"> 16.7337676 </td>
   <td style="text-align:left;"> South Asia </td>
   <td style="text-align:left;"> Asia </td>
  </tr>
  <tr>
   <td style="text-align:left;"> North Korea </td>
   <td style="text-align:left;"> PRK </td>
   <td style="text-align:right;"> 2010 </td>
   <td style="text-align:right;"> 16.6592005 </td>
   <td style="text-align:left;"> East Asia &amp; Pacific </td>
   <td style="text-align:left;"> Asia </td>
  </tr>
  <tr>
   <td style="text-align:left;"> Liberia </td>
   <td style="text-align:left;"> LBR </td>
   <td style="text-align:right;"> 2016 </td>
   <td style="text-align:right;"> 10.9697544 </td>
   <td style="text-align:left;"> Sub-Saharan Africa </td>
   <td style="text-align:left;"> Africa </td>
  </tr>
  <tr>
   <td style="text-align:left;"> Lebanon </td>
   <td style="text-align:left;"> LBN </td>
   <td style="text-align:right;"> 2017 </td>
   <td style="text-align:right;"> 0.0356969 </td>
   <td style="text-align:left;"> Middle East &amp; North Africa </td>
   <td style="text-align:left;"> Asia </td>
  </tr>
  <tr>
   <td style="text-align:left;"> South Korea </td>
   <td style="text-align:left;"> KOR </td>
   <td style="text-align:right;"> 2003 </td>
   <td style="text-align:right;"> 0.0237672 </td>
   <td style="text-align:left;"> East Asia &amp; Pacific </td>
   <td style="text-align:left;"> Asia </td>
  </tr>
  <tr>
   <td style="text-align:left;"> Indonesia </td>
   <td style="text-align:left;"> IDN </td>
   <td style="text-align:right;"> 1999 </td>
   <td style="text-align:right;"> 8.0169192 </td>
   <td style="text-align:left;"> East Asia &amp; Pacific </td>
   <td style="text-align:left;"> Asia </td>
  </tr>
  <tr>
   <td style="text-align:left;"> Chad </td>
   <td style="text-align:left;"> TCD </td>
   <td style="text-align:right;"> 1995 </td>
   <td style="text-align:right;"> 14.7487894 </td>
   <td style="text-align:left;"> Sub-Saharan Africa </td>
   <td style="text-align:left;"> Africa </td>
  </tr>
  <tr>
   <td style="text-align:left;"> Bangladesh </td>
   <td style="text-align:left;"> BGD </td>
   <td style="text-align:right;"> 2014 </td>
   <td style="text-align:right;"> 13.2256805 </td>
   <td style="text-align:left;"> South Asia </td>
   <td style="text-align:left;"> Asia </td>
  </tr>
  <tr>
   <td style="text-align:left;"> Palau </td>
   <td style="text-align:left;"> PLW </td>
   <td style="text-align:right;"> 2012 </td>
   <td style="text-align:right;"> 0.0069264 </td>
   <td style="text-align:left;"> East Asia &amp; Pacific </td>
   <td style="text-align:left;"> Oceania </td>
  </tr>
  <tr>
   <td style="text-align:left;"> Guatemala </td>
   <td style="text-align:left;"> GTM </td>
   <td style="text-align:right;"> 2019 </td>
   <td style="text-align:right;"> 6.0633548 </td>
   <td style="text-align:left;"> Latin America &amp; Caribbean </td>
   <td style="text-align:left;"> Americas </td>
  </tr>
  <tr>
   <td style="text-align:left;"> Antigua and Barbuda </td>
   <td style="text-align:left;"> ATG </td>
   <td style="text-align:right;"> 2017 </td>
   <td style="text-align:right;"> 0.0951739 </td>
   <td style="text-align:left;"> Latin America &amp; Caribbean </td>
   <td style="text-align:left;"> Americas </td>
  </tr>
  <tr>
   <td style="text-align:left;"> Niger </td>
   <td style="text-align:left;"> NER </td>
   <td style="text-align:right;"> 2014 </td>
   <td style="text-align:right;"> 15.2807980 </td>
   <td style="text-align:left;"> Sub-Saharan Africa </td>
   <td style="text-align:left;"> Africa </td>
  </tr>
  <tr>
   <td style="text-align:left;"> Algeria </td>
   <td style="text-align:left;"> DZA </td>
   <td style="text-align:right;"> 2008 </td>
   <td style="text-align:right;"> 0.1763812 </td>
   <td style="text-align:left;"> Middle East &amp; North Africa </td>
   <td style="text-align:left;"> Africa </td>
  </tr>
  <tr>
   <td style="text-align:left;"> Netherlands </td>
   <td style="text-align:left;"> NLD </td>
   <td style="text-align:right;"> 2012 </td>
   <td style="text-align:right;"> 0.0051779 </td>
   <td style="text-align:left;"> Europe &amp; Central Asia </td>
   <td style="text-align:left;"> Europe </td>
  </tr>
  <tr>
   <td style="text-align:left;"> Togo </td>
   <td style="text-align:left;"> TGO </td>
   <td style="text-align:right;"> 1998 </td>
   <td style="text-align:right;"> 11.9795518 </td>
   <td style="text-align:left;"> Sub-Saharan Africa </td>
   <td style="text-align:left;"> Africa </td>
  </tr>
  <tr>
   <td style="text-align:left;"> Cape Verde </td>
   <td style="text-align:left;"> CPV </td>
   <td style="text-align:right;"> 2019 </td>
   <td style="text-align:right;"> 2.9312565 </td>
   <td style="text-align:left;"> Sub-Saharan Africa </td>
   <td style="text-align:left;"> Africa </td>
  </tr>
  <tr>
   <td style="text-align:left;"> Lesotho </td>
   <td style="text-align:left;"> LSO </td>
   <td style="text-align:right;"> 1993 </td>
   <td style="text-align:right;"> 11.1795001 </td>
   <td style="text-align:left;"> Sub-Saharan Africa </td>
   <td style="text-align:left;"> Africa </td>
  </tr>
  <tr>
   <td style="text-align:left;"> Singapore </td>
   <td style="text-align:left;"> SGP </td>
   <td style="text-align:right;"> 1999 </td>
   <td style="text-align:right;"> 0.1159034 </td>
   <td style="text-align:left;"> East Asia &amp; Pacific </td>
   <td style="text-align:left;"> Asia </td>
  </tr>
  <tr>
   <td style="text-align:left;"> Dominica </td>
   <td style="text-align:left;"> DMA </td>
   <td style="text-align:right;"> 2001 </td>
   <td style="text-align:right;"> 1.7132676 </td>
   <td style="text-align:left;"> Latin America &amp; Caribbean </td>
   <td style="text-align:left;"> Americas </td>
  </tr>
  <tr>
   <td style="text-align:left;"> Azerbaijan </td>
   <td style="text-align:left;"> AZE </td>
   <td style="text-align:right;"> 2016 </td>
   <td style="text-align:right;"> 1.0134733 </td>
   <td style="text-align:left;"> Europe &amp; Central Asia </td>
   <td style="text-align:left;"> Asia </td>
  </tr>
  <tr>
   <td style="text-align:left;"> New Zealand </td>
   <td style="text-align:left;"> NZL </td>
   <td style="text-align:right;"> 2016 </td>
   <td style="text-align:right;"> 0.0083834 </td>
   <td style="text-align:left;"> East Asia &amp; Pacific </td>
   <td style="text-align:left;"> Oceania </td>
  </tr>
  <tr>
   <td style="text-align:left;"> Nicaragua </td>
   <td style="text-align:left;"> NIC </td>
   <td style="text-align:right;"> 2001 </td>
   <td style="text-align:right;"> 10.6326676 </td>
   <td style="text-align:left;"> Latin America &amp; Caribbean </td>
   <td style="text-align:left;"> Americas </td>
  </tr>
  <tr>
   <td style="text-align:left;"> Guyana </td>
   <td style="text-align:left;"> GUY </td>
   <td style="text-align:right;"> 1999 </td>
   <td style="text-align:right;"> 3.0904560 </td>
   <td style="text-align:left;"> Latin America &amp; Caribbean </td>
   <td style="text-align:left;"> Americas </td>
  </tr>
  <tr>
   <td style="text-align:left;"> Sri Lanka </td>
   <td style="text-align:left;"> LKA </td>
   <td style="text-align:right;"> 2002 </td>
   <td style="text-align:right;"> 7.7091634 </td>
   <td style="text-align:left;"> South Asia </td>
   <td style="text-align:left;"> Asia </td>
  </tr>
  <tr>
   <td style="text-align:left;"> Andorra </td>
   <td style="text-align:left;"> AND </td>
   <td style="text-align:right;"> 2005 </td>
   <td style="text-align:right;"> 0.0072572 </td>
   <td style="text-align:left;"> Europe &amp; Central Asia </td>
   <td style="text-align:left;"> Europe </td>
  </tr>
  <tr>
   <td style="text-align:left;"> Tanzania </td>
   <td style="text-align:left;"> TZA </td>
   <td style="text-align:right;"> 1992 </td>
   <td style="text-align:right;"> 11.9568481 </td>
   <td style="text-align:left;"> Sub-Saharan Africa </td>
   <td style="text-align:left;"> Africa </td>
  </tr>
  <tr>
   <td style="text-align:left;"> Ukraine </td>
   <td style="text-align:left;"> UKR </td>
   <td style="text-align:right;"> 2011 </td>
   <td style="text-align:right;"> 0.5303188 </td>
   <td style="text-align:left;"> Europe &amp; Central Asia </td>
   <td style="text-align:left;"> Europe </td>
  </tr>
  <tr>
   <td style="text-align:left;"> Lithuania </td>
   <td style="text-align:left;"> LTU </td>
   <td style="text-align:right;"> 2014 </td>
   <td style="text-align:right;"> 0.2051428 </td>
   <td style="text-align:left;"> Europe &amp; Central Asia </td>
   <td style="text-align:left;"> Europe </td>
  </tr>
  <tr>
   <td style="text-align:left;"> Ghana </td>
   <td style="text-align:left;"> GHA </td>
   <td style="text-align:right;"> 2005 </td>
   <td style="text-align:right;"> 9.0486328 </td>
   <td style="text-align:left;"> Sub-Saharan Africa </td>
   <td style="text-align:left;"> Africa </td>
  </tr>
  <tr>
   <td style="text-align:left;"> New Zealand </td>
   <td style="text-align:left;"> NZL </td>
   <td style="text-align:right;"> 2006 </td>
   <td style="text-align:right;"> 0.0160841 </td>
   <td style="text-align:left;"> East Asia &amp; Pacific </td>
   <td style="text-align:left;"> Oceania </td>
  </tr>
  <tr>
   <td style="text-align:left;"> Poland </td>
   <td style="text-align:left;"> POL </td>
   <td style="text-align:right;"> 1998 </td>
   <td style="text-align:right;"> 2.0630118 </td>
   <td style="text-align:left;"> Europe &amp; Central Asia </td>
   <td style="text-align:left;"> Europe </td>
  </tr>
  <tr>
   <td style="text-align:left;"> Marshall Islands </td>
   <td style="text-align:left;"> MHL </td>
   <td style="text-align:right;"> 1993 </td>
   <td style="text-align:right;"> 9.9426165 </td>
   <td style="text-align:left;"> East Asia &amp; Pacific </td>
   <td style="text-align:left;"> Oceania </td>
  </tr>
  <tr>
   <td style="text-align:left;"> Suriname </td>
   <td style="text-align:left;"> SUR </td>
   <td style="text-align:right;"> 2010 </td>
   <td style="text-align:right;"> 1.8084414 </td>
   <td style="text-align:left;"> Latin America &amp; Caribbean </td>
   <td style="text-align:left;"> Americas </td>
  </tr>
  <tr>
   <td style="text-align:left;"> Poland </td>
   <td style="text-align:left;"> POL </td>
   <td style="text-align:right;"> 1999 </td>
   <td style="text-align:right;"> 1.9249036 </td>
   <td style="text-align:left;"> Europe &amp; Central Asia </td>
   <td style="text-align:left;"> Europe </td>
  </tr>
  <tr>
   <td style="text-align:left;"> Niger </td>
   <td style="text-align:left;"> NER </td>
   <td style="text-align:right;"> 2018 </td>
   <td style="text-align:right;"> 15.9351940 </td>
   <td style="text-align:left;"> Sub-Saharan Africa </td>
   <td style="text-align:left;"> Africa </td>
  </tr>
  <tr>
   <td style="text-align:left;"> Angola </td>
   <td style="text-align:left;"> AGO </td>
   <td style="text-align:right;"> 2009 </td>
   <td style="text-align:right;"> 9.8825685 </td>
   <td style="text-align:left;"> Sub-Saharan Africa </td>
   <td style="text-align:left;"> Africa </td>
  </tr>
  <tr>
   <td style="text-align:left;"> Cameroon </td>
   <td style="text-align:left;"> CMR </td>
   <td style="text-align:right;"> 2017 </td>
   <td style="text-align:right;"> 7.1194224 </td>
   <td style="text-align:left;"> Sub-Saharan Africa </td>
   <td style="text-align:left;"> Africa </td>
  </tr>
  <tr>
   <td style="text-align:left;"> Malaysia </td>
   <td style="text-align:left;"> MYS </td>
   <td style="text-align:right;"> 2003 </td>
   <td style="text-align:right;"> 0.2143972 </td>
   <td style="text-align:left;"> East Asia &amp; Pacific </td>
   <td style="text-align:left;"> Asia </td>
  </tr>
  <tr>
   <td style="text-align:left;"> Peru </td>
   <td style="text-align:left;"> PER </td>
   <td style="text-align:right;"> 2015 </td>
   <td style="text-align:right;"> 2.3680350 </td>
   <td style="text-align:left;"> Latin America &amp; Caribbean </td>
   <td style="text-align:left;"> Americas </td>
  </tr>
  <tr>
   <td style="text-align:left;"> Iceland </td>
   <td style="text-align:left;"> ISL </td>
   <td style="text-align:right;"> 2019 </td>
   <td style="text-align:right;"> 0.0033658 </td>
   <td style="text-align:left;"> Europe &amp; Central Asia </td>
   <td style="text-align:left;"> Europe </td>
  </tr>
  <tr>
   <td style="text-align:left;"> Singapore </td>
   <td style="text-align:left;"> SGP </td>
   <td style="text-align:right;"> 2009 </td>
   <td style="text-align:right;"> 0.0222953 </td>
   <td style="text-align:left;"> East Asia &amp; Pacific </td>
   <td style="text-align:left;"> Asia </td>
  </tr>
  <tr>
   <td style="text-align:left;"> Uzbekistan </td>
   <td style="text-align:left;"> UZB </td>
   <td style="text-align:right;"> 2005 </td>
   <td style="text-align:right;"> 4.1693585 </td>
   <td style="text-align:left;"> Europe &amp; Central Asia </td>
   <td style="text-align:left;"> Asia </td>
  </tr>
  <tr>
   <td style="text-align:left;"> Bermuda </td>
   <td style="text-align:left;"> BMU </td>
   <td style="text-align:right;"> 1993 </td>
   <td style="text-align:right;"> 1.0048165 </td>
   <td style="text-align:left;"> North America </td>
   <td style="text-align:left;"> Americas </td>
  </tr>
  <tr>
   <td style="text-align:left;"> Uzbekistan </td>
   <td style="text-align:left;"> UZB </td>
   <td style="text-align:right;"> 1992 </td>
   <td style="text-align:right;"> 6.8665761 </td>
   <td style="text-align:left;"> Europe &amp; Central Asia </td>
   <td style="text-align:left;"> Asia </td>
  </tr>
  <tr>
   <td style="text-align:left;"> American Samoa </td>
   <td style="text-align:left;"> ASM </td>
   <td style="text-align:right;"> 1998 </td>
   <td style="text-align:right;"> 3.0939245 </td>
   <td style="text-align:left;"> East Asia &amp; Pacific </td>
   <td style="text-align:left;"> Oceania </td>
  </tr>
  <tr>
   <td style="text-align:left;"> Namibia </td>
   <td style="text-align:left;"> NAM </td>
   <td style="text-align:right;"> 2018 </td>
   <td style="text-align:right;"> 4.7609914 </td>
   <td style="text-align:left;"> Sub-Saharan Africa </td>
   <td style="text-align:left;"> Africa </td>
  </tr>
  <tr>
   <td style="text-align:left;"> Latvia </td>
   <td style="text-align:left;"> LVA </td>
   <td style="text-align:right;"> 2003 </td>
   <td style="text-align:right;"> 0.8337569 </td>
   <td style="text-align:left;"> Europe &amp; Central Asia </td>
   <td style="text-align:left;"> Europe </td>
  </tr>
  <tr>
   <td style="text-align:left;"> Taiwan </td>
   <td style="text-align:left;"> TWN </td>
   <td style="text-align:right;"> 1999 </td>
   <td style="text-align:right;"> 1.2086446 </td>
   <td style="text-align:left;"> East Asia &amp; Pacific </td>
   <td style="text-align:left;"> Asia </td>
  </tr>
  <tr>
   <td style="text-align:left;"> Serbia </td>
   <td style="text-align:left;"> SRB </td>
   <td style="text-align:right;"> 1998 </td>
   <td style="text-align:right;"> 5.1890268 </td>
   <td style="text-align:left;"> Europe &amp; Central Asia </td>
   <td style="text-align:left;"> Europe </td>
  </tr>
  <tr>
   <td style="text-align:left;"> Cambodia </td>
   <td style="text-align:left;"> KHM </td>
   <td style="text-align:right;"> 2011 </td>
   <td style="text-align:right;"> 14.7934822 </td>
   <td style="text-align:left;"> East Asia &amp; Pacific </td>
   <td style="text-align:left;"> Asia </td>
  </tr>
  <tr>
   <td style="text-align:left;"> Australia </td>
   <td style="text-align:left;"> AUS </td>
   <td style="text-align:right;"> 2012 </td>
   <td style="text-align:right;"> 0.0120786 </td>
   <td style="text-align:left;"> East Asia &amp; Pacific </td>
   <td style="text-align:left;"> Oceania </td>
  </tr>
  <tr>
   <td style="text-align:left;"> Libya </td>
   <td style="text-align:left;"> LBY </td>
   <td style="text-align:right;"> 2001 </td>
   <td style="text-align:right;"> 0.3813018 </td>
   <td style="text-align:left;"> Middle East &amp; North Africa </td>
   <td style="text-align:left;"> Africa </td>
  </tr>
</tbody>
</table>

`````

:::
:::

::: {.cell}

```{.r .cell-code}
library(countrycode)

exploratory_data %>%
  
  rename(percent_deaths_by_household_pollution = deaths_cause_all_causes_risk_household_air_pollution_from_solid_fuels_sex_both_age_age_standardized_percent) %>%

  
   mutate(country_region = countrycode(entity, origin = "country.name", destination = "region")) %>%
  
  mutate(continent = countrycode (entity, origin = "country.name", destination = "continent")) %>%

   filter(continent!= 0) %>%
  
  ggplot() + 
  geom_histogram(mapping = aes(fill = country_region, 
                               x= percent_deaths_by_household_pollution,
                               na.rm = TRUE)) +
  facet_wrap(~country_region)
```

::: {.cell-output .cell-output-stderr}
```
Warning: There was 1 warning in `mutate()`.
ℹ In argument: `country_region = countrycode(entity, origin = "country.name",
  destination = "region")`.
Caused by warning in `countrycode_convert()`:
! Some values were not matched unambiguously: Africa, African Region, African Union, America, Andean Latin America, Asia, Australasia, Caribbean, Central Asia, Central Europe, Central Europe, Eastern Europe, and Central Asia, Central Latin America, Central sub-Saharan Africa, Commonwealth, Commonwealth High Income, Commonwealth Low Income, Commonwealth Middle Income, East Asia, East Asia & Pacific - World Bank region, Eastern Europe, Eastern Mediterranean Region, Eastern sub-Saharan Africa, England, Europe, Europe & Central Asia - World Bank region, European Region, European Union, G20, High-income, High-income Asia Pacific, High-income North America, High-middle SDI, High SDI, Latin America & Caribbean - World Bank region, Low-middle SDI, Low SDI, Micronesia (country), Middle East & North Africa, Middle SDI, Nordic Region, North Africa and Middle East, North America, Northern Ireland, Oceania, OECD Countries, Region of the Americas, Scotland, South-East Asia Region, South Asia - World Bank region, Southeast Asia, Southeast Asia, East Asia, and Oceania, Southern Latin America, Southern sub-Saharan Africa, Sub-Saharan Africa - World Bank region, Timor, Tropical Latin America, Wales, Western Europe, Western Pacific Region, Western sub-Saharan Africa, World, World Bank High Income, World Bank Low Income, World Bank Lower Middle Income, World Bank Upper Middle Income
```
:::

::: {.cell-output .cell-output-stderr}
```
Warning: There was 1 warning in `mutate()`.
ℹ In argument: `continent = countrycode(entity, origin = "country.name",
  destination = "continent")`.
Caused by warning in `countrycode_convert()`:
! Some values were not matched unambiguously: Africa, African Region, African Union, America, Andean Latin America, Asia, Australasia, Caribbean, Central Asia, Central Europe, Central Europe, Eastern Europe, and Central Asia, Central Latin America, Central sub-Saharan Africa, Commonwealth, Commonwealth High Income, Commonwealth Low Income, Commonwealth Middle Income, East Asia, East Asia & Pacific - World Bank region, Eastern Europe, Eastern Mediterranean Region, Eastern sub-Saharan Africa, England, Europe, Europe & Central Asia - World Bank region, European Region, European Union, G20, High-income, High-income Asia Pacific, High-income North America, High-middle SDI, High SDI, Latin America & Caribbean - World Bank region, Low-middle SDI, Low SDI, Micronesia (country), Middle East & North Africa, Middle SDI, Nordic Region, North Africa and Middle East, North America, Northern Ireland, Oceania, OECD Countries, Region of the Americas, Scotland, South-East Asia Region, South Asia - World Bank region, Southeast Asia, Southeast Asia, East Asia, and Oceania, Southern Latin America, Southern sub-Saharan Africa, Sub-Saharan Africa - World Bank region, Timor, Tropical Latin America, Wales, Western Europe, Western Pacific Region, Western sub-Saharan Africa, World, World Bank High Income, World Bank Low Income, World Bank Lower Middle Income, World Bank Upper Middle Income
```
:::

::: {.cell-output .cell-output-stderr}
```
Warning in geom_histogram(mapping = aes(fill = country_region, x =
percent_deaths_by_household_pollution, : Ignoring unknown aesthetics: na.rm
```
:::

::: {.cell-output .cell-output-stderr}
```
`stat_bin()` using `bins = 30`. Pick better value with `binwidth`.
```
:::

::: {.cell-output-display}
![](IndoorPollution_files/figure-html/unnamed-chunk-5-1.png){width=672}
:::
:::


::: callout-note
## Observations

-   these graphs show variables taken across a range of 30 years where the number of deaths in the country attributed to indoor air pollution caused by the combustion of bio fuels. each graph represents a distribution of these variables grouped by region.

    -   **East Asia & Pacific** contains a large distribution of countries with different rates of death. there is a larger amount of countries in east asia with a very low percentage of deaths for this cause.

    -   **Europe and Central Asia** has the largest amount of variables measured at 0 percent deaths caused by IAP (over 125) compared to any other region. Nearly all variables in this region are below 10 percent deaths by IAP, but a fairly large amount of variables fall between 1% and 5%.

    -   **Latin America & Caribbean** data is relatively spread out across 0% through 5% with over 100 countries in this group. there are a a smaller number of countries spread between 6% and 15% with no species over 15 percent.

    -   **Middle East and North Africa** does not contain a large number of countries in the histogram (less then 100). The largest portion are between 0% and 5%, relatively low levels compared to other countries like East Asia and Sub-Saharan Africa.

    -   **North America** is represented by the fewest countries of any region in this comparison. Considering that two of these countries are the U.S and Canada it is not suprising that there are a very low percentage of deaths per year caused by indoor air pollution.

    -   **South Asia** is not represented by any countries that are 0 or even nearly 0% deaths caused by IAP from bio fuels. Although this region has \~50 variables, the % death ranges from a minimum of 4% to as high as 19%.

    -   **Sub-Saharan Africa** is represented by a larger number of variables that are in the 5%-15% range. this region will likely provide an interesting year-to-year comparison that could show improvement over time.

-   because these variables are taken across such a wide temporal scale, it does not serve as a distinctly insightful comparison. Still, some things can be inferred.
:::

What am I trying to do?

graph the average percent deaths by region to determine if there is a correlation b


::: {.cell}

```{.r .cell-code}
exploratory_data %>%
  
  filter(year>2014) %>%
  
  rename(n = deaths_cause_all_causes_risk_household_air_pollution_from_solid_fuels_sex_both_age_age_standardized_percent) %>%
  
   mutate("Regions" = countrycode(entity, 
                                       origin = "country.name", 
                                       destination= "region"
                                       )) %>%
  filter(!is.na(Regions)) %>%
  group_by(Regions) %>%

  summarize(
    'Average Percent Deaths by Region' = mean(n),
    'Standard Deviation of % Deaths by Region' = sd(n),
    
    'Number of Variables Measured' = length(unique(entity))) %>%
  
 kable(digits = c(0,4,4,0)) %>%
    kable_styling(bootstrap_options = c("hover", "striped"))
```

::: {.cell-output .cell-output-stderr}
```
Warning: There was 1 warning in `mutate()`.
ℹ In argument: `Regions = countrycode(entity, origin = "country.name",
  destination = "region")`.
Caused by warning in `countrycode_convert()`:
! Some values were not matched unambiguously: African Union, Andean Latin America, Asia, Australasia, Central Europe, Commonwealth, Commonwealth Low Income, Commonwealth Middle Income, East Asia & Pacific - World Bank region, Eastern Europe, Eastern Mediterranean Region, Eastern sub-Saharan Africa, Europe, Europe & Central Asia - World Bank region, European Region, European Union, G20, High-income Asia Pacific, High-income North America, High-middle SDI, Latin America & Caribbean - World Bank region, Micronesia (country), Middle East & North Africa, Middle SDI, Oceania, OECD Countries, Region of the Americas, South Asia - World Bank region, Southeast Asia, Southern Latin America, Sub-Saharan Africa - World Bank region, Timor, Wales, Western Europe, Western sub-Saharan Africa, World Bank High Income, World Bank Low Income
```
:::

::: {.cell-output-display}

`````{=html}
<table class="table table-hover table-striped" style="margin-left: auto; margin-right: auto;">
 <thead>
  <tr>
   <th style="text-align:left;"> Regions </th>
   <th style="text-align:right;"> Average Percent Deaths by Region </th>
   <th style="text-align:right;"> Standard Deviation of % Deaths by Region </th>
   <th style="text-align:right;"> Number of Variables Measured </th>
  </tr>
 </thead>
<tbody>
  <tr>
   <td style="text-align:left;"> East Asia &amp; Pacific </td>
   <td style="text-align:right;"> 5.4629 </td>
   <td style="text-align:right;"> 6.5259 </td>
   <td style="text-align:right;"> 13 </td>
  </tr>
  <tr>
   <td style="text-align:left;"> Europe &amp; Central Asia </td>
   <td style="text-align:right;"> 0.5155 </td>
   <td style="text-align:right;"> 0.7869 </td>
   <td style="text-align:right;"> 32 </td>
  </tr>
  <tr>
   <td style="text-align:left;"> Latin America &amp; Caribbean </td>
   <td style="text-align:right;"> 1.5908 </td>
   <td style="text-align:right;"> 1.9561 </td>
   <td style="text-align:right;"> 16 </td>
  </tr>
  <tr>
   <td style="text-align:left;"> Middle East &amp; North Africa </td>
   <td style="text-align:right;"> 1.0283 </td>
   <td style="text-align:right;"> 2.3061 </td>
   <td style="text-align:right;"> 11 </td>
  </tr>
  <tr>
   <td style="text-align:left;"> North America </td>
   <td style="text-align:right;"> 0.0506 </td>
   <td style="text-align:right;"> 0.0780 </td>
   <td style="text-align:right;"> 2 </td>
  </tr>
  <tr>
   <td style="text-align:left;"> South Asia </td>
   <td style="text-align:right;"> 7.7096 </td>
   <td style="text-align:right;"> 1.8564 </td>
   <td style="text-align:right;"> 3 </td>
  </tr>
  <tr>
   <td style="text-align:left;"> Sub-Saharan Africa </td>
   <td style="text-align:right;"> 9.2736 </td>
   <td style="text-align:right;"> 4.0055 </td>
   <td style="text-align:right;"> 30 </td>
  </tr>
</tbody>
</table>

`````

:::
:::


::: callout-note
## Observations

This data summarizes the variables from only 2014-2019 to see a comparison of all the countries in recent years. Something that can be inferred from this data is that some countries have a significantly more significant standard deviation then others.

-   While South Asia and Sub-Saharan Africa have the highest average, the deviation is much smaller, suggesting most of the variables (countries) measured in this timespan are relatively close to the 10.00 % mean percent deaths by IAP

<!-- -->

    -   East Asia & Pacific are significantly lower in percent deaths by IAP at nearly half of the previously mentioned, however, the standard deviation is over 1% greater then either South Asia or Sub-Saharan Africa, suggesting that some countries could be significantly worse off then most.

    -   although Middle East & North Africa does not have a particularly large standard deviation, it is the largest in comparison to its average. This Region along with Latin America & Caribbean do not fit this theory as nicely, and show the need for economic factors that play a role in deaths caused by pollution.

        -   This will be important for my hypothesis, since the geographic position of East Asia & pacific would likely make climate an interesting factor for comparison.

<!-- -->

-   Regions with the lowest deviation from the mean were Europe & Central Asia as well as North America (despite lacking a standard deviation, it is only made up of 3 countries and likely deviated minimally
:::


::: {.cell}

```{.r .cell-code}
exploratory_data %>%
  
count(year) %>%
 
  kable() %>%
    kable_styling(bootstrap_options = c("hover", "striped"))
```

::: {.cell-output-display}

`````{=html}
<table class="table table-hover table-striped" style="margin-left: auto; margin-right: auto;">
 <thead>
  <tr>
   <th style="text-align:right;"> year </th>
   <th style="text-align:right;"> n </th>
  </tr>
 </thead>
<tbody>
  <tr>
   <td style="text-align:right;"> 1990 </td>
   <td style="text-align:right;"> 43 </td>
  </tr>
  <tr>
   <td style="text-align:right;"> 1991 </td>
   <td style="text-align:right;"> 24 </td>
  </tr>
  <tr>
   <td style="text-align:right;"> 1992 </td>
   <td style="text-align:right;"> 45 </td>
  </tr>
  <tr>
   <td style="text-align:right;"> 1993 </td>
   <td style="text-align:right;"> 38 </td>
  </tr>
  <tr>
   <td style="text-align:right;"> 1994 </td>
   <td style="text-align:right;"> 27 </td>
  </tr>
  <tr>
   <td style="text-align:right;"> 1995 </td>
   <td style="text-align:right;"> 31 </td>
  </tr>
  <tr>
   <td style="text-align:right;"> 1996 </td>
   <td style="text-align:right;"> 41 </td>
  </tr>
  <tr>
   <td style="text-align:right;"> 1997 </td>
   <td style="text-align:right;"> 32 </td>
  </tr>
  <tr>
   <td style="text-align:right;"> 1998 </td>
   <td style="text-align:right;"> 49 </td>
  </tr>
  <tr>
   <td style="text-align:right;"> 1999 </td>
   <td style="text-align:right;"> 46 </td>
  </tr>
  <tr>
   <td style="text-align:right;"> 2000 </td>
   <td style="text-align:right;"> 37 </td>
  </tr>
  <tr>
   <td style="text-align:right;"> 2001 </td>
   <td style="text-align:right;"> 54 </td>
  </tr>
  <tr>
   <td style="text-align:right;"> 2002 </td>
   <td style="text-align:right;"> 38 </td>
  </tr>
  <tr>
   <td style="text-align:right;"> 2003 </td>
   <td style="text-align:right;"> 39 </td>
  </tr>
  <tr>
   <td style="text-align:right;"> 2004 </td>
   <td style="text-align:right;"> 34 </td>
  </tr>
  <tr>
   <td style="text-align:right;"> 2005 </td>
   <td style="text-align:right;"> 46 </td>
  </tr>
  <tr>
   <td style="text-align:right;"> 2006 </td>
   <td style="text-align:right;"> 41 </td>
  </tr>
  <tr>
   <td style="text-align:right;"> 2007 </td>
   <td style="text-align:right;"> 44 </td>
  </tr>
  <tr>
   <td style="text-align:right;"> 2008 </td>
   <td style="text-align:right;"> 36 </td>
  </tr>
  <tr>
   <td style="text-align:right;"> 2009 </td>
   <td style="text-align:right;"> 40 </td>
  </tr>
  <tr>
   <td style="text-align:right;"> 2010 </td>
   <td style="text-align:right;"> 41 </td>
  </tr>
  <tr>
   <td style="text-align:right;"> 2011 </td>
   <td style="text-align:right;"> 51 </td>
  </tr>
  <tr>
   <td style="text-align:right;"> 2012 </td>
   <td style="text-align:right;"> 54 </td>
  </tr>
  <tr>
   <td style="text-align:right;"> 2013 </td>
   <td style="text-align:right;"> 34 </td>
  </tr>
  <tr>
   <td style="text-align:right;"> 2014 </td>
   <td style="text-align:right;"> 39 </td>
  </tr>
  <tr>
   <td style="text-align:right;"> 2015 </td>
   <td style="text-align:right;"> 44 </td>
  </tr>
  <tr>
   <td style="text-align:right;"> 2016 </td>
   <td style="text-align:right;"> 33 </td>
  </tr>
  <tr>
   <td style="text-align:right;"> 2017 </td>
   <td style="text-align:right;"> 44 </td>
  </tr>
  <tr>
   <td style="text-align:right;"> 2018 </td>
   <td style="text-align:right;"> 39 </td>
  </tr>
  <tr>
   <td style="text-align:right;"> 2019 </td>
   <td style="text-align:right;"> 37 </td>
  </tr>
</tbody>
</table>

`````

:::
:::


::: callout-note
## Observations

This data shows the distribution of variables across several years.

-   This spread suggests there is a lot of value in interpreting this data by year.

    -   How do different regions compare to each other? What can be said about the rate of progress towards reducing the amount of deaths over the 30 years of data records?

    -   Can environmental factors be considered? N. America has a lot of rich natural resources and mild climates that, among other factors, has enabled its success in terms of providing humanitarian needs to a large population.

        -   areas where temperatures are very cold probably have more insulated homes burning bio fuels on top of cooking with combustion. Can a correlation be made suggesting that, despite economic wealth per capita, IAP caused by combustion is a bigger issue in Northern countries? This would likely only apply to countries that have not reached "western civilization" levels of wealth

-   It seems that the quantity of variables spread across this time span eliminates any hypothesis that would not involve a temporal comparison. If focusing on a single-year comparison becomes useful, it may be valuable to pick a year that contains the most variables, since they are not all the same.
:::


::: {.cell}

```{.r .cell-code}
exploratory_data %>%
  
  rename(n = deaths_cause_all_causes_risk_household_air_pollution_from_solid_fuels_sex_both_age_age_standardized_percent) %>%
  
   mutate("Regions" = countrycode(entity, 
                                       origin = "country.name", 
                                       destination= "region"
                                       )) %>%
  filter(!is.na(Regions)) %>%
  
  group_by(Regions) %>%
  
  ggplot(aes(x= year,y= n, group = 1)) +
  geom_line() +
  facet_wrap(~Regions)+
  geom_smooth()
```

::: {.cell-output .cell-output-stderr}
```
Warning: There was 1 warning in `mutate()`.
ℹ In argument: `Regions = countrycode(entity, origin = "country.name",
  destination = "region")`.
Caused by warning in `countrycode_convert()`:
! Some values were not matched unambiguously: Africa, African Region, African Union, America, Andean Latin America, Asia, Australasia, Caribbean, Central Asia, Central Europe, Central Europe, Eastern Europe, and Central Asia, Central Latin America, Central sub-Saharan Africa, Commonwealth, Commonwealth High Income, Commonwealth Low Income, Commonwealth Middle Income, East Asia, East Asia & Pacific - World Bank region, Eastern Europe, Eastern Mediterranean Region, Eastern sub-Saharan Africa, England, Europe, Europe & Central Asia - World Bank region, European Region, European Union, G20, High-income, High-income Asia Pacific, High-income North America, High-middle SDI, High SDI, Latin America & Caribbean - World Bank region, Low-middle SDI, Low SDI, Micronesia (country), Middle East & North Africa, Middle SDI, Nordic Region, North Africa and Middle East, North America, Northern Ireland, Oceania, OECD Countries, Region of the Americas, Scotland, South-East Asia Region, South Asia - World Bank region, Southeast Asia, Southeast Asia, East Asia, and Oceania, Southern Latin America, Southern sub-Saharan Africa, Sub-Saharan Africa - World Bank region, Timor, Tropical Latin America, Wales, Western Europe, Western Pacific Region, Western sub-Saharan Africa, World, World Bank High Income, World Bank Low Income, World Bank Lower Middle Income, World Bank Upper Middle Income
```
:::

::: {.cell-output .cell-output-stderr}
```
`geom_smooth()` using method = 'loess' and formula = 'y ~ x'
```
:::

::: {.cell-output-display}
![](IndoorPollution_files/figure-html/unnamed-chunk-8-1.png){width=672}
:::
:::


::: callout-note
## Observations

These graphs seem to give some very valuable comparative evidence for both the value of region and time based analysis. Some things that can be seen as trends in this graph would be the downward trend downward that can be seen at the global scale.

-   Europe & Central Asia, Latin America & Caribbean ,Middle East & North Africa and North America were relatively low compared to East Asia & Pacific, South Asia and Sub-Saharan Africa.

-   East Asia & Pacific countries seem to have the greatest reduction in the percent deaths that were measured

-   While this is a global issue, there are clearly countries and regions that are significantly more impacted then others by the loss of life attributed to indoor air pollution and bio fuel combustion
:::

**Potential Hypothesis: Average % deaths decreased globally from 1990 - 2020**

**Potential Hypothesis: Temperate vs. Tropical regions will impact the IAP of developing countries**


::: {.cell}

```{.r .cell-code}
model_data<- exploratory_data %>%
  
  rename(n = deaths_cause_all_causes_risk_household_air_pollution_from_solid_fuels_sex_both_age_age_standardized_percent)
  
model_region_temp <- linear_reg() %>% 
  set_engine("lm")  #construct model instance

model_region_reg<-
  recipe(year~n,
         data = model_data)
  #generate a recipe -- what variables do we have in y = mx+b

model_region<-
  workflow() %>%
  add_model(model_region_temp) %>%
  add_recipe(model_region_reg) #combine the model and recipe to generate a regression analysis

model_region_fit <- model_region %>% fit(model_data)

model_region_fit %>%
  glance() %>%
  kable(digits=c(4,4,2,4,0,0,2,2,2,2,0,0)) %>%
  kable_styling(bootstrap_options = c("hover", "striped"))
```

::: {.cell-output-display}

`````{=html}
<table class="table table-hover table-striped" style="margin-left: auto; margin-right: auto;">
 <thead>
  <tr>
   <th style="text-align:right;"> r.squared </th>
   <th style="text-align:right;"> adj.r.squared </th>
   <th style="text-align:right;"> sigma </th>
   <th style="text-align:right;"> statistic </th>
   <th style="text-align:right;"> p.value </th>
   <th style="text-align:right;"> df </th>
   <th style="text-align:right;"> logLik </th>
   <th style="text-align:right;"> AIC </th>
   <th style="text-align:right;"> BIC </th>
   <th style="text-align:right;"> deviance </th>
   <th style="text-align:right;"> df.residual </th>
   <th style="text-align:right;"> nobs </th>
  </tr>
 </thead>
<tbody>
  <tr>
   <td style="text-align:right;"> 0.026 </td>
   <td style="text-align:right;"> 0.0251 </td>
   <td style="text-align:right;"> 8.36 </td>
   <td style="text-align:right;"> 31.9534 </td>
   <td style="text-align:right;"> 0 </td>
   <td style="text-align:right;"> 1 </td>
   <td style="text-align:right;"> -4254.03 </td>
   <td style="text-align:right;"> 8514.07 </td>
   <td style="text-align:right;"> 8529.34 </td>
   <td style="text-align:right;"> 83883.52 </td>
   <td style="text-align:right;"> 1199 </td>
   <td style="text-align:right;"> 1201 </td>
  </tr>
</tbody>
</table>

`````

:::

```{.r .cell-code}
  # looking to build a regression analysis to determine if a correltion can be seen in the data. prediction is decreasing n over time grouped by region 
```
:::


::: {.callout-note appearance="simple"}
## Observations

This regression analysis provides some useful information that will be valuable to repeat with the entire data set.

The r.squared value: this shows that
:::

## Hypothesis: **Fuel combustion and Indoor Air Pollution**

Considering the data that is present, their are two layers of analysis that could generate useful hypothesis for understanding the global impact of indoor air pollution. Considering the country-specific data, it may be valuable to try and find an additional data set for each layer to use for an additional comparison. These second data sets will be considered in thy hypothesis for each layer.

## Layer One: Regions and Climates

**The correlation that would be most interesting to explore would be between the regional characteristics in terms of climate and the type of winters that are common. Temperate regions are more likely to invest in indoor heating which often involved the use of bio fuels like wood stoves. Poor ventilation is also likely to be a component that will play a role in temperate regions being associated with greater % deaths by IAP.**

-   

## Layer Two: Countries and Resources

**It is likely that this data will show a correlation between countries with low economic resources having significantly higher rates of premature death to exposures that have been removed from developed country households**

-   GDP is likely a strong contributing factor that does not even in tropical regions where ventilation is less of a concern, access to clean fuels is limited, indoor burning of fuels for cooking contributes to close proximity to the source of toxic compounds and heavy metals.

    -   despite the likely correlation between a nations GDP and its percent deaths caused by IAP, it also seems the data will show an overall decrease in the 30yrs in every part of the world, but most noticeable in regions with high average percents.

-   Central America, South America, North Africa, South Asia, and the Pacific Islands are the regions that may not be correlated to climate, but contain significant number of countries with low GDP that contribute to IAP deaths.

::: callout-note
## Future Questions
:::
