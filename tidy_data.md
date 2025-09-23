Tidy Data
================
Ruihan Ding
2025-09-23

## pivot longer

Example 1

``` r
pulse_df = 
  read_sas("data_import_examples/public_pulse_data.sas7bdat") |> 
  janitor::clean_names() |> 
  pivot_longer(
    cols = bdi_score_bl:bdi_score_12m, 
    names_to = "visit", 
    values_to = "bdi_score", 
    names_prefix = "bdi_score_"
  ) |> 
  mutate(visit = replace(visit, visit == "bl", "00m")) |> 
  relocate(id, visit)
```

Example 2

``` r
litters_df = 
  read_csv("data_import_examples/FAS_litters.csv", na = c("NA", ".", "")) |> 
  janitor::clean_names() |> 
  pivot_longer(
    cols = gd0_weight:gd18_weight, 
    names_to = "gd_time",
    values_to = "weight"
  ) |> 
  mutate(gd_time = case_match(
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
