# Cleaning data in R

[Wikipedia's entry on dirty data](https://en.wikipedia.org/wiki/Dirty_data) defines it as:

> “Dirty data, also known as rogue data, are inaccurate, incomplete or inconsistent data, especially in a computer system or database.”

We can also add data that is **inconvenient or incompatible** - areas where R, and the `tidyverse` in particular, come in very useful.

This folder contains notebooks with some techniques in R for working with dirty data.

* [Cleaning and reshaping data on firefighters](https://github.com/paulbradshaw/cleaning/blob/master/cleaning_r/reshaping_fire.md)
* [Cleaning health data including splitting columns](https://github.com/paulbradshaw/cleaning/blob/master/cleaning_r/import_cleaning.md)
* [Reshaping wide to long](https://github.com/paulbradshaw/cleaning/blob/master/cleaning_r/police_reshape_long.md): police misconduct data
* [Reshaping long to wide and filtering data](https://github.com/paulbradshaw/cleaning/blob/master/cleaning_r/reshape.md) and calculating year on year changes and percentages

Some recommended resources for using R to clean data include:

* Emily Halford's [guide to cleaning with the Janitor package](https://towardsdatascience.com/cleaning-and-exploring-data-with-the-janitor-package-ee4a3edf085e)
* Matt Waite's Gitbook on data journalism, which [covers cleaning in chapter 10](http://mattwaite.github.io/datajournalism/data-cleaning-part-i-data-smells.html)...
* ...[Janitor in chapter 11](http://mattwaite.github.io/datajournalism/data-cleaning-part-ii-janitor.html) and
* ...[Refinr, Open Refine in R in chapter 12](http://mattwaite.github.io/datajournalism/data-cleaning-part-iii-open-refine.html)
