# Reshaping data on police misconduct

This notebook explains some code used for a [BBC story on police misconduct](https://www.bbc.co.uk/news/uk-59594712). The [GitHub repo for the story can be found here](https://github.com/BBC-Data-Unit/police_misconduct).

Earlier in the process, data had been scraped from the IOPC website. 

Scraping the data involved a tricky challenge: each report included tags that identified the forces involved, and the category of the report (e.g. Corruption). To store this I created a column for each and showed 'true' or 'false'.

But we now need to clean that data by getting it into a shape that makes it possible to analyse. That involves reshaping the data.

## Import the data

The data can be found in the same folder as this notebook, or [downloaded from the story's GitHub repo](https://github.com/BBC-Data-Unit/police_misconduct/blob/main/rfiles/recs30june_wforces_refs.xlsx). 

```{r import}
#import data
forces <- rio::import('recs30june_wforces_refs.xlsx', sheet = 1)
names(forces)
```

## Reshape the data

You can see there are a lot of columns in this data because there were so many tags. It is 'wide' and we want it to be 'narrow'.

We can do that using the `tidyr` package's `gather()` function. This needs 4 pieces of information:

* The name of the dataframe
* What we want to call the new column that's going to be created to contain the old column names (the names of the forces)
* What we want to call the column that's going to be created to contain the value in those columns (true or false, in this case)
* The range of columns we want to be converted from wide to long

```{r gather wide to long}
#reshape from wide to long, gathering all forces into one column
forces_long <- tidyr::gather(forces, 
                             force, 
                             tf, 
                             `Avon and Somerset Constabulary`:`Wiltshire Constabulary`)

#show the column names of the new reshaped data frame
names(forces_long)
```

You can see we now have 33 columns where we had 74 before.

Let's see what's in the two new columns

```{r table tf}
#tally of true and false
table(forces_long$tf)
```


```{r table force}
#tally of values in the force column
table(forces_long$force)
```

## Filter out the 'FALSE' forces

This new data frame retains all the instances where a force is marked as 'FALSE', i.e. *not* tagged in a report. 

We don't need those, so we can use the `subset()` function to create a filtered subset of just those rows where the value in that column is TRUE.

```{r filter false forces}
#filter out the FALSE values 
forces_long <- subset(forces_long, forces_long$tf == TRUE)
```

## Reshape category columns wide to long

The same process can be applied to the category columns. 

```{r gather category columns and filter}
#repeat for categories
forces_long_wcats <- tidyr::gather(forces_long, 
                                   categ, 
                                   tfcat, 
                                   `Child sexual abuse`:`Welfare and vulnerable people`)
#and filter out FALSE again
forces_long_wcats <- subset(forces_long_wcats, forces_long_wcats$tfcat == TRUE)

#show column names
names(forces_long_wcats)
```

We now have 25 columns instead of 33, and 40 fewer rows.

## Export the results

That's ready to export. 

```{r export}

#export
write.csv(forces_long_wcats,'forces_long.csv')
```

