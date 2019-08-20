# Mind the gap

Data from the [gapminder](https://github.com/jennybc/gapminder) package spread across multiple files to provide data ingest and parameterised report practice.

```R
usethis::use_course("http://bit.ly/30lAoSe")
```

## Data generation

```r
library(tidyverse)
library(gapminder)
library(writexl)

by_year <- gapminder %>% nest(data = -year)
by_country <- gapminder %>% nest(data = -country)
by_continent <- gapminder %>% nest(data = -continent)

# excel -------------------------------------------------------------------
dir.create("excel")
write_xlsx(set_names(by_year$data, by_year$year), "excel/gapminder-year.xlsx")
write_xlsx(set_names(by_country$data, by_country$country), "excel/gapminder-country.xlsx")

path <- paste0("excel/continent-", by_continent$continent, ".xlsx")
walk2(by_continent$data, path, function(data, path) {
  by_country <- data %>% nest(data = -country)
  write_xlsx(set_names(by_country$data, by_country$country), path)
})

# csv ---------------------------------------------------------------------
dir.create("csv")

dir.create("csv/year")
by_year$path <- paste0("csv/year/", by_year$year, ".csv")
walk2(by_year$data, by_year$path, write_csv)

dir.create("csv/country")
by_country$path <- paste0("csv/country/", by_country$country, ".csv")
walk2(by_country$data, by_country$path, write_csv)
```
