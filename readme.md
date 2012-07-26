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

Edit the content by adjusting, removing, or adding to `index.html`. This is
the main markup document with the content and layout for the map-site.

Adjust the design by editing the `style.css` file and adding any additional
supporting structure to `index.html`.

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

Most of the hard work on a microsite build is template design implemented through CSS. This template by default is simple and clean, and it's based on the tiles.mapbox.com full map view. This design and be completely overridden by applying new CSS styles. `style.css` contains all the layout and typographic styles as well as some overridden styles for map controls, as well as a [reset stylesheet](http://meyerweb.com/eric/tools/css/reset/). Implement your design by editing this file.

## Javascript interaction

The map is configured in `script.js` and takes advantage of many [MapBox Javascript API](http://mapbox.com/developers/mapbox.js/)
features - so the documentation for the MapBox Javascript API applies to every part
of this site.

Additional integration is added with `mapbox.jquery.js`, which automatically binds
links that control the map - see the navigation links for examples.

All the following controls require that the id of the element containing the map be specified using the `data-control` attribute. In this case it is `data-control="map"`. This attribute may be placed in any of the controls' parent elements.

### Address search

To search for an address, we need a geocoding service that converts a plain-text
address query into a geographic location. This template uses [MapQuest Open](http://open.mapquest.com/)
search, which is free to use for noncommercial and commercial applications alike. If you'd
like to use another service, edit the `geocode` function in `script.js`.

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

By specifying `data-control="gecode"` on the `div` containing your `form`,
`mapbox.jquery.geocoder.js` will bind a function that handles address searches and repositions
the map accordingly. If the geocoder has a successful response to a search, it
will center the map and zoom it to show the bounding box extent of that response. If
the bounding box is small enough to zoom the map to its maximum zoom, the geocoder
will also place a pin with a star over the response's exact location.


### Easing links
To link to a geographic location add at least one of the following data attributes:

- `data-lat`: The latitude of the location.
- `data-lon`: The longitude of the location.
- `data-zoom`: The zoom level.

```html
<a data-lat="39" data-lon"77" data-zoom="10" href="#">
```

If you specify any of these, the link will be automatically bound to the map.


### Layer Switcher
Use `data-control="switcher"` to bind all links in child elements to the layer switcher function. Specify the layer by setting the `href` attribute of anchors to the layer's name. There are two optional layer attributes:

- `data-group`: Specifies the group, defaulting to 0. Only one layer per group can be enabled at any time.
- `data-toggle="true"`: Allow a layer to be toggled off.

```html
<div data-control="switcher">
    <a data-group="0" href="#streets">Streets</a>
    <a data-group="1" href="#construction">Construction projects</a>
    <a data-group="1" href="#building">Building permits</a>
</div>
```

Easing links can be used together with the layer switcher.
