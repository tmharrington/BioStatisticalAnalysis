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
• Use tidymodels_prefer() to resolve common conflicts.
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

#data is saved locally, not accessable off of this PC
invertebrates <- read_excel("Invertebrates in R/inverts_class_data.xlsx") %>%
  
clean_names() 
```
:::


## Initial Data Split


::: {.cell}

```{.r .cell-code}
   # Initial data intake / hypothesis generating exploration before further data investigation.
   
   my_data_splits <- initial_split(invertebrates, prop = 0.5,
                                   pool = 1) 

exploratory_data <- training(my_data_splits)
test_data <- testing(my_data_splits)

exploratory_data %>%
  
   t %>% as.data.frame( row.names = NULL, optional = FALSE,
              cut.names = FALSE, col.names = names('S1', 'S2'), fix.empty.names = TRUE,
              check.names = !optional,
              stringsAsFactors = FALSE) %>%

kable() %>%
  kable_styling(bootstrap_options = c("hover", "striped"))
```

::: {.cell-output-display}

`````{=html}
<table class="table table-hover table-striped" style="margin-left: auto; margin-right: auto;">
 <thead>
  <tr>
   <th style="text-align:left;">   </th>
   <th style="text-align:left;"> V1 </th>
   <th style="text-align:left;"> V2 </th>
  </tr>
 </thead>
<tbody>
  <tr>
   <td style="text-align:left;"> site </td>
   <td style="text-align:left;"> 2 (upstream) </td>
   <td style="text-align:left;"> 1 (upstream) </td>
  </tr>
  <tr>
   <td style="text-align:left;"> riffle_pool </td>
   <td style="text-align:left;"> Pool </td>
   <td style="text-align:left;"> Pool </td>
  </tr>
  <tr>
   <td style="text-align:left;"> flow_velocity </td>
   <td style="text-align:left;"> 0.18 </td>
   <td style="text-align:left;"> 3.00 </td>
  </tr>
  <tr>
   <td style="text-align:left;"> stream_width </td>
   <td style="text-align:left;"> 10 </td>
   <td style="text-align:left;"> 10 </td>
  </tr>
  <tr>
   <td style="text-align:left;"> stream_depth </td>
   <td style="text-align:left;"> 1.9 </td>
   <td style="text-align:left;"> 1.5 </td>
  </tr>
  <tr>
   <td style="text-align:left;"> gastropoda </td>
   <td style="text-align:left;"> 0 </td>
   <td style="text-align:left;"> 0 </td>
  </tr>
  <tr>
   <td style="text-align:left;"> bivalvia </td>
   <td style="text-align:left;"> 0 </td>
   <td style="text-align:left;"> 0 </td>
  </tr>
  <tr>
   <td style="text-align:left;"> diptera </td>
   <td style="text-align:left;"> 0 </td>
   <td style="text-align:left;"> 0 </td>
  </tr>
  <tr>
   <td style="text-align:left;"> turbellaria </td>
   <td style="text-align:left;"> 0 </td>
   <td style="text-align:left;"> 0 </td>
  </tr>
  <tr>
   <td style="text-align:left;"> oliggocheata </td>
   <td style="text-align:left;"> 0 </td>
   <td style="text-align:left;"> 0 </td>
  </tr>
  <tr>
   <td style="text-align:left;"> hirundinea </td>
   <td style="text-align:left;"> 0 </td>
   <td style="text-align:left;"> 0 </td>
  </tr>
  <tr>
   <td style="text-align:left;"> decapoda </td>
   <td style="text-align:left;"> 0 </td>
   <td style="text-align:left;"> 0 </td>
  </tr>
  <tr>
   <td style="text-align:left;"> amphipod </td>
   <td style="text-align:left;"> 0 </td>
   <td style="text-align:left;"> 0 </td>
  </tr>
  <tr>
   <td style="text-align:left;"> isopod </td>
   <td style="text-align:left;"> 0 </td>
   <td style="text-align:left;"> 0 </td>
  </tr>
  <tr>
   <td style="text-align:left;"> trombidiforme </td>
   <td style="text-align:left;"> 0 </td>
   <td style="text-align:left;"> 0 </td>
  </tr>
  <tr>
   <td style="text-align:left;"> plecoptera </td>
   <td style="text-align:left;"> 0 </td>
   <td style="text-align:left;"> 0 </td>
  </tr>
  <tr>
   <td style="text-align:left;"> trichoptera </td>
   <td style="text-align:left;"> 6 </td>
   <td style="text-align:left;"> 2 </td>
  </tr>
  <tr>
   <td style="text-align:left;"> ephemroptera </td>
   <td style="text-align:left;"> 2 </td>
   <td style="text-align:left;"> 0 </td>
  </tr>
  <tr>
   <td style="text-align:left;"> megaloptera </td>
   <td style="text-align:left;"> 0 </td>
   <td style="text-align:left;"> 0 </td>
  </tr>
  <tr>
   <td style="text-align:left;"> coleoptera </td>
   <td style="text-align:left;"> 0 </td>
   <td style="text-align:left;"> 4 </td>
  </tr>
  <tr>
   <td style="text-align:left;"> hemiptera </td>
   <td style="text-align:left;"> 0 </td>
   <td style="text-align:left;"> 1 </td>
  </tr>
  <tr>
   <td style="text-align:left;"> odonta </td>
   <td style="text-align:left;"> 7 </td>
   <td style="text-align:left;"> 4 </td>
  </tr>
  <tr>
   <td style="text-align:left;"> lepidoptera </td>
   <td style="text-align:left;"> 0 </td>
   <td style="text-align:left;"> 0 </td>
  </tr>
