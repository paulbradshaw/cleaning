# Grabbing or checking the first, middle or last part of a piece of information: `RIGHT`, `LEFT` and `MID`

Sometimes you only want to look at part of some data - for example the first or last part of a postcode, zip code, or telephone number. Dates which are stored as text rather than numbers are also prime candidates.

The functions `RIGHT`, `LEFT` and `MID` are tailor-made for those problems. These will, respectively, grab a specified number of characters from the end, beginning, or middle of a cell. 

Used on their own they work particularly well where you always want the same number of characters - but they can also be combined with other functions to grab different amounts of text, as we'll see.

A> ### Splitting names, dates, addresses and other data: text to columns
A>
A> If your data already has a particular character that marks one part from another - such as a space between first name and surname, or slashes between day, month and year - then you can use the menu option **Text to columns...** (normally in the **Data** menu) to split it, rather than needing to use a function. Note that if you use this, the original column will no longer exist, so you might want to duplicate it first.
A>
A> To use this function first make sure that the column you want to split is placed *in the last column of your data*. You may need to select it, cut or copy it, and paste it at the far right of your data to get it there. 
A>
A> This is because when you split the column *it will overwrite anything to the right of it*. 
A>
A> Once the column is in the right place, select it, and then go to **Data > Text to columns...** 
A>
A> ![](images/texttocols.png)
A>
A> The *Text to columns wizard* will now open. The first option it presents you with is whether you are splitting 'Delimited' or 'Fixed width' data. Fixed width data can be split 	on a specified character position (for example, it can split all cells at the fourth character) but here we're talking about *delimited* data: that is, data where a specific character sets the limits of one part, such as the end of the first name or the day. Make sure this is selected and click **Next**.
A>
A> The next step presents you with a number of common characters you can select as your delimiter: *space*, for example (which may separate a first name from a second name), and *comma* (which may separate the first line of an address from a second part, and a third part). If you want to split your data on either of those characters, or *tab*, or *semicolon*, then tick the relevant box and click **Next**. If you want to specify another character - such as a slash - then tick *Other:* and type the character in the box next to it. Note that you cannot enter multiple characters here.
A>
A> In the final step you have some options to format the new columns you are creating. This defaults to 'general' and in most cases that will be fine. However, if one part of your data is a date, you can *select that column* in the preview at the bottom and then tick *Date:* in the area above and to the right of that. You can also, next to *Date:*, select what format the date uses - YMD (Year/Month/Date), MDY (Month/Date/Year) and so on.
A>
A> Note when you change the format that the preview underneath reflects this: the first row describes the format of the column underneath. So you can see at a glance how each column is going to be formatted.
A>
A> If you're happy with the preview then click **Finish** and you should now have at least two columns where you previously only had one.
A>
A> In some cases, you may find moving columns around more hassle than using a `LEFT` function. Perhaps more likely, however, is that you want to retain your original data and use those other functions to extract from it, rather than splitting it up.

## Grabbing characters from the beginning: LEFT

The `LEFT` function has two ingredients: the cell you want to use, and the number of characters you want to grab from the beginning (the left). If you wanted to grab the first two characters of cell A2, for example, you would use the following formula:

`=LEFT(A2,2)`

This function is particularly useful whenever you are working with any sort of code - whether commonly known codes like area codes or postcodes, or internal codes such as institution codes, invoice numbers and cost codes, where different parts refer to different things (just as in a telephone number the first part refers to the area).

If you only wanted the area codes from a list of phone numbers, for example, you could use this to grab them. You could also use this to separate different types of numbers: for example, the first two digits of mobile numbers might always be '07'; the first two digits of international numbers always '00'; and the first two digits of landlines always '01' or '02'.

Dates are worth a particular mention here - as you will get different results depending on whether the date is stored as a number, or as a series of text characters. 

If the date is stored as a number it is easiest to use the functions detailed in the previous chapter on the functions `YEAR`, `MONTH` and `DAY`. 

