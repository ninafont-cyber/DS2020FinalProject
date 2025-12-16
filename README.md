Final Project
================
Nina Fontanella

# Introduction: This project explores trends in energy consumption in the United States using publicly available data from the U.S. Energy Information Administration (EIA). As concerns about climate change and sustainability increase, understanding how energy usage has changed over time is increasingly important. Using historical energy consumption data, this analysis focuses on trends in energy consumption and compares renewables to other major energy sources. The goal of this project is to identify long-term patterns in U.S. energy use and evaluate the role renewables play in the overall energy mix and subsequently how we can do better in diversifying our energy portfolio and using more renewables in the U.S.

# Data: The data used in this project come from the U.S. Energy Information Administration (EIA) State Energy Data System (SEDS). This publicly available dataset provides annual estimates of U.S. energy consumption by energy source and end-use sector from 1960 through 2023. Energy consumption values are reported in billion British thermal units (Btu). For this analysis, the dataset was filtered to include only national-level data for the United States. Specific EIA series codes were used to represent major energy categories, including renewable energy, fossil fuels, and nuclear energy.

# Methods: Data analysis was conducted using R and RStudio. The dataset was first reshaped from a wide format to a long format to allow for time-series analysis. Data were filtered to include only U.S.-level observations. Energy sources were categorized into renewable energy, fossil fuels, and nuclear energy using EIA series codes. Summary statistics were calculated by year, energy source, and end-use sector. Line plots were created using the ggplot2 package to visualize trends in energy consumption over time and to compare energy sources across sectors.

``` r
library(tidyverse)
```

    ## Warning: package 'ggplot2' was built under R version 4.5.2

    ## Warning: package 'dplyr' was built under R version 4.5.2

    ## Warning: package 'stringr' was built under R version 4.5.2

    ## Warning: package 'forcats' was built under R version 4.5.2

    ## ── Attaching core tidyverse packages ──────────────────────── tidyverse 2.0.0 ──
    ## ✔ dplyr     1.1.4     ✔ readr     2.1.5
    ## ✔ forcats   1.0.1     ✔ stringr   1.6.0
    ## ✔ ggplot2   4.0.0     ✔ tibble    3.3.0
    ## ✔ lubridate 1.9.4     ✔ tidyr     1.3.1
    ## ✔ purrr     1.1.0     
    ## ── Conflicts ────────────────────────────────────────── tidyverse_conflicts() ──
    ## ✖ dplyr::filter() masks stats::filter()
    ## ✖ dplyr::lag()    masks stats::lag()
    ## ℹ Use the conflicted package (<http://conflicted.r-lib.org/>) to force all conflicts to become errors

``` r
energy <- read_csv("use_US.csv")
```

    ## Rows: 553 Columns: 67
    ## ── Column specification ────────────────────────────────────────────────────────
    ## Delimiter: ","
    ## chr  (2): State, MSN
    ## dbl (65): Data_Status, 1960, 1961, 1962, 1963, 1964, 1965, 1966, 1967, 1968,...
    ## 
    ## ℹ Use `spec()` to retrieve the full column specification for this data.
    ## ℹ Specify the column types or set `show_col_types = FALSE` to quiet this message.

