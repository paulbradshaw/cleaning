# Reshaping data: analysing the impact of COVID on special educational needs

Go to [National statistics on special educational needs in England](https://explore-education-statistics.service.gov.uk/find-statistics/special-educational-needs-in-england) and click 'Download all data', then unzip, it's called sen_ncyear_.csv - it has a column for each year group (Q is nc_year_1) and a column (M) for 'primary_need' - so you can filter that down to 'Speech, Language and Communications needs'

## Install the packages

We will need the `tidyverse` to do the reshaping; and `downloader` to download data files from the web.

```{r install packages}
#install the tidyverse package
#install.packages('tidyverse')
library('tidyverse')
#install the downloader package: https://cran.r-project.org/web/packages/downloader/index.html
install.packages("downloader")
library(downloader)
```

## Import the zip file, extract the data

The data is published in a zip file that can be accessed from the 'Download all data' button at https://explore-education-statistics.service.gov.uk/find-statistics/special-educational-needs-in-england/2020-21#dataDownloads-1

```{r download zip files}
#store the URL for the data zip file which is found by right-clicking on 
#'Download all data' at https://explore-education-statistics.service.gov.uk/find-statistics/special-educational-needs-in-england/2020-21#dataDownloads-1
# This is the 21/22 data
zipurl <- "https://content.explore-education-statistics.service.gov.uk/api/releases/daf8d64d-ce21-4f21-c28c-08da3ee963bf/files"

#download the zip file from the url
downloader::download(zipurl, dest="datasets.zip", mode="wb") 
#unzip it 
unzip ("datasets.zip", exdir = "./")
```

Once downloaded and unzipped we can then import the CSV.

```{r import CSV}
#import the data extracted from the zip
data22 <- "data/sen_ncyear_.csv"
data <- readr::read_csv(data22)


```


## Filter the data to what we need

This is a very large dataframe, and we don't need all of it. Firstly, we're specifically interested in the figures on speech and language.

```{r filter to one category of SEN}
#filter to where column values match specified string
slcdf <- filter(data, primary_need == "Speech, Language and Communications needs")
print(slcdf)
```

```{r filter primary only}
#filter to where column values match specified string
slcdf <- filter(slcdf, phase_type_grouping == "State-funded primary")
print(slcdf)
```

We also don't need the sub-categories - just the total of those needing support.

```{r show categories of status}
#show the unique values and their frequency
table(slcdf['pupil_sen_status'])
```

```{r filter totals only}
#filter to where column values match specified string
slcdf_filter3 <- filter(slcdf, pupil_sen_status == "Total")
print(slcdf_filter3)
```

We also only want to look at data for local authorities. 

```{r filter to LA}
#show the unique values and their frequency
print(table(slcdf_filter3['geographic_level']))

#filter to where column values match specified string
slcdf_filter4 <- filter(slcdf_filter3, geographic_level == "Local authority")
print(slcdf_filter4)
```

## Remove data frames no longer needed

If we're happy with the filtered data frame we can now delete the intermediate data frames using `rm()`

```{r rm data frames}
rm(data, slcdf, slcdf_filter3)
```


## Identify columns we don't need

We also have a number of columns we don't need.

```{r show columns}
#show the columns
names(slcdf_filter4)
```

A few columns only have one value:

```{r show unique values in selected cols}
#count the unique values in the time_identifier column
print(table(slcdf_filter4['time_identifier']))
#repeat for the geographic level column
print(table(slcdf_filter4['geographic_level']))
#repeat for country_name
print(table(slcdf_filter4['country_name']))
#repeat for country_code
print(table(slcdf_filter4['country_code']))
#repeat for phase_type_grouping
print(table(slcdf_filter4['phase_type_grouping']))
#repeat for pupil_sen_status
print(table(slcdf_filter4['pupil_sen_status']))
```

## Remove columns using `select`

We remove those by using `select()` and a minus before the column we want to exclude.

```{r remove columns}
#remove the specified columns
slcdf_col_filter1 <- select(slcdf_filter4, 
                            -c(time_identifier, 
                               geographic_level,
                               country_name,
                               country_code,
                               phase_type_grouping,
                               pupil_sen_status))
slcdf_col_filter1
```

And we don't need years 7 onwards because that's after primary school.

```{r show column names within ranges}
#show the columns we want to exclude
print(colnames(slcdf_col_filter1)[17:25])
print(colnames(slcdf_col_filter1)[34:42])
```

```{r remove those cols}
#remove the specified column
slcdf_col_filter2 <- select(slcdf_col_filter1, 
                            -colnames(slcdf_col_filter1)[17:25]) %>% 
  select(-colnames(slcdf_col_filter1)[34:42])
#show the column names left
colnames(slcdf_col_filter2)
```

## Pivot by LA and year using `group_by()` and `summarize()`

There are a number of ways to create pivot table-like summaries in R, including [the `pivottabler` package](https://cran.r-project.org/web/packages/pivottabler/vignettes/v00-vignettes.html) and `dplyr`. We'll be using the latter approach, [documented here](https://rstudio-conf-2020.github.io/r-for-excel/pivot-tables.html)

```{r pivot by LA}
#the group_by line specifies the rows of the pivot table
slcdf_col_filter2 %>%
  group_by(la_name) %>% 
  summarize(count_of_la = n()) # this specifies the name and calculation to create the 'values' part
```
### Adding rows

This tells us that there are 7 values for each LA. Those would be the figures for each of the 7 academic years covered by the data. We need to add those as rows so that they are separated out.

We can begin to do this by adding the `time_period` column as a second argument in `group_by()`

```{r group by time period too}
#the group_by line specifies the rows of the pivot table
slcdf_col_filter2 %>%
  group_by(la_name, time_period) %>% 
  summarize(count_of_la = n()) # this specifies the name and calculation to create the 'values' part
```

### Sum, don't count

Now, this just adds another column rather than a column headed with each year, but we can reshape this table to fix that. First, let's change from counting to summing.

```{r pivot with sum}
#the group_by line specifies the rows of the pivot table
slcdf_col_filter2 %>%
  group_by(la_name, time_period) %>% 
  summarize(sum_of_yr1 = sum(nc_year_1)) # now we 'sum' a specific column, 
  #and the resulting column is called 'sum_of_yr1'
```

## Reshape long to wide using `spread()`

We now reshape to make that second column of years into the column headings, so there's only one row per LA, and we can more easily calculate year on year change.

Note that R doesn't like number-only column names, so it puts each one inside the code accent: `

```{r}
#save the previous results
pivot_by_la_yr_yr1 <- slcdf_col_filter2 %>%
  group_by(la_name, time_period) %>% 
  summarize(sum_of_yr1 = sum(nc_year_1)) # now we 'sum' a specific column, 
  #and the resulting column is called 'sum_of_yr1'
#specify we want to convert the time_period column values to column names 
#and insert the values from the sum_of_yr1 column underneath
yr1_wide <- pivot_by_la_yr_yr1 %>% spread(time_period, sum_of_yr1)
#show the results
yr1_wide
```



## Add a grand total

We will want a national change figure, too. So let's add that row.

```{r number of rows}
#check how many rows so we don't overwrite any
nrow(yr1_wide)
```

```{r create total}
#create empty vector to contain totals, 
#we start with a number which will be replaced with the text that will go in the first column
#but for now we need a number so the others aren't stored as strings
totalsvec <- (0)
#loop through the other column names
for (i in colnames(yr1_wide)[seq(2,8)]){
    #print the column name we're working with
    print(i)
    #add all the numbers in that column
    coltotal <- sum(yr1_wide[i][!is.na(yr1_wide[i])])
    print(coltotal)
    #add it to the vector
    totalsvec <- c(totalsvec,coltotal)
}
print(totalsvec)
```

```{r transpose}
#transpose the vector and convert to a dataframe
newdf <- as.data.frame(t(totalsvec))
print(newdf)
#assign the column names to that dataframe
colnames(newdf) <- colnames(yr1_wide)
print(newdf)
#change the first cell to 'total'
newdf[1,1] <- "Total"
print(newdf)
```

```{r bind data frames}
#bind the two dataframes together
yr1_wtotal <- rbind(yr1_wide,newdf)
#and check the last few rows
print(yr1_wtotal[seq(152,155),] )
#overwrite the original dataframe
yr1_wide <- yr1_wtotal
```


## Calculate year on year changes

Now we can add new columns with the year on year changes.

```{r col names}
#remind ourselves of the column names
colnames(yr1_wide)
```


```{r YOY changes}
#subtract 2021 from 2122 to get the change - and store in a new column
yr1_wide['YOY21to22'] <- yr1_wide['202122'] - yr1_wide['202021']
#show the results to check they tally
yr1_wide[, c(1,7,8,9)]
```

### Calculate them as percentages

Now add another column with those values as percentage changes.

```{r YOY percentages}
#divide the change by the older 2021 figure to get the % change - and store in a new column
yr1_wide['YOYperc21to22'] <- yr1_wide['YOY21to22']/yr1_wide['202021']
#show the results to check they tally - the first figure should be around 1% or 0.01 when rounded
yr1_wide[, c(1,7,8,9,10)]
```

## Repeat for other years

Can we codify this process and loop to create the other columns?

```{r loop for other YOY percs}
#loop through the column names from 3rd to 7th position
for (i in seq(3,7)){
    print(i)
    thisyr <- colnames(yr1_wide)[i]
    previousyr <- colnames(yr1_wide)[i-1]
    print(thisyr)
    print(previousyr)
    #subtract the previous year from the current one to get the change - and store 
    YOYchange <- yr1_wide[thisyr] - yr1_wide[previousyr]
    YOYpercChange <- YOYchange/yr1_wide[previousyr]
    #extract the two digits for the later year
    toyr <- substr(thisyr,5,6)
    fromyr <- substr(previousyr,5,6)
    #create column names from these
    colname <- paste0("YOY",fromyr,"to",toyr)
    perccolname <- paste0("YOYperc",fromyr,"to",toyr)
    #create a new column with that name and the values calculated
    yr1_wide[colname] <- YOYchange
    yr1_wide[perccolname] <- YOYpercChange
}
```

## Export results

```{r export}
#export as a CSV
write.csv(yr1_wide, "yr1_analysis.csv")
```

