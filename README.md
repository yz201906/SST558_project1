JSON Vignette
================
Yinzhou Zhu
5/31/2020

  - [About JSON data](#about-json-data)
  - [Packages for reading JSON](#packages-for-reading-json)
  - [JSON example: NHL API](#json-example-nhl-api)
  - [Formatted data](#formatted-data)
  - [References](#references)

## About JSON data

**JavaScript** is the most widely used language that powers interactions
between client devices and server/web applications, where it allows
programmers to implement complex features<sup>1</sup>. As a high-level
language, it is also light-weight<sup>2</sup>.  
JavaScript Object Notation (**JSON**), as its name also implies, is a
type of data that is easily converted from and to **JavaScript**
objects<sup>3</sup>. **JSON** is stored as text string, which makes it
possible to store **JavaScript** objects as text. It is now a very
standard way for data communication over the network, which used to be
primarily with **XML** format that is much heavier in weight.  
As described, **JSON** was built with purpose of being a standardized
transfer language that is easy for human to read and at the same time
easy for machine to parse and execute <sup>4</sup>.  
The flexibility of **JSON** means it’s also suitable for general data
transfer beyond **JavaScript** objects between devices across the web.
This makes it a popular way for accessing data through cloud where
**JSON API** is implemented<sup>5</sup>.

## Packages for reading JSON

### `rjson`, `jsonlite`, `RJSONIO` and `tidyjson`

`rjson`, `jsonlite` and `RJSONIO` are 3 popular R packages used for
working with **JSON** data. Functionality wise, they are all very
similar, where they allow conversion of **R** objects from and to
**JSON** data. The major differences being mainly: usage syntax,
internal implementation methods (such as how data is read etc., thus
affecting speed) and extended features. For example, `jsonlite` is able
to stream to/from JSON file if the data that we are dealing with is
large, which could be an advantage<sup>6</sup>. `tidyjson` on the other
hand seems to be gaining traction. Under the same framework of
`tidyverse`, this package could be particularly useful in streamlining
our pipeline when appropriate<sup>7</sup>.

### Why choosing `jsonlite`?

I have chosen to work with `jsonlite` for the following reasons:  
*. I have gotten most familiar with this package due to course usage.  
*. It has advantage and flexibility for dealing with a large amount of
data as mentioned above.  
\*. The overall consensus I got from reading online is that `jsonlite`
strikes a very good balance between features and performance and is
particulate in format conversions.

## JSON example: NHL API

``` r
api_url <- 'https://records.nhl.com/site/api'
nhl_franchise <- function(){
  franchises <- httr::GET(paste0(api_url, '/franchise'))
  franchises <- httr::content(franchises, 'text')
  franchises <- fromJSON(franchises, flatten = TRUE)
}
nhl_franchise_team_totals <- function(){
  franchises <- httr::GET(paste0(api_url, '/franchise-team-totals'))
  franchises <- httr::content(franchises, 'text')
  franchises <- fromJSON(franchises, flatten = TRUE)
}
nhl_franchise_records <- function(franchise_id, records='season'){
  if (!records%in%c('season', 'goalie', 'skater')) {
    stop("Records can only be 'season', 'goalie' or 'skater'.")
  }
  if (is.na(as.numeric(franchise_id))) {
    stop("Please enter a valid franchasie ID.")
  }
  if (records=='season') {  
    franchises <- httr::GET(paste0(api_url, '/franchise-season-records?cayenneExp=franchiseId=',franchise_id))
  } else if (records=='goalie') {
    franchises <- httr::GET(paste0(api_url, '/franchise-goalie-records?cayenneExp=franchiseId=',franchise_id))
  } else {
    franchises <- httr::GET(paste0(api_url, '/franchise-skater-records?cayenneExp=franchiseId=',franchise_id))
  }
  franchises <- httr::content(franchises, 'text')
  franchises <- fromJSON(franchises, flatten = TRUE)
}
```

## Formatted data

List of all franchises.

``` r
nhl_franchise() %>% head()
```

    ## $data
    ##    id firstSeasonId lastSeasonId mostRecentTeamId teamCommonName teamPlaceName
    ## 1   1      19171918           NA                8      Canadiens      Montréal
    ## 2   2      19171918     19171918               41      Wanderers      Montreal
    ## 3   3      19171918     19341935               45         Eagles     St. Louis
    ## 4   4      19191920     19241925               37         Tigers      Hamilton
    ## 5   5      19171918           NA               10    Maple Leafs       Toronto
    ## 6   6      19241925           NA                6         Bruins        Boston
    ## 7   7      19241925     19371938               43        Maroons      Montreal
    ## 8   8      19251926     19411942               51      Americans      Brooklyn
    ## 9   9      19251926     19301931               39        Quakers  Philadelphia
    ## 10 10      19261927           NA                3        Rangers      New York
    ## 11 11      19261927           NA               16     Blackhawks       Chicago
    ## 12 12      19261927           NA               17      Red Wings       Detroit
    ## 13 13      19671968     19771978               49         Barons     Cleveland
    ## 14 14      19671968           NA               26          Kings   Los Angeles
    ## 15 15      19671968           NA               25          Stars        Dallas
    ## 16 16      19671968           NA                4         Flyers  Philadelphia
    ## 17 17      19671968           NA                5       Penguins    Pittsburgh
    ## 18 18      19671968           NA               19          Blues     St. Louis
    ## 19 19      19701971           NA                7         Sabres       Buffalo
    ## 20 20      19701971           NA               23        Canucks     Vancouver
    ## 21 21      19721973           NA               20         Flames       Calgary
    ## 22 22      19721973           NA                2      Islanders      New York
    ## 23 23      19741975           NA                1         Devils    New Jersey
    ## 24 24      19741975           NA               15       Capitals    Washington
    ## 25 25      19791980           NA               22         Oilers      Edmonton
    ## 26 26      19791980           NA               12     Hurricanes      Carolina
    ## 27 27      19791980           NA               21      Avalanche      Colorado
    ## 28 28      19791980           NA               53        Coyotes       Arizona
    ## 29 29      19911992           NA               28         Sharks      San Jose
    ## 30 30      19921993           NA                9       Senators        Ottawa
    ## 31 31      19921993           NA               14      Lightning     Tampa Bay
    ## 32 32      19931994           NA               24          Ducks       Anaheim
    ## 33 33      19931994           NA               13       Panthers       Florida
    ## 34 34      19981999           NA               18      Predators     Nashville
    ## 35 35      19992000           NA               52           Jets      Winnipeg
    ## 36 36      20002001           NA               29   Blue Jackets      Columbus
    ## 37 37      20002001           NA               30           Wild     Minnesota
    ## 38 38      20172018           NA               54 Golden Knights         Vegas
    ## 
    ## $total
    ## [1] 38

How many times have New York Rangers and Boston Bruins each played?

``` r
totals <- nhl_franchise_team_totals()
nyr_totals <- totals$data %>% filter(franchiseId==6) %>% tally(gamesPlayed, name = "New York Rangers")
bb_totals <- totals$data %>% filter(franchiseId==10) %>% tally(gamesPlayed, name = "Boston Bruins")
row.names(nyr_totals)<-"Total games played"
kable(cbind(nyr_totals, bb_totals))
```

|                    | New York Rangers | Boston Bruins |
| ------------------ | ---------------: | ------------: |
| Total games played |             7221 |          7019 |

## References

**1.**
<https://www.bigcommerce.com/ecommerce-answers/what-javascript-and-why-it-important/>  
**2.** <https://developer.mozilla.org/en-US/docs/Web/JavaScript>  
**3.** <https://www.w3schools.com/js/js_json_intro.asp>  
**4.** <https://www.json.org/json-en.html>  
**5.** <https://nordicapis.com/the-benefits-of-using-json-api/>  
**6.**
<http://anotherpeak.org/blog/tech/2016/03/10/understand_json_3.html>
**7.**
<https://cran.csiro.au/web/packages/tidyjson/vignettes/introduction-to-tidyjson.html>
