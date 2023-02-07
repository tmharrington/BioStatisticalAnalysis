---
title: "Indoor Air Analysis"
author: "Trevor Harrington"
format: html
editor: visual
keep-md: TRUE
prefer-html: true
---



# Analysis of Indoor Air Quality


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
• Dig deeper into tidy modeling with R at https://www.tmwr.org
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
  
kable() %>%
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
   <td style="text-align:left;"> Uruguay </td>
   <td style="text-align:left;"> URY </td>
   <td style="text-align:right;"> 2016 </td>
   <td style="text-align:right;"> 0.1874101 </td>
  </tr>
  <tr>
   <td style="text-align:left;"> Brunei </td>
   <td style="text-align:left;"> BRN </td>
   <td style="text-align:right;"> 1994 </td>
   <td style="text-align:right;"> 0.6344918 </td>
  </tr>
  <tr>
   <td style="text-align:left;"> Low-middle SDI </td>
   <td style="text-align:left;"> NA </td>
   <td style="text-align:right;"> 1994 </td>
   <td style="text-align:right;"> 14.6491448 </td>
  </tr>
  <tr>
   <td style="text-align:left;"> Guyana </td>
   <td style="text-align:left;"> GUY </td>
   <td style="text-align:right;"> 2009 </td>
   <td style="text-align:right;"> 2.0087772 </td>
  </tr>
  <tr>
   <td style="text-align:left;"> Southeast Asia, East Asia, and Oceania </td>
   <td style="text-align:left;"> NA </td>
   <td style="text-align:right;"> 1999 </td>
   <td style="text-align:right;"> 11.4652539 </td>
  </tr>
  <tr>
   <td style="text-align:left;"> Eastern sub-Saharan Africa </td>
   <td style="text-align:left;"> NA </td>
   <td style="text-align:right;"> 1992 </td>
   <td style="text-align:right;"> 12.6657359 </td>
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
  
  kable() %>%
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
   <td style="text-align:right;"> 0.0008376 </td>
  </tr>
  <tr>
   <td style="text-align:left;"> Highest percentage of deaths </td>
   <td style="text-align:right;"> 23.3369531 </td>
  </tr>
  <tr>
   <td style="text-align:left;"> average percentage of deaths </td>
   <td style="text-align:right;"> 5.3095275 </td>
  </tr>
  <tr>
   <td style="text-align:left;"> Standard deviation </td>
   <td style="text-align:right;"> 5.5925460 </td>
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
   <td style="text-align:left;"> Uruguay </td>
   <td style="text-align:left;"> URY </td>
   <td style="text-align:right;"> 2016 </td>
   <td style="text-align:right;"> 0.1874101 </td>
   <td style="text-align:left;"> Latin America &amp; Caribbean </td>
   <td style="text-align:left;"> Americas </td>
  </tr>
  <tr>
   <td style="text-align:left;"> Brunei </td>
   <td style="text-align:left;"> BRN </td>
   <td style="text-align:right;"> 1994 </td>
   <td style="text-align:right;"> 0.6344918 </td>
   <td style="text-align:left;"> East Asia &amp; Pacific </td>
   <td style="text-align:left;"> Asia </td>
  </tr>
  <tr>
   <td style="text-align:left;"> Guyana </td>
   <td style="text-align:left;"> GUY </td>
   <td style="text-align:right;"> 2009 </td>
   <td style="text-align:right;"> 2.0087772 </td>
   <td style="text-align:left;"> Latin America &amp; Caribbean </td>
   <td style="text-align:left;"> Americas </td>
  </tr>
  <tr>
   <td style="text-align:left;"> South Africa </td>
   <td style="text-align:left;"> ZAF </td>
   <td style="text-align:right;"> 2013 </td>
   <td style="text-align:right;"> 1.5466907 </td>
   <td style="text-align:left;"> Sub-Saharan Africa </td>
   <td style="text-align:left;"> Africa </td>
  </tr>
  <tr>
   <td style="text-align:left;"> Finland </td>
   <td style="text-align:left;"> FIN </td>
   <td style="text-align:right;"> 1993 </td>
   <td style="text-align:right;"> 0.0377372 </td>
   <td style="text-align:left;"> Europe &amp; Central Asia </td>
   <td style="text-align:left;"> Europe </td>
  </tr>
  <tr>
   <td style="text-align:left;"> Moldova </td>
   <td style="text-align:left;"> MDA </td>
   <td style="text-align:right;"> 2003 </td>
   <td style="text-align:right;"> 4.4540176 </td>
   <td style="text-align:left;"> Europe &amp; Central Asia </td>
   <td style="text-align:left;"> Europe </td>
  </tr>
  <tr>
   <td style="text-align:left;"> Saint Lucia </td>
   <td style="text-align:left;"> LCA </td>
   <td style="text-align:right;"> 2001 </td>
   <td style="text-align:right;"> 1.8253406 </td>
   <td style="text-align:left;"> Latin America &amp; Caribbean </td>
   <td style="text-align:left;"> Americas </td>
  </tr>
  <tr>
   <td style="text-align:left;"> South Africa </td>
   <td style="text-align:left;"> ZAF </td>
   <td style="text-align:right;"> 2002 </td>
   <td style="text-align:right;"> 2.8743014 </td>
   <td style="text-align:left;"> Sub-Saharan Africa </td>
   <td style="text-align:left;"> Africa </td>
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
   <td style="text-align:left;"> Tokelau </td>
   <td style="text-align:left;"> TKL </td>
   <td style="text-align:right;"> 1990 </td>
   <td style="text-align:right;"> 0.0807632 </td>
   <td style="text-align:left;"> East Asia &amp; Pacific </td>
   <td style="text-align:left;"> Oceania </td>
  </tr>
  <tr>
   <td style="text-align:left;"> Cambodia </td>
   <td style="text-align:left;"> KHM </td>
   <td style="text-align:right;"> 1995 </td>
   <td style="text-align:right;"> 17.5798372 </td>
   <td style="text-align:left;"> East Asia &amp; Pacific </td>
   <td style="text-align:left;"> Asia </td>
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
   <td style="text-align:right;"> 1990 </td>
   <td style="text-align:right;"> 13.9290333 </td>
   <td style="text-align:left;"> Sub-Saharan Africa </td>
   <td style="text-align:left;"> Africa </td>
  </tr>
  <tr>
   <td style="text-align:left;"> France </td>
   <td style="text-align:left;"> FRA </td>
   <td style="text-align:right;"> 2009 </td>
   <td style="text-align:right;"> 0.0105861 </td>
   <td style="text-align:left;"> Europe &amp; Central Asia </td>
   <td style="text-align:left;"> Europe </td>
  </tr>
  <tr>
   <td style="text-align:left;"> Guyana </td>
   <td style="text-align:left;"> GUY </td>
   <td style="text-align:right;"> 2013 </td>
   <td style="text-align:right;"> 1.3780406 </td>
   <td style="text-align:left;"> Latin America &amp; Caribbean </td>
   <td style="text-align:left;"> Americas </td>
  </tr>
  <tr>
   <td style="text-align:left;"> Pakistan </td>
   <td style="text-align:left;"> PAK </td>
   <td style="text-align:right;"> 2013 </td>
   <td style="text-align:right;"> 10.0094442 </td>
   <td style="text-align:left;"> South Asia </td>
   <td style="text-align:left;"> Asia </td>
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
   <td style="text-align:left;"> Kiribati </td>
   <td style="text-align:left;"> KIR </td>
   <td style="text-align:right;"> 2008 </td>
   <td style="text-align:right;"> 13.1474441 </td>
   <td style="text-align:left;"> East Asia &amp; Pacific </td>
   <td style="text-align:left;"> Oceania </td>
  </tr>
  <tr>
   <td style="text-align:left;"> Slovenia </td>
   <td style="text-align:left;"> SVN </td>
   <td style="text-align:right;"> 2004 </td>
   <td style="text-align:right;"> 0.5951827 </td>
   <td style="text-align:left;"> Europe &amp; Central Asia </td>
   <td style="text-align:left;"> Europe </td>
  </tr>
  <tr>
   <td style="text-align:left;"> Venezuela </td>
   <td style="text-align:left;"> VEN </td>
   <td style="text-align:right;"> 1991 </td>
   <td style="text-align:right;"> 0.4370129 </td>
   <td style="text-align:left;"> Latin America &amp; Caribbean </td>
   <td style="text-align:left;"> Americas </td>
  </tr>
  <tr>
   <td style="text-align:left;"> Peru </td>
   <td style="text-align:left;"> PER </td>
   <td style="text-align:right;"> 1996 </td>
   <td style="text-align:right;"> 5.3813163 </td>
   <td style="text-align:left;"> Latin America &amp; Caribbean </td>
   <td style="text-align:left;"> Americas </td>
  </tr>
  <tr>
   <td style="text-align:left;"> North Korea </td>
   <td style="text-align:left;"> PRK </td>
   <td style="text-align:right;"> 1994 </td>
   <td style="text-align:right;"> 20.1267041 </td>
   <td style="text-align:left;"> East Asia &amp; Pacific </td>
   <td style="text-align:left;"> Asia </td>
  </tr>
  <tr>
   <td style="text-align:left;"> Niger </td>
   <td style="text-align:left;"> NER </td>
   <td style="text-align:right;"> 2015 </td>
   <td style="text-align:right;"> 15.5552787 </td>
   <td style="text-align:left;"> Sub-Saharan Africa </td>
   <td style="text-align:left;"> Africa </td>
  </tr>
  <tr>
   <td style="text-align:left;"> Yemen </td>
   <td style="text-align:left;"> YEM </td>
   <td style="text-align:right;"> 1990 </td>
   <td style="text-align:right;"> 17.1926092 </td>
   <td style="text-align:left;"> Middle East &amp; North Africa </td>
   <td style="text-align:left;"> Asia </td>
  </tr>
  <tr>
   <td style="text-align:left;"> Bolivia </td>
   <td style="text-align:left;"> BOL </td>
   <td style="text-align:right;"> 2017 </td>
   <td style="text-align:right;"> 3.4625731 </td>
   <td style="text-align:left;"> Latin America &amp; Caribbean </td>
   <td style="text-align:left;"> Americas </td>
  </tr>
  <tr>
   <td style="text-align:left;"> Kazakhstan </td>
   <td style="text-align:left;"> KAZ </td>
   <td style="text-align:right;"> 1996 </td>
   <td style="text-align:right;"> 4.7108138 </td>
   <td style="text-align:left;"> Europe &amp; Central Asia </td>
   <td style="text-align:left;"> Asia </td>
  </tr>
  <tr>
   <td style="text-align:left;"> Libya </td>
   <td style="text-align:left;"> LBY </td>
   <td style="text-align:right;"> 1998 </td>
   <td style="text-align:right;"> 0.7039047 </td>
   <td style="text-align:left;"> Middle East &amp; North Africa </td>
   <td style="text-align:left;"> Africa </td>
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
   <td style="text-align:left;"> South Sudan </td>
   <td style="text-align:left;"> SSD </td>
   <td style="text-align:right;"> 2001 </td>
   <td style="text-align:right;"> 11.6753459 </td>
   <td style="text-align:left;"> Sub-Saharan Africa </td>
   <td style="text-align:left;"> Africa </td>
  </tr>
  <tr>
   <td style="text-align:left;"> Democratic Republic of Congo </td>
   <td style="text-align:left;"> COD </td>
   <td style="text-align:right;"> 2015 </td>
   <td style="text-align:right;"> 13.3026446 </td>
   <td style="text-align:left;"> Sub-Saharan Africa </td>
   <td style="text-align:left;"> Africa </td>
  </tr>
  <tr>
   <td style="text-align:left;"> Lithuania </td>
   <td style="text-align:left;"> LTU </td>
   <td style="text-align:right;"> 1997 </td>
   <td style="text-align:right;"> 0.7278800 </td>
   <td style="text-align:left;"> Europe &amp; Central Asia </td>
   <td style="text-align:left;"> Europe </td>
  </tr>
  <tr>
   <td style="text-align:left;"> Mozambique </td>
   <td style="text-align:left;"> MOZ </td>
   <td style="text-align:right;"> 2018 </td>
   <td style="text-align:right;"> 11.6465602 </td>
   <td style="text-align:left;"> Sub-Saharan Africa </td>
   <td style="text-align:left;"> Africa </td>
  </tr>
  <tr>
   <td style="text-align:left;"> Dominica </td>
   <td style="text-align:left;"> DMA </td>
   <td style="text-align:right;"> 2016 </td>
   <td style="text-align:right;"> 0.6283808 </td>
   <td style="text-align:left;"> Latin America &amp; Caribbean </td>
   <td style="text-align:left;"> Americas </td>
  </tr>
  <tr>
   <td style="text-align:left;"> Zambia </td>
   <td style="text-align:left;"> ZMB </td>
   <td style="text-align:right;"> 2010 </td>
   <td style="text-align:right;"> 9.0378618 </td>
   <td style="text-align:left;"> Sub-Saharan Africa </td>
   <td style="text-align:left;"> Africa </td>
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
   <td style="text-align:left;"> Laos </td>
   <td style="text-align:left;"> LAO </td>
   <td style="text-align:right;"> 2001 </td>
   <td style="text-align:right;"> 17.6510981 </td>
   <td style="text-align:left;"> East Asia &amp; Pacific </td>
   <td style="text-align:left;"> Asia </td>
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
   <td style="text-align:left;"> Ireland </td>
   <td style="text-align:left;"> IRL </td>
   <td style="text-align:right;"> 2012 </td>
   <td style="text-align:right;"> 0.0099438 </td>
   <td style="text-align:left;"> Europe &amp; Central Asia </td>
   <td style="text-align:left;"> Europe </td>
  </tr>
  <tr>
   <td style="text-align:left;"> Malta </td>
   <td style="text-align:left;"> MLT </td>
   <td style="text-align:right;"> 2003 </td>
   <td style="text-align:right;"> 0.0419429 </td>
   <td style="text-align:left;"> Middle East &amp; North Africa </td>
   <td style="text-align:left;"> Europe </td>
  </tr>
  <tr>
   <td style="text-align:left;"> Samoa </td>
   <td style="text-align:left;"> WSM </td>
   <td style="text-align:right;"> 2013 </td>
   <td style="text-align:right;"> 12.5066049 </td>
   <td style="text-align:left;"> East Asia &amp; Pacific </td>
   <td style="text-align:left;"> Oceania </td>
  </tr>
  <tr>
   <td style="text-align:left;"> Niger </td>
   <td style="text-align:left;"> NER </td>
   <td style="text-align:right;"> 1997 </td>
   <td style="text-align:right;"> 15.0844004 </td>
   <td style="text-align:left;"> Sub-Saharan Africa </td>
   <td style="text-align:left;"> Africa </td>
  </tr>
  <tr>
   <td style="text-align:left;"> Antigua and Barbuda </td>
   <td style="text-align:left;"> ATG </td>
   <td style="text-align:right;"> 1995 </td>
   <td style="text-align:right;"> 0.4163425 </td>
   <td style="text-align:left;"> Latin America &amp; Caribbean </td>
   <td style="text-align:left;"> Americas </td>
  </tr>
  <tr>
   <td style="text-align:left;"> Japan </td>
   <td style="text-align:left;"> JPN </td>
   <td style="text-align:right;"> 2003 </td>
   <td style="text-align:right;"> 0.0172335 </td>
   <td style="text-align:left;"> East Asia &amp; Pacific </td>
   <td style="text-align:left;"> Asia </td>
  </tr>
  <tr>
   <td style="text-align:left;"> Taiwan </td>
   <td style="text-align:left;"> TWN </td>
   <td style="text-align:right;"> 1992 </td>
   <td style="text-align:right;"> 2.7139282 </td>
   <td style="text-align:left;"> East Asia &amp; Pacific </td>
   <td style="text-align:left;"> Asia </td>
  </tr>
  <tr>
   <td style="text-align:left;"> Kuwait </td>
   <td style="text-align:left;"> KWT </td>
   <td style="text-align:right;"> 1992 </td>
   <td style="text-align:right;"> 0.2000318 </td>
   <td style="text-align:left;"> Middle East &amp; North Africa </td>
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
   <td style="text-align:left;"> Syria </td>
   <td style="text-align:left;"> SYR </td>
   <td style="text-align:right;"> 2016 </td>
   <td style="text-align:right;"> 0.0211907 </td>
   <td style="text-align:left;"> Middle East &amp; North Africa </td>
   <td style="text-align:left;"> Asia </td>
  </tr>
  <tr>
   <td style="text-align:left;"> Andorra </td>
   <td style="text-align:left;"> AND </td>
   <td style="text-align:right;"> 2003 </td>
   <td style="text-align:right;"> 0.0084011 </td>
   <td style="text-align:left;"> Europe &amp; Central Asia </td>
   <td style="text-align:left;"> Europe </td>
  </tr>
  <tr>
   <td style="text-align:left;"> Slovakia </td>
   <td style="text-align:left;"> SVK </td>
   <td style="text-align:right;"> 2008 </td>
   <td style="text-align:right;"> 0.1085565 </td>
   <td style="text-align:left;"> Europe &amp; Central Asia </td>
   <td style="text-align:left;"> Europe </td>
  </tr>
  <tr>
   <td style="text-align:left;"> Rwanda </td>
   <td style="text-align:left;"> RWA </td>
   <td style="text-align:right;"> 1994 </td>
   <td style="text-align:right;"> 2.4585404 </td>
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
   <td style="text-align:left;"> Philippines </td>
   <td style="text-align:left;"> PHL </td>
   <td style="text-align:right;"> 1998 </td>
   <td style="text-align:right;"> 8.2767052 </td>
   <td style="text-align:left;"> East Asia &amp; Pacific </td>
   <td style="text-align:left;"> Asia </td>
  </tr>
  <tr>
   <td style="text-align:left;"> Lesotho </td>
   <td style="text-align:left;"> LSO </td>
   <td style="text-align:right;"> 2004 </td>
   <td style="text-align:right;"> 6.3595093 </td>
   <td style="text-align:left;"> Sub-Saharan Africa </td>
   <td style="text-align:left;"> Africa </td>
  </tr>
  <tr>
   <td style="text-align:left;"> Russia </td>
   <td style="text-align:left;"> RUS </td>
   <td style="text-align:right;"> 1991 </td>
   <td style="text-align:right;"> 0.7715719 </td>
   <td style="text-align:left;"> Europe &amp; Central Asia </td>
   <td style="text-align:left;"> Europe </td>
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
   <td style="text-align:left;"> Suriname </td>
   <td style="text-align:left;"> SUR </td>
   <td style="text-align:right;"> 2004 </td>
   <td style="text-align:right;"> 2.6686830 </td>
   <td style="text-align:left;"> Latin America &amp; Caribbean </td>
   <td style="text-align:left;"> Americas </td>
  </tr>
  <tr>
   <td style="text-align:left;"> Ukraine </td>
   <td style="text-align:left;"> UKR </td>
   <td style="text-align:right;"> 2010 </td>
   <td style="text-align:right;"> 0.5619895 </td>
   <td style="text-align:left;"> Europe &amp; Central Asia </td>
   <td style="text-align:left;"> Europe </td>
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
   <td style="text-align:left;"> Austria </td>
   <td style="text-align:left;"> AUT </td>
   <td style="text-align:right;"> 1996 </td>
   <td style="text-align:right;"> 0.0426625 </td>
   <td style="text-align:left;"> Europe &amp; Central Asia </td>
   <td style="text-align:left;"> Europe </td>
  </tr>
  <tr>
   <td style="text-align:left;"> France </td>
   <td style="text-align:left;"> FRA </td>
   <td style="text-align:right;"> 2010 </td>
   <td style="text-align:right;"> 0.0098937 </td>
   <td style="text-align:left;"> Europe &amp; Central Asia </td>
   <td style="text-align:left;"> Europe </td>
  </tr>
  <tr>
   <td style="text-align:left;"> El Salvador </td>
   <td style="text-align:left;"> SLV </td>
   <td style="text-align:right;"> 2010 </td>
   <td style="text-align:right;"> 4.0312574 </td>
   <td style="text-align:left;"> Latin America &amp; Caribbean </td>
   <td style="text-align:left;"> Americas </td>
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
   <td style="text-align:left;"> Nicaragua </td>
   <td style="text-align:left;"> NIC </td>
   <td style="text-align:right;"> 2010 </td>
   <td style="text-align:right;"> 8.5510065 </td>
   <td style="text-align:left;"> Latin America &amp; Caribbean </td>
   <td style="text-align:left;"> Americas </td>
  </tr>
  <tr>
   <td style="text-align:left;"> Georgia </td>
   <td style="text-align:left;"> GEO </td>
   <td style="text-align:right;"> 1995 </td>
   <td style="text-align:right;"> 7.6385228 </td>
   <td style="text-align:left;"> Europe &amp; Central Asia </td>
   <td style="text-align:left;"> Asia </td>
  </tr>
  <tr>
   <td style="text-align:left;"> Angola </td>
   <td style="text-align:left;"> AGO </td>
   <td style="text-align:right;"> 1992 </td>
   <td style="text-align:right;"> 12.4778119 </td>
   <td style="text-align:left;"> Sub-Saharan Africa </td>
   <td style="text-align:left;"> Africa </td>
  </tr>
  <tr>
   <td style="text-align:left;"> Chile </td>
   <td style="text-align:left;"> CHL </td>
   <td style="text-align:right;"> 1991 </td>
   <td style="text-align:right;"> 2.6116421 </td>
   <td style="text-align:left;"> Latin America &amp; Caribbean </td>
   <td style="text-align:left;"> Americas </td>
  </tr>
  <tr>
   <td style="text-align:left;"> Cameroon </td>
   <td style="text-align:left;"> CMR </td>
   <td style="text-align:right;"> 2019 </td>
   <td style="text-align:right;"> 6.7310895 </td>
   <td style="text-align:left;"> Sub-Saharan Africa </td>
   <td style="text-align:left;"> Africa </td>
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
   <td style="text-align:left;"> India </td>
   <td style="text-align:left;"> IND </td>
   <td style="text-align:right;"> 2001 </td>
   <td style="text-align:right;"> 12.1590567 </td>
   <td style="text-align:left;"> South Asia </td>
   <td style="text-align:left;"> Asia </td>
  </tr>
  <tr>
   <td style="text-align:left;"> Vietnam </td>
   <td style="text-align:left;"> VNM </td>
   <td style="text-align:right;"> 2009 </td>
   <td style="text-align:right;"> 8.5942047 </td>
   <td style="text-align:left;"> East Asia &amp; Pacific </td>
   <td style="text-align:left;"> Asia </td>
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
   <td style="text-align:left;"> Laos </td>
   <td style="text-align:left;"> LAO </td>
   <td style="text-align:right;"> 1996 </td>
   <td style="text-align:right;"> 17.5392019 </td>
   <td style="text-align:left;"> East Asia &amp; Pacific </td>
   <td style="text-align:left;"> Asia </td>
  </tr>
  <tr>
   <td style="text-align:left;"> Togo </td>
   <td style="text-align:left;"> TGO </td>
   <td style="text-align:right;"> 2010 </td>
   <td style="text-align:right;"> 11.6769081 </td>
   <td style="text-align:left;"> Sub-Saharan Africa </td>
   <td style="text-align:left;"> Africa </td>
  </tr>
  <tr>
   <td style="text-align:left;"> Belarus </td>
   <td style="text-align:left;"> BLR </td>
   <td style="text-align:right;"> 2007 </td>
   <td style="text-align:right;"> 0.2548650 </td>
   <td style="text-align:left;"> Europe &amp; Central Asia </td>
   <td style="text-align:left;"> Europe </td>
  </tr>
  <tr>
   <td style="text-align:left;"> Gabon </td>
   <td style="text-align:left;"> GAB </td>
   <td style="text-align:right;"> 2011 </td>
   <td style="text-align:right;"> 1.7681053 </td>
   <td style="text-align:left;"> Sub-Saharan Africa </td>
   <td style="text-align:left;"> Africa </td>
  </tr>
  <tr>
   <td style="text-align:left;"> United States </td>
   <td style="text-align:left;"> USA </td>
   <td style="text-align:right;"> 1993 </td>
   <td style="text-align:right;"> 0.0150604 </td>
   <td style="text-align:left;"> North America </td>
   <td style="text-align:left;"> Americas </td>
  </tr>
  <tr>
   <td style="text-align:left;"> Moldova </td>
   <td style="text-align:left;"> MDA </td>
   <td style="text-align:right;"> 2011 </td>
   <td style="text-align:right;"> 1.2163504 </td>
   <td style="text-align:left;"> Europe &amp; Central Asia </td>
   <td style="text-align:left;"> Europe </td>
  </tr>
  <tr>
   <td style="text-align:left;"> United Kingdom </td>
   <td style="text-align:left;"> GBR </td>
   <td style="text-align:right;"> 1997 </td>
   <td style="text-align:right;"> 0.0073519 </td>
   <td style="text-align:left;"> Europe &amp; Central Asia </td>
   <td style="text-align:left;"> Europe </td>
  </tr>
  <tr>
   <td style="text-align:left;"> Iran </td>
   <td style="text-align:left;"> IRN </td>
   <td style="text-align:right;"> 1991 </td>
   <td style="text-align:right;"> 1.8526842 </td>
   <td style="text-align:left;"> Middle East &amp; North Africa </td>
   <td style="text-align:left;"> Asia </td>
  </tr>
  <tr>
   <td style="text-align:left;"> Palau </td>
   <td style="text-align:left;"> PLW </td>
   <td style="text-align:right;"> 1991 </td>
   <td style="text-align:right;"> 0.0151167 </td>
   <td style="text-align:left;"> East Asia &amp; Pacific </td>
   <td style="text-align:left;"> Oceania </td>
  </tr>
  <tr>
   <td style="text-align:left;"> Monaco </td>
   <td style="text-align:left;"> MCO </td>
   <td style="text-align:right;"> 1994 </td>
   <td style="text-align:right;"> 0.0081788 </td>
   <td style="text-align:left;"> Europe &amp; Central Asia </td>
   <td style="text-align:left;"> Europe </td>
  </tr>
  <tr>
   <td style="text-align:left;"> Austria </td>
   <td style="text-align:left;"> AUT </td>
   <td style="text-align:right;"> 2003 </td>
   <td style="text-align:right;"> 0.0273848 </td>
   <td style="text-align:left;"> Europe &amp; Central Asia </td>
   <td style="text-align:left;"> Europe </td>
  </tr>
  <tr>
   <td style="text-align:left;"> Tunisia </td>
   <td style="text-align:left;"> TUN </td>
   <td style="text-align:right;"> 1997 </td>
   <td style="text-align:right;"> 1.1401337 </td>
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
   <td style="text-align:left;"> Bolivia </td>
   <td style="text-align:left;"> BOL </td>
   <td style="text-align:right;"> 2003 </td>
   <td style="text-align:right;"> 5.5213086 </td>
   <td style="text-align:left;"> Latin America &amp; Caribbean </td>
   <td style="text-align:left;"> Americas </td>
  </tr>
  <tr>
   <td style="text-align:left;"> Barbados </td>
   <td style="text-align:left;"> BRB </td>
   <td style="text-align:right;"> 2017 </td>
   <td style="text-align:right;"> 0.0073003 </td>
   <td style="text-align:left;"> Latin America &amp; Caribbean </td>
   <td style="text-align:left;"> Americas </td>
  </tr>
  <tr>
   <td style="text-align:left;"> Iceland </td>
   <td style="text-align:left;"> ISL </td>
   <td style="text-align:right;"> 1996 </td>
   <td style="text-align:right;"> 0.0211944 </td>
   <td style="text-align:left;"> Europe &amp; Central Asia </td>
   <td style="text-align:left;"> Europe </td>
  </tr>
  <tr>
   <td style="text-align:left;"> Jordan </td>
   <td style="text-align:left;"> JOR </td>
   <td style="text-align:right;"> 2005 </td>
   <td style="text-align:right;"> 0.0273923 </td>
   <td style="text-align:left;"> Middle East &amp; North Africa </td>
   <td style="text-align:left;"> Asia </td>
  </tr>
  <tr>
   <td style="text-align:left;"> Equatorial Guinea </td>
   <td style="text-align:left;"> GNQ </td>
   <td style="text-align:right;"> 2010 </td>
   <td style="text-align:right;"> 4.3319100 </td>
   <td style="text-align:left;"> Sub-Saharan Africa </td>
   <td style="text-align:left;"> Africa </td>
  </tr>
  <tr>
   <td style="text-align:left;"> Luxembourg </td>
   <td style="text-align:left;"> LUX </td>
   <td style="text-align:right;"> 2007 </td>
   <td style="text-align:right;"> 0.0113734 </td>
   <td style="text-align:left;"> Europe &amp; Central Asia </td>
   <td style="text-align:left;"> Europe </td>
  </tr>
  <tr>
   <td style="text-align:left;"> El Salvador </td>
   <td style="text-align:left;"> SLV </td>
   <td style="text-align:right;"> 2009 </td>
   <td style="text-align:right;"> 4.2028322 </td>
   <td style="text-align:left;"> Latin America &amp; Caribbean </td>
   <td style="text-align:left;"> Americas </td>
  </tr>
  <tr>
   <td style="text-align:left;"> Dominican Republic </td>
   <td style="text-align:left;"> DOM </td>
   <td style="text-align:right;"> 2016 </td>
   <td style="text-align:right;"> 1.6818169 </td>
   <td style="text-align:left;"> Latin America &amp; Caribbean </td>
   <td style="text-align:left;"> Americas </td>
  </tr>
  <tr>
   <td style="text-align:left;"> Italy </td>
   <td style="text-align:left;"> ITA </td>
   <td style="text-align:right;"> 2004 </td>
   <td style="text-align:right;"> 0.0464116 </td>
   <td style="text-align:left;"> Europe &amp; Central Asia </td>
   <td style="text-align:left;"> Europe </td>
  </tr>
  <tr>
   <td style="text-align:left;"> El Salvador </td>
   <td style="text-align:left;"> SLV </td>
   <td style="text-align:right;"> 1997 </td>
   <td style="text-align:right;"> 7.6670209 </td>
   <td style="text-align:left;"> Latin America &amp; Caribbean </td>
   <td style="text-align:left;"> Americas </td>
  </tr>
  <tr>
   <td style="text-align:left;"> Iraq </td>
   <td style="text-align:left;"> IRQ </td>
   <td style="text-align:right;"> 1999 </td>
   <td style="text-align:right;"> 2.1843656 </td>
   <td style="text-align:left;"> Middle East &amp; North Africa </td>
   <td style="text-align:left;"> Asia </td>
  </tr>
  <tr>
   <td style="text-align:left;"> North Macedonia </td>
   <td style="text-align:left;"> MKD </td>
   <td style="text-align:right;"> 2009 </td>
   <td style="text-align:right;"> 3.1225053 </td>
   <td style="text-align:left;"> Europe &amp; Central Asia </td>
   <td style="text-align:left;"> Europe </td>
  </tr>
  <tr>
   <td style="text-align:left;"> Pakistan </td>
   <td style="text-align:left;"> PAK </td>
   <td style="text-align:right;"> 2009 </td>
   <td style="text-align:right;"> 11.4512901 </td>
   <td style="text-align:left;"> South Asia </td>
   <td style="text-align:left;"> Asia </td>
  </tr>
  <tr>
   <td style="text-align:left;"> Portugal </td>
   <td style="text-align:left;"> PRT </td>
   <td style="text-align:right;"> 2006 </td>
   <td style="text-align:right;"> 0.1071355 </td>
   <td style="text-align:left;"> Europe &amp; Central Asia </td>
   <td style="text-align:left;"> Europe </td>
  </tr>
  <tr>
   <td style="text-align:left;"> Niger </td>
   <td style="text-align:left;"> NER </td>
   <td style="text-align:right;"> 2001 </td>
   <td style="text-align:right;"> 14.9109364 </td>
   <td style="text-align:left;"> Sub-Saharan Africa </td>
   <td style="text-align:left;"> Africa </td>
  </tr>
  <tr>
   <td style="text-align:left;"> Djibouti </td>
   <td style="text-align:left;"> DJI </td>
   <td style="text-align:right;"> 2019 </td>
   <td style="text-align:right;"> 2.9379829 </td>
   <td style="text-align:left;"> Middle East &amp; North Africa </td>
   <td style="text-align:left;"> Africa </td>
  </tr>
  <tr>
   <td style="text-align:left;"> Equatorial Guinea </td>
   <td style="text-align:left;"> GNQ </td>
   <td style="text-align:right;"> 2002 </td>
   <td style="text-align:right;"> 9.1047556 </td>
   <td style="text-align:left;"> Sub-Saharan Africa </td>
   <td style="text-align:left;"> Africa </td>
  </tr>
  <tr>
   <td style="text-align:left;"> South Sudan </td>
   <td style="text-align:left;"> SSD </td>
   <td style="text-align:right;"> 1996 </td>
   <td style="text-align:right;"> 12.8032278 </td>
   <td style="text-align:left;"> Sub-Saharan Africa </td>
   <td style="text-align:left;"> Africa </td>
  </tr>
  <tr>
   <td style="text-align:left;"> Sweden </td>
   <td style="text-align:left;"> SWE </td>
   <td style="text-align:right;"> 2007 </td>
   <td style="text-align:right;"> 0.0081032 </td>
   <td style="text-align:left;"> Europe &amp; Central Asia </td>
   <td style="text-align:left;"> Europe </td>
  </tr>
  <tr>
   <td style="text-align:left;"> Greece </td>
   <td style="text-align:left;"> GRC </td>
   <td style="text-align:right;"> 2013 </td>
   <td style="text-align:right;"> 0.0431520 </td>
   <td style="text-align:left;"> Europe &amp; Central Asia </td>
   <td style="text-align:left;"> Europe </td>
  </tr>
  <tr>
   <td style="text-align:left;"> Mauritius </td>
   <td style="text-align:left;"> MUS </td>
   <td style="text-align:right;"> 2008 </td>
   <td style="text-align:right;"> 0.3845047 </td>
   <td style="text-align:left;"> Sub-Saharan Africa </td>
   <td style="text-align:left;"> Africa </td>
  </tr>
  <tr>
   <td style="text-align:left;"> Zimbabwe </td>
   <td style="text-align:left;"> ZWE </td>
   <td style="text-align:right;"> 2004 </td>
   <td style="text-align:right;"> 5.3420462 </td>
   <td style="text-align:left;"> Sub-Saharan Africa </td>
   <td style="text-align:left;"> Africa </td>
  </tr>
  <tr>
   <td style="text-align:left;"> Bermuda </td>
   <td style="text-align:left;"> BMU </td>
   <td style="text-align:right;"> 1997 </td>
   <td style="text-align:right;"> 0.7363057 </td>
   <td style="text-align:left;"> North America </td>
   <td style="text-align:left;"> Americas </td>
  </tr>
  <tr>
   <td style="text-align:left;"> Niue </td>
   <td style="text-align:left;"> NIU </td>
   <td style="text-align:right;"> 2007 </td>
   <td style="text-align:right;"> 1.2649687 </td>
   <td style="text-align:left;"> East Asia &amp; Pacific </td>
   <td style="text-align:left;"> Oceania </td>
  </tr>
  <tr>
   <td style="text-align:left;"> Bulgaria </td>
   <td style="text-align:left;"> BGR </td>
   <td style="text-align:right;"> 2004 </td>
   <td style="text-align:right;"> 2.2614750 </td>
   <td style="text-align:left;"> Europe &amp; Central Asia </td>
   <td style="text-align:left;"> Europe </td>
  </tr>
  <tr>
   <td style="text-align:left;"> Chad </td>
   <td style="text-align:left;"> TCD </td>
   <td style="text-align:right;"> 1992 </td>
   <td style="text-align:right;"> 14.9047169 </td>
   <td style="text-align:left;"> Sub-Saharan Africa </td>
   <td style="text-align:left;"> Africa </td>
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
   <td style="text-align:left;"> Azerbaijan </td>
   <td style="text-align:left;"> AZE </td>
   <td style="text-align:right;"> 1994 </td>
   <td style="text-align:right;"> 6.7318379 </td>
   <td style="text-align:left;"> Europe &amp; Central Asia </td>
   <td style="text-align:left;"> Asia </td>
  </tr>
  <tr>
   <td style="text-align:left;"> Romania </td>
   <td style="text-align:left;"> ROU </td>
   <td style="text-align:right;"> 2017 </td>
   <td style="text-align:right;"> 0.9851874 </td>
   <td style="text-align:left;"> Europe &amp; Central Asia </td>
   <td style="text-align:left;"> Europe </td>
  </tr>
  <tr>
   <td style="text-align:left;"> Rwanda </td>
   <td style="text-align:left;"> RWA </td>
   <td style="text-align:right;"> 1995 </td>
   <td style="text-align:right;"> 11.6179500 </td>
   <td style="text-align:left;"> Sub-Saharan Africa </td>
   <td style="text-align:left;"> Africa </td>
  </tr>
  <tr>
   <td style="text-align:left;"> Saint Lucia </td>
   <td style="text-align:left;"> LCA </td>
   <td style="text-align:right;"> 1990 </td>
   <td style="text-align:right;"> 5.5899891 </td>
   <td style="text-align:left;"> Latin America &amp; Caribbean </td>
   <td style="text-align:left;"> Americas </td>
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
   <td style="text-align:left;"> New Zealand </td>
   <td style="text-align:left;"> NZL </td>
   <td style="text-align:right;"> 2018 </td>
   <td style="text-align:right;"> 0.0078025 </td>
   <td style="text-align:left;"> East Asia &amp; Pacific </td>
   <td style="text-align:left;"> Oceania </td>
  </tr>
  <tr>
   <td style="text-align:left;"> Antigua and Barbuda </td>
   <td style="text-align:left;"> ATG </td>
   <td style="text-align:right;"> 1992 </td>
   <td style="text-align:right;"> 0.5393477 </td>
   <td style="text-align:left;"> Latin America &amp; Caribbean </td>
   <td style="text-align:left;"> Americas </td>
  </tr>
  <tr>
   <td style="text-align:left;"> Madagascar </td>
   <td style="text-align:left;"> MDG </td>
   <td style="text-align:right;"> 2005 </td>
   <td style="text-align:right;"> 15.9763576 </td>
   <td style="text-align:left;"> Sub-Saharan Africa </td>
   <td style="text-align:left;"> Africa </td>
  </tr>
  <tr>
   <td style="text-align:left;"> Romania </td>
   <td style="text-align:left;"> ROU </td>
   <td style="text-align:right;"> 2013 </td>
   <td style="text-align:right;"> 1.2031370 </td>
   <td style="text-align:left;"> Europe &amp; Central Asia </td>
   <td style="text-align:left;"> Europe </td>
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
   <td style="text-align:left;"> Slovenia </td>
   <td style="text-align:left;"> SVN </td>
   <td style="text-align:right;"> 2016 </td>
   <td style="text-align:right;"> 0.3347933 </td>
   <td style="text-align:left;"> Europe &amp; Central Asia </td>
   <td style="text-align:left;"> Europe </td>
  </tr>
  <tr>
   <td style="text-align:left;"> Rwanda </td>
   <td style="text-align:left;"> RWA </td>
   <td style="text-align:right;"> 1998 </td>
   <td style="text-align:right;"> 11.4825911 </td>
   <td style="text-align:left;"> Sub-Saharan Africa </td>
   <td style="text-align:left;"> Africa </td>
  </tr>
  <tr>
   <td style="text-align:left;"> Russia </td>
   <td style="text-align:left;"> RUS </td>
   <td style="text-align:right;"> 2002 </td>
   <td style="text-align:right;"> 0.4780448 </td>
   <td style="text-align:left;"> Europe &amp; Central Asia </td>
   <td style="text-align:left;"> Europe </td>
  </tr>
  <tr>
   <td style="text-align:left;"> Kyrgyzstan </td>
   <td style="text-align:left;"> KGZ </td>
   <td style="text-align:right;"> 1998 </td>
   <td style="text-align:right;"> 9.8013987 </td>
   <td style="text-align:left;"> Europe &amp; Central Asia </td>
   <td style="text-align:left;"> Asia </td>
  </tr>
  <tr>
   <td style="text-align:left;"> Jamaica </td>
   <td style="text-align:left;"> JAM </td>
   <td style="text-align:right;"> 2011 </td>
   <td style="text-align:right;"> 1.6109776 </td>
   <td style="text-align:left;"> Latin America &amp; Caribbean </td>
   <td style="text-align:left;"> Americas </td>
  </tr>
  <tr>
   <td style="text-align:left;"> Bulgaria </td>
   <td style="text-align:left;"> BGR </td>
   <td style="text-align:right;"> 2010 </td>
   <td style="text-align:right;"> 1.8176558 </td>
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
   <td style="text-align:left;"> Germany </td>
   <td style="text-align:left;"> DEU </td>
   <td style="text-align:right;"> 2002 </td>
   <td style="text-align:right;"> 0.0073218 </td>
   <td style="text-align:left;"> Europe &amp; Central Asia </td>
   <td style="text-align:left;"> Europe </td>
  </tr>
  <tr>
   <td style="text-align:left;"> Montenegro </td>
   <td style="text-align:left;"> MNE </td>
   <td style="text-align:right;"> 2018 </td>
   <td style="text-align:right;"> 2.4587196 </td>
   <td style="text-align:left;"> Europe &amp; Central Asia </td>
   <td style="text-align:left;"> Europe </td>
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
   <td style="text-align:left;"> Nauru </td>
   <td style="text-align:left;"> NRU </td>
   <td style="text-align:right;"> 2019 </td>
   <td style="text-align:right;"> 0.5921484 </td>
   <td style="text-align:left;"> East Asia &amp; Pacific </td>
   <td style="text-align:left;"> Oceania </td>
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
   <td style="text-align:left;"> Guam </td>
   <td style="text-align:left;"> GUM </td>
   <td style="text-align:right;"> 2000 </td>
   <td style="text-align:right;"> 0.4464930 </td>
   <td style="text-align:left;"> East Asia &amp; Pacific </td>
   <td style="text-align:left;"> Oceania </td>
  </tr>
  <tr>
   <td style="text-align:left;"> Dominican Republic </td>
   <td style="text-align:left;"> DOM </td>
   <td style="text-align:right;"> 1997 </td>
   <td style="text-align:right;"> 5.5065043 </td>
   <td style="text-align:left;"> Latin America &amp; Caribbean </td>
   <td style="text-align:left;"> Americas </td>
  </tr>
  <tr>
   <td style="text-align:left;"> Dominican Republic </td>
   <td style="text-align:left;"> DOM </td>
   <td style="text-align:right;"> 2018 </td>
   <td style="text-align:right;"> 1.4393115 </td>
   <td style="text-align:left;"> Latin America &amp; Caribbean </td>
   <td style="text-align:left;"> Americas </td>
  </tr>
  <tr>
   <td style="text-align:left;"> Saint Kitts and Nevis </td>
   <td style="text-align:left;"> KNA </td>
   <td style="text-align:right;"> 2005 </td>
   <td style="text-align:right;"> 0.5341922 </td>
   <td style="text-align:left;"> Latin America &amp; Caribbean </td>
   <td style="text-align:left;"> Americas </td>
  </tr>
  <tr>
   <td style="text-align:left;"> Azerbaijan </td>
   <td style="text-align:left;"> AZE </td>
   <td style="text-align:right;"> 1995 </td>
   <td style="text-align:right;"> 6.7436315 </td>
   <td style="text-align:left;"> Europe &amp; Central Asia </td>
   <td style="text-align:left;"> Asia </td>
  </tr>
  <tr>
   <td style="text-align:left;"> Lesotho </td>
   <td style="text-align:left;"> LSO </td>
   <td style="text-align:right;"> 2005 </td>
   <td style="text-align:right;"> 6.3243268 </td>
   <td style="text-align:left;"> Sub-Saharan Africa </td>
   <td style="text-align:left;"> Africa </td>
  </tr>
  <tr>
   <td style="text-align:left;"> Syria </td>
   <td style="text-align:left;"> SYR </td>
   <td style="text-align:right;"> 2001 </td>
   <td style="text-align:right;"> 0.3003931 </td>
   <td style="text-align:left;"> Middle East &amp; North Africa </td>
   <td style="text-align:left;"> Asia </td>
  </tr>
  <tr>
   <td style="text-align:left;"> Oman </td>
   <td style="text-align:left;"> OMN </td>
   <td style="text-align:right;"> 1994 </td>
   <td style="text-align:right;"> 4.5665262 </td>
   <td style="text-align:left;"> Middle East &amp; North Africa </td>
   <td style="text-align:left;"> Asia </td>
  </tr>
  <tr>
   <td style="text-align:left;"> Maldives </td>
   <td style="text-align:left;"> MDV </td>
   <td style="text-align:right;"> 2008 </td>
   <td style="text-align:right;"> 4.1813488 </td>
   <td style="text-align:left;"> South Asia </td>
   <td style="text-align:left;"> Asia </td>
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
   <td style="text-align:left;"> Cambodia </td>
   <td style="text-align:left;"> KHM </td>
   <td style="text-align:right;"> 1992 </td>
   <td style="text-align:right;"> 17.9062329 </td>
   <td style="text-align:left;"> East Asia &amp; Pacific </td>
   <td style="text-align:left;"> Asia </td>
  </tr>
  <tr>
   <td style="text-align:left;"> India </td>
   <td style="text-align:left;"> IND </td>
   <td style="text-align:right;"> 2005 </td>
   <td style="text-align:right;"> 11.2061246 </td>
   <td style="text-align:left;"> South Asia </td>
   <td style="text-align:left;"> Asia </td>
  </tr>
  <tr>
   <td style="text-align:left;"> Kenya </td>
   <td style="text-align:left;"> KEN </td>
   <td style="text-align:right;"> 1997 </td>
   <td style="text-align:right;"> 8.2210608 </td>
   <td style="text-align:left;"> Sub-Saharan Africa </td>
   <td style="text-align:left;"> Africa </td>
  </tr>
  <tr>
   <td style="text-align:left;"> Singapore </td>
   <td style="text-align:left;"> SGP </td>
   <td style="text-align:right;"> 2007 </td>
   <td style="text-align:right;"> 0.0308578 </td>
   <td style="text-align:left;"> East Asia &amp; Pacific </td>
   <td style="text-align:left;"> Asia </td>
  </tr>
  <tr>
   <td style="text-align:left;"> Israel </td>
   <td style="text-align:left;"> ISR </td>
   <td style="text-align:right;"> 2016 </td>
   <td style="text-align:right;"> 0.0089538 </td>
   <td style="text-align:left;"> Middle East &amp; North Africa </td>
   <td style="text-align:left;"> Asia </td>
  </tr>
  <tr>
   <td style="text-align:left;"> Solomon Islands </td>
   <td style="text-align:left;"> SLB </td>
   <td style="text-align:right;"> 2002 </td>
   <td style="text-align:right;"> 22.1679400 </td>
   <td style="text-align:left;"> East Asia &amp; Pacific </td>
   <td style="text-align:left;"> Oceania </td>
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
   <td style="text-align:left;"> Niue </td>
   <td style="text-align:left;"> NIU </td>
   <td style="text-align:right;"> 2011 </td>
   <td style="text-align:right;"> 0.8079687 </td>
   <td style="text-align:left;"> East Asia &amp; Pacific </td>
   <td style="text-align:left;"> Oceania </td>
  </tr>
  <tr>
   <td style="text-align:left;"> Armenia </td>
   <td style="text-align:left;"> ARM </td>
   <td style="text-align:right;"> 2006 </td>
   <td style="text-align:right;"> 1.6005792 </td>
   <td style="text-align:left;"> Europe &amp; Central Asia </td>
   <td style="text-align:left;"> Asia </td>
  </tr>
  <tr>
   <td style="text-align:left;"> Qatar </td>
   <td style="text-align:left;"> QAT </td>
   <td style="text-align:right;"> 2014 </td>
   <td style="text-align:right;"> 0.0016354 </td>
   <td style="text-align:left;"> Middle East &amp; North Africa </td>
   <td style="text-align:left;"> Asia </td>
  </tr>
  <tr>
   <td style="text-align:left;"> Guyana </td>
   <td style="text-align:left;"> GUY </td>
   <td style="text-align:right;"> 2011 </td>
   <td style="text-align:right;"> 1.6754385 </td>
   <td style="text-align:left;"> Latin America &amp; Caribbean </td>
   <td style="text-align:left;"> Americas </td>
  </tr>
  <tr>
   <td style="text-align:left;"> Burkina Faso </td>
   <td style="text-align:left;"> BFA </td>
   <td style="text-align:right;"> 2004 </td>
   <td style="text-align:right;"> 12.3239976 </td>
   <td style="text-align:left;"> Sub-Saharan Africa </td>
   <td style="text-align:left;"> Africa </td>
  </tr>
  <tr>
   <td style="text-align:left;"> Vietnam </td>
   <td style="text-align:left;"> VNM </td>
   <td style="text-align:right;"> 2011 </td>
   <td style="text-align:right;"> 7.8160350 </td>
   <td style="text-align:left;"> East Asia &amp; Pacific </td>
   <td style="text-align:left;"> Asia </td>
  </tr>
  <tr>
   <td style="text-align:left;"> Jordan </td>
   <td style="text-align:left;"> JOR </td>
   <td style="text-align:right;"> 2017 </td>
   <td style="text-align:right;"> 0.0055866 </td>
   <td style="text-align:left;"> Middle East &amp; North Africa </td>
   <td style="text-align:left;"> Asia </td>
  </tr>
  <tr>
   <td style="text-align:left;"> Monaco </td>
   <td style="text-align:left;"> MCO </td>
   <td style="text-align:right;"> 2016 </td>
   <td style="text-align:right;"> 0.0030159 </td>
   <td style="text-align:left;"> Europe &amp; Central Asia </td>
   <td style="text-align:left;"> Europe </td>
  </tr>
  <tr>
   <td style="text-align:left;"> Barbados </td>
   <td style="text-align:left;"> BRB </td>
   <td style="text-align:right;"> 2019 </td>
   <td style="text-align:right;"> 0.0064758 </td>
   <td style="text-align:left;"> Latin America &amp; Caribbean </td>
   <td style="text-align:left;"> Americas </td>
  </tr>
  <tr>
   <td style="text-align:left;"> Papua New Guinea </td>
   <td style="text-align:left;"> PNG </td>
   <td style="text-align:right;"> 2011 </td>
   <td style="text-align:right;"> 19.0492515 </td>
   <td style="text-align:left;"> East Asia &amp; Pacific </td>
   <td style="text-align:left;"> Oceania </td>
  </tr>
  <tr>
   <td style="text-align:left;"> Guam </td>
   <td style="text-align:left;"> GUM </td>
   <td style="text-align:right;"> 1997 </td>
   <td style="text-align:right;"> 0.3783822 </td>
   <td style="text-align:left;"> East Asia &amp; Pacific </td>
   <td style="text-align:left;"> Oceania </td>
  </tr>
  <tr>
   <td style="text-align:left;"> Tajikistan </td>
   <td style="text-align:left;"> TJK </td>
   <td style="text-align:right;"> 2010 </td>
   <td style="text-align:right;"> 8.7442293 </td>
   <td style="text-align:left;"> Europe &amp; Central Asia </td>
   <td style="text-align:left;"> Asia </td>
  </tr>
  <tr>
   <td style="text-align:left;"> Fiji </td>
   <td style="text-align:left;"> FJI </td>
   <td style="text-align:right;"> 1996 </td>
   <td style="text-align:right;"> 9.0859204 </td>
   <td style="text-align:left;"> East Asia &amp; Pacific </td>
   <td style="text-align:left;"> Oceania </td>
  </tr>
  <tr>
   <td style="text-align:left;"> United States </td>
   <td style="text-align:left;"> USA </td>
   <td style="text-align:right;"> 2012 </td>
   <td style="text-align:right;"> 0.0070332 </td>
   <td style="text-align:left;"> North America </td>
   <td style="text-align:left;"> Americas </td>
  </tr>
  <tr>
   <td style="text-align:left;"> Solomon Islands </td>
   <td style="text-align:left;"> SLB </td>
   <td style="text-align:right;"> 1992 </td>
   <td style="text-align:right;"> 23.3369531 </td>
   <td style="text-align:left;"> East Asia &amp; Pacific </td>
   <td style="text-align:left;"> Oceania </td>
  </tr>
  <tr>
   <td style="text-align:left;"> Tuvalu </td>
   <td style="text-align:left;"> TUV </td>
   <td style="text-align:right;"> 2009 </td>
   <td style="text-align:right;"> 3.4219698 </td>
   <td style="text-align:left;"> East Asia &amp; Pacific </td>
   <td style="text-align:left;"> Oceania </td>
  </tr>
  <tr>
   <td style="text-align:left;"> Iran </td>
   <td style="text-align:left;"> IRN </td>
   <td style="text-align:right;"> 1995 </td>
   <td style="text-align:right;"> 1.1796704 </td>
   <td style="text-align:left;"> Middle East &amp; North Africa </td>
   <td style="text-align:left;"> Asia </td>
  </tr>
  <tr>
   <td style="text-align:left;"> Iceland </td>
   <td style="text-align:left;"> ISL </td>
   <td style="text-align:right;"> 2008 </td>
   <td style="text-align:right;"> 0.0075401 </td>
   <td style="text-align:left;"> Europe &amp; Central Asia </td>
   <td style="text-align:left;"> Europe </td>
  </tr>
  <tr>
   <td style="text-align:left;"> Lithuania </td>
   <td style="text-align:left;"> LTU </td>
   <td style="text-align:right;"> 2012 </td>
   <td style="text-align:right;"> 0.2359895 </td>
   <td style="text-align:left;"> Europe &amp; Central Asia </td>
   <td style="text-align:left;"> Europe </td>
  </tr>
  <tr>
   <td style="text-align:left;"> Bahrain </td>
   <td style="text-align:left;"> BHR </td>
   <td style="text-align:right;"> 2002 </td>
   <td style="text-align:right;"> 0.3031292 </td>
   <td style="text-align:left;"> Middle East &amp; North Africa </td>
   <td style="text-align:left;"> Asia </td>
  </tr>
  <tr>
   <td style="text-align:left;"> Yemen </td>
   <td style="text-align:left;"> YEM </td>
   <td style="text-align:right;"> 2009 </td>
   <td style="text-align:right;"> 9.8119761 </td>
   <td style="text-align:left;"> Middle East &amp; North Africa </td>
   <td style="text-align:left;"> Asia </td>
  </tr>
  <tr>
   <td style="text-align:left;"> India </td>
   <td style="text-align:left;"> IND </td>
   <td style="text-align:right;"> 2017 </td>
   <td style="text-align:right;"> 7.2710429 </td>
   <td style="text-align:left;"> South Asia </td>
   <td style="text-align:left;"> Asia </td>
  </tr>
  <tr>
   <td style="text-align:left;"> Kiribati </td>
   <td style="text-align:left;"> KIR </td>
   <td style="text-align:right;"> 1991 </td>
   <td style="text-align:right;"> 13.9076912 </td>
   <td style="text-align:left;"> East Asia &amp; Pacific </td>
   <td style="text-align:left;"> Oceania </td>
  </tr>
  <tr>
   <td style="text-align:left;"> Russia </td>
   <td style="text-align:left;"> RUS </td>
   <td style="text-align:right;"> 2004 </td>
   <td style="text-align:right;"> 0.4016809 </td>
   <td style="text-align:left;"> Europe &amp; Central Asia </td>
   <td style="text-align:left;"> Europe </td>
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
   <td style="text-align:left;"> Namibia </td>
   <td style="text-align:left;"> NAM </td>
   <td style="text-align:right;"> 2005 </td>
   <td style="text-align:right;"> 6.0292090 </td>
   <td style="text-align:left;"> Sub-Saharan Africa </td>
   <td style="text-align:left;"> Africa </td>
  </tr>
  <tr>
   <td style="text-align:left;"> Congo </td>
   <td style="text-align:left;"> COG </td>
   <td style="text-align:right;"> 1996 </td>
   <td style="text-align:right;"> 9.4392508 </td>
   <td style="text-align:left;"> Sub-Saharan Africa </td>
   <td style="text-align:left;"> Africa </td>
  </tr>
  <tr>
   <td style="text-align:left;"> Madagascar </td>
   <td style="text-align:left;"> MDG </td>
   <td style="text-align:right;"> 1992 </td>
   <td style="text-align:right;"> 15.7018447 </td>
   <td style="text-align:left;"> Sub-Saharan Africa </td>
   <td style="text-align:left;"> Africa </td>
  </tr>
  <tr>
   <td style="text-align:left;"> Armenia </td>
   <td style="text-align:left;"> ARM </td>
   <td style="text-align:right;"> 1993 </td>
   <td style="text-align:right;"> 4.1107227 </td>
   <td style="text-align:left;"> Europe &amp; Central Asia </td>
   <td style="text-align:left;"> Asia </td>
  </tr>
  <tr>
   <td style="text-align:left;"> Mongolia </td>
   <td style="text-align:left;"> MNG </td>
   <td style="text-align:right;"> 2006 </td>
   <td style="text-align:right;"> 9.1159213 </td>
   <td style="text-align:left;"> East Asia &amp; Pacific </td>
   <td style="text-align:left;"> Asia </td>
  </tr>
  <tr>
   <td style="text-align:left;"> Thailand </td>
   <td style="text-align:left;"> THA </td>
   <td style="text-align:right;"> 2014 </td>
   <td style="text-align:right;"> 2.0389738 </td>
   <td style="text-align:left;"> East Asia &amp; Pacific </td>
   <td style="text-align:left;"> Asia </td>
  </tr>
  <tr>
   <td style="text-align:left;"> Jordan </td>
   <td style="text-align:left;"> JOR </td>
   <td style="text-align:right;"> 2010 </td>
   <td style="text-align:right;"> 0.0118614 </td>
   <td style="text-align:left;"> Middle East &amp; North Africa </td>
   <td style="text-align:left;"> Asia </td>
  </tr>
  <tr>
   <td style="text-align:left;"> Singapore </td>
   <td style="text-align:left;"> SGP </td>
   <td style="text-align:right;"> 1992 </td>
   <td style="text-align:right;"> 0.3650727 </td>
   <td style="text-align:left;"> East Asia &amp; Pacific </td>
   <td style="text-align:left;"> Asia </td>
  </tr>
  <tr>
   <td style="text-align:left;"> Puerto Rico </td>
   <td style="text-align:left;"> PRI </td>
   <td style="text-align:right;"> 2001 </td>
   <td style="text-align:right;"> 0.0048893 </td>
   <td style="text-align:left;"> Latin America &amp; Caribbean </td>
   <td style="text-align:left;"> Americas </td>
  </tr>
  <tr>
   <td style="text-align:left;"> Samoa </td>
   <td style="text-align:left;"> WSM </td>
   <td style="text-align:right;"> 2016 </td>
   <td style="text-align:right;"> 11.6459726 </td>
   <td style="text-align:left;"> East Asia &amp; Pacific </td>
   <td style="text-align:left;"> Oceania </td>
  </tr>
  <tr>
   <td style="text-align:left;"> Vanuatu </td>
   <td style="text-align:left;"> VUT </td>
   <td style="text-align:right;"> 2014 </td>
   <td style="text-align:right;"> 16.8947949 </td>
   <td style="text-align:left;"> East Asia &amp; Pacific </td>
   <td style="text-align:left;"> Oceania </td>
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
   <td style="text-align:left;"> Ireland </td>
   <td style="text-align:left;"> IRL </td>
   <td style="text-align:right;"> 2008 </td>
   <td style="text-align:right;"> 0.0137819 </td>
   <td style="text-align:left;"> Europe &amp; Central Asia </td>
   <td style="text-align:left;"> Europe </td>
  </tr>
  <tr>
   <td style="text-align:left;"> Malawi </td>
   <td style="text-align:left;"> MWI </td>
   <td style="text-align:right;"> 1991 </td>
   <td style="text-align:right;"> 11.4285056 </td>
   <td style="text-align:left;"> Sub-Saharan Africa </td>
   <td style="text-align:left;"> Africa </td>
  </tr>
  <tr>
   <td style="text-align:left;"> Uruguay </td>
   <td style="text-align:left;"> URY </td>
   <td style="text-align:right;"> 1990 </td>
   <td style="text-align:right;"> 1.3099788 </td>
   <td style="text-align:left;"> Latin America &amp; Caribbean </td>
   <td style="text-align:left;"> Americas </td>
  </tr>
  <tr>
   <td style="text-align:left;"> United Arab Emirates </td>
   <td style="text-align:left;"> ARE </td>
   <td style="text-align:right;"> 2013 </td>
   <td style="text-align:right;"> 0.0008376 </td>
   <td style="text-align:left;"> Middle East &amp; North Africa </td>
   <td style="text-align:left;"> Asia </td>
  </tr>
  <tr>
   <td style="text-align:left;"> Australia </td>
   <td style="text-align:left;"> AUS </td>
   <td style="text-align:right;"> 2015 </td>
   <td style="text-align:right;"> 0.0099497 </td>
   <td style="text-align:left;"> East Asia &amp; Pacific </td>
   <td style="text-align:left;"> Oceania </td>
  </tr>
  <tr>
   <td style="text-align:left;"> Tanzania </td>
   <td style="text-align:left;"> TZA </td>
   <td style="text-align:right;"> 1990 </td>
   <td style="text-align:right;"> 12.8436686 </td>
   <td style="text-align:left;"> Sub-Saharan Africa </td>
   <td style="text-align:left;"> Africa </td>
  </tr>
  <tr>
   <td style="text-align:left;"> Serbia </td>
   <td style="text-align:left;"> SRB </td>
   <td style="text-align:right;"> 2019 </td>
   <td style="text-align:right;"> 1.5765989 </td>
   <td style="text-align:left;"> Europe &amp; Central Asia </td>
   <td style="text-align:left;"> Europe </td>
  </tr>
  <tr>
   <td style="text-align:left;"> Mali </td>
   <td style="text-align:left;"> MLI </td>
   <td style="text-align:right;"> 2004 </td>
   <td style="text-align:right;"> 12.1428574 </td>
   <td style="text-align:left;"> Sub-Saharan Africa </td>
   <td style="text-align:left;"> Africa </td>
  </tr>
  <tr>
   <td style="text-align:left;"> Philippines </td>
   <td style="text-align:left;"> PHL </td>
   <td style="text-align:right;"> 1996 </td>
   <td style="text-align:right;"> 8.4379430 </td>
   <td style="text-align:left;"> East Asia &amp; Pacific </td>
   <td style="text-align:left;"> Asia </td>
  </tr>
  <tr>
   <td style="text-align:left;"> Azerbaijan </td>
   <td style="text-align:left;"> AZE </td>
   <td style="text-align:right;"> 1993 </td>
   <td style="text-align:right;"> 6.8820465 </td>
   <td style="text-align:left;"> Europe &amp; Central Asia </td>
   <td style="text-align:left;"> Asia </td>
  </tr>
  <tr>
   <td style="text-align:left;"> Denmark </td>
   <td style="text-align:left;"> DNK </td>
   <td style="text-align:right;"> 2015 </td>
   <td style="text-align:right;"> 0.0044517 </td>
   <td style="text-align:left;"> Europe &amp; Central Asia </td>
   <td style="text-align:left;"> Europe </td>
  </tr>
  <tr>
   <td style="text-align:left;"> Canada </td>
   <td style="text-align:left;"> CAN </td>
   <td style="text-align:right;"> 2015 </td>
   <td style="text-align:right;"> 0.0033341 </td>
   <td style="text-align:left;"> North America </td>
   <td style="text-align:left;"> Americas </td>
  </tr>
  <tr>
   <td style="text-align:left;"> Tanzania </td>
   <td style="text-align:left;"> TZA </td>
   <td style="text-align:right;"> 2011 </td>
   <td style="text-align:right;"> 11.0878758 </td>
   <td style="text-align:left;"> Sub-Saharan Africa </td>
   <td style="text-align:left;"> Africa </td>
  </tr>
  <tr>
   <td style="text-align:left;"> Cambodia </td>
   <td style="text-align:left;"> KHM </td>
   <td style="text-align:right;"> 1999 </td>
   <td style="text-align:right;"> 16.8016371 </td>
   <td style="text-align:left;"> East Asia &amp; Pacific </td>
   <td style="text-align:left;"> Asia </td>
  </tr>
  <tr>
   <td style="text-align:left;"> Bahrain </td>
   <td style="text-align:left;"> BHR </td>
   <td style="text-align:right;"> 1993 </td>
   <td style="text-align:right;"> 1.0370835 </td>
   <td style="text-align:left;"> Middle East &amp; North Africa </td>
   <td style="text-align:left;"> Asia </td>
  </tr>
  <tr>
   <td style="text-align:left;"> Trinidad and Tobago </td>
   <td style="text-align:left;"> TTO </td>
   <td style="text-align:right;"> 2004 </td>
   <td style="text-align:right;"> 0.0379728 </td>
   <td style="text-align:left;"> Latin America &amp; Caribbean </td>
   <td style="text-align:left;"> Americas </td>
  </tr>
  <tr>
   <td style="text-align:left;"> Guinea </td>
   <td style="text-align:left;"> GIN </td>
   <td style="text-align:right;"> 1995 </td>
   <td style="text-align:right;"> 14.6491243 </td>
   <td style="text-align:left;"> Sub-Saharan Africa </td>
   <td style="text-align:left;"> Africa </td>
  </tr>
  <tr>
   <td style="text-align:left;"> Egypt </td>
   <td style="text-align:left;"> EGY </td>
   <td style="text-align:right;"> 1997 </td>
   <td style="text-align:right;"> 0.6752659 </td>
   <td style="text-align:left;"> Middle East &amp; North Africa </td>
   <td style="text-align:left;"> Africa </td>
  </tr>
  <tr>
   <td style="text-align:left;"> Nigeria </td>
   <td style="text-align:left;"> NGA </td>
   <td style="text-align:right;"> 1990 </td>
   <td style="text-align:right;"> 12.4422217 </td>
   <td style="text-align:left;"> Sub-Saharan Africa </td>
   <td style="text-align:left;"> Africa </td>
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
   <td style="text-align:left;"> Russia </td>
   <td style="text-align:left;"> RUS </td>
   <td style="text-align:right;"> 2011 </td>
   <td style="text-align:right;"> 0.2051149 </td>
   <td style="text-align:left;"> Europe &amp; Central Asia </td>
   <td style="text-align:left;"> Europe </td>
  </tr>
  <tr>
   <td style="text-align:left;"> North Macedonia </td>
   <td style="text-align:left;"> MKD </td>
   <td style="text-align:right;"> 2010 </td>
   <td style="text-align:right;"> 3.0002872 </td>
   <td style="text-align:left;"> Europe &amp; Central Asia </td>
   <td style="text-align:left;"> Europe </td>
  </tr>
  <tr>
   <td style="text-align:left;"> Nepal </td>
   <td style="text-align:left;"> NPL </td>
   <td style="text-align:right;"> 2014 </td>
   <td style="text-align:right;"> 13.7081035 </td>
   <td style="text-align:left;"> South Asia </td>
   <td style="text-align:left;"> Asia </td>
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
   <td style="text-align:left;"> Brazil </td>
   <td style="text-align:left;"> BRA </td>
   <td style="text-align:right;"> 1996 </td>
   <td style="text-align:right;"> 4.0757206 </td>
   <td style="text-align:left;"> Latin America &amp; Caribbean </td>
   <td style="text-align:left;"> Americas </td>
  </tr>
  <tr>
   <td style="text-align:left;"> Romania </td>
   <td style="text-align:left;"> ROU </td>
   <td style="text-align:right;"> 1993 </td>
   <td style="text-align:right;"> 4.5339027 </td>
   <td style="text-align:left;"> Europe &amp; Central Asia </td>
   <td style="text-align:left;"> Europe </td>
  </tr>
  <tr>
   <td style="text-align:left;"> Romania </td>
   <td style="text-align:left;"> ROU </td>
   <td style="text-align:right;"> 1998 </td>
   <td style="text-align:right;"> 3.6587190 </td>
   <td style="text-align:left;"> Europe &amp; Central Asia </td>
   <td style="text-align:left;"> Europe </td>
  </tr>
  <tr>
   <td style="text-align:left;"> South Sudan </td>
   <td style="text-align:left;"> SSD </td>
   <td style="text-align:right;"> 1997 </td>
   <td style="text-align:right;"> 11.7453873 </td>
   <td style="text-align:left;"> Sub-Saharan Africa </td>
   <td style="text-align:left;"> Africa </td>
  </tr>
  <tr>
   <td style="text-align:left;"> Singapore </td>
   <td style="text-align:left;"> SGP </td>
   <td style="text-align:right;"> 2000 </td>
   <td style="text-align:right;"> 0.0986003 </td>
   <td style="text-align:left;"> East Asia &amp; Pacific </td>
   <td style="text-align:left;"> Asia </td>
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
   <td style="text-align:left;"> Panama </td>
   <td style="text-align:left;"> PAN </td>
   <td style="text-align:right;"> 2018 </td>
   <td style="text-align:right;"> 0.8512415 </td>
   <td style="text-align:left;"> Latin America &amp; Caribbean </td>
   <td style="text-align:left;"> Americas </td>
  </tr>
  <tr>
   <td style="text-align:left;"> Slovakia </td>
   <td style="text-align:left;"> SVK </td>
   <td style="text-align:right;"> 2011 </td>
   <td style="text-align:right;"> 0.0907437 </td>
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
   <td style="text-align:left;"> Japan </td>
   <td style="text-align:left;"> JPN </td>
   <td style="text-align:right;"> 1993 </td>
   <td style="text-align:right;"> 0.0486968 </td>
   <td style="text-align:left;"> East Asia &amp; Pacific </td>
   <td style="text-align:left;"> Asia </td>
  </tr>
  <tr>
   <td style="text-align:left;"> Chad </td>
   <td style="text-align:left;"> TCD </td>
   <td style="text-align:right;"> 1998 </td>
   <td style="text-align:right;"> 14.3444996 </td>
   <td style="text-align:left;"> Sub-Saharan Africa </td>
   <td style="text-align:left;"> Africa </td>
  </tr>
  <tr>
   <td style="text-align:left;"> Zambia </td>
   <td style="text-align:left;"> ZMB </td>
   <td style="text-align:right;"> 1995 </td>
   <td style="text-align:right;"> 8.8637690 </td>
   <td style="text-align:left;"> Sub-Saharan Africa </td>
   <td style="text-align:left;"> Africa </td>
  </tr>
  <tr>
   <td style="text-align:left;"> Belarus </td>
   <td style="text-align:left;"> BLR </td>
   <td style="text-align:right;"> 2005 </td>
   <td style="text-align:right;"> 0.3273516 </td>
   <td style="text-align:left;"> Europe &amp; Central Asia </td>
   <td style="text-align:left;"> Europe </td>
  </tr>
  <tr>
   <td style="text-align:left;"> Mongolia </td>
   <td style="text-align:left;"> MNG </td>
   <td style="text-align:right;"> 2008 </td>
   <td style="text-align:right;"> 8.0170333 </td>
   <td style="text-align:left;"> East Asia &amp; Pacific </td>
   <td style="text-align:left;"> Asia </td>
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
   <td style="text-align:left;"> Cyprus </td>
   <td style="text-align:left;"> CYP </td>
   <td style="text-align:right;"> 1997 </td>
   <td style="text-align:right;"> 0.0663289 </td>
   <td style="text-align:left;"> Europe &amp; Central Asia </td>
   <td style="text-align:left;"> Asia </td>
  </tr>
  <tr>
   <td style="text-align:left;"> Grenada </td>
   <td style="text-align:left;"> GRD </td>
   <td style="text-align:right;"> 2010 </td>
   <td style="text-align:right;"> 0.5172826 </td>
   <td style="text-align:left;"> Latin America &amp; Caribbean </td>
   <td style="text-align:left;"> Americas </td>
  </tr>
  <tr>
   <td style="text-align:left;"> Uruguay </td>
   <td style="text-align:left;"> URY </td>
   <td style="text-align:right;"> 2008 </td>
   <td style="text-align:right;"> 0.3249201 </td>
   <td style="text-align:left;"> Latin America &amp; Caribbean </td>
   <td style="text-align:left;"> Americas </td>
  </tr>
  <tr>
   <td style="text-align:left;"> Northern Mariana Islands </td>
   <td style="text-align:left;"> MNP </td>
   <td style="text-align:right;"> 2001 </td>
   <td style="text-align:right;"> 1.3877803 </td>
   <td style="text-align:left;"> East Asia &amp; Pacific </td>
   <td style="text-align:left;"> Oceania </td>
  </tr>
  <tr>
   <td style="text-align:left;"> Kuwait </td>
   <td style="text-align:left;"> KWT </td>
   <td style="text-align:right;"> 2013 </td>
   <td style="text-align:right;"> 0.0100488 </td>
   <td style="text-align:left;"> Middle East &amp; North Africa </td>
   <td style="text-align:left;"> Asia </td>
  </tr>
  <tr>
   <td style="text-align:left;"> Canada </td>
   <td style="text-align:left;"> CAN </td>
   <td style="text-align:right;"> 2016 </td>
   <td style="text-align:right;"> 0.0032029 </td>
   <td style="text-align:left;"> North America </td>
   <td style="text-align:left;"> Americas </td>
  </tr>
  <tr>
   <td style="text-align:left;"> Belgium </td>
   <td style="text-align:left;"> BEL </td>
   <td style="text-align:right;"> 1995 </td>
   <td style="text-align:right;"> 0.0275256 </td>
   <td style="text-align:left;"> Europe &amp; Central Asia </td>
   <td style="text-align:left;"> Europe </td>
  </tr>
  <tr>
   <td style="text-align:left;"> Belarus </td>
   <td style="text-align:left;"> BLR </td>
   <td style="text-align:right;"> 2016 </td>
   <td style="text-align:right;"> 0.0930432 </td>
   <td style="text-align:left;"> Europe &amp; Central Asia </td>
   <td style="text-align:left;"> Europe </td>
  </tr>
  <tr>
   <td style="text-align:left;"> Sweden </td>
   <td style="text-align:left;"> SWE </td>
   <td style="text-align:right;"> 2008 </td>
   <td style="text-align:right;"> 0.0076538 </td>
   <td style="text-align:left;"> Europe &amp; Central Asia </td>
   <td style="text-align:left;"> Europe </td>
  </tr>
  <tr>
   <td style="text-align:left;"> Mozambique </td>
   <td style="text-align:left;"> MOZ </td>
   <td style="text-align:right;"> 2005 </td>
   <td style="text-align:right;"> 10.9786152 </td>
   <td style="text-align:left;"> Sub-Saharan Africa </td>
   <td style="text-align:left;"> Africa </td>
  </tr>
  <tr>
   <td style="text-align:left;"> Bulgaria </td>
   <td style="text-align:left;"> BGR </td>
   <td style="text-align:right;"> 2019 </td>
   <td style="text-align:right;"> 1.3926602 </td>
   <td style="text-align:left;"> Europe &amp; Central Asia </td>
   <td style="text-align:left;"> Europe </td>
  </tr>
  <tr>
   <td style="text-align:left;"> Angola </td>
   <td style="text-align:left;"> AGO </td>
   <td style="text-align:right;"> 2018 </td>
   <td style="text-align:right;"> 6.0186394 </td>
   <td style="text-align:left;"> Sub-Saharan Africa </td>
   <td style="text-align:left;"> Africa </td>
  </tr>
  <tr>
   <td style="text-align:left;"> Turkey </td>
   <td style="text-align:left;"> TUR </td>
   <td style="text-align:right;"> 2002 </td>
   <td style="text-align:right;"> 0.4706266 </td>
   <td style="text-align:left;"> Europe &amp; Central Asia </td>
   <td style="text-align:left;"> Asia </td>
  </tr>
  <tr>
   <td style="text-align:left;"> Taiwan </td>
   <td style="text-align:left;"> TWN </td>
   <td style="text-align:right;"> 2005 </td>
   <td style="text-align:right;"> 0.6473961 </td>
   <td style="text-align:left;"> East Asia &amp; Pacific </td>
   <td style="text-align:left;"> Asia </td>
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
   <td style="text-align:left;"> Sudan </td>
   <td style="text-align:left;"> SDN </td>
   <td style="text-align:right;"> 2007 </td>
   <td style="text-align:right;"> 12.6278016 </td>
   <td style="text-align:left;"> Sub-Saharan Africa </td>
   <td style="text-align:left;"> Africa </td>
  </tr>
  <tr>
   <td style="text-align:left;"> Malta </td>
   <td style="text-align:left;"> MLT </td>
   <td style="text-align:right;"> 1997 </td>
   <td style="text-align:right;"> 0.0764122 </td>
   <td style="text-align:left;"> Middle East &amp; North Africa </td>
   <td style="text-align:left;"> Europe </td>
  </tr>
  <tr>
   <td style="text-align:left;"> Canada </td>
   <td style="text-align:left;"> CAN </td>
   <td style="text-align:right;"> 2018 </td>
   <td style="text-align:right;"> 0.0029674 </td>
   <td style="text-align:left;"> North America </td>
   <td style="text-align:left;"> Americas </td>
  </tr>
  <tr>
   <td style="text-align:left;"> Jordan </td>
   <td style="text-align:left;"> JOR </td>
   <td style="text-align:right;"> 1995 </td>
   <td style="text-align:right;"> 0.1326154 </td>
   <td style="text-align:left;"> Middle East &amp; North Africa </td>
   <td style="text-align:left;"> Asia </td>
  </tr>
  <tr>
   <td style="text-align:left;"> Solomon Islands </td>
   <td style="text-align:left;"> SLB </td>
   <td style="text-align:right;"> 2014 </td>
   <td style="text-align:right;"> 21.3418847 </td>
   <td style="text-align:left;"> East Asia &amp; Pacific </td>
   <td style="text-align:left;"> Oceania </td>
  </tr>
  <tr>
   <td style="text-align:left;"> Bulgaria </td>
   <td style="text-align:left;"> BGR </td>
   <td style="text-align:right;"> 2006 </td>
   <td style="text-align:right;"> 2.1216908 </td>
   <td style="text-align:left;"> Europe &amp; Central Asia </td>
   <td style="text-align:left;"> Europe </td>
  </tr>
  <tr>
   <td style="text-align:left;"> Iraq </td>
   <td style="text-align:left;"> IRQ </td>
   <td style="text-align:right;"> 2010 </td>
   <td style="text-align:right;"> 0.3130770 </td>
   <td style="text-align:left;"> Middle East &amp; North Africa </td>
   <td style="text-align:left;"> Asia </td>
  </tr>
  <tr>
   <td style="text-align:left;"> Oman </td>
   <td style="text-align:left;"> OMN </td>
   <td style="text-align:right;"> 2004 </td>
   <td style="text-align:right;"> 0.5596392 </td>
   <td style="text-align:left;"> Middle East &amp; North Africa </td>
   <td style="text-align:left;"> Asia </td>
  </tr>
  <tr>
   <td style="text-align:left;"> Niger </td>
   <td style="text-align:left;"> NER </td>
   <td style="text-align:right;"> 1993 </td>
   <td style="text-align:right;"> 15.4268778 </td>
   <td style="text-align:left;"> Sub-Saharan Africa </td>
   <td style="text-align:left;"> Africa </td>
  </tr>
  <tr>
   <td style="text-align:left;"> Bolivia </td>
   <td style="text-align:left;"> BOL </td>
   <td style="text-align:right;"> 1990 </td>
   <td style="text-align:right;"> 8.4001375 </td>
   <td style="text-align:left;"> Latin America &amp; Caribbean </td>
   <td style="text-align:left;"> Americas </td>
  </tr>
  <tr>
   <td style="text-align:left;"> Venezuela </td>
   <td style="text-align:left;"> VEN </td>
   <td style="text-align:right;"> 2016 </td>
   <td style="text-align:right;"> 0.0549612 </td>
   <td style="text-align:left;"> Latin America &amp; Caribbean </td>
   <td style="text-align:left;"> Americas </td>
  </tr>
  <tr>
   <td style="text-align:left;"> Mali </td>
   <td style="text-align:left;"> MLI </td>
   <td style="text-align:right;"> 2002 </td>
   <td style="text-align:right;"> 12.0523720 </td>
   <td style="text-align:left;"> Sub-Saharan Africa </td>
   <td style="text-align:left;"> Africa </td>
  </tr>
  <tr>
   <td style="text-align:left;"> Libya </td>
   <td style="text-align:left;"> LBY </td>
   <td style="text-align:right;"> 2008 </td>
   <td style="text-align:right;"> 0.0881764 </td>
   <td style="text-align:left;"> Middle East &amp; North Africa </td>
   <td style="text-align:left;"> Africa </td>
  </tr>
  <tr>
   <td style="text-align:left;"> Sweden </td>
   <td style="text-align:left;"> SWE </td>
   <td style="text-align:right;"> 2017 </td>
   <td style="text-align:right;"> 0.0032938 </td>
   <td style="text-align:left;"> Europe &amp; Central Asia </td>
   <td style="text-align:left;"> Europe </td>
  </tr>
  <tr>
   <td style="text-align:left;"> Costa Rica </td>
   <td style="text-align:left;"> CRI </td>
   <td style="text-align:right;"> 2003 </td>
   <td style="text-align:right;"> 1.2308958 </td>
   <td style="text-align:left;"> Latin America &amp; Caribbean </td>
   <td style="text-align:left;"> Americas </td>
  </tr>
  <tr>
   <td style="text-align:left;"> Brazil </td>
   <td style="text-align:left;"> BRA </td>
   <td style="text-align:right;"> 1990 </td>
   <td style="text-align:right;"> 6.1032983 </td>
   <td style="text-align:left;"> Latin America &amp; Caribbean </td>
   <td style="text-align:left;"> Americas </td>
  </tr>
  <tr>
   <td style="text-align:left;"> Colombia </td>
   <td style="text-align:left;"> COL </td>
   <td style="text-align:right;"> 1996 </td>
   <td style="text-align:right;"> 4.3880516 </td>
   <td style="text-align:left;"> Latin America &amp; Caribbean </td>
   <td style="text-align:left;"> Americas </td>
  </tr>
  <tr>
   <td style="text-align:left;"> Greenland </td>
   <td style="text-align:left;"> GRL </td>
   <td style="text-align:right;"> 1998 </td>
   <td style="text-align:right;"> 0.4494264 </td>
   <td style="text-align:left;"> Europe &amp; Central Asia </td>
   <td style="text-align:left;"> Americas </td>
  </tr>
  <tr>
   <td style="text-align:left;"> Comoros </td>
   <td style="text-align:left;"> COM </td>
   <td style="text-align:right;"> 2018 </td>
   <td style="text-align:right;"> 11.5266531 </td>
   <td style="text-align:left;"> Sub-Saharan Africa </td>
   <td style="text-align:left;"> Africa </td>
  </tr>
  <tr>
   <td style="text-align:left;"> Barbados </td>
   <td style="text-align:left;"> BRB </td>
   <td style="text-align:right;"> 1992 </td>
   <td style="text-align:right;"> 0.0317064 </td>
   <td style="text-align:left;"> Latin America &amp; Caribbean </td>
   <td style="text-align:left;"> Americas </td>
  </tr>
  <tr>
   <td style="text-align:left;"> Congo </td>
   <td style="text-align:left;"> COG </td>
   <td style="text-align:right;"> 2007 </td>
   <td style="text-align:right;"> 8.2025782 </td>
   <td style="text-align:left;"> Sub-Saharan Africa </td>
   <td style="text-align:left;"> Africa </td>
  </tr>
  <tr>
   <td style="text-align:left;"> Mali </td>
   <td style="text-align:left;"> MLI </td>
   <td style="text-align:right;"> 2009 </td>
   <td style="text-align:right;"> 12.4187318 </td>
   <td style="text-align:left;"> Sub-Saharan Africa </td>
   <td style="text-align:left;"> Africa </td>
  </tr>
  <tr>
   <td style="text-align:left;"> Hungary </td>
   <td style="text-align:left;"> HUN </td>
   <td style="text-align:right;"> 2009 </td>
   <td style="text-align:right;"> 1.3981036 </td>
   <td style="text-align:left;"> Europe &amp; Central Asia </td>
   <td style="text-align:left;"> Europe </td>
  </tr>
  <tr>
   <td style="text-align:left;"> Saudi Arabia </td>
   <td style="text-align:left;"> SAU </td>
   <td style="text-align:right;"> 2015 </td>
   <td style="text-align:right;"> 0.0559492 </td>
   <td style="text-align:left;"> Middle East &amp; North Africa </td>
   <td style="text-align:left;"> Asia </td>
  </tr>
  <tr>
   <td style="text-align:left;"> Cambodia </td>
   <td style="text-align:left;"> KHM </td>
   <td style="text-align:right;"> 1993 </td>
   <td style="text-align:right;"> 17.8300134 </td>
   <td style="text-align:left;"> East Asia &amp; Pacific </td>
   <td style="text-align:left;"> Asia </td>
  </tr>
  <tr>
   <td style="text-align:left;"> Switzerland </td>
   <td style="text-align:left;"> CHE </td>
   <td style="text-align:right;"> 2001 </td>
   <td style="text-align:right;"> 0.0054563 </td>
   <td style="text-align:left;"> Europe &amp; Central Asia </td>
   <td style="text-align:left;"> Europe </td>
  </tr>
  <tr>
   <td style="text-align:left;"> Czechia </td>
   <td style="text-align:left;"> CZE </td>
   <td style="text-align:right;"> 2012 </td>
   <td style="text-align:right;"> 0.0925132 </td>
   <td style="text-align:left;"> Europe &amp; Central Asia </td>
   <td style="text-align:left;"> Europe </td>
  </tr>
  <tr>
   <td style="text-align:left;"> Morocco </td>
   <td style="text-align:left;"> MAR </td>
   <td style="text-align:right;"> 2001 </td>
   <td style="text-align:right;"> 4.4449132 </td>
   <td style="text-align:left;"> Middle East &amp; North Africa </td>
   <td style="text-align:left;"> Africa </td>
  </tr>
  <tr>
   <td style="text-align:left;"> Nicaragua </td>
   <td style="text-align:left;"> NIC </td>
   <td style="text-align:right;"> 1994 </td>
   <td style="text-align:right;"> 12.0238176 </td>
   <td style="text-align:left;"> Latin America &amp; Caribbean </td>
   <td style="text-align:left;"> Americas </td>
  </tr>
  <tr>
   <td style="text-align:left;"> Uzbekistan </td>
   <td style="text-align:left;"> UZB </td>
   <td style="text-align:right;"> 2012 </td>
   <td style="text-align:right;"> 2.7147055 </td>
   <td style="text-align:left;"> Europe &amp; Central Asia </td>
   <td style="text-align:left;"> Asia </td>
  </tr>
  <tr>
   <td style="text-align:left;"> Palau </td>
   <td style="text-align:left;"> PLW </td>
   <td style="text-align:right;"> 2013 </td>
   <td style="text-align:right;"> 0.0076840 </td>
   <td style="text-align:left;"> East Asia &amp; Pacific </td>
   <td style="text-align:left;"> Oceania </td>
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
   <td style="text-align:left;"> Tajikistan </td>
   <td style="text-align:left;"> TJK </td>
   <td style="text-align:right;"> 2000 </td>
   <td style="text-align:right;"> 12.9216571 </td>
   <td style="text-align:left;"> Europe &amp; Central Asia </td>
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
   <td style="text-align:left;"> Armenia </td>
   <td style="text-align:left;"> ARM </td>
   <td style="text-align:right;"> 2001 </td>
   <td style="text-align:right;"> 3.3245928 </td>
   <td style="text-align:left;"> Europe &amp; Central Asia </td>
   <td style="text-align:left;"> Asia </td>
  </tr>
  <tr>
   <td style="text-align:left;"> Sierra Leone </td>
   <td style="text-align:left;"> SLE </td>
   <td style="text-align:right;"> 2003 </td>
   <td style="text-align:right;"> 13.4382500 </td>
   <td style="text-align:left;"> Sub-Saharan Africa </td>
   <td style="text-align:left;"> Africa </td>
  </tr>
  <tr>
   <td style="text-align:left;"> Singapore </td>
   <td style="text-align:left;"> SGP </td>
   <td style="text-align:right;"> 1993 </td>
   <td style="text-align:right;"> 0.3094763 </td>
   <td style="text-align:left;"> East Asia &amp; Pacific </td>
   <td style="text-align:left;"> Asia </td>
  </tr>
  <tr>
   <td style="text-align:left;"> Kyrgyzstan </td>
   <td style="text-align:left;"> KGZ </td>
   <td style="text-align:right;"> 1992 </td>
   <td style="text-align:right;"> 10.8306899 </td>
   <td style="text-align:left;"> Europe &amp; Central Asia </td>
   <td style="text-align:left;"> Asia </td>
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
   <td style="text-align:left;"> Senegal </td>
   <td style="text-align:left;"> SEN </td>
   <td style="text-align:right;"> 2009 </td>
   <td style="text-align:right;"> 11.4730675 </td>
   <td style="text-align:left;"> Sub-Saharan Africa </td>
   <td style="text-align:left;"> Africa </td>
  </tr>
  <tr>
   <td style="text-align:left;"> Greenland </td>
   <td style="text-align:left;"> GRL </td>
   <td style="text-align:right;"> 1999 </td>
   <td style="text-align:right;"> 0.4178627 </td>
   <td style="text-align:left;"> Europe &amp; Central Asia </td>
   <td style="text-align:left;"> Americas </td>
  </tr>
  <tr>
   <td style="text-align:left;"> North Macedonia </td>
   <td style="text-align:left;"> MKD </td>
   <td style="text-align:right;"> 1994 </td>
   <td style="text-align:right;"> 5.8955084 </td>
   <td style="text-align:left;"> Europe &amp; Central Asia </td>
   <td style="text-align:left;"> Europe </td>
  </tr>
  <tr>
   <td style="text-align:left;"> Albania </td>
   <td style="text-align:left;"> ALB </td>
   <td style="text-align:right;"> 1998 </td>
   <td style="text-align:right;"> 10.1474871 </td>
   <td style="text-align:left;"> Europe &amp; Central Asia </td>
   <td style="text-align:left;"> Europe </td>
  </tr>
  <tr>
   <td style="text-align:left;"> Botswana </td>
   <td style="text-align:left;"> BWA </td>
   <td style="text-align:right;"> 2004 </td>
   <td style="text-align:right;"> 4.4951558 </td>
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
   <td style="text-align:right;"> 1996 </td>
   <td style="text-align:right;"> 0.7447248 </td>
   <td style="text-align:left;"> Latin America &amp; Caribbean </td>
   <td style="text-align:left;"> Americas </td>
  </tr>
  <tr>
   <td style="text-align:left;"> Nicaragua </td>
   <td style="text-align:left;"> NIC </td>
   <td style="text-align:right;"> 1997 </td>
   <td style="text-align:right;"> 11.6948668 </td>
   <td style="text-align:left;"> Latin America &amp; Caribbean </td>
   <td style="text-align:left;"> Americas </td>
  </tr>
  <tr>
   <td style="text-align:left;"> Barbados </td>
   <td style="text-align:left;"> BRB </td>
   <td style="text-align:right;"> 1993 </td>
   <td style="text-align:right;"> 0.0292277 </td>
   <td style="text-align:left;"> Latin America &amp; Caribbean </td>
   <td style="text-align:left;"> Americas </td>
  </tr>
  <tr>
   <td style="text-align:left;"> Argentina </td>
   <td style="text-align:left;"> ARG </td>
   <td style="text-align:right;"> 2012 </td>
   <td style="text-align:right;"> 0.3075327 </td>
   <td style="text-align:left;"> Latin America &amp; Caribbean </td>
   <td style="text-align:left;"> Americas </td>
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
   <td style="text-align:left;"> Mozambique </td>
   <td style="text-align:left;"> MOZ </td>
   <td style="text-align:right;"> 2019 </td>
   <td style="text-align:right;"> 11.6596677 </td>
   <td style="text-align:left;"> Sub-Saharan Africa </td>
   <td style="text-align:left;"> Africa </td>
  </tr>
  <tr>
   <td style="text-align:left;"> Djibouti </td>
   <td style="text-align:left;"> DJI </td>
   <td style="text-align:right;"> 2015 </td>
   <td style="text-align:right;"> 3.4582777 </td>
   <td style="text-align:left;"> Middle East &amp; North Africa </td>
   <td style="text-align:left;"> Africa </td>
  </tr>
  <tr>
   <td style="text-align:left;"> Guinea </td>
   <td style="text-align:left;"> GIN </td>
   <td style="text-align:right;"> 2015 </td>
   <td style="text-align:right;"> 13.5242799 </td>
   <td style="text-align:left;"> Sub-Saharan Africa </td>
   <td style="text-align:left;"> Africa </td>
  </tr>
  <tr>
   <td style="text-align:left;"> Belarus </td>
   <td style="text-align:left;"> BLR </td>
   <td style="text-align:right;"> 1995 </td>
   <td style="text-align:right;"> 0.6036668 </td>
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
   <td style="text-align:left;"> Bahamas </td>
   <td style="text-align:left;"> BHS </td>
   <td style="text-align:right;"> 2010 </td>
   <td style="text-align:right;"> 0.1255229 </td>
   <td style="text-align:left;"> Latin America &amp; Caribbean </td>
   <td style="text-align:left;"> Americas </td>
  </tr>
  <tr>
   <td style="text-align:left;"> Saint Lucia </td>
   <td style="text-align:left;"> LCA </td>
   <td style="text-align:right;"> 2012 </td>
   <td style="text-align:right;"> 0.5132948 </td>
   <td style="text-align:left;"> Latin America &amp; Caribbean </td>
   <td style="text-align:left;"> Americas </td>
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
   <td style="text-align:left;"> Cambodia </td>
   <td style="text-align:left;"> KHM </td>
   <td style="text-align:right;"> 2001 </td>
   <td style="text-align:right;"> 16.3553215 </td>
   <td style="text-align:left;"> East Asia &amp; Pacific </td>
   <td style="text-align:left;"> Asia </td>
  </tr>
  <tr>
   <td style="text-align:left;"> Brazil </td>
   <td style="text-align:left;"> BRA </td>
   <td style="text-align:right;"> 1998 </td>
   <td style="text-align:right;"> 3.6569369 </td>
   <td style="text-align:left;"> Latin America &amp; Caribbean </td>
   <td style="text-align:left;"> Americas </td>
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
   <td style="text-align:left;"> Oman </td>
   <td style="text-align:left;"> OMN </td>
   <td style="text-align:right;"> 2010 </td>
   <td style="text-align:right;"> 0.2010436 </td>
   <td style="text-align:left;"> Middle East &amp; North Africa </td>
   <td style="text-align:left;"> Asia </td>
  </tr>
  <tr>
   <td style="text-align:left;"> Sao Tome and Principe </td>
   <td style="text-align:left;"> STP </td>
   <td style="text-align:right;"> 1991 </td>
   <td style="text-align:right;"> 16.0809855 </td>
   <td style="text-align:left;"> Sub-Saharan Africa </td>
   <td style="text-align:left;"> Africa </td>
  </tr>
  <tr>
   <td style="text-align:left;"> Qatar </td>
   <td style="text-align:left;"> QAT </td>
   <td style="text-align:right;"> 2018 </td>
   <td style="text-align:right;"> 0.0010865 </td>
   <td style="text-align:left;"> Middle East &amp; North Africa </td>
   <td style="text-align:left;"> Asia </td>
  </tr>
  <tr>
   <td style="text-align:left;"> Tokelau </td>
   <td style="text-align:left;"> TKL </td>
   <td style="text-align:right;"> 2015 </td>
   <td style="text-align:right;"> 0.0077971 </td>
   <td style="text-align:left;"> East Asia &amp; Pacific </td>
   <td style="text-align:left;"> Oceania </td>
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
   <td style="text-align:left;"> Israel </td>
   <td style="text-align:left;"> ISR </td>
   <td style="text-align:right;"> 1991 </td>
   <td style="text-align:right;"> 0.0473004 </td>
   <td style="text-align:left;"> Middle East &amp; North Africa </td>
   <td style="text-align:left;"> Asia </td>
  </tr>
  <tr>
   <td style="text-align:left;"> United Arab Emirates </td>
   <td style="text-align:left;"> ARE </td>
   <td style="text-align:right;"> 2001 </td>
   <td style="text-align:right;"> 0.0070218 </td>
   <td style="text-align:left;"> Middle East &amp; North Africa </td>
   <td style="text-align:left;"> Asia </td>
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
   <td style="text-align:left;"> Belgium </td>
   <td style="text-align:left;"> BEL </td>
   <td style="text-align:right;"> 2018 </td>
   <td style="text-align:right;"> 0.0052752 </td>
   <td style="text-align:left;"> Europe &amp; Central Asia </td>
   <td style="text-align:left;"> Europe </td>
  </tr>
  <tr>
   <td style="text-align:left;"> Malaysia </td>
   <td style="text-align:left;"> MYS </td>
   <td style="text-align:right;"> 2013 </td>
   <td style="text-align:right;"> 0.1026064 </td>
   <td style="text-align:left;"> East Asia &amp; Pacific </td>
   <td style="text-align:left;"> Asia </td>
  </tr>
  <tr>
   <td style="text-align:left;"> Angola </td>
   <td style="text-align:left;"> AGO </td>
   <td style="text-align:right;"> 2016 </td>
   <td style="text-align:right;"> 6.8341990 </td>
   <td style="text-align:left;"> Sub-Saharan Africa </td>
   <td style="text-align:left;"> Africa </td>
  </tr>
  <tr>
   <td style="text-align:left;"> Slovenia </td>
   <td style="text-align:left;"> SVN </td>
   <td style="text-align:right;"> 2001 </td>
   <td style="text-align:right;"> 0.7123592 </td>
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
   <td style="text-align:left;"> Albania </td>
   <td style="text-align:left;"> ALB </td>
   <td style="text-align:right;"> 2019 </td>
   <td style="text-align:right;"> 3.0463064 </td>
   <td style="text-align:left;"> Europe &amp; Central Asia </td>
   <td style="text-align:left;"> Europe </td>
  </tr>
  <tr>
   <td style="text-align:left;"> Estonia </td>
   <td style="text-align:left;"> EST </td>
   <td style="text-align:right;"> 2014 </td>
   <td style="text-align:right;"> 0.3265481 </td>
   <td style="text-align:left;"> Europe &amp; Central Asia </td>
   <td style="text-align:left;"> Europe </td>
  </tr>
  <tr>
   <td style="text-align:left;"> Tuvalu </td>
   <td style="text-align:left;"> TUV </td>
   <td style="text-align:right;"> 2005 </td>
   <td style="text-align:right;"> 4.6452515 </td>
   <td style="text-align:left;"> East Asia &amp; Pacific </td>
   <td style="text-align:left;"> Oceania </td>
  </tr>
  <tr>
   <td style="text-align:left;"> Solomon Islands </td>
   <td style="text-align:left;"> SLB </td>
   <td style="text-align:right;"> 2003 </td>
   <td style="text-align:right;"> 21.9630674 </td>
   <td style="text-align:left;"> East Asia &amp; Pacific </td>
   <td style="text-align:left;"> Oceania </td>
  </tr>
  <tr>
   <td style="text-align:left;"> Cape Verde </td>
   <td style="text-align:left;"> CPV </td>
   <td style="text-align:right;"> 2003 </td>
   <td style="text-align:right;"> 7.8359397 </td>
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
   <td style="text-align:left;"> Malawi </td>
   <td style="text-align:left;"> MWI </td>
   <td style="text-align:right;"> 1992 </td>
   <td style="text-align:right;"> 11.1352863 </td>
   <td style="text-align:left;"> Sub-Saharan Africa </td>
   <td style="text-align:left;"> Africa </td>
  </tr>
  <tr>
   <td style="text-align:left;"> Myanmar </td>
   <td style="text-align:left;"> MMR </td>
   <td style="text-align:right;"> 2009 </td>
   <td style="text-align:right;"> 14.0682215 </td>
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
   <td style="text-align:left;"> Dominica </td>
   <td style="text-align:left;"> DMA </td>
   <td style="text-align:right;"> 2009 </td>
   <td style="text-align:right;"> 0.9435069 </td>
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
   <td style="text-align:left;"> Lithuania </td>
   <td style="text-align:left;"> LTU </td>
   <td style="text-align:right;"> 2015 </td>
   <td style="text-align:right;"> 0.1921743 </td>
   <td style="text-align:left;"> Europe &amp; Central Asia </td>
   <td style="text-align:left;"> Europe </td>
  </tr>
  <tr>
   <td style="text-align:left;"> Oman </td>
   <td style="text-align:left;"> OMN </td>
   <td style="text-align:right;"> 2005 </td>
   <td style="text-align:right;"> 0.4672549 </td>
   <td style="text-align:left;"> Middle East &amp; North Africa </td>
   <td style="text-align:left;"> Asia </td>
  </tr>
  <tr>
   <td style="text-align:left;"> Bermuda </td>
   <td style="text-align:left;"> BMU </td>
   <td style="text-align:right;"> 1995 </td>
   <td style="text-align:right;"> 0.8681930 </td>
   <td style="text-align:left;"> North America </td>
   <td style="text-align:left;"> Americas </td>
  </tr>
  <tr>
   <td style="text-align:left;"> Austria </td>
   <td style="text-align:left;"> AUT </td>
   <td style="text-align:right;"> 2017 </td>
   <td style="text-align:right;"> 0.0115418 </td>
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
   <td style="text-align:left;"> Puerto Rico </td>
   <td style="text-align:left;"> PRI </td>
   <td style="text-align:right;"> 1993 </td>
   <td style="text-align:right;"> 0.0073727 </td>
   <td style="text-align:left;"> Latin America &amp; Caribbean </td>
   <td style="text-align:left;"> Americas </td>
  </tr>
  <tr>
   <td style="text-align:left;"> Colombia </td>
   <td style="text-align:left;"> COL </td>
   <td style="text-align:right;"> 1994 </td>
   <td style="text-align:right;"> 4.7934913 </td>
   <td style="text-align:left;"> Latin America &amp; Caribbean </td>
   <td style="text-align:left;"> Americas </td>
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
   <td style="text-align:left;"> Belize </td>
   <td style="text-align:left;"> BLZ </td>
   <td style="text-align:right;"> 1997 </td>
   <td style="text-align:right;"> 4.4190986 </td>
   <td style="text-align:left;"> Latin America &amp; Caribbean </td>
   <td style="text-align:left;"> Americas </td>
  </tr>
  <tr>
   <td style="text-align:left;"> Bangladesh </td>
   <td style="text-align:left;"> BGD </td>
   <td style="text-align:right;"> 2003 </td>
   <td style="text-align:right;"> 16.8014936 </td>
   <td style="text-align:left;"> South Asia </td>
   <td style="text-align:left;"> Asia </td>
  </tr>
  <tr>
   <td style="text-align:left;"> Russia </td>
   <td style="text-align:left;"> RUS </td>
   <td style="text-align:right;"> 2001 </td>
   <td style="text-align:right;"> 0.5077678 </td>
   <td style="text-align:left;"> Europe &amp; Central Asia </td>
   <td style="text-align:left;"> Europe </td>
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
   <td style="text-align:left;"> Uruguay </td>
   <td style="text-align:left;"> URY </td>
   <td style="text-align:right;"> 1999 </td>
   <td style="text-align:right;"> 0.5556986 </td>
   <td style="text-align:left;"> Latin America &amp; Caribbean </td>
   <td style="text-align:left;"> Americas </td>
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
   <td style="text-align:left;"> Mali </td>
   <td style="text-align:left;"> MLI </td>
   <td style="text-align:right;"> 2006 </td>
   <td style="text-align:right;"> 12.1924097 </td>
   <td style="text-align:left;"> Sub-Saharan Africa </td>
   <td style="text-align:left;"> Africa </td>
  </tr>
  <tr>
   <td style="text-align:left;"> Brunei </td>
   <td style="text-align:left;"> BRN </td>
   <td style="text-align:right;"> 2016 </td>
   <td style="text-align:right;"> 0.0473346 </td>
   <td style="text-align:left;"> East Asia &amp; Pacific </td>
   <td style="text-align:left;"> Asia </td>
  </tr>
  <tr>
   <td style="text-align:left;"> Iran </td>
   <td style="text-align:left;"> IRN </td>
   <td style="text-align:right;"> 2015 </td>
   <td style="text-align:right;"> 0.0435173 </td>
   <td style="text-align:left;"> Middle East &amp; North Africa </td>
   <td style="text-align:left;"> Asia </td>
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
   <td style="text-align:left;"> Zimbabwe </td>
   <td style="text-align:left;"> ZWE </td>
   <td style="text-align:right;"> 1994 </td>
   <td style="text-align:right;"> 7.5206205 </td>
   <td style="text-align:left;"> Sub-Saharan Africa </td>
   <td style="text-align:left;"> Africa </td>
  </tr>
  <tr>
   <td style="text-align:left;"> Namibia </td>
   <td style="text-align:left;"> NAM </td>
   <td style="text-align:right;"> 2011 </td>
   <td style="text-align:right;"> 6.0396492 </td>
   <td style="text-align:left;"> Sub-Saharan Africa </td>
   <td style="text-align:left;"> Africa </td>
  </tr>
  <tr>
   <td style="text-align:left;"> Gabon </td>
   <td style="text-align:left;"> GAB </td>
   <td style="text-align:right;"> 2003 </td>
   <td style="text-align:right;"> 3.4103603 </td>
   <td style="text-align:left;"> Sub-Saharan Africa </td>
   <td style="text-align:left;"> Africa </td>
  </tr>
  <tr>
   <td style="text-align:left;"> Saint Vincent and the Grenadines </td>
   <td style="text-align:left;"> VCT </td>
   <td style="text-align:right;"> 2015 </td>
   <td style="text-align:right;"> 0.5232985 </td>
   <td style="text-align:left;"> Latin America &amp; Caribbean </td>
   <td style="text-align:left;"> Americas </td>
  </tr>
  <tr>
   <td style="text-align:left;"> Guyana </td>
   <td style="text-align:left;"> GUY </td>
   <td style="text-align:right;"> 1991 </td>
   <td style="text-align:right;"> 4.4690086 </td>
   <td style="text-align:left;"> Latin America &amp; Caribbean </td>
   <td style="text-align:left;"> Americas </td>
  </tr>
  <tr>
   <td style="text-align:left;"> Malaysia </td>
   <td style="text-align:left;"> MYS </td>
   <td style="text-align:right;"> 2001 </td>
   <td style="text-align:right;"> 0.2608086 </td>
   <td style="text-align:left;"> East Asia &amp; Pacific </td>
   <td style="text-align:left;"> Asia </td>
  </tr>
  <tr>
   <td style="text-align:left;"> United Arab Emirates </td>
   <td style="text-align:left;"> ARE </td>
   <td style="text-align:right;"> 2005 </td>
   <td style="text-align:right;"> 0.0031221 </td>
   <td style="text-align:left;"> Middle East &amp; North Africa </td>
   <td style="text-align:left;"> Asia </td>
  </tr>
  <tr>
   <td style="text-align:left;"> Albania </td>
   <td style="text-align:left;"> ALB </td>
   <td style="text-align:right;"> 2012 </td>
   <td style="text-align:right;"> 4.1183613 </td>
   <td style="text-align:left;"> Europe &amp; Central Asia </td>
   <td style="text-align:left;"> Europe </td>
  </tr>
  <tr>
   <td style="text-align:left;"> Italy </td>
   <td style="text-align:left;"> ITA </td>
   <td style="text-align:right;"> 2006 </td>
   <td style="text-align:right;"> 0.0399439 </td>
   <td style="text-align:left;"> Europe &amp; Central Asia </td>
   <td style="text-align:left;"> Europe </td>
  </tr>
  <tr>
   <td style="text-align:left;"> Bermuda </td>
   <td style="text-align:left;"> BMU </td>
   <td style="text-align:right;"> 2016 </td>
   <td style="text-align:right;"> 0.1465764 </td>
   <td style="text-align:left;"> North America </td>
   <td style="text-align:left;"> Americas </td>
  </tr>
  <tr>
   <td style="text-align:left;"> Philippines </td>
   <td style="text-align:left;"> PHL </td>
   <td style="text-align:right;"> 2002 </td>
   <td style="text-align:right;"> 9.1272339 </td>
   <td style="text-align:left;"> East Asia &amp; Pacific </td>
   <td style="text-align:left;"> Asia </td>
  </tr>
  <tr>
   <td style="text-align:left;"> Serbia </td>
   <td style="text-align:left;"> SRB </td>
   <td style="text-align:right;"> 2003 </td>
   <td style="text-align:right;"> 3.9410108 </td>
   <td style="text-align:left;"> Europe &amp; Central Asia </td>
   <td style="text-align:left;"> Europe </td>
  </tr>
  <tr>
   <td style="text-align:left;"> Sudan </td>
   <td style="text-align:left;"> SDN </td>
   <td style="text-align:right;"> 2008 </td>
   <td style="text-align:right;"> 12.1821287 </td>
   <td style="text-align:left;"> Sub-Saharan Africa </td>
   <td style="text-align:left;"> Africa </td>
  </tr>
  <tr>
   <td style="text-align:left;"> Chad </td>
   <td style="text-align:left;"> TCD </td>
   <td style="text-align:right;"> 1996 </td>
   <td style="text-align:right;"> 14.7100376 </td>
   <td style="text-align:left;"> Sub-Saharan Africa </td>
   <td style="text-align:left;"> Africa </td>
  </tr>
  <tr>
   <td style="text-align:left;"> Azerbaijan </td>
   <td style="text-align:left;"> AZE </td>
   <td style="text-align:right;"> 2005 </td>
   <td style="text-align:right;"> 3.3649091 </td>
   <td style="text-align:left;"> Europe &amp; Central Asia </td>
   <td style="text-align:left;"> Asia </td>
  </tr>
  <tr>
   <td style="text-align:left;"> Saint Kitts and Nevis </td>
   <td style="text-align:left;"> KNA </td>
   <td style="text-align:right;"> 2016 </td>
   <td style="text-align:right;"> 0.2097574 </td>
   <td style="text-align:left;"> Latin America &amp; Caribbean </td>
   <td style="text-align:left;"> Americas </td>
  </tr>
  <tr>
   <td style="text-align:left;"> Saint Vincent and the Grenadines </td>
   <td style="text-align:left;"> VCT </td>
   <td style="text-align:right;"> 2019 </td>
   <td style="text-align:right;"> 0.4202021 </td>
   <td style="text-align:left;"> Latin America &amp; Caribbean </td>
   <td style="text-align:left;"> Americas </td>
  </tr>
  <tr>
   <td style="text-align:left;"> Kenya </td>
   <td style="text-align:left;"> KEN </td>
   <td style="text-align:right;"> 2016 </td>
   <td style="text-align:right;"> 8.6619046 </td>
   <td style="text-align:left;"> Sub-Saharan Africa </td>
   <td style="text-align:left;"> Africa </td>
  </tr>
  <tr>
   <td style="text-align:left;"> Djibouti </td>
   <td style="text-align:left;"> DJI </td>
   <td style="text-align:right;"> 1993 </td>
   <td style="text-align:right;"> 9.5395769 </td>
   <td style="text-align:left;"> Middle East &amp; North Africa </td>
   <td style="text-align:left;"> Africa </td>
  </tr>
  <tr>
   <td style="text-align:left;"> Panama </td>
   <td style="text-align:left;"> PAN </td>
   <td style="text-align:right;"> 2016 </td>
   <td style="text-align:right;"> 1.0208518 </td>
   <td style="text-align:left;"> Latin America &amp; Caribbean </td>
   <td style="text-align:left;"> Americas </td>
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
   <td style="text-align:left;"> Laos </td>
   <td style="text-align:left;"> LAO </td>
   <td style="text-align:right;"> 2019 </td>
   <td style="text-align:right;"> 14.9863711 </td>
   <td style="text-align:left;"> East Asia &amp; Pacific </td>
   <td style="text-align:left;"> Asia </td>
  </tr>
  <tr>
   <td style="text-align:left;"> Northern Mariana Islands </td>
   <td style="text-align:left;"> MNP </td>
   <td style="text-align:right;"> 1990 </td>
   <td style="text-align:right;"> 2.9322304 </td>
   <td style="text-align:left;"> East Asia &amp; Pacific </td>
   <td style="text-align:left;"> Oceania </td>
  </tr>
  <tr>
   <td style="text-align:left;"> Saudi Arabia </td>
   <td style="text-align:left;"> SAU </td>
   <td style="text-align:right;"> 2018 </td>
   <td style="text-align:right;"> 0.0353510 </td>
   <td style="text-align:left;"> Middle East &amp; North Africa </td>
   <td style="text-align:left;"> Asia </td>
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
   <td style="text-align:left;"> Liberia </td>
   <td style="text-align:left;"> LBR </td>
   <td style="text-align:right;"> 2004 </td>
   <td style="text-align:right;"> 11.5107476 </td>
   <td style="text-align:left;"> Sub-Saharan Africa </td>
   <td style="text-align:left;"> Africa </td>
  </tr>
  <tr>
   <td style="text-align:left;"> Vietnam </td>
   <td style="text-align:left;"> VNM </td>
   <td style="text-align:right;"> 1997 </td>
   <td style="text-align:right;"> 13.2506171 </td>
   <td style="text-align:left;"> East Asia &amp; Pacific </td>
   <td style="text-align:left;"> Asia </td>
  </tr>
  <tr>
   <td style="text-align:left;"> Myanmar </td>
   <td style="text-align:left;"> MMR </td>
   <td style="text-align:right;"> 2000 </td>
   <td style="text-align:right;"> 17.5459198 </td>
   <td style="text-align:left;"> East Asia &amp; Pacific </td>
   <td style="text-align:left;"> Asia </td>
  </tr>
  <tr>
   <td style="text-align:left;"> Uzbekistan </td>
   <td style="text-align:left;"> UZB </td>
   <td style="text-align:right;"> 1998 </td>
   <td style="text-align:right;"> 5.4015177 </td>
   <td style="text-align:left;"> Europe &amp; Central Asia </td>
   <td style="text-align:left;"> Asia </td>
  </tr>
  <tr>
   <td style="text-align:left;"> Syria </td>
   <td style="text-align:left;"> SYR </td>
   <td style="text-align:right;"> 2009 </td>
   <td style="text-align:right;"> 0.0668088 </td>
   <td style="text-align:left;"> Middle East &amp; North Africa </td>
   <td style="text-align:left;"> Asia </td>
  </tr>
  <tr>
   <td style="text-align:left;"> Bhutan </td>
   <td style="text-align:left;"> BTN </td>
   <td style="text-align:right;"> 2004 </td>
   <td style="text-align:right;"> 14.7760379 </td>
   <td style="text-align:left;"> South Asia </td>
   <td style="text-align:left;"> Asia </td>
  </tr>
  <tr>
   <td style="text-align:left;"> Saint Vincent and the Grenadines </td>
   <td style="text-align:left;"> VCT </td>
   <td style="text-align:right;"> 2008 </td>
   <td style="text-align:right;"> 0.7631425 </td>
   <td style="text-align:left;"> Latin America &amp; Caribbean </td>
   <td style="text-align:left;"> Americas </td>
  </tr>
  <tr>
   <td style="text-align:left;"> Eswatini </td>
   <td style="text-align:left;"> SWZ </td>
   <td style="text-align:right;"> 1997 </td>
   <td style="text-align:right;"> 8.5160722 </td>
   <td style="text-align:left;"> Sub-Saharan Africa </td>
   <td style="text-align:left;"> Africa </td>
  </tr>
  <tr>
   <td style="text-align:left;"> Czechia </td>
   <td style="text-align:left;"> CZE </td>
   <td style="text-align:right;"> 2010 </td>
   <td style="text-align:right;"> 0.0990454 </td>
   <td style="text-align:left;"> Europe &amp; Central Asia </td>
   <td style="text-align:left;"> Europe </td>
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
   <td style="text-align:left;"> Laos </td>
   <td style="text-align:left;"> LAO </td>
   <td style="text-align:right;"> 2012 </td>
   <td style="text-align:right;"> 16.1311491 </td>
   <td style="text-align:left;"> East Asia &amp; Pacific </td>
   <td style="text-align:left;"> Asia </td>
  </tr>
  <tr>
   <td style="text-align:left;"> South Korea </td>
   <td style="text-align:left;"> KOR </td>
   <td style="text-align:right;"> 2009 </td>
   <td style="text-align:right;"> 0.0102636 </td>
   <td style="text-align:left;"> East Asia &amp; Pacific </td>
   <td style="text-align:left;"> Asia </td>
  </tr>
  <tr>
   <td style="text-align:left;"> Slovenia </td>
   <td style="text-align:left;"> SVN </td>
   <td style="text-align:right;"> 2007 </td>
   <td style="text-align:right;"> 0.4956056 </td>
   <td style="text-align:left;"> Europe &amp; Central Asia </td>
   <td style="text-align:left;"> Europe </td>
  </tr>
  <tr>
   <td style="text-align:left;"> Congo </td>
   <td style="text-align:left;"> COG </td>
   <td style="text-align:right;"> 2009 </td>
   <td style="text-align:right;"> 7.9257128 </td>
   <td style="text-align:left;"> Sub-Saharan Africa </td>
   <td style="text-align:left;"> Africa </td>
  </tr>
  <tr>
   <td style="text-align:left;"> Tonga </td>
   <td style="text-align:left;"> TON </td>
   <td style="text-align:right;"> 2008 </td>
   <td style="text-align:right;"> 6.9587117 </td>
   <td style="text-align:left;"> East Asia &amp; Pacific </td>
   <td style="text-align:left;"> Oceania </td>
  </tr>
  <tr>
   <td style="text-align:left;"> Paraguay </td>
   <td style="text-align:left;"> PRY </td>
   <td style="text-align:right;"> 2017 </td>
   <td style="text-align:right;"> 4.0729324 </td>
   <td style="text-align:left;"> Latin America &amp; Caribbean </td>
   <td style="text-align:left;"> Americas </td>
  </tr>
  <tr>
   <td style="text-align:left;"> Bosnia and Herzegovina </td>
   <td style="text-align:left;"> BIH </td>
   <td style="text-align:right;"> 2015 </td>
   <td style="text-align:right;"> 3.3538343 </td>
   <td style="text-align:left;"> Europe &amp; Central Asia </td>
   <td style="text-align:left;"> Europe </td>
  </tr>
  <tr>
   <td style="text-align:left;"> New Zealand </td>
   <td style="text-align:left;"> NZL </td>
   <td style="text-align:right;"> 2009 </td>
   <td style="text-align:right;"> 0.0131330 </td>
   <td style="text-align:left;"> East Asia &amp; Pacific </td>
   <td style="text-align:left;"> Oceania </td>
  </tr>
  <tr>
   <td style="text-align:left;"> Yemen </td>
   <td style="text-align:left;"> YEM </td>
   <td style="text-align:right;"> 2003 </td>
   <td style="text-align:right;"> 11.9609212 </td>
   <td style="text-align:left;"> Middle East &amp; North Africa </td>
   <td style="text-align:left;"> Asia </td>
  </tr>
  <tr>
   <td style="text-align:left;"> Thailand </td>
   <td style="text-align:left;"> THA </td>
   <td style="text-align:right;"> 2004 </td>
   <td style="text-align:right;"> 3.4814463 </td>
   <td style="text-align:left;"> East Asia &amp; Pacific </td>
   <td style="text-align:left;"> Asia </td>
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
   <td style="text-align:left;"> Maldives </td>
   <td style="text-align:left;"> MDV </td>
   <td style="text-align:right;"> 2006 </td>
   <td style="text-align:right;"> 5.1622309 </td>
   <td style="text-align:left;"> South Asia </td>
   <td style="text-align:left;"> Asia </td>
  </tr>
  <tr>
   <td style="text-align:left;"> Malawi </td>
   <td style="text-align:left;"> MWI </td>
   <td style="text-align:right;"> 1999 </td>
   <td style="text-align:right;"> 9.4260724 </td>
   <td style="text-align:left;"> Sub-Saharan Africa </td>
   <td style="text-align:left;"> Africa </td>
  </tr>
  <tr>
   <td style="text-align:left;"> North Korea </td>
   <td style="text-align:left;"> PRK </td>
   <td style="text-align:right;"> 2006 </td>
   <td style="text-align:right;"> 17.7001957 </td>
   <td style="text-align:left;"> East Asia &amp; Pacific </td>
   <td style="text-align:left;"> Asia </td>
  </tr>
  <tr>
   <td style="text-align:left;"> Sudan </td>
   <td style="text-align:left;"> SDN </td>
   <td style="text-align:right;"> 2010 </td>
   <td style="text-align:right;"> 11.1772441 </td>
   <td style="text-align:left;"> Sub-Saharan Africa </td>
   <td style="text-align:left;"> Africa </td>
  </tr>
  <tr>
   <td style="text-align:left;"> Tunisia </td>
   <td style="text-align:left;"> TUN </td>
   <td style="text-align:right;"> 1994 </td>
   <td style="text-align:right;"> 1.6807828 </td>
   <td style="text-align:left;"> Middle East &amp; North Africa </td>
   <td style="text-align:left;"> Africa </td>
  </tr>
  <tr>
   <td style="text-align:left;"> Turkey </td>
   <td style="text-align:left;"> TUR </td>
   <td style="text-align:right;"> 2004 </td>
   <td style="text-align:right;"> 0.3501356 </td>
   <td style="text-align:left;"> Europe &amp; Central Asia </td>
   <td style="text-align:left;"> Asia </td>
  </tr>
  <tr>
   <td style="text-align:left;"> Latvia </td>
   <td style="text-align:left;"> LVA </td>
   <td style="text-align:right;"> 1995 </td>
   <td style="text-align:right;"> 1.0810986 </td>
   <td style="text-align:left;"> Europe &amp; Central Asia </td>
   <td style="text-align:left;"> Europe </td>
  </tr>
  <tr>
   <td style="text-align:left;"> Cuba </td>
   <td style="text-align:left;"> CUB </td>
   <td style="text-align:right;"> 2017 </td>
   <td style="text-align:right;"> 0.2170814 </td>
   <td style="text-align:left;"> Latin America &amp; Caribbean </td>
   <td style="text-align:left;"> Americas </td>
  </tr>
  <tr>
   <td style="text-align:left;"> Afghanistan </td>
   <td style="text-align:left;"> AFG </td>
   <td style="text-align:right;"> 1993 </td>
   <td style="text-align:right;"> 19.6776071 </td>
   <td style="text-align:left;"> South Asia </td>
   <td style="text-align:left;"> Asia </td>
  </tr>
  <tr>
   <td style="text-align:left;"> Mauritius </td>
   <td style="text-align:left;"> MUS </td>
   <td style="text-align:right;"> 2010 </td>
   <td style="text-align:right;"> 0.3198540 </td>
   <td style="text-align:left;"> Sub-Saharan Africa </td>
   <td style="text-align:left;"> Africa </td>
  </tr>
  <tr>
   <td style="text-align:left;"> Seychelles </td>
   <td style="text-align:left;"> SYC </td>
   <td style="text-align:right;"> 2009 </td>
   <td style="text-align:right;"> 0.1033891 </td>
   <td style="text-align:left;"> Sub-Saharan Africa </td>
   <td style="text-align:left;"> Africa </td>
  </tr>
  <tr>
   <td style="text-align:left;"> Cuba </td>
   <td style="text-align:left;"> CUB </td>
   <td style="text-align:right;"> 1996 </td>
   <td style="text-align:right;"> 0.7815995 </td>
   <td style="text-align:left;"> Latin America &amp; Caribbean </td>
   <td style="text-align:left;"> Americas </td>
  </tr>
  <tr>
   <td style="text-align:left;"> Bangladesh </td>
   <td style="text-align:left;"> BGD </td>
   <td style="text-align:right;"> 2011 </td>
   <td style="text-align:right;"> 15.1435969 </td>
   <td style="text-align:left;"> South Asia </td>
   <td style="text-align:left;"> Asia </td>
  </tr>
  <tr>
   <td style="text-align:left;"> Ireland </td>
   <td style="text-align:left;"> IRL </td>
   <td style="text-align:right;"> 2013 </td>
   <td style="text-align:right;"> 0.0092801 </td>
   <td style="text-align:left;"> Europe &amp; Central Asia </td>
   <td style="text-align:left;"> Europe </td>
  </tr>
  <tr>
   <td style="text-align:left;"> Lithuania </td>
   <td style="text-align:left;"> LTU </td>
   <td style="text-align:right;"> 1991 </td>
   <td style="text-align:right;"> 1.0936610 </td>
   <td style="text-align:left;"> Europe &amp; Central Asia </td>
   <td style="text-align:left;"> Europe </td>
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
   <td style="text-align:left;"> Poland </td>
   <td style="text-align:left;"> POL </td>
   <td style="text-align:right;"> 1995 </td>
   <td style="text-align:right;"> 2.5203339 </td>
   <td style="text-align:left;"> Europe &amp; Central Asia </td>
   <td style="text-align:left;"> Europe </td>
  </tr>
  <tr>
   <td style="text-align:left;"> Croatia </td>
   <td style="text-align:left;"> HRV </td>
   <td style="text-align:right;"> 2009 </td>
   <td style="text-align:right;"> 0.5318899 </td>
   <td style="text-align:left;"> Europe &amp; Central Asia </td>
   <td style="text-align:left;"> Europe </td>
  </tr>
  <tr>
   <td style="text-align:left;"> Greenland </td>
   <td style="text-align:left;"> GRL </td>
   <td style="text-align:right;"> 2005 </td>
   <td style="text-align:right;"> 0.1988710 </td>
   <td style="text-align:left;"> Europe &amp; Central Asia </td>
   <td style="text-align:left;"> Americas </td>
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
   <td style="text-align:left;"> Australia </td>
   <td style="text-align:left;"> AUS </td>
   <td style="text-align:right;"> 1999 </td>
   <td style="text-align:right;"> 0.0486500 </td>
   <td style="text-align:left;"> East Asia &amp; Pacific </td>
   <td style="text-align:left;"> Oceania </td>
  </tr>
  <tr>
   <td style="text-align:left;"> Czechia </td>
   <td style="text-align:left;"> CZE </td>
   <td style="text-align:right;"> 2017 </td>
   <td style="text-align:right;"> 0.0750907 </td>
   <td style="text-align:left;"> Europe &amp; Central Asia </td>
   <td style="text-align:left;"> Europe </td>
  </tr>
  <tr>
   <td style="text-align:left;"> Madagascar </td>
   <td style="text-align:left;"> MDG </td>
   <td style="text-align:right;"> 2013 </td>
   <td style="text-align:right;"> 15.6028482 </td>
   <td style="text-align:left;"> Sub-Saharan Africa </td>
   <td style="text-align:left;"> Africa </td>
  </tr>
  <tr>
   <td style="text-align:left;"> Kuwait </td>
   <td style="text-align:left;"> KWT </td>
   <td style="text-align:right;"> 1998 </td>
   <td style="text-align:right;"> 0.0885149 </td>
   <td style="text-align:left;"> Middle East &amp; North Africa </td>
   <td style="text-align:left;"> Asia </td>
  </tr>
  <tr>
   <td style="text-align:left;"> Cuba </td>
   <td style="text-align:left;"> CUB </td>
   <td style="text-align:right;"> 2003 </td>
   <td style="text-align:right;"> 0.5503800 </td>
   <td style="text-align:left;"> Latin America &amp; Caribbean </td>
   <td style="text-align:left;"> Americas </td>
  </tr>
  <tr>
   <td style="text-align:left;"> Barbados </td>
   <td style="text-align:left;"> BRB </td>
   <td style="text-align:right;"> 2015 </td>
   <td style="text-align:right;"> 0.0080867 </td>
   <td style="text-align:left;"> Latin America &amp; Caribbean </td>
   <td style="text-align:left;"> Americas </td>
  </tr>
  <tr>
   <td style="text-align:left;"> Antigua and Barbuda </td>
   <td style="text-align:left;"> ATG </td>
   <td style="text-align:right;"> 2014 </td>
   <td style="text-align:right;"> 0.1211412 </td>
   <td style="text-align:left;"> Latin America &amp; Caribbean </td>
   <td style="text-align:left;"> Americas </td>
  </tr>
  <tr>
   <td style="text-align:left;"> Kiribati </td>
   <td style="text-align:left;"> KIR </td>
   <td style="text-align:right;"> 2007 </td>
   <td style="text-align:right;"> 13.1294423 </td>
   <td style="text-align:left;"> East Asia &amp; Pacific </td>
   <td style="text-align:left;"> Oceania </td>
  </tr>
  <tr>
   <td style="text-align:left;"> Tokelau </td>
   <td style="text-align:left;"> TKL </td>
   <td style="text-align:right;"> 2005 </td>
   <td style="text-align:right;"> 0.0191271 </td>
   <td style="text-align:left;"> East Asia &amp; Pacific </td>
   <td style="text-align:left;"> Oceania </td>
  </tr>
  <tr>
   <td style="text-align:left;"> Yemen </td>
   <td style="text-align:left;"> YEM </td>
   <td style="text-align:right;"> 2014 </td>
   <td style="text-align:right;"> 8.1044221 </td>
   <td style="text-align:left;"> Middle East &amp; North Africa </td>
   <td style="text-align:left;"> Asia </td>
  </tr>
  <tr>
   <td style="text-align:left;"> Malta </td>
   <td style="text-align:left;"> MLT </td>
   <td style="text-align:right;"> 1990 </td>
   <td style="text-align:right;"> 0.1463832 </td>
   <td style="text-align:left;"> Middle East &amp; North Africa </td>
   <td style="text-align:left;"> Europe </td>
  </tr>
  <tr>
   <td style="text-align:left;"> Benin </td>
   <td style="text-align:left;"> BEN </td>
   <td style="text-align:right;"> 2009 </td>
   <td style="text-align:right;"> 13.3087053 </td>
   <td style="text-align:left;"> Sub-Saharan Africa </td>
   <td style="text-align:left;"> Africa </td>
  </tr>
  <tr>
   <td style="text-align:left;"> Tokelau </td>
   <td style="text-align:left;"> TKL </td>
   <td style="text-align:right;"> 2006 </td>
   <td style="text-align:right;"> 0.0175172 </td>
   <td style="text-align:left;"> East Asia &amp; Pacific </td>
   <td style="text-align:left;"> Oceania </td>
  </tr>
  <tr>
   <td style="text-align:left;"> Tajikistan </td>
   <td style="text-align:left;"> TJK </td>
   <td style="text-align:right;"> 1996 </td>
   <td style="text-align:right;"> 12.6728700 </td>
   <td style="text-align:left;"> Europe &amp; Central Asia </td>
   <td style="text-align:left;"> Asia </td>
  </tr>
  <tr>
   <td style="text-align:left;"> Trinidad and Tobago </td>
   <td style="text-align:left;"> TTO </td>
   <td style="text-align:right;"> 1993 </td>
   <td style="text-align:right;"> 0.1137077 </td>
   <td style="text-align:left;"> Latin America &amp; Caribbean </td>
   <td style="text-align:left;"> Americas </td>
  </tr>
  <tr>
   <td style="text-align:left;"> Liberia </td>
   <td style="text-align:left;"> LBR </td>
   <td style="text-align:right;"> 1993 </td>
   <td style="text-align:right;"> 12.1968391 </td>
   <td style="text-align:left;"> Sub-Saharan Africa </td>
   <td style="text-align:left;"> Africa </td>
  </tr>
  <tr>
   <td style="text-align:left;"> Singapore </td>
   <td style="text-align:left;"> SGP </td>
   <td style="text-align:right;"> 1994 </td>
   <td style="text-align:right;"> 0.2605330 </td>
   <td style="text-align:left;"> East Asia &amp; Pacific </td>
   <td style="text-align:left;"> Asia </td>
  </tr>
  <tr>
   <td style="text-align:left;"> Turkmenistan </td>
   <td style="text-align:left;"> TKM </td>
   <td style="text-align:right;"> 1999 </td>
   <td style="text-align:right;"> 0.1088288 </td>
   <td style="text-align:left;"> Europe &amp; Central Asia </td>
   <td style="text-align:left;"> Asia </td>
  </tr>
  <tr>
   <td style="text-align:left;"> Netherlands </td>
   <td style="text-align:left;"> NLD </td>
   <td style="text-align:right;"> 2019 </td>
   <td style="text-align:right;"> 0.0035662 </td>
   <td style="text-align:left;"> Europe &amp; Central Asia </td>
   <td style="text-align:left;"> Europe </td>
  </tr>
  <tr>
   <td style="text-align:left;"> Greenland </td>
   <td style="text-align:left;"> GRL </td>
   <td style="text-align:right;"> 2018 </td>
   <td style="text-align:right;"> 0.0747349 </td>
   <td style="text-align:left;"> Europe &amp; Central Asia </td>
   <td style="text-align:left;"> Americas </td>
  </tr>
  <tr>
   <td style="text-align:left;"> Finland </td>
   <td style="text-align:left;"> FIN </td>
   <td style="text-align:right;"> 2015 </td>
   <td style="text-align:right;"> 0.0047693 </td>
   <td style="text-align:left;"> Europe &amp; Central Asia </td>
   <td style="text-align:left;"> Europe </td>
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
   <td style="text-align:left;"> France </td>
   <td style="text-align:left;"> FRA </td>
   <td style="text-align:right;"> 2013 </td>
   <td style="text-align:right;"> 0.0084239 </td>
   <td style="text-align:left;"> Europe &amp; Central Asia </td>
   <td style="text-align:left;"> Europe </td>
  </tr>
  <tr>
   <td style="text-align:left;"> Papua New Guinea </td>
   <td style="text-align:left;"> PNG </td>
   <td style="text-align:right;"> 2009 </td>
   <td style="text-align:right;"> 19.1313678 </td>
   <td style="text-align:left;"> East Asia &amp; Pacific </td>
   <td style="text-align:left;"> Oceania </td>
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
   <td style="text-align:left;"> Croatia </td>
   <td style="text-align:left;"> HRV </td>
   <td style="text-align:right;"> 1998 </td>
   <td style="text-align:right;"> 1.1066330 </td>
   <td style="text-align:left;"> Europe &amp; Central Asia </td>
   <td style="text-align:left;"> Europe </td>
  </tr>
  <tr>
   <td style="text-align:left;"> Laos </td>
   <td style="text-align:left;"> LAO </td>
   <td style="text-align:right;"> 2018 </td>
   <td style="text-align:right;"> 15.2346065 </td>
   <td style="text-align:left;"> East Asia &amp; Pacific </td>
   <td style="text-align:left;"> Asia </td>
  </tr>
  <tr>
   <td style="text-align:left;"> Sierra Leone </td>
   <td style="text-align:left;"> SLE </td>
   <td style="text-align:right;"> 1994 </td>
   <td style="text-align:right;"> 13.7279485 </td>
   <td style="text-align:left;"> Sub-Saharan Africa </td>
   <td style="text-align:left;"> Africa </td>
  </tr>
  <tr>
   <td style="text-align:left;"> Gambia </td>
   <td style="text-align:left;"> GMB </td>
   <td style="text-align:right;"> 2016 </td>
   <td style="text-align:right;"> 12.1438565 </td>
   <td style="text-align:left;"> Sub-Saharan Africa </td>
   <td style="text-align:left;"> Africa </td>
  </tr>
  <tr>
   <td style="text-align:left;"> Congo </td>
   <td style="text-align:left;"> COG </td>
   <td style="text-align:right;"> 2000 </td>
   <td style="text-align:right;"> 8.5950352 </td>
   <td style="text-align:left;"> Sub-Saharan Africa </td>
   <td style="text-align:left;"> Africa </td>
  </tr>
  <tr>
   <td style="text-align:left;"> Zambia </td>
   <td style="text-align:left;"> ZMB </td>
   <td style="text-align:right;"> 2014 </td>
   <td style="text-align:right;"> 8.8262945 </td>
   <td style="text-align:left;"> Sub-Saharan Africa </td>
   <td style="text-align:left;"> Africa </td>
  </tr>
  <tr>
   <td style="text-align:left;"> Pakistan </td>
   <td style="text-align:left;"> PAK </td>
   <td style="text-align:right;"> 2017 </td>
   <td style="text-align:right;"> 8.8160133 </td>
   <td style="text-align:left;"> South Asia </td>
   <td style="text-align:left;"> Asia </td>
  </tr>
  <tr>
   <td style="text-align:left;"> Italy </td>
   <td style="text-align:left;"> ITA </td>
   <td style="text-align:right;"> 2001 </td>
   <td style="text-align:right;"> 0.0588638 </td>
   <td style="text-align:left;"> Europe &amp; Central Asia </td>
   <td style="text-align:left;"> Europe </td>
  </tr>
  <tr>
   <td style="text-align:left;"> Bermuda </td>
   <td style="text-align:left;"> BMU </td>
   <td style="text-align:right;"> 1991 </td>
   <td style="text-align:right;"> 1.1686602 </td>
   <td style="text-align:left;"> North America </td>
   <td style="text-align:left;"> Americas </td>
  </tr>
  <tr>
   <td style="text-align:left;"> Haiti </td>
   <td style="text-align:left;"> HTI </td>
   <td style="text-align:right;"> 2009 </td>
   <td style="text-align:right;"> 14.2444670 </td>
   <td style="text-align:left;"> Latin America &amp; Caribbean </td>
   <td style="text-align:left;"> Americas </td>
  </tr>
  <tr>
   <td style="text-align:left;"> Philippines </td>
   <td style="text-align:left;"> PHL </td>
   <td style="text-align:right;"> 2016 </td>
   <td style="text-align:right;"> 7.9963125 </td>
   <td style="text-align:left;"> East Asia &amp; Pacific </td>
   <td style="text-align:left;"> Asia </td>
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
   <td style="text-align:left;"> Libya </td>
   <td style="text-align:left;"> LBY </td>
   <td style="text-align:right;"> 2003 </td>
   <td style="text-align:right;"> 0.2443475 </td>
   <td style="text-align:left;"> Middle East &amp; North Africa </td>
   <td style="text-align:left;"> Africa </td>
  </tr>
  <tr>
   <td style="text-align:left;"> Afghanistan </td>
   <td style="text-align:left;"> AFG </td>
   <td style="text-align:right;"> 2000 </td>
   <td style="text-align:right;"> 19.8186738 </td>
   <td style="text-align:left;"> South Asia </td>
   <td style="text-align:left;"> Asia </td>
  </tr>
  <tr>
   <td style="text-align:left;"> Albania </td>
   <td style="text-align:left;"> ALB </td>
   <td style="text-align:right;"> 1999 </td>
   <td style="text-align:right;"> 9.6431634 </td>
   <td style="text-align:left;"> Europe &amp; Central Asia </td>
   <td style="text-align:left;"> Europe </td>
  </tr>
  <tr>
   <td style="text-align:left;"> Papua New Guinea </td>
   <td style="text-align:left;"> PNG </td>
   <td style="text-align:right;"> 2003 </td>
   <td style="text-align:right;"> 19.6579131 </td>
   <td style="text-align:left;"> East Asia &amp; Pacific </td>
   <td style="text-align:left;"> Oceania </td>
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
   <td style="text-align:left;"> Australia </td>
   <td style="text-align:left;"> AUS </td>
   <td style="text-align:right;"> 1990 </td>
   <td style="text-align:right;"> 0.1189367 </td>
   <td style="text-align:left;"> East Asia &amp; Pacific </td>
   <td style="text-align:left;"> Oceania </td>
  </tr>
  <tr>
   <td style="text-align:left;"> Georgia </td>
   <td style="text-align:left;"> GEO </td>
   <td style="text-align:right;"> 2006 </td>
   <td style="text-align:right;"> 6.5788029 </td>
   <td style="text-align:left;"> Europe &amp; Central Asia </td>
   <td style="text-align:left;"> Asia </td>
  </tr>
  <tr>
   <td style="text-align:left;"> Malawi </td>
   <td style="text-align:left;"> MWI </td>
   <td style="text-align:right;"> 2004 </td>
   <td style="text-align:right;"> 9.0662063 </td>
   <td style="text-align:left;"> Sub-Saharan Africa </td>
   <td style="text-align:left;"> Africa </td>
  </tr>
  <tr>
   <td style="text-align:left;"> Indonesia </td>
   <td style="text-align:left;"> IDN </td>
   <td style="text-align:right;"> 2004 </td>
   <td style="text-align:right;"> 7.0030931 </td>
   <td style="text-align:left;"> East Asia &amp; Pacific </td>
   <td style="text-align:left;"> Asia </td>
  </tr>
  <tr>
   <td style="text-align:left;"> Dominican Republic </td>
   <td style="text-align:left;"> DOM </td>
   <td style="text-align:right;"> 1993 </td>
   <td style="text-align:right;"> 6.9172718 </td>
   <td style="text-align:left;"> Latin America &amp; Caribbean </td>
   <td style="text-align:left;"> Americas </td>
  </tr>
  <tr>
   <td style="text-align:left;"> Azerbaijan </td>
   <td style="text-align:left;"> AZE </td>
   <td style="text-align:right;"> 2008 </td>
   <td style="text-align:right;"> 2.1972342 </td>
   <td style="text-align:left;"> Europe &amp; Central Asia </td>
   <td style="text-align:left;"> Asia </td>
  </tr>
  <tr>
   <td style="text-align:left;"> Pakistan </td>
   <td style="text-align:left;"> PAK </td>
   <td style="text-align:right;"> 1992 </td>
   <td style="text-align:right;"> 14.9182562 </td>
   <td style="text-align:left;"> South Asia </td>
   <td style="text-align:left;"> Asia </td>
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
   <td style="text-align:left;"> Vietnam </td>
   <td style="text-align:left;"> VNM </td>
   <td style="text-align:right;"> 2017 </td>
   <td style="text-align:right;"> 5.8836123 </td>
   <td style="text-align:left;"> East Asia &amp; Pacific </td>
   <td style="text-align:left;"> Asia </td>
  </tr>
  <tr>
   <td style="text-align:left;"> Democratic Republic of Congo </td>
   <td style="text-align:left;"> COD </td>
   <td style="text-align:right;"> 2007 </td>
   <td style="text-align:right;"> 13.9609107 </td>
   <td style="text-align:left;"> Sub-Saharan Africa </td>
   <td style="text-align:left;"> Africa </td>
  </tr>
  <tr>
   <td style="text-align:left;"> Brunei </td>
   <td style="text-align:left;"> BRN </td>
   <td style="text-align:right;"> 1991 </td>
   <td style="text-align:right;"> 0.9742741 </td>
   <td style="text-align:left;"> East Asia &amp; Pacific </td>
   <td style="text-align:left;"> Asia </td>
  </tr>
  <tr>
   <td style="text-align:left;"> Sri Lanka </td>
   <td style="text-align:left;"> LKA </td>
   <td style="text-align:right;"> 2019 </td>
   <td style="text-align:right;"> 4.7735064 </td>
   <td style="text-align:left;"> South Asia </td>
   <td style="text-align:left;"> Asia </td>
  </tr>
  <tr>
   <td style="text-align:left;"> Burundi </td>
   <td style="text-align:left;"> BDI </td>
   <td style="text-align:right;"> 1993 </td>
   <td style="text-align:right;"> 12.4696072 </td>
   <td style="text-align:left;"> Sub-Saharan Africa </td>
   <td style="text-align:left;"> Africa </td>
  </tr>
  <tr>
   <td style="text-align:left;"> Andorra </td>
   <td style="text-align:left;"> AND </td>
   <td style="text-align:right;"> 1991 </td>
   <td style="text-align:right;"> 0.0224718 </td>
   <td style="text-align:left;"> Europe &amp; Central Asia </td>
   <td style="text-align:left;"> Europe </td>
  </tr>
  <tr>
   <td style="text-align:left;"> Ethiopia </td>
   <td style="text-align:left;"> ETH </td>
   <td style="text-align:right;"> 2002 </td>
   <td style="text-align:right;"> 13.2384288 </td>
   <td style="text-align:left;"> Sub-Saharan Africa </td>
   <td style="text-align:left;"> Africa </td>
  </tr>
  <tr>
   <td style="text-align:left;"> Nauru </td>
   <td style="text-align:left;"> NRU </td>
   <td style="text-align:right;"> 2002 </td>
   <td style="text-align:right;"> 2.3715572 </td>
   <td style="text-align:left;"> East Asia &amp; Pacific </td>
   <td style="text-align:left;"> Oceania </td>
  </tr>
  <tr>
   <td style="text-align:left;"> Bosnia and Herzegovina </td>
   <td style="text-align:left;"> BIH </td>
   <td style="text-align:right;"> 2000 </td>
   <td style="text-align:right;"> 7.2992129 </td>
   <td style="text-align:left;"> Europe &amp; Central Asia </td>
   <td style="text-align:left;"> Europe </td>
  </tr>
  <tr>
   <td style="text-align:left;"> Laos </td>
   <td style="text-align:left;"> LAO </td>
   <td style="text-align:right;"> 2006 </td>
   <td style="text-align:right;"> 17.1601993 </td>
   <td style="text-align:left;"> East Asia &amp; Pacific </td>
   <td style="text-align:left;"> Asia </td>
  </tr>
  <tr>
   <td style="text-align:left;"> Niue </td>
   <td style="text-align:left;"> NIU </td>
   <td style="text-align:right;"> 2018 </td>
   <td style="text-align:right;"> 0.5024120 </td>
   <td style="text-align:left;"> East Asia &amp; Pacific </td>
   <td style="text-align:left;"> Oceania </td>
  </tr>
  <tr>
   <td style="text-align:left;"> Bolivia </td>
   <td style="text-align:left;"> BOL </td>
   <td style="text-align:right;"> 2010 </td>
   <td style="text-align:right;"> 4.5945211 </td>
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
   <td style="text-align:left;"> Singapore </td>
   <td style="text-align:left;"> SGP </td>
   <td style="text-align:right;"> 2014 </td>
   <td style="text-align:right;"> 0.0132201 </td>
   <td style="text-align:left;"> East Asia &amp; Pacific </td>
   <td style="text-align:left;"> Asia </td>
  </tr>
  <tr>
   <td style="text-align:left;"> United Arab Emirates </td>
   <td style="text-align:left;"> ARE </td>
   <td style="text-align:right;"> 2011 </td>
   <td style="text-align:right;"> 0.0010027 </td>
   <td style="text-align:left;"> Middle East &amp; North Africa </td>
   <td style="text-align:left;"> Asia </td>
  </tr>
  <tr>
   <td style="text-align:left;"> Malta </td>
   <td style="text-align:left;"> MLT </td>
   <td style="text-align:right;"> 2013 </td>
   <td style="text-align:right;"> 0.0161693 </td>
   <td style="text-align:left;"> Middle East &amp; North Africa </td>
   <td style="text-align:left;"> Europe </td>
  </tr>
  <tr>
   <td style="text-align:left;"> Mauritius </td>
   <td style="text-align:left;"> MUS </td>
   <td style="text-align:right;"> 2016 </td>
   <td style="text-align:right;"> 0.1697149 </td>
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
   <td style="text-align:left;"> Brunei </td>
   <td style="text-align:left;"> BRN </td>
   <td style="text-align:right;"> 2011 </td>
   <td style="text-align:right;"> 0.0604161 </td>
   <td style="text-align:left;"> East Asia &amp; Pacific </td>
   <td style="text-align:left;"> Asia </td>
  </tr>
  <tr>
   <td style="text-align:left;"> Guatemala </td>
   <td style="text-align:left;"> GTM </td>
   <td style="text-align:right;"> 2004 </td>
   <td style="text-align:right;"> 9.9655638 </td>
   <td style="text-align:left;"> Latin America &amp; Caribbean </td>
   <td style="text-align:left;"> Americas </td>
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
   <td style="text-align:left;"> Australia </td>
   <td style="text-align:left;"> AUS </td>
   <td style="text-align:right;"> 2016 </td>
   <td style="text-align:right;"> 0.0091758 </td>
   <td style="text-align:left;"> East Asia &amp; Pacific </td>
   <td style="text-align:left;"> Oceania </td>
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
   <td style="text-align:left;"> Palestine </td>
   <td style="text-align:left;"> PSE </td>
   <td style="text-align:right;"> 1996 </td>
   <td style="text-align:right;"> 3.4250360 </td>
   <td style="text-align:left;"> Middle East &amp; North Africa </td>
   <td style="text-align:left;"> Asia </td>
  </tr>
  <tr>
   <td style="text-align:left;"> Saint Kitts and Nevis </td>
   <td style="text-align:left;"> KNA </td>
   <td style="text-align:right;"> 2007 </td>
   <td style="text-align:right;"> 0.4431143 </td>
   <td style="text-align:left;"> Latin America &amp; Caribbean </td>
   <td style="text-align:left;"> Americas </td>
  </tr>
  <tr>
   <td style="text-align:left;"> Russia </td>
   <td style="text-align:left;"> RUS </td>
   <td style="text-align:right;"> 1994 </td>
   <td style="text-align:right;"> 0.6416960 </td>
   <td style="text-align:left;"> Europe &amp; Central Asia </td>
   <td style="text-align:left;"> Europe </td>
  </tr>
  <tr>
   <td style="text-align:left;"> El Salvador </td>
   <td style="text-align:left;"> SLV </td>
   <td style="text-align:right;"> 2004 </td>
   <td style="text-align:right;"> 5.0665782 </td>
   <td style="text-align:left;"> Latin America &amp; Caribbean </td>
   <td style="text-align:left;"> Americas </td>
  </tr>
  <tr>
   <td style="text-align:left;"> Monaco </td>
   <td style="text-align:left;"> MCO </td>
   <td style="text-align:right;"> 2008 </td>
   <td style="text-align:right;"> 0.0042901 </td>
   <td style="text-align:left;"> Europe &amp; Central Asia </td>
   <td style="text-align:left;"> Europe </td>
  </tr>
  <tr>
   <td style="text-align:left;"> Burundi </td>
   <td style="text-align:left;"> BDI </td>
   <td style="text-align:right;"> 2007 </td>
   <td style="text-align:right;"> 13.4564221 </td>
   <td style="text-align:left;"> Sub-Saharan Africa </td>
   <td style="text-align:left;"> Africa </td>
  </tr>
  <tr>
   <td style="text-align:left;"> Ireland </td>
   <td style="text-align:left;"> IRL </td>
   <td style="text-align:right;"> 2011 </td>
   <td style="text-align:right;"> 0.0106735 </td>
   <td style="text-align:left;"> Europe &amp; Central Asia </td>
   <td style="text-align:left;"> Europe </td>
  </tr>
  <tr>
   <td style="text-align:left;"> American Samoa </td>
   <td style="text-align:left;"> ASM </td>
   <td style="text-align:right;"> 2000 </td>
   <td style="text-align:right;"> 2.9145449 </td>
   <td style="text-align:left;"> East Asia &amp; Pacific </td>
   <td style="text-align:left;"> Oceania </td>
  </tr>
  <tr>
   <td style="text-align:left;"> Italy </td>
   <td style="text-align:left;"> ITA </td>
   <td style="text-align:right;"> 2009 </td>
   <td style="text-align:right;"> 0.0318466 </td>
   <td style="text-align:left;"> Europe &amp; Central Asia </td>
   <td style="text-align:left;"> Europe </td>
  </tr>
  <tr>
   <td style="text-align:left;"> Eswatini </td>
   <td style="text-align:left;"> SWZ </td>
   <td style="text-align:right;"> 2005 </td>
   <td style="text-align:right;"> 5.4787924 </td>
   <td style="text-align:left;"> Sub-Saharan Africa </td>
   <td style="text-align:left;"> Africa </td>
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
   <td style="text-align:left;"> Ecuador </td>
   <td style="text-align:left;"> ECU </td>
   <td style="text-align:right;"> 2008 </td>
   <td style="text-align:right;"> 1.1094215 </td>
   <td style="text-align:left;"> Latin America &amp; Caribbean </td>
   <td style="text-align:left;"> Americas </td>
  </tr>
  <tr>
   <td style="text-align:left;"> Burundi </td>
   <td style="text-align:left;"> BDI </td>
   <td style="text-align:right;"> 2008 </td>
   <td style="text-align:right;"> 13.5929389 </td>
   <td style="text-align:left;"> Sub-Saharan Africa </td>
   <td style="text-align:left;"> Africa </td>
  </tr>
  <tr>
   <td style="text-align:left;"> Brunei </td>
   <td style="text-align:left;"> BRN </td>
   <td style="text-align:right;"> 2012 </td>
   <td style="text-align:right;"> 0.0582512 </td>
   <td style="text-align:left;"> East Asia &amp; Pacific </td>
   <td style="text-align:left;"> Asia </td>
  </tr>
  <tr>
   <td style="text-align:left;"> Saint Lucia </td>
   <td style="text-align:left;"> LCA </td>
   <td style="text-align:right;"> 2010 </td>
   <td style="text-align:right;"> 0.6329876 </td>
   <td style="text-align:left;"> Latin America &amp; Caribbean </td>
   <td style="text-align:left;"> Americas </td>
  </tr>
  <tr>
   <td style="text-align:left;"> Ghana </td>
   <td style="text-align:left;"> GHA </td>
   <td style="text-align:right;"> 1999 </td>
   <td style="text-align:right;"> 9.6059109 </td>
   <td style="text-align:left;"> Sub-Saharan Africa </td>
   <td style="text-align:left;"> Africa </td>
  </tr>
  <tr>
   <td style="text-align:left;"> Vanuatu </td>
   <td style="text-align:left;"> VUT </td>
   <td style="text-align:right;"> 2005 </td>
   <td style="text-align:right;"> 16.4150521 </td>
   <td style="text-align:left;"> East Asia &amp; Pacific </td>
   <td style="text-align:left;"> Oceania </td>
  </tr>
  <tr>
   <td style="text-align:left;"> Sudan </td>
   <td style="text-align:left;"> SDN </td>
   <td style="text-align:right;"> 1998 </td>
   <td style="text-align:right;"> 15.7036110 </td>
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
   <td style="text-align:left;"> Laos </td>
   <td style="text-align:left;"> LAO </td>
   <td style="text-align:right;"> 1991 </td>
   <td style="text-align:right;"> 17.5080752 </td>
   <td style="text-align:left;"> East Asia &amp; Pacific </td>
   <td style="text-align:left;"> Asia </td>
  </tr>
  <tr>
   <td style="text-align:left;"> Portugal </td>
   <td style="text-align:left;"> PRT </td>
   <td style="text-align:right;"> 2004 </td>
   <td style="text-align:right;"> 0.1360968 </td>
   <td style="text-align:left;"> Europe &amp; Central Asia </td>
   <td style="text-align:left;"> Europe </td>
  </tr>
  <tr>
   <td style="text-align:left;"> United States Virgin Islands </td>
   <td style="text-align:left;"> VIR </td>
   <td style="text-align:right;"> 2015 </td>
   <td style="text-align:right;"> 0.0683821 </td>
   <td style="text-align:left;"> Latin America &amp; Caribbean </td>
   <td style="text-align:left;"> Americas </td>
  </tr>
  <tr>
   <td style="text-align:left;"> Belgium </td>
   <td style="text-align:left;"> BEL </td>
   <td style="text-align:right;"> 1991 </td>
   <td style="text-align:right;"> 0.0382713 </td>
   <td style="text-align:left;"> Europe &amp; Central Asia </td>
   <td style="text-align:left;"> Europe </td>
  </tr>
  <tr>
   <td style="text-align:left;"> Palau </td>
   <td style="text-align:left;"> PLW </td>
   <td style="text-align:right;"> 2000 </td>
   <td style="text-align:right;"> 0.0074873 </td>
   <td style="text-align:left;"> East Asia &amp; Pacific </td>
   <td style="text-align:left;"> Oceania </td>
  </tr>
  <tr>
   <td style="text-align:left;"> Gabon </td>
   <td style="text-align:left;"> GAB </td>
   <td style="text-align:right;"> 1995 </td>
   <td style="text-align:right;"> 6.5020456 </td>
   <td style="text-align:left;"> Sub-Saharan Africa </td>
   <td style="text-align:left;"> Africa </td>
  </tr>
  <tr>
   <td style="text-align:left;"> United States </td>
   <td style="text-align:left;"> USA </td>
   <td style="text-align:right;"> 2011 </td>
   <td style="text-align:right;"> 0.0074457 </td>
   <td style="text-align:left;"> North America </td>
   <td style="text-align:left;"> Americas </td>
  </tr>
  <tr>
   <td style="text-align:left;"> Kazakhstan </td>
   <td style="text-align:left;"> KAZ </td>
   <td style="text-align:right;"> 1994 </td>
   <td style="text-align:right;"> 5.0699747 </td>
   <td style="text-align:left;"> Europe &amp; Central Asia </td>
   <td style="text-align:left;"> Asia </td>
  </tr>
  <tr>
   <td style="text-align:left;"> Ukraine </td>
   <td style="text-align:left;"> UKR </td>
   <td style="text-align:right;"> 2008 </td>
   <td style="text-align:right;"> 0.6414594 </td>
   <td style="text-align:left;"> Europe &amp; Central Asia </td>
   <td style="text-align:left;"> Europe </td>
  </tr>
  <tr>
   <td style="text-align:left;"> China </td>
   <td style="text-align:left;"> CHN </td>
   <td style="text-align:right;"> 1994 </td>
   <td style="text-align:right;"> 15.4395940 </td>
   <td style="text-align:left;"> East Asia &amp; Pacific </td>
   <td style="text-align:left;"> Asia </td>
  </tr>
  <tr>
   <td style="text-align:left;"> Saint Kitts and Nevis </td>
   <td style="text-align:left;"> KNA </td>
   <td style="text-align:right;"> 2011 </td>
   <td style="text-align:right;"> 0.3053381 </td>
   <td style="text-align:left;"> Latin America &amp; Caribbean </td>
   <td style="text-align:left;"> Americas </td>
  </tr>
  <tr>
   <td style="text-align:left;"> Nepal </td>
   <td style="text-align:left;"> NPL </td>
   <td style="text-align:right;"> 1997 </td>
   <td style="text-align:right;"> 19.2414951 </td>
   <td style="text-align:left;"> South Asia </td>
   <td style="text-align:left;"> Asia </td>
  </tr>
  <tr>
   <td style="text-align:left;"> Nigeria </td>
   <td style="text-align:left;"> NGA </td>
   <td style="text-align:right;"> 1998 </td>
   <td style="text-align:right;"> 11.5486039 </td>
   <td style="text-align:left;"> Sub-Saharan Africa </td>
   <td style="text-align:left;"> Africa </td>
  </tr>
  <tr>
   <td style="text-align:left;"> Mexico </td>
   <td style="text-align:left;"> MEX </td>
   <td style="text-align:right;"> 1995 </td>
   <td style="text-align:right;"> 3.3566106 </td>
   <td style="text-align:left;"> Latin America &amp; Caribbean </td>
   <td style="text-align:left;"> Americas </td>
  </tr>
  <tr>
   <td style="text-align:left;"> Brazil </td>
   <td style="text-align:left;"> BRA </td>
   <td style="text-align:right;"> 1997 </td>
   <td style="text-align:right;"> 3.8556060 </td>
   <td style="text-align:left;"> Latin America &amp; Caribbean </td>
   <td style="text-align:left;"> Americas </td>
  </tr>
  <tr>
   <td style="text-align:left;"> Finland </td>
   <td style="text-align:left;"> FIN </td>
   <td style="text-align:right;"> 2010 </td>
   <td style="text-align:right;"> 0.0087496 </td>
   <td style="text-align:left;"> Europe &amp; Central Asia </td>
   <td style="text-align:left;"> Europe </td>
  </tr>
  <tr>
   <td style="text-align:left;"> Cyprus </td>
   <td style="text-align:left;"> CYP </td>
   <td style="text-align:right;"> 2009 </td>
   <td style="text-align:right;"> 0.0163221 </td>
   <td style="text-align:left;"> Europe &amp; Central Asia </td>
   <td style="text-align:left;"> Asia </td>
  </tr>
  <tr>
   <td style="text-align:left;"> Burundi </td>
   <td style="text-align:left;"> BDI </td>
   <td style="text-align:right;"> 1997 </td>
   <td style="text-align:right;"> 10.5267621 </td>
   <td style="text-align:left;"> Sub-Saharan Africa </td>
   <td style="text-align:left;"> Africa </td>
  </tr>
  <tr>
   <td style="text-align:left;"> Mongolia </td>
   <td style="text-align:left;"> MNG </td>
   <td style="text-align:right;"> 2010 </td>
   <td style="text-align:right;"> 7.0816528 </td>
   <td style="text-align:left;"> East Asia &amp; Pacific </td>
   <td style="text-align:left;"> Asia </td>
  </tr>
  <tr>
   <td style="text-align:left;"> Guam </td>
   <td style="text-align:left;"> GUM </td>
   <td style="text-align:right;"> 2011 </td>
   <td style="text-align:right;"> 0.4431576 </td>
   <td style="text-align:left;"> East Asia &amp; Pacific </td>
   <td style="text-align:left;"> Oceania </td>
  </tr>
  <tr>
   <td style="text-align:left;"> Monaco </td>
   <td style="text-align:left;"> MCO </td>
   <td style="text-align:right;"> 2009 </td>
   <td style="text-align:right;"> 0.0040454 </td>
   <td style="text-align:left;"> Europe &amp; Central Asia </td>
   <td style="text-align:left;"> Europe </td>
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
   <td style="text-align:left;"> Montenegro </td>
   <td style="text-align:left;"> MNE </td>
   <td style="text-align:right;"> 2010 </td>
   <td style="text-align:right;"> 2.9028185 </td>
   <td style="text-align:left;"> Europe &amp; Central Asia </td>
   <td style="text-align:left;"> Europe </td>
  </tr>
  <tr>
   <td style="text-align:left;"> Gambia </td>
   <td style="text-align:left;"> GMB </td>
   <td style="text-align:right;"> 1999 </td>
   <td style="text-align:right;"> 13.7935851 </td>
   <td style="text-align:left;"> Sub-Saharan Africa </td>
   <td style="text-align:left;"> Africa </td>
  </tr>
  <tr>
   <td style="text-align:left;"> Cape Verde </td>
   <td style="text-align:left;"> CPV </td>
   <td style="text-align:right;"> 1999 </td>
   <td style="text-align:right;"> 9.3297722 </td>
   <td style="text-align:left;"> Sub-Saharan Africa </td>
   <td style="text-align:left;"> Africa </td>
  </tr>
  <tr>
   <td style="text-align:left;"> Malawi </td>
   <td style="text-align:left;"> MWI </td>
   <td style="text-align:right;"> 1997 </td>
   <td style="text-align:right;"> 9.6606601 </td>
   <td style="text-align:left;"> Sub-Saharan Africa </td>
   <td style="text-align:left;"> Africa </td>
  </tr>
  <tr>
   <td style="text-align:left;"> Brazil </td>
   <td style="text-align:left;"> BRA </td>
   <td style="text-align:right;"> 2006 </td>
   <td style="text-align:right;"> 2.3798633 </td>
   <td style="text-align:left;"> Latin America &amp; Caribbean </td>
   <td style="text-align:left;"> Americas </td>
  </tr>
  <tr>
   <td style="text-align:left;"> Canada </td>
   <td style="text-align:left;"> CAN </td>
   <td style="text-align:right;"> 2005 </td>
   <td style="text-align:right;"> 0.0129862 </td>
   <td style="text-align:left;"> North America </td>
   <td style="text-align:left;"> Americas </td>
  </tr>
  <tr>
   <td style="text-align:left;"> Luxembourg </td>
   <td style="text-align:left;"> LUX </td>
   <td style="text-align:right;"> 1991 </td>
   <td style="text-align:right;"> 0.0296342 </td>
   <td style="text-align:left;"> Europe &amp; Central Asia </td>
   <td style="text-align:left;"> Europe </td>
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
   <td style="text-align:left;"> Ghana </td>
   <td style="text-align:left;"> GHA </td>
   <td style="text-align:right;"> 2015 </td>
   <td style="text-align:right;"> 7.0340240 </td>
   <td style="text-align:left;"> Sub-Saharan Africa </td>
   <td style="text-align:left;"> Africa </td>
  </tr>
  <tr>
   <td style="text-align:left;"> Iceland </td>
   <td style="text-align:left;"> ISL </td>
   <td style="text-align:right;"> 2000 </td>
   <td style="text-align:right;"> 0.0157977 </td>
   <td style="text-align:left;"> Europe &amp; Central Asia </td>
   <td style="text-align:left;"> Europe </td>
  </tr>
  <tr>
   <td style="text-align:left;"> Fiji </td>
   <td style="text-align:left;"> FJI </td>
   <td style="text-align:right;"> 1994 </td>
   <td style="text-align:right;"> 9.6773613 </td>
   <td style="text-align:left;"> East Asia &amp; Pacific </td>
   <td style="text-align:left;"> Oceania </td>
  </tr>
  <tr>
   <td style="text-align:left;"> Nicaragua </td>
   <td style="text-align:left;"> NIC </td>
   <td style="text-align:right;"> 1992 </td>
   <td style="text-align:right;"> 12.4339172 </td>
   <td style="text-align:left;"> Latin America &amp; Caribbean </td>
   <td style="text-align:left;"> Americas </td>
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
   <td style="text-align:left;"> Malaysia </td>
   <td style="text-align:left;"> MYS </td>
   <td style="text-align:right;"> 2003 </td>
   <td style="text-align:right;"> 0.2143972 </td>
   <td style="text-align:left;"> East Asia &amp; Pacific </td>
   <td style="text-align:left;"> Asia </td>
  </tr>
  <tr>
   <td style="text-align:left;"> Hungary </td>
   <td style="text-align:left;"> HUN </td>
   <td style="text-align:right;"> 1993 </td>
   <td style="text-align:right;"> 2.6407676 </td>
   <td style="text-align:left;"> Europe &amp; Central Asia </td>
   <td style="text-align:left;"> Europe </td>
  </tr>
  <tr>
   <td style="text-align:left;"> Cyprus </td>
   <td style="text-align:left;"> CYP </td>
   <td style="text-align:right;"> 1991 </td>
   <td style="text-align:right;"> 0.1400386 </td>
   <td style="text-align:left;"> Europe &amp; Central Asia </td>
   <td style="text-align:left;"> Asia </td>
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
   <td style="text-align:left;"> Jordan </td>
   <td style="text-align:left;"> JOR </td>
   <td style="text-align:right;"> 2018 </td>
   <td style="text-align:right;"> 0.0050760 </td>
   <td style="text-align:left;"> Middle East &amp; North Africa </td>
   <td style="text-align:left;"> Asia </td>
  </tr>
  <tr>
   <td style="text-align:left;"> Slovakia </td>
   <td style="text-align:left;"> SVK </td>
   <td style="text-align:right;"> 2007 </td>
   <td style="text-align:right;"> 0.1161647 </td>
   <td style="text-align:left;"> Europe &amp; Central Asia </td>
   <td style="text-align:left;"> Europe </td>
  </tr>
  <tr>
   <td style="text-align:left;"> Azerbaijan </td>
   <td style="text-align:left;"> AZE </td>
   <td style="text-align:right;"> 2002 </td>
   <td style="text-align:right;"> 4.5726917 </td>
   <td style="text-align:left;"> Europe &amp; Central Asia </td>
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
   <td style="text-align:left;"> Seychelles </td>
   <td style="text-align:left;"> SYC </td>
   <td style="text-align:right;"> 2012 </td>
   <td style="text-align:right;"> 0.0806702 </td>
   <td style="text-align:left;"> Sub-Saharan Africa </td>
   <td style="text-align:left;"> Africa </td>
  </tr>
  <tr>
   <td style="text-align:left;"> Dominica </td>
   <td style="text-align:left;"> DMA </td>
   <td style="text-align:right;"> 2014 </td>
   <td style="text-align:right;"> 0.6961462 </td>
   <td style="text-align:left;"> Latin America &amp; Caribbean </td>
   <td style="text-align:left;"> Americas </td>
  </tr>
  <tr>
   <td style="text-align:left;"> China </td>
   <td style="text-align:left;"> CHN </td>
   <td style="text-align:right;"> 2001 </td>
   <td style="text-align:right;"> 11.1495405 </td>
   <td style="text-align:left;"> East Asia &amp; Pacific </td>
   <td style="text-align:left;"> Asia </td>
  </tr>
  <tr>
   <td style="text-align:left;"> Guatemala </td>
   <td style="text-align:left;"> GTM </td>
   <td style="text-align:right;"> 2010 </td>
   <td style="text-align:right;"> 8.3411812 </td>
   <td style="text-align:left;"> Latin America &amp; Caribbean </td>
   <td style="text-align:left;"> Americas </td>
  </tr>
  <tr>
   <td style="text-align:left;"> Argentina </td>
   <td style="text-align:left;"> ARG </td>
   <td style="text-align:right;"> 1993 </td>
   <td style="text-align:right;"> 1.2413031 </td>
   <td style="text-align:left;"> Latin America &amp; Caribbean </td>
   <td style="text-align:left;"> Americas </td>
  </tr>
  <tr>
   <td style="text-align:left;"> Qatar </td>
   <td style="text-align:left;"> QAT </td>
   <td style="text-align:right;"> 2019 </td>
   <td style="text-align:right;"> 0.0009699 </td>
   <td style="text-align:left;"> Middle East &amp; North Africa </td>
   <td style="text-align:left;"> Asia </td>
  </tr>
  <tr>
   <td style="text-align:left;"> Guyana </td>
   <td style="text-align:left;"> GUY </td>
   <td style="text-align:right;"> 2005 </td>
   <td style="text-align:right;"> 2.6140985 </td>
   <td style="text-align:left;"> Latin America &amp; Caribbean </td>
   <td style="text-align:left;"> Americas </td>
  </tr>
  <tr>
   <td style="text-align:left;"> Bahrain </td>
   <td style="text-align:left;"> BHR </td>
   <td style="text-align:right;"> 1999 </td>
   <td style="text-align:right;"> 0.4844744 </td>
   <td style="text-align:left;"> Middle East &amp; North Africa </td>
   <td style="text-align:left;"> Asia </td>
  </tr>
  <tr>
   <td style="text-align:left;"> Switzerland </td>
   <td style="text-align:left;"> CHE </td>
   <td style="text-align:right;"> 2005 </td>
   <td style="text-align:right;"> 0.0043229 </td>
   <td style="text-align:left;"> Europe &amp; Central Asia </td>
   <td style="text-align:left;"> Europe </td>
  </tr>
  <tr>
   <td style="text-align:left;"> Tuvalu </td>
   <td style="text-align:left;"> TUV </td>
   <td style="text-align:right;"> 2007 </td>
   <td style="text-align:right;"> 4.0096915 </td>
   <td style="text-align:left;"> East Asia &amp; Pacific </td>
   <td style="text-align:left;"> Oceania </td>
  </tr>
  <tr>
   <td style="text-align:left;"> Guatemala </td>
   <td style="text-align:left;"> GTM </td>
   <td style="text-align:right;"> 1997 </td>
   <td style="text-align:right;"> 11.7633156 </td>
   <td style="text-align:left;"> Latin America &amp; Caribbean </td>
   <td style="text-align:left;"> Americas </td>
  </tr>
  <tr>
   <td style="text-align:left;"> Serbia </td>
   <td style="text-align:left;"> SRB </td>
   <td style="text-align:right;"> 1991 </td>
   <td style="text-align:right;"> 6.4658184 </td>
   <td style="text-align:left;"> Europe &amp; Central Asia </td>
   <td style="text-align:left;"> Europe </td>
  </tr>
  <tr>
   <td style="text-align:left;"> Singapore </td>
   <td style="text-align:left;"> SGP </td>
   <td style="text-align:right;"> 2015 </td>
   <td style="text-align:right;"> 0.0121706 </td>
   <td style="text-align:left;"> East Asia &amp; Pacific </td>
   <td style="text-align:left;"> Asia </td>
  </tr>
  <tr>
   <td style="text-align:left;"> Yemen </td>
   <td style="text-align:left;"> YEM </td>
   <td style="text-align:right;"> 2007 </td>
   <td style="text-align:right;"> 10.4712208 </td>
   <td style="text-align:left;"> Middle East &amp; North Africa </td>
   <td style="text-align:left;"> Asia </td>
  </tr>
  <tr>
   <td style="text-align:left;"> Qatar </td>
   <td style="text-align:left;"> QAT </td>
   <td style="text-align:right;"> 2003 </td>
   <td style="text-align:right;"> 0.0074812 </td>
   <td style="text-align:left;"> Middle East &amp; North Africa </td>
   <td style="text-align:left;"> Asia </td>
  </tr>
  <tr>
   <td style="text-align:left;"> Northern Mariana Islands </td>
   <td style="text-align:left;"> MNP </td>
   <td style="text-align:right;"> 2010 </td>
   <td style="text-align:right;"> 1.3910565 </td>
   <td style="text-align:left;"> East Asia &amp; Pacific </td>
   <td style="text-align:left;"> Oceania </td>
  </tr>
  <tr>
   <td style="text-align:left;"> Peru </td>
   <td style="text-align:left;"> PER </td>
   <td style="text-align:right;"> 1991 </td>
   <td style="text-align:right;"> 6.3450720 </td>
   <td style="text-align:left;"> Latin America &amp; Caribbean </td>
   <td style="text-align:left;"> Americas </td>
  </tr>
  <tr>
   <td style="text-align:left;"> Thailand </td>
   <td style="text-align:left;"> THA </td>
   <td style="text-align:right;"> 2011 </td>
   <td style="text-align:right;"> 2.4531593 </td>
   <td style="text-align:left;"> East Asia &amp; Pacific </td>
   <td style="text-align:left;"> Asia </td>
  </tr>
  <tr>
   <td style="text-align:left;"> Eritrea </td>
   <td style="text-align:left;"> ERI </td>
   <td style="text-align:right;"> 1995 </td>
   <td style="text-align:right;"> 11.6605626 </td>
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
   <td style="text-align:left;"> Poland </td>
   <td style="text-align:left;"> POL </td>
   <td style="text-align:right;"> 2015 </td>
   <td style="text-align:right;"> 0.8269782 </td>
   <td style="text-align:left;"> Europe &amp; Central Asia </td>
   <td style="text-align:left;"> Europe </td>
  </tr>
  <tr>
   <td style="text-align:left;"> Mexico </td>
   <td style="text-align:left;"> MEX </td>
   <td style="text-align:right;"> 1994 </td>
   <td style="text-align:right;"> 3.4756696 </td>
   <td style="text-align:left;"> Latin America &amp; Caribbean </td>
   <td style="text-align:left;"> Americas </td>
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
   <td style="text-align:left;"> Yemen </td>
   <td style="text-align:left;"> YEM </td>
   <td style="text-align:right;"> 1994 </td>
   <td style="text-align:right;"> 15.9547880 </td>
   <td style="text-align:left;"> Middle East &amp; North Africa </td>
   <td style="text-align:left;"> Asia </td>
  </tr>
  <tr>
   <td style="text-align:left;"> Saudi Arabia </td>
   <td style="text-align:left;"> SAU </td>
   <td style="text-align:right;"> 2003 </td>
   <td style="text-align:right;"> 0.5765383 </td>
   <td style="text-align:left;"> Middle East &amp; North Africa </td>
   <td style="text-align:left;"> Asia </td>
  </tr>
  <tr>
   <td style="text-align:left;"> Northern Mariana Islands </td>
   <td style="text-align:left;"> MNP </td>
   <td style="text-align:right;"> 1998 </td>
   <td style="text-align:right;"> 1.5229319 </td>
   <td style="text-align:left;"> East Asia &amp; Pacific </td>
   <td style="text-align:left;"> Oceania </td>
  </tr>
  <tr>
   <td style="text-align:left;"> Samoa </td>
   <td style="text-align:left;"> WSM </td>
   <td style="text-align:right;"> 2008 </td>
   <td style="text-align:right;"> 12.9418849 </td>
   <td style="text-align:left;"> East Asia &amp; Pacific </td>
   <td style="text-align:left;"> Oceania </td>
  </tr>
  <tr>
   <td style="text-align:left;"> Lesotho </td>
   <td style="text-align:left;"> LSO </td>
   <td style="text-align:right;"> 1997 </td>
   <td style="text-align:right;"> 8.4922225 </td>
   <td style="text-align:left;"> Sub-Saharan Africa </td>
   <td style="text-align:left;"> Africa </td>
  </tr>
  <tr>
   <td style="text-align:left;"> Moldova </td>
   <td style="text-align:left;"> MDA </td>
   <td style="text-align:right;"> 2008 </td>
   <td style="text-align:right;"> 1.8583228 </td>
   <td style="text-align:left;"> Europe &amp; Central Asia </td>
   <td style="text-align:left;"> Europe </td>
  </tr>
  <tr>
   <td style="text-align:left;"> American Samoa </td>
   <td style="text-align:left;"> ASM </td>
   <td style="text-align:right;"> 2013 </td>
   <td style="text-align:right;"> 2.0165932 </td>
   <td style="text-align:left;"> East Asia &amp; Pacific </td>
   <td style="text-align:left;"> Oceania </td>
  </tr>
  <tr>
   <td style="text-align:left;"> Mozambique </td>
   <td style="text-align:left;"> MOZ </td>
   <td style="text-align:right;"> 2016 </td>
   <td style="text-align:right;"> 11.6172331 </td>
   <td style="text-align:left;"> Sub-Saharan Africa </td>
   <td style="text-align:left;"> Africa </td>
  </tr>
  <tr>
   <td style="text-align:left;"> Niue </td>
   <td style="text-align:left;"> NIU </td>
   <td style="text-align:right;"> 1991 </td>
   <td style="text-align:right;"> 4.7985502 </td>
   <td style="text-align:left;"> East Asia &amp; Pacific </td>
   <td style="text-align:left;"> Oceania </td>
  </tr>
  <tr>
   <td style="text-align:left;"> Tanzania </td>
   <td style="text-align:left;"> TZA </td>
   <td style="text-align:right;"> 2005 </td>
   <td style="text-align:right;"> 10.1338731 </td>
   <td style="text-align:left;"> Sub-Saharan Africa </td>
   <td style="text-align:left;"> Africa </td>
  </tr>
  <tr>
   <td style="text-align:left;"> Peru </td>
   <td style="text-align:left;"> PER </td>
   <td style="text-align:right;"> 2016 </td>
   <td style="text-align:right;"> 2.1373238 </td>
   <td style="text-align:left;"> Latin America &amp; Caribbean </td>
   <td style="text-align:left;"> Americas </td>
  </tr>
  <tr>
   <td style="text-align:left;"> Panama </td>
   <td style="text-align:left;"> PAN </td>
   <td style="text-align:right;"> 1999 </td>
   <td style="text-align:right;"> 2.7851487 </td>
   <td style="text-align:left;"> Latin America &amp; Caribbean </td>
   <td style="text-align:left;"> Americas </td>
  </tr>
  <tr>
   <td style="text-align:left;"> Cameroon </td>
   <td style="text-align:left;"> CMR </td>
   <td style="text-align:right;"> 2014 </td>
   <td style="text-align:right;"> 7.4018086 </td>
   <td style="text-align:left;"> Sub-Saharan Africa </td>
   <td style="text-align:left;"> Africa </td>
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
   <td style="text-align:left;"> Angola </td>
   <td style="text-align:left;"> AGO </td>
   <td style="text-align:right;"> 1994 </td>
   <td style="text-align:right;"> 12.2425309 </td>
   <td style="text-align:left;"> Sub-Saharan Africa </td>
   <td style="text-align:left;"> Africa </td>
  </tr>
  <tr>
   <td style="text-align:left;"> Sri Lanka </td>
   <td style="text-align:left;"> LKA </td>
   <td style="text-align:right;"> 2011 </td>
   <td style="text-align:right;"> 6.6818933 </td>
   <td style="text-align:left;"> South Asia </td>
   <td style="text-align:left;"> Asia </td>
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
   <td style="text-align:left;"> North Korea </td>
   <td style="text-align:left;"> PRK </td>
   <td style="text-align:right;"> 2002 </td>
   <td style="text-align:right;"> 13.7928696 </td>
   <td style="text-align:left;"> East Asia &amp; Pacific </td>
   <td style="text-align:left;"> Asia </td>
  </tr>
  <tr>
   <td style="text-align:left;"> Tajikistan </td>
   <td style="text-align:left;"> TJK </td>
   <td style="text-align:right;"> 2001 </td>
   <td style="text-align:right;"> 12.6267213 </td>
   <td style="text-align:left;"> Europe &amp; Central Asia </td>
   <td style="text-align:left;"> Asia </td>
  </tr>
  <tr>
   <td style="text-align:left;"> Sri Lanka </td>
   <td style="text-align:left;"> LKA </td>
   <td style="text-align:right;"> 1994 </td>
   <td style="text-align:right;"> 9.6137978 </td>
   <td style="text-align:left;"> South Asia </td>
   <td style="text-align:left;"> Asia </td>
  </tr>
  <tr>
   <td style="text-align:left;"> Guinea-Bissau </td>
   <td style="text-align:left;"> GNB </td>
   <td style="text-align:right;"> 1993 </td>
   <td style="text-align:right;"> 14.2344720 </td>
   <td style="text-align:left;"> Sub-Saharan Africa </td>
   <td style="text-align:left;"> Africa </td>
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
   <td style="text-align:left;"> Ukraine </td>
   <td style="text-align:left;"> UKR </td>
   <td style="text-align:right;"> 2003 </td>
   <td style="text-align:right;"> 0.8810336 </td>
   <td style="text-align:left;"> Europe &amp; Central Asia </td>
   <td style="text-align:left;"> Europe </td>
  </tr>
  <tr>
   <td style="text-align:left;"> Uruguay </td>
   <td style="text-align:left;"> URY </td>
   <td style="text-align:right;"> 2000 </td>
   <td style="text-align:right;"> 0.5106119 </td>
   <td style="text-align:left;"> Latin America &amp; Caribbean </td>
   <td style="text-align:left;"> Americas </td>
  </tr>
  <tr>
   <td style="text-align:left;"> Peru </td>
   <td style="text-align:left;"> PER </td>
   <td style="text-align:right;"> 2007 </td>
   <td style="text-align:right;"> 3.9410163 </td>
   <td style="text-align:left;"> Latin America &amp; Caribbean </td>
   <td style="text-align:left;"> Americas </td>
  </tr>
  <tr>
   <td style="text-align:left;"> Haiti </td>
   <td style="text-align:left;"> HTI </td>
   <td style="text-align:right;"> 2015 </td>
   <td style="text-align:right;"> 13.9965702 </td>
   <td style="text-align:left;"> Latin America &amp; Caribbean </td>
   <td style="text-align:left;"> Americas </td>
  </tr>
  <tr>
   <td style="text-align:left;"> Comoros </td>
   <td style="text-align:left;"> COM </td>
   <td style="text-align:right;"> 2001 </td>
   <td style="text-align:right;"> 13.4674397 </td>
   <td style="text-align:left;"> Sub-Saharan Africa </td>
   <td style="text-align:left;"> Africa </td>
  </tr>
  <tr>
   <td style="text-align:left;"> India </td>
   <td style="text-align:left;"> IND </td>
   <td style="text-align:right;"> 1990 </td>
   <td style="text-align:right;"> 13.5225391 </td>
   <td style="text-align:left;"> South Asia </td>
   <td style="text-align:left;"> Asia </td>
  </tr>
  <tr>
   <td style="text-align:left;"> Rwanda </td>
   <td style="text-align:left;"> RWA </td>
   <td style="text-align:right;"> 1991 </td>
   <td style="text-align:right;"> 13.0897593 </td>
   <td style="text-align:left;"> Sub-Saharan Africa </td>
   <td style="text-align:left;"> Africa </td>
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
   <td style="text-align:left;"> Portugal </td>
   <td style="text-align:left;"> PRT </td>
   <td style="text-align:right;"> 1996 </td>
   <td style="text-align:right;"> 0.3061380 </td>
   <td style="text-align:left;"> Europe &amp; Central Asia </td>
   <td style="text-align:left;"> Europe </td>
  </tr>
  <tr>
   <td style="text-align:left;"> Guam </td>
   <td style="text-align:left;"> GUM </td>
   <td style="text-align:right;"> 1993 </td>
   <td style="text-align:right;"> 0.6472999 </td>
   <td style="text-align:left;"> East Asia &amp; Pacific </td>
   <td style="text-align:left;"> Oceania </td>
  </tr>
  <tr>
   <td style="text-align:left;"> Tuvalu </td>
   <td style="text-align:left;"> TUV </td>
   <td style="text-align:right;"> 2003 </td>
   <td style="text-align:right;"> 5.3650781 </td>
   <td style="text-align:left;"> East Asia &amp; Pacific </td>
   <td style="text-align:left;"> Oceania </td>
  </tr>
  <tr>
   <td style="text-align:left;"> Vietnam </td>
   <td style="text-align:left;"> VNM </td>
   <td style="text-align:right;"> 2000 </td>
   <td style="text-align:right;"> 12.2880557 </td>
   <td style="text-align:left;"> East Asia &amp; Pacific </td>
   <td style="text-align:left;"> Asia </td>
  </tr>
  <tr>
   <td style="text-align:left;"> Botswana </td>
   <td style="text-align:left;"> BWA </td>
   <td style="text-align:right;"> 2016 </td>
   <td style="text-align:right;"> 3.9886017 </td>
   <td style="text-align:left;"> Sub-Saharan Africa </td>
   <td style="text-align:left;"> Africa </td>
  </tr>
  <tr>
   <td style="text-align:left;"> Djibouti </td>
   <td style="text-align:left;"> DJI </td>
   <td style="text-align:right;"> 1999 </td>
   <td style="text-align:right;"> 7.3431821 </td>
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
   <td style="text-align:left;"> Venezuela </td>
   <td style="text-align:left;"> VEN </td>
   <td style="text-align:right;"> 2009 </td>
   <td style="text-align:right;"> 0.1060430 </td>
   <td style="text-align:left;"> Latin America &amp; Caribbean </td>
   <td style="text-align:left;"> Americas </td>
  </tr>
  <tr>
   <td style="text-align:left;"> Thailand </td>
   <td style="text-align:left;"> THA </td>
   <td style="text-align:right;"> 1991 </td>
   <td style="text-align:right;"> 7.4160796 </td>
   <td style="text-align:left;"> East Asia &amp; Pacific </td>
   <td style="text-align:left;"> Asia </td>
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
   <td style="text-align:left;"> Angola </td>
   <td style="text-align:left;"> AGO </td>
   <td style="text-align:right;"> 2007 </td>
   <td style="text-align:right;"> 10.3075334 </td>
   <td style="text-align:left;"> Sub-Saharan Africa </td>
   <td style="text-align:left;"> Africa </td>
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
   <td style="text-align:left;"> Mozambique </td>
   <td style="text-align:left;"> MOZ </td>
   <td style="text-align:right;"> 2009 </td>
   <td style="text-align:right;"> 11.5026051 </td>
   <td style="text-align:left;"> Sub-Saharan Africa </td>
   <td style="text-align:left;"> Africa </td>
  </tr>
  <tr>
   <td style="text-align:left;"> Tanzania </td>
   <td style="text-align:left;"> TZA </td>
   <td style="text-align:right;"> 2015 </td>
   <td style="text-align:right;"> 11.8664150 </td>
   <td style="text-align:left;"> Sub-Saharan Africa </td>
   <td style="text-align:left;"> Africa </td>
  </tr>
  <tr>
   <td style="text-align:left;"> Tunisia </td>
   <td style="text-align:left;"> TUN </td>
   <td style="text-align:right;"> 2005 </td>
   <td style="text-align:right;"> 0.2544714 </td>
   <td style="text-align:left;"> Middle East &amp; North Africa </td>
   <td style="text-align:left;"> Africa </td>
  </tr>
  <tr>
   <td style="text-align:left;"> Turkey </td>
   <td style="text-align:left;"> TUR </td>
   <td style="text-align:right;"> 2015 </td>
   <td style="text-align:right;"> 0.0871198 </td>
   <td style="text-align:left;"> Europe &amp; Central Asia </td>
   <td style="text-align:left;"> Asia </td>
  </tr>
  <tr>
   <td style="text-align:left;"> Northern Mariana Islands </td>
   <td style="text-align:left;"> MNP </td>
   <td style="text-align:right;"> 1993 </td>
   <td style="text-align:right;"> 2.0950392 </td>
   <td style="text-align:left;"> East Asia &amp; Pacific </td>
   <td style="text-align:left;"> Oceania </td>
  </tr>
  <tr>
   <td style="text-align:left;"> Kyrgyzstan </td>
   <td style="text-align:left;"> KGZ </td>
   <td style="text-align:right;"> 2018 </td>
   <td style="text-align:right;"> 4.5455530 </td>
   <td style="text-align:left;"> Europe &amp; Central Asia </td>
   <td style="text-align:left;"> Asia </td>
  </tr>
  <tr>
   <td style="text-align:left;"> Bhutan </td>
   <td style="text-align:left;"> BTN </td>
   <td style="text-align:right;"> 1991 </td>
   <td style="text-align:right;"> 16.3791532 </td>
   <td style="text-align:left;"> South Asia </td>
   <td style="text-align:left;"> Asia </td>
  </tr>
  <tr>
   <td style="text-align:left;"> Dominican Republic </td>
   <td style="text-align:left;"> DOM </td>
   <td style="text-align:right;"> 2007 </td>
   <td style="text-align:right;"> 2.7804257 </td>
   <td style="text-align:left;"> Latin America &amp; Caribbean </td>
   <td style="text-align:left;"> Americas </td>
  </tr>
  <tr>
   <td style="text-align:left;"> Saint Vincent and the Grenadines </td>
   <td style="text-align:left;"> VCT </td>
   <td style="text-align:right;"> 2012 </td>
   <td style="text-align:right;"> 0.6151468 </td>
   <td style="text-align:left;"> Latin America &amp; Caribbean </td>
   <td style="text-align:left;"> Americas </td>
  </tr>
  <tr>
   <td style="text-align:left;"> Togo </td>
   <td style="text-align:left;"> TGO </td>
   <td style="text-align:right;"> 2006 </td>
   <td style="text-align:right;"> 11.4073844 </td>
   <td style="text-align:left;"> Sub-Saharan Africa </td>
   <td style="text-align:left;"> Africa </td>
  </tr>
  <tr>
   <td style="text-align:left;"> Argentina </td>
   <td style="text-align:left;"> ARG </td>
   <td style="text-align:right;"> 2001 </td>
   <td style="text-align:right;"> 0.6914511 </td>
   <td style="text-align:left;"> Latin America &amp; Caribbean </td>
   <td style="text-align:left;"> Americas </td>
  </tr>
  <tr>
   <td style="text-align:left;"> Germany </td>
   <td style="text-align:left;"> DEU </td>
   <td style="text-align:right;"> 2005 </td>
   <td style="text-align:right;"> 0.0060995 </td>
   <td style="text-align:left;"> Europe &amp; Central Asia </td>
   <td style="text-align:left;"> Europe </td>
  </tr>
  <tr>
   <td style="text-align:left;"> Mexico </td>
   <td style="text-align:left;"> MEX </td>
   <td style="text-align:right;"> 2001 </td>
   <td style="text-align:right;"> 2.5069589 </td>
   <td style="text-align:left;"> Latin America &amp; Caribbean </td>
   <td style="text-align:left;"> Americas </td>
  </tr>
  <tr>
   <td style="text-align:left;"> Antigua and Barbuda </td>
   <td style="text-align:left;"> ATG </td>
   <td style="text-align:right;"> 2010 </td>
   <td style="text-align:right;"> 0.1563244 </td>
   <td style="text-align:left;"> Latin America &amp; Caribbean </td>
   <td style="text-align:left;"> Americas </td>
  </tr>
  <tr>
   <td style="text-align:left;"> Mali </td>
   <td style="text-align:left;"> MLI </td>
   <td style="text-align:right;"> 2007 </td>
   <td style="text-align:right;"> 12.2309241 </td>
   <td style="text-align:left;"> Sub-Saharan Africa </td>
   <td style="text-align:left;"> Africa </td>
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
   <td style="text-align:left;"> South Africa </td>
   <td style="text-align:left;"> ZAF </td>
   <td style="text-align:right;"> 1990 </td>
   <td style="text-align:right;"> 4.7073957 </td>
   <td style="text-align:left;"> Sub-Saharan Africa </td>
   <td style="text-align:left;"> Africa </td>
  </tr>
  <tr>
   <td style="text-align:left;"> Egypt </td>
   <td style="text-align:left;"> EGY </td>
   <td style="text-align:right;"> 2002 </td>
   <td style="text-align:right;"> 0.2568546 </td>
   <td style="text-align:left;"> Middle East &amp; North Africa </td>
   <td style="text-align:left;"> Africa </td>
  </tr>
  <tr>
   <td style="text-align:left;"> Togo </td>
   <td style="text-align:left;"> TGO </td>
   <td style="text-align:right;"> 2005 </td>
   <td style="text-align:right;"> 11.2570594 </td>
   <td style="text-align:left;"> Sub-Saharan Africa </td>
   <td style="text-align:left;"> Africa </td>
  </tr>
  <tr>
   <td style="text-align:left;"> Belarus </td>
   <td style="text-align:left;"> BLR </td>
   <td style="text-align:right;"> 2008 </td>
   <td style="text-align:right;"> 0.2179966 </td>
   <td style="text-align:left;"> Europe &amp; Central Asia </td>
   <td style="text-align:left;"> Europe </td>
  </tr>
  <tr>
   <td style="text-align:left;"> South Korea </td>
   <td style="text-align:left;"> KOR </td>
   <td style="text-align:right;"> 2006 </td>
   <td style="text-align:right;"> 0.0150923 </td>
   <td style="text-align:left;"> East Asia &amp; Pacific </td>
   <td style="text-align:left;"> Asia </td>
  </tr>
  <tr>
   <td style="text-align:left;"> Azerbaijan </td>
   <td style="text-align:left;"> AZE </td>
   <td style="text-align:right;"> 1992 </td>
   <td style="text-align:right;"> 6.9118831 </td>
   <td style="text-align:left;"> Europe &amp; Central Asia </td>
   <td style="text-align:left;"> Asia </td>
  </tr>
  <tr>
   <td style="text-align:left;"> Nigeria </td>
   <td style="text-align:left;"> NGA </td>
   <td style="text-align:right;"> 2005 </td>
   <td style="text-align:right;"> 9.9740614 </td>
   <td style="text-align:left;"> Sub-Saharan Africa </td>
   <td style="text-align:left;"> Africa </td>
  </tr>
  <tr>
   <td style="text-align:left;"> Monaco </td>
   <td style="text-align:left;"> MCO </td>
   <td style="text-align:right;"> 2006 </td>
   <td style="text-align:right;"> 0.0048875 </td>
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
   <td style="text-align:left;"> Belize </td>
   <td style="text-align:left;"> BLZ </td>
   <td style="text-align:right;"> 2014 </td>
   <td style="text-align:right;"> 2.1048609 </td>
   <td style="text-align:left;"> Latin America &amp; Caribbean </td>
   <td style="text-align:left;"> Americas </td>
  </tr>
  <tr>
   <td style="text-align:left;"> Haiti </td>
   <td style="text-align:left;"> HTI </td>
   <td style="text-align:right;"> 2001 </td>
   <td style="text-align:right;"> 14.1259905 </td>
   <td style="text-align:left;"> Latin America &amp; Caribbean </td>
   <td style="text-align:left;"> Americas </td>
  </tr>
  <tr>
   <td style="text-align:left;"> Mozambique </td>
   <td style="text-align:left;"> MOZ </td>
   <td style="text-align:right;"> 2003 </td>
   <td style="text-align:right;"> 11.0756016 </td>
   <td style="text-align:left;"> Sub-Saharan Africa </td>
   <td style="text-align:left;"> Africa </td>
  </tr>
  <tr>
   <td style="text-align:left;"> Algeria </td>
   <td style="text-align:left;"> DZA </td>
   <td style="text-align:right;"> 2004 </td>
   <td style="text-align:right;"> 0.3453163 </td>
   <td style="text-align:left;"> Middle East &amp; North Africa </td>
   <td style="text-align:left;"> Africa </td>
  </tr>
  <tr>
   <td style="text-align:left;"> Brazil </td>
   <td style="text-align:left;"> BRA </td>
   <td style="text-align:right;"> 2007 </td>
   <td style="text-align:right;"> 2.2424996 </td>
   <td style="text-align:left;"> Latin America &amp; Caribbean </td>
   <td style="text-align:left;"> Americas </td>
  </tr>
  <tr>
   <td style="text-align:left;"> Malta </td>
   <td style="text-align:left;"> MLT </td>
   <td style="text-align:right;"> 1991 </td>
   <td style="text-align:right;"> 0.1342061 </td>
   <td style="text-align:left;"> Middle East &amp; North Africa </td>
   <td style="text-align:left;"> Europe </td>
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
   <td style="text-align:left;"> Turkmenistan </td>
   <td style="text-align:left;"> TKM </td>
   <td style="text-align:right;"> 1995 </td>
   <td style="text-align:right;"> 0.1669222 </td>
   <td style="text-align:left;"> Europe &amp; Central Asia </td>
   <td style="text-align:left;"> Asia </td>
  </tr>
  <tr>
   <td style="text-align:left;"> Chad </td>
   <td style="text-align:left;"> TCD </td>
   <td style="text-align:right;"> 2014 </td>
   <td style="text-align:right;"> 14.1679253 </td>
   <td style="text-align:left;"> Sub-Saharan Africa </td>
   <td style="text-align:left;"> Africa </td>
  </tr>
  <tr>
   <td style="text-align:left;"> China </td>
   <td style="text-align:left;"> CHN </td>
   <td style="text-align:right;"> 2015 </td>
   <td style="text-align:right;"> 4.4296377 </td>
   <td style="text-align:left;"> East Asia &amp; Pacific </td>
   <td style="text-align:left;"> Asia </td>
  </tr>
  <tr>
   <td style="text-align:left;"> Angola </td>
   <td style="text-align:left;"> AGO </td>
   <td style="text-align:right;"> 1998 </td>
   <td style="text-align:right;"> 11.2111734 </td>
   <td style="text-align:left;"> Sub-Saharan Africa </td>
   <td style="text-align:left;"> Africa </td>
  </tr>
  <tr>
   <td style="text-align:left;"> Indonesia </td>
   <td style="text-align:left;"> IDN </td>
   <td style="text-align:right;"> 2007 </td>
   <td style="text-align:right;"> 7.1677427 </td>
   <td style="text-align:left;"> East Asia &amp; Pacific </td>
   <td style="text-align:left;"> Asia </td>
  </tr>
  <tr>
   <td style="text-align:left;"> Malawi </td>
   <td style="text-align:left;"> MWI </td>
   <td style="text-align:right;"> 2001 </td>
   <td style="text-align:right;"> 9.1691905 </td>
   <td style="text-align:left;"> Sub-Saharan Africa </td>
   <td style="text-align:left;"> Africa </td>
  </tr>
  <tr>
   <td style="text-align:left;"> El Salvador </td>
   <td style="text-align:left;"> SLV </td>
   <td style="text-align:right;"> 2006 </td>
   <td style="text-align:right;"> 4.6541503 </td>
   <td style="text-align:left;"> Latin America &amp; Caribbean </td>
   <td style="text-align:left;"> Americas </td>
  </tr>
  <tr>
   <td style="text-align:left;"> American Samoa </td>
   <td style="text-align:left;"> ASM </td>
   <td style="text-align:right;"> 2016 </td>
   <td style="text-align:right;"> 1.7100601 </td>
   <td style="text-align:left;"> East Asia &amp; Pacific </td>
   <td style="text-align:left;"> Oceania </td>
  </tr>
  <tr>
   <td style="text-align:left;"> Chad </td>
   <td style="text-align:left;"> TCD </td>
   <td style="text-align:right;"> 1991 </td>
   <td style="text-align:right;"> 15.0199634 </td>
   <td style="text-align:left;"> Sub-Saharan Africa </td>
   <td style="text-align:left;"> Africa </td>
  </tr>
  <tr>
   <td style="text-align:left;"> Guinea-Bissau </td>
   <td style="text-align:left;"> GNB </td>
   <td style="text-align:right;"> 2006 </td>
   <td style="text-align:right;"> 13.5442665 </td>
   <td style="text-align:left;"> Sub-Saharan Africa </td>
   <td style="text-align:left;"> Africa </td>
  </tr>
  <tr>
   <td style="text-align:left;"> Bahrain </td>
   <td style="text-align:left;"> BHR </td>
   <td style="text-align:right;"> 2018 </td>
   <td style="text-align:right;"> 0.0459195 </td>
   <td style="text-align:left;"> Middle East &amp; North Africa </td>
   <td style="text-align:left;"> Asia </td>
  </tr>
  <tr>
   <td style="text-align:left;"> Malta </td>
   <td style="text-align:left;"> MLT </td>
   <td style="text-align:right;"> 1995 </td>
   <td style="text-align:right;"> 0.0924073 </td>
   <td style="text-align:left;"> Middle East &amp; North Africa </td>
   <td style="text-align:left;"> Europe </td>
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
   <td style="text-align:left;"> Tonga </td>
   <td style="text-align:left;"> TON </td>
   <td style="text-align:right;"> 2016 </td>
   <td style="text-align:right;"> 5.5238530 </td>
   <td style="text-align:left;"> East Asia &amp; Pacific </td>
   <td style="text-align:left;"> Oceania </td>
  </tr>
  <tr>
   <td style="text-align:left;"> Turkmenistan </td>
   <td style="text-align:left;"> TKM </td>
   <td style="text-align:right;"> 1993 </td>
   <td style="text-align:right;"> 0.1864338 </td>
   <td style="text-align:left;"> Europe &amp; Central Asia </td>
   <td style="text-align:left;"> Asia </td>
  </tr>
  <tr>
   <td style="text-align:left;"> North Macedonia </td>
   <td style="text-align:left;"> MKD </td>
   <td style="text-align:right;"> 1999 </td>
   <td style="text-align:right;"> 5.0065987 </td>
   <td style="text-align:left;"> Europe &amp; Central Asia </td>
   <td style="text-align:left;"> Europe </td>
  </tr>
  <tr>
   <td style="text-align:left;"> Kuwait </td>
   <td style="text-align:left;"> KWT </td>
   <td style="text-align:right;"> 2017 </td>
   <td style="text-align:right;"> 0.0067576 </td>
   <td style="text-align:left;"> Middle East &amp; North Africa </td>
   <td style="text-align:left;"> Asia </td>
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
   <td style="text-align:left;"> Tuvalu </td>
   <td style="text-align:left;"> TUV </td>
   <td style="text-align:right;"> 1992 </td>
   <td style="text-align:right;"> 9.8398013 </td>
   <td style="text-align:left;"> East Asia &amp; Pacific </td>
   <td style="text-align:left;"> Oceania </td>
  </tr>
  <tr>
   <td style="text-align:left;"> Comoros </td>
   <td style="text-align:left;"> COM </td>
   <td style="text-align:right;"> 2013 </td>
   <td style="text-align:right;"> 11.8880480 </td>
   <td style="text-align:left;"> Sub-Saharan Africa </td>
   <td style="text-align:left;"> Africa </td>
  </tr>
  <tr>
   <td style="text-align:left;"> Chile </td>
   <td style="text-align:left;"> CHL </td>
   <td style="text-align:right;"> 2009 </td>
   <td style="text-align:right;"> 0.5975054 </td>
   <td style="text-align:left;"> Latin America &amp; Caribbean </td>
   <td style="text-align:left;"> Americas </td>
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
   <td style="text-align:left;"> Niger </td>
   <td style="text-align:left;"> NER </td>
   <td style="text-align:right;"> 2007 </td>
   <td style="text-align:right;"> 15.2492386 </td>
   <td style="text-align:left;"> Sub-Saharan Africa </td>
   <td style="text-align:left;"> Africa </td>
  </tr>
  <tr>
   <td style="text-align:left;"> United States </td>
   <td style="text-align:left;"> USA </td>
   <td style="text-align:right;"> 1998 </td>
   <td style="text-align:right;"> 0.0136096 </td>
   <td style="text-align:left;"> North America </td>
   <td style="text-align:left;"> Americas </td>
  </tr>
  <tr>
   <td style="text-align:left;"> Mongolia </td>
   <td style="text-align:left;"> MNG </td>
   <td style="text-align:right;"> 2005 </td>
   <td style="text-align:right;"> 9.5671184 </td>
   <td style="text-align:left;"> East Asia &amp; Pacific </td>
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
   <td style="text-align:left;"> Antigua and Barbuda </td>
   <td style="text-align:left;"> ATG </td>
   <td style="text-align:right;"> 1993 </td>
   <td style="text-align:right;"> 0.4967074 </td>
   <td style="text-align:left;"> Latin America &amp; Caribbean </td>
   <td style="text-align:left;"> Americas </td>
  </tr>
  <tr>
   <td style="text-align:left;"> Djibouti </td>
   <td style="text-align:left;"> DJI </td>
   <td style="text-align:right;"> 2013 </td>
   <td style="text-align:right;"> 3.6484563 </td>
   <td style="text-align:left;"> Middle East &amp; North Africa </td>
   <td style="text-align:left;"> Africa </td>
  </tr>
  <tr>
   <td style="text-align:left;"> Nepal </td>
   <td style="text-align:left;"> NPL </td>
   <td style="text-align:right;"> 1993 </td>
   <td style="text-align:right;"> 19.3239966 </td>
   <td style="text-align:left;"> South Asia </td>
   <td style="text-align:left;"> Asia </td>
  </tr>
  <tr>
   <td style="text-align:left;"> Nauru </td>
   <td style="text-align:left;"> NRU </td>
   <td style="text-align:right;"> 2008 </td>
   <td style="text-align:right;"> 1.6895440 </td>
   <td style="text-align:left;"> East Asia &amp; Pacific </td>
   <td style="text-align:left;"> Oceania </td>
  </tr>
  <tr>
   <td style="text-align:left;"> Uganda </td>
   <td style="text-align:left;"> UGA </td>
   <td style="text-align:right;"> 1998 </td>
   <td style="text-align:right;"> 9.8315678 </td>
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
   <td style="text-align:left;"> Togo </td>
   <td style="text-align:left;"> TGO </td>
   <td style="text-align:right;"> 2017 </td>
   <td style="text-align:right;"> 11.2566248 </td>
   <td style="text-align:left;"> Sub-Saharan Africa </td>
   <td style="text-align:left;"> Africa </td>
  </tr>
  <tr>
   <td style="text-align:left;"> El Salvador </td>
   <td style="text-align:left;"> SLV </td>
   <td style="text-align:right;"> 2019 </td>
   <td style="text-align:right;"> 1.5562496 </td>
   <td style="text-align:left;"> Latin America &amp; Caribbean </td>
   <td style="text-align:left;"> Americas </td>
  </tr>
  <tr>
   <td style="text-align:left;"> Fiji </td>
   <td style="text-align:left;"> FJI </td>
   <td style="text-align:right;"> 2008 </td>
   <td style="text-align:right;"> 6.5842287 </td>
   <td style="text-align:left;"> East Asia &amp; Pacific </td>
   <td style="text-align:left;"> Oceania </td>
  </tr>
  <tr>
   <td style="text-align:left;"> Namibia </td>
   <td style="text-align:left;"> NAM </td>
   <td style="text-align:right;"> 2004 </td>
   <td style="text-align:right;"> 6.0314979 </td>
   <td style="text-align:left;"> Sub-Saharan Africa </td>
   <td style="text-align:left;"> Africa </td>
  </tr>
  <tr>
   <td style="text-align:left;"> Panama </td>
   <td style="text-align:left;"> PAN </td>
   <td style="text-align:right;"> 1994 </td>
   <td style="text-align:right;"> 4.0677751 </td>
   <td style="text-align:left;"> Latin America &amp; Caribbean </td>
   <td style="text-align:left;"> Americas </td>
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
   <td style="text-align:left;"> Gambia </td>
   <td style="text-align:left;"> GMB </td>
   <td style="text-align:right;"> 1995 </td>
   <td style="text-align:right;"> 14.5561118 </td>
   <td style="text-align:left;"> Sub-Saharan Africa </td>
   <td style="text-align:left;"> Africa </td>
  </tr>
  <tr>
   <td style="text-align:left;"> Saint Lucia </td>
   <td style="text-align:left;"> LCA </td>
   <td style="text-align:right;"> 1997 </td>
   <td style="text-align:right;"> 2.6960222 </td>
   <td style="text-align:left;"> Latin America &amp; Caribbean </td>
   <td style="text-align:left;"> Americas </td>
  </tr>
  <tr>
   <td style="text-align:left;"> Tuvalu </td>
   <td style="text-align:left;"> TUV </td>
   <td style="text-align:right;"> 2019 </td>
   <td style="text-align:right;"> 1.5159753 </td>
   <td style="text-align:left;"> East Asia &amp; Pacific </td>
   <td style="text-align:left;"> Oceania </td>
  </tr>
  <tr>
   <td style="text-align:left;"> Czechia </td>
   <td style="text-align:left;"> CZE </td>
   <td style="text-align:right;"> 2011 </td>
   <td style="text-align:right;"> 0.0957588 </td>
   <td style="text-align:left;"> Europe &amp; Central Asia </td>
   <td style="text-align:left;"> Europe </td>
  </tr>
  <tr>
   <td style="text-align:left;"> Bulgaria </td>
   <td style="text-align:left;"> BGR </td>
   <td style="text-align:right;"> 2008 </td>
   <td style="text-align:right;"> 1.9522489 </td>
   <td style="text-align:left;"> Europe &amp; Central Asia </td>
   <td style="text-align:left;"> Europe </td>
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
   <td style="text-align:left;"> Comoros </td>
   <td style="text-align:left;"> COM </td>
   <td style="text-align:right;"> 2010 </td>
   <td style="text-align:right;"> 12.1082135 </td>
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
   <td style="text-align:left;"> Grenada </td>
   <td style="text-align:left;"> GRD </td>
   <td style="text-align:right;"> 2002 </td>
   <td style="text-align:right;"> 1.0016062 </td>
   <td style="text-align:left;"> Latin America &amp; Caribbean </td>
   <td style="text-align:left;"> Americas </td>
  </tr>
  <tr>
   <td style="text-align:left;"> Vanuatu </td>
   <td style="text-align:left;"> VUT </td>
   <td style="text-align:right;"> 2013 </td>
   <td style="text-align:right;"> 16.9492812 </td>
   <td style="text-align:left;"> East Asia &amp; Pacific </td>
   <td style="text-align:left;"> Oceania </td>
  </tr>
  <tr>
   <td style="text-align:left;"> Eswatini </td>
   <td style="text-align:left;"> SWZ </td>
   <td style="text-align:right;"> 1999 </td>
   <td style="text-align:right;"> 7.2040012 </td>
   <td style="text-align:left;"> Sub-Saharan Africa </td>
   <td style="text-align:left;"> Africa </td>
  </tr>
  <tr>
   <td style="text-align:left;"> Greece </td>
   <td style="text-align:left;"> GRC </td>
   <td style="text-align:right;"> 2017 </td>
   <td style="text-align:right;"> 0.0354547 </td>
   <td style="text-align:left;"> Europe &amp; Central Asia </td>
   <td style="text-align:left;"> Europe </td>
  </tr>
  <tr>
   <td style="text-align:left;"> Ecuador </td>
   <td style="text-align:left;"> ECU </td>
   <td style="text-align:right;"> 2003 </td>
   <td style="text-align:right;"> 1.6175839 </td>
   <td style="text-align:left;"> Latin America &amp; Caribbean </td>
   <td style="text-align:left;"> Americas </td>
  </tr>
  <tr>
   <td style="text-align:left;"> Ukraine </td>
   <td style="text-align:left;"> UKR </td>
   <td style="text-align:right;"> 1999 </td>
   <td style="text-align:right;"> 1.0063473 </td>
   <td style="text-align:left;"> Europe &amp; Central Asia </td>
   <td style="text-align:left;"> Europe </td>
  </tr>
  <tr>
   <td style="text-align:left;"> Brunei </td>
   <td style="text-align:left;"> BRN </td>
   <td style="text-align:right;"> 2015 </td>
   <td style="text-align:right;"> 0.0526530 </td>
   <td style="text-align:left;"> East Asia &amp; Pacific </td>
   <td style="text-align:left;"> Asia </td>
  </tr>
  <tr>
   <td style="text-align:left;"> Iceland </td>
   <td style="text-align:left;"> ISL </td>
   <td style="text-align:right;"> 2017 </td>
   <td style="text-align:right;"> 0.0037281 </td>
   <td style="text-align:left;"> Europe &amp; Central Asia </td>
   <td style="text-align:left;"> Europe </td>
  </tr>
  <tr>
   <td style="text-align:left;"> Taiwan </td>
   <td style="text-align:left;"> TWN </td>
   <td style="text-align:right;"> 1993 </td>
   <td style="text-align:right;"> 2.4328216 </td>
   <td style="text-align:left;"> East Asia &amp; Pacific </td>
   <td style="text-align:left;"> Asia </td>
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
   <td style="text-align:left;"> Congo </td>
   <td style="text-align:left;"> COG </td>
   <td style="text-align:right;"> 1995 </td>
   <td style="text-align:right;"> 9.6343054 </td>
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
   <td style="text-align:left;"> Guinea </td>
   <td style="text-align:left;"> GIN </td>
   <td style="text-align:right;"> 2004 </td>
   <td style="text-align:right;"> 13.7803404 </td>
   <td style="text-align:left;"> Sub-Saharan Africa </td>
   <td style="text-align:left;"> Africa </td>
  </tr>
  <tr>
   <td style="text-align:left;"> Seychelles </td>
   <td style="text-align:left;"> SYC </td>
   <td style="text-align:right;"> 1998 </td>
   <td style="text-align:right;"> 0.3342432 </td>
   <td style="text-align:left;"> Sub-Saharan Africa </td>
   <td style="text-align:left;"> Africa </td>
  </tr>
  <tr>
   <td style="text-align:left;"> Burundi </td>
   <td style="text-align:left;"> BDI </td>
   <td style="text-align:right;"> 2013 </td>
   <td style="text-align:right;"> 14.2220858 </td>
   <td style="text-align:left;"> Sub-Saharan Africa </td>
   <td style="text-align:left;"> Africa </td>
  </tr>
  <tr>
   <td style="text-align:left;"> Moldova </td>
   <td style="text-align:left;"> MDA </td>
   <td style="text-align:right;"> 2002 </td>
   <td style="text-align:right;"> 5.2427758 </td>
   <td style="text-align:left;"> Europe &amp; Central Asia </td>
   <td style="text-align:left;"> Europe </td>
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
   <td style="text-align:left;"> Algeria </td>
   <td style="text-align:left;"> DZA </td>
   <td style="text-align:right;"> 2016 </td>
   <td style="text-align:right;"> 0.0491542 </td>
   <td style="text-align:left;"> Middle East &amp; North Africa </td>
   <td style="text-align:left;"> Africa </td>
  </tr>
  <tr>
   <td style="text-align:left;"> Indonesia </td>
   <td style="text-align:left;"> IDN </td>
   <td style="text-align:right;"> 2002 </td>
   <td style="text-align:right;"> 7.6785741 </td>
   <td style="text-align:left;"> East Asia &amp; Pacific </td>
   <td style="text-align:left;"> Asia </td>
  </tr>
  <tr>
   <td style="text-align:left;"> Tokelau </td>
   <td style="text-align:left;"> TKL </td>
   <td style="text-align:right;"> 2019 </td>
   <td style="text-align:right;"> 0.0057128 </td>
   <td style="text-align:left;"> East Asia &amp; Pacific </td>
   <td style="text-align:left;"> Oceania </td>
  </tr>
  <tr>
   <td style="text-align:left;"> Lithuania </td>
   <td style="text-align:left;"> LTU </td>
   <td style="text-align:right;"> 2009 </td>
   <td style="text-align:right;"> 0.2912664 </td>
   <td style="text-align:left;"> Europe &amp; Central Asia </td>
   <td style="text-align:left;"> Europe </td>
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
   <td style="text-align:left;"> Colombia </td>
   <td style="text-align:left;"> COL </td>
   <td style="text-align:right;"> 2016 </td>
   <td style="text-align:right;"> 1.2658706 </td>
   <td style="text-align:left;"> Latin America &amp; Caribbean </td>
   <td style="text-align:left;"> Americas </td>
  </tr>
  <tr>
   <td style="text-align:left;"> Mexico </td>
   <td style="text-align:left;"> MEX </td>
   <td style="text-align:right;"> 1990 </td>
   <td style="text-align:right;"> 3.9763899 </td>
   <td style="text-align:left;"> Latin America &amp; Caribbean </td>
   <td style="text-align:left;"> Americas </td>
  </tr>
  <tr>
   <td style="text-align:left;"> Nicaragua </td>
   <td style="text-align:left;"> NIC </td>
   <td style="text-align:right;"> 1993 </td>
   <td style="text-align:right;"> 12.2285878 </td>
   <td style="text-align:left;"> Latin America &amp; Caribbean </td>
   <td style="text-align:left;"> Americas </td>
  </tr>
  <tr>
   <td style="text-align:left;"> Mongolia </td>
   <td style="text-align:left;"> MNG </td>
   <td style="text-align:right;"> 2014 </td>
   <td style="text-align:right;"> 5.2379927 </td>
   <td style="text-align:left;"> East Asia &amp; Pacific </td>
   <td style="text-align:left;"> Asia </td>
  </tr>
  <tr>
   <td style="text-align:left;"> Eritrea </td>
   <td style="text-align:left;"> ERI </td>
   <td style="text-align:right;"> 2008 </td>
   <td style="text-align:right;"> 11.2032213 </td>
   <td style="text-align:left;"> Sub-Saharan Africa </td>
   <td style="text-align:left;"> Africa </td>
  </tr>
  <tr>
   <td style="text-align:left;"> Norway </td>
   <td style="text-align:left;"> NOR </td>
   <td style="text-align:right;"> 2007 </td>
   <td style="text-align:right;"> 0.0058233 </td>
   <td style="text-align:left;"> Europe &amp; Central Asia </td>
   <td style="text-align:left;"> Europe </td>
  </tr>
  <tr>
   <td style="text-align:left;"> Sweden </td>
   <td style="text-align:left;"> SWE </td>
   <td style="text-align:right;"> 1991 </td>
   <td style="text-align:right;"> 0.0293310 </td>
   <td style="text-align:left;"> Europe &amp; Central Asia </td>
   <td style="text-align:left;"> Europe </td>
  </tr>
  <tr>
   <td style="text-align:left;"> Azerbaijan </td>
   <td style="text-align:left;"> AZE </td>
   <td style="text-align:right;"> 2010 </td>
   <td style="text-align:right;"> 1.6287129 </td>
   <td style="text-align:left;"> Europe &amp; Central Asia </td>
   <td style="text-align:left;"> Asia </td>
  </tr>
  <tr>
   <td style="text-align:left;"> Belarus </td>
   <td style="text-align:left;"> BLR </td>
   <td style="text-align:right;"> 2013 </td>
   <td style="text-align:right;"> 0.1197601 </td>
   <td style="text-align:left;"> Europe &amp; Central Asia </td>
   <td style="text-align:left;"> Europe </td>
  </tr>
  <tr>
   <td style="text-align:left;"> Central African Republic </td>
   <td style="text-align:left;"> CAF </td>
   <td style="text-align:right;"> 1995 </td>
   <td style="text-align:right;"> 11.6455407 </td>
   <td style="text-align:left;"> Sub-Saharan Africa </td>
   <td style="text-align:left;"> Africa </td>
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
   <td style="text-align:left;"> Croatia </td>
   <td style="text-align:left;"> HRV </td>
   <td style="text-align:right;"> 2014 </td>
   <td style="text-align:right;"> 0.3974937 </td>
   <td style="text-align:left;"> Europe &amp; Central Asia </td>
   <td style="text-align:left;"> Europe </td>
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
   <td style="text-align:left;"> South Sudan </td>
   <td style="text-align:left;"> SSD </td>
   <td style="text-align:right;"> 2003 </td>
   <td style="text-align:right;"> 11.5738848 </td>
   <td style="text-align:left;"> Sub-Saharan Africa </td>
   <td style="text-align:left;"> Africa </td>
  </tr>
  <tr>
   <td style="text-align:left;"> Haiti </td>
   <td style="text-align:left;"> HTI </td>
   <td style="text-align:right;"> 1993 </td>
   <td style="text-align:right;"> 15.2964069 </td>
   <td style="text-align:left;"> Latin America &amp; Caribbean </td>
   <td style="text-align:left;"> Americas </td>
  </tr>
  <tr>
   <td style="text-align:left;"> Norway </td>
   <td style="text-align:left;"> NOR </td>
   <td style="text-align:right;"> 2017 </td>
   <td style="text-align:right;"> 0.0027308 </td>
   <td style="text-align:left;"> Europe &amp; Central Asia </td>
   <td style="text-align:left;"> Europe </td>
  </tr>
  <tr>
   <td style="text-align:left;"> Sri Lanka </td>
   <td style="text-align:left;"> LKA </td>
   <td style="text-align:right;"> 2013 </td>
   <td style="text-align:right;"> 6.2871000 </td>
   <td style="text-align:left;"> South Asia </td>
   <td style="text-align:left;"> Asia </td>
  </tr>
  <tr>
   <td style="text-align:left;"> Tajikistan </td>
   <td style="text-align:left;"> TJK </td>
   <td style="text-align:right;"> 2011 </td>
   <td style="text-align:right;"> 8.3618321 </td>
   <td style="text-align:left;"> Europe &amp; Central Asia </td>
   <td style="text-align:left;"> Asia </td>
  </tr>
  <tr>
   <td style="text-align:left;"> Ukraine </td>
   <td style="text-align:left;"> UKR </td>
   <td style="text-align:right;"> 1997 </td>
   <td style="text-align:right;"> 1.0488946 </td>
   <td style="text-align:left;"> Europe &amp; Central Asia </td>
   <td style="text-align:left;"> Europe </td>
  </tr>
  <tr>
   <td style="text-align:left;"> Tajikistan </td>
   <td style="text-align:left;"> TJK </td>
   <td style="text-align:right;"> 2003 </td>
   <td style="text-align:right;"> 11.4220973 </td>
   <td style="text-align:left;"> Europe &amp; Central Asia </td>
   <td style="text-align:left;"> Asia </td>
  </tr>
  <tr>
   <td style="text-align:left;"> Mongolia </td>
   <td style="text-align:left;"> MNG </td>
   <td style="text-align:right;"> 1999 </td>
   <td style="text-align:right;"> 11.0864008 </td>
   <td style="text-align:left;"> East Asia &amp; Pacific </td>
   <td style="text-align:left;"> Asia </td>
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
   <td style="text-align:left;"> Denmark </td>
   <td style="text-align:left;"> DNK </td>
   <td style="text-align:right;"> 2007 </td>
   <td style="text-align:right;"> 0.0069469 </td>
   <td style="text-align:left;"> Europe &amp; Central Asia </td>
   <td style="text-align:left;"> Europe </td>
  </tr>
  <tr>
   <td style="text-align:left;"> Hungary </td>
   <td style="text-align:left;"> HUN </td>
   <td style="text-align:right;"> 1998 </td>
   <td style="text-align:right;"> 1.9864583 </td>
   <td style="text-align:left;"> Europe &amp; Central Asia </td>
   <td style="text-align:left;"> Europe </td>
  </tr>
  <tr>
   <td style="text-align:left;"> Papua New Guinea </td>
   <td style="text-align:left;"> PNG </td>
   <td style="text-align:right;"> 1998 </td>
   <td style="text-align:right;"> 19.6549022 </td>
   <td style="text-align:left;"> East Asia &amp; Pacific </td>
   <td style="text-align:left;"> Oceania </td>
  </tr>
  <tr>
   <td style="text-align:left;"> Egypt </td>
   <td style="text-align:left;"> EGY </td>
   <td style="text-align:right;"> 2018 </td>
   <td style="text-align:right;"> 0.0147225 </td>
   <td style="text-align:left;"> Middle East &amp; North Africa </td>
   <td style="text-align:left;"> Africa </td>
  </tr>
  <tr>
   <td style="text-align:left;"> Grenada </td>
   <td style="text-align:left;"> GRD </td>
   <td style="text-align:right;"> 2018 </td>
   <td style="text-align:right;"> 0.3135422 </td>
   <td style="text-align:left;"> Latin America &amp; Caribbean </td>
   <td style="text-align:left;"> Americas </td>
  </tr>
  <tr>
   <td style="text-align:left;"> Senegal </td>
   <td style="text-align:left;"> SEN </td>
   <td style="text-align:right;"> 2006 </td>
   <td style="text-align:right;"> 10.8155892 </td>
   <td style="text-align:left;"> Sub-Saharan Africa </td>
   <td style="text-align:left;"> Africa </td>
  </tr>
  <tr>
   <td style="text-align:left;"> Gabon </td>
   <td style="text-align:left;"> GAB </td>
   <td style="text-align:right;"> 2013 </td>
   <td style="text-align:right;"> 1.3678415 </td>
   <td style="text-align:left;"> Sub-Saharan Africa </td>
   <td style="text-align:left;"> Africa </td>
  </tr>
  <tr>
   <td style="text-align:left;"> Fiji </td>
   <td style="text-align:left;"> FJI </td>
   <td style="text-align:right;"> 1992 </td>
   <td style="text-align:right;"> 10.2917629 </td>
   <td style="text-align:left;"> East Asia &amp; Pacific </td>
   <td style="text-align:left;"> Oceania </td>
  </tr>
  <tr>
   <td style="text-align:left;"> Brunei </td>
   <td style="text-align:left;"> BRN </td>
   <td style="text-align:right;"> 1998 </td>
   <td style="text-align:right;"> 0.3602534 </td>
   <td style="text-align:left;"> East Asia &amp; Pacific </td>
   <td style="text-align:left;"> Asia </td>
  </tr>
  <tr>
   <td style="text-align:left;"> Nicaragua </td>
   <td style="text-align:left;"> NIC </td>
   <td style="text-align:right;"> 2015 </td>
   <td style="text-align:right;"> 7.1273793 </td>
   <td style="text-align:left;"> Latin America &amp; Caribbean </td>
   <td style="text-align:left;"> Americas </td>
  </tr>
  <tr>
   <td style="text-align:left;"> Cuba </td>
   <td style="text-align:left;"> CUB </td>
   <td style="text-align:right;"> 1997 </td>
   <td style="text-align:right;"> 0.7518400 </td>
   <td style="text-align:left;"> Latin America &amp; Caribbean </td>
   <td style="text-align:left;"> Americas </td>
  </tr>
  <tr>
   <td style="text-align:left;"> Belarus </td>
   <td style="text-align:left;"> BLR </td>
   <td style="text-align:right;"> 2004 </td>
   <td style="text-align:right;"> 0.3590733 </td>
   <td style="text-align:left;"> Europe &amp; Central Asia </td>
   <td style="text-align:left;"> Europe </td>
  </tr>
  <tr>
   <td style="text-align:left;"> Solomon Islands </td>
   <td style="text-align:left;"> SLB </td>
   <td style="text-align:right;"> 1999 </td>
   <td style="text-align:right;"> 22.8794882 </td>
   <td style="text-align:left;"> East Asia &amp; Pacific </td>
   <td style="text-align:left;"> Oceania </td>
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
   <td style="text-align:left;"> Uruguay </td>
   <td style="text-align:left;"> URY </td>
   <td style="text-align:right;"> 2011 </td>
   <td style="text-align:right;"> 0.2684852 </td>
   <td style="text-align:left;"> Latin America &amp; Caribbean </td>
   <td style="text-align:left;"> Americas </td>
  </tr>
  <tr>
   <td style="text-align:left;"> Monaco </td>
   <td style="text-align:left;"> MCO </td>
   <td style="text-align:right;"> 1991 </td>
   <td style="text-align:right;"> 0.0103633 </td>
   <td style="text-align:left;"> Europe &amp; Central Asia </td>
   <td style="text-align:left;"> Europe </td>
  </tr>
  <tr>
   <td style="text-align:left;"> Estonia </td>
   <td style="text-align:left;"> EST </td>
   <td style="text-align:right;"> 2013 </td>
   <td style="text-align:right;"> 0.3657843 </td>
   <td style="text-align:left;"> Europe &amp; Central Asia </td>
   <td style="text-align:left;"> Europe </td>
  </tr>
  <tr>
   <td style="text-align:left;"> Italy </td>
   <td style="text-align:left;"> ITA </td>
   <td style="text-align:right;"> 1996 </td>
   <td style="text-align:right;"> 0.0851093 </td>
   <td style="text-align:left;"> Europe &amp; Central Asia </td>
   <td style="text-align:left;"> Europe </td>
  </tr>
  <tr>
   <td style="text-align:left;"> Estonia </td>
   <td style="text-align:left;"> EST </td>
   <td style="text-align:right;"> 2002 </td>
   <td style="text-align:right;"> 0.9831060 </td>
   <td style="text-align:left;"> Europe &amp; Central Asia </td>
   <td style="text-align:left;"> Europe </td>
  </tr>
  <tr>
   <td style="text-align:left;"> Myanmar </td>
   <td style="text-align:left;"> MMR </td>
   <td style="text-align:right;"> 2018 </td>
   <td style="text-align:right;"> 12.3683854 </td>
   <td style="text-align:left;"> East Asia &amp; Pacific </td>
   <td style="text-align:left;"> Asia </td>
  </tr>
  <tr>
   <td style="text-align:left;"> Paraguay </td>
   <td style="text-align:left;"> PRY </td>
   <td style="text-align:right;"> 2010 </td>
   <td style="text-align:right;"> 6.2344802 </td>
   <td style="text-align:left;"> Latin America &amp; Caribbean </td>
   <td style="text-align:left;"> Americas </td>
  </tr>
  <tr>
   <td style="text-align:left;"> South Korea </td>
   <td style="text-align:left;"> KOR </td>
   <td style="text-align:right;"> 1998 </td>
   <td style="text-align:right;"> 0.0535961 </td>
   <td style="text-align:left;"> East Asia &amp; Pacific </td>
   <td style="text-align:left;"> Asia </td>
  </tr>
  <tr>
   <td style="text-align:left;"> Papua New Guinea </td>
   <td style="text-align:left;"> PNG </td>
   <td style="text-align:right;"> 2015 </td>
   <td style="text-align:right;"> 18.6227771 </td>
   <td style="text-align:left;"> East Asia &amp; Pacific </td>
   <td style="text-align:left;"> Oceania </td>
  </tr>
  <tr>
   <td style="text-align:left;"> Myanmar </td>
   <td style="text-align:left;"> MMR </td>
   <td style="text-align:right;"> 1991 </td>
   <td style="text-align:right;"> 18.3445010 </td>
   <td style="text-align:left;"> East Asia &amp; Pacific </td>
   <td style="text-align:left;"> Asia </td>
  </tr>
  <tr>
   <td style="text-align:left;"> Malaysia </td>
   <td style="text-align:left;"> MYS </td>
   <td style="text-align:right;"> 1998 </td>
   <td style="text-align:right;"> 0.3844039 </td>
   <td style="text-align:left;"> East Asia &amp; Pacific </td>
   <td style="text-align:left;"> Asia </td>
  </tr>
  <tr>
   <td style="text-align:left;"> Yemen </td>
   <td style="text-align:left;"> YEM </td>
   <td style="text-align:right;"> 2010 </td>
   <td style="text-align:right;"> 9.4088512 </td>
   <td style="text-align:left;"> Middle East &amp; North Africa </td>
   <td style="text-align:left;"> Asia </td>
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
   <td style="text-align:left;"> Chad </td>
   <td style="text-align:left;"> TCD </td>
   <td style="text-align:right;"> 2003 </td>
   <td style="text-align:right;"> 14.2215819 </td>
   <td style="text-align:left;"> Sub-Saharan Africa </td>
   <td style="text-align:left;"> Africa </td>
  </tr>
  <tr>
   <td style="text-align:left;"> Afghanistan </td>
   <td style="text-align:left;"> AFG </td>
   <td style="text-align:right;"> 2012 </td>
   <td style="text-align:right;"> 16.3419810 </td>
   <td style="text-align:left;"> South Asia </td>
   <td style="text-align:left;"> Asia </td>
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
   <td style="text-align:left;"> Malawi </td>
   <td style="text-align:left;"> MWI </td>
   <td style="text-align:right;"> 2008 </td>
   <td style="text-align:right;"> 9.8257231 </td>
   <td style="text-align:left;"> Sub-Saharan Africa </td>
   <td style="text-align:left;"> Africa </td>
  </tr>
  <tr>
   <td style="text-align:left;"> Ukraine </td>
   <td style="text-align:left;"> UKR </td>
   <td style="text-align:right;"> 2006 </td>
   <td style="text-align:right;"> 0.7458095 </td>
   <td style="text-align:left;"> Europe &amp; Central Asia </td>
   <td style="text-align:left;"> Europe </td>
  </tr>
  <tr>
   <td style="text-align:left;"> Vietnam </td>
   <td style="text-align:left;"> VNM </td>
   <td style="text-align:right;"> 1996 </td>
   <td style="text-align:right;"> 13.6378818 </td>
   <td style="text-align:left;"> East Asia &amp; Pacific </td>
   <td style="text-align:left;"> Asia </td>
  </tr>
  <tr>
   <td style="text-align:left;"> Belize </td>
   <td style="text-align:left;"> BLZ </td>
   <td style="text-align:right;"> 2003 </td>
   <td style="text-align:right;"> 3.3807558 </td>
   <td style="text-align:left;"> Latin America &amp; Caribbean </td>
   <td style="text-align:left;"> Americas </td>
  </tr>
  <tr>
   <td style="text-align:left;"> South Korea </td>
   <td style="text-align:left;"> KOR </td>
   <td style="text-align:right;"> 2015 </td>
   <td style="text-align:right;"> 0.0061674 </td>
   <td style="text-align:left;"> East Asia &amp; Pacific </td>
   <td style="text-align:left;"> Asia </td>
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
   <td style="text-align:left;"> Turkey </td>
   <td style="text-align:left;"> TUR </td>
   <td style="text-align:right;"> 2010 </td>
   <td style="text-align:right;"> 0.1639389 </td>
   <td style="text-align:left;"> Europe &amp; Central Asia </td>
   <td style="text-align:left;"> Asia </td>
  </tr>
  <tr>
   <td style="text-align:left;"> Italy </td>
   <td style="text-align:left;"> ITA </td>
   <td style="text-align:right;"> 2010 </td>
   <td style="text-align:right;"> 0.0297198 </td>
   <td style="text-align:left;"> Europe &amp; Central Asia </td>
   <td style="text-align:left;"> Europe </td>
  </tr>
  <tr>
   <td style="text-align:left;"> Belgium </td>
   <td style="text-align:left;"> BEL </td>
   <td style="text-align:right;"> 2007 </td>
   <td style="text-align:right;"> 0.0110323 </td>
   <td style="text-align:left;"> Europe &amp; Central Asia </td>
   <td style="text-align:left;"> Europe </td>
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
   <td style="text-align:left;"> Estonia </td>
   <td style="text-align:left;"> EST </td>
   <td style="text-align:right;"> 1996 </td>
   <td style="text-align:right;"> 1.4287274 </td>
   <td style="text-align:left;"> Europe &amp; Central Asia </td>
   <td style="text-align:left;"> Europe </td>
  </tr>
  <tr>
   <td style="text-align:left;"> Rwanda </td>
   <td style="text-align:left;"> RWA </td>
   <td style="text-align:right;"> 1992 </td>
   <td style="text-align:right;"> 12.8287173 </td>
   <td style="text-align:left;"> Sub-Saharan Africa </td>
   <td style="text-align:left;"> Africa </td>
  </tr>
  <tr>
   <td style="text-align:left;"> Guinea-Bissau </td>
   <td style="text-align:left;"> GNB </td>
   <td style="text-align:right;"> 2014 </td>
   <td style="text-align:right;"> 13.6599469 </td>
   <td style="text-align:left;"> Sub-Saharan Africa </td>
   <td style="text-align:left;"> Africa </td>
  </tr>
  <tr>
   <td style="text-align:left;"> Northern Mariana Islands </td>
   <td style="text-align:left;"> MNP </td>
   <td style="text-align:right;"> 1997 </td>
   <td style="text-align:right;"> 1.5894918 </td>
   <td style="text-align:left;"> East Asia &amp; Pacific </td>
   <td style="text-align:left;"> Oceania </td>
  </tr>
  <tr>
   <td style="text-align:left;"> Cambodia </td>
   <td style="text-align:left;"> KHM </td>
   <td style="text-align:right;"> 2003 </td>
   <td style="text-align:right;"> 16.0265455 </td>
   <td style="text-align:left;"> East Asia &amp; Pacific </td>
   <td style="text-align:left;"> Asia </td>
  </tr>
  <tr>
   <td style="text-align:left;"> Costa Rica </td>
   <td style="text-align:left;"> CRI </td>
   <td style="text-align:right;"> 2001 </td>
   <td style="text-align:right;"> 1.4824857 </td>
   <td style="text-align:left;"> Latin America &amp; Caribbean </td>
   <td style="text-align:left;"> Americas </td>
  </tr>
  <tr>
   <td style="text-align:left;"> Somalia </td>
   <td style="text-align:left;"> SOM </td>
   <td style="text-align:right;"> 1993 </td>
   <td style="text-align:right;"> 15.3043747 </td>
   <td style="text-align:left;"> Sub-Saharan Africa </td>
   <td style="text-align:left;"> Africa </td>
  </tr>
  <tr>
   <td style="text-align:left;"> Georgia </td>
   <td style="text-align:left;"> GEO </td>
   <td style="text-align:right;"> 1998 </td>
   <td style="text-align:right;"> 7.9668452 </td>
   <td style="text-align:left;"> Europe &amp; Central Asia </td>
   <td style="text-align:left;"> Asia </td>
  </tr>
  <tr>
   <td style="text-align:left;"> Kyrgyzstan </td>
   <td style="text-align:left;"> KGZ </td>
   <td style="text-align:right;"> 2012 </td>
   <td style="text-align:right;"> 6.3118626 </td>
   <td style="text-align:left;"> Europe &amp; Central Asia </td>
   <td style="text-align:left;"> Asia </td>
  </tr>
  <tr>
   <td style="text-align:left;"> Mauritania </td>
   <td style="text-align:left;"> MRT </td>
   <td style="text-align:right;"> 2005 </td>
   <td style="text-align:right;"> 9.5510992 </td>
   <td style="text-align:left;"> Sub-Saharan Africa </td>
   <td style="text-align:left;"> Africa </td>
  </tr>
  <tr>
   <td style="text-align:left;"> Mexico </td>
   <td style="text-align:left;"> MEX </td>
   <td style="text-align:right;"> 1997 </td>
   <td style="text-align:right;"> 3.0514400 </td>
   <td style="text-align:left;"> Latin America &amp; Caribbean </td>
   <td style="text-align:left;"> Americas </td>
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
   <td style="text-align:left;"> Jamaica </td>
   <td style="text-align:left;"> JAM </td>
   <td style="text-align:right;"> 2006 </td>
   <td style="text-align:right;"> 1.7124655 </td>
   <td style="text-align:left;"> Latin America &amp; Caribbean </td>
   <td style="text-align:left;"> Americas </td>
  </tr>
  <tr>
   <td style="text-align:left;"> Norway </td>
   <td style="text-align:left;"> NOR </td>
   <td style="text-align:right;"> 1991 </td>
   <td style="text-align:right;"> 0.0215507 </td>
   <td style="text-align:left;"> Europe &amp; Central Asia </td>
   <td style="text-align:left;"> Europe </td>
  </tr>
  <tr>
   <td style="text-align:left;"> Eswatini </td>
   <td style="text-align:left;"> SWZ </td>
   <td style="text-align:right;"> 2010 </td>
   <td style="text-align:right;"> 5.4908490 </td>
   <td style="text-align:left;"> Sub-Saharan Africa </td>
   <td style="text-align:left;"> Africa </td>
  </tr>
  <tr>
   <td style="text-align:left;"> Saint Vincent and the Grenadines </td>
   <td style="text-align:left;"> VCT </td>
   <td style="text-align:right;"> 2007 </td>
   <td style="text-align:right;"> 0.8205803 </td>
   <td style="text-align:left;"> Latin America &amp; Caribbean </td>
   <td style="text-align:left;"> Americas </td>
  </tr>
  <tr>
   <td style="text-align:left;"> Germany </td>
   <td style="text-align:left;"> DEU </td>
   <td style="text-align:right;"> 2010 </td>
   <td style="text-align:right;"> 0.0044754 </td>
   <td style="text-align:left;"> Europe &amp; Central Asia </td>
   <td style="text-align:left;"> Europe </td>
  </tr>
  <tr>
   <td style="text-align:left;"> Barbados </td>
   <td style="text-align:left;"> BRB </td>
   <td style="text-align:right;"> 2010 </td>
   <td style="text-align:right;"> 0.0108307 </td>
   <td style="text-align:left;"> Latin America &amp; Caribbean </td>
   <td style="text-align:left;"> Americas </td>
  </tr>
  <tr>
   <td style="text-align:left;"> Tokelau </td>
   <td style="text-align:left;"> TKL </td>
   <td style="text-align:right;"> 1994 </td>
   <td style="text-align:right;"> 0.0588513 </td>
   <td style="text-align:left;"> East Asia &amp; Pacific </td>
   <td style="text-align:left;"> Oceania </td>
  </tr>
  <tr>
   <td style="text-align:left;"> Belgium </td>
   <td style="text-align:left;"> BEL </td>
   <td style="text-align:right;"> 1997 </td>
   <td style="text-align:right;"> 0.0241643 </td>
   <td style="text-align:left;"> Europe &amp; Central Asia </td>
   <td style="text-align:left;"> Europe </td>
  </tr>
  <tr>
   <td style="text-align:left;"> Kiribati </td>
   <td style="text-align:left;"> KIR </td>
   <td style="text-align:right;"> 1998 </td>
   <td style="text-align:right;"> 13.6712003 </td>
   <td style="text-align:left;"> East Asia &amp; Pacific </td>
   <td style="text-align:left;"> Oceania </td>
  </tr>
  <tr>
   <td style="text-align:left;"> Puerto Rico </td>
   <td style="text-align:left;"> PRI </td>
   <td style="text-align:right;"> 1998 </td>
   <td style="text-align:right;"> 0.0057400 </td>
   <td style="text-align:left;"> Latin America &amp; Caribbean </td>
   <td style="text-align:left;"> Americas </td>
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
   <td style="text-align:left;"> Slovakia </td>
   <td style="text-align:left;"> SVK </td>
   <td style="text-align:right;"> 1991 </td>
   <td style="text-align:right;"> 0.3380206 </td>
   <td style="text-align:left;"> Europe &amp; Central Asia </td>
   <td style="text-align:left;"> Europe </td>
  </tr>
  <tr>
   <td style="text-align:left;"> Nicaragua </td>
   <td style="text-align:left;"> NIC </td>
   <td style="text-align:right;"> 2019 </td>
   <td style="text-align:right;"> 6.2664545 </td>
   <td style="text-align:left;"> Latin America &amp; Caribbean </td>
   <td style="text-align:left;"> Americas </td>
  </tr>
  <tr>
   <td style="text-align:left;"> Cambodia </td>
   <td style="text-align:left;"> KHM </td>
   <td style="text-align:right;"> 2010 </td>
   <td style="text-align:right;"> 14.9050656 </td>
   <td style="text-align:left;"> East Asia &amp; Pacific </td>
   <td style="text-align:left;"> Asia </td>
  </tr>
  <tr>
   <td style="text-align:left;"> Kiribati </td>
   <td style="text-align:left;"> KIR </td>
   <td style="text-align:right;"> 2015 </td>
   <td style="text-align:right;"> 12.2424831 </td>
   <td style="text-align:left;"> East Asia &amp; Pacific </td>
   <td style="text-align:left;"> Oceania </td>
  </tr>
  <tr>
   <td style="text-align:left;"> Djibouti </td>
   <td style="text-align:left;"> DJI </td>
   <td style="text-align:right;"> 2012 </td>
   <td style="text-align:right;"> 3.7141825 </td>
   <td style="text-align:left;"> Middle East &amp; North Africa </td>
   <td style="text-align:left;"> Africa </td>
  </tr>
  <tr>
   <td style="text-align:left;"> Trinidad and Tobago </td>
   <td style="text-align:left;"> TTO </td>
   <td style="text-align:right;"> 1994 </td>
   <td style="text-align:right;"> 0.1062870 </td>
   <td style="text-align:left;"> Latin America &amp; Caribbean </td>
   <td style="text-align:left;"> Americas </td>
  </tr>
  <tr>
   <td style="text-align:left;"> Australia </td>
   <td style="text-align:left;"> AUS </td>
   <td style="text-align:right;"> 1996 </td>
   <td style="text-align:right;"> 0.0672701 </td>
   <td style="text-align:left;"> East Asia &amp; Pacific </td>
   <td style="text-align:left;"> Oceania </td>
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
   <td style="text-align:left;"> Togo </td>
   <td style="text-align:left;"> TGO </td>
   <td style="text-align:right;"> 1998 </td>
   <td style="text-align:right;"> 11.9795518 </td>
   <td style="text-align:left;"> Sub-Saharan Africa </td>
   <td style="text-align:left;"> Africa </td>
  </tr>
  <tr>
   <td style="text-align:left;"> Ghana </td>
   <td style="text-align:left;"> GHA </td>
   <td style="text-align:right;"> 1998 </td>
   <td style="text-align:right;"> 9.8744265 </td>
   <td style="text-align:left;"> Sub-Saharan Africa </td>
   <td style="text-align:left;"> Africa </td>
  </tr>
  <tr>
   <td style="text-align:left;"> Costa Rica </td>
   <td style="text-align:left;"> CRI </td>
   <td style="text-align:right;"> 2011 </td>
   <td style="text-align:right;"> 0.6790604 </td>
   <td style="text-align:left;"> Latin America &amp; Caribbean </td>
   <td style="text-align:left;"> Americas </td>
  </tr>
  <tr>
   <td style="text-align:left;"> North Korea </td>
   <td style="text-align:left;"> PRK </td>
   <td style="text-align:right;"> 2000 </td>
   <td style="text-align:right;"> 14.0933377 </td>
   <td style="text-align:left;"> East Asia &amp; Pacific </td>
   <td style="text-align:left;"> Asia </td>
  </tr>
  <tr>
   <td style="text-align:left;"> Mexico </td>
   <td style="text-align:left;"> MEX </td>
   <td style="text-align:right;"> 1996 </td>
   <td style="text-align:right;"> 3.2085859 </td>
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
   <td style="text-align:left;"> Namibia </td>
   <td style="text-align:left;"> NAM </td>
   <td style="text-align:right;"> 2010 </td>
   <td style="text-align:right;"> 6.1958219 </td>
   <td style="text-align:left;"> Sub-Saharan Africa </td>
   <td style="text-align:left;"> Africa </td>
  </tr>
  <tr>
   <td style="text-align:left;"> Malawi </td>
   <td style="text-align:left;"> MWI </td>
   <td style="text-align:right;"> 1998 </td>
   <td style="text-align:right;"> 9.5154026 </td>
   <td style="text-align:left;"> Sub-Saharan Africa </td>
   <td style="text-align:left;"> Africa </td>
  </tr>
  <tr>
   <td style="text-align:left;"> Uganda </td>
   <td style="text-align:left;"> UGA </td>
   <td style="text-align:right;"> 2017 </td>
   <td style="text-align:right;"> 11.0622373 </td>
   <td style="text-align:left;"> Sub-Saharan Africa </td>
   <td style="text-align:left;"> Africa </td>
  </tr>
  <tr>
   <td style="text-align:left;"> Brunei </td>
   <td style="text-align:left;"> BRN </td>
   <td style="text-align:right;"> 2018 </td>
   <td style="text-align:right;"> 0.0394396 </td>
   <td style="text-align:left;"> East Asia &amp; Pacific </td>
   <td style="text-align:left;"> Asia </td>
  </tr>
  <tr>
   <td style="text-align:left;"> Armenia </td>
   <td style="text-align:left;"> ARM </td>
   <td style="text-align:right;"> 2013 </td>
   <td style="text-align:right;"> 0.4532683 </td>
   <td style="text-align:left;"> Europe &amp; Central Asia </td>
   <td style="text-align:left;"> Asia </td>
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
   <td style="text-align:left;"> India </td>
   <td style="text-align:left;"> IND </td>
   <td style="text-align:right;"> 2000 </td>
   <td style="text-align:right;"> 12.2799871 </td>
   <td style="text-align:left;"> South Asia </td>
   <td style="text-align:left;"> Asia </td>
  </tr>
  <tr>
   <td style="text-align:left;"> Chad </td>
   <td style="text-align:left;"> TCD </td>
   <td style="text-align:right;"> 1990 </td>
   <td style="text-align:right;"> 15.1731836 </td>
   <td style="text-align:left;"> Sub-Saharan Africa </td>
   <td style="text-align:left;"> Africa </td>
  </tr>
  <tr>
   <td style="text-align:left;"> Guinea </td>
   <td style="text-align:left;"> GIN </td>
   <td style="text-align:right;"> 1994 </td>
   <td style="text-align:right;"> 14.7775953 </td>
   <td style="text-align:left;"> Sub-Saharan Africa </td>
   <td style="text-align:left;"> Africa </td>
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
   <td style="text-align:left;"> Taiwan </td>
   <td style="text-align:left;"> TWN </td>
   <td style="text-align:right;"> 2012 </td>
   <td style="text-align:right;"> 0.3756513 </td>
   <td style="text-align:left;"> East Asia &amp; Pacific </td>
   <td style="text-align:left;"> Asia </td>
  </tr>
  <tr>
   <td style="text-align:left;"> Bolivia </td>
   <td style="text-align:left;"> BOL </td>
   <td style="text-align:right;"> 1993 </td>
   <td style="text-align:right;"> 7.3873424 </td>
   <td style="text-align:left;"> Latin America &amp; Caribbean </td>
   <td style="text-align:left;"> Americas </td>
  </tr>
  <tr>
   <td style="text-align:left;"> Sudan </td>
   <td style="text-align:left;"> SDN </td>
   <td style="text-align:right;"> 2009 </td>
   <td style="text-align:right;"> 11.6199007 </td>
   <td style="text-align:left;"> Sub-Saharan Africa </td>
   <td style="text-align:left;"> Africa </td>
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
   <td style="text-align:left;"> Togo </td>
   <td style="text-align:left;"> TGO </td>
   <td style="text-align:right;"> 2011 </td>
   <td style="text-align:right;"> 11.6856748 </td>
   <td style="text-align:left;"> Sub-Saharan Africa </td>
   <td style="text-align:left;"> Africa </td>
  </tr>
  <tr>
   <td style="text-align:left;"> Thailand </td>
   <td style="text-align:left;"> THA </td>
   <td style="text-align:right;"> 1992 </td>
   <td style="text-align:right;"> 7.0895600 </td>
   <td style="text-align:left;"> East Asia &amp; Pacific </td>
   <td style="text-align:left;"> Asia </td>
  </tr>
  <tr>
   <td style="text-align:left;"> Israel </td>
   <td style="text-align:left;"> ISR </td>
   <td style="text-align:right;"> 2012 </td>
   <td style="text-align:right;"> 0.0103194 </td>
   <td style="text-align:left;"> Middle East &amp; North Africa </td>
   <td style="text-align:left;"> Asia </td>
  </tr>
  <tr>
   <td style="text-align:left;"> Iceland </td>
   <td style="text-align:left;"> ISL </td>
   <td style="text-align:right;"> 2004 </td>
   <td style="text-align:right;"> 0.0106592 </td>
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
   <td style="text-align:left;"> Norway </td>
   <td style="text-align:left;"> NOR </td>
   <td style="text-align:right;"> 1998 </td>
   <td style="text-align:right;"> 0.0124193 </td>
   <td style="text-align:left;"> Europe &amp; Central Asia </td>
   <td style="text-align:left;"> Europe </td>
  </tr>
  <tr>
   <td style="text-align:left;"> Andorra </td>
   <td style="text-align:left;"> AND </td>
   <td style="text-align:right;"> 2009 </td>
   <td style="text-align:right;"> 0.0056606 </td>
   <td style="text-align:left;"> Europe &amp; Central Asia </td>
   <td style="text-align:left;"> Europe </td>
  </tr>
  <tr>
   <td style="text-align:left;"> Cambodia </td>
   <td style="text-align:left;"> KHM </td>
   <td style="text-align:right;"> 2000 </td>
   <td style="text-align:right;"> 16.4939009 </td>
   <td style="text-align:left;"> East Asia &amp; Pacific </td>
   <td style="text-align:left;"> Asia </td>
  </tr>
  <tr>
   <td style="text-align:left;"> Cyprus </td>
   <td style="text-align:left;"> CYP </td>
   <td style="text-align:right;"> 1994 </td>
   <td style="text-align:right;"> 0.0943293 </td>
   <td style="text-align:left;"> Europe &amp; Central Asia </td>
   <td style="text-align:left;"> Asia </td>
  </tr>
  <tr>
   <td style="text-align:left;"> Ecuador </td>
   <td style="text-align:left;"> ECU </td>
   <td style="text-align:right;"> 2012 </td>
   <td style="text-align:right;"> 0.8580190 </td>
   <td style="text-align:left;"> Latin America &amp; Caribbean </td>
   <td style="text-align:left;"> Americas </td>
  </tr>
  <tr>
   <td style="text-align:left;"> Iran </td>
   <td style="text-align:left;"> IRN </td>
   <td style="text-align:right;"> 2010 </td>
   <td style="text-align:right;"> 0.0850665 </td>
   <td style="text-align:left;"> Middle East &amp; North Africa </td>
   <td style="text-align:left;"> Asia </td>
  </tr>
  <tr>
   <td style="text-align:left;"> Nepal </td>
   <td style="text-align:left;"> NPL </td>
   <td style="text-align:right;"> 2011 </td>
   <td style="text-align:right;"> 15.3957903 </td>
   <td style="text-align:left;"> South Asia </td>
   <td style="text-align:left;"> Asia </td>
  </tr>
  <tr>
   <td style="text-align:left;"> United States </td>
   <td style="text-align:left;"> USA </td>
   <td style="text-align:right;"> 2005 </td>
   <td style="text-align:right;"> 0.0105422 </td>
   <td style="text-align:left;"> North America </td>
   <td style="text-align:left;"> Americas </td>
  </tr>
  <tr>
   <td style="text-align:left;"> North Macedonia </td>
   <td style="text-align:left;"> MKD </td>
   <td style="text-align:right;"> 2019 </td>
   <td style="text-align:right;"> 2.0821503 </td>
   <td style="text-align:left;"> Europe &amp; Central Asia </td>
   <td style="text-align:left;"> Europe </td>
  </tr>
  <tr>
   <td style="text-align:left;"> Latvia </td>
   <td style="text-align:left;"> LVA </td>
   <td style="text-align:right;"> 2014 </td>
   <td style="text-align:right;"> 0.4394848 </td>
   <td style="text-align:left;"> Europe &amp; Central Asia </td>
   <td style="text-align:left;"> Europe </td>
  </tr>
  <tr>
   <td style="text-align:left;"> Tajikistan </td>
   <td style="text-align:left;"> TJK </td>
   <td style="text-align:right;"> 2015 </td>
   <td style="text-align:right;"> 6.5175735 </td>
   <td style="text-align:left;"> Europe &amp; Central Asia </td>
   <td style="text-align:left;"> Asia </td>
  </tr>
  <tr>
   <td style="text-align:left;"> Honduras </td>
   <td style="text-align:left;"> HND </td>
   <td style="text-align:right;"> 1999 </td>
   <td style="text-align:right;"> 10.7768316 </td>
   <td style="text-align:left;"> Latin America &amp; Caribbean </td>
   <td style="text-align:left;"> Americas </td>
  </tr>
  <tr>
   <td style="text-align:left;"> Senegal </td>
   <td style="text-align:left;"> SEN </td>
   <td style="text-align:right;"> 1999 </td>
   <td style="text-align:right;"> 10.0647276 </td>
   <td style="text-align:left;"> Sub-Saharan Africa </td>
   <td style="text-align:left;"> Africa </td>
  </tr>
  <tr>
   <td style="text-align:left;"> Iran </td>
   <td style="text-align:left;"> IRN </td>
   <td style="text-align:right;"> 2011 </td>
   <td style="text-align:right;"> 0.0739227 </td>
   <td style="text-align:left;"> Middle East &amp; North Africa </td>
   <td style="text-align:left;"> Asia </td>
  </tr>
  <tr>
   <td style="text-align:left;"> Eritrea </td>
   <td style="text-align:left;"> ERI </td>
   <td style="text-align:right;"> 2019 </td>
   <td style="text-align:right;"> 10.2382636 </td>
   <td style="text-align:left;"> Sub-Saharan Africa </td>
   <td style="text-align:left;"> Africa </td>
  </tr>
  <tr>
   <td style="text-align:left;"> Jamaica </td>
   <td style="text-align:left;"> JAM </td>
   <td style="text-align:right;"> 1991 </td>
   <td style="text-align:right;"> 7.2885524 </td>
   <td style="text-align:left;"> Latin America &amp; Caribbean </td>
   <td style="text-align:left;"> Americas </td>
  </tr>
  <tr>
   <td style="text-align:left;"> Albania </td>
   <td style="text-align:left;"> ALB </td>
   <td style="text-align:right;"> 2015 </td>
   <td style="text-align:right;"> 3.5925211 </td>
   <td style="text-align:left;"> Europe &amp; Central Asia </td>
   <td style="text-align:left;"> Europe </td>
  </tr>
  <tr>
   <td style="text-align:left;"> Latvia </td>
   <td style="text-align:left;"> LVA </td>
   <td style="text-align:right;"> 2010 </td>
   <td style="text-align:right;"> 0.5204009 </td>
   <td style="text-align:left;"> Europe &amp; Central Asia </td>
   <td style="text-align:left;"> Europe </td>
  </tr>
  <tr>
   <td style="text-align:left;"> Lithuania </td>
   <td style="text-align:left;"> LTU </td>
   <td style="text-align:right;"> 2002 </td>
   <td style="text-align:right;"> 0.5244295 </td>
   <td style="text-align:left;"> Europe &amp; Central Asia </td>
   <td style="text-align:left;"> Europe </td>
  </tr>
  <tr>
   <td style="text-align:left;"> Tajikistan </td>
   <td style="text-align:left;"> TJK </td>
   <td style="text-align:right;"> 1992 </td>
   <td style="text-align:right;"> 12.5992595 </td>
   <td style="text-align:left;"> Europe &amp; Central Asia </td>
   <td style="text-align:left;"> Asia </td>
  </tr>
  <tr>
   <td style="text-align:left;"> Kiribati </td>
   <td style="text-align:left;"> KIR </td>
   <td style="text-align:right;"> 2018 </td>
   <td style="text-align:right;"> 10.9992064 </td>
   <td style="text-align:left;"> East Asia &amp; Pacific </td>
   <td style="text-align:left;"> Oceania </td>
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
   <td style="text-align:left;"> Bhutan </td>
   <td style="text-align:left;"> BTN </td>
   <td style="text-align:right;"> 1999 </td>
   <td style="text-align:right;"> 16.5966771 </td>
   <td style="text-align:left;"> South Asia </td>
   <td style="text-align:left;"> Asia </td>
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
   <td style="text-align:left;"> Croatia </td>
   <td style="text-align:left;"> HRV </td>
   <td style="text-align:right;"> 2006 </td>
   <td style="text-align:right;"> 0.6420743 </td>
   <td style="text-align:left;"> Europe &amp; Central Asia </td>
   <td style="text-align:left;"> Europe </td>
  </tr>
  <tr>
   <td style="text-align:left;"> Dominica </td>
   <td style="text-align:left;"> DMA </td>
   <td style="text-align:right;"> 1994 </td>
   <td style="text-align:right;"> 3.5247247 </td>
   <td style="text-align:left;"> Latin America &amp; Caribbean </td>
   <td style="text-align:left;"> Americas </td>
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
   <td style="text-align:left;"> Uruguay </td>
   <td style="text-align:left;"> URY </td>
   <td style="text-align:right;"> 2009 </td>
   <td style="text-align:right;"> 0.3042811 </td>
   <td style="text-align:left;"> Latin America &amp; Caribbean </td>
   <td style="text-align:left;"> Americas </td>
  </tr>
  <tr>
   <td style="text-align:left;"> Ireland </td>
   <td style="text-align:left;"> IRL </td>
   <td style="text-align:right;"> 1994 </td>
   <td style="text-align:right;"> 0.0552709 </td>
   <td style="text-align:left;"> Europe &amp; Central Asia </td>
   <td style="text-align:left;"> Europe </td>
  </tr>
  <tr>
   <td style="text-align:left;"> Jamaica </td>
   <td style="text-align:left;"> JAM </td>
   <td style="text-align:right;"> 1996 </td>
   <td style="text-align:right;"> 4.5080322 </td>
   <td style="text-align:left;"> Latin America &amp; Caribbean </td>
   <td style="text-align:left;"> Americas </td>
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
   <td style="text-align:left;"> Finland </td>
   <td style="text-align:left;"> FIN </td>
   <td style="text-align:right;"> 2007 </td>
   <td style="text-align:right;"> 0.0108621 </td>
   <td style="text-align:left;"> Europe &amp; Central Asia </td>
   <td style="text-align:left;"> Europe </td>
  </tr>
  <tr>
   <td style="text-align:left;"> Grenada </td>
   <td style="text-align:left;"> GRD </td>
   <td style="text-align:right;"> 1998 </td>
   <td style="text-align:right;"> 1.8497998 </td>
   <td style="text-align:left;"> Latin America &amp; Caribbean </td>
   <td style="text-align:left;"> Americas </td>
  </tr>
  <tr>
   <td style="text-align:left;"> Iraq </td>
   <td style="text-align:left;"> IRQ </td>
   <td style="text-align:right;"> 2006 </td>
   <td style="text-align:right;"> 0.6956982 </td>
   <td style="text-align:left;"> Middle East &amp; North Africa </td>
   <td style="text-align:left;"> Asia </td>
  </tr>
  <tr>
   <td style="text-align:left;"> Algeria </td>
   <td style="text-align:left;"> DZA </td>
   <td style="text-align:right;"> 1999 </td>
   <td style="text-align:right;"> 0.7250376 </td>
   <td style="text-align:left;"> Middle East &amp; North Africa </td>
   <td style="text-align:left;"> Africa </td>
  </tr>
  <tr>
   <td style="text-align:left;"> Bermuda </td>
   <td style="text-align:left;"> BMU </td>
   <td style="text-align:right;"> 1994 </td>
   <td style="text-align:right;"> 0.9334103 </td>
   <td style="text-align:left;"> North America </td>
   <td style="text-align:left;"> Americas </td>
  </tr>
  <tr>
   <td style="text-align:left;"> Zambia </td>
   <td style="text-align:left;"> ZMB </td>
   <td style="text-align:right;"> 2011 </td>
   <td style="text-align:right;"> 9.0377804 </td>
   <td style="text-align:left;"> Sub-Saharan Africa </td>
   <td style="text-align:left;"> Africa </td>
  </tr>
  <tr>
   <td style="text-align:left;"> Madagascar </td>
   <td style="text-align:left;"> MDG </td>
   <td style="text-align:right;"> 2007 </td>
   <td style="text-align:right;"> 15.9176278 </td>
   <td style="text-align:left;"> Sub-Saharan Africa </td>
   <td style="text-align:left;"> Africa </td>
  </tr>
  <tr>
   <td style="text-align:left;"> Burkina Faso </td>
   <td style="text-align:left;"> BFA </td>
   <td style="text-align:right;"> 2015 </td>
   <td style="text-align:right;"> 12.8842204 </td>
   <td style="text-align:left;"> Sub-Saharan Africa </td>
   <td style="text-align:left;"> Africa </td>
  </tr>
  <tr>
   <td style="text-align:left;"> Belize </td>
   <td style="text-align:left;"> BLZ </td>
   <td style="text-align:right;"> 2017 </td>
   <td style="text-align:right;"> 1.7937447 </td>
   <td style="text-align:left;"> Latin America &amp; Caribbean </td>
   <td style="text-align:left;"> Americas </td>
  </tr>
  <tr>
   <td style="text-align:left;"> Rwanda </td>
   <td style="text-align:left;"> RWA </td>
   <td style="text-align:right;"> 1993 </td>
   <td style="text-align:right;"> 12.3357055 </td>
   <td style="text-align:left;"> Sub-Saharan Africa </td>
   <td style="text-align:left;"> Africa </td>
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
   <td style="text-align:left;"> Norway </td>
   <td style="text-align:left;"> NOR </td>
   <td style="text-align:right;"> 2005 </td>
   <td style="text-align:right;"> 0.0066274 </td>
   <td style="text-align:left;"> Europe &amp; Central Asia </td>
   <td style="text-align:left;"> Europe </td>
  </tr>
  <tr>
   <td style="text-align:left;"> Ethiopia </td>
   <td style="text-align:left;"> ETH </td>
   <td style="text-align:right;"> 2019 </td>
   <td style="text-align:right;"> 13.0824327 </td>
   <td style="text-align:left;"> Sub-Saharan Africa </td>
   <td style="text-align:left;"> Africa </td>
  </tr>
  <tr>
   <td style="text-align:left;"> France </td>
   <td style="text-align:left;"> FRA </td>
   <td style="text-align:right;"> 1992 </td>
   <td style="text-align:right;"> 0.0341629 </td>
   <td style="text-align:left;"> Europe &amp; Central Asia </td>
   <td style="text-align:left;"> Europe </td>
  </tr>
  <tr>
   <td style="text-align:left;"> Grenada </td>
   <td style="text-align:left;"> GRD </td>
   <td style="text-align:right;"> 1994 </td>
   <td style="text-align:right;"> 3.5347482 </td>
   <td style="text-align:left;"> Latin America &amp; Caribbean </td>
   <td style="text-align:left;"> Americas </td>
  </tr>
  <tr>
   <td style="text-align:left;"> Mongolia </td>
   <td style="text-align:left;"> MNG </td>
   <td style="text-align:right;"> 1995 </td>
   <td style="text-align:right;"> 11.6580195 </td>
   <td style="text-align:left;"> East Asia &amp; Pacific </td>
   <td style="text-align:left;"> Asia </td>
  </tr>
  <tr>
   <td style="text-align:left;"> Czechia </td>
   <td style="text-align:left;"> CZE </td>
   <td style="text-align:right;"> 1999 </td>
   <td style="text-align:right;"> 0.1698383 </td>
   <td style="text-align:left;"> Europe &amp; Central Asia </td>
   <td style="text-align:left;"> Europe </td>
  </tr>
  <tr>
   <td style="text-align:left;"> Central African Republic </td>
   <td style="text-align:left;"> CAF </td>
   <td style="text-align:right;"> 1993 </td>
   <td style="text-align:right;"> 12.4595630 </td>
   <td style="text-align:left;"> Sub-Saharan Africa </td>
   <td style="text-align:left;"> Africa </td>
  </tr>
  <tr>
   <td style="text-align:left;"> China </td>
   <td style="text-align:left;"> CHN </td>
   <td style="text-align:right;"> 2007 </td>
   <td style="text-align:right;"> 7.9277682 </td>
   <td style="text-align:left;"> East Asia &amp; Pacific </td>
   <td style="text-align:left;"> Asia </td>
  </tr>
  <tr>
   <td style="text-align:left;"> United Arab Emirates </td>
   <td style="text-align:left;"> ARE </td>
   <td style="text-align:right;"> 2007 </td>
   <td style="text-align:right;"> 0.0020616 </td>
   <td style="text-align:left;"> Middle East &amp; North Africa </td>
   <td style="text-align:left;"> Asia </td>
  </tr>
  <tr>
   <td style="text-align:left;"> El Salvador </td>
   <td style="text-align:left;"> SLV </td>
   <td style="text-align:right;"> 1991 </td>
   <td style="text-align:right;"> 9.3074417 </td>
   <td style="text-align:left;"> Latin America &amp; Caribbean </td>
   <td style="text-align:left;"> Americas </td>
  </tr>
  <tr>
   <td style="text-align:left;"> Cote d'Ivoire </td>
   <td style="text-align:left;"> CIV </td>
   <td style="text-align:right;"> 2009 </td>
   <td style="text-align:right;"> 10.5151660 </td>
   <td style="text-align:left;"> Sub-Saharan Africa </td>
   <td style="text-align:left;"> Africa </td>
  </tr>
  <tr>
   <td style="text-align:left;"> Montenegro </td>
   <td style="text-align:left;"> MNE </td>
   <td style="text-align:right;"> 1992 </td>
   <td style="text-align:right;"> 4.4440727 </td>
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
   <td style="text-align:left;"> Tunisia </td>
   <td style="text-align:left;"> TUN </td>
   <td style="text-align:right;"> 2006 </td>
   <td style="text-align:right;"> 0.2105741 </td>
   <td style="text-align:left;"> Middle East &amp; North Africa </td>
   <td style="text-align:left;"> Africa </td>
  </tr>
  <tr>
   <td style="text-align:left;"> Kyrgyzstan </td>
   <td style="text-align:left;"> KGZ </td>
   <td style="text-align:right;"> 1999 </td>
   <td style="text-align:right;"> 9.6641384 </td>
   <td style="text-align:left;"> Europe &amp; Central Asia </td>
   <td style="text-align:left;"> Asia </td>
  </tr>
  <tr>
   <td style="text-align:left;"> Azerbaijan </td>
   <td style="text-align:left;"> AZE </td>
   <td style="text-align:right;"> 2007 </td>
   <td style="text-align:right;"> 2.5926361 </td>
   <td style="text-align:left;"> Europe &amp; Central Asia </td>
   <td style="text-align:left;"> Asia </td>
  </tr>
  <tr>
   <td style="text-align:left;"> Cambodia </td>
   <td style="text-align:left;"> KHM </td>
   <td style="text-align:right;"> 1997 </td>
   <td style="text-align:right;"> 17.1888673 </td>
   <td style="text-align:left;"> East Asia &amp; Pacific </td>
   <td style="text-align:left;"> Asia </td>
  </tr>
  <tr>
   <td style="text-align:left;"> Japan </td>
   <td style="text-align:left;"> JPN </td>
   <td style="text-align:right;"> 2011 </td>
   <td style="text-align:right;"> 0.0090530 </td>
   <td style="text-align:left;"> East Asia &amp; Pacific </td>
   <td style="text-align:left;"> Asia </td>
  </tr>
  <tr>
   <td style="text-align:left;"> Samoa </td>
   <td style="text-align:left;"> WSM </td>
   <td style="text-align:right;"> 2003 </td>
   <td style="text-align:right;"> 12.8589139 </td>
   <td style="text-align:left;"> East Asia &amp; Pacific </td>
   <td style="text-align:left;"> Oceania </td>
  </tr>
  <tr>
   <td style="text-align:left;"> Kazakhstan </td>
   <td style="text-align:left;"> KAZ </td>
   <td style="text-align:right;"> 2008 </td>
   <td style="text-align:right;"> 2.1903308 </td>
   <td style="text-align:left;"> Europe &amp; Central Asia </td>
   <td style="text-align:left;"> Asia </td>
  </tr>
  <tr>
   <td style="text-align:left;"> Greenland </td>
   <td style="text-align:left;"> GRL </td>
   <td style="text-align:right;"> 2015 </td>
   <td style="text-align:right;"> 0.0827399 </td>
   <td style="text-align:left;"> Europe &amp; Central Asia </td>
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
   <td style="text-align:left;"> Spain </td>
   <td style="text-align:left;"> ESP </td>
   <td style="text-align:right;"> 2006 </td>
   <td style="text-align:right;"> 0.1042913 </td>
   <td style="text-align:left;"> Europe &amp; Central Asia </td>
   <td style="text-align:left;"> Europe </td>
  </tr>
  <tr>
   <td style="text-align:left;"> Comoros </td>
   <td style="text-align:left;"> COM </td>
   <td style="text-align:right;"> 2000 </td>
   <td style="text-align:right;"> 13.5348550 </td>
   <td style="text-align:left;"> Sub-Saharan Africa </td>
   <td style="text-align:left;"> Africa </td>
  </tr>
  <tr>
   <td style="text-align:left;"> Oman </td>
   <td style="text-align:left;"> OMN </td>
   <td style="text-align:right;"> 2002 </td>
   <td style="text-align:right;"> 0.8499383 </td>
   <td style="text-align:left;"> Middle East &amp; North Africa </td>
   <td style="text-align:left;"> Asia </td>
  </tr>
  <tr>
   <td style="text-align:left;"> Kyrgyzstan </td>
   <td style="text-align:left;"> KGZ </td>
   <td style="text-align:right;"> 2003 </td>
   <td style="text-align:right;"> 9.1033560 </td>
   <td style="text-align:left;"> Europe &amp; Central Asia </td>
   <td style="text-align:left;"> Asia </td>
  </tr>
  <tr>
   <td style="text-align:left;"> Syria </td>
   <td style="text-align:left;"> SYR </td>
   <td style="text-align:right;"> 1990 </td>
   <td style="text-align:right;"> 1.8977519 </td>
   <td style="text-align:left;"> Middle East &amp; North Africa </td>
   <td style="text-align:left;"> Asia </td>
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
   <td style="text-align:left;"> Philippines </td>
   <td style="text-align:left;"> PHL </td>
   <td style="text-align:right;"> 2019 </td>
   <td style="text-align:right;"> 7.0167477 </td>
   <td style="text-align:left;"> East Asia &amp; Pacific </td>
   <td style="text-align:left;"> Asia </td>
  </tr>
  <tr>
   <td style="text-align:left;"> Serbia </td>
   <td style="text-align:left;"> SRB </td>
   <td style="text-align:right;"> 2009 </td>
   <td style="text-align:right;"> 2.6718435 </td>
   <td style="text-align:left;"> Europe &amp; Central Asia </td>
   <td style="text-align:left;"> Europe </td>
  </tr>
  <tr>
   <td style="text-align:left;"> Turkmenistan </td>
   <td style="text-align:left;"> TKM </td>
   <td style="text-align:right;"> 2014 </td>
   <td style="text-align:right;"> 0.0258030 </td>
   <td style="text-align:left;"> Europe &amp; Central Asia </td>
   <td style="text-align:left;"> Asia </td>
  </tr>
  <tr>
   <td style="text-align:left;"> Colombia </td>
   <td style="text-align:left;"> COL </td>
   <td style="text-align:right;"> 2002 </td>
   <td style="text-align:right;"> 3.0296502 </td>
   <td style="text-align:left;"> Latin America &amp; Caribbean </td>
   <td style="text-align:left;"> Americas </td>
  </tr>
  <tr>
   <td style="text-align:left;"> Saint Kitts and Nevis </td>
   <td style="text-align:left;"> KNA </td>
   <td style="text-align:right;"> 2002 </td>
   <td style="text-align:right;"> 0.7102436 </td>
   <td style="text-align:left;"> Latin America &amp; Caribbean </td>
   <td style="text-align:left;"> Americas </td>
  </tr>
  <tr>
   <td style="text-align:left;"> Ecuador </td>
   <td style="text-align:left;"> ECU </td>
   <td style="text-align:right;"> 1993 </td>
   <td style="text-align:right;"> 3.2347097 </td>
   <td style="text-align:left;"> Latin America &amp; Caribbean </td>
   <td style="text-align:left;"> Americas </td>
  </tr>
  <tr>
   <td style="text-align:left;"> Central African Republic </td>
   <td style="text-align:left;"> CAF </td>
   <td style="text-align:right;"> 2014 </td>
   <td style="text-align:right;"> 11.0047958 </td>
   <td style="text-align:left;"> Sub-Saharan Africa </td>
   <td style="text-align:left;"> Africa </td>
  </tr>
  <tr>
   <td style="text-align:left;"> Morocco </td>
   <td style="text-align:left;"> MAR </td>
   <td style="text-align:right;"> 2005 </td>
   <td style="text-align:right;"> 3.4925394 </td>
   <td style="text-align:left;"> Middle East &amp; North Africa </td>
   <td style="text-align:left;"> Africa </td>
  </tr>
  <tr>
   <td style="text-align:left;"> Ethiopia </td>
   <td style="text-align:left;"> ETH </td>
   <td style="text-align:right;"> 1990 </td>
   <td style="text-align:right;"> 13.6790511 </td>
   <td style="text-align:left;"> Sub-Saharan Africa </td>
   <td style="text-align:left;"> Africa </td>
  </tr>
  <tr>
   <td style="text-align:left;"> Myanmar </td>
   <td style="text-align:left;"> MMR </td>
   <td style="text-align:right;"> 2001 </td>
   <td style="text-align:right;"> 17.2474953 </td>
   <td style="text-align:left;"> East Asia &amp; Pacific </td>
   <td style="text-align:left;"> Asia </td>
  </tr>
  <tr>
   <td style="text-align:left;"> Solomon Islands </td>
   <td style="text-align:left;"> SLB </td>
   <td style="text-align:right;"> 2008 </td>
   <td style="text-align:right;"> 20.7302387 </td>
   <td style="text-align:left;"> East Asia &amp; Pacific </td>
   <td style="text-align:left;"> Oceania </td>
  </tr>
  <tr>
   <td style="text-align:left;"> Bahamas </td>
   <td style="text-align:left;"> BHS </td>
   <td style="text-align:right;"> 2006 </td>
   <td style="text-align:right;"> 0.1750553 </td>
   <td style="text-align:left;"> Latin America &amp; Caribbean </td>
   <td style="text-align:left;"> Americas </td>
  </tr>
  <tr>
   <td style="text-align:left;"> Zambia </td>
   <td style="text-align:left;"> ZMB </td>
   <td style="text-align:right;"> 2005 </td>
   <td style="text-align:right;"> 8.1238025 </td>
   <td style="text-align:left;"> Sub-Saharan Africa </td>
   <td style="text-align:left;"> Africa </td>
  </tr>
  <tr>
   <td style="text-align:left;"> Sierra Leone </td>
   <td style="text-align:left;"> SLE </td>
   <td style="text-align:right;"> 2009 </td>
   <td style="text-align:right;"> 13.3844332 </td>
   <td style="text-align:left;"> Sub-Saharan Africa </td>
   <td style="text-align:left;"> Africa </td>
  </tr>
  <tr>
   <td style="text-align:left;"> Gambia </td>
   <td style="text-align:left;"> GMB </td>
   <td style="text-align:right;"> 2001 </td>
   <td style="text-align:right;"> 12.8027438 </td>
   <td style="text-align:left;"> Sub-Saharan Africa </td>
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
  
 kable() %>%
    kable_styling(bootstrap_options = c("hover", "striped"))
