# Dealing with dirty data problems and using Open Refine

This repo contains resources for dealing with data problems. In particular, the tool Open Refine.

## Data cleaning tools

* [Download Open Refine](http://openrefine.org/download.html)
* [Mr People](http://people.ericson.net/) - for splitting names
* [Dataproofer](http://dataproofer.org/)
* [Data Wrangler](http://vis.stanford.edu/wrangler/) (no longer supported)
* [Gender API](https://gender-api.com) and [Genderize.io](https://Genderize.io) (also available as [an R package](https://cran.r-project.org/web/packages/genderizeR/index.html)) can be used to classify names by gender
* [Parserator](https://parserator.datamade.us/) can be used to split addresses
* I keep a [list of data cleaning tools on Pinboard](https://pinboard.in/u:paulbradshaw/t:cleaning+tools)

## Typical data problems

* Numbers/dates treated as strings (often because of currency or percentage signs, or even spaces - try find and replace)
* Strings treated as numbers: e.g. company ‘numbers’, phone numbers and codes often have leading zeroes removed when they are an integral part of the code.
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

I also bookmark examples of dirty data at https://pinboard.in/u:paulbradshaw/t:dirtydata

## Tutorials

A [series of introductory guides to Open Refine can be found in the GitHub repo for one of my modules at Birmingham City University here](https://github.com/paulbradshaw/MED7369-Specialist-Investigative-Journalism/tree/master/cleaning)

* [Open Refine's 'common transforms' features for basic cleaning](https://onlinejournalismblog.com/2011/07/05/cleaning-data-using-google-refine-a-quick-guide-2/)
* [How to: clean up spreadsheet headings that run across multiple rows using Open Refine](https://onlinejournalismblog.com/2013/11/13/how-to-clean-up-spreadsheet-headings-that-run-across-multiple-rows-using-open-refine/)
* [Converting JSON or XML into spreadsheets using Open Refine](https://onlinejournalismblog.com/2015/10/21/how-to-convert-xml-or-json-into-spreadsheets-using-open-refine/)
* Tony Hirst explains [Data Shaping in Google Refine – Generating New Rows from Multiple Values in a Single Column](https://onlinejournalismblog.com/2012/07/30/data-shaping-in-google-refine-generating-new-rows-from-multiple-values-in-a-single-column/)
* I've written about [Scraping data with Google Refine](https://onlinejournalismblog.com/2012/01/13/sftw-scraping-data-with-google-refine/)
* Former MA Online Journalism student Ion Mates explains how he used some of Refine's more advanced functionality, and regex, in his post on [cleaning a converted PDF](https://onlinejournalismblog.com/2015/04/07/how-to-clean-a-converted-pdf-using-open-refine/)
* Former MA Online Journalism student Cristian Giuletti explains [How to: combine multiple rows in a dataset where text is split across them](https://onlinejournalismblog.com/2014/05/30/how-to-combine-multiple-rows-in-a-dataset-where-text-is-split-across-them-open-refine/)
* More at https://onlinejournalismblog.com/tag/google-refine/page/2/
