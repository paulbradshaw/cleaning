# Reshaping fire data in R

This notebook details the process of cleaning and reshaping data on firefighters. 

First, download the data from the [fire statistics data tables ](https://www.gov.uk/government/statistical-data-sets/fire-statistics-data-tables#full-publication-update-history) - specifically [FIRE1121: Staff joining fire authorities, by fire and rescue authority, ethnicity and role](https://assets.publishing.service.gov.uk/media/652d0e41697260000dccf852/fire-statistics-data-tables-fire1121-191023.xlsx).

Then move that file into the same folder as your R project where you are writing your code.

## Import packages

We start by importing the packages that we will need to work with this data: 

* The `tidyverse` is actually a group of packages, largely related to cleaning data.
* And `readxl` is a package with functions for importing Excel files.


```{r}
library(tidyverse)
library(readxl)
```

## Import data - first attempt

Now we use a function from `readxl` called `read_excel()` - this just needs one ingredient: the address of the Excel file. 

As we've put the file in the same location as this notebook, the address is just the name of the file. 

```{r}
#import a file with the specified name
firedf <- readxl::read_excel("fire-statistics-data-tables-fire1121-191023.xlsx")
#show the results
print(firedf)
```
## Importing a specific sheet

We can see it hasn't imported the data we want. This is because the spreadsheet - or *workbook* - has many sheets, and the `read_excel()` function will default to the first sheet. 

To stop it doing that, we need to specify that we want another sheet. 

We can do that by adding a second ingredient to specify the name or number of the sheet we want: `sheet =`

```{r}
#import a file with the specified name, and the specified sheet
firedf <- readxl::read_excel("fire-statistics-data-tables-fire1121-191023.xlsx",
                             sheet = "FIRE1121")
#show it
print(firedf)
```

Note: we could also use `sheet = 4` to indicate we want the fourth sheet. Sometimes that's going to be easier; in workbooks with lots of sheets it may be easier to specify the name instead of counting them.

## Importing while specifying a row for our headers

Now we have the right sheet, but the headers are wrong. It's taken the first row as the header row - but in the spreadsheet the headers actually start in row 4 and 5. 

Let's add another ingredient to specify we don't want it to start from the first row: `skip =`

We are going to ask it to skip *four* rows of data and start with the headings in row 5 instead (the headings in row 4 we will deal with separately).

```{r}
#import a file with the specified name, and the specified sheet, skipping the first 4 rows
firedf <- readxl::read_excel("fire-statistics-data-tables-fire1121-191023.xlsx",
                             sheet = "FIRE1121",
                             skip = 4)
#show it
print(firedf)
```



## Importing while limiting the columns (and excluding footers)

That looks better. We are going to focus our analysis on 'Wholetime firefighters', which is covered in the columns B-I. 

To limit the range of cells imported, we can add another ingredient: `range =`

This takes a cell range just as might use in a spreadsheet formula. In this case we are going to specify cells A5 to I56, or `A5:I56`. That will also solve another problem with the data: over a dozen rows of notes below the table (look at the bottom of the current dataframe to see these).

As with the other ingredients, `A5:I56` needs to be put in quotes.

```{r}
#import a file with the specified name, and the specified sheet, and cell range
firedf <- readxl::read_excel("fire-statistics-data-tables-fire1121-191023.xlsx",
                             sheet = "FIRE1121",
                             range = "A5:I56")
#show it
print(firedf)
```

Note: now that we've specified a cell range, we don't need to use the `skip=` parameter, although it won't affect the results.

## Checking the results

We can use R's basic `summary()` function to get an overview of the data and check whether it's treating columns as text or numeric.

```{r}
#generate a summary of the data frame
summary(firedf)
```

## Convert column types from text to numeric

The last two columns are being treated as **character vectors**. We actually want them to be treated as numbers.

One way to do this is to convert the particular columns from a character vector to a numeric vector with the function `as.numeric()`.

Once converted, it can be assigned back to the column to overwrite it.

Here's the code doing that.

```{r}
#convert the column to numeric, and assign to the column
firedf$`% from an ethnic minority4` <- as.numeric(firedf$`% from an ethnic minority4`)
#repeat with the other column
firedf$`% not stated` <- as.numeric(firedf$`% not stated`)
#check the summary to see it's now showing numerical stats
summary(firedf)
```

## Alternative: converting column types on import

Another way to  is to use the `col_types=` parameter when importing. 

This allows us to provide a vector of data types (e.g. `"text"`, `"numeric"`, and so on) - one for each column. 

Because this data has nine columns, we need a vector of nine items. It would look like this:

`c("text","numeric","numeric","numeric","numeric","numeric","numeric","numeric","numeric")`

That's a lot of typing to do, so you might just want to copy `,"numeric"` the first time you type it and paste that seven times to create the last seven items in the vector. 

Here's what that looks like in the code

```{r}
#import a file with the specified name, and the specified sheet, and specified range
#specify the column types
firedf <- readxl::read_excel("fire-statistics-data-tables-fire1121-191023.xlsx",
                             sheet = "FIRE1121",
                             range = "A5:I56",
                             col_types = c("text","numeric","numeric","numeric","numeric","numeric","numeric","numeric","numeric"))
#show a summary
summary(firedf)
```

## Rename columns

Another thing we might want to clean is the column headings. To show - or change - these you can use the `colnames()` function with the name of a data frame:

```{r}
#show the column names of the specified data frame
colnames(firedf)
```

This is a **vector**. You can access individual items within a vector by adding square brackets after the name of the vector with a number indicating the position of the item you want (i.e. first, second, third etc.). For example, to access the first item in a vector you would write:

```{r}
#show the first item in the (vector of) column names of firedf
colnames(firedf)[1]
```

That number - indicating a position - is called an **index**. 

R uses a **1-based index**, meaning it starts counting from 1. However, it's worth mentioning that many programming languages use a **zero-based-index**, meaning they start counting from zero (so the first position is indicated by 0, the second by 1 and so on).

We can use more than one index to call more than one item. Using two indices with a colon between will call all the items *between those two positions*, like so:

```{r}
#show the items from 2nd to 9th position in the (vector of) column names of firedf
colnames(firedf)[2:9]
```

Not only can we call an item this way, we can **change** an item by accessing it this way, too, like so:

```{r}
#change the name of the first item in the column names
colnames(firedf)[1] <- "area"
#print the column names now
print(colnames(firedf))
```

If you check the data frame now you'll see the new column name is there.

## Reshaping wide to long

Now we have the data but really we don't want each ethnicity to be in its own column. Following **tidy data principles** ("each variable must have its own column") we want a column that says 'ethnicity' - because ethnicity is a variable with multiple values.

That will make it easier to analyse the data in different ways.

This is quite a common challenge in data cleaning (another example would be data with a column for each year, where we would rather have a column called 'year'). 

Essentially we are probably looking at the results of a pivot table, and we want to reshape the data to something closer to its original state before being pivoted.

When we reshape the data in this way, we should end up with something that has fewer columns ("ethnicity" instead of eight different columns) but more rows - it will be longer.

The `tidyverse` function to do that is called `pivot_longer()`. Here's the code to use that to reshape our data:

```{r}
#reshape from wide to long - reshape columns 2-9
longversion <- pivot_longer(data = firedf, 
             cols = colnames(firedf)[2:9],
             names_to = "ethnicity",
             values_to = "value")
```

There are four ingredients here:

* `data =` specifies the dataframe we want to reshape
* `cols =` specifies the columns we want to convert - the names of the column will go in one new column; the corresponding values will go in a second new column. Rather than type a long list of column names, we use `colnames()` and those indices from earlier to generate the list we need.
* `names_to =` specifies the name we want to give to the new column which will store what was the column name. In our case the column names contain data about the ethnicity, so that's what we've called it
* `values_to =` specifies the name we want to give to the new column which will store the values that were associated with each of the old columns. We have just called it 'value' because we have a mix of values here (counts and percentages)

Another way of specifying columns is shown below. In this code we use an exclamation mark to indicate 'everything but' a column. So `!area` means all the columns apart from the one called `area`.

```{r}
#reshape from wide to long - reshape all but the 'area' column
longversion <- pivot_longer(data = firedf, 
             cols = !area,
             names_to = "ethnicity",
             values_to = "value")
#show results
longversion
```

### What happens if you don't reshape so many columns?

It's worth showing you what would happen if we reshaped fewer columns - for example, what if we only wanted to reshape the columns that gave a specific count and not the ones that give percentages.

Below we do that, by changing the columns from those in the range 2:9 to 2:7.

We also change the name of the new values column, because we can now say it's a count.

```{r}
#reshape from wide to long - reshape columns 2-9
longversion <- pivot_longer(data = firedf, 
             cols = colnames(firedf)[2:7],
             names_to = "ethnicity",
             values_to = "count")
#show result
longversion
```
This time, the two final columns remain, but their values are repeated for each row that they relate to. 

In fact, all that's happening here is the same as with the 'area' column: any column that we don't reshape just has its value copied so we are able to see the value that relates to the particular area.

## Export the results

We could now export the results. We use the `tidyverse` function `write_csv()` - note the underscore: this is slightly [different to the base R function `write.csv()`](https://www.rdocumentation.org/packages/readr/versions/0.1.0/topics/write_csv): it's faster and doesn't create row names.

You will find the resulting CSV file in the same location as your project.

```{r}
#export a CSV
write_csv(longversion,"longversion.csv")
```

## Add more data

Now although this process is useful in demonstrating a number of cleaning processes in R, you might be a bit underwhelmed by the resulting data. Although you can now create pivot tables that you couldn't create with the original data, the results of those pivot tables will look very similar to the original data you started with. 

But it does lay the foundations for us to do something a bit more powerful: adding the other parts of the original data.

Let's start by importing another part of the same spreadsheet: the part that relates to on-call firefighters.

We can reuse the code from earlier, just changing the name of the dataframe, and the `range=` argument to a different range of cells:

```{r}
#import a file with the specified name, and the specified sheet, and cell range
firedfoncall <- readxl::read_excel("fire-statistics-data-tables-fire1121-191023.xlsx",
                             sheet = "FIRE1121",
                             range = "K5:R56")
#show it
print(firedfoncall)
```

Here we hit a limitation of the `read_excel()` function: we cannot specify multiple ranges so we haven't added the area names in column A.

But we can import that separately, and then merge the two.

```{r}
#import a file with the specified name, and the specified sheet, and cell range
firedfareas <- readxl::read_excel("fire-statistics-data-tables-fire1121-191023.xlsx",
                             sheet = "FIRE1121",
                             range = "A5:A56")
#show it
print(firedfareas)
```

## Merging two frames horizontally

To merge two dataframes horizontally (where they have the same number of rows) we use the base R function `cbind`:

```{r}
#combine the two dataframes horizontally and overwrite one with it
firedfoncall <- cbind(firedfareas,firedfoncall)
firedfoncall
```
## Reshaping the new data

Now we can repeat the reshape with this new data, making sure that we rename the first column again.

```{r}
#change the name of the first item in the column names
colnames(firedfoncall)[1] <- "area"
#print the column names now
print(colnames(firedfoncall))
```

Note that in the code below we've changed the name of the new data frame being created (we don't want to overwrite the other one) and the data frame being used to create it.

```{r}
#reshape from wide to long - reshape columns 2-9
longversiononcall <- pivot_longer(data = firedfoncall, 
             cols = colnames(firedfoncall)[2:7],
             names_to = "ethnicity",
             values_to = "count")
```

## Store the category information in a new column

We can now merge those two data frames - only this time one on top of the other, as they both have the same *columns*

But before we do that, we need to include the information that's embedded in the heading above the data. Remember that one set of data relates to on-call firefighters, and the other to 'wholetime' firefighters - but that information isn't in the data itself.

We can add it by creating a new column in each dataset that contains that variable.

To create a new column we just need to name the data frame, followed by a `$` sign, and then a name for the new column (no spaces). We assign a value to this column with the `<-` operator, and then the value we want to assign (or some code generating it) is added to the right of that operator, like this:

```{r}
#create a new column filled with the given string
longversion$category <- "wholetime firefighters"
#repeat for the other data frame
longversiononcall$category <- "on call firefighters"
```

It's important that the column in each data frame has the same name, so that they will merge neatly in the next step.

## Merge the two data frames vertically

To merge the data frames vertically we use the `rbind()` function:

```{r}
#merge the two dataframes on top of each other, and assign to a new data frame variable
combined_df <- rbind(longversion,longversiononcall)
```


## Export the results

Now we can export the new dataframe

```{r}
#export a CSV
write_csv(combined_df,"firefighter_reshaped.csv")
```

## Repeat for other tables (if you want)

This process can now be repeated for as many ranges as you need. You could also use this to combine different years - making sure you add a column called 'year' to differentiate between data from different periods.
