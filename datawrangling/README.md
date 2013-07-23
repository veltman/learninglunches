# Using code for data wrangling #



## Some common newsroom use cases ##

* **Turn complicated data into a table**, or something more table-like. XML and JSON are wonderful, flexible formats for representing all kinds of data.  But complex, nested data is often more challenging to analyze, especially in the exploratory phases of a project.  If you have five-levels-deep XML full of information you don't need, code is a great way to go through (a.k.a. "traverse" or "parse") nested data and pull out the few choice bits you care about.

* **Loop through a bunch of records and clean them**.  Maybe you need to pull out the text, but take out certain punctuation and the word "the".  Maybe you need to extract the ZIP code from a big generic address field.  Maybe you need to figure out what kinds of values are in a list, and fold the weird ones into other values.

* **Loop through a bunch of records and normalize some values**.  Maybe you need to adjust numbers for inflation.  Maybe you need to turn latitudes/longitudes expressed with degree symbols into decimal numbers.  Maybe you need to do different things to different rows, depending on some value.

* **Get distinct values, ranges, and averages at a glance**.  Are all 50 states represented?  Are there MORE than 50 states represented?  What's the earliest date in the table of school records for private schools in a specific county with average SAT scores between 1300 and 1500?

* **Do something with relationships between multiple tables**.  Maybe you have a table of senators, and a table of votes, and a table of bills, and you're looking for a particular voting pattern, by senator.

## A note on the "right way" to do this stuff ##

If you remember one thing, remember this: when it comes to code, there is more than one way to skin a cat.  Any one of the examples below could be done lots of different ways.  For nuts and bolts tasks like this, programming languages are almost totally interchangeable.  Even within one language, there may be a dozen perfectly valid approaches within the code.  Or maybe you'd rather load your data into a relational database and then poke around using SQL queries.  That's cool too.  And don't sleep on Excel.  Some jobs don't require anything beyond some sorting and an Excel formula and writing code is overkill.

JavaScript, Python, PHP, Ruby, use whatever works for you and ignore the haters.  The important thing is know your data, know what you're trying to do with it, and be mindful throughout.  Break things into steps that you can retrace.  Spot check your work.  Question your assumptions about the data (What's that?  You don't have any assumptions about your data?  Bullshit.).  Look out for common pitfalls, like blank values and numbers that aren't numbers.  READ the data.  [See this Learning Lunch on data hygiene](/datahygiene) for more suggestions.

## Examples ##

The examples below will generally use one of the following programming languages: Python, PHP, or JavaScript.  I'll throw in some SQL too.  I'll show some examples in multiple languages so you can see that they're much more similar than they are different, really more like dialects than separate languages.

If you want to play along at home and write some code, scroll to the end for tips on how to get set up on your local computer.

### Example #1: drone strikes ###
To do: PHP XML version
To do: JavaScript version

### Example #2: Pakistan votes ###
To do: JavaScript version

### Example #3: Citi bikes ###
To do: JavaScript version

### Example #4: Meat exports ###
To do: JavaScript version

### Example #5: UK unemployment ###
To do: Python version
To do: SQL version

### Example #6: Cats ###
To do: command line
To do: SQL
To do: JavaScript

### Example #7: Olympic athletes ###
To do: SQL

## If you want to play along at home ##

