# Cleaning data with tidyverse

The book [R for Data Science](http://r4ds.had.co.nz/) has a [tidy data section](http://r4ds.had.co.nz/tidy-data.html) which outlines some key techniques for cleaning data using the `tidyverse` collection of packages.

Let's start by installing the tidyverse collection of packages, and activating it in our library.

```{r}
install.packages("tidyverse")
library(tidyverse)
```

And a few other packages that contain data we want to use:

```{r}
install.packages(c("nycflights13", "gapminder", "Lahman"))
```


## Story based inquiry, or hypothesis generation?

Hadley Wickham conveniently mirrors the division within journalism between hypothesis-driven investigation, and using data to generate leads:

> "It’s possible to divide data analysis into two camps: hypothesis generation and hypothesis confirmation (sometimes called confirmatory analysis)."

## Importing data

The tidyverse uses `read_csv` and `write_csv` as alternatives to R's own `read.csv` etc. However: "Note that the type information is lost when you save to csv ... you need to recreate the column specification every time you load in."

Alternatives include writing as RDS or feather (using the `feather` package).

[Other useful packages](http://r4ds.had.co.nz/data-import.html#other-types-of-data) include:

* `haven` (SPSS, Stata, and SAS)
* `readxl` (.xls and .xlsx).
* `DBI` "along with a database specific backend (e.g. RMySQL, RSQLite, RPostgreSQL etc) allows you to run SQL queries against a database and return a data frame."
* `jsonlite` for json
* `xml2` for XML. See: https://jennybc.github.io/purrr-tutorial/.
* `rio`

## Cleaning data: `parse_` functions

The various functions beginning `parse_` such as `parse_logical()`, etc. will ensure that a vector is imported and encoded correctly, with non-conforming values encoded as `NA`. 

You can use `guess_parser()` to guess what type of data is in a vector (or column):

```{r}
#Create vector with 1 integer and 2 doubles (floats)
myvec <- c(1, 3.1, 5.6)
#Guess what data is in that
guess_parser(myvec)
#Now parse it as that type of data
myvec <- parse_double(myvec)
#And see the results - note the integer has been converted to a double to make it consistent
myvec
#Remove the vector
rm(myvec)
```

If it cannot convert then it will convert to `NA` values, but also generate a tibble to tell us more.

```{r}
#Create vector with 1 integer and 2 doubles (floats) and 1 character
myvec <- c(1, 3.1, 5.6, "one")
#Guess what data is in that
guess_parser(myvec)
#We are going to ignore that and parse it as double anyway
myvec <- parse_double(myvec)
#And see the results - note that a tibble is also generated to tell us about NA conversions
myvec
#Remove the vector
rm(myvec)
```


The `na=` parameter can specify which characters are encoded as `NA`, e.g. `na="."` will treat decimal places as `NA` values. 

Equally, the `locale=` parameter can help specify what language and conventions are being used in describing numbers, dates, etc. For example in European data where commas are used to indicate decimal places you can add `locale = locale(decimal_mark = ",")` to ensure the parsing understands this.

### Times and dates

You can specify date formats using `%d` etc. as follows:

`parse_date("01/02/15", "%d/%m/%y")`

4-digit years are represented by `%Y`.

Months-as-words can also be understood using `%b` (3-letter-code such as 'Jan') or `%B` (full word). Other languages are supported by adding their code, e.g. `locale = locale("fr")`. Type `date_names_langs()` to see them.

```{r}
date_names_langs()
```

As well as `parse_date()` you can import [the `hms` library](https://cran.r-project.org/web/packages/hms/index.html) and its `parse_time()` function.

### Readr's built-in dirty data

`readr` has a built-in dirty dataset that you can use to test some of these processes:

```{r}
#Import the built-in dirty dataset
challenge <- read_csv(readr_example("challenge.csv"))
```

We can follow the instructions to use `problems()`:

```{r}
problems(challenge)
```

### Columns: the `col_` equivalents of `parse_` functions

We can fix this formatting by specifying `col_double()` in our importing - this is similar to `parse_double()` but for columns when loading data. Note that it is included within the `col_types` parameter of `read_csv()` and we are naming each column:

```{r}
challenge <- read_csv(
  readr_example("challenge.csv"), 
  col_types = cols(
    x = col_double(),
    y = col_character()
  )
)
```

Hadley Freeman says: 

> "I highly recommend always supplying col_types, building up from the print-out provided by readr. This ensures that you have a consistent and reproducible data import script. If you rely on the default guesses and your data changes, readr will continue to read it in. If you want to be really strict, use stop_for_problems(): that will throw an error and stop your script if there are any parsing problems."

Checking the head *and* tail of the data shows that it starts with a bunch of `NA` values but by the end we have a bunch of dates:

```{r}
head(challenge)
tail(challenge)
```
So we need to specify that in importing:

```{r}
challenge <- read_csv(
  readr_example("challenge.csv"), 
  col_types = cols(
    x = col_double(),
    y = col_date()
  )
)
```

Now to check again - note that under the y column it now says *date* and the entries are right-aligned, indicating that the data is fundamentally numeric (character data would be left-aligned).

```{r}
tail(challenge)
```

An alternative is to set all columns to the same type:

```{r}
challenge <- read_csv(
  readr_example("challenge.csv"), 
  col_types = cols(
    .default = col_character()
  )
)
tail(challenge)
```

Note that now both columns are described as `<chr>` underneath their name.

### Using `type_convert` to reformat

Once we've done that we can use `type_convert` to reformat a data frame based on what types of data it thinks is in each:

```{r}
challenge.converted <- type_convert(challenge)
tail(challenge.converted)
```

## Tidy data

![](http://r4ds.had.co.nz/images/tidy-1.png)

Hadley Freeman's [explanation of tidy data](http://r4ds.had.co.nz/tidy-data.html) specifies "three interrelated rules which make a dataset tidy":

* Each variable must have its own column.
* Each observation must have its own row.
* Each value must have its own cell.

> "There’s a general advantage to picking one consistent way of storing data. If you have a consistent data structure, it’s easier to learn the tools that work with it because they have an underlying uniformity ... dplyr, ggplot2, and all the other packages in the tidyverse are designed to work with tidy data."

### When years or categories are used as headings: `gather()`

Often organisations publish data where a *value* is used as a heading. The most common example is when each year's data is stored in a different column, where really we would prefer to have a separate column for *year* and the year itself provided as data.

The `gather()` function is designed to deal with this - it will *gather* the years from two columns (or more) into just one. It has three parameters:

* The columns you want to deal with (those for each year)
* What sort of data those headings represent: this is a variable. "I call that the key", [says Freeman](http://r4ds.had.co.nz/tidy-data.html#gathering), e.g. *year*
* What sort of data is *under* those headings, in the *cells*. "I call that value," says Freeman.

You can call a built-in example, `table4a`:

```{r}
table4a
```
Note that this has 3 rows. Because there are 2 years and we want to add a 'years' column, we should end up with 6 rows. The two columns will be collapsed into one, but we also gain a new column for 'year'

Now to use `gather()` with those parameters: first the names of the columns (because these are numerical, or *[non-syntactic](https://adv-r.hadley.nz/names-values.html)* we have to include the backtick `), then the more specific parameters. Remember that 'year' and 'cases' are arbitrary names:

```{r}
table4a %>%
  gather(`1999`,`2000`, key = "year", value = "cases")
```

Note also that we're using a pipe - `%>%` - which alters the direction of the flow of the code: the table is taken as an ingredient to use in that function. 

This is in contrast to `<-` which assigns the value (or the result of a series of steps) to the right of that symbol to the variable to the left of it. In other words, with `<-` the right hand side is executed first, and then we move to the left to store the results.

Likewise with most functions we start inside the parenthesis to the right of the function name, and whatever happens there is used as the ingredient for the function to the left.

With `%>%` we are taking something to the left of that pipe *first*, and then moving to the right.

This might seem a bit strange, given that `gather()` already has a bunch of ingredients being fed to it in its parentheses, but it can make code clearer by avoiding parentheses nested within parentheses within parentheses...

You can [read more about pipes in R here](https://www.datacamp.com/community/tutorials/pipe-r-tutorial)

Let's store the results in a couple of new variables:

```{r}
tidy4a <- table4a %>%
  gather(`1999`,`2000`,key="year",value="cases")
tidy4b <- table4b %>%
  gather(`1999`,`2000`,key="year",value="population")
```


### When a numerical column can refer to different categories: `spread()`

Here's another example of dirty data:

```{r}
table2
```

Note that *count* can refer to either cases or population, which isn't ideal. This is what `spread()` is designed for - it will *spread* the categories (two in this case) into their own columns, and move the values into those. It has two variables: the column containing the category (`key`); and the column containing the `value`. This time the number of rows is *reduced*: with 12 rows showing 2 categories, it becomes 6 rows with a column for each category:

```{r}
table2 %>%
  spread(key= type, value = count)
```