If it is stored as text, however, the `LEFT`, `RIGHT` and `MID` functions can help you out.

To give you an example: in the series of characters "2012-08-16" *not* stored as a date this formula will grab the first two: `20`. However, if it *is* a real date then it will grab the first two digits of that date *when expressed as a number*: `41` (the full number for that date is 41137).

If you get a result like this you know the date is stored as a number, and you should use `YEAR`, `MONTH` or `DAY` instead.

A> ### Dealing with dates and partial dates stored as text: `DATEVALUE`
A>
A> If you are trying to clean up dates that have been stored as text, take a look at the `DATEVALUE` function. This will convert a date which is stored as a text string, into a date which is stored as a number string.
A>
A> The best way of demonstrating this is by using the `TEXT` function to show you what happens when dates are stored as text.
A> 
A> Type a date in cell A1 - for example `12/12/99`. Excel interprets this as a date and stores it as the number of days since 1900 (35044), but still *formats* the date as `12/12/99`. The date will be aligned right.
A>
A> In cell B1 type the formula `=TEXT(A1,"dd/mm/yyyy")`. This will turn that date into some text which shows the days as two characters, followed by a slash, followed by the month as two characters, followed by another slash, and then the year as four characters. You can tell it's text because it's aligned *left*.
A>
A> Now if you right click on cell B1 and select **Format cells...** you'll notice that changing the format to *General* or *Number* does not change the date in the same way that it would in cell A1.
A>
A> In cell C1, then, you can turn the date-as-text-string *back* into a date-as-number by using the `DATEVALUE` function like so: `=DATEVALUE(B1)`
A>
A> Unless C1 is formatted in some other way, you should see the result as a number: 35044. You can format this as a date by, again, right-clicking and selecting the **Format cells...** option, then choosing *Date*.
A>
A> `DATEVALUE` is particularly good for dealing with **partial dates** - as long as they refer to this year. A text string like `12-Dec` will be interpreted by `DATEVALUE` as the 12th December *this year*, and converted accordingly. 





## Grabbing characters from the end: RIGHT

The `RIGHT` function works in exactly the same way as `LEFT`, with two parameters: the cell you want to use, and the number of characters you wish to grab - this time from the *end* (the right). 

If you wanted to grab the *last four* characters of A2, then, you would use this formula:

`=RIGHT(A2,4)`

Again, with dates which are actually stored as the number of days since 1900 this will grab the final digits of that number. 

What if the number of characters *depends* on the length of the data? Well you can include a calculation to work that out - we'll come onto this below.

## Grabbing characters from the middle: MID

The `MID` function is slightly more complicated, needing three parameters: the cell you want to extract from; *what character position you want to start grabbing from*; and how many characters you want to grab. 

Obviously the starting point for `LEFT` and `RIGHT` is predetermined, but `MID` can start from anywhere - including the beginning (position 1).

*The starting character is included in the results* and the counting. For example, if you want to grab two characters from character position 3, it will grab that third character, and the fourth (two in total).

Remember also that spaces are counted as characters and will be grabbed just as text characters will be.

If cell A1 contained the string `Finding stories in spreadsheets`, then, here are some formulae using `MID` with the results:

`=MID(A1,1,5)` will get you 'Findi' (the five characters starting from position 1)

`=MID(A1,10,5)` will get you 'torie' (the five characters starting from position 10)

`=MID(A1,30,5)` will get you 'ts' - as there are only 31 characters in the string of text it's working with, this grabs the first two from position 30.

A `MID` formula working on this text which specified a starting position of 32 or higher would return nothing, because there are no characters from that position. But it will *not* generate an error.

T> ### When you don't know how many characters from the end: MID as an alternative to RIGHT
T> 
T> Because `MID` can hit the end of your text and not generate an error, the `MID` function can be a useful alternative to `RIGHT` when you do not know exactly how many characters you want to grab at the end of a cell, but you do know what position those characters start *from*.
T> 
T> In these cases, just set the number of characters to a value which you know will always capture *all* characters after your specified point, for example `100`. In practical terms this translates as 'grab all the characters from this point until the end'.

