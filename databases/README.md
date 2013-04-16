# SQL: The Prequel (Excel vs. Databases) #

## What Excel is good at ##
Excel gets a bad rap.  It's a very flexible, powerful piece of software that's good at a lot of things.

* It's easy to **browse** data.
* It's easy to **manually enter and edit** data.
* It's easy to **share** copies of files.
* You have fine control over **visual presentation**.
* It has a very **flexible structure**.  Every cell is a unique snowflake.
* It integrates with other **popular office software**.
* Formulas make it a **living document**.
* It has a **built-in suite of helpers** for charts, comments, spellchecking, etc.
* It's relatively **easy to learn**.

## What Excel is bad at ##
Unfortunately Excel also has its limits.  It's pretty bad at some other things.

* It **lacks data integrity**.  Because every cell is a unique snowflake, things can get very inconsistent.  What you see doesn't necessarily represent the underlying data.  A number is not necessarily a number.  Data is not necessarily data.  Excel tries to make educated guesses about you want, and sometimes it's wrong.
* It's not very good for **working with multiple datasets** in combination.
* It's not very good for **answering detailed questions** with your data.
* It **doesn't scale**.  As the amount of data increases, performance suffers, and the visual interface becomes a liability instead of a benefit.  It also has fixed limits on how big a spreadsheet and its cells can be.
* **Collaborating is hard**.  It's hard to control versions and have a "master" set of data, especially when many people are working on the same project (Google Spreadsheets fix some of this).

## Enter relational databases ##
What is a relational database?  We could get very picky about terminology, but let's not.  Broadly speaking, it consists of a "server" that stores all your data (think of a huge library) and a mechanism for querying it (think of a reference librarian).

The querying is where **SQL** comes in, emphasis on the Q.  SQL stands for *Structured Query Language*, and it is a syntax for requesting things from the database.  It's the language the reference librarian speaks.  More on this later.

The "relational" part is a hint that these databases care about relationships between data.  And yes, there are non-relational databases but let's keep this simple.  We're all friends here.

## The database mantra ##

### Everything in its proper place. ###
A database encourages you to **store things logically**.  Sometimes it forces you to.

Every database consists of **tables**.  Think of a table like a single worksheet in an Excel file, except with more ground rules.  A database table consists of **columns** and **rows**.

#### Columns ####
Every column is given a **name** (like 'Address') and a defined **column type** (like 'Integer,' 'Date', 'Date+Time', or 'Text').  You have to pick a column type and it stays the same for every row.  The database will coerce all the data you put in to that type.  This sounds annoying but is very helpful.  If you try to put the wrong kind of data in a column, it will get upset.  This tells you that there's a problem with your data, your understanding of the data, or both.  Excel would just let you continue being wrong until it comes back to bite you.
 
You can also specify helpful things like…  
  … whether a column can have duplicate values.  
  … whether a column can be empty.  
  … the default value for a column if you don't specify one.  

*Columns define the structure of your data*.

#### Rows ####
Rows are the actual data in the table.  Once you establish the column structure, you can add in as many rows as you like.

Every row has a value for every column.  Excel is a visual canvas and will allow you to create any intricate quilt of irregular and merged cells you'd like.  You can make Tetris shapes and put a legend in the corner and footnotes at the bottom, all sharing the same cells.

This won't fly with a database.  A database table expects an actual grid.  It's OK for cells to be empty, but to a computer intentionally empty is not the same as nonexistent. 

#### Multiple tables, joins, and keys ####
More on this later, but part of putting everything in its proper place means making it easy to break up your data into several tables for different data categories and work with them as a set.

### Let data be data. ###
A database focuses only on the data layer of things and ignores visual presentation.  Colors, fonts, borders, date formatting, and number formatting basically don't exist.  What you see is mostly what you get.  That's good news and bad news: it means that a database is usually really good at what it does do, but it also often needs to be paired with other things in order to create a final product, like a chart or a web page.  **A database is designed to plug in to other things.**  This extra step is one of the things that turns a lot of people off to databases.

By being really good at data storage and processing and not at other things, databases are extremely scaleable.  1 million rows of data? 10 million? No problem.  For newsroom purposes, there's virtually no upper limit to how much data you can store, or how complicated you can make your queries.

## Databases & the web ##
Databases are great for preliminary analysis, exploration, and data cleaning.  But they're even better for connecting to something else once you know what you want to do with the data.  Virtually every web application is powered by databases like this.  When you log in somewhere, it's checking your username and password against a database.  When you go to IMDB and click on a movie, it's looking up a database.  Twitter, Facebook, Gmail, it's databases all the way down.

