# Dataset contract — VanArsdel Executive Dashboard

## Source files

| File | Sheet / Tab | Rows (approx) | Period | Notes |
|------|-------------|--------------|--------|-------|
| `VanArsdel_Actuals.xlsx` | `Date` | — | 2015–2020 | Date dimension |
| `VanArsdel_Actuals.xlsx` | `Campaign` | — | — | Campaign/marketing dimension |
| `VanArsdel_Actuals.xlsx` | `Customer` | — | — | Customer dimension |
| `VanArsdel_Actuals.xlsx` | `Product` | — | — | Product hierarchy dimension |
| `VanArsdel_Actuals.xlsx` | `Geo` | — | — | Geography dimension |
| `VanArsdel_Actuals.xlsx` | `Sales` | ~675 000 | 2015–2020 | Fact table — actual sales |
| `VanArsdel_Budget_Forecast.xlsx` | *(main sheet)* | — | TBD | Budget & forecast — wide format |

## How to load

See `Actuals_Path.txt`, `Budget_Path.txt`, `Number_Days.txt` — Power Query M function hints
describing how source files are referenced. Use these to understand the intended folder structure
before building the semantic model partition.

## Paste target

Place source files in `dataset/raw/`. The dataset contract is satisfied when all files above are present
and column names match the data dictionary exactly.
