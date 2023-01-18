---
title: "InvertebratesAnalysis"
author: "Trevor Harrington"
format: html
editor: visual
execute: 
  keep-md: TRUE
---



# Invertebrate Biodiversity in Riffle-Pool Streams: The Influence of Water Flow Characteristics. 


::: {.cell}

```{.r .cell-code}
#Load all valuable libraries to manipulate and display data
library(readxl)
library(tidyverse)
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
✔ broom        1.0.2     ✔ rsample      1.1.1
✔ dials        1.1.0     ✔ tune         1.0.1
✔ infer        1.0.4     ✔ workflows    1.1.2
✔ modeldata    1.0.1     ✔ workflowsets 1.0.0
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
#data is saved locally, not accessable off of this PC
invertebrates <- read_excel("Invertebrates in R/inverts_class_data.xlsx")
```
:::


## Initial data processing/ finding a hypothesis


::: {.cell}

```{.r .cell-code}
 invertebrates %>% 
   # Initial data intake / hypothesis generating exploration before further data investigation.
   
   head() %>%
 kable() %>%
  kable_styling(bootstrap_options = c("hover", "striped"))
```

::: {.cell-output-display}

`````{=html}
<table class="table table-hover table-striped" style="margin-left: auto; margin-right: auto;">
 <thead>
  <tr>
   <th style="text-align:left;"> Site </th>
   <th style="text-align:left;"> Riffle/Pool </th>
   <th style="text-align:right;"> Flow_Velocity </th>
   <th style="text-align:right;"> Stream_Width </th>
   <th style="text-align:right;"> Stream_Depth </th>
   <th style="text-align:right;"> Gastropoda </th>
   <th style="text-align:right;"> Bivalvia </th>
   <th style="text-align:right;"> Diptera </th>
   <th style="text-align:right;"> Turbellaria </th>
   <th style="text-align:right;"> Oliggocheata </th>
   <th style="text-align:right;"> Hirundinea </th>
   <th style="text-align:right;"> Decapoda </th>
   <th style="text-align:right;"> Amphipod </th>
   <th style="text-align:right;"> Isopod </th>
   <th style="text-align:right;"> Trombidiforme </th>
   <th style="text-align:right;"> Plecoptera </th>
   <th style="text-align:right;"> Trichoptera </th>
   <th style="text-align:right;"> Ephemroptera </th>
   <th style="text-align:right;"> Megaloptera </th>
   <th style="text-align:right;"> Coleoptera </th>
   <th style="text-align:right;"> Hemiptera </th>
   <th style="text-align:right;"> Odonta </th>
   <th style="text-align:right;"> Lepidoptera </th>
  </tr>
 </thead>
<tbody>
  <tr>
   <td style="text-align:left;"> 1 (upstream) </td>
   <td style="text-align:left;"> Riffle </td>
   <td style="text-align:right;"> 3.00 </td>
   <td style="text-align:right;"> 10 </td>
   <td style="text-align:right;"> 1.5 </td>
   <td style="text-align:right;"> 0 </td>
   <td style="text-align:right;"> 0 </td>
   <td style="text-align:right;"> 3 </td>
   <td style="text-align:right;"> 0 </td>
   <td style="text-align:right;"> 0 </td>
   <td style="text-align:right;"> 0 </td>
   <td style="text-align:right;"> 0 </td>
   <td style="text-align:right;"> 0 </td>
   <td style="text-align:right;"> 0 </td>
   <td style="text-align:right;"> 0 </td>
   <td style="text-align:right;"> 10 </td>
   <td style="text-align:right;"> 7 </td>
   <td style="text-align:right;"> 14 </td>
   <td style="text-align:right;"> 4 </td>
   <td style="text-align:right;"> 5 </td>
   <td style="text-align:right;"> 1 </td>
   <td style="text-align:right;"> 10 </td>
   <td style="text-align:right;"> 1 </td>
  </tr>
  <tr>
   <td style="text-align:left;"> 1(upstream) </td>
   <td style="text-align:left;"> Pool </td>
   <td style="text-align:right;"> 3.00 </td>
   <td style="text-align:right;"> 10 </td>
   <td style="text-align:right;"> 1.5 </td>
   <td style="text-align:right;"> 0 </td>
   <td style="text-align:right;"> 0 </td>
   <td style="text-align:right;"> 0 </td>
   <td style="text-align:right;"> 0 </td>
   <td style="text-align:right;"> 0 </td>
   <td style="text-align:right;"> 0 </td>
   <td style="text-align:right;"> 0 </td>
   <td style="text-align:right;"> 0 </td>
   <td style="text-align:right;"> 0 </td>
   <td style="text-align:right;"> 0 </td>
   <td style="text-align:right;"> 0 </td>
   <td style="text-align:right;"> 2 </td>
   <td style="text-align:right;"> 0 </td>
   <td style="text-align:right;"> 0 </td>
   <td style="text-align:right;"> 4 </td>
   <td style="text-align:right;"> 1 </td>
   <td style="text-align:right;"> 4 </td>
   <td style="text-align:right;"> 0 </td>
  </tr>
  <tr>
   <td style="text-align:left;"> 2 (upstream) </td>
   <td style="text-align:left;"> Riffle </td>
   <td style="text-align:right;"> 0.18 </td>
   <td style="text-align:right;"> 10 </td>
   <td style="text-align:right;"> 1.9 </td>
   <td style="text-align:right;"> 0 </td>
   <td style="text-align:right;"> 0 </td>
   <td style="text-align:right;"> 2 </td>
   <td style="text-align:right;"> 0 </td>
   <td style="text-align:right;"> 0 </td>
   <td style="text-align:right;"> 0 </td>
   <td style="text-align:right;"> 0 </td>
   <td style="text-align:right;"> 0 </td>
   <td style="text-align:right;"> 0 </td>
   <td style="text-align:right;"> 0 </td>
   <td style="text-align:right;"> 1 </td>
   <td style="text-align:right;"> 20 </td>
   <td style="text-align:right;"> 6 </td>
   <td style="text-align:right;"> 0 </td>
   <td style="text-align:right;"> 9 </td>
   <td style="text-align:right;"> 0 </td>
   <td style="text-align:right;"> 24 </td>
   <td style="text-align:right;"> 0 </td>
  </tr>
  <tr>
   <td style="text-align:left;"> 2 (upstream) </td>
   <td style="text-align:left;"> Pool </td>
   <td style="text-align:right;"> 0.18 </td>
   <td style="text-align:right;"> 10 </td>
   <td style="text-align:right;"> 1.9 </td>
   <td style="text-align:right;"> 0 </td>
   <td style="text-align:right;"> 0 </td>
   <td style="text-align:right;"> 0 </td>
   <td style="text-align:right;"> 0 </td>
   <td style="text-align:right;"> 0 </td>
   <td style="text-align:right;"> 0 </td>
   <td style="text-align:right;"> 0 </td>
   <td style="text-align:right;"> 0 </td>
   <td style="text-align:right;"> 0 </td>
   <td style="text-align:right;"> 0 </td>
   <td style="text-align:right;"> 0 </td>
   <td style="text-align:right;"> 6 </td>
   <td style="text-align:right;"> 2 </td>
   <td style="text-align:right;"> 0 </td>
   <td style="text-align:right;"> 0 </td>
   <td style="text-align:right;"> 0 </td>
   <td style="text-align:right;"> 7 </td>
   <td style="text-align:right;"> 0 </td>
  </tr>
</tbody>
</table>

`````

