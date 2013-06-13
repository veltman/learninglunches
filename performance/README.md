# Performance and the web #

## What do we mean by "performance"? ##

When people talk about performance on the web, they're usually talking about one of these things:

**Loading time:** When a user tries to load a page, or a specific file within a page, how long does it take?  When the user sends a request to the server, how long does it take for the response to come back?  The most basic concern here is file size - if you make a user download more data, it will take longer - but there are lots of other factors we'll talk about in a minute.

**Efficiency:** Speed and efficiency are close cousins.  In the context of web performance, efficiency means that your code is doing the minimum amount of work to accomplish a particular task.  There are usually many ways to code any process, ranging from incredibly efficient to incredibly inefficient, that all have the same result but require very different amounts of time and computing power (the difference can literally be a factor of thousands).  Efficiency usually translates into faster speeds for the user, but it also means your servers can handle more users at the same time.

**Browser performance:** The more work you ask a user's browser to do, the more potential there is for delays or crashes.  Your intention might be for something to happen instantly when a user clicks a button, but if there's enough JavaScript involved, or you're drawing complex vector graphics, or the user is on an older browser, it might not be so instant.  In the worst case scenario, a user stares at a spinning wheel for 20 seconds and then gets a prompt to kill your script because it's taking too long.
    
## Some factors that influence performance on the web ##

### Static vs. dynamic: being a good prep cook ###

When your server sends things to users, sometimes it's just sending a static file (a static file may also be called a "flat" file; it just means a plain file, usually text).  A user requests an image, the server sends it back.  The file might be large, but handling this request is a pretty fast process from the server's point of view.  It just has to pull the file off the shelf and start transmitting it.  In this case, the server is a vending machine.  You punch in the code, and get back a pre-packaged snack.  This is great when your content doesn't need to change.

Other times, your server doesn't just have static files at the ready.  Instead, it has databases and other ingredients, and when a request comes in, it goes into the kitchen and assembles the desired file from scratch before sending it back.  This is what we mean when we talk about doing something **dynamically**: there is no file yet, just some instructions for how to create the file each time someone asks for it.  This usually takes longer, sometimes a lot longer.  In this case, the server is a fancy cooked-to-order restaurant.  It can do lots of customization and change things on the fly, but each order takes longer to cook, and filling lots of orders at once becomes a problem.

The more work you ask the server to do for each request, the faster it will become overwhelmed by lots of simultaneous users.  Think of a restaurant kitchen during the dinner rush, or Lucy in the chocolate factory, unable to keep up with the conveyor belt.  Your server doing more work for each user means an increased risk of long delays and crashes at peak times,  needing more servers to handle the same number of users, or both.

Anyone who has ever catered a big dinner party or worked in a restaurant knows that there is a third way here.  In order to successfully serve dozens or hundreds of guests at the same time, you **do as much of the prep work as possible ahead of time**, chopping and mixing and pre-cooking whatever you can and then sticking it in the fridge to put the finishing touches on at the last moment.  The same is true when it comes to serving web content.  You can have the best of both worlds by being your own prep cook.  You figure out what *really* needs to be done on the fly when the order comes in and what you can prep ahead of time as a static file.  Then, when your server gets a request, it only has to do a little bit of work at that moment, putting some grill marks on the meat.

There are lots of different strategies and considerations for caching your dynamic content as static files, and they're beyond the scope of this primer.  The important thing to keep in mind is to think carefully about what content is genuinely dynamic and needs to always be cooked from scratch, and what content can be pre-cooked.

For example, let's say you have a news app built on quarterly unemployment figures from the Bureau of Labor Statistics.  You could build a database that stores the figures from each quarter when they come in, and build an app that gets the figures from that database every time a user loads the page.  This would be pretty silly though, because the data would be the same for three whole months at a time (The web server definition of madness: querying the same database a million times if you'll get the same result).  You'd be better off generating a new plain text file with the figures each time new data comes in, and having your app use that instead.

### Trimming the download fat ###

Asking a user to download more data means more loading time.  There are a number of strategies you can use to trim download size:

* **Strip out things you aren't using.** When you're using a data file you didn't create, it's often full of data your users won't need.  You might have a big CSV from the census with 22 columns, and your app only references 2 of them.  Or you might have a big geographic data file that specifies latitude and longitude to twelve decimal places when you only need three.  Strip out that extraneous data!  It might seem like a small difference; after all, it is just text.  But multiply that difference by 100,000 rows, and then multiply it by 100,000 users, and suddenly it starts to look pretty big.  Similarly, if there's a 1 MB external resource that you're using for one tiny piece of your app, think hard about whether you need that resource or whether there's an alternative approach.
* **Don't make users download everything at once.** Let's say you have some wonderful, rich web app with lots of different bits and pieces.  Maybe the total download size is large, but you can structure the experience so that the user doesn't have to wait for everything.  Maybe you have a news app with some intro text, a chart, and a big interactive map.  If you load the map in the background via AJAX after the page has loaded, your users can at least start reading the intro and the chart in the meantime, instead of potentially looking at a blank page until everything is ready.  Load things on demand when appropriate - if your app has hundreds of clickable things, each of which displays a giant image, don't make every user download every image when they'll probably only click on one or two items.
* **Cache things.** Don't make a user download the same file over and over again.  When evaluating performance, keep in mind what will be cached after the first download and what won't.  Asking someone to download 100 different images is different than asking someone to load 100 pages with the same image.
* **Serve things from faster sources.** Your own servers may not be the fastest way to get a file to a user.  The user's own cache will almost certainly be faster.  A [CDN](https://en.wikipedia.org/wiki/Content_delivery_network) will probably be faster too.  There are lots of other cases where using an external provider may be faster (unless you're pretty awesome, YouTube probably serves video faster than you do).  These strategies all have downsides too, but they can potentially ease the load on your own servers and get content to users faster.

