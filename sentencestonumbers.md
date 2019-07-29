# How to extract numbers from sentences written as phrases

Sometimes you need to deal with numerical data presented in a textual form where units and amounts are combined and need to be extracted separately. 

One [story on unduly lenient sentences](https://www.bbc.co.uk/news/uk-47879288) illustrates this particularly well. The data detailed sentences in a variety of phrases which identified the number of years, months, weeks and even days that the sentence was composed of. 

Here’s what the data looks like:

| Original Sentence | Revised Sentence |
|---|---|
| 9 years' imprisonment | 11 years and 5 months' imprisonment |
| 2 years' imprisonment suspended for 2 years | Sentence Unchanged |
| 12 years' imprisonment | 14 years' imprisonment |
| 22 months' imprisonment | Sentence Unchanged |

(You can [find the data and other background in a GitHub repo here](https://github.com/BBC-Data-Unit/unduly-lenient-sentences).)

## Break down the steps


The first thing to do in any situation like this is break down the task into its constituent problems/challenges. Well, we need to:


* Identify *where* the number of years is stated
* Extract that number of years
* Identify *where* the number of months is stated
* Extract that number of months
* Identify other context like ‘suspended’, ‘minimum’ etc.
* Convert years and months to a number
* Convert years and months to a common measure (total months)

## Identify where the years/months are detailed: using `SEARCH`


The function `SEARCH` will tell you where the first mention of a word appears. It’s case-insensitive:

`=SEARCH("year",G14)`

*Translation: Search for where year appears in `I2`*

This (written in cell I2) returns a position, e.g. 4. If it doesn’t find it, it returns `#VALUE`

We assume that the number of years appears 3 positions *before* that (one space, plus two digits). So we subtract 3 to get the position of the *number* of years.

`=search("year",G14)-3`

## Extract the number of years/months (and correct for problems)

If it’s a single figure, we should get just the space before it, which we will deal with below. But if it’s at the start of a line, we won’t, and will get an error instead.

So we need a new column (L) to correct for that:

`=IF(I14=0,1,I14)`

Now nest that within a `MID` function to extract the numbers at that position:

`=MID(H14, IF(I14=0,1,I14),2)`

This grabs characters from that position, and continues grabbing for 2 characters.

However, this might also grab spaces if only one character is a digit.

## Handling an unnecessary space


To correct that unnecessary space, and make sure the result is formatted as a number, nest this again in an `INT` function (which turns a value into an integer):

`=INT(MID(I2, IF(K2=0,1,K2),2))`

We could use the `TRIM` function, which removes trailing and leading white space, but this would not change the *type* of data that the space surrounds.


Note that this process involves a lot of *trial and error*: our initial formula works for most cells, but we then focus on *error handling* with the cells where it does not.


You can adapt and repeat the above two processes for months, weeks and days - and also for the revised years, months and so on - to get the numbers you need.

## Converting to a common measure


Once you have columns extracting the numbers of years, months, weeks and days you need to be able to convert them to a common measure in order to perform any calculations. How much longer, for example, is 1 year and 2 months, than 0 years and 5 months?


The simple way to do this is to **convert all measures to the lowest common denominator**.

In this case that would be *days*, so:


* Multiply the years figures by 365 to get days (we could use 365.25 to account for leap years, but it's not relevant)


* Multiply the months by 31 (again, we don't need to use a decimal to account for the fact that month lengths vary - we only need a consistent figure that allows us to calculate differences)


* Multiply the weeks by 7


Once this is done, we can add up all those figures for each sentence to get a total number of days for that sentence.


Repeat for the revised sentence and you now have two figures that you can compare to get a new figure: the **difference** between the two sentences.


That difference can now be averaged (for example, the average change in sentence by offence), and converted accordingly (years, months and weeks) if needed.


## Manual cleaning: identifying unusual words

It’s worth adding a column which just measures the length of the description cell, so you can sort it and pick out outliers:

`=LEN(I2)`

The longest cell on that basis is this one:

> *“4 years and 6 months’ imprisonment with a licence extension of 2 years and 6 months"*

These unusual entries may contain more than one mention of the word being searched for ("years") or extra caveats which are important to factor in.


You decide to add a column checking for those key words:

`=COUNTIF(I2,"*licence*")`

This will count 1 if ‘licence’ is in that cell, or 0 if it is not. The asterisks are wildcards which mean ‘any or no characters’.

You can use the same function to check whether a sentence mentions years or months:

`=COUNTIF(I2,"*year*")`

A `0` value should match up with a `#VALUE` error in your earlier formula. You can use filters to check that they all do, and investigate any that don’t. This may lead you to extra cleaning steps, or in most cases you might decide it is quicker to manually correct or clarify those entries.


## Sometimes hard work ends up left out of the story

Despite all this work to extract the numbers from the data, in the end it was decided to leave this dimension out of the eventual story, as it became clear that the story was going to focus on the proportion of requests which were not eligible for review.

It's important to mention this because often in journalism — not just data journalism — you have to be prepared to leave out material because the focus of your story has changed, regardless of the work that went into it. Ultimately it doesn't matter how much work something involved: if it's not central to the story, then it shouldn't be there.
