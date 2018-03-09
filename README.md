Popularity of searches for Meatball and how that relates to National Meatball Day
================
Kevin "lil Sweet" Thompson
3/9/2018

National Meatball Day
=====================

It turns out, [that's a real thing](https://www.google.com/search?q=international+meatball+day&oq=international+meatball+day&aqs=chrome..69i57j0.3576j0j7&sourceid=chrome&ie=UTF-8).

But strangely, it appears that searches for meatball are not as popular during the week of National Meatball day as they are during other parts of the year.

``` r
library(tidyverse)
```

    ## ── Attaching packages ─────────────────────────────────────── tidyverse 1.2.1 ──

    ## ✔ ggplot2 2.2.1     ✔ purrr   0.2.4
    ## ✔ tibble  1.4.2     ✔ dplyr   0.7.4
    ## ✔ tidyr   0.8.0     ✔ stringr 1.2.0
    ## ✔ readr   1.1.1     ✔ forcats 0.3.0

    ## ── Conflicts ────────────────────────────────────────── tidyverse_conflicts() ──
    ## ✖ dplyr::filter() masks stats::filter()
    ## ✖ dplyr::lag()    masks stats::lag()

``` r
library(lubridate)
```

    ## 
    ## Attaching package: 'lubridate'

    ## The following object is masked from 'package:base':
    ## 
    ##     date

``` r
mb <- read_csv("meatball.csv", skip=2, col_names = c("week", "count"), col_types = cols(week=col_date(), count=col_integer()))
```

    ## Warning: 2 parsing failures.
    ## row # A tibble: 2 x 5 col     row col   expected     actual                file           expected   <int> <chr> <chr>        <chr>                 <chr>          actual 1     1 week  "date like " Week                  'meatball.csv' file 2     1 count an integer   meatball: (Worldwide) 'meatball.csv'

``` r
herokuLight <- "#D3CBED"
```

``` r
plot <- mb %>%
  ggplot(aes(x=week, y=count/100)) +
  
  ## This is ugly. There has to be a better way to shade the weeks where meatball day fell
  geom_rect(fill=herokuLight, aes(ymin=0, ymax=Inf, xmin=ymd("2014-03-09"), xmax=ymd("2014-03-16"))) + 
  geom_rect(fill=herokuLight, aes(ymin=0, ymax=Inf, xmin=ymd("2015-03-08"), xmax=ymd("2015-03-15"))) +
  geom_rect(fill=herokuLight, aes(ymin=0, ymax=Inf, xmin=ymd("2016-03-06"), xmax=ymd("2016-03-13"))) +
  geom_rect(fill=herokuLight, aes(ymin=0, ymax=Inf, xmin=ymd("2017-03-05"), xmax=ymd("2017-03-12"))) +
  
  geom_line(stat="identity") +
  theme_light() +
  scale_y_continuous() + 
  labs(title="Five years of Google searches for \"meatball\"",
       subtitle = "Source: Google Trends as of Mar 9, 2018",
       y="Proportion of searches",
       x="Week")
print(plot)
```

    ## Warning: Removed 1 rows containing missing values (geom_path).

![](meatball_files/figure-markdown_github-ascii_identifiers/viz-1.png)

``` r
ggsave(plot, filename="meatball.png")
```

    ## Saving 7 x 5 in image

    ## Warning: Removed 1 rows containing missing values (geom_path).