</tbody>
</table>

`````

:::
:::


::: callout-note
**Observations:**

-   Several rows of data are 0 for both sample sites, and could be filtered out for lack of relevance to any potential analysis.

    -   Flow velocity is the most significant difference that can be attributed to this data set for comparing water characteristics to the presence of species.

-   All the species seem to favor one steam against the other

    -   the closest to having a even comparison is **coleoptera** (9:4) having less preference for riffle run vs. pool

    -   
:::


::: {.cell}

```{.r .cell-code}
exploratory_data %>%
  
  group_by(site) %>%

  mutate('total_individuals_present' = bivalvia + gastropoda + diptera + turbellaria + oliggocheata + hirundinea + decapoda + amphipod + isopod + trombidiforme + plecoptera + trichoptera + ephemroptera + megaloptera + coleoptera + hemiptera + odonta + lepidoptera) %>%
  
summarise(
  "average invertebrates present" = mean(total_individuals_present),
  "'Unit' flow" = mean(flow_velocity),
  "Riffle/pool" = list(riffle_pool),
  )%>%

kable() %>%
  kable_styling(bootstrap_options = c("hover", "striped"))
```

::: {.cell-output-display}

`````{=html}
<table class="table table-hover table-striped" style="margin-left: auto; margin-right: auto;">
 <thead>
  <tr>
   <th style="text-align:left;"> site </th>
   <th style="text-align:right;"> average invertebrates present </th>
   <th style="text-align:right;"> 'Unit' flow </th>
   <th style="text-align:left;"> Riffle/pool </th>
  </tr>
 </thead>
<tbody>
  <tr>
   <td style="text-align:left;"> 1 (upstream) </td>
   <td style="text-align:right;"> 11 </td>
   <td style="text-align:right;"> 3.00 </td>
   <td style="text-align:left;"> Pool </td>
  </tr>
  <tr>
   <td style="text-align:left;"> 2 (upstream) </td>
   <td style="text-align:right;"> 15 </td>
   <td style="text-align:right;"> 0.18 </td>
   <td style="text-align:left;"> Pool </td>
  </tr>
</tbody>
</table>