Simply getting to the point where you can write your first line of code can be a big barrier.  You often have to install or configure things on your local machine.  If you struggle to get up and running, trust me, it's not just you.  Ask your friendly neighborhood developer for help if possible.  The details of this installation process, and all the ways it may not work and drive you nuts, are beyond this primer, but if you want to have a go at some of this stuff yourself, here's the quick version for Windows/Mac OS X (if you're on Ubuntu Desktop I'm making the cautious assumption that you DON'T need any help in this department):

### Python ###
* On Windows, install Python using one of the [installers](http://www.python.org/download/releases/2.7.5/).  On Mac OS X, you've already got Python on your machine.  You may want to to [upgrade it](http://www.python.org/getit/mac/), though.
* Open a command prompt.  On Windows, do this by going to Start Menu->Run and typing in 'cmd', or just searching for 'command prompt'.  On a Mac, do this by going to Applications->Utilities->Terminal.
* Type `python` at the prompt and hit enter.  You're now in the Python interpreter!  You can put in Python code and it will run it for you.  

	```
	>>> 1+1
	2
	>>> print "Greetings, human."
	Greetings, human.
	```	

* Most likely you'll want to work using files instead of doing things one line at a time using this interpreter, although it is a fun way of feeling like you're in the movie *WarGames*.  To run Python code from a file (a "script") instead of opening the interpreter, use the command `python [filename]`.  For example:

`python cleanmydata.py`

If you aren't already in the same folder as your file, you'll need to either navigate to that folder at the command prompt using the `cd` command, or specify the exact location of the file, like:

`python /Users/joe/Documents/scripts/cleanmydata.py` (Mac)

`python C:\Users\joe\Documents\scripts\cleanmydata.py` (Windows)

### PHP ###
* On Windows, install [WAMP](http://www.wampserver.com/en/)
* On Mac OS X, install [MAMP](http://www.mamp.info/en/index.html)
* Use PHP from the command prompt/terminal much as you would use Python.  To open an interpreter, type `php -a`.  To run PHP code from a file (a "script"), use `php -f [filename]`.  

	```
	php > echo "Greetings, human.";
	Greetings, human.
	```

### SQL ###
* If you installed WAMP or MAMP (see above), it comes with MySQL built in.  For some more options for playing with SQL, read the end of [this Learning Lunch on Excel vs. databases](/databases]).

### JavaScript ###

The lack of any installation headaches is one of the reasons I love JavaScript.  Unlike the above three, you just need a web browser, no other installation required.  NB: I recommend Firefox or Chrome for this.

![Don't use IE](https://raw.github.com/veltman/learninglunches/master/datawrangling/images/badtime.jpg)

Go ahead, copy this code into a blank text file, save it as `test.html`, and then open the file in your web browser of choice (you can probably just drag the file's icon into your browser):

	<!DOCTYPE html>
	<body>
	</body>
	<script>

		var answer = "yes";

		//Set the content of <body>, which will show up in your browser
		document.body.innerHTML = "Does this work? " + answer;

	</script>

You should see something like this:

![JavaScript in browser](https://raw.github.com/veltman/learninglunches/master/datawrangling/images/js-in-browser.png)

JavaScript plays especially nicely with JSON data.  You can just paste it straight into your code and treat it as a variable.  Let's say you have a JSON data file that looks like this:

	{
		"state": "California",
		"cities": ["San Francisco","Oakland","Los Angeles","San Diego","Santa Barbara","Fresno","Sacramento","San Jose"]
	}

You can paste it into some code like so:

	<!DOCTYPE html>
	<body>
	</body>
	<script>

		//You need to assign your JSON data to a variable, like "data"
		var data = 	{
						"state": "California",
						"cities": ["San Francisco","Oakland","Los Angeles","San Diego","Santa Barbara","Fresno","Sacramento","San Jose"]
					};

		output = "";

		//For each city in the "cities" list, print a sentence
		for (var i = 0; i < data.cities.length; i++) {		

			//The <br/> tag adds a line break
			output = output + data.cities[i] + " is a city in " + data.state + " <br/>";

		}

		document.body.innerHTML = output;

	</script>

And then open that in your browser, and voila:

![JavaScript in browser](https://raw.github.com/veltman/learninglunches/master/datawrangling/images/js-in-browser-2.png)

There's a lot more to it than that, and plenty of things to watch out for (see some of the caveats above), but JavaScript is easier to get started with than most.  Another side benefit of doing stuff with JavaScript is that it's good practice for the other stuff you might want to do later, like create visualizations and interactive web apps.

### What should I use for doing [x]? ###

Remember that there are a lot of ways to skin any code cat.  The short answer is to use what you like and will get the job done.  Here are some general tips on why I might choose one technology for a particular task, but your mileage may vary a lot:

* **Python** is very useful for all kinds of meat-and-potatoes data work.  It's very flexible, and there are lots of add-on libraries that cover common data use cases.  If I'm doing something a little tricky and it's not for public consumption, just for data wrangling, I'll usually use Python.

* **PHP** can be a little bit messier than Python for behind-the-scenes coding, but it's more web-native.  If you want to write dynamic scripts that can be opened in a web browser, PHP makes that a lot easier than Python.  Once you're up and running (see below), you can save a `.php` file and open it in your browser much as you would open a `.html` file.

* **SQL** is great for forcing you to play by rules.  SQL-powered relational databases (I like MySQL, but there are other good ones) make you define structures for your tables.  You can't just be all loosey-goosey with your data like you can in Excel.  It can handle huge datasets without breaking a sweat.  Most helpfully, you can have lots of related datasets stored in different tables, and work on them as a group, using joins to link them up (Datas&eacute;ts:  Gotta catch em all!).  Sometimes I clean up data entirely with SQL, but just as often I combine it with Python or PHP.  I might write keep the data in a database, but write a Python script that pulls out each row, does something, and then updates that row back in the database.  For more on what SQL is good and bad at, read [this Learning Lunch on Excel vs. databases](/databases]).

* **JavaScript** can be run in your browser for smaller tasks.  No scary dark screen with a blinking cursor.  You can use Chrome and Firefox's consoles and inspectors for your data.  It plays really nicely with JSON, if that's what you're starting with or what you want to end up with.  It's easier to visualize things.  It runs into performance problems once your data gets big enough, though.

## Questions/Comments/Suggestions ##
Noah Veltman  
Web: http://noahveltman.com  
Twitter: [@veltman](http://twitter.com/veltman)  
Email: [noah@noahveltman.com](mailto:noah@noahveltman.com)  
