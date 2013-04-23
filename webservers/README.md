# How a web server works, in several bad analogies #

## What is a web server? ##

A **server** gets its name from the fact that it lives to serve.  It is a computer system that is available to do things you ask it to do.  You can ask a server to do things like:

* Hold on to some files for you (Dropbox)
* Look something up for you (Wikipedia)
* Call someone else and connect you (Skype)
* Do math for you (Wolfram Alpha)

In the old days, a server was usually a big physical computer sitting in a room somewhere.  These days, almost anything can be a server.  A wristwatch can be a server.  Several computers can team up to act as a single server, like three people under different parts of the dragon float in a parade. A single computer can act as lots of servers, like a single PO Box that represents hundreds of Cayman Island corporations.

Ultimately a server is just any computer or part of a computer that can listen for **requests** from a client and do something with them.  Usually a server sends back a **response** once it's finished with the request.  If the request is "send me back some information," the response would include that information, but sometimes the response is just a confirmation message or, if it didn't like your tone, a rejection message.

When you request something from the server, you're the **client**.  Keep in mind that "server" and "client" are just *roles* for a particular interaction, they can change.  When server A asks server B for a file, server A has become the client.

If a ship captain calls up to the lookout in the crow's nest asking if there any icebergs ahead, the captain is a client and the lookout is a server.  The lookout takes in a **request**, does something with it (in this case, looks through his binoculars), and sends back a **response** ("all clear!" or "iceberg, dead ahead!").

A **web server** is a particular flavor of server that listens for certain types of requests and provides certain types of responses.  It's a system for easily looking up documents from other computers that are willing to give them to you.  This is where that annoying `http://` at the start of every web address comes in.  HTTP stands for **H**yper**t**ext **T**ransfer **P**rotocol.  There are lots of other protocols, but for our purposes all we care about is HTTP.

When interacting with a web server, the actual "client" is usually your web browser, and you're just guiding it.  If you hear "client" in the context of the web, think browser.  A web browser plays two main roles:

1. A rendering engine that takes raw HTML and JavaScript code and makes them into pretty, clickable webpages for your viewing pleasure
2. A terminal that formats your URLs and other interactions into formal HTTP requests that a server will understand, like a telegraph operator in the Wild West (more on this soon).

## Anatomy of a pageview ##