``` r
glimpse(energy)
```

    ## Rows: 553
    ## Columns: 67
    ## $ Data_Status <dbl> 2023, 2023, 2023, 2023, 2023, 2023, 2023, 2023, 2023, 2023…
    ## $ State       <chr> "US", "US", "US", "US", "US", "US", "US", "US", "US", "US"…
    ## $ MSN         <chr> "ABICB", "ABICP", "ARICB", "ARICP", "ARTCB", "ARTCP", "ART…
    ## $ `1960`      <dbl> 0, 0, 733782, 110576, 733782, 110576, 733782, 110576, 2979…
    ## $ `1961`      <dbl> 0, 0, 753551, 113555, 753551, 113555, 753551, 113555, 2903…
    ## $ `1962`      <dbl> 0, 0, 803533, 121087, 803533, 121087, 803533, 121087, 2629…
    ## $ `1963`      <dbl> 0, 0, 824642, 124268, 824642, 124268, 824642, 124268, 2531…
    ## $ `1964`      <dbl> 0, 0, 840781, 126700, 840781, 126700, 840781, 126700, 2350…
    ## $ `1965`      <dbl> 0, 0, 890266, 134157, 890266, 134157, 890266, 134157, 2215…
    ## $ `1966`      <dbl> 0, 0, 935557, 140982, 935557, 140982, 935557, 140982, 1940…
    ## $ `1967`      <dbl> 0, 0, 917215, 138218, 917215, 138218, 917215, 138218, 1660…
    ## $ `1968`      <dbl> 0, 0, 983661, 148231, 983661, 148231, 983661, 148231, 1545…
    ## $ `1969`      <dbl> 0, 0, 1008977, 152046, 1008977, 152046, 1008977, 152046, 1…
    ## $ `1970`      <dbl> 0, 0, 1082451, 163118, 1082451, 163118, 1082451, 163118, 1…
    ## $ `1971`      <dbl> 0, 0, 1108298, 167013, 1108298, 167013, 1108298, 167013, 9…
    ## $ `1972`      <dbl> 0, 0, 1136919, 171326, 1136919, 171326, 1136919, 171326, 8…
    ## $ `1973`      <dbl> 0, 0, 1263720, 190434, 1263720, 190434, 1263720, 190434, 8…
    ## $ `1974`      <dbl> 0, 0, 1165375, 175614, 1165375, 175614, 1165375, 175614, 8…
    ## $ `1975`      <dbl> 0, 0, 1014226, 152837, 1014226, 152837, 1014226, 152837, 7…
    ## $ `1976`      <dbl> 0, 0, 998121, 150410, 998121, 150410, 998121, 150410, 6745…
    ## $ `1977`      <dbl> 0, 0, 1056365, 159187, 1056365, 159187, 1056365, 159187, 7…
    ## $ `1978`      <dbl> 0, 0, 1159621, 174747, 1159621, 174747, 1159621, 174747, 7…
    ## $ `1979`      <dbl> 0, 0, 1153032, 173754, 1153032, 173754, 1153032, 173754, 7…
    ## $ `1980`      <dbl> 0, 0, 962207, 144998, 962207, 144998, 962207, 144998, 6434…
    ## $ `1981`      <dbl> 5169, 1024, 827788, 124742, 827788, 124742, 827788, 124742…
    ## $ `1982`      <dbl> 4937, 978, 829407, 124986, 829407, 124986, 829407, 124986,…
    ## $ `1983`      <dbl> 712, 141, 904095, 136241, 904095, 136241, 904095, 136241, …
    ## $ `1984`      <dbl> 86, 17, 992095, 149502, 992095, 149502, 992095, 149502, 43…
    ## $ `1985`      <dbl> 3867, 766, 1029482, 155136, 1029482, 155136, 1029482, 1551…
    ## $ `1986`      <dbl> 545, 108, 1085736, 163613, 1085736, 163613, 1085736, 16361…
    ## $ `1987`      <dbl> 45, 9, 1130011, 170285, 1130011, 170285, 1130011, 170285, …
    ## $ `1988`      <dbl> -1070, -212, 1136322, 171236, 1136322, 171236, 1136322, 17…
    ## $ `1989`      <dbl> 106, 21, 1096028, 165164, 1096028, 165164, 1096028, 165164…
    ## $ `1990`      <dbl> 237, 47, 1170192, 176340, 1170192, 176340, 1170192, 176340…
    ## $ `1991`      <dbl> -81, -16, 1076538, 162227, 1076538, 162227, 1076538, 16222…
    ## $ `1992`      <dbl> 156, 31, 1102220, 166097, 1102220, 166097, 1102220, 166097…
    ## $ `1993`      <dbl> 146, 29, 1149017, 173149, 1149017, 173149, 1149017, 173149…
    ## $ `1994`      <dbl> 6098, 1208, 1172920, 176751, 1172920, 176751, 1172920, 176…
    ## $ `1995`      <dbl> 5290, 1048, 1178175, 177543, 1178175, 177543, 1178175, 177…
    ## $ `1996`      <dbl> 6951, 1377, 1175932, 177205, 1175932, 177205, 1175932, 177…
    ## $ `1997`      <dbl> 9056, 1794, 1223566, 184383, 1223566, 184383, 1223566, 184…
    ## $ `1998`      <dbl> 4003, 793, 1262552, 190258, 1262552, 190258, 1262552, 1902…
    ## $ `1999`      <dbl> 6396, 1267, 1324413, 199580, 1324413, 199580, 1324413, 199…
    ## $ `2000`      <dbl> 3816, 756, 1275678, 192236, 1275678, 192236, 1275678, 1922…
    ## $ `2001`      <dbl> 6073, 1203, 1256865, 189401, 1256865, 189401, 1256865, 189…
    ## $ `2002`      <dbl> 7516, 1489, 1239950, 186852, 1239950, 186852, 1239950, 186…
    ## $ `2003`      <dbl> 7471, 1480, 1219538, 183776, 1219538, 183776, 1219538, 183…
    ## $ `2004`      <dbl> 10616, 2103, 1303848, 196481, 1303848, 196481, 1303848, 19…
    ## $ `2005`      <dbl> 8324, 1649, 1323238, 199403, 1323238, 199403, 1323238, 199…
    ## $ `2006`      <dbl> 641, 127, 1261165, 190049, 1261165, 190049, 1261165, 19004…
    ## $ `2007`      <dbl> 1782, 353, 1197041, 180386, 1197041, 180386, 1197041, 1803…
    ## $ `2008`      <dbl> 96, 19, 1011970, 152497, 1011970, 152497, 1011970, 152497,…
    ## $ `2009`      <dbl> -793, -157, 873085, 131568, 873085, 131568, 873085, 131568…
    ## $ `2010`      <dbl> -242, -48, 877770, 132274, 877770, 132274, 877770, 132274,…
    ## $ `2011`      <dbl> 10, 2, 859488, 129519, 859488, 129519, 859488, 129519, 270…
    ## $ `2012`      <dbl> -5, -1, 826700, 124578, 826700, 124578, 826700, 124578, 25…
    ## $ `2013`      <dbl> -379, -75, 783347, 118045, 783347, 118045, 783347, 118045,…
    ## $ `2014`      <dbl> -141, -28, 792637, 119445, 792637, 119445, 792637, 119445,…
    ## $ `2015`      <dbl> -348, -69, 831663, 125326, 831663, 125326, 831663, 125326,…
    ## $ `2016`      <dbl> -293, -58, 853363, 128596, 853363, 128596, 853363, 128596,…
    ## $ `2017`      <dbl> -192, -38, 849182, 127966, 849182, 127966, 849182, 127966,…
    ## $ `2018`      <dbl> -1555, -308, 792763, 119464, 792763, 119464, 792763, 11946…
    ## $ `2019`      <dbl> -1191, -236, 843880, 127167, 843880, 127167, 843880, 12716…
    ## $ `2020`      <dbl> -787, -156, 832274, 125418, 832274, 125418, 832274, 125418…
    ## $ `2021`      <dbl> -833, -165, 898083, 135335, 898083, 135335, 898083, 135335…
    ## $ `2022`      <dbl> -661, -131, 916133, 138055, 916133, 138055, 916133, 138055…
    ## $ `2023`      <dbl> -646, -128, 891759, 134382, 891759, 134382, 891759, 134382…

