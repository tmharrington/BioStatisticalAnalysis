---
title: "InvertebratesAnalysis"
author: "Trevor Harrington"
format: html
editor: visual
execute: 
  keep-md: TRUE
---



# Invertebrates


::: {.cell}

```{.r .cell-code}
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

::: {.cell}

```{.r .cell-code}
invertebrates %>%
  
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


Clearly there is a difference in


::: {.cell}

:::


**Potential Hypotheses**
