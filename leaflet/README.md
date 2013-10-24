# Making maps with Leaflet: A starter guide

A work-in-progress.

This is a *very* quick overview of how to use Leaflet.js to perform
some basic mapping tasks:

* Making a map in a webpage
* Adding points to the map
* Creating a "popup" information window when clicking on a point.
* Adding lines to the map.
* Adding shapes to the map.

## 1: A super basic HTML page.
Here's the plain ol' HTML page we'll use to create a map:

```html
<!DOCTYPE html>
<html>
<head>
	<title>Leaflet!</title>
	<meta charset="utf-8" />
</head>
<body>
	<div id="map" style="position: absolute; top: 0; bottom: 0; left: 0;
	right: 0; width: 100%; height: 100%"></div>
</body>
</html>
```
The only complicated part is the `style` element, which just makes the map tag cover the entire surface of the page.




## 2: Loading a map

Now, we'll throw the Leaflet CSS and Javascript into the header so it loads...

```html
	<link rel="stylesheet" href="http://cdn.leafletjs.com/leaflet-0.6.4/leaflet.css" />
	<!--[if lte IE 8]>
		<link rel="stylesheet" href="http://cdn.leafletjs.com/leaflet-0.6.4/leaflet.ie.css" />
	<![endif]-->
	<script src="http://cdn.leafletjs.com/leaflet-0.6.4/leaflet.js"></script>
```

...and some simple script code to turn our full `<div id="map">` tag into a real, working map.

```html
	<script>
		var map = L.map('map').setView([-34.6094, -58.4098], 14);
		L.tileLayer('http://otile1.mqcdn.com/tiles/1.0.0/map/{z}/{x}/{y}.png').addTo(map);
	</script>
```

The `L.map('map')` part tells Leaflet to turn the `map` div into an interactive Leaflet map. Then, we center it on the coordinate -34.6094, -58.4098 (which is a few blocks south of where we are) and use "zoom level 14".

To actually give it a usable road map, we simply use [OpenStreetMap tiles that MapQuest hosts][mq_osm], set it up as a `tileLayer`, and add it to the map we just created.

[mq_osm]: https://wiki.openstreetmap.org/wiki/MapQuest

---

Here's what the page code should look like:

```html
<!DOCTYPE html>
<html>
<head>
	<title>Leaflet!</title>
	<meta charset="utf-8" />
	<link rel="stylesheet" href="http://cdn.leafletjs.com/leaflet-0.6.4/leaflet.css" />
	<!--[if lte IE 8]>
		<link rel="stylesheet" href="http://cdn.leafletjs.com/leaflet-0.6.4/leaflet.ie.css" />
	<![endif]-->
	<script src="http://cdn.leafletjs.com/leaflet-0.6.4/leaflet.js"></script>
</head>
<body>
	<div id="map" style="position: absolute; top: 0; bottom: 0; left: 0;
	right: 0; width: 100%; height: 100%"></div>
	<script>
		var map = L.map('map').setView([-34.6094, -58.4098], 14);
		L.tileLayer('http://otile1.mqcdn.com/tiles/1.0.0/map/{z}/{x}/{y}.png').addTo(map);
	</script>
</body>
</html>
```