`````

:::
:::


::: callout-note
**Observations:**

This data shows a comparison between two sample sites. Using the data generated in this instance, the comparison is between a Pool in site 1 and a Riffle run in site 2.

-   Site 1 contained 11 vertebrates and was measured at a flow rate of 3.00 'units of flow'

-   Site 2 contained 62 (6x more invertebrates) then site 1, at a 0.18 'unit of flow' (16x slower flow rate)
:::

### What can be inferred? Data-split exploration

-   Using exploratory data, the first things to notice are that there are multiple locations where data has been collected.

-   data for this set are the species of invertebrates that were identified in two streams, across four locations. Each stream was measured at one 'riffle run' and one 'pool' section

    -   **A riffle stream** is a shallow section moving water characterized by rapid flow with a rocky or gravel bottom, where the water flows over small obstructions and creates a 'riffling' sound. Riffle sections of a stream are typically found in the most upstream part of a stream where the slope is steeper and the water flow is more energetic.

    -   A **pool stream** is a deeper section of a stream characterized by a slower flow rate with a smooth bottom, where the water flows around larger obstructions and creates a pooling effect. Pools are typically found in the lower half of a stream where the gradient is less steep and the water flow reduced.

Considering the characteristics of riffle and pool streams, it is possible some hypotheses could be generate on which would be more conducive of some species more then others.

-   It is reasonable to believe that among the 18 invertebrate species measured in this data set, some would have less preference to the water characteristics then others. However, it is still not without reason that one type of stream can be suitable for a larger proportion over another.

# 

## Data analysis using initial exploratory hypotheses


::: {.cell}

```{.r .cell-code}
invertebrates %>%
  
  # reorient the data where variables are listed under the site columns.
  
  t %>% as.data.frame(colnames('Stream',
                               prefix = col)) %>%

 # filter out columns where every variable is 0
  
 filter_all(any_vars(.!=0)) %>%

  kable() %>%
    kable_styling(bootstrap_options = c("hover", "striped"))
```

::: {.cell-output-display}

`````{=html}
<table class="table table-hover table-striped" style="margin-left: auto; margin-right: auto;">
 <thead>
  <tr>
   <th style="text-align:left;">   </th>
   <th style="text-align:left;"> V1 </th>
   <th style="text-align:left;"> V2 </th>
   <th style="text-align:left;"> V3 </th>
   <th style="text-align:left;"> V4 </th>
  </tr>
 </thead>
<tbody>
  <tr>
   <td style="text-align:left;"> site </td>
   <td style="text-align:left;"> 1 (upstream) </td>
   <td style="text-align:left;"> 1 (upstream) </td>
   <td style="text-align:left;"> 2 (upstream) </td>
   <td style="text-align:left;"> 2 (upstream) </td>
  </tr>
  <tr>
   <td style="text-align:left;"> riffle_pool </td>
   <td style="text-align:left;"> Riffle </td>
   <td style="text-align:left;"> Pool </td>
   <td style="text-align:left;"> Riffle </td>
   <td style="text-align:left;"> Pool </td>
  </tr>
  <tr>
   <td style="text-align:left;"> flow_velocity </td>
   <td style="text-align:left;"> 3.00 </td>
   <td style="text-align:left;"> 3.00 </td>
   <td style="text-align:left;"> 0.18 </td>
   <td style="text-align:left;"> 0.18 </td>
  </tr>
  <tr>
   <td style="text-align:left;"> stream_width </td>
   <td style="text-align:left;"> 10 </td>
   <td style="text-align:left;"> 10 </td>
   <td style="text-align:left;"> 10 </td>
   <td style="text-align:left;"> 10 </td>
  </tr>
  <tr>
   <td style="text-align:left;"> stream_depth </td>
   <td style="text-align:left;"> 1.5 </td>
   <td style="text-align:left;"> 1.5 </td>
   <td style="text-align:left;"> 1.9 </td>
   <td style="text-align:left;"> 1.9 </td>
  </tr>
  <tr>
   <td style="text-align:left;"> diptera </td>
   <td style="text-align:left;"> 3 </td>
   <td style="text-align:left;"> 0 </td>
   <td style="text-align:left;"> 2 </td>
   <td style="text-align:left;"> 0 </td>
  </tr>
  <tr>
   <td style="text-align:left;"> plecoptera </td>
   <td style="text-align:left;"> 10 </td>
   <td style="text-align:left;"> 0 </td>
   <td style="text-align:left;"> 1 </td>
   <td style="text-align:left;"> 0 </td>
  </tr>
  <tr>
   <td style="text-align:left;"> trichoptera </td>
   <td style="text-align:left;"> 7 </td>
   <td style="text-align:left;"> 2 </td>
   <td style="text-align:left;"> 20 </td>
   <td style="text-align:left;"> 6 </td>
  </tr>
  <tr>
   <td style="text-align:left;"> ephemroptera </td>
   <td style="text-align:left;"> 14 </td>
   <td style="text-align:left;"> 0 </td>
   <td style="text-align:left;"> 6 </td>
   <td style="text-align:left;"> 2 </td>
  </tr>
  <tr>
   <td style="text-align:left;"> megaloptera </td>
   <td style="text-align:left;"> 4 </td>
   <td style="text-align:left;"> 0 </td>
   <td style="text-align:left;"> 0 </td>
   <td style="text-align:left;"> 0 </td>
  </tr>
  <tr>
   <td style="text-align:left;"> coleoptera </td>
   <td style="text-align:left;"> 5 </td>
   <td style="text-align:left;"> 4 </td>
   <td style="text-align:left;"> 9 </td>
   <td style="text-align:left;"> 0 </td>
  </tr>
  <tr>
   <td style="text-align:left;"> hemiptera </td>
   <td style="text-align:left;"> 1 </td>
   <td style="text-align:left;"> 1 </td>
   <td style="text-align:left;"> 0 </td>
   <td style="text-align:left;"> 0 </td>
  </tr>
  <tr>
   <td style="text-align:left;"> odonta </td>
   <td style="text-align:left;"> 10 </td>
   <td style="text-align:left;"> 4 </td>
   <td style="text-align:left;"> 24 </td>
   <td style="text-align:left;"> 7 </td>
  </tr>
  <tr>
   <td style="text-align:left;"> lepidoptera </td>
   <td style="text-align:left;"> 1 </td>
   <td style="text-align:left;"> 0 </td>
   <td style="text-align:left;"> 0 </td>
   <td style="text-align:left;"> 0 </td>
  </tr>
