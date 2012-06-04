## About Map Site Templates

[Map Site templates](http://mapbox.com/map-sites) from MapBox are a way to jumpstart building a map-based web feature. The map-site templates bundles common html and css formatting with reusable javascript components.

To build a project based on this template, fork this repository, edit the html content and css, and alter the configuration script.


## Using this template

Edit the content by adjusting, removing, or adding to `index.html`. This is the main markup document with the content and layout for the map-site.

Adjust the design by editing the `style.css` file and adding any additional supporting structure to `index.html`.

Set the map features by writing a configuration script at the bottom of `index.html`.

## HTML layout

The html markup for the template is in `index.html`. It's a simple HTML5 page layout. Generally, there are three things to change in this document:

1. Content elements like the `title`, `h1`, and `div#about` elements
2. Add new container elements for features like maps, layer switchers, and geocoders
3. Layout structure, as controlled by the `class` attribute on the `body` element

There are three layout classes that can be applied to the `body` element:

- `right` A full screen map with a header and right content sidebar (default)
- `left` A similar full screen map with a centered header and left content sidebar
- `hero` An inline map hero with a header and full-width, scrollable content section

## CSS styles

Most of the hard work on a microsite build is template design implemented through CSS. This template by default is simple and clean, and it's based on the tiles.mapbox.com full map view. This design and be completely overridden by applying new CSS styles.

CSS styles are in two files:

- `style.css` contains all the layout and typographic styles as well as some overridden styles for map controls, as well as a [reset stylesheet](http://meyerweb.com/eric/tools/css/reset/). Implement your design by editing this file.
- `map.css` holds the default map styles from tiles.mapbox.com embeds.

## Javascript interaction

All of the external javascript libraries to make the map interactive and connect it to MapBox are stored in the `ext` directory. For this template, we're using [Modest Maps](http://modestmaps.com/) and [Wax](http://mapbox.com/wax) to make the map interactive, [Easey](https://github.com/mapbox/easey) for smooth aninmated panning and zooming, and [MMG](http://mapbox.com/mmg/) for adding markers to the map based on [geojson](http://www.geojson.org/)-formatted data. We're also using [jQuery](http://jquery.com/) for DOM manipulation and handling events.

An internal javascript library, `script.js`, abstracts common map settings, and includes configuration for layer switching controls and a geocoding address search.


### Map configuration

The map is added to the `<div>` container in `index.html` with `id="map"`. Styles to lay out the map container come from `class="map"`.

```html
<div id="map" class="map"></div>
```

At the bottom of the `index.html` document, we set up the map. The `id` of the container is the first argument (`'map'`), and an object of options is the second argument. The third arugement is the name of an optional callback function that is called once the map is loaded.

The only required option is `api`, and it should contain the API URL from MapBox. After you create a new map through your MapBox account, click `embed` on the `info` tab and copy the API URL.

```js
var main = Map('map', {
    api: 'http://a.tiles.mapbox.com/v3/mapbox.map-hv50mbs9.jsonp'
});
```

In this example, we're directly specifying what tilesets to use by listing them in the API url separated by comas: `http://a.tiles.mapbox.com/v3/mapbox.mapbox-light,mapbox.dc-property-values.jsonp`. For more information on building custom API URLs, see the [MapBox API documentation](http://mapbox.com/hosting/api/).

The map options object can take several options:

- `api` The MapBox API URL from the `embed` button on your map:
  ![](http://mapbox.com/images/hosting/embedding-4.png)
- `center` An object of `{ lat: ##, lon: ##, zoom: ## }` that defines the map's initial view. If not is provided, the default center set from MapBox will be used
- `zoomRange` An array of `[##, ##]` where the first element is the minimum zoom level and the second is the max
- `features` An array of additional features for the map

The features object may contain any of the following:

- `zoomwheel` Use the scroll wheel on the mouse to zoom the map
- `tooltips` or `movetips` For layers with interactive overlays, display fixed `tooltips` or `movetips`, which are overlays the follow the cursor
- `zoombox` Allow uses to zoom to a bounding box by holding the shift key and dragging over the map
- `zoompan` Show zoom controls on the map
- `legend` Show a legend on the map. Legends from multiple layers will stack on top of each other
- `share` Show a share button on the map with Twitter, Facebook links and an embed code for the map. The embedded view of the map will add a `class="embed"` to the `<body>` element of the page for easy theming. For instance, by default the embed layout is a full screen map with the layer switcher over it on the left. The header and content are hidden.
- `bwdetect` Automatically detect low bandwidth contections and decrease the quality of the map images to accomodate
- `static` Disables panning and zooming mouse and touch interaction on the map

A map with all the options would look like this:

```js
var main =  Map('map', {
    api: 'http://a.tiles.mapbox.com/v3/mapbox.map-hv50mbs9.jsonp',
    center: {
        lat: 38.8850033656251,
        lon: -77.01439615889109,
        zoom: 14
    },
    zoomRange: [0, 15],
    features: [
        'zoomwheel',
        'tooltips', // or 'movetips'
        'zoombox',
        'zoompan',
        'legend',
        'share',
        'bwdetect'
    ]
});
```


### Layer switching

The `script.js` provides an easy way to toggle between layers on a map. When activated, layers will overlay the initial base map. If turned on, tooltips and legends will update to pull from the current layer.

Layers are bound to links on the page by specifying a name for the layer in the `href` attribute of the link element and giving it a `data-control=layer` attribute.

```html
<div id="projects" class="layers">
   <a data-control="layer" href="#building">Building Permits, 2011</a>
   <a data-control="layer" href="#construction">City Construction Projects, 2011</a>
</div>
```
Then specify the configuration for your layers in the script at the end of `index.html`:

```js
main.layers({
    building: {
        api: 'http://a.tiles.mapbox.com/v3/mapbox.dc-building.jsonp',
        center: {                   // Optionally reposition the map.
            lat: 38.910606275724,   // New center point and zoom level
            lon: -77.00126406355,   // for the map. Specific either
            zoom: 14,               // lat and lon, zoom, or both.
            ease: 500               // Optional time to animimate moving
        },                          // the map in milliseconds.
        group: 1                    // An internally exclusive layer group
    },
    construction: {
        api: 'http://a.tiles.mapbox.com/v3/mapbox.dc-construction.jsonp',
        center: { zoom: 12, ease: 1000 },
        group: 1
    }
});

```

Here, each layer gets a name, in this case `building` and `construction` that binds the layer to its link element (e.g. `<a data-control="layer" href="#building">`). Each layer is an object of options that will affect the map when the layer is turned on, which happens by clicking or tapping its bound link.

- `api` If api is specified, this map layer will be displayed on top of the base map specified when the map was initialized. The API URL may be omitted if you only want the layer to move the map and not add new content
- `center` The center object has `lat`, `lon`, and `zoom` properties. If `zoom` is omitted, the map will be repositioned, but keep its current zoom level. If `lat` and `lon` are omitted but `zoom` is specified, the map will change zoom level but keep its current centerpoint. There's an additional property called `ease`, which is the time in milliseconds to animate moving the map to the new location. It's optional too. Omitting it will snap the map to its new location.
- `group` An optional group property for each layer. Layers within the same group are exclusive and can only be displayed one at a time. Layers in different groups are inclusive and will be stacked in order of the group number. Group numbers should be squential starting with `0` as the bottom-most layer.
- `initial` Set to `true` if this layer should be displayed by default on initial page load. 

Add as many layers as you need and bind them to any link element. Layers are exclusive and will toggle on and off on click or tap.


### Address search

To search for an address, we need a geocoding service that converts a plain-text address query into a geographic location. This template uses MapQuest Open search, which is free to use for noncommercial and commercial applications alike. If you'd like to use another service, edit the `geocode` function in `script.js`.

To add an address search to your page, build a simple html form to gather user input:

```html
<div data-control="geocode" id="search">
    <form class="geocode">
        <input placeholder="Search for an address" type="text">
        <input type="submit" />
        <div id="geocode-error"></div>
    </form>
</div>
```

By specifying `data-control="gecode"` on the `div` containing your `form`, `script.js` will bind a function that handles address searches and repositions the map accordingly. If the geocoder has a successful response to a search, it will center the map and zoom it to show the bounding box extent of that response. If the bounding box is small enough to zoom the map to its maximum zoom, the geocoder will also place a pin with a star over the response's exact location. You can adjust this marker or hide is by editing the `mmg-default` styles in `style.css`.

![](https://img.skitch.com/20120502-cxftsce4ejckxxjwjs6h2jp95s.jpg)

## Further Reading

* [MapBox API](http://mapbox.com/hosting/api/)
* [MapBox Wax](http://mapbox.com/wax/)
* [MapBox MMG](http://mapbox.com/mmg/)
* [MapBox Easey](http://mapbox.com/easey/)
* [Modest Maps](http://modestmaps.com/)
* [jQuery](http://jquery.com/)