```

::: {.cell-output .cell-output-stderr}
```
Warning: There was 1 warning in `mutate()`.
ℹ In argument: `Regions = countrycode(entity, origin = "country.name",
  destination = "region")`.
Caused by warning in `countrycode_convert()`:
! Some values were not matched unambiguously: Asia, Central Asia, Central Europe, Central Europe, Eastern Europe, and Central Asia, Commonwealth, Commonwealth Middle Income, East Asia & Pacific - World Bank region, Eastern Mediterranean Region, Eastern sub-Saharan Africa, England, Europe & Central Asia - World Bank region, European Region, G20, High-income, High-income Asia Pacific, High SDI, Latin America & Caribbean - World Bank region, Low-middle SDI, Middle East & North Africa, Nordic Region, North Africa and Middle East, North America, Northern Ireland, OECD Countries, Region of the Americas, Scotland, Southeast Asia, Southern Latin America, Western Europe, Western Pacific Region, Western sub-Saharan Africa, World, World Bank Low Income, World Bank Lower Middle Income, World Bank Upper Middle Income
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
   <td style="text-align:right;"> 5.0564909 </td>
   <td style="text-align:right;"> 6.006491 </td>
   <td style="text-align:right;"> 19 </td>
  </tr>
  <tr>
   <td style="text-align:left;"> Europe &amp; Central Asia </td>
   <td style="text-align:right;"> 0.9668023 </td>
   <td style="text-align:right;"> 1.605355 </td>
   <td style="text-align:right;"> 28 </td>
  </tr>
  <tr>
   <td style="text-align:left;"> Latin America &amp; Caribbean </td>
   <td style="text-align:right;"> 1.8941899 </td>
   <td style="text-align:right;"> 2.987595 </td>
   <td style="text-align:right;"> 21 </td>
  </tr>
  <tr>
   <td style="text-align:left;"> Middle East &amp; North Africa </td>
   <td style="text-align:right;"> 0.4200464 </td>
   <td style="text-align:right;"> 1.088760 </td>
   <td style="text-align:right;"> 12 </td>
  </tr>
  <tr>
   <td style="text-align:left;"> North America </td>
   <td style="text-align:right;"> 0.0593336 </td>
   <td style="text-align:right;"> 0.076937 </td>
   <td style="text-align:right;"> 2 </td>
  </tr>
  <tr>
   <td style="text-align:left;"> South Asia </td>
   <td style="text-align:right;"> 7.3360184 </td>
   <td style="text-align:right;"> 1.832831 </td>
   <td style="text-align:right;"> 3 </td>
  </tr>
  <tr>
   <td style="text-align:left;"> Sub-Saharan Africa </td>
   <td style="text-align:right;"> 9.7122837 </td>
   <td style="text-align:right;"> 3.633842 </td>
   <td style="text-align:right;"> 20 </td>
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
   <td style="text-align:right;"> 35 </td>
  </tr>
  <tr>
   <td style="text-align:right;"> 1991 </td>
   <td style="text-align:right;"> 47 </td>
  </tr>
  <tr>
   <td style="text-align:right;"> 1992 </td>
   <td style="text-align:right;"> 34 </td>
  </tr>
  <tr>
   <td style="text-align:right;"> 1993 </td>
   <td style="text-align:right;"> 42 </td>
  </tr>
  <tr>
   <td style="text-align:right;"> 1994 </td>
   <td style="text-align:right;"> 40 </td>
  </tr>
  <tr>
   <td style="text-align:right;"> 1995 </td>
   <td style="text-align:right;"> 35 </td>
  </tr>
  <tr>
   <td style="text-align:right;"> 1996 </td>
   <td style="text-align:right;"> 35 </td>
  </tr>
  <tr>
   <td style="text-align:right;"> 1997 </td>
   <td style="text-align:right;"> 40 </td>
  </tr>
  <tr>
   <td style="text-align:right;"> 1998 </td>
   <td style="text-align:right;"> 42 </td>
  </tr>
  <tr>
   <td style="text-align:right;"> 1999 </td>
   <td style="text-align:right;"> 41 </td>
  </tr>
  <tr>
   <td style="text-align:right;"> 2000 </td>
   <td style="text-align:right;"> 30 </td>
  </tr>
  <tr>
   <td style="text-align:right;"> 2001 </td>
   <td style="text-align:right;"> 40 </td>
  </tr>
  <tr>
   <td style="text-align:right;"> 2002 </td>
   <td style="text-align:right;"> 31 </td>
  </tr>
  <tr>
   <td style="text-align:right;"> 2003 </td>
   <td style="text-align:right;"> 43 </td>
  </tr>
  <tr>
   <td style="text-align:right;"> 2004 </td>
   <td style="text-align:right;"> 38 </td>
  </tr>
  <tr>
   <td style="text-align:right;"> 2005 </td>
   <td style="text-align:right;"> 47 </td>
  </tr>
  <tr>
   <td style="text-align:right;"> 2006 </td>
   <td style="text-align:right;"> 46 </td>
  </tr>
  <tr>
   <td style="text-align:right;"> 2007 </td>
   <td style="text-align:right;"> 46 </td>
  </tr>
  <tr>
   <td style="text-align:right;"> 2008 </td>
   <td style="text-align:right;"> 43 </td>
  </tr>
  <tr>
   <td style="text-align:right;"> 2009 </td>
   <td style="text-align:right;"> 55 </td>
  </tr>
  <tr>
   <td style="text-align:right;"> 2010 </td>
   <td style="text-align:right;"> 55 </td>
  </tr>
  <tr>
   <td style="text-align:right;"> 2011 </td>
   <td style="text-align:right;"> 47 </td>
  </tr>
  <tr>
   <td style="text-align:right;"> 2012 </td>
   <td style="text-align:right;"> 34 </td>
  </tr>
  <tr>
   <td style="text-align:right;"> 2013 </td>
   <td style="text-align:right;"> 36 </td>
  </tr>
  <tr>
   <td style="text-align:right;"> 2014 </td>
   <td style="text-align:right;"> 33 </td>
  </tr>
  <tr>
   <td style="text-align:right;"> 2015 </td>
   <td style="text-align:right;"> 49 </td>
  </tr>
  <tr>
   <td style="text-align:right;"> 2016 </td>
   <td style="text-align:right;"> 42 </td>
  </tr>
  <tr>
   <td style="text-align:right;"> 2017 </td>
   <td style="text-align:right;"> 35 </td>
  </tr>
  <tr>
   <td style="text-align:right;"> 2018 </td>
   <td style="text-align:right;"> 30 </td>
  </tr>
  <tr>
   <td style="text-align:right;"> 2019 </td>
   <td style="text-align:right;"> 30 </td>
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

        -   areas where temperatures are very cold probably have more insulated homes burning bio fuels on top of cooking with combustion. Can a correlation be made suggesting that, despite economic wealth per capita, IAP caused by combustion is a bigger issue in Nothern countries? This would likely only apply to countries that have not reached "western civilization" levels of wealth

