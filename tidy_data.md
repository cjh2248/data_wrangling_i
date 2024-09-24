Tidy Data
================

This document wil show how to tidy data.

``` r
pulse_df = 
  haven::read_sas("./data/public_pulse_data.sas7bdat") |>
  janitor::clean_names()
```

This needs to go from wide to long format.

``` r
pulse_tidy_df = 
  pulse_df |>
  pivot_longer(
    cols = bdi_score_bl:bdi_score_12m,
    names_to = "visit", 
    values_to = "bdi_score",
    names_prefix = "bdi_score_"
    ) |>
    mutate(
      visit = replace(visit, visit == "bl", "00m")
    ) |>
    relocate(id,visit)
```

Do one more example.

``` r
litters_df = 
  read_csv("data/FAS_litters.csv", na = c("NA", ".", "")) |>
  janitor::clean_names() |>
  pivot_longer(
    cols = gd0_weight:gd18_weight, 
    names_to= "gd_time", 
    values_to = "weight"
  ) |>
  mutate(
    gd_time = case_match(
      gd_time, 
      "gd0_weight" ~ 0, 
      "gd18_weight" ~ 18
    ))
```

    ## Rows: 49 Columns: 8
    ## ── Column specification ────────────────────────────────────────────────────────
    ## Delimiter: ","
    ## chr (2): Group, Litter Number
    ## dbl (6): GD0 weight, GD18 weight, GD of Birth, Pups born alive, Pups dead @ ...
    ## 
    ## ℹ Use `spec()` to retrieve the full column specification for this data.
    ## ℹ Specify the column types or set `show_col_types = FALSE` to quiet this message.

``` r
  fellowship_ring = 
    read_excel("data/LotR_Words.xlsx", range = "B3:D6") |> 
    mutate(movie = "fellowship_ring")

  two_towers = 
    read_excel("data/LotR_Words.xlsx", range = "F3:H6") |> 
    mutate(movie = "two_towers")
  
  return_king = 
    read_excel("data/LotR_Words.xlsx", range = "J3:L6") |> 
    mutate(movie = "return_king")
  
  lotr_df = 
    bind_rows(fellowship_ring, two_towers, return_king) |> 
    janitor::clean_names() |> 
    pivot_longer(
    cols = female:male, 
    names_to = "sex", 
    values_to = "words"
  ) |>  
  relocate(movie) |> 
  mutate(race = str_to_lower(race))
```
