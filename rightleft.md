# Grabbing or checking the first, middle or last part of a piece of information: `RIGHT`, `LEFT` and `MID`

*This is an extract from [Finding Stories in Spreadsheets](http://leanpub.com/spreadsheetstories/)*

Sometimes you only want to look at part of some data - for example the first or last part of a postcode, zip code, or telephone number. Dates which are stored as text rather than numbers are also prime candidates.

The functions `RIGHT`, `LEFT` and `MID` are tailor-made for those problems. These will, respectively, grab a specified number of characters from the end, beginning, or middle of a cell. 

Used on their own they work particularly well where you always want the same number of characters - but they can also be combined with other functions to grab different amounts of text, as we'll see.

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