### Heavy browser lifting: JavaScript, SVGs, etc. ###

Besides download time, another performance consideration comes up once the content is in a user's browser (the so-called "client side").  Including lots of cutting-edge JavaScript, SVG graphics, WebGL, and other fancy things can put quite a strain on a user's browser, especially if it's an older one.  When you're prototyping ideas, make sure you take them for a spin on older browsers.  It's easy to get way too far on something in Chrome and then be shocked when someone opens it up in IE8 and the beautiful masterpiece is a hot mess of delays, choppy transitions, and outright crashes.  The performance gap between old and modern browsers is growing every day.  Note that I'm not referring to browser compatibility, which is another can of worms; I'm just talking about cases where something theoretically works on a particular browser, but the overall experience sucks.  How much you need to worry about satisfying users on lower-performance browsers will depend a lot on your particular audience, which brings me to my next point...

### Things you can't control ###

How quickly and smoothly your users will be able to get at your content will also depend a lot on things that you can't control, especially what browsers and devices they use.  This is one of many reasons why **you really need to know your users**.

Get data on who they are and how they access your site before trying to decide how much to care about different elements of performance.  There isn't a one-size-fits-all answer.  If you have lots of IE8 users, you need to worry more about JavaScript complexity than a site with all of its users on newer browsers.  You need to worry more about file size if you have lots of mobile users, especially if you're in a country with expensive and slow mobile data plans.  If every single one of your users has the latest version of Chrome, a brand new MacBook, a 100Mbps fiber connection, and a 30" monitor, then hey, maybe you don't need to worry too much about any of this and we can all just have a pizza party instead.

## Why does performance matter? ##

**Poor performance = poor user experience**.  Users don't like to wait for things.  This is doubly true in a world where the share of users on mobile is increasing every day, because the demands of large downloads or complex browser interaction are amplified by the limitations of mobile data and browsers.  It's easy to lose sight of this when you're in the warm confines of an office with a big fat broadband connection and a nice computer.  Put yourself in the shoes of someone who wants to load an article on their phone on the train, or on crappy public WiFi.  Requiring a user to wait 20 seconds to load your news app is like selling your newspaper in clamshell packaging.  It's not necessarily a dealbreaker, but it's certainly a strike against you.

**Servers cost money**.  Keeping filesize and server loads down benefits your users, but it also benefits your budget.  It's true that a basic web server is dirt cheap, but it's also true that if you're a sizeable news organization with a lot of traffic, you use a lot more horsepower than that, and costs can add up quickly.

**Servers have limits**.  Even the biggest, most badass, most cleverly-configured array of web servers will buckle under enough strain.  If you make your server do a lot of extra work to fulfill page requests, and send lots of large unnecessary files, you're increasing the strain of each user, and increasing the risk that the server will max out (ask your sysadmins what keeps them up at night).

## The tradeoffs of higher performance ##

There are plenty of benefits to building your web app with performance in mind.  But there are also diminishing returns on that process.  You can _always_ optimize your performance more.  There's always another byte of data to shave off, or an algorithm to rewrite. It's not always worth it; optimizing performance takes time and resources away from other things, like your actual content.  Consider your users, your needs, and your available resources before deciding how much to care about different aspects of performance, and what level is "good enough."

Pursuing better performance for an app also has implications for its future.  Sometimes better performance means less code, cleaner code, a simpler architecture, fewer points of failure: something that will be easier to maintain, improve, and reuse later.  But sometimes improving performance means tricks, hacks, and complexity, things that make an app harder to maintain, harder to improve, harder to reuse.  High performance for the end user can require a lot of processes behind-the-scenes, and getting those right takes time and manpower.  Consider your team and the relative value of keeping things simple when thinking about potentially complexifying performance enhancements.

## Food for thought ##

**Know your users.** The single most important factor in deciding how much to care about performance is knowing who your users are.  Understanding what browsers they use, what devices they use, and how they interact with your content will go a long way towards indicating which performance factors you should prioritize and which ones aren't a big deal.

**Know yourself.**  The word of the day is "tradeoffs."  Sure, there are plenty of low-hanging fruit for better performance.  But most of the time it's a balancing act between performance and other factors.  What resources do you have available?  What's your deadline?  What will it mean for your development situation in six months or a year to build things a certain way?

**Think about performance early.**  The best time to avoid performance headaches is at the beginning of a project.  If you choose not to worry about it until the end, your design and development decisions are likely to paint you into a corner.  An ounce of prevention, etc.  Don't worry if you don't know what the key performance issues will be ahead of time when a project is still taking shape, but you should at least try to predict what they _might_ be, and come up with a plan of attack for what you need to keep an eye on as you go, and when you need to consider alternative options.

**Ask a developer.** It's not always obvious what will impact performance on the web a lot and what won't, and questions of design and performance are often hopelessly intertwined.  When in doubt, ask your friendly neighborhood web developers.  And involve them early; creating detailed specs and getting weeks into a project before you sanity check your technical assumptions with a developer is a recipe for disaster.

---

## Questions/Comments/Suggestions ##
Noah Veltman  
Web: http://noahveltman.com  
Twitter: [@veltman](http://twitter.com/veltman)  
Email: [noah@noahveltman.com](mailto:noah@noahveltman.com)  