When it comes to news features, a database usually gets involved when the amount of data is large or it is expected to change over time.  Rather than having a static JSON file with your data, you keep a database and you write an app that queries the database for the current data.  Then, when the data changes, all you have to do is update the database and the changes will be reflected in the app.  For one-off apps where the data is not going to change and the amount is small, a database is usually overkill, although you may still use one in the early stages to generate a data file of some sort.

If you're using an API to feed current data into an app instead, you're still using a database, you're just letting someone else host it for you.  This is much easier, but also poses risks because you access the data at their pleasure.

Potential semantic quibble: sometimes an app isn't directly accessing a database to fetch information.  Sometimes it's accessing cached files instead, but those cached files are generated automatically based on what's in the database.  Tomato, Tomahto.

## So when should I use a database instead of Excel? ##
Excel and databases are good for very different things.  Neither is per se good or bad.  A rule of thumb: you should strongly consider using a database for a project to the extent that the following are true:
You have a lot of data.
Your data is messy or complex.
You want to power something else with your data.
Other people need to work with the same data.

## OK, I'm intrigued.  How do I get started? ##
Because databases have a learning curve, it makes sense not to dive in too deep right away.  Start off using it only in cases where the advantage is especially strong and your needs are pretty simple.  As your comfort level increases, you'll increasingly look towards databases to get what you want faster.
 
### Option 1: SQLite ###
SQLite is a good way to get started.  You can install the "SQLite Manager" add-on for Firefox and do everything within the browser.

A SQL tutorial based on SQLite: https://github.com/tthibo/SQL-Tutorial

### Option 2: Microsoft Access ###
Microsoft Access runs on SQL and presents a more traditional desktop software interface.  Depending on who you ask, it's either a helpful tool or just makes things more confusing.  I wouldn't personally recommend it, but your mileage may vary.

### Option 3: Set up a shared web hosting account ###
You can set up a shared web hosting account as a sandbox to play with this.  This can cost as little as £20 a year.  These accounts typically come with an interface to let you create, edit, and interact with databases without writing any SQL.  They also give you a place to play around with any other web-related skills you're interested in and share the results with others!  
A Small Orange (a good, cheap hosting option): http://asmallorange.com/

### Option 4: Install MySQL or PostgreSQL on your computer ###
You can install MAMP (http://www.mamp.info/) on a Mac or WAMP (http://www.wampserver.com/) on a Windows PC.  This will install MySQL as well as a great web-based interface called phpMyAdmin (http://www.phpmyadmin.net).  Once you have MySQL installed, you have lots of additional options for free software to serve as a browser/editor for your SQL databases.  If you prefer you can install PostgreSQL instead, a slightly different database flavor (there are many different flavors of database but MySQL and PostgreSQL are two popular ones with lots of documentation, don't overthink it for now).

---

## Appendix: querying for fun and profit ##
Much of the power of relational databases comes from SQL, a very flexible language for asking a database questions or giving it orders.  The learning curve is steeper than Excel, but once you get the hang of it you can quickly answer almost any question about your data.  Let's go through some brief examples of how a desired action looks in SQL.

The basic building blocks of SQL are four verbs: `SELECT` (look up something), `UPDATE` (change some existing rows), `INSERT` (add some new rows), `DELETE` (delete some rows).  There are many other verbs but you will use these the most, especially `SELECT`.

Let's imagine a table called athletes of Olympic athletes with six columns:

    name
    country
    birthdate
    height
    weight
    gender


When creating our table, we might also specify things like "country can be empty" or "gender must be either M or F."

**Query 1: Get a list of all the athletes in alphabetical order.  This will show you the entire table, sorted by name from A-Z.**

    SELECT
    *
    FROM athletes
    ORDER BY name ASC


**Query 2: Get a list of all the athletes on Team GB.  This will show you only the rows for British athletes.  You didn't specify how to sort it, so you can't count on it coming back in the order you want.**

    SELECT
    *
    FROM athletes
    WHERE country = 'Great Britain'


**Query 3: What country is the heaviest on average?  This will take all the rows and put them into groups by country.  It will show you a list of country names and the average weight for each group.**

    SELECT
    country,AVG(WEIGHT)
    FROM athletes
    GROUP BY country


**Query 4: What birth month produces the most Olympic athletes?  Maybe you want to test an astrological theory about Leos being great athletes.  This will show you how many Olympic athletes were born in each month.**

    SELECT
    MONTHNAME(birthdate),COUNT(*)
    FROM athletes
    GROUP BY MONTHNAME(birthdate)


**Query 5: Add a new athlete to the table.  To insert a row, you specify the columns you're adding (because you don't necessarily need to add all of them every time), and the value for each.**

    INSERT
    INTO athletes
    (name,country,height,weight,gender)
    VALUES ('Andrew Leimdorfer','Great Britain',180,74.8,'M')