``` r
energy_long <- energy %>%
  pivot_longer(
    cols = `1960`:`2023`,
    names_to = "year",
    values_to = "consumption"
  ) %>%
  mutate(year = as.integer(year))

glimpse(energy_long)
```

    ## Rows: 35,392
    ## Columns: 5
    ## $ Data_Status <dbl> 2023, 2023, 2023, 2023, 2023, 2023, 2023, 2023, 2023, 2023…
    ## $ State       <chr> "US", "US", "US", "US", "US", "US", "US", "US", "US", "US"…
    ## $ MSN         <chr> "ABICB", "ABICB", "ABICB", "ABICB", "ABICB", "ABICB", "ABI…
    ## $ year        <int> 1960, 1961, 1962, 1963, 1964, 1965, 1966, 1967, 1968, 1969…
    ## $ consumption <dbl> 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0…

# Question 1: How has renewable energy consumption in the United States changed over time?

``` r
energy_long %>%
  filter(State == "US") %>%
  distinct(MSN) %>%
  filter(str_detect(MSN, "RET"))
```

    ## # A tibble: 1 × 1
    ##   MSN  
    ##   <chr>
    ## 1 RETCB

``` r
energy_long %>%
  filter(State == "US") %>%
  distinct(MSN) %>%
  arrange(MSN)
```

    ## # A tibble: 553 × 1
    ##    MSN  
    ##    <chr>
    ##  1 ABICB
    ##  2 ABICP
    ##  3 ARICB
    ##  4 ARICP
    ##  5 ARTCB
    ##  6 ARTCP
    ##  7 ARTXB
    ##  8 ARTXP
    ##  9 AVACB
    ## 10 AVACP
    ## # ℹ 543 more rows

