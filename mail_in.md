mail-in\_votes
================
ymysherry
12/4/2020

## Comparison of both candidates’ mail-in votes

\#Compare the number of mail-in ballots of dem/rep in each state

``` r
returnedballots_df =
  read_csv("data/mail_in.csv") %>% 
  select(state, party, returned_ballots) %>% 
  filter(party %in% c("Democrats", "Republicans")) %>%
  group_by(state, party) 
```

    ## Parsed with column specification:
    ## cols(
    ##   state = col_character(),
    ##   party = col_character(),
    ##   returned_ballots = col_number(),
    ##   freq_distribution = col_double(),
    ##   requested_ballots = col_number(),
    ##   return_rate = col_double()
    ## )

``` r
plot_returned_ballots = 
  returnedballots_df %>%
  ggplot(aes(x = state, y = returned_ballots, fill = party)) +
  geom_bar(stat="identity") +
  theme(axis.text.x = element_text(angle = 90, vjust = 0.5, hjust = 1)) +
  labs(
  title = "The number of returned mail-in ballots of each party by state",
  x = "State",
  y = "Number of returned mail-in ballots",
  caption = "Data from 'https://electproject.github.io/Early-Vote-2020G/'"
  )
```

\#Compare the return rate of mail-in ballots for each party by state

``` r
returnrate_df =
  read_csv("data/mail_in.csv") %>% 
  select(state, party, return_rate) %>% 
  filter(party %in% c("Democrats", "Republicans")) %>%
  group_by(state, party) 
```

    ## Parsed with column specification:
    ## cols(
    ##   state = col_character(),
    ##   party = col_character(),
    ##   returned_ballots = col_number(),
    ##   freq_distribution = col_double(),
    ##   requested_ballots = col_number(),
    ##   return_rate = col_double()
    ## )

``` r
plot_return_rate = 
  returnrate_df %>%
  ggplot(aes(x =return_rate, y = state, fill = party)) +
  geom_bar(stat="identity", position=position_dodge()) +
  theme(axis.text.x = element_text(angle = 90, vjust = 0.5, hjust = 1)) +
  labs(
  title = "The return rate of mail-in ballots for each party by state",
  x = "Return rate of mail-in ballots",
  y = "State",
  caption = "Data from 'https://electproject.github.io/Early-Vote-2020G/'"
  )
```

``` r
knitr::opts_chunk$set(
  fig.width = 6,
  fig.asp = .6,
  out.width = "90%"
)
```
