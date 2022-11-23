# Dealing with dirty data problems (and using Open Refine)

This repo contains resources for dealing with data problems. In particular, the tool Open Refine.

## Data cleaning tools

* [Open Refine](http://openrefine.org/download.html) 
* [Crosswalker](https://crosswalker.washingtonpost.com) - allows you to match datasets where the matching columns may not match entirely
* [Dataproofer](http://dataproofer.org/)
* [Gender API](https://gender-api.com) and [Genderize.io](https://Genderize.io) (also available as [an R package](https://cran.r-project.org/web/packages/genderizeR/index.html)) can be used to classify names by gender
* [Parserator](https://parserator.datamade.us/) can be used to split addresses
* The [tidyverse](https://www.tidyverse.org/) in R: a collection of libraries for making data 'tidy'. Also [janitor](https://towardsdatascience.com/cleaning-and-exploring-data-with-the-janitor-package-ee4a3edf085e)
* Python has [pandas](https://pandas.pydata.org/), and [pyjanitor](https://github.com/pyjanitor-devs/pyjanitor)
* [Workbench](https://workbenchdata.com/) (no longer supported but can be run using Docker) - includes much of Refine's functionality including clustering
* [Data Wrangler](http://vis.stanford.edu/wrangler/) (no longer supported)
* I keep a [list of data cleaning tools on Pinboard](https://pinboard.in/u:paulbradshaw/t:cleaning+tools)

## Typical data problems

* Numbers/dates treated as strings (often because of currency or percentage signs, or even spaces - try find and replace)
* Strings treated as numbers: e.g. company ‘numbers’, phone numbers and codes often have leading zeroes removed when they are an integral part of the code.
* [Numbers and units combined in sentences structures](https://github.com/paulbradshaw/cleaning/blob/master/sentencestonumbers.md)
* Combined data (addresses)
* Different data in one column (country, region and authority, for example, with spaces or formatting used to indicate the difference)
* Variant spellings
* Inconsistently entered info (e.g. £5k vs £5,000)
* Different terms for same thing
* Mistypings - missing decimals etc.
* Merged cells
* Empty rows
* Headings across multiple rows
* Converted PDFs
* Missing information
* Duplicate information
* Format
* Need to extract information - e.g. first name/surname; street name/region; year/month
* Need to classify information - e.g. male vs female name

I keep a series of bookmarked materials on cleaning using Pinboard at https://pinboard.in/u:paulbradshaw/t:cleaning


## Examples of dirty data

See the [dirtydata](https://github.com/paulbradshaw/cleaning/tree/master/dirtydata) folder in this repo for examples of dirty data.

[This sample dirty dataset can be used for basic data cleaning in Open Refine](https://docs.google.com/spreadsheets/d/1CDWBeqpUTBd1TkmDz_M6UGRWdHgU7LOcoiGRTvIttKA/edit?usp=sharing#gid=0)

[The European Investment Bank database](http://www.eib.org/en/projects/loan/list/index.htm) can be downloaded (look for *Export to Excel* near the bottom) and provides a useful example of data where dates are formatted as strings.

I also bookmark examples of dirty data at https://pinboard.in/u:paulbradshaw/t:dirtydata

For working with XML files try the ones that can be [downloaded from the Food Standards Agency API page](http://ratings.food.gov.uk/open-data/en-GB)

For JSON files try [petition.parliament.uk](https://petition.parliament.uk/) - go to any petition and look for the JSON link at the bottom of the page.

## Tutorials: Open Refine

A [series of introductory guides to Open Refine can be found in the GitHub repo for one of my modules at Birmingham City University here](https://github.com/paulbradshaw/MED7369-Specialist-Investigative-Journalism/tree/master/cleaning)

* [Open Refine's 'common transforms' features for basic cleaning](https://onlinejournalismblog.com/2011/07/05/cleaning-data-using-google-refine-a-quick-guide-2/)
* [How to: clean up spreadsheet headings that run across multiple rows using Open Refine](https://onlinejournalismblog.com/2013/11/13/how-to-clean-up-spreadsheet-headings-that-run-across-multiple-rows-using-open-refine/)
* [Converting JSON or XML into spreadsheets using Open Refine](https://onlinejournalismblog.com/2015/10/21/how-to-convert-xml-or-json-into-spreadsheets-using-open-refine/)
* Tony Hirst explains [Data Shaping in Google Refine – Generating New Rows from Multiple Values in a Single Column](https://onlinejournalismblog.com/2012/07/30/data-shaping-in-google-refine-generating-new-rows-from-multiple-values-in-a-single-column/)
* I've written about [Scraping data with Google Refine](https://onlinejournalismblog.com/2012/01/13/sftw-scraping-data-with-google-refine/)
* Former MA Online Journalism student Ion Mates explains how he used some of Refine's more advanced functionality, and regex, in his post on [cleaning a converted PDF](https://onlinejournalismblog.com/2015/04/07/how-to-clean-a-converted-pdf-using-open-refine/)
* Former MA Online Journalism student Cristian Giuletti explains [How to: combine multiple rows in a dataset where text is split across them](https://onlinejournalismblog.com/2014/05/30/how-to-combine-multiple-rows-in-a-dataset-where-text-is-split-across-them-open-refine/)
* More at https://onlinejournalismblog.com/tag/google-refine/page/2/

## Tutorials: Cleaning in spreadsheets

* [Grabbing or checking the first, middle or last part of a piece of information: RIGHT, LEFT and MID](https://github.com/paulbradshaw/cleaning/blob/master/rightleft.md)
* [What day did that date fall on? Which year was the worst? Extracting days, months and years from full dates](https://github.com/paulbradshaw/cleaning/blob/master/dates.md)
* [Converting sentences to numbers using a spreadsheet](https://github.com/paulbradshaw/cleaning/blob/master/sentencestonumbers.md)

## Tutorials: Cleaning in R

* [Cleaning data while importing](https://github.com/paulbradshaw/cleaning/blob/master/cleaning_r/import_cleaning.md)
* [Reshaping wide to long](https://github.com/paulbradshaw/cleaning/blob/master/cleaning_r/police_reshape_long.md): police misconduct data
* [Reshaping long to wide and filtering data](https://github.com/paulbradshaw/cleaning/blob/master/cleaning_r/reshape.md) and calculating year on year changes and percentages