</tbody>
</table>

`````

:::
:::


::: callout-note
**Observations:**

-   The riffle environment seems to be more conducive to invertebrate life judging by the larger population found in riffle sites. Does this correspond to biodiversity or is one species largely successful?
-   **Trichoptera** (caddisfly), **Odonta (**dragonfly), **ephemroptera** (mayfly) make up [50.4%]{.underline} of the upstream total species identified
:::

### Reconsidering Hypotheses with Full Dataset

Investigation of raw data suggests some potential questions could be related to a preference of species towards one type of stream versus another, with potentially some variation of these four testing sites providing an environment that is suitably favorable for a range of species.

This data has some significant limitations that prevent a in-depth investigation of the cause for success over one species versus another. Namely, this data lacks temporal, geographical, and methodology data that could provide insight for what other variables may contribute to the data collected. This data also lacks any measurement units for flow rate and width/depth, meaning they do not provide sufficient data to make any assumptions on the actual size, depth, or total flow of the stream to contribute in analysis. Overall, this data will be most valuable for

### **What can be inferred about from the addition data?**

-   How are the characteristics between Site 1 & 2 different? Which combination of features seems to be the most ideal for invertebrate success.

    -   Is large species count the same as biodiversity? Does the site with largest amount of species also have the most unique species, or is favorable for only a few?

-   Something we are becoming increasingly aware of is the impact of dams on aquatic habitats. If this stream is or is not dammed, there could be some inference or comparison made with other researchers findings and how the presents of man-made obstacles could impact invertebrate habitats.

    -   likely, this kind of inference would require a larger data set to work with, but it could potentially provide some useful insight on making comparisons with other analysis conducted on a similar topic.
    -   The invertebrate species found in the pool sites of one stream should be more similar to those in the pool sites of the other stream compared to the riffle sites of the two streams.

-   The invertebrate species found in the pool sites of one stream should be more similar to those in the pool sites of the other stream compared to the riffle sites of the two streams.


    ::: {.cell}
    
    ```{.r .cell-code}
    invertebrates %>%
      
       pivot_longer(invertebrates,
                   cols = 6:23,
                   names_to = "order",
                   values_to = "count") %>%
      
      group_by(riffle_pool,site) %>%
      
      filter(count != 0) %>%
    
      kable() %>%
        kable_styling(bootstrap_options = c("hover", "striped"))
    ```
    
    ::: {.cell-output .cell-output-stderr}
    ```
    Warning in gsub(vec_paste0("^", names_prefix), "", cols): argument 'pattern' has
    length > 1 and only the first element will be used
    ```
    :::
    
    ::: {.cell-output-display}

    `````{=html}
    <table class="table table-hover table-striped" style="margin-left: auto; margin-right: auto;">
     <thead>
      <tr>
       <th style="text-align:left;"> site </th>
       <th style="text-align:left;"> riffle_pool </th>
       <th style="text-align:right;"> flow_velocity </th>
       <th style="text-align:right;"> stream_width </th>
       <th style="text-align:right;"> stream_depth </th>
       <th style="text-align:left;"> order </th>
       <th style="text-align:right;"> count </th>
      </tr>
     </thead>
    <tbody>
      <tr>
       <td style="text-align:left;"> 1 (upstream) </td>
       <td style="text-align:left;"> Riffle </td>
       <td style="text-align:right;"> 3.00 </td>
       <td style="text-align:right;"> 10 </td>
       <td style="text-align:right;"> 1.5 </td>
       <td style="text-align:left;"> diptera </td>
       <td style="text-align:right;"> 3 </td>
      </tr>
      <tr>
       <td style="text-align:left;"> 1 (upstream) </td>
       <td style="text-align:left;"> Riffle </td>
       <td style="text-align:right;"> 3.00 </td>
       <td style="text-align:right;"> 10 </td>
       <td style="text-align:right;"> 1.5 </td>
       <td style="text-align:left;"> plecoptera </td>
       <td style="text-align:right;"> 10 </td>
      </tr>
      <tr>
       <td style="text-align:left;"> 1 (upstream) </td>
       <td style="text-align:left;"> Riffle </td>
       <td style="text-align:right;"> 3.00 </td>
       <td style="text-align:right;"> 10 </td>
       <td style="text-align:right;"> 1.5 </td>
       <td style="text-align:left;"> trichoptera </td>
       <td style="text-align:right;"> 7 </td>
      </tr>
      <tr>
       <td style="text-align:left;"> 1 (upstream) </td>
       <td style="text-align:left;"> Riffle </td>
       <td style="text-align:right;"> 3.00 </td>
       <td style="text-align:right;"> 10 </td>
       <td style="text-align:right;"> 1.5 </td>
       <td style="text-align:left;"> ephemroptera </td>
       <td style="text-align:right;"> 14 </td>
      </tr>
      <tr>
       <td style="text-align:left;"> 1 (upstream) </td>
       <td style="text-align:left;"> Riffle </td>
       <td style="text-align:right;"> 3.00 </td>
       <td style="text-align:right;"> 10 </td>
       <td style="text-align:right;"> 1.5 </td>
       <td style="text-align:left;"> megaloptera </td>
       <td style="text-align:right;"> 4 </td>
      </tr>
      <tr>
       <td style="text-align:left;"> 1 (upstream) </td>
       <td style="text-align:left;"> Riffle </td>
       <td style="text-align:right;"> 3.00 </td>
       <td style="text-align:right;"> 10 </td>
       <td style="text-align:right;"> 1.5 </td>
       <td style="text-align:left;"> coleoptera </td>
       <td style="text-align:right;"> 5 </td>
      </tr>
      <tr>
       <td style="text-align:left;"> 1 (upstream) </td>
       <td style="text-align:left;"> Riffle </td>
       <td style="text-align:right;"> 3.00 </td>
       <td style="text-align:right;"> 10 </td>
       <td style="text-align:right;"> 1.5 </td>
       <td style="text-align:left;"> hemiptera </td>
       <td style="text-align:right;"> 1 </td>
      </tr>
      <tr>
       <td style="text-align:left;"> 1 (upstream) </td>
       <td style="text-align:left;"> Riffle </td>
       <td style="text-align:right;"> 3.00 </td>
       <td style="text-align:right;"> 10 </td>
       <td style="text-align:right;"> 1.5 </td>
       <td style="text-align:left;"> odonta </td>
       <td style="text-align:right;"> 10 </td>
      </tr>
      <tr>
       <td style="text-align:left;"> 1 (upstream) </td>
       <td style="text-align:left;"> Riffle </td>
       <td style="text-align:right;"> 3.00 </td>
       <td style="text-align:right;"> 10 </td>
       <td style="text-align:right;"> 1.5 </td>
       <td style="text-align:left;"> lepidoptera </td>
       <td style="text-align:right;"> 1 </td>
      </tr>
      <tr>
       <td style="text-align:left;"> 1 (upstream) </td>
       <td style="text-align:left;"> Pool </td>
       <td style="text-align:right;"> 3.00 </td>
       <td style="text-align:right;"> 10 </td>
       <td style="text-align:right;"> 1.5 </td>
       <td style="text-align:left;"> trichoptera </td>
       <td style="text-align:right;"> 2 </td>
      </tr>
      <tr>
       <td style="text-align:left;"> 1 (upstream) </td>
       <td style="text-align:left;"> Pool </td>
       <td style="text-align:right;"> 3.00 </td>
       <td style="text-align:right;"> 10 </td>
       <td style="text-align:right;"> 1.5 </td>
       <td style="text-align:left;"> coleoptera </td>
       <td style="text-align:right;"> 4 </td>
      </tr>
      <tr>
       <td style="text-align:left;"> 1 (upstream) </td>
       <td style="text-align:left;"> Pool </td>
       <td style="text-align:right;"> 3.00 </td>
       <td style="text-align:right;"> 10 </td>
       <td style="text-align:right;"> 1.5 </td>
       <td style="text-align:left;"> hemiptera </td>
       <td style="text-align:right;"> 1 </td>
      </tr>
      <tr>
       <td style="text-align:left;"> 1 (upstream) </td>
       <td style="text-align:left;"> Pool </td>
       <td style="text-align:right;"> 3.00 </td>
       <td style="text-align:right;"> 10 </td>
       <td style="text-align:right;"> 1.5 </td>
       <td style="text-align:left;"> odonta </td>
       <td style="text-align:right;"> 4 </td>
      </tr>
      <tr>
       <td style="text-align:left;"> 2 (upstream) </td>
       <td style="text-align:left;"> Riffle </td>
       <td style="text-align:right;"> 0.18 </td>
       <td style="text-align:right;"> 10 </td>
       <td style="text-align:right;"> 1.9 </td>
       <td style="text-align:left;"> diptera </td>
       <td style="text-align:right;"> 2 </td>
      </tr>
      <tr>
       <td style="text-align:left;"> 2 (upstream) </td>
       <td style="text-align:left;"> Riffle </td>
       <td style="text-align:right;"> 0.18 </td>
       <td style="text-align:right;"> 10 </td>
       <td style="text-align:right;"> 1.9 </td>
       <td style="text-align:left;"> plecoptera </td>
       <td style="text-align:right;"> 1 </td>
      </tr>
      <tr>
       <td style="text-align:left;"> 2 (upstream) </td>
       <td style="text-align:left;"> Riffle </td>
       <td style="text-align:right;"> 0.18 </td>
       <td style="text-align:right;"> 10 </td>
       <td style="text-align:right;"> 1.9 </td>
       <td style="text-align:left;"> trichoptera </td>
       <td style="text-align:right;"> 20 </td>
      </tr>
      <tr>
       <td style="text-align:left;"> 2 (upstream) </td>
       <td style="text-align:left;"> Riffle </td>
       <td style="text-align:right;"> 0.18 </td>
       <td style="text-align:right;"> 10 </td>
       <td style="text-align:right;"> 1.9 </td>
       <td style="text-align:left;"> ephemroptera </td>
       <td style="text-align:right;"> 6 </td>
      </tr>
      <tr>
       <td style="text-align:left;"> 2 (upstream) </td>
       <td style="text-align:left;"> Riffle </td>
       <td style="text-align:right;"> 0.18 </td>
       <td style="text-align:right;"> 10 </td>
       <td style="text-align:right;"> 1.9 </td>
       <td style="text-align:left;"> coleoptera </td>
       <td style="text-align:right;"> 9 </td>
      </tr>
      <tr>
       <td style="text-align:left;"> 2 (upstream) </td>
       <td style="text-align:left;"> Riffle </td>
       <td style="text-align:right;"> 0.18 </td>
       <td style="text-align:right;"> 10 </td>
       <td style="text-align:right;"> 1.9 </td>
       <td style="text-align:left;"> odonta </td>
       <td style="text-align:right;"> 24 </td>
      </tr>
      <tr>
       <td style="text-align:left;"> 2 (upstream) </td>
       <td style="text-align:left;"> Pool </td>
       <td style="text-align:right;"> 0.18 </td>
       <td style="text-align:right;"> 10 </td>
       <td style="text-align:right;"> 1.9 </td>
       <td style="text-align:left;"> trichoptera </td>
       <td style="text-align:right;"> 6 </td>
      </tr>
      <tr>
       <td style="text-align:left;"> 2 (upstream) </td>
       <td style="text-align:left;"> Pool </td>
       <td style="text-align:right;"> 0.18 </td>
       <td style="text-align:right;"> 10 </td>
       <td style="text-align:right;"> 1.9 </td>
       <td style="text-align:left;"> ephemroptera </td>
       <td style="text-align:right;"> 2 </td>
      </tr>
      <tr>
       <td style="text-align:left;"> 2 (upstream) </td>
       <td style="text-align:left;"> Pool </td>
       <td style="text-align:right;"> 0.18 </td>
       <td style="text-align:right;"> 10 </td>
       <td style="text-align:right;"> 1.9 </td>
       <td style="text-align:left;"> odonta </td>
       <td style="text-align:right;"> 7 </td>
      </tr>
    </tbody>
    </table>
    
    `````

    :::
    :::

::: {.cell}

```{.r .cell-code}
invertebrates %>% #simple exploratory analysis 
  
  mutate(total_individuals_present = bivalvia + gastropoda + diptera + turbellaria + oliggocheata + hirundinea + decapoda + amphipod + isopod + trombidiforme + plecoptera + trichoptera + ephemroptera + megaloptera + coleoptera + hemiptera + odonta + lepidoptera) %>%
  
  group_by(site, riffle_pool) %>%
  
