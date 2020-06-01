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

## Formatted data

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