If you try to visit `http://wikipedia.org/wiki/List_of_fictional_ducks` in Firefox, the basic transaction looks like this (we'll go into more detail on each of these in a moment):

[Firefox looks up the phone number for `wikipedia.org` in the DNS yellow pages, then dials 555-WIKI]  
**Wikipedia:** Hello?  
**Firefox:** Hey, it's Firefox, I'm calling for `wikipedia.org`.  I was hoping it could send me a copy of `wiki/List_of_fictional_ducks`.  
**Wikipedia:** Sure.  It's an HTML file, the last time it was updated was on Monday.  Here you go...  
[Wikipedia starts reciting the list of fictional ducks for you]  

### Step 1: URLs ###

URLs are a standard format for finding the right server and then asking it for some content.  A URL contains two key pieces: the **domain name** and the **path**.  

In `http://wikipedia.org/wiki/List_of_fictional_ducks`,

`wikipedia.org` is the **domain name**.
`/wiki/List_of_fictional_ducks` is the **path**.

The domain name is what you look up in the DNS yellow pages to figure out who to call with your request.  Don't worry about the details of how DNS works, as long as you understand that you look up a domain name and it tells you the actual digital address (IP address) for the server that handles requests for that domain name.

Once you've found the right server to call with your request, you contact that server and send it the path you wanted.  The server looks at the path and decides what to do with it.

You may notice that the path portion of a URL sometimes looks an awful lot like a file path on your own computer.  That's because, in the early days of the web, when you called up a server asking for `/files/images/launchpad_mcquack.jpg`, you were probably asking for an actual file stored in that location on the server.  Some servers still behave this way, but others treat the URL in some other way and store their files in a different structure hidden from view.

### Step 2: an HTTP request ###

Once you've successfully connected to wikipedia.org, you request the path you wanted.  You specify a **method** for the request.  There are lots of methods.  The most common method is `GET`, which just says, "I want to get whatever is at this URL, no funny business."  Later we'll talk a little bit about `POST`, which is a method for submitting forms and other hidden data along with your request.  The actual HTTP request is just plain text with a particular syntax:

    GET /wiki/List_of_fictional_ducks HTTP/1.1
    
That's it!  That's all your browser (the client) needs to send to the server to get back a response.  That's so short you could tweet it with room to spare.

Of course, in the noblest traditions of the web, we like to take beautifully simple technologies and complicate them to meet new needs until we look down and we have an unrecognizable Frankentechnology.  In addition to this request, your browser will send any number of **headers** along with the request.  Headers are basic name-value pairs that specify extra options or restrictions for an HTTP transaction.  Here are some examples of common headers:

* `Host: wikipedia.org` means "this request is for wikipedia.org."  This is necessary in the modern era because a given server might handle incoming requests for lots of different domain names.  It needs to know that you want `wikipedia.org/wiki/List_of_fictional_ducks` and not `okcupid.com/wiki/List_of_fictional_ducks`.
* `Referer: http://www.google.com/search?query=darkwing` means "I got here from a Google search for 'darkwing'."  The `Referer` header helps a server understand where its traffic is coming from.  Sound like something a newsroom might care about?
* `User-Agent: Mozilla/5.0 (Windows NT 6.1; WOW64; rv:20.0) Gecko/20100101 Firefox/20.0` means "I'm using the Firefox 20.0 browser on Windows."  This helps you understand what browsers your visitors are using, and, if you choose to, serve them different content based on their browsers.  When you get redirected to an http://m.site.com mobile site on your phone, it's usually because the server detected that your `User-Agent` header was describing a mobile browser and rerouted you.

So, with the headers, our full request actually looks like:

    GET /wiki/List_of_fictional_ducks HTTP/1.1
    Host: wikipedia.org
    Referer: http://www.google.com/search?q=darkwing
    User-Agent: Mozilla/5.0 (Windows NT 6.1; WOW64; rv:20.0) Gecko/20100101 Firefox/20.0

Headers are just extra information that your browser sends with its request to help it get what it wants.  They are controlled by the client (that's you!).  They can be manipulated.  You can, for example, masquerade as a mobile browser on a desktop computer.

Wikipedia offers a long list of some other HTTP request headers for your perusal, ranging from somewhat common to very obscure:  
http://en.wikipedia.org/wiki/List_of_HTTP_header_fields#Requests

### Step 3: an HTTP response ###

OK, so your browser (the client) has sent a nicely-formatted request to the wikipedia.org server, and it has also used headers to identify what browser it is, how it got to this page, and maybe some other things like which languages it prefers.

The server takes in that request, and then it can do whatever it wants.  It is free to reject the request, or ignore it completely.  More likely, if it's a friendly web server, it will take that request, go into the back room, do whatever it needs to do to satisfy your request, and then you send you back a response.

A response looks quite similar to a request, but instead of starting by asking for a path, it starts with a **status code** that tells you what kind of response you're getting:

    HTTP/1.1 200 OK

This means, "Your request is OK, I'll give you what you asked for."  There are lots of other status codes, like:

* `200 OK`: Your request is OK.
* `301 Moved Permanently`: You asked for something that lives somewhere else now.  The equivalent of the post office change of address form.
* `403 Forbidden`: The thing you asked for exists, but you're not allowed to see it.
* `404 Not Found`: I can't find what you asked for, I don't think it exists. (You've probably seen this error before)
* `503 Service unavailable`: The server is having some issues right now, maybe due to an overload of traffic.  Try again later.  The Twitter "fail whale" is a marine mammal mascot for this status code.

As usual, Wikipedia has the full list:  
http://en.wikipedia.org/wiki/List_of_HTTP_status_codes

HTTP responses have optional extra headers, just like requests do.  Examples include:

* `Content-Type: text/html` means "This is an HTML document."  When your browser knows whether to display something as a web page or download it as a different kind of file, this is usually why.  For example, `Content-Type: application/pdf` would tell it to open it as a PDF file instead.
* `Last-Modified: Tue, 16 Apr 2013 12:20:41 GMT` means "This file hasn't changed since April 16, 2013, in case you want to use a cached version instead."
* `Cache-Control: no-cache` means "Hey, don't cache this file.  If they ask for the same URL later, come get a new version.  Seriously, don't cache it.  No, really, don't.  Please?"  Dealing with caching is hard.  We'll talk more about it later.

Wikipedia has a longer list of HTTP response headers:  
http://en.wikipedia.org/wiki/List_of_HTTP_header_fields#Responses

In the same way that request headers are controlled by the client, the response headers are controlled by the server.  They too can be manipulated.  There's nothing to stop a server from sending you an image and telling you to open it in Excel, or from telling you something was changed yesterday when it hasn't been changed since 2006, or from telling you something is in Greek when it's actually in English.

Once the server has sent back a response code and probably some headers, it will typically also send back a message that consists of the actual content you wanted in the first place.  In this respect, all the header nonsense is like the addresses at the top of a formal business letter that follow a nitpicky formatting scheme that you probably had to learn in fifth grade.

    HTTP/1.1 200 OK
    Content-Type: text/html

    <html>
    <body>
        Browser, I am in you!
    </body>
    </html>

If you put on a server costume and sent those 8 lines into a browser, it would display a webpage for you with that message.

### Round-up ###

That's it! A simple series of transactions from client to server: request-response, request-response, request-response.  This is (pretty much) all the web is made of.  The fact that out of this humble premise we have Amazon, Gmail, eBay, and billions of other complex websites, all at our fingertips, is nothing short of magical.

## What does it mean? ##

### What browsers and servers know about each other, and what they don't ###

HTTP is a **stateless protocol**.  This means that any two requests are treated as basically independent.  When you load:

http://en.wikipedia.org/wiki/List_of_fictional_ducks

and then click through to:

http://en.wikipedia.org/wiki/Sir_Swamphole_McDuck

A server doesn't know that those requests are connected, it has to figure that out on its own somehow.  It can figure out where you just came from as long as your browser supplies the `Referer:` header, but that only keeps track of your last stop.  It can identify you based on IP address, but that's not a reliable measure, especially on shared computers.  In order to keep track of who you are and your entire **session** on the site, it needs to mark you somehow.  This is usually done with a **cookie**.

#### Cookies ####

Cookies are an extra ingredient in HTTP.  They allow a server to give your browser a little bit of extra information to hold onto for it to reference later.  Cookies are requested and created during an HTTP transaction using headers, just like the other added functionality of HTTP.

When it sends you back a page, a server might also send back the header:

    Set-Cookie: visitorId=1098

This means "Hey, Firefox, please store a cookie for this domain name that you're visitor ID 1098.  When you make requests for this domain name later, send me back the cookie to as a reminder, I have a terrible memory."

Next time you make a request from that server, you will preemptively show the server all of its cookies with your request:

    Cookie: visitorId=1098

This is your browser telling the server "When you send me back the content I'm requesting, keep in mind that I'm visitor ID 1098."

In this way, servers can connect the dots between your different page requests so it doesn't treat you like a stranger every time.  Without this method, you'd have to log back in to a site every time you visited a new page.  That would suck.

Cookies are **browser-specific**, meaning if you switch from Firefox to Chrome on your computer, you have a completely different cookie jar, and never the two shall mix.

When cookies are created, they can also be given a lifespan.  **Session cookies** are supposed to disappear when you close your browser - this is like a parking enforcement officer who chalks your tire so that, when he comes back two hours later, he knows it's the same car.  If you drive away and come back, the chalk will be gone.  But cookies can also last for longer periods.  When you check the "Remember Me" box when logging in somewhere, that's usually setting a cookie lifespan of a few weeks or months in the future instead of a short-lived session cookie.

You can also track a user by other methods, like IP address.  This can be useful because, when it comes to cookies, the user is in control.  If you are running an online poll, you don't want someone to be able to stuff the ballot box by repeatedly clearing their cookies or switching browsers.

#### Browser detection ####

Along with figuring out who exactly you are, it's often useful for a web server to know what browser you're using.  If you recall the `User-Agent` header above, you know that most web browsers will send a long piece of impenetrable gibberish identifying their browser version whenever they contact a server.  Servers can then parse that information and try to figure out what browser, version, and operating system you're using.  This allows a server to track browser statistics ("Do I still need to support IE8?"), but it also allows you to tailor your content differently for different situations.  You can send mobile devices mobile-optimized content, or you can send older browsers Flash instead of a `<video>` element.

This browser detection is a messy process.  The `User-Agent` information is not precisely standardized, and new browsers, devices, and versions are constantly popping up.  In order to do this well, you need to rely on some sort of reference list of which `User-Agent`s correspond to which real-world scenarios, and that list will almost always be a step or two behind the times.

#### Voluntary cooperation ####

One of the themes here is information changing hands in an HTTP transaction is generally voluntary; the client has total control over what it sends to the server and vice versa.  A web browser and server use the lingua franca of HTTP and generally respect each other's headers because they're usually on the same team.  When your browser sents a request header that says it is a mobile device, it does so because it assumes servers may need that information to better tailor content for it.  When a server sends response headers, it does so because it assumes the client can use that extra information to handle the content better.

To the extent that you don't want to tell a server what browser you're using, or what cookies you have, you can withhold it.  To the extent that a server doesn't want to tell you what file type it's giving you, or what language it's in, it can withhold it.  Each side can also lie and send headers that aren't true.  This power can be used for good or evil.

### "Client side" vs. "Server side" ###

When it comes to developing things for the web, you will hear the terms "client side" and "server side" a lot.  Now that we've talked about the roles of client and server, we can be a little more precise about what this means.

**Server side** means things that happen in the server, often after it receives an HTTP request but before it sends back a response.  When a server takes in a request, and goes into the back room to get what you asked for, the "server side" is the back room.  If it's looking up who you are in a database, or changing what it's about to show you based on your browser, or inserting the latest headlines into a webpage, that all happens on the server side. If your content is mostly static, not dynamic, meaning that the content at a particular URL doesn't change, then there isn't much server side programming involved; the server is just pulling a file off a shelf.

**Client side** means things that happen in the client after an HTTP transaction is done.  This typically refers to JavaScript code that your browser gets as part of a webpage and then executes locally.  Doing things on the client side when possible is nice because it lets you freeload off the client's computing resources and allows you to modify the page they're viewing on the fly without additional page requests.  It's less nice because you don't control the environment and you're very limited in what you can do.

### AJAX: a little bit of client, a little bit of server ####

AJAX (**A**synchronous **J**avaScript **A**nd **X**ML) is a way of getting around the one-and-done transactional nature of a pageview.  It allows your browser to make additional HTTP requests in the background without leaving a webpage.

In the old days, if you wanted to, say, get the latest headline on a news website, you had no choice but to refresh the entire page.  This sucks because it requires you to reload everything when all you wanted was to update one little bit of text.  It also sucks because you might want to preserve other aspects of the page you're looking at, like forms you've filled in, and some browsers discard those when you reload a page.

AJAX solves this problem by letting you initiate what is basically a ghost HTTP request in the background and then do something with the response, all using JavaScript.

In this way you can load CNN at its standard URL:

    http://www.cnn.com/

And then that page can contain JavaScript that silently requests another URL every 30 seconds:

    http://www.cnn.com/current_headline

When it gets a response for that URL, it can see whether the headline has changed, and then update the page you're still looking at.  This all happens without you lifting a finger or seeing it in action.

Doing things via AJAX is also nice if you have large files to process.  News apps will often have a big data file powering the app, and rather than require the user to download it upfront, it may make more sense to download it AFTER the page has loaded using AJAX.  That way, the user can start reading the rest of the page, and then when the file is done downloading, the loading animation can be replaced with the new content on the fly.  If you load that all up front, the user has to wait for everything to come in order before they can load the page.  This is where the "asynchronous" part comes in.  You send a request and wait for the response, but you can do other things in the meantime.

Using AJAX instead of multiple pageviews is an elegant solution to creating a web experience that's more like a desktop app.  Instead of having to look at a series of static pages, a single page can send and receive data in the background to update on the fly.  AJAX also has some important downsides:

* **Different page states don't have their own URLs**.  URLs are a great feature of the web.  By giving pages a consistent locator, they become easy to share, bookmark, and revisit.  They make a browser history possible, so that you can navigate with the back and forward buttons.  The more you use AJAX to play with the state of a page without changing the URL, the more you break this.  The refresh and back buttons will not behave the way they're expected to.  If you load a URL that then changes a bunch of things dynamically using AJAX, when you share that URL later, you're sharing something different than what you're looking at.  The next person will see the initial state of the page instead.  There are some workarounds for these problems, but they aren't pretty (the internet: it's workarounds all the way down!).
* It introduces additional **design complexity**.  Because you have these asynchronous, invisible requests happening, you have to make sure to design the page to work smoothly no matter what the status.  Those requests could fail, or your user could lose their internet connection while looking at the page on their phone.  This means lots of fallbacks, loading indicators, and that sort of thing.

### GET vs. POST ###

`GET` is the basic HTTP method.  It's a one-way transaction.  You ask for something, you get it back.  Your browser only sends the server the URL it wants and standard headers.  You can, however, send custom data as part of your `GET` request by including it in the URL:

`http://www.google.com/search?query=ducktales+theme+song&language=english`

The URL you're requesting is `http://www.google.com/search` and everything after the `?` is a series of variables.  These variables follow the format:

`?name=value&name=value&name=value...`

This is helpful because sometimes you need to pass information to the server besides just the URL, like when you're filling out a form.  But you don't always want the information you're passing to be part of the URL, out in the open.  And you don't always want a user to be able to re-submit information by requesting that URL again.

This is where `POST` comes in.  `POST` is another method for requesting a URL and passing some custom data along with it.  The only difference, an important one, is that instead of tacking it on to the end of the URL, it sends those variables as an extra message in the request.  When you fill in your information on Amazon, that information is getting sent to the next page using `POST` instead of `GET`.  As a general rule, `GET` should only be used for submitting custom data if:

* It's OK for the same data to be submitted repeatedly.
* The data isn't sensitive.

These two methods are the most common, but there are lots more:  
http://en.wikipedia.org/wiki/Hypertext_Transfer_Protocol#Request_methods

### URLs ###

URLs are important.  They are the way anything gets found on the web, and they're still the main way people save and share things.  Selecting meaningful, well-structured URLs is valuable, and leaving URLs alone is valuable too.  Moving things around breaks the web.  Some useful further reading on what makes a good URL:

URLs are for people, not computers:  
http://www.not-implemented.com/urls-are-for-people-not-computers/

Cool URIs don't change: 
http://www.w3.org/Provider/Style/URI.html

### Caching ###

Caching is important, because most content on the web doesn't change that often.  Loading the same thing over and over, especially something like a large image, is bad news for everyone.  It uses server resources, it uses your bandwidth and/or data allowance, and it makes everything load slowly.  Most modern browsers cache files over time and, when you make a request to a URL you've visited before, decide whether you need to download it over again or you can just use the cached version instead.

A server can help avoid under- or overcaching by sending the right headers with its responses.  Headers like `Cache-Control`, `Expires`, and `Last-Modified`, specify whether to cache a file, how long to consider this version current, and when the last time it was changed, respectively.

---

## Questions/Comments/Suggestions ##
Noah Veltman  
Web: http://noahveltman.com  
Twitter: [@veltman](http://twitter.com/veltman)  
Email: [noah@noahveltman.com](mailto:noah@noahveltman.com)  