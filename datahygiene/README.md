# Avoiding mistakes when cleaning your data #

One of the most important and oft-neglected steps in any data journalism project is cleaning up raw data.  Almost always, when you first get the data you want (or the data you've settled for), it's not yet usable.  It's riddled with inconsistencies and doesn't express the relationships you want to analyze.  You don't fully understand its parameters yet.  Before you can make it interesting, you need to field strip it and put it back together.

This process is dangerous; there are lots of ways to go wrong and make mistakes without even realizing it.  WHen you screw up your data in the process of cleaning it, the best case scenario is that you waste a lot of time, but the worst case scenario is that your ultimate analysis will be wrong because you've unknowingly changed the data.

Here are some guiding principles to consider whenever you're about to start cleaning up and reshaping raw data for analysis.

## Don’t change your original files/tables ##

If you have a big Excel file or a MySQL table that needs cleaning, don’t just go in there and start drilling holes in the wall.  Make a copy and work on that instead.  It will make it easier to start over if you screwed up, and, more importantly, you can compare what you’ve got at different stages of cleaning and mutation to make sure it matches up not just with the previous step in your cleaning, but with the starting data.

As a corollary to this, you shouldn't throw away data just because you think you don't need it.  Be a bit of a digital packrat.  Keep things around, but also keep them clearly labeled so you don't mistake one file for another.

## Spot check everything ##

As soon as your dataset is large enough that you can’t read it all line-by-line (which is to say, just about every dataset), you have to start spot checking.  Any assumptions you have about the nature and consistency of the data are just a guess, and they’re probably wrong.  `SELECT * FROM `TABLE` ORDER BY RAND() LIMIT 5` is one my most frequent SQL statements (for non-SQL people: “show me 5 random rows from the table”).  Use it, love it.   Get a few random records at a time and read them closely.  See if anything seems off.  Do this early and often.  It will prevent dumb mistakes but there’s also a bonus: it will give you much better insight into your data.

## Check for weird values ##

Look out for empty values, and especially for pseudo-empty values.  Excel files from government agencies are unendingly creative when it comes to weird placeholder characters, and you’ll get data that uses - or * or ~ to fill a blank space.  For bonus points, check for extreme value lengths too.  If most of the street addresses in your data are about 75 characters long and one of them is 450 characters long, strange things are afoot at the Circle K.

For every categorical column, check the list of values that exist in the data.  Make sure everything that should be there is, and anything that shouldn’t be isn’t.  `GROUP BY` (or its twin `SELECT DISTINCT`) should become your new best friend.  For every numerical column, at least sanity check mins and maxes.  Check the ranges of every special data type (e.g. ZIP codes).  Don’t assume that all dates will be formatted the same, or even be dates.

When you’re `GROUP`ing things, use `COUNT()` to see how many items there are in each category.  See if all the numbers pass your basic smell test.  If they don’t, you probably screwed something up, but if you’re lucky it means there’s a story there.  Either way, you want to know.

Text data sucks.  Be careful about whitespace, capitalization, and character collation in text columns.  There could be random whitespace in your cells that breaks your ability to compare or group them.  You know that "Paris" and "Paris " are the same, but your database doesn’t.  Use `TRIM()` where it’s reasonable to do so.  And remember Veltman’s Law of Character Sets:

**Veltman’s Law of Character Sets:** there will be exactly one é somewhere in your data and you won’t find it until it ruins everything.

## Worry about your column types ##

Whether you’re using Excel, a relational database, or something else, make sure that your columns/fields are the type you want, and ALL of the data going in matches those types.  If you don’t, your data is going to get squeezed through a special decorative mold when you import it and all your candy cane integers will become snowflake text and you won’t even know it’s happening.  This is especially important if your data is going to pulled out the other side later to power some sort of cool app, where it’s going to go through the variable type wringer again.

**Pop quiz:** which of the following things evaluate as `false` in whatever language your developers use?  
`0, “0”, 0.0, “0.0”, [], {}, [{}], null, undefined`

**Answer:** don’t let this question come up.

## Do dry runs before you change things ##

Are you changing all 50,000 rows in a table with a complicated `UPDATE` statement?  Try updating a few of them first and inspecting the results.  Make sure your clever operation with five nested `REPLACE()`s and an inscrutable `WHERE` clause does what you think it does.  Or maybe you’re auto-generating a thousand database queries with a nifty script you wrote.  Run a version that prints the queries without executing them first and read the output.  `SELECT` liberally, `INSERT` and `UPDATE` conservatively.

## Clear is better than clever ##

There’s no prize for writing the world’s most beautiful `JOIN` or most deeply-nested Excel formula, so don’t try.  If you’re a journalist trying to whip some data into shape, it doesn’t matter if you take a few extra computing cycles.  You are not working with Big Data.  Processing power is cheap.  Storage is cheap.  Your time is not.  Trying to be clever will make things harder to understand for you and for others, and it increases the chances that you’ll do something really stupid.  When clever things fail, they tend to fail insidiously (I’m looking at you, crazy regexes).  Keep your operations simple.  Work stepwise.  Break complicated operations into multiple simple ones.  It creates a better trail so that when the results are all screwed up (they will be), you can figure out why.

## Keep track of what you’re doing ##

Out of any piece of advice on this list, this is the one I’ve most consistently failed to heed.  Don’t be like me.  Keep a log of what you’re doing to the data.  It can be a paper notebook or a slick automatic query logger or file version control with commit messages or a friendly parrot.  It doesn’t matter.  Knowing what exactly you’ve done to your data is important for troubleshooting and undoing mistakes, but it’s essential for quality journalism.