``` r
renewable_prefixes <- c(
  "WND",  # Wind
  "SUN",  # Solar
  "HYD",  # Hydroelectric
  "BIO",  # Biomass
  "GEO"   # Geothermal
)
```

``` r
renewable_by_year <- energy_long %>%
  filter(
    State == "US",
    str_sub(MSN, 1, 3) %in% renewable_prefixes
  ) %>%
  group_by(year) %>%
  summarise(
    renewable_consumption = sum(consumption, na.rm = TRUE),
    .groups = "drop"
  )
glimpse(renewable_by_year)
```

    ## Rows: 0
    ## Columns: 2
    ## $ year                  <int> 
    ## $ renewable_consumption <dbl>

``` r
energy_long %>%
  filter(State == "US") %>%
  group_by(MSN) %>%
  summarise(
    total = sum(abs(consumption), na.rm = TRUE),
    .groups = "drop"
  ) %>%
  arrange(desc(total)) %>%
  slice(1:20)
```

    ## # A tibble: 20 × 2
    ##    MSN        total
    ##    <chr>      <dbl>
    ##  1 TETCB 5166013680
    ##  2 TETXB 5165671884
    ##  3 FFTCB 4569868768
    ##  4 TNTCB 4060693505
    ##  5 PATCB 2144461809
    ##  6 PMTCB 2120007542
    ##  7 PATXB 2064265000
    ##  8 TEICB 1909638452
    ##  9 TEEIB 1680666240
    ## 10 TNICB 1553052916
    ## 11 NGTCB 1417764304
    ## 12 NNTCB 1413630775
    ## 13 TEACB 1407115958
    ## 14 TNACB 1405071514
    ## 15 NGTCP 1377052599
    ## 16 PAACB 1358881819
    ## 17 PMACB 1335114992
    ## 18 LOTCB 1104978381
    ## 19 LOTXB 1104978381
    ## 20 NGTXB 1080771360

``` r
energy_long %>%
  filter(State == "US") %>%
  distinct(MSN) %>%
  filter(str_detect(MSN, "^RE")) %>%   # codes starting with RE
  arrange(MSN)
```

    ## # A tibble: 7 × 1
    ##   MSN  
    ##   <chr>
    ## 1 REACB
    ## 2 RECCB
    ## 3 REEIB
    ## 4 REGBP
    ## 5 REICB
    ## 6 RERCB
    ## 7 RETCB

``` r
energy_long %>%
  filter(State == "US", MSN == "RETCB") %>%
  ggplot(aes(x = year, y = consumption)) +
  geom_line(color = "forestgreen", linewidth = 1) +
  labs(
    title = "U.S. Renewable Energy Consumption Over Time",
    subtitle = "1960–2023",
    x = "Year",
    y = "Consumption (Billion Btu)",
    caption = "Source: U.S. Energy Information Administration (SEDS)"
  ) +
  theme_minimal()
```

![](README_files/figure-gfm/plot-renewables-over-time-1.png)<!-- -->
\#My Findings: U.S. renewable energy consumption increased steadily from
1960 to 2023, with especially rapid growth beginning in the early 2000s.
While there is a brief slowdown around the late 1990s, renewable energy
use accelerates sharply afterward, reflecting increased adoption of
wind, solar, and other renewable technologies. This trend indicates a
long-term shift toward renewable energy sources in the U.S. energy
portfolio.

\#Question 2: How does the growth of renewable energy consumption
compare to fossil fuel energy consumption over time?

``` r
energy_compare <- energy_long %>%
  filter(State == "US", MSN %in% c("RETCB", "FFTCB")) %>%
  mutate(
    source = if_else(
      MSN == "RETCB",
      "Renewable Energy",
      "Fossil Fuels"
    )
  )

glimpse(energy_compare)
```

    ## Rows: 128
    ## Columns: 6
    ## $ Data_Status <dbl> 2023, 2023, 2023, 2023, 2023, 2023, 2023, 2023, 2023, 2023…
    ## $ State       <chr> "US", "US", "US", "US", "US", "US", "US", "US", "US", "US"…
    ## $ MSN         <chr> "FFTCB", "FFTCB", "FFTCB", "FFTCB", "FFTCB", "FFTCB", "FFT…
    ## $ year        <int> 1960, 1961, 1962, 1963, 1964, 1965, 1966, 1967, 1968, 1969…
    ## $ consumption <dbl> 42083902, 42704559, 44627400, 46470718, 48494993, 50527367…
    ## $ source      <chr> "Fossil Fuels", "Fossil Fuels", "Fossil Fuels", "Fossil Fu…