## What if the starting position or number of characters *depends*? Introducing LEN

In the examples above we have had to specify how many characters we want to grab, or what position to start grabbing from. However, in many codes that number *is not always the same*.

UK postcodes are a great example of this: they come in two basic parts - the first part indicates the area, and the second a street or part of a street in that area. The postcode B422SU, for example, is split into 'B42' (an area in Birmingham) and '2SU'.

The second part of a UK postcode is *always* three characters - but the first part can be two, three, or four characters. Once you know that, you know that you can deduce the length of the first part by the length of the whole.

Let me explain what I mean: if the postcode is five characters long in total, then the first part is two characters (5 minus the 3 characters in the second part). If the postcode is six characters then the first part of it is three characters long; and a seven character postcode has a four-character first part.

Telephone numbers are similar: although area codes might be three, four or five characters long, we might be able to assume that the local number always takes up the final six digits. 

Many other codes share similar characteristics. It's just a case of identifying them.

So: how do we work out the total number of characters in the postcode or telephone number? A function called `LEN`.

`LEN` will tell you the number of characters in a given cell - the *length* of that cell, in other words, spaces included. It only uses one parameter: the cell you want to measure, like so:

`=LEN(A2)`

You can use `LEN` to calculate the number of characters you need in a more sophisticated `LEFT`, `RIGHT` or `MID` function in one of two ways:

* Create a new column for the length of each cell, and use the relevant cell in that column for each `LEFT`, `RIGHT` or `MID` function.
* Use `LEN` *within* a `LEFT`, `RIGHT` or `MID` function, as a nested function.

For example: if we had our postcodes in column A, we might write the following formula in B2 to calculate how many characters the *first* part of each postcode should be:

`=LEN(A2)-3`

For any seven character postcodes, this will return `4` (seven minus three); for six character postcodes, `3`, and for five character postcodes `2`.

In column C, then, we can use that cell within a formula using the `LEFT` function like so:

`=LEFT(A2,B2)`

In other words: grab characters from the left in cell A2, for the number of characters specified in B2.

The *nested function* version of this would bring the `LEN` function directly into the formula without needing an extra column, like so:

`=LEFT(A2,LEN(A2)-3)`

In other words: grab characters from the left in cell A2, for the number of characters that you get by taking the length of A2 and subtracting 3.

You can use the same logic with the `MID` function or `RIGHT` function.

## What if the starting position or number of characters *depends*? Part two: SEARCH and FIND

Sometimes it is not the *length* of a code which determines how much you need to grab, but a particular character or characters *within* it.

Let's say, for example, that we have a collection of spending codes where the department code is always prefixed by a 'K'. All we need to know is 'at what position does the character K occur?' and then we can grab the characters following that.

The `SEARCH` function tells us just that: it searches for a specified character or characters in a specified cell, and returns a number for its position in that cell.

If cell A2 contains the code 123K45, then, we might use the following formula to tell us what position the K appears in:

`=SEARCH("K",A2)`

The function needs just two parameters: the characters being searched for; and the cell being looked in.