:::
:::


### Potential Questions for Investigation

Investigation of raw data suggests some potential questions could be related to a preference of species towards one type of stream versus another, with potentially some variation of these four testing sites providing an environment that is suitably favorable for a range of species.

This data has some significant limitations that prevent a in-depth investigation of the cause for success over one species versus another. Namely, this data lacks temporal, geographical, and methodology data that could provide insight for what other variables may contribute to the data collected. This data also lacks any measurement units for flow rate and width/depth, meaning they do not provide sufficient data to make any assumptions on the actual size, depth, or total flow of the stream to contribute in analysis. Overall, this data will be most valuable for


::: {.cell}

```{.r .cell-code}
invertebrates %>% #simple exploratory analysis 
  
  mutate(total_individuals_present = Gastropoda + Bivalvia + Diptera + Turbellaria + Oliggocheata + Hirundinea + Decapoda + Amphipod + Isopod + Trombidiforme + Plecoptera + Trichoptera + Ephemroptera + Megaloptera + Coleoptera + Hemiptera + Odonta + Lepidoptera) %>%

mutate(Flow_Volume_m_sqrd = Stream_Width * Stream_Depth) %>%
  
  group_by(`Riffle/Pool`) %>%
  
summarise(
  "average species present" = mean(total_individuals_present),
  "average volume of flow (m2)" = mean(Flow_Volume_m_sqrd)
) %>%

kable() %>%
  kable_styling(bootstrap_options = c("hover", "striped"))
```

::: {.cell-output-display}

`````{=html}
<table class="table table-hover table-striped" style="margin-left: auto; margin-right: auto;">
 <thead>
  <tr>
   <th style="text-align:left;"> Riffle/Pool </th>
   <th style="text-align:right;"> average species present </th>
   <th style="text-align:right;"> average volume of flow (m2) </th>
  </tr>
 </thead>
<tbody>
  <tr>
   <td style="text-align:left;"> Pool </td>
   <td style="text-align:right;"> 13.0 </td>
   <td style="text-align:right;"> 17 </td>
  </tr>
  <tr>
   <td style="text-align:left;"> Riffle </td>
   <td style="text-align:right;"> 58.5 </td>
   <td style="text-align:right;"> 17 </td>
  </tr>
</tbody>
</table>

`````

:::
:::


Doing a simple


::: {.cell}

:::


**Potential Hypotheses**
