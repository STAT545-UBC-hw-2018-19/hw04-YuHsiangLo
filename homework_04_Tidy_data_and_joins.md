Homework 04: Tidy data and joins
================
Roger Yu-Hsiang Lo
2018-10-09

-   [Bring rectangular data in](#bring-rectangular-data-in)
-   [Data reshaping](#data-reshaping)
-   [Data join](#data-join)

Bring rectangular data in
-------------------------

-   Load the `Gapminder`, `tidyverse`, and `GGally`:

``` r
#install.packages("GGally")
library(gapminder)
library(tidyverse)
library(GGally)
```

-   Some sanity check to make sure the `Gapminder` data was loaded properly:

``` r
head(gapminder) %>%
  knitr::kable(.)
```

| country     | continent |  year|  lifeExp|       pop|  gdpPercap|
|:------------|:----------|-----:|--------:|---------:|----------:|
| Afghanistan | Asia      |  1952|   28.801|   8425333|   779.4453|
| Afghanistan | Asia      |  1957|   30.332|   9240934|   820.8530|
| Afghanistan | Asia      |  1962|   31.997|  10267083|   853.1007|
| Afghanistan | Asia      |  1967|   34.020|  11537966|   836.1971|
| Afghanistan | Asia      |  1972|   36.088|  13079460|   739.9811|
| Afghanistan | Asia      |  1977|   38.438|  14880372|   786.1134|

Data reshaping
--------------

The life expectancy of three Asian countries (i.e., Japan, South Korea, and Taiwan) and three African countries (i.e., Benin, Ghana, and Togo) over the years is tabulated below:

``` r
gapminder %>%
  filter(country %in% c("Japan", "Korea, Rep.", "Taiwan", "Benin", "Ghana", "Togo")) %>%
  select(country, year, lifeExp) %>%
  spread(key = country, value = lifeExp) %>%
  select(year, "Japan", "Korea, Rep.", "Taiwan", "Benin", "Ghana", "Togo") %>%  # Reorder the columns
  knitr::kable()
```

|  year|   Japan|  Korea, Rep.|  Taiwan|   Benin|   Ghana|    Togo|
|-----:|-------:|------------:|-------:|-------:|-------:|-------:|
|  1952|  63.030|       47.453|   58.50|  38.223|  43.149|  38.596|
|  1957|  65.500|       52.681|   62.40|  40.358|  44.779|  41.208|
|  1962|  68.730|       55.292|   65.20|  42.618|  46.452|  43.922|
|  1967|  71.430|       57.716|   67.50|  44.885|  48.072|  46.769|
|  1972|  73.420|       62.612|   69.39|  47.014|  49.875|  49.759|
|  1977|  75.380|       64.766|   70.59|  49.190|  51.756|  52.887|
|  1982|  77.110|       67.123|   72.16|  50.904|  53.744|  55.471|
|  1987|  78.670|       69.810|   73.40|  52.337|  55.729|  56.941|
|  1992|  79.360|       72.244|   74.26|  53.919|  57.501|  58.061|
|  1997|  80.690|       74.647|   75.25|  54.777|  58.556|  58.390|
|  2002|  82.000|       77.045|   76.99|  54.406|  58.453|  57.561|
|  2007|  82.603|       78.623|   78.40|  56.728|  60.022|  58.420|

We can see the correlation of life expectancy between different countries by using a correlation plot:

``` r
gapminder %>%
  filter(country %in% c("Japan", "Korea, Rep.", "Taiwan", "Benin", "Ghana", "Togo")) %>%
  select(country, year, lifeExp) %>%
  spread(key = country, value = lifeExp) %>%
  select(year, "Japan", "Korea, Rep.", "Taiwan", "Benin", "Ghana", "Togo") %>%
  ggpairs(data = ., columns = 2:7)
```

<img src="homework_04_Tidy_data_and_joins_files/figure-markdown_github/unnamed-chunk-4-1.png" style="display: block; margin: auto;" />

As shown in the plot, life expectancy over the years is highly correlated across different countries, even countries in different continents.

Data join
---------