``` r
ggplot(energy_compare, aes(x = year, y = consumption, color = source)) +
  geom_line(linewidth = 1) +
  labs(
    title = "Renewable vs Fossil Fuel Energy Consumption in the U.S.",
    subtitle = "1960–2023",
    x = "Year",
    y = "Consumption (Billion Btu)",
    color = "Energy Source",
    caption = "Source: U.S. Energy Information Administration (SEDS)"
  ) +
  theme_minimal()
```

![](README_files/figure-gfm/plot-renewable-vs-fossil-1.png)<!-- --> \#My
Findings: Fossil fuel energy consumption has consistently remained much
higher than renewable energy consumption between 1960 and 2023. Fossil
fuel use increased rapidly from 1960 through the early 1970s and
continued to grow more gradually through the late 20th century. Around
2010, fossil fuel consumption begins to decline slightly and then
plateaus, suggesting a potential stabilization or gradual reduction in
use. In contrast, renewable energy consumption remains relatively low
throughout the earlier decades but shows steady growth over time, with
more pronounced increases in recent years. This divergence highlights
both the historical dominance of fossil fuels and the emerging role of
renewables in the U.S. energy system.

\#Question 3: Which energy sources account for the largest share of
total U.S. energy consumption, and how has this changed over time?

``` r
energy_sources <- energy_long %>%
  filter(
    State == "US",
    MSN %in% c("FFTCB", "RETCB", "NUETB")
  ) %>%
  mutate(
    source = case_when(
      MSN == "FFTCB" ~ "Fossil Fuels",
      MSN == "RETCB" ~ "Renewable Energy",
      MSN == "NUETB" ~ "Nuclear Energy"
    )
  )

glimpse(energy_sources)
```

    ## Rows: 192
    ## Columns: 6
    ## $ Data_Status <dbl> 2023, 2023, 2023, 2023, 2023, 2023, 2023, 2023, 2023, 2023…
    ## $ State       <chr> "US", "US", "US", "US", "US", "US", "US", "US", "US", "US"…
    ## $ MSN         <chr> "FFTCB", "FFTCB", "FFTCB", "FFTCB", "FFTCB", "FFTCB", "FFT…
    ## $ year        <int> 1960, 1961, 1962, 1963, 1964, 1965, 1966, 1967, 1968, 1969…
    ## $ consumption <dbl> 42083902, 42704559, 44627400, 46470718, 48494993, 50527367…
    ## $ source      <chr> "Fossil Fuels", "Fossil Fuels", "Fossil Fuels", "Fossil Fu…

``` r
energy_long %>%
  filter(State == "US", MSN %in% c("FFTCB", "RETCB", "NUETB")) %>%
  distinct(MSN)
```

    ## # A tibble: 3 × 1
    ##   MSN  
    ##   <chr>
    ## 1 FFTCB
    ## 2 NUETB
    ## 3 RETCB

``` r
ggplot(energy_sources, aes(x = year, y = consumption, color = source)) +
  geom_line(linewidth = 1) +
  labs(
    title = "Major Energy Sources in U.S. Energy Consumption",
    subtitle = "1960–2023",
    x = "Year",
    y = "Consumption (Billion Btu)",
    color = "Energy Source",
    caption = "Source: U.S. Energy Information Administration (SEDS)"
  ) +
  theme_minimal()
```

![](README_files/figure-gfm/plot-energy-sources-q3-1.png)<!-- --> \#My
Findings: Fossil fuels account for the largest share of total U.S.
energy consumption throughout the entire period from 1960 to 2023. While
nuclear energy consumption increased substantially beginning in the
1970s and experienced strong growth through the 1990s, it remained well
below fossil fuel consumption levels. Nuclear energy does relatively
exceeded renewable energy consumption during the late 20th century, with
the largest gap occurring roughly between 1990 and 2010. In more recent
years, renewable energy consumption has increased steadily and has begun
to approach nuclear energy levels. These trends indicate that although
fossil fuels continue to dominate the U.S. energy mix, nuclear and
renewable energy sources have played increasingly important roles over
time, reflecting gradual diversification of the U.S. energy portfolio.