**Query 6: Get all male athletes in order of height:weight ratio.  Maybe you would notice something strange about Canadian sprinter Ian Warner.**

    SELECT
    *
    FROM athletes
    WHERE gender = 'M'
    ORDER BY height/weight ASC


**Query 7: If you got your data from london2012.com, you would think 5' 7"  Ian Warner was 160 kg, because his weight was probably entered in lbs instead.  Let's fix that by UPDATEing his row.**

    UPDATE
    athletes
    SET weight = weight/2.2
    WHERE NAME = 'Ian Warner'


**Query 8: Delete all the American and Canadian athletes, those jerks.**

    DELETE
    FROM athletes
    WHERE country = 'United States of America' OR country = 'Canada';


Once you look at enough queries you'll see that a query is like a sentence with a grammar.  It has a "verb" (what kind of action do I want?), an "object" (what tables do I want to do the action to?), and optional "adverbs" (how specifically do I want to do the action?).  The "adverbs" include details like "sort by this column" and "only do this for certain rows."

### Bonus Level: multiple tables and a brief taste of JOINs ###
You probably have a lot more data than just athletes.  For each country, you might also have its flag, its population, and its capital city.  You also have all the Olympic events, which athletes participated in them, which venues they were at, which sport category they belong to.  For each event, you also end up with results: who got which medals, and in some cases what the finishing times/scores were.

The typical Excel approach to this is one of two forms of madness: either you have a bunch of spreadsheets and painstakingly cross-reference them, or you have one mega-spreadsheet with every column (government data sources love mega-spreadsheets).

You might have a spreadsheet where each row is an athlete, and then you have a long list of columns including lots of redundant and awkwardly-stored information, like:

    name, country, birthdate, height, weight, gender, country_population, country_flag_url, country_gdp, event1, event1_date, event1_result, event2_date, event2_result, event3_date, event3_result, event4_date, event4_result, number_of_medals

This mega-spreadsheet approach has all kinds of weaknesses:
* **You lose the benefit of visually browsing** the information once a table gets this big.  It's a mess.
* **This structure is very inflexible**.  The law of mega-spreadsheets states that once you arbitrarily define n columns as the maximum number of instances a row could need, something will need n+1.
* **It has no sense of relationships**.  Athletes are one atomic unit here, but there are others.   You have countries, you have events (which belong to sports), you have results (which belong to events), you have athletes (which compete in events, have results in those events, and almost always belong to countries).  These relationships will probably be the basis for lots of interesting stories in your data, and the mega-spreadsheet does a poor job of accounting for them.
* **Analysis is difficult**.  How do you find all the athletes in the men's 100m dash?  Some of them might have their time in event1_result, some of them might have it in event2_result.  Have fun with those nested IF() statements!  And if any of this data is manually entered, there's a good chance you'll get textual inconsistencies between things like "Men's 100m," "Men's 100 meter," and "100m."

SQL lets you keep these things in a bunch of separate tables but use logical connections between them to smoothly treat them as one big set of data.  To combine tables like this, you use what are called JOINs.  In a single sentence: a JOIN takes two tables and overlaps them to connect rows from each table into a single row.  JOINs are beyond the scope of this primer, but they are one of the things that make databases great, so a brief example is in order. 

You might create a table for athletes with basic info like height & weight, a table for events with details about where and when it takes place and the current world record, a table for countries with information about each country, and a table for results where each row contains an athlete, an event, their result, and what medal they earned (if any).  Then you use joins to temporarily combine multiple tables in a query.

**Who won gold medals today?**

    SELECT
    athletes.name, athletes.country, event.name
    FROM athletes, results, events
    WHERE athletes.id = results.athlete_id AND event.id = results.event_id
    AND event.date = DATE(NOW()) AND results.medal = 'Gold'


**How many medals does each country have?**

    SELECT
    countries.name, COUNT(*)
    FROM athletes, countries, results, events
    WHERE athletes.id = results.athlete_id AND event.id = results.event_id AND athletes.country_id = countries.id
    AND results.medal IN ('Gold','Silver','Bronze')
    GROUP BY countries.id
    
---

## Questions/Comments/Suggestions ##
Noah Veltman  
Web: http://noahveltman.com  
Twitter: [@veltman](http://twitter.com/veltman)  
Email: [noah@noahveltman.com](mailto:noah@noahveltman.com)  