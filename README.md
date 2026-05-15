![Leaflet compatible!](https://img.shields.io/badge/Leaflet-0.7.7-blue.svg?style=flat)

> **⚠️ Disclaimer: This is a legacy fork for Leaflet 0.7.7.**  
> It is not recommended to use this project.  
> Please use the current version with Leaflet 1.x instead:  
> https://github.com/ptv-logistics/leaflet-ptv-developer

## Purpose

leaflet-ptv-developer-legacy provides classes to add [PTV Developer](https://developer.myptv.com/) specific features to Leaflet 0.7.7.

## Components

* [L.TileLayer.PtvDeveloper](#tilelayerptvdeveloper)

## How to build

```npm install```

```npm run build```

<a name="tilelayerptvdeveloper"></a>
### L.TileLayer.PtvDeveloper

The Layer class `L.TileLayer.PtvDeveloper` can be used to make PTV Developer [`data-tiles`](https://developer.myptv.com/Documentation/Raster%20Maps%20API/Code%20Samples/Data%20Tiles.htm) elements clickable or request tiles with specific parameters.

#### Additional options

* *disableMouseEvents* - disables all mouse click and hover events. Default: ```false```

#### Integration as single raster map

The easiest way to add a clickable layer is to use the class `L.TileLayer.PtvDeveloper`, append a clickable `data-tiles` layer (e.g. `restrictions` or `trafficIncidents`) to the profile and set the api key.  
The icons of the layer can now be clicked to display the object information.  
The options are the same as for `L.TileLayer`.

```javascript
var map = L.map('map').setView(new L.LatLng(49.012, 8.4044), 17);

var interactiveTileLayer = L.tileLayer.ptvDeveloper(
    'https://api.myptv.com/rastermaps/v1/data-tiles/{z}/{x}/{y}' +
    '?apiKey={token}&layers={layers}', {
        attribution: '&copy; ' + new Date().getFullYear() + ' PTV Group, HERE',
        layers: 'background,transport,labels,restrictions',
        token: window.apiKey,
        maxZoom: 22
    }).addTo(map);
```

#### Integration as layered raster map

It's also possible to split the PTV Developer raster tiles into separate Leaflet layers.  
This sample creates a [`image-tiles`](https://developer.myptv.com/Documentation/Raster%20Maps%20API/Code%20Samples/Image%20Tiles.htm) base map layer and a clickable restrictions `data-tiles` overlay.  
Use the `zIndex` option to control the layer stacking order.

```javascript
var map = L.map('map').setView(new L.LatLng(49.012, 8.4044), 17);

var basemapLayer = L.tileLayer(
    'https://api.myptv.com/rastermaps/v1/image-tiles/{z}/{x}/{y}' +
    '?apiKey={token}&layers={layers}', {
        attribution: '&copy; ' + new Date().getFullYear() + ' PTV Group, HERE',
        layers: 'background,transport',
        token: window.apiKey,
        maxZoom: 22
    }).addTo(map);

var restrictionsLayer = L.tileLayer.ptvDeveloper(
    'https://api.myptv.com/rastermaps/v1/data-tiles/{z}/{x}/{y}' +
    '?apiKey={token}&layers={layers}', {
        layers: 'restrictions',
        token: window.apiKey,
        maxZoom: 22,
        zIndex: 5
    }).addTo(map);

var labelsLayer = L.tileLayer(
    'https://api.myptv.com/rastermaps/v1/image-tiles/{z}/{x}/{y}' +
    '?apiKey={token}&layers={layers}', {
        layers: 'labels',
        token: window.apiKey,
        maxZoom: 22,
        zIndex: 7
    }).addTo(map);
```

## Limitations

- Vector tiles (MapLibre GL) are not supported with Leaflet 0.7.  
- The `createPane` API is not available.  
  Use the `zIndex` option on tile layers to control stacking order instead.