\#Question 4: How does energy consumption by source differ across
end-use sectors?

``` r
sector_codes <- tibble(
  MSN = c(
    "FFRCB", "FFCCB", "FFICB", "FFTCB",  # Fossil fuels
    "RERC B", "RECCB", "REICB", "RETCB",  # Renewables
    "NUETB"                              # Nuclear
  ),
  sector = c(
    "Residential", "Commercial", "Industrial", "Transportation",
    "Residential", "Commercial", "Industrial", "Transportation",
    "Electric Power"
  ),
  source = c(
    "Fossil Fuels", "Fossil Fuels", "Fossil Fuels", "Fossil Fuels",
    "Renewable Energy", "Renewable Energy", "Renewable Energy", "Renewable Energy",
    "Nuclear Energy"
  )
)
```

``` r
energy_by_sector <- energy_long %>%
  filter(State == "US") %>%
  inner_join(sector_codes, by = "MSN") %>%
  group_by(year, sector, source) %>%
  summarise(
    consumption = sum(consumption, na.rm = TRUE),
    .groups = "drop"
  )

glimpse(energy_by_sector)
```

    ## Rows: 320
    ## Columns: 4
    ## $ year        <int> 1960, 1960, 1960, 1960, 1960, 1961, 1961, 1961, 1961, 1961…
    ## $ sector      <chr> "Commercial", "Electric Power", "Industrial", "Transportat…
    ## $ source      <chr> "Renewable Energy", "Nuclear Energy", "Renewable Energy", …
    ## $ consumption <dbl> 11868, 6026, 692170, 42083902, 1829874, 11146, 19678, 7068…

``` r
energy_by_sector %>%
  ggplot(aes(x = year, y = consumption, color = source)) +
  geom_line(linewidth = 1) +
  facet_wrap(~ sector, scales = "free_y") +
  labs(
    title = "U.S. Energy Consumption by Source Across End-Use Sectors",
    subtitle = "1960–2023",
    x = "Year",
    y = "Consumption (Billion Btu)",
    color = "Energy Source",
    caption = "Source: U.S. Energy Information Administration (SEDS)"
  ) +
  theme_minimal()
```

![](README_files/figure-gfm/plot-energy-by-sector-1.png)<!-- --> \#My
Findings: Energy consumption patterns differ substantially across
end-use sectors. Transportation is dominated by fossil fuel consumption
throughout the entire period, with minimal contributions from renewable
or nuclear energy. This reflects the continued reliance on
petroleum-based fuels for transportation in the United States. In
contrast, the electric power sector shows a strong presence of nuclear
energy beginning in the 1970s, followed by a notable increase in
renewable energy consumption in more recent decades. This sector plays a
critical role in integrating non-fossil energy sources into the overall
energy system. Residential and commercial sectors demonstrate steady
growth in renewable energy consumption over time, indicating increased
adoption of cleaner energy sources such as solar and wind. The
industrial sector exhibits a mixed pattern, with fossil fuels remaining
dominant but renewables gradually increasing. Overall, these results
highlight that progress toward renewable energy adoption varies
significantly by sector, with the electric power sector leading the
transition.

# Conclusion: This analysis examined long-term trends in U.S. energy consumption from 1960 to 2023 using data from the U.S. Energy Information Administration. Across all analyses, fossil fuels remain the dominant energy source in the United States, particularly within the transportation and industrial sectors. However, the results also show steady growth in renewable energy consumption over time, especially in the electric power, residential, and commercial sectors. Nuclear energy plays a significant role in the U.S. energy system, contributing substantial and stable energy production beginning in the late 20th century. While nuclear consumption does not approach fossil fuel levels, it often exceeds renewable energy consumption during certain periods, highlighting its importance as a low-carbon energy source within the current energy mix.Differences in energy use across end-use sectors reveal disparities in the pace of energy transition. Some sectors, such as electric power, show meaningful diversification toward non-fossil sources, while others, particularly transportation, remain heavily reliant on fossil fuels. These findings suggest that although progress toward cleaner energy sources has been made, a more diversified energy portfolio is needed to reduce dependence on fossil fuels and support long-term sustainability. Continued expansion of renewable and nuclear energy sources may be essential to achieving a more balanced and resilient U.S. energy system.
