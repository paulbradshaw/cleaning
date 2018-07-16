# What day did that date fall on? Which year was the worst? Extracting days, months and years from full dates

*This is an extract from [Finding Stories in Spreadsheets](http://leanpub.com/spreadsheetstories/)*

Dates are enormously useful bits of data. They can help us to find stories about peaks and troughs: whether things are getting better or worse; what times of the year or week are or were best or worst for a particular event - whether that event is a crime, or an extreme bit of weather.

But if you only have *dates* and you want to look more widely than *daily* trends you'll hit a problem: aggregating data by year or month is not as straightforward as you might expect. And trying to work out anything based on days of the week is another challenge altogether. 

This is because, as mentioned previously, dates are not stored as a series of characters such as "11/11/14" but rather as a number: the number of days since January 1st 1900 (or in some cases, 1901).

So we can't simply ask "How many times does 'September' appear?" or "Which day of the week appears most often?" of a list of numbers like '41954'. Pivot tables will give us a different row for every date.

Instead, we need to *extract* the month, day or year from *each* date in turn - typically to create a new column whose values we can then count.

## Extracting dates, months and years: DAY, MONTH and YEAR

The functions `DAY`, `MONTH` and `YEAR` are designed for this purpose. Try the following:

* Open an empty spreadsheet and type a date in the first cell, A1 - for example '11/12/2013'. 
* In cell B1 type the formula `=YEAR(A1)`.
* In cell C1 type the formula `=MONTH(A1)`.
* In cell D1 type the formula `=DAY(A1)`.
* In cell E1 type the formula `=WEEKDAY(A1)`

Each of those formulae should give you a number corresponding to the year, month, or day in the date it uses (`WEEKDAY` returns a number representing the day of the week - between 1 (for Sunday) and 7 (for Saturday)).

This is very useful - but what if you don't want a number? What if you want that day or month as a word? Here we'll need a slightly more powerful function.

## Extracting days and months as words or years as '66, '94 etc: TEXT

The `TEXT` function allows you to grab the day, month or year *in a format you dictate*. For example, whereas `=DAY(A1)` would grab `11` for the date `11/12/2013`, what if you wanted it to say `Wednesday` or `Wed`? What if you wanted to grab `December` or `Dec`? What if you wanted to grab `13` rather than `2013`?

In order to allow you to specify this, `TEXT` has two parameters: the cell you want to work from, and the format of the date information you want to extract.

This format is expressed as a series of characters. For example, if you wanted the month you would use the series of characters `"MMMM"`; if you wanted the month as a three-character expression (i.e. `Dec`) then you would use `"MMM"`. Days are indicated by between one and four Ds, and years by Ys.

To show you how this works in practice, here are a series of `TEXT` formulae using different series of characters to indicate what they want to extract - followed by the result when used on the date 11/07/2013 (represented by `A1` in the formula):

* `=TEXT(A1,"MMMM")` gives you `July`
* `=TEXT(A1,"MMM")` gives you `Jul`
* `=TEXT(A1,"MM")` gives you `07`
* `=TEXT(A1,"M")` gives you `7`
* `=TEXT(A1,"D")` gives you `11`
* `=TEXT(A1,"DD")` gives you `11`
* `=TEXT(A1,"DDD")` gives you `Thu`
* `=TEXT(A1,"DDDD")` gives you `Thursday`
* `=TEXT(A1,"YYYY")` gives you `2013`
* `=TEXT(A1,"YYY")` gives you `2013`
* `=TEXT(A1,"YY")` gives you `13`
* `=TEXT(A1,"Y")` gives you `13`

You'll notice that the one-character options like `"M"`, `"D"` and `"Y"` will grab only one digit when the month or day is below 10 (i.e. the months January to September and the first nine days in every month), but if the month or day is 10 or higher, it will still show two digits. So in our examples above the 11th day is shown as `11` whether you use `"D"` or `"DD"`, but for the seventh month it does make a difference.

With years this doesn't make any difference at all: 2001 comes back as `01` whether you use `"Y"` or `"YY"`. 

Only one of those letters can be used *five* times: 

* `=TEXT(A1,"MMMMM")` gives you the first character of the month: `J` for July, for example, but also January and June - so, probably not much use then.


### Month and year combinations

You can also combine these characters in any way you want to generate specific date combinations. For example, if you wanted the month and the year together you could use a formula like this: 

`=TEXT(A1,"MM YY")`

You can also add slashes, dashes, commas or other characters like so:

`=TEXT(A1,"MM/YY")`

Using one of these formulae allows you to generate whole columns showing just the year, month, or day on which an event occurred - as numbers, words, or three-character codes. Now you can calculate the trends you're looking for.

## Using the Format Cells 'Custom' option to do the same thing to existing dates

If all you want is a series of months or days, you can actually use the **Format Cells** menu to change a date column in the same way. 

This can be accessed by first selecting your date column (so that the results apply to the whole column), and then selecting the **Format** menu and, from that, **Cells**. A quicker approach is to right-click on your selection and select **Format cells** from the dropdown menu that appears - or using the keyboard shortcut CTRL + 1 (CMD + 1 on a Mac)

This should bring up the *Format cells* menu. You'll notice that this has a *Date* option to change how a date is displayed. Instead of the three series of digits separated by a slash, for example (i.e. 11/07/2013), you can choose a more natural language expression such as "Thursday, 11 July 13" and a number of other variations. 


As always, this doesn't affect the underlying information (which is that number representing the number of days since the start of 1900) but it still shows *all* the information: day, month, year.

To show *only* that, select the *Custom* option at the bottom of the list of formatting options on the left. If the cell is already formatted as a date, then you should see date-related options suggested in the box underneath.


If not, you will see a series of other options. But it doesn't matter: either way you will need to type what you want into the box above those, and underneath *Type:*.

At the moment this will show something like `dd/mm/yyyy` and the grey *Sample* area above should show a preview of how that will affect the information in the first cell you're formatting.

Now, knowing those special series of characters used to specify 'a month as a word' or 'the day as a three character code' you can enter your own characters here:

* `mmmm` to show just the month for each date (as a full word)
* `mmm` to show just the month for each date as a three character code
* `ddd` to show just the day for each date as a three character code
* `dddd` to show just the day for each date as a full word

...and so on. As you type the characters you can see the effect on the preview immediately. 

And of course these can be combined: `mmmm dd yy` will give you `July 11 13`; `yyyy-mmm-d` will give you `2013-Jul-11`.

You can add text strings here if you feel like it, by putting them in quotation marks like so: `mmmm "the" dd` which will produce `July the 11`. If you feel tempted to add "th" on the end, remember that this will apply to *all* dates, including the "1th", "2th", "3th" and so on.

Once applied, you will have a whole column containing just months, days, years, or any combination of those. The major disadvantage is that your original full dates are now hidden. This is why it's often better to use the `TEXT` function to fill extra columns with the specific data you want to work with, while leaving the full dates alone to be checked alongside that.

## Hours and minutes: HOUR, MINUTE, SECOND and TEXT again

Sometimes you may come across dates that also include time information like so:

`06/06/2014 01:34`

If this is treated by Excel as a date (you can tell because it will be aligned right) then the same functions should work on this. `DAY`, `MONTH` and `YEAR` will still grab just that, and `TEXT` can be used for those ends too.

In addition, you can use three more functions to grab the hour, minute or second: `HOUR`, not surprisingly, `MINUTE` and... well of course, it's `SECOND`.

This formula, then, would grab the hour from any date in cell A2:

`=HOUR(A2)`

This includes dates without any specified hour (the result will be `0` - that is, midnight).

And this formula would grab the minute:

`=MINUTE(A2)`

Again, this will be `0` for dates without any hour or minute, because Excel assumes the time for those to be 00:00.

Finally, for those pieces of data where even the seconds are recorded, you might use the `SECOND` function in the same way:

`=SECOND(A2)`

The `TEXT` function also allows you to do the same with a little more control. This form example will give you the hour as a two-character result:

`=TEXT(A2,"HH")`

So for 01:34 the result will be `01`, rather than `1`, which would be the result of using `HOUR`.

A major peculiarity here is that the letter M can be used to identify both month and minute. Here's how it works:

In both the `TEXT` function and the **Format cells** menu the *default* assumption is that when you use `M` you mean 'month'. However, *if the M is used with H (hour) or S (second) it will assume you mean minute*. The following formula for example will grab the hour and minute, separated by a colon:

`=TEXT(A2,"HH:MM")`

And this will grab the minutes and seconds:

`=TEXT(A2,"MM:SS")`

It doesn't matter if no seconds are given in the cell - again it will count this as 'zero seconds'. So you can still use this to grab minutes and perform calculations with those.

And of course you can grab all three:

`=TEXT(A2,"HH:MM:SS")`

A final note on the `TEXT` function: it is not just designed to extract details from dates: [the same function can be used with numbers](http://www.techonthenet.com/excel/formulas/text.php) - particularly financial figures to add currency signs, commas, round up or down and add other formatting. 



## When things don't go as you expect them to: dealing with errors in date functions

There are two errors you are likely to get when dealing with dates: the first is a problem with your source data (the dates themselves); the second is a problem with your results *other* than an error message. For example, you expect to see a number between 1 and 12 to indicate a month, but instead are seeing a whole new date.

If you have written a formula to extract part of a date and get a *whole* date as a result (and an odd one at that), it's probably because the cell you're writing in is formatted as date itself (and so the resulting number is formatted as a date). 

Why is this a problem? Because the number `2013`, when formatted as a date, will appear as `05/07/1905` (the 5th of July 1905 was 2013 days after the start of 1900). The number `11` will be shown as `11/01/1900`, and so on.

To solve this problem just fix your formatting: select the cells in question and open the **Format cells** menu (either right-click, or use the *Format* menu at the top of the screen).

If, however, the problem is with your source data and you get a `#VALUE!` error when you try to use one of the date functions detailed above, it means that what you think is a 'date' actually isn't: most likely it's a series of characters that Excel hasn't been able to interpret as a date. 

Throughout this chapter - and in the earlier chapter on *'Hitting the deadline: understanding and formatting the data'* I have explained how dates are actually stored as numbers: the amount of days since the start of 1900. Hours and minutes are stored as decimal places, so 12 midday would be .5.

If a date isn't stored this way - if it is expressed as a series of characters, or Excel thinks it is just a series of characters - then it cannot perform calculations and use date functions like `YEAR` (which are, after all, calculations to convert a number like 41466 into 2013).

This might happen for a number of reasons: it is not uncommon in data that has been extracted from a PDF, for example. Or it may be that extra characters have been entered, like "pm", or even extra spaces, which can cause problems. Sometimes it's to do with the way data entry is set up, and the 'date' is classified as a text field. 

But whatever the reason, you have to deal with it. On this front, the function `DATEVALUE` is worth trying first. 

`DATEVALUE` takes a series of characters and attempts to convert it into a value which can be understood as a date. 

For example, `=DATEVALUE("18/10/2012")` will return the value `41200` (assuming the cell is formatted as 'general' or 'number' - if it is formatted as a date it will return `18/10/2012`).

Likewise, `=DATEVALUE(A2)` will do exactly the same to the contents of cell A2. If it works on one cell, you should be able to copy it down an entire column to perform the same conversion on a column full of dates-as-text. You can then format the column to show the numbers as dates, etc.

Because `DATEVALUE` works with text characters rather than underlying dates-as-numbers it is important to check that those dates use the same formatting as your system's date formatting. For example, if you're converting a US-style date format (month-day-year) but your computer system uses UK-style date formatting (day-month-year) or vice versa. In these cases you might need to treat it using the approaches detailed in the next chapter.

Likewise, if `DATEVALUE` gives you a `#VALUE!` error you'll need to use some different functions to extract parts of your date-as-text also detailed in the following chapters.


## Recap

* To find a story about trends in dates, you first need to **extract the month, day or year from each date**.

* The simple functions **`DAY`, `MONTH` and `YEAR` will extract just the day (number from 1 to 31), month or year** from any cell it uses as an ingredient. For example, `=YEAR(A1)` will give you the year of the date in cell A1.

* However, **those functions will only give you results as a number**: for example, `DAY` will return `10` for the 10th day, `21` for the 21st, and so on. It will not tell you what day of the week it was.

* **The `TEXT` function gives you much greater control over what date information you want to extract, and how it is shown**, including the ability to show days or months as words or three character codes.

* You can control what information you extract and how it is shown by using a series of characters for 'month', 'year', 'day', and so on, used as a second parameter in quotation marks like so: `=TEXT(A1,"DDDD")`. Four Ds for 'day', in this example, will extract the day as a word. 

* As a general rule, **one character means 'one or two digits'; two characters means 'two digits'; three characters means 'a word as a three letter code' and four means 'a whole word'**. For a date in January each of these would return `1`, `01`, `Jan` and `January` respectively.  

* **You can also combine more than one series of characters to generate combinations**, such as 'month/year' or 'day/month'. Such a formula would look something like this: `=TEXT(A1,"MMMM/YYYY")` or this: `=TEXT(A1,"DDDD/MMMM")`.

* **Dates are stored as the number of days since the beginning of 1900**. This allows Excel to make all sorts of calculations.

* **These numbers can be formatted in any way you like by using the *Format cells* menu** and selecting the *Date* options.

* **To display just the month, day or year of dates you can use the *Custom* section** and the same letter codes as the `TEXT` function, e.g. `mmmm`

* However, **it is best to extract parts of dates into a separate column so you can still see the original dates in full**.

* **`WEEKDAY` is another date function which will tell you on which day of the week a date occurred, expressed as a number between 1 and 7**.

* **You can specify whether `WEEKDAY` starts counting from Sunday, Monday, or any other day by using a second parameter**. You can also specify that it starts counting from zero.

* **Some dates include hours, minutes and seconds. These are stored as fractions of days**. So 12 midday, for example, is represented by adding an extra 0.5 to the number representing the date.

* **You can extract the hour, minute or second of a date using the `HOUR`, `MINUTE` or `SECOND` functions**.

* **You can also extract and combine hours, minutes and seconds using the `TEXT` function and *Custom* formatting option too**. 

* **The codes to extract hours or seconds are `HH` and `SS`**. 

* **Minutes can be extracted using `MM` *alongside* the `H` or `S` codes. However, Excel will assume you want the *month* if you use `MM` on its own**.

* The `TIMEVALUE` function can **convert text expressions of times into numerical ones. It can also extract times from date-and-time entries**.

* The `TIME` function can **combine and convert parts of times (hours, minutes and seconds) into a single time entry** - but you must give it all three parts separately.

* **If a date is stored as a series of characters instead of as a number, date functions will return an error: `#VALUE!`**. In these situations you can try to convert the series of characters into a date using the `DATEVALUE` function; or use other functions to extract the relevant part of the 'date' - for example the first two characters (day) or last four characters (year). We'll explain this process in the next chapter.

* **If the result of your formula to extract a year, month or day is another full date then it's likely the results have been wrongly formatted** to convert whole numbers like '2013' or '12' into a date based on that number of days since 1900. To fix this change the formatting to a number.


## Finding the story: which outlets have consistently bad scores?

At the end of the last chapter we looked at [some food hygiene ratings data]() and suggested that we could use the `AND` function to identify businesses with a particular *combination* of qualities. Here's how to do that:

1. In column N your `AND` formula to identify which outlets scored 10 or above across all three criteria in columns K, L and M should look something like this: `=AND(K2>=9,L2>9,M2>9)` - or, if you were using the 'greater than or equals to' operator, this: `=AND(K2>=10,L2>=10,M2>=10)` 

2. Changing the formula so it returns TRUE if *any* rating is 15 or above would mean switching to the `OR` function like so: =OR(K2>=15,L2>=15,M2>=15)

3. Now, the tricky one. Finding out which outlets have a bad overall rating is straightforward: our formula would begin `=AND(A2<3,` ... but to add a criterion which tests whether the date was a year ago we need to make sure it can perform that calculation. There are a number of ways to do this...

> The simplest is to put your date in quotation marks. If one year ago was the first of January 2013, you might express 'less than' that date like so: `<"2013-01-01"`. Adding that to your formula would look like this: `=AND(A2<3,A2<"2013-01-01")`.
>
> A second option is to express that date as a number. To do this, type it in a cell somewhere and then change the formatting of that cell from 'date' to 'number'. The number for the first of January 2013 is 41275. See the chapter on dates for more on formatting.
>
> A third option is to put your date in another cell, and refer to that in your formula. For example, if you put the date in cell O1 (the first row in column O) your formula could test against that like so: `=AND(A2<3,A2<O1)`. However, remember that if you copy this formula down to further cells, that cell reference will change - to O2, then O3, O4, and so on. To fix it you need to use a dollar sign before the column name and row number, like so: $O$1 - making the formula look like this: `=AND(A2<3,A2<$O$1)`

X> ## Find the story: what years and months are worst for hygiene inspections?
X> 
X> In the [data for the previous exercise](https://drive.google.com/file/d/0B5To6f5Yj1iJeUM2VFZkaUc3MDQ/edit?usp=sharing) is a column called `RatingDate`. This contains date information for all the inspections. Based on this, try the following:
X> 
X> ![](images/ratingdate.png)
X> 
X> 1. At the moment the cells show the dates as numbers - e.g. `41137`. Select the whole column and change the formatting so we can see those formatted properly as dates. 
X> 
X> 2. Insert a new column to the right of the date column. Call it 'Year'. Write a formula to extract the year from those dates. Copy it down the whole column so you have a year for each inspection.
X> 
X> 3. Insert another new column to the right of your new year column. Call it 'Month'. Write a formula to extract the month from the dates. Copy it down the whole column so you have a month for each inspection.
X> 
X> 4. You know what's coming don't you? Insert yet another new column to the right of your new month column. Call it 'Day'. This time, however, write a formula to extract the day in each date *as a word*, i.e. Monday, Tuesday, Wednesday etc. rather than 21, 22, 23, etc. Copy it down the whole column so you have a month for each inspection.
X> 
X> 5. How can you work out in which year or month, or on which day, the failing ratings (2 or below) are given? (There is more than one way).
X> 
X> 6. What issues might you need to watch for in drawing any conclusions about the 'worst' month, year or day etc?
X> 
X> 7. How might you account for those issues?