The result here is `4`, which can be used as the basis for a `MID` function or `RIGHT` (subtracting 4 from the length of the cell in question would give us the number of characters to grab from the end to get everything up to that point. We'd need to add 1 to include the K too).

The same function can be used to look for names: `=SEARCH(A2, "Smith")` will return 6, for example, if A2 contains "John Smith".

There is a third, *optional*, parameter for the `SEARCH` function: *where to start looking*. This is useful if you want to search more specifically within the last few characters of a cell. 

If the specified character/s are *not* in the specified cell, you will get a `#VALUE!` error.

`SEARCH` is not case sensitive, so if you want to be a little fussier you can use the very similar `FIND` function. This has exactly the same parameters: what you're looking for; where you're looking; and an optional third parameter specifying what position you want to search from. But if you're searching for 'M' and it only has an 'm', the result will be a `#VALUE!` error.

W> For languages that support double-byte character sets (DBCS) such as Japanese, Chinese (Simplified), Chinese (Traditional), and Korean you can also use functions like `MIDB`, `LEFTB`, `RIGHTB`, `LENB`, `REPLACEB`, `SEARCHB` and `FINDB` functions which count each double-byte character as 2. 


A> ## Extracting characters by getting rid of the others: REPLACE
A>
A> An alternative to using `LEN` to calculate the starting point of the text you want is to use a different function to get rid of the text that you *don't want*.
A> 
A> A useful function to that end is `REPLACE`. This will replace characters in any cell at a specified character position for a specified number of characters - rather like a reverse `MID`.
A> 
A> Unfortunately `REPLACE` is one of the more complex functions, requiring four parameters: the cell you want to use; what character to begin replacing from; how many characters to replace for; and what to replace those characters with.
A> 
A> If we had a six character code and wanted to grab the first three characters using this approach, the formula would look like this:
A> 
A> `=REPLACE(A2,4,3,"")`
A> 
A> Translated, this says: replace in cell A2, from the fourth character, for three characters, with nothing (`""`).
A> 
A> As with `MID` you can ensure the replacing runs to the end by using an artificially large number like 100:
A> 
A> `=REPLACE(A2,4,100,"")`
A> 
A> And like `MID`, this will not generate an error.


## Recap

* The functions `RIGHT`, `LEFT` and `MID` will, respectively, **grab a specified number of characters from the end, beginning, or middle of a cell**.

* These can be **especially useful for codes where you just want to extract or test *part of* that code** - for example the first two digits of a telephone code, or the first half of a postcode or cost centre.

* If your data uses slashes, dashes, spaces, commas or other characters that you can split it on, **use the menu option *Text to columns...* (normally in the *Data* menu) to do that. But first make sure that the column you want to split is placed in the last column of your data.

* **The `LEFT` and `RIGHT` functions have two ingredients: the cell you want to use, and the number of characters you want to grab** from the beginning (the left) or the end (the right).

* **The `MID` function needs another parameter: what character position you want to start grabbing from**. This comes second in sequence like so: 'What cell are you using, what position you want to start grabbing characters from, how many characters do you want to grab'.

* **Spaces are counted as characters too** so watch out for empty spaces at the start and end (consider using the `TRIM` function to remove them first) and remember to count them in the middle too!

* **The `LEN` function will tell you the number of characters in a given cell** including spaces, which can then be used to calculate the start or end position for a `MID`, `LEFT` or `RIGHT` function.

* **The `SEARCH` function will tell you the position of a specified character or characters in a cell**, which can then be used as the basis for a `MID`, `LEFT` or `RIGHT` function to grab characters from or up to that position.

* **The `FIND` function is the same as the `SEARCH` function, but is case sensitive**.

* **The `REPLACE` function offers an alternative option which will replace specified character positions with nothing**, leaving you everything else.



## Finding the story: how old are Guantanamo prisoners?

In the last chapter I linked to [a spreadsheet containing details on almost 800 individuals detained by the US Department of Defense at Guantanamo Bay, Cuba](https://drive.google.com/file/d/0B5To6f5Yj1iJa0xmbVNRZTdyQk0/edit?usp=sharing), and suggested some exercises in finding stories using the techniques for getting ages in Excel. 

1. You could check that the date of birth is being treated as a number by Excel in a number of ways. Firstly, by looking at the alignment: it's aligned right, which is how Excel aligns numbers. Secondly, you could right-click on the date and select **Format cells...** then reformat it as *number* or *general* - if it doesn't change to a number then it's a text string. Thirdly, you could use the function `ISNUMBER` to check if a specified cell contains a number - and copy that down a new column.

2. A formula in column C to give us the age of each individual based on their date of birth might look like this: `=YEAR(TODAY())-YEAR(B2)-(TEXT(B2,"mmdd")>TEXT(TODAY(),"mmdd"))`. That formula has two parts: today's year minus the year of the date of birth - `=YEAR(TODAY())-YEAR(B2)` and then a TRUE/FALSE test on whether the month-day of the date of birth is greater than the month-day of today's date: `(TEXT(B2,"mmdd")>TEXT(TODAY(),"mmdd"))`. The result of that second part (TRUE = 1 and FALSE = 0) is subtracted from the first calculation.

> It will be clearer to put those two parts in two separate columns: `=YEAR(TODAY())-YEAR(B2)` in column C and `=TEXT(B2,"mmdd")>TEXT(TODAY(),"mmdd")` in column D, then combine them in a further column as `=C2-D2`.

> If you can remember the `DATEDIF` function it's much easier: `=DATEDIF(B2,TODAY(),"Y")`. That is: *give me the difference between the date in B2 and today's date, in years*.

> There's also Steve Doig's approach, which would give you an age including decimal places. That would be adapted as this formula: `=(TODAY()-B2)/365.25`. Remember not to reduce the decimal places which will round the number up!

W> Because the B column is formatted as a date, when you insert a new column it will use the same formatting for your results: 101 will be formatted as `04/10/00` or similar. Remember to format the cells in the new column as a number to prevent this happening.

3: The answers above cover some of the error checking: for example, checking if someone's birth date is later than today's date, or ensuring that numbers are not rounded up. However, you'll also need some more traditional error checking: the first prisoner listed, for example, seems to have been born in 1913, making him over 100 years old. Or was he born in 2013, making him a mere baby?

> It seems Mohammed Sadiq was indeed born in 1913. [A document from the Department of Defense published on The Guardian website](http://www.theguardian.com/world/guantanamo-files/US9AF-000349DP) appears to confirm it. But whether he is over 100 - or has since died - we would also have to check.

> There are also rows towards the bottom of the spreadsheet which generate `#VALUE!` errors when you attempt to calculate an age. This is because the 'date of birth' field does not contain a date, but a string of text such as "26 yrs old in 07", "UNDISCLOSED", "UNKNOWN" or "c. 1974". Some further research might yield more information, but more likely we will have to be clear that a certain proportion of our figures are unknown.

4: The stories we might tell about these ages include: a story on the oldest prisoner in Guantanamo; a story on the youngest prisoner(s) in Guantanamo; a story on young prisoners generally - how old were they when they were arrested. Mohammed Sadiq, for example, according to that document, was 89 when he was detained in 2002. You could also write a story on the ageing section of prisoners. And of course you could chart the age distribution of prisoners from different countries: Sudanese prisoners, for example, tend to be much older than those with Yemeni citizenship. Is this because some prisoners are accused of being fighters while others are suspected of being politically powerful?


X> ## Find the story: what postcode areas are worst for hygiene inspections?
X> 
X> In [the food hygiene data you've been working on](https://drive.google.com/file/d/0B5To6f5Yj1iJeUM2VFZkaUc3MDQ/edit?usp=sharing) for the last couple of chapters is a 'PostCode' column. As explained in this chapter, UK postcodes can be anything from five to seven characters long, but the second part is always three characters.
X> 
X> 1. Create a new column which uses the `LEN` function to calculate the length of each postcode in turn.
X> 
X> 2. Create a new column which uses *that* column (the lengths of the postcodes) to calculate how long the first part of each postcode is: 2, 3 or 4.
X> 
X> 3. Create a third new column which uses the `LEFT` function and *that* column (the lengths of the first part of each postcode) to extract the first part of each postcode.
X> 
X> 4. In a fourth column try to write one *nested* function which uses both `LEN` and `LEFT` to grab the right number of characters from the left for each postcode. If it works you can delete the other three that it took to do the same job!
X> 
X> 5. In a further column see if you can get the same result by using the `REPLACE` function. There's no reason why you should use this more complex function instead for this particular job, but it's useful to get some experience in using it.
X> 
X> 6. How can you work out which postcode area appears most often?