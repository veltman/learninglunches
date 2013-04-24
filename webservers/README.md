# How the web works, in several bad analogies #

Whenever you visit a webpage, you're interacting with a **web server**.  That interaction has a particular structure, called an HTTP transaction (that's where the `http://` at the beginning of a URL comes in), and it looks something like this:

### The request ###

The client (that's you, or, if you want to be precise, your web browser) sends a **request** to the server.  The request is usually pretty basic, asking to download a copy of a document the server has.  When you request:

`http://wikipedia.org/wiki/List_of_fictional_ducks`

You're sending a request to `wikipedia.org`, saying "Hey, can you send me whatever document is located at `/wiki/List_of_fictional_ducks`?"

Your browser will probably attach some extra **headers** along with its request saying things like "I'm using Firefox," or "Please send me stuff in English."  These headers are voluntary and controlled by the client.  Your browser includes them because they will usually help the server fulfill your request the way you like.  For example, if you're on a mobile device, and you send the header "Hey!  I'm a mobile browser!", the server might redirect you to http://m.wikipedia.org/ for a mobile-optimized layout.  You've probably been shunted off to a crappy, unzoomable mobile site when you'd rather get the desktop version, so you know that this well-meaning process can go wrong.  Headers can be used for good or evil.

### The response ###

The server is listening for incoming requests, and when it receives yours, it decides how to fulfill it, and then sends a **response** back.  That response hopefully includes the document you asked for, but it might instead be something like the dreaded 404 error, which is server-speak for "I couldn't find that document."

The server is acting like a librarian - you come in with a dewey decimal number on a slip of paper, and the librarian goes into the back room, looks for the book, and brings it back for you.

The server can also send back **headers** that specify helpful things like "Don't cache this document," or "This document hasn't been updated since last month."  As with request headers, these are voluntary, controlled by the server, and can be used for good or evil.

### Statelessness ###

One of the key takeaways is that HTTP is a protocol for **stateless** transactions.  When you are using a modern website, jumping from page to page it feels like you have a deep relationship and like Amazon really cares about you as a person, your hopes and dreams, and you guys are like, totally on the same page about how many kids you want.  This is largely an illusion.  You're really just having a lot of one-night stands.  You don't have some complex connection to the server, only a series of one-and-done transactions.

Stateless means that **any two HTTP requests are basically independent**.  You ask for a document, you get one back.  Request, response.  Rinse, repeat.  When you load a complex page with a lot of images and other embedded content, you're making a dozen separate HTTP requests, one for the main page, and one for each other piece of content your browser wants to display in it.  And each time you walk up like a stranger; the server doesn't intrinsically know that it's still you, the person who was just looking at `http://wikipedia.org/wiki/List_of_fictional_ducks`, who is now requesting `http://en.wikipedia.org/wiki/Sir_Swamphole_McDuck`.

But clearly web servers DO figure out who you are and keep track.  How do they do that?  With a whole bag of tricks.  One is the `Referer` header, which your browser will send with most request to tell the server where you just were.  When you click on a link in Google search results, the `Referer` header tells the new server that you just came from those search results.

Another way is the use of **cookies**.  Cookies are small files that a server leaves on your browser as an identifying marker, like a colored wristband at a festival.  Your browser keeps these cookies, and when you make future requests of the same server, it attaches the cookies with its request so the server can use that information.  Cookies can be **session cookies**, that disappear when you close your browser, or they can have a specified lifespan.  When you check the "Remember Me" box on a login form, you're opting for a cookie with a longer life.  Cookies can be a little creepy, since they are used by advertisers and others to gather lots of data about your browsing behavior.  But they are also a pretty standard mechanism for a server to keep track of who you are.  Without them, you'd have to log back in to Facebook every time you tried to view a new page.  That would suck.

Note that like so much of how HTTP transactions work, cookies are voluntary.  You and your browser are in the driver's seat.  You can delete them or manipulate them.  They're also browser-specific, so they don't carry over if you switch from Firefox to Chrome.

### A static server ("the vending machine") ###

In the early days of the web, HTTP was mostly used for retrieving static (unchanging) documents.  The documents were mostly text, which is probably for the best since loading a YouTube video on a 28.8k modem would take quite a while.  Servers have learned new tricks since then, but this basic concept, where a server is holding a bunch of actual files and all it does is retrieve copies for people who ask, still underpins a lot of the web.

You've probably noticed that the part of a URL after a domain name looks an awful lot like a path to a file on your local computer.  That's because in the early days of the web, it was just the path to a file on someone else's computer that was behaving as a server.

When you asked for:

    http://www.joe.com/images/launchpad_mcquack.jpg

Joe was running a server with a folder called `images` with a file called `launchpad_mcquack.jpg` in it.

Serving static files is great because it's very efficient.  It's not great because you can't tailor the content for the situation or the person asking for it.

### A dynamic server ("the restaurant") ###

Nowadays a lot of web content is served **dynamically**, which means that instead of having a file ready to go for a particular URL, the server writes it on the fly.  This is more like a restaurant, where you order a dish, the server (see what I did there?) takes your order into the kitchen, and then it gets prepared behind closed doors and brought back out to you.

Like a restaurant, there is a spectrum of static to dynamic.  You might be making everything from scratch after the order comes in, assembling the response with whatever is in season, adjusting it for that customer's particular tastes.  Or you might be a fast food joint, with some pre-made assets (frozen patties) that you microwave for 30 seconds.  You're still doing some work, but not as much.  Doing things dynamically on a server is similar, in that the more dynamic your response is, the more freedom you have to customize what leaves the kitchen, but the slower and more resource-intensive it is to prepare, and the more ways things can go wrong.  When a web server becomes unresponsive because of too much traffic, that's the dinner rush.  Only so many cooks, only so many stovetops, too many orders coming in.

Servers do a lot of things dynamically these days, because the web is a lot more personal.  Servers need to tailor content so that the same URL returns different results for different people and different situations.  `http://www.gmail.com/` needs to know that you want your email, not someone else's.

When people talk about **server-side programming**, they are talking about rules for what happens in the kitchen.  They are talking about ways to link up a URL request that comes in with whatever other ingredients the server has (e.g., a database of users and user-specific content) in order to cook up some piping-hot content to send back out.

#### Sidebar: URLS ####

Note that with a dynamic server, the actual files don't exist at the location the URL describes, so URLs play a different role.  Instead of specifying the location of a file, they're a way of supplying extra data with the request.  When you visit:

`http://noahveltman.com/books/page/8/`

There is no folder called 'books', let alone a subfolder called 'page'.  Instead, a catch-all operator takes the request, and interprets the URL as "get one page worth of book reviews from the database, skip the first 7 pages, and then print them out as a webpage."  Lots of URLs work like this now, not referring to a file or folder that exists, but merely supplying data about what you want.  There are other ways to supply this data, like a cookie, but URLs are a great way to do this because:

* They're easy to *save* for later.
* They're easy to *share*.
* They're *persistent*, or at least they should be (see also: [Cool URIs don't change](http://www.w3.org/Provider/Style/URI.html)).  The content on a page may be dynamic, but what it refers to shouldn't change.  So if I add new book reviews, you'll get different results on page 8, but the URL still refers to whatever is currently the 8th page of reviews.

URLs are an important part of the web, and an important design feature for a site.  When you have a dynamic server, you have total freedom to make any URL point to whatever content you want.  Use it wisely.  See also: [URLs are for people, not computers](http://www.not-implemented.com/urls-are-for-people-not-computers/)

### The client side ("playing with your food") ###

The term **client side** means things that happen in your browser after the HTTP transaction is done.  A server might just send you some plain text or basic HTML, but a lot of web documents also include some JavaScript code.

Your browser renders HTML as what's called the Document Object Model (DOM).  Don't worry about the details.  Just think: a webpage is a document full of objects (images, videos, tables, buttons, text, etc.).  JavaScript is a language that speaks DOM.  It can change the color of an object, change the text in it, delete it, move it, and so on.  It can make your webpage come alive.  Without it, a webpage isn't much more interactive than a printout on paper.

Recall that an HTTP transaction is one-and-done.  Once the server has sent the sealed envelope with the document you asked for, it's gone.  In order to change that page without making a new request to the server, you need JavaScript.  In the restaurant analogy, this is changing your food on the table, without the cooks.  You don't have access to the kitchen or any of its supplies, you only have access to what's on the table.  Sorry, I'll stop with the restaurant analogy.

So you would want to do things on the client side **because it involves modifying the contents of the page being displayed, or because it involves something the user does**.

Some examples of "client-side" programming include:

* Making it so that clicking a button changes the page.
* Making things change their appearance when you hover the mouse over them.
* Allowing you to drag things all around the page.
* Setting a timer so that something happens 5 seconds later.

Doing things on the client side is nice because you can modify the page the way the user experiences it, and without loading the entire page over again for every single change.  It's less nice because sometimes you need access to the kitchen.  If you are powering your app with data, your server needs to do some work and reference that data in order to prepare the content at a given URL.  A user's browser doesn't have access to the guts of your server, it can't just tap into the database whenever it feels like it.  It has to communicate through page requests.

### AJAX: a little bit of client side, a little bit of server side (no food analogy available) ###

When I just told you that the client has no way to keep talking to the server after its HTTP request, that it's a one-night stand, I sort of lied.  It's literally true, but through a clever trick called AJAX (**A**synchronous **J**avaScript **A**nd **X**ML) a webpage can have an ongoing conversation with a server and behave more like a desktop application, submitting and receiving new data while staying on the same page.

AJAX works by allowing your browser to **make additional HTTP requests in the background without leaving a URL**.  It does this using JavaScript (the J) and it does it after the server has responded with your initial document (asynchronously, the A).  Don't worry about the X.

In this way you can load CNN at its standard URL:

    http://www.cnn.com/

And then that page contains JavaScript that tells your browser to silently request another URL every 30 seconds:

    http://www.cnn.com/current_headline

That same JavaScript also instructs your browser on what to do with the response.  It might, for example, compare the response to the headline you're looking at, and, if it's different, update the page.

If you are using a webpage and things are happening that seem to involve a server somewhere, but the URL isn't changing, that's AJAX at work.

AJAX is nice because it allows for a desktop app-like experience instead of the natural document-hopping experience of the web.  It's also nice because, if a given page involves large files for certain part, you don't want to make the user wait for all of that content before the user sees anything (that would be _synchronous_).

Let's say you have a news article, and in the middle of the article you have an interactive graphic that requires the download of a 5 MB data file.  In the pre-AJAX world, you'd have to include that as part of the initial download of the page.  This makes the whole thing slow to load, and causes problems if, for example, the user is on a phone and loses their data connection partway through.  In the AJAX world, you can load the rest of the article so a user can start reading it, and in the background, you can start downloading the data (_asynchronously_), displaying a loading animation in place of the graphic until the data is done downloading.  As AJAX becomes more and more common, we increasingly live in an era of modular webpages full of loading animations.

AJAX has some downsides:

* **Different page states don't have their own URLs**.  As stated above, URLs are a great feature of the web.  The more you use AJAX to play with the state of a page without changing the URL, the more you break this.  The refresh and back buttons will not behave the way they're expected to.  If you load a URL that then changes a bunch of things dynamically using AJAX, when you share that URL later, you're sharing something different than what you're looking at.  The next person will see the initial state of the page instead.  Sometimes this is desirable (you don't want a URL that goes straight to a confirmation message after someone has submitted an order), but sometime's it isn't.  There are some workarounds for these problems, but they aren't pretty (the internet: it's workarounds all the way down!).  Think carefully about when you want your URLs to change and when you don't.
* It introduces additional **design complexity**.  Because you have these asynchronous, invisible requests happening, you have to make sure to design the page to work smoothly no matter what the status.  Those requests could fail, or your user could lose their internet connection while looking at the page on their phone.  This means lots of fallbacks, loading indicators, and that sort of thing.
* It can affect **crawlability of your content**.  Search engine web crawlers have different levels of ability to crawl content delivered via AJAX, depending on the details.  For sites that care about inbound traffic from search engine (that would be... all of them?), this can be a consideration in whether/how to use AJAX.

---

## Some implications ##

Now that we've gone over the basic bones of pageviews, we can talk about some of the implications for how to get content to users the way you want.

### Browser detection and feature detection ###

It's often useful for a web server to know what browser you're using.  One way it can determine this is with a `User-Agent` header that most browsers will send as part of a request.  It tells a server what operating system and browser you're using in a very confusing format like this:

    User-Agent: Mozilla/5.0 (Windows NT 6.1; WOW64; rv:20.0) Gecko/20100101 Firefox/20.0

Detecting a user's browser this way is sometimes called "browser sniffing."  It allows a server to track browser statistics ("Do I still need to support IE8?"), but it also allows you to tailor your content differently for different situations.  You can send mobile devices mobile-optimized content, or you can send older browsers Flash instead of a `<video>` element.

Often when you think you need browser detection, what you really need is feature detection.  Does the device have a touchscreen?  Does the browser support SVG graphics?  Does the browser support drawing shadows behind elements?  You can try to deduce this by detecting the browser, and then comparing it to what you know about whether that browser supports that feature.  Alternatively, you can use JavaScript to test for certain features after the page has loaded.

Either way, browser and feature detection is a messy process.  The `User-Agent` information is not precisely standardized, and new browsers, devices, and versions are constantly popping up.  New features are also popping up all the time, and different browsers often come up vendor-specific features.  Support varies widely, especially for newer or more obscure features.  Knowing what browsers your site traffic usually comes from (see above) is a key component in making decisions about how much time to invest in cutting-edge features or fallbacks for people on older browsers.

For a journey into the heart of browser support darkness, check out: http://caniuse.com/

One JavaScript library that can be used to detect support of certain features is Modernizr: http://modernizr.com/

### Responsive design ###

One particular "feature" that matters a lot for how your webpage looks is simple: how big is the screen (a.k.a. the **viewport**)?

A lot of sites have traditionally gone with a "fixed width" layout, where they pick a reasonable width for the content that will fit on most desktop/laptop screens, and then they paint everything in that box with total precision.  If you're on a smaller screen, or you shrink the window, tough luck, the content doesn't fit on your screen.  If you're on a bigger screen, a lot of the real estate is wasted.

When smart phones came along, a lot of sites starting creating mobile versions of their desktop sites, slimmed down versions that were meant to be viewed on a small screen without a mouse.  They would use server-side browser detection and create a different document to send back, depending on the browser.  This approach allows you to optimize precisely for any given device, but it's also brittle.  You're inevitably playing catch-up.  New devices and new operating systems are coming out all the time, and just because someone downloads your content on one device doesn't mean that's how they're viewing it.  It also handcuffs your users.  You've probably been shunted off to a mobile site before and found that the mobile-"optimized" site is missing the one thing you came for.

A philosophy that's gaining ground these days is **responsive design**.  This school of thought says that trying to control every little pixel on every screen of every device, and splitting our site into lots of special versions, is a losing battle, especially as the universe of web-enabled devices gets ever more diverse.  We should instead create something fluid that adapts to whatever screen it's on (this is how the web used to work until we broke it with our cleverness).

For example, you might have a gallery of thumbnail images that are each 100 pixels wide, but instead of just staking out 800 pixels of width and displaying them 8 across, you can instead tell the browser to just display as many as it can per row.  A narrow browser will display more rows, fewer per row, instead of making you scroll horizontally (boo, horizontal scrolling!), and you don't have to create a whole extra site for mobile.

Responsive design is an elegant approach with a lot of advantages - you can have a single set of HTML and styling for your page, and you can ensure that the experience doesn't break down just because someone uses a device you didn't expect (including, maybe, a device that didn't exist when you created the site).  But figuring out how to make everything flow the way you want for any screen size can be a complex puzzle to solve.  Painting is hard when you don't know the size of the canvas.  This is both a downside and an upside, insofar as it forces you to think critically about what matters on your site and what doesn't.

Responsive design is not a panacea.  In some cases visitors on tiny screens want drastically different things than visitors on big screens, and trying to treate the layout as a fluid continuum doesn't make sense.  Responsiveness is best suited for situations where users on different devices are using your site in broadly similar ways, like, say, reading news articles.  As always, the lesson is: know your users.

A concept related to responsive design is **progressive enhancement**, that you should make sure that the most basic version of a page works, and then progressively add content and functionality for browsers than support it.

### Browser caching ###

Most content on the web doesn't change that often.  Loading the same thing over and over, especially something like a large image, is bad news for everyone.  It uses server resources, it uses bandwidth and/or data allowance, and it makes everything load slowly.  Most modern browsers cache files over time and, when you make a request to a URL you've visited before, decide whether you need to download it over again or you can just use the cached version instead.

Caching is tricky.  A server can send HTTP headers like `Cache-Control`, `Expires`, and `Last-Modified` with a response, which specify whether to cache a document, how long to consider this version current, and when the last time it was changed, respectively.  But this doesn't always work.  Some browsers don't handle those headers well, especially older ones.  Some servers don't supply the right headers.  Some proxy servers, middlemen for web traffic, strip them out.  Overcaching is bad because a user sees the wrong content - yesterday's news instead of today's news - and it can sometimes screw up your analytics.  Undercaching is bad because it's a waste of resources.

### Server side caching ###

Remember the server side?  One approach some newsrooms use now is caching things on their own server.  In this scheme, instead of taking in each request, looking up a bunch of stuff in the database, and then assembling a custom response, the server can take a different approach, either:

* cache every unique request the first time it receives it, so that every other person who asks for the same thing gets a pre-made document.
* proactively figure out every possible request you might have to respond to, and then "bake" flat files so that they all point to pre-made documents.  If we go back to the kitchen analogy, this is like pre-making all the different orders you expect and then sticking them under the warming lamps so they're ready immediately.

Either of these strategies reduces the realtime strain on a server, since users are just getting static documents back.  But it requires careful planning to figure out what needs to stay dynamic and what doesn't, and how to update the cache when the data that went into it changes.

## Taking the red pill ##

If you want to take a look under the hood of your own browsing to understand just what real-world HTTP transactions look like, download a web proxy like Fiddler:  
http://fiddler2.com/

It's free, and if you run it while you browse the web, it will show you what requests your browser is actually sending, and what responses it's actually receiving.  The format of these requests is a little stiff and formal, but it's all plain text.  It's like an old-timey telegram (`SEND HISTORY OF CLAN MCDUCK. STOP.`) Poke around and see what you find!

![Fiddler example](https://raw.github.com/veltman/learninglunches/master/webservers/images/fiddler.png)

---

## Questions/Comments/Suggestions ##
Noah Veltman  
Web: http://noahveltman.com  
Twitter: [@veltman](http://twitter.com/veltman)  
Email: [noah@noahveltman.com](mailto:noah@noahveltman.com)  