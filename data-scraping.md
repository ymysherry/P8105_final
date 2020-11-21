Data scraping
================
11/15/2020

# Presidential poll

## PA example

``` r
pres_url = "https://www.realclearpolitics.com/epolls/2020/president/pa/pennsylvania_trump_vs_biden-6861.html"
pres_html = read_html(pres_url)
pres_pa = 
  pres_html %>% html_nodes(css = ".large") %>% 
  html_table()

pres_pa = pres_pa[[2]] %>% janitor::clean_names()

pres_1 = pres_pa %>% 
  mutate(biden = biden_d,
         trump = trump_r,
         state = "PA"
         ) %>% 
select(state,poll, date,biden, trump) %>% 
  separate(date,into = c("start_date","end_date"), sep = "-") 
```

    ## Warning: Expected 2 pieces. Additional pieces discarded in 1 rows [1].

``` r
pres_1 = pres_1 %>% 
  mutate(
    start_date = as.Date(pres_1$start_date, "%m/%d"),
    end_date = as.Date(pres_1$end_date, "%m/%d")
  )
```

## Create full list, list that the url has only 1 table & list that the url has 2 tables

Create Full URL list

``` r
state_list = list(state_name = list("florida","pennsylvania","wisconsin","north_carolina","michigan","ohio","minnesota","iowa", "arizona","nevada","texax","georgia","virginia","new_hampshire","maine","colorado","new_mexico"),

  
  url = list("https://www.realclearpolitics.com/epolls/2020/president/fl/florida_trump_vs_biden-6841.html", "https://www.realclearpolitics.com/epolls/2020/president/pa/pennsylvania_trump_vs_biden-6861.html","https://www.realclearpolitics.com/epolls/2020/president/wi/wisconsin_trump_vs_biden-6849.html", "https://www.realclearpolitics.com/epolls/2020/president/nc/north_carolina_trump_vs_biden-6744.html","https://www.realclearpolitics.com/epolls/2020/president/mi/michigan_trump_vs_biden-6761.html","https://www.realclearpolitics.com/epolls/2020/president/oh/ohio_trump_vs_biden-6765.html","https://www.realclearpolitics.com/epolls/2020/president/mn/minnesota_trump_vs_biden-6966.html","https://www.realclearpolitics.com/epolls/2020/president/ia/iowa_trump_vs_biden-6787.html","https://www.realclearpolitics.com/epolls/2020/president/az/arizona_trump_vs_biden-6807.html","https://www.realclearpolitics.com/epolls/2020/president/nv/nevada_trump_vs_biden-6867.html","https://www.realclearpolitics.com/epolls/2020/president/tx/texas_trump_vs_biden-6818.html","https://www.realclearpolitics.com/epolls/2020/president/ga/georgia_trump_vs_biden-6974.html","https://www.realclearpolitics.com/epolls/2020/president/va/virginia_trump_vs_biden-6988.html","https://www.realclearpolitics.com/epolls/2020/president/nh/new_hampshire_trump_vs_biden-6779.html","https://www.realclearpolitics.com/epolls/2020/president/me/maine_trump_vs_biden-6922.html","https://www.realclearpolitics.com/epolls/2020/president/co/colorado_trump_vs_biden-6940.html","https://www.realclearpolitics.com/epolls/2020/president/nm/new_mexico_trump_vs_biden-6993.html"))
```

List that has 2 tables from FL to GA

``` r
state_list_2 = list(
  state_name = c("florida","pennsylvania","wisconsin","north_carolina","michigan","ohio","minnesota","iowa", "arizona","nevada","texax","georgia"),

                url = c("https://www.realclearpolitics.com/epolls/2020/president/fl/florida_trump_vs_biden-6841.html", "https://www.realclearpolitics.com/epolls/2020/president/pa/pennsylvania_trump_vs_biden-6861.html","https://www.realclearpolitics.com/epolls/2020/president/wi/wisconsin_trump_vs_biden-6849.html", "https://www.realclearpolitics.com/epolls/2020/president/nc/north_carolina_trump_vs_biden-6744.html","https://www.realclearpolitics.com/epolls/2020/president/mi/michigan_trump_vs_biden-6761.html","https://www.realclearpolitics.com/epolls/2020/president/oh/ohio_trump_vs_biden-6765.html","https://www.realclearpolitics.com/epolls/2020/president/mn/minnesota_trump_vs_biden-6966.html","https://www.realclearpolitics.com/epolls/2020/president/ia/iowa_trump_vs_biden-6787.html","https://www.realclearpolitics.com/epolls/2020/president/az/arizona_trump_vs_biden-6807.html","https://www.realclearpolitics.com/epolls/2020/president/nv/nevada_trump_vs_biden-6867.html","https://www.realclearpolitics.com/epolls/2020/president/tx/texas_trump_vs_biden-6818.html","https://www.realclearpolitics.com/epolls/2020/president/ga/georgia_trump_vs_biden-6974.html")
)
```