[And here’s what that HTML page should look like if you open it up in your browser.](http://d2p12wh0p3fo1n.cloudfront.net/files/20130828/workshop_1.html)  
![map](https://d2p12wh0p3fo1n.cloudfront.net/files/20130828/workshop1.jpg)

---

If we want to be a good citizen of the world, we should give credit to the OpenStreetMap project and to MapQuest for the tiles. MapQuest's documetation also tells us that they have multiple domains that serve the same tiles (so that a CDN — content-delivery network — can serve the tiles to users more quickly). So let's change the tileLayer line to this:

```html
		L.tileLayer('http://otile{s}.mqcdn.com/tiles/1.0.0/map/{z}/{x}/{y}.png', {
			attribution: 'Map data &copy;<a href="http://www.openstreetmap.org/copyright" target="_blank">OpenStreetMap</a>, Map imagery &copy;<a href="http://open.mapquest.com/" target="_blank">MapQuest</a>',
			subdomains: '1234'
		}).addTo(map);
```

Now our basic map page looks like this:

```html
<!DOCTYPE html>
<html>
<head>
	<title>Leaflet!</title>
	<meta charset="utf-8" />
	<link rel="stylesheet" href="http://cdn.leafletjs.com/leaflet-0.6.4/leaflet.css" />
	<!--[if lte IE 8]>
		<link rel="stylesheet" href="http://cdn.leafletjs.com/leaflet-0.6.4/leaflet.ie.css" />
	<![endif]-->
	<script src="http://cdn.leafletjs.com/leaflet-0.6.4/leaflet.js"></script>
</head>
<body>
	<div id="map" style="position: absolute; top: 0; bottom: 0; left: 0;
	right: 0; width: 100%; height: 100%"></div>
	<script>
		var map = L.map('map').setView([-34.6094, -58.4098], 14);
		L.tileLayer('http://otile{s}.mqcdn.com/tiles/1.0.0/map/{z}/{x}/{y}.png', {
			attribution: 'Map data &copy;<a href="http://www.openstreetmap.org/copyright" target="_blank">OpenStreetMap</a>, Map imagery &copy;<a href="http://open.mapquest.com/" target="_blank">MapQuest</a>',
			subdomains: '1234'
		}).addTo(map);
	</script>
</body>
</html>
```





## 3: Markers to show location

Now let's learn how to add markers — a visual representation of a latitude/longitude location — to our map.

Similar to how we added `tileLayer` object to our map, we can also add a `marker`:

```javascript
  	L.marker([-34.60622, -58.41047]).addTo(map)
			.bindPopup("Ciudad Cultural Konex!");
```

Add this below where we added our tileLayer, like this:

```html
<!DOCTYPE html>
<html>
<head>
  <title>Leaflet!</title>
	<meta charset="utf-8" />
	<link rel="stylesheet" href="http://cdn.leafletjs.com/leaflet-0.6.4/leaflet.css" />
	<!--[if lte IE 8]>
		<link rel="stylesheet" href="http://cdn.leafletjs.com/leaflet-0.6.4/leaflet.ie.css" />
	<![endif]-->
	<script src="http://cdn.leafletjs.com/leaflet-0.6.4/leaflet.js"></script>
</head>
<body>
	<div id="map" style="position: absolute; top: 0; bottom: 0; left: 0; right: 0; width: 100%; height: 100%"></div>
	<script>
		var map = L.map('map').setView([-34.6094, -58.4098], 14);
  	L.tileLayer('http://otile{s}.mqcdn.com/tiles/1.0.0/map/{z}/{x}/{y}.png', {
			attribution: 'Map data &copy;<a href="http://www.openstreetmap.org/copyright" target="_blank">OpenStreetMap</a>, Map imagery &copy;<a href="http://open.mapquest.com/" target="_blank">MapQuest</a>',
			subdomains: '1234'
		}).addTo(map);

		L.marker([-34.60622, -58.41047]).addTo(map)
			.bindPopup("Ciudad Cultural Konex!");
	</script>
</body>
</html>
```

Now you should have [a map with a point, like this](http://s3.amazonaws.com/media.miketigas.com/files/20130828/workshop_2.html). You can even click on the point and see the "Ciudad Cultural Konex!" popup.

It should look like this:  
![map](https://d2p12wh0p3fo1n.cloudfront.net/files/20130828/workshop2.jpg)
![map](https://d2p12wh0p3fo1n.cloudfront.net/files/20130828/workshop3.jpg)

---

You can add as many points as you want. For example, replacing the above `L.marker` block with the following...

```javascript
		L.marker([-34.5974, -58.3983]).addTo(map);
		L.marker([-34.6102, -58.39]).addTo(map);
		L.marker([-34.602, -58.42]).addTo(map);
		L.marker([-34.60622, -58.41047]).addTo(map);
```

...will get you a map that looks like  
![map](https://d2p12wh0p3fo1n.cloudfront.net/files/20130828/workshop4.jpg)




## 4: Drawing lines between points

Now we know how to start a map and put points on a map. Let's learn how to put together a map with lines.

We'll take the previous full example and replace all the `L.marker` lines with this:

```javascript
		L.marker([-34.60622, -58.41047]).addTo(map)
			.bindPopup("Ciudad Cultural Konex!");

		L.marker([-34.6102, -58.39]).addTo(map);

		L.polyline([
			[-34.60622, -58.41047],
			[-34.6102, -58.39]
		], {color: 'red'}).addTo(map);
```

Instead of a simple `latitude, longitude` coordinate, a polyline takes several coordinates as its input, and then draws lines between them.

Here's the full HTML of the page:

```html
<!DOCTYPE html>
<html>
<head>
  <title>Leaflet!</title>
	<meta charset="utf-8" />
	<link rel="stylesheet" href="http://cdn.leafletjs.com/leaflet-0.6.4/leaflet.css" />
	<!--[if lte IE 8]>
		<link rel="stylesheet" href="http://cdn.leafletjs.com/leaflet-0.6.4/leaflet.ie.css" />
	<![endif]-->
	<script src="http://cdn.leafletjs.com/leaflet-0.6.4/leaflet.js"></script>
</head>
<body>
	<div id="map" style="position: absolute; top: 0; bottom: 0; left: 0; right: 0; width: 100%; height: 100%"></div>
	<script>
		var map = L.map('map').setView([-34.6094, -58.4098], 14);
		L.tileLayer('http://otile{s}.mqcdn.com/tiles/1.0.0/map/{z}/{x}/{y}.png', {
			attribution: 'Map data &copy;<a href="http://www.openstreetmap.org/copyright" target="_blank">OpenStreetMap</a>, Map imagery &copy;<a href="http://open.mapquest.com/" target="_blank">MapQuest</a>',
			subdomains: '1234'
		}).addTo(map);

		L.marker([-34.60622, -58.41047]).addTo(map)
			.bindPopup("Ciudad Cultural Konex!");

		L.marker([-34.6102, -58.39]).addTo(map);

		L.polyline([
			[-34.60622, -58.41047],
			[-34.6102, -58.39]
		], {color: 'red'}).addTo(map);
	</script>
</body>
</html>
```

![map](https://d2p12wh0p3fo1n.cloudfront.net/files/20130828/workshop5.jpg)




## 5: Drawing shapes

Similarly, `L.polygon` objects work like `L.polyline` objects, but they need a minimum of three points (so a shape is completed).

Replace the markers and polyline portions of the code with this:
```javascript
		L.polygon([
			[-34.60622, -58.41047],
			[-34.6102, -58.39],
			[-34.5974, -58.3983]
		], {color: 'green'}).addTo(map);
```

You'll end up with something like this:
![map](https://d2p12wh0p3fo1n.cloudfront.net/files/20130828/workshop6.jpg)