-   It seems that the quantity of variables spread across this time span eliminates any hypothesis that would not involve a temporal comparison. If focusing on a single-year comparison becomes useful, it may be valuable to pick a year that contains the most variables, since they are not all the same.
:::

**Potential Hypothesis:**


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

```{}
```

**Potential Hypothesis: Temperate vs. Tropical regions will impact the IAP of developing countries**

## Hypothesis: **Fuel combustion and Indoor Air Pollution**

Considering the data that is present, their are two layers of analysis that could generate useful hypothesis for understanding the global impact of indoor air pollution. Considering the country-specific data, it may be valuable to try and find an additional data set for each layer to use for an additional comparison. These second datasets will be considered in thy hypothesis for each layer.

## Layer One: Regions and Climates

**The correlation that would be most interesting to explore would be between the regional characteristics in terms of climate and the type of winters that are common. Temperate regions are more likely to invest in indoor heating which often involved the use of bio fuels like wood stoves. Poor ventilation is also likely to be a component that will play a role in temperate regions being associated with greater % deaths by IAP.**

-   

## Layer Two: Countries and Resources

**It is likely that this data will show a correlation between countries with low economic resources having significantly higher rates of premature death to exposures that have been removed from developed country households**

-   GDP is likely a strong contributing factor that does not even in tropical regions where ventilation is less of a concern, access to clean fuels is limited, indoor burning of fuels for cooking contributes to close proximity to the source of toxic compounds and heavy metals.

    -   despite the likely correlation between a nations GDP and its percent deaths caused by IAP, it also seems the data will show an overall decrease in the 30yrs in every part of the world, but most noticeable in regions with high average percents.

-   Central America, South America, North Africa, South Asia, and the Pacific Islands are the regions that may not be correlated to climate, but contain significant number of countries with low GDP that contribute to IAP deaths.