Function\_read url

``` r
url_function = function(x){


pres_df = read_html(x) %>% 
  html_nodes(css = ".large") %>% 
  html_table()
  
}
```

Apply url function

``` r
url_list_2 = map(state_list_2[[2]],url_function)
```

select list 2 function

``` r
select_function = function(x){
  x[[2]] %>% janitor::clean_names()
}
```

apply selection function

``` r
selection_list_2 = map(url_list_2,select_function)
```

data cleaning function

``` r
clean_function = function(x){
  x %>% 
  mutate(biden = biden_d,
         trump = trump_r
         ) %>% 
select(poll, date,biden, trump) %>% 
  separate(date,into = c("start_date","end_date"), sep = "-")
    
}
```

apply data cleaning function

``` r
cleaned_list_2 = map(selection_list_2, clean_function)
```

    ## Warning: Expected 2 pieces. Additional pieces discarded in 1 rows [1].
    
    ## Warning: Expected 2 pieces. Additional pieces discarded in 1 rows [1].
    
    ## Warning: Expected 2 pieces. Additional pieces discarded in 1 rows [1].
    
    ## Warning: Expected 2 pieces. Additional pieces discarded in 1 rows [1].
    
    ## Warning: Expected 2 pieces. Additional pieces discarded in 1 rows [1].
    
    ## Warning: Expected 2 pieces. Additional pieces discarded in 1 rows [1].
    
    ## Warning: Expected 2 pieces. Additional pieces discarded in 1 rows [1].
    
    ## Warning: Expected 2 pieces. Additional pieces discarded in 1 rows [1].
    
    ## Warning: Expected 2 pieces. Additional pieces discarded in 1 rows [1].
    
    ## Warning: Expected 2 pieces. Additional pieces discarded in 1 rows [1].
    
    ## Warning: Expected 2 pieces. Additional pieces discarded in 1 rows [1].
    
    ## Warning: Expected 2 pieces. Additional pieces discarded in 1 rows [1].

Transform date format function

``` r
date_function = function(x){
  x %>% 
  mutate(
    start_date = as.Date(x$start_date, "%m/%d"),
    end_date = as.Date(x$end_date, "%m/%d")
  )
}
```

Apply date function

``` r
dateformat_list_2 = map(cleaned_list_2, date_function)
```

List that has 1 table from VA to NM

``` r
state_list_1 = list(
  state_name = c("virginia","new_hampshire","maine","colorado","new_mexico"),

                url = c("https://www.realclearpolitics.com/epolls/2020/president/va/virginia_trump_vs_biden-6988.html","https://www.realclearpolitics.com/epolls/2020/president/nh/new_hampshire_trump_vs_biden-6779.html","https://www.realclearpolitics.com/epolls/2020/president/me/maine_trump_vs_biden-6922.html","https://www.realclearpolitics.com/epolls/2020/president/co/colorado_trump_vs_biden-6940.html","https://www.realclearpolitics.com/epolls/2020/president/nm/new_mexico_trump_vs_biden-6993.html")
)
```

Apply url function

``` r
url_list_1 = map(state_list_1[[2]],url_function)
```

select list 1 function

``` r
select_function = function(x){
  x[[1]] %>% janitor::clean_names()
}
```

apply selection function

``` r
selection_list_1 = map(url_list_1, select_function)
```

apply data cleaning function

``` r
cleaned_list_1 = map(selection_list_1, clean_function)
```

    ## Warning: Expected 2 pieces. Additional pieces discarded in 1 rows [1].
    
    ## Warning: Expected 2 pieces. Additional pieces discarded in 1 rows [1].
    
    ## Warning: Expected 2 pieces. Additional pieces discarded in 1 rows [1].
    
    ## Warning: Expected 2 pieces. Additional pieces discarded in 1 rows [1].
    
    ## Warning: Expected 2 pieces. Additional pieces discarded in 1 rows [1].

Apply date function

``` r
dateformat_list_1 = map(cleaned_list_1, date_function)
```

Combine 2 lists

``` r
state_poll = c(
  dateformat_list_2,
  dateformat_list_1
) 
```
