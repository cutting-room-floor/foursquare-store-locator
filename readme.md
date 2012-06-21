# Foursquare Store Locator

This template uses the foursquare API to build a store locator feature that maps all addresses for venues in a foursquare list and allows users to search for closest venues. 

This whole template can be emedded as an iframe or integrated into your existing website template.

![](http://farm8.staticflickr.com/7098/7336417746_2fd7fe6fa0_z.jpg)

By adding your store location data to foursquare, you have a simple interface to manage your locations and allow customers interact with them both on-line and off. While browsing the map for nearby locations, users will see the total checkins and foursquare users who have visited your location, and they will have the option to add your location to their foursquare todo list. When users with the foursquare mobile app walk by venues on their todo list, they'll get reminded to check out your store. 

## About Map Site Templates

[Map Site templates](http://mapbox.com/map-sites) from MapBox are a way to jumpstart building a map-based web feature. The map-site templates bundles common html and css formatting with reusable javascript components. 

To build a project based on this template, fork this repository, edit the html content and css, and alter the configuration script.

To make your custom base map, [sign up for MapBox](http://mapbox.com/plans/) and [create a map](http://mapbox.com/hosting/creating/).


## Using this template

Edit the content by adjusting, removing, or adding to `index.html`. This is the main markup document with the content and layout for the map-site.

Adjust the design by editing the `style.css` file and adding any additional supporting structure to `index.html`.

Set the map features by writing a configuration script at the bottom of `index.html`. 


## HTML layout

The html markup for the template is in `index.html`. It's a simple html page layout. Generally, you'll want to change the content elements like `title`, `h1`, `img#logo` and `div#about`.


## CSS styles

Most of the hard work on a map site build is template design implemented through CSS. This template by default is simple and clean so you can modify or replace it. This design and be completely overridden by applying new CSS styles or changing the exisiting rules in `style.css`.

CSS rules are set in two files:

- `style.css` contains all the layout and typographic styles as well as some overridden styles for map controls, and a [reset stylesheet](http://meyerweb.com/eric/tools/css/reset/). Implement your design by editing this file.
- `map.css` holds the default map styles from tiles.mapbox.com embeds.


## Javascript interaction

All of the external javascript libraries to make the map interactive and connect it to MapBox are stored in the `ext` directory. For this template, we're using [Modest Maps](http://modestmaps.com/) and [Wax](http://mapbox.com/wax) to make the map interactive, [Easey](https://github.com/mapbox/easey) for smooth aninmated panning and zooming, and [MMG](http://mapbox.com/mmg/) for adding markers to the map based on [geojson](http://www.geojson.org/)-formatted data.

An internal javascript library, `script.js`, abstracts common map settings, and `foursquare.js` is the library we put together map the foursquare API.

We're also using [jQuery](http://jquery.com/) for DOM manipulation and handling events, and [Underscore.js](http://documentcloud.github.com/underscore/) for data processing.

### Map configuration

The map is added to the `<div>` container in `index.html` with `id="map"`. Styles to lay out the map container come from `class="map"`.

```html
<div id="map" class="map"></div>
```

At the bottom of the `index.html` document, we set up the map. The `id` of the container is the first argument (`'map'`), and an object of options is the second argument. The third arugement is the name of an optional callback function, which we use to start the `foursquare.js` main function, once the map is loaded. 

The only required option is `api`, and it should contain the API URL from MapBox. After you create a new map through your MapBox account, click `embed` on the `info` tab and copy the API URL.

```js
var main = Map('map', { 
    api: 'http://a.tiles.mapbox.com/v3/mapbox.map-hv50mbs9.jsonp' 
});
```

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

A map with all the options and a callback function would look like this:

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
}, foursquare.start);
```

### foursquare.js

All data about store locations for this template comes from the foursquare API. Begin by logging into foursquare and [creating a list of all of your venues](http://support.foursquare.com/entries/20386796-how-do-i-add-or-create-a-list). This example pulls addresses and phone numbers from the API, but you could extend it to include business hours, menues, photos and more by entering more information about your stores in their foursquare venue profiles. [See here](http://support.foursquare.com/entries/188296-how-do-i-add-my-business-to-foursquare) more about entering your store locations as foursquare venues.

To connect this template with your foursquare list, add the following to the top of your `index.html` configuration:

```js
// Set up the foursquare API
foursquare.settings = {
    /* foursquare API keys. See: https://foursquare.com/oauth/register */
    client_id: '1SHOHFLYHC3KIQKMBMKRWHASORK0TPCNPPH04OQCT1Y5ZRGW',
    client_secret: '2DMK0XSZL3ZMZDNR0G0UQ4ARJYN2HIJXL4FKXZ1WUALXZYZV',
    /* List ID. See: https://developer.foursquare.com/docs/explore#req=users/self/lists */
    list: '4fc674d7e4b07a1f71542757',
    /* Search radius for nearby address matches */
    radius: 8046.72 // 5 miles in meters
};
```

- `client_id` and `client_secret` These are your foursquare API keys. To get them, register your site at https://foursquare.com/oauth/register and replace the ones we are using here.
- `list` This is the foursquare list ID for the list you made with your locations. To find it, we need to browse the foursquare API. Visit [the foursquare API explorer](https://developer.foursquare.com/docs/explore#req=users/self/lists) and scan the json data until you find the ID for your list in the `items` array.
  ![](https://img.skitch.com/20120604-geg541i387j3jw176181unef7w.jpg)
- `radius` This is the radius in meters in which an location is considered a match for an address search. You could also allow the user to set this value by setting it with javascript.

The work of fetching, parsing, mapping, and filtering the venues (store locations) in your foursquare list is all handled by `foursquare.js`. 

## Further Reading

* [MapBox API](http://mapbox.com/hosting/api/)
* [MapBox Wax](http://mapbox.com/wax/)
* [MapBox MMG](http://mapbox.com/mmg/)
* [MapBox Easey](http://mapbox.com/easey/)
* [Modest Maps](http://modestmaps.com/)
* [jQuery](http://jquery.com/)
* [Underscore.js](http://documentcloud.github.com/underscore/)
* [foursquare API](https://developer.foursquare.com/)