summarise(
  "average species present" = mean(total_individuals_present),
  "'Unit' flow" = list(flow_velocity),
  "Riffle/pool" = list(riffle_pool),
  ) %>%

kable() %>%
  kable_styling(bootstrap_options = c("hover", "striped"))
```

::: {.cell-output .cell-output-stderr}
```
`summarise()` has grouped output by 'site'. You can override using the
`.groups` argument.
```
:::

::: {.cell-output-display}

`````{=html}
<table class="table table-hover table-striped" style="margin-left: auto; margin-right: auto;">
 <thead>
  <tr>
   <th style="text-align:left;"> site </th>
   <th style="text-align:left;"> riffle_pool </th>
   <th style="text-align:right;"> average species present </th>
   <th style="text-align:left;"> 'Unit' flow </th>
   <th style="text-align:left;"> Riffle/pool </th>
  </tr>
 </thead>
<tbody>
  <tr>
   <td style="text-align:left;"> 1 (upstream) </td>
   <td style="text-align:left;"> Pool </td>
   <td style="text-align:right;"> 11 </td>
   <td style="text-align:left;"> 3 </td>
   <td style="text-align:left;"> Pool </td>
  </tr>
  <tr>
   <td style="text-align:left;"> 1 (upstream) </td>
   <td style="text-align:left;"> Riffle </td>
   <td style="text-align:right;"> 55 </td>
   <td style="text-align:left;"> 3 </td>
   <td style="text-align:left;"> Riffle </td>
  </tr>
  <tr>
   <td style="text-align:left;"> 2 (upstream) </td>
   <td style="text-align:left;"> Pool </td>
   <td style="text-align:right;"> 15 </td>
   <td style="text-align:left;"> 0.18 </td>
   <td style="text-align:left;"> Pool </td>
  </tr>
  <tr>
   <td style="text-align:left;"> 2 (upstream) </td>
   <td style="text-align:left;"> Riffle </td>
   <td style="text-align:right;"> 62 </td>
   <td style="text-align:left;"> 0.18 </td>
   <td style="text-align:left;"> Riffle </td>
  </tr>
</tbody>
</table>

`````

:::
:::


::: callout-note
**Observations:**
:::

#### ***Hypothesis:*** By analyzing these four locations, the data may be able to determine whether the faster-flowing riffle run stream is more conducive to a greater number of of unique invertebrate species, or just contains a large quantity of a few well adapted species.

-   Something I am thinking about that lead to this hypothesis is how invasive species impact ecosystems -- while the total quantity of life may be greater, the impact can still be negative if the species present are reducing biodiversity in the environment.

***Null Hypothesis:*** insufficient statistical evidence is available to suggest this data shows a difference between riffle and pool streams.

## Data analysis -- Answering the Hypothesis

How can we test the claims?

#### Viewing Distribution of Species Across Sites

**Hypotheses:**

**NULL Hypothesis:** Characteristics
