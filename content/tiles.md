## Tiles API

The MetGIS Maps API offers forecasts as a Tile Map Service (TMS). This standardized format enables you to integrate our weather predictions for Central Europe into any dynamic map on your website or within your app. Check our [demo page](http://tiles.metgis.com/tiles-demo/) to see an example of weather data layers integrated into zoomable maps.

Our forecasts are updated every 24 hours. Once a day, at approximately 2:00 UTC, a new forecast is available, covering the following 75 hours mostly in three-hour time intervals. Beside regular tiles (in PNG-format) we also provide UTFGrid Tiles to enable a fast numerical representation of our colored maps.

![Coverage of MetGIS Maps API](./img/coverage_thumb.jpg)

TODO: GIF?

The following sections provide detailed information about our Maps API, with various [examples](#examples) showing how to include our weather in your maps.

### Weather Parameters

The following weather parameters are currently available:

| **Weather Parameter** | **Code** | **Time Interval** |
|------|:-----------:|:-----------:|
| Temperature | `tmp2m` | 3 hours |
| Precipitation | `hr_p` | 3 hours |
| Fresh Snow | `hr_sn` | 3 hours |
| Cloudiness | `tcdcprs` | 3 hours |
| Wind | `wind` | 3 hours |
| Fresh Snow Today | `hr_sn_0-24` | 24 hours |
| Fresh Snow Tomorrow | `hr_sn_24-48` | 24 hours |
| Fresh Snow Day after Tomorrow | `hr_sn_48-72` | 24 hours |
| Fresh Snow 0 - 48 hours | `hr_sn_0-48` | 48 hours |
| Fresh Snow 24 - 72 hours | `hr_sn_24-72` | 48 hours |
| Fresh Snow 0 - 72 hours | `hr_sn_0-72` | 72 hours |

TODO: Soll hier auch hrsn -24 - 24 angeführt werden??

### API Call for Tiles

To visualize our weather tiles within your map or application, you need to call the following URL with the fields specified according to your requirements.

#### Tiles URL

```http
http://{t1-t3}.metgis.com/{parameter}_{timestep}/{z}/{x}/{y}.png?key={YOUR-API-KEY}
```

**Parameters:**

| **URL Parameter** | **Description** |
|-----------|-------------|
| `{t1-t3}` | subdomains to get around browser limitations on the number of simultaneous HTTP connections to each host |
| `{parameter}` | the code of the desired weather parameter as described in `meta.json` |
| `{timestep}` | value of the desired time step, starting at 1 which reflects the weather forecasts for the time  `forecastIssued`, see [meta.json](#metajson) |
| `{x}`, `{y}`, `{z}` | x,y coordinates and zoom level of a tile |
| `{YOUR-API-KEY}` | your unique API-Key, [more information](#api-key) |

Please check our [sample code](#examples) to see how to use the tiles in your application.

### UTFGrid

Our [UTFGrids](https://github.com/mapbox/utfgrid-spec) provide numerical information related to our tiles. Like in our [demo page](http://tiles.metgis.com/tiles-demo/) you can use the UTFGrid Layers with a "mouseover" or "on tap" on mobile devices. Thus numerical forecast values can be displayed at specified geographic locations.

#### UTFGrid URL

```http
http://{t1-t3}.metgis.com/{parameter}_{timestep}_grid/{z}/{x}/{y}.json?callback={cb}&key={YOUR-API-KEY}
```

**Parameters:**

| **URL Parameter** | **Description** |
|---------------|-----------------|
| `{t1-t3}` | subdomains to get around browser limitations on the number of simultaneous HTTP connections to each host |
| `{parameter}` | the code of the desired weather parameter as described in `meta.json` |
| `{timestep}` | value of the desired time step, starting at 1 which reflects the weather forecasts for the time  `forecastIssued`, see [meta.json](#metajson) |
| `{x}`, `{y}`, `{z}` | x,y coordinates and zoom level of a tile |
| `{YOUR-API-KEY}` | your unique API-Key, [more information](#api-key) |

Please check our [sample code](#examples) to see how to use the UTFGrids in your application.

### Numerical values for Selected Locations

TODO: Screenshot

Please note that this feature is currently only available in our [demo page](http://tiles.metgis.com/tiles-demo/) after checking the "Show numbers" box. Please [contact us](http://www.metgis.com/about/contact/) for further information.

To show values for a variety of significant locations (cities, towns, peaks, ...) as additional overlay on your map, you can integrate our Point-Layers. They are available for all of our [weather parameters](#weather-parameters).

#### Sample call

```http
http://{t1-t3}.metgis.com/{parameter}_{timestep}_point/{z}/{x}/{y}.png?key={YOUR-API-KEY}
```


### API-Key

TODO: Eigene Sektion am Anfang (mach Stefan)


### meta.json

For every forecast run a file with meta-information is produced. It contains the start time of the forecast run and other useful information. This `meta.json` is located at `http://tiles.metgis.com/meta/{V}/meta.json` where `{V}` stands for the version number. Please check the current version in [the changelog](http://tiles.metgis.com/meta/).

```endpoint
GET http://tiles.metgis.com/meta/{V}/meta.json
```

**Properties:**


| **Property** | **Description** |
|--------------|-----------------|
| `description` | a brief description of the service |
| `productInfo` | information on the product |
| `forecastIssued` | start time of the calculations of the meteorological forecast model as ISO-Datestring (UTC). This is also the time of the latest measurements that exercise an influence on the forecast. |
| `forecastCompleted` | end time of the calculation of the tiles as ISO-Datestring (UTC), the time when the forecasts can be accessed. |
| `projection` | EPSG-Code of the coordinate reference system used for the tiles |
| `bbox` | the bounding box of the currently available tiles (latitude/longitude of the lower left and upper right box limit) |
| `timeStep` | length of period of time between two forecast times, typically 3 hours |
| `timeStepUnit` | the unit of `timeStep` and `parameterPeriod`, `h` stands for hour |
| `timeStepNumber` | number of `timeSteps` for this forecast |
| `attribution` | attribution that has to be shown in conjunction with the forecasts, see the [demo page](http://tiles.metgis.com/tiles-demo/) |
| `parameters` | list of available weather parameters |
| `parameterUnit` | unit of the given parameter |
| `parameterPeriod` | time period which a single prediction refers to in hours. For most parameters (e.g. temperature) this is a point in time, for some others (e.g. `precipitation`) it is a period (typically as long as the `timeStep`). `0` indicates a point in time, any other number describes the length of the time period. |
| `parameterName` | Language dependent name of the given parameter, indetified by ISO 639-1 Codes. Currently available languages: English, German, Spanish, French, Italian, Slovenian. |


#### Example Response

```json
{
   "description":"MetGIS Tile Server",
   "productInfo":"MetGIS Weather Forecast based on GFS Data",
   "forecastIssued":"2015-01-03T18:00Z",
   "forecastCompleted":"2015-01-04T03:44Z",
   "projection":"EPSG:3857",
   "bbox":"43.0,5.0,50.0,18.0",
   "timeStep":3,
   "timeStepUnit":"h",
   "timeStepNumber":25,
   "attribution":"Weather Forecast &copy; <a title='MetGIS Professional Weather Service' href='http:\/\/www.metgis.com\/' target='_blank'>MetGIS</a>",
   "parameters":{
      "wind":{
         "parameterUnit":"km\/h",
         "parameterPeriod":0,
         "parameterName":{
            "sl":"Veter",
            "de":"Wind",
            "it":"Vento",
            "fr":"Vent",
            "en":"Wind",
            "es":"Viento"
         }
      },
      "tmp2m":{
         "parameterUnit":"°C",
         "parameterPeriod":0,
         "parameterName":{
            "sl":"Temperatura",
            "de":"Temperatur",
            "it":"Temperatura",
            "fr":"Température",
            "en":"Temperature",
            "es":"Temperatura"
         }
      },
      "tcdcprs":{
         "parameterUnit":"%",
         "parameterPeriod":0,
         "parameterName":{
            "sl":"Oblačnost",
            "de":"Bewölkung",
            "it":"Nuvolosità",
            "fr":"Nébulosité",
            "en":"Cloudiness",
            "es":"Nubosidad"
         }
      },
      "hr_p":{
         "parameterUnit":"mm",
         "parameterPeriod":3,
         "parameterName":{
            "sl":"Padavine",
            "de":"Niederschlag",
            "it":"Precipitazione",
            "fr":"Précipitations",
            "en":"Precipitation",
            "es":"Precipitación"
         }
      },
      "hr_sn":{
         "parameterUnit":"cm",
         "parameterPeriod":3,
         "parameterName":{
            "sl":"Novozapadli sneg",
            "de":"Neuschnee",
            "it":"Neve fresca",
            "fr":"Neige fraiche",
            "en":"Fresh Snow",
            "es":"Nieve fresca"
         }
      }
   }
}
```

### Color Values

The color values related to the parameters are as follows:

TODO: Table Breite?

**Temperature**

| **Value [°C]** | **Color** |
|----------------|-----------|
| <-30 | `#737373` |
| -30 | `#969696` |
| -28 | `#bdbdbd` |
| -26 | `#efedf5` |
| -24 | `#dadaeb` |
| -22 | `#bcbddc` |
| -20 | `#9e9ac8` |
| -18 | `#807dba` |
| -16 | `#6a51a3` |
| -14 | `#544082` |
| -12 | `#06407c` |
| -10 | `#08519c` |
| -8 | `#2171b5` |
| -6 | `#4292c6` |
| -4 | `#6baed6` |
| -2 | `#9ecae1` |
| 0 | `#238443` |
| 2 | `#41ab5d` |
| 4 | `#78c679` |
| 6 | `#addd8e` |
| 8 | `#d9f0a3` |
| 10 | `#f7fcb9` |
| 12 | `#ffffcc` |
| 14 | `#ffeda0` |
| 16 | `#fed976` |
| 18 | `#feb24c` |
| 20 | `#fd8d3c` |
| 22 | `#fdbb84` |
| 24 | `#fc8d59` |
| 26 | `#ef6548` |
| 28 | `#d7301f` |
| 30 | `#bd0026` |
| 32 | `#b30000` |
| 34 | `#800026` |
| 36 | `#7f0000` |
| 38 | `#4c0016` |
| >40 | `#35000f` |

**Precipitation**

| **Value [mm]** | **Color** |
|----------------|-----------|
| 0.1 | `#deebf7` |
| 0.2 | `#c6dbef` |
| 0.5 | `#9ecae1` |
| 1 | `#6baed6` |
| 1.5 | `#4292c6` |
| 2 | `#2171b5` |
| 3 | `#08519c` |
| 4 | `#08306b` |
| 6 | `#fde0ef` |
| 8 | `#f1b6da` |
| 12 | `#de77ae` |
| 16 | `#c51b7d` |

**Fresh Snow**

| **Value [cm]** | **Color** |
|----------------|-----------|
| 0.1 | `#ffffff` |
| 0.3 | `#c6ecf9` |
| 1 | `#a0e0f6` |
| 2 | `#59ccf2` |
| 4 | `#19bbf1` |
| 6 | `#019ccf` |
| 8 | `#f79df3` |
| 12 | `#f353ec` |
| 16 | `#f311e8` |
| 20 | `#fbc607` |
| 30 | `#fba204` |
| 50 | `#f7760f` |
| 80 | `#e03603` |
| 120 | `#a11a04` |

**Cloudiness**

| **Value [%]** | **Color** |
|---------------|-----------|
| 25 | `#cccccc` |
| 37.5 | `#b2b2b2` |
| 50 | `#999999` |
| 62.5 | `#7f7f7f` |
| 75 | `#666666` |
| 87.5 | `#4c4c4c` |

**Wind**

| **Value [km/h]** | **Image** |
|------------------|-----------|
| 0 | wind-0-270.png |
| 2 | wind-2-270.png |
| 5 | wind-5-270.png |
| 10 | wind-10-270.png |
| 20 | wind-20-270.png |
| 30 | wind-30-270.png |
| 40 | wind-40-270.png |
| 50 | wind-50-270.png |
| 75 | wind-75-270.png |
| 100 | wind-100-270.png |

#### JavaScript Objects for generating legends

``` javascript
{
    "tmp2m": [[">40","#35000f"],["38","#4c0016"],["36","#7f0000"],["34","#800026"],["32","#b30000"],["30","#bd0026"],["28","#d7301f"],["26","#ef6548"],["24","#fc8d59"],["22","#fdbb84"],["20","#fd8d3c"],["18","#feb24c"],["16","#fed976"],["14","#ffeda0"],["12","#ffffcc"],["10","#f7fcb9"],["8","#d9f0a3"],["6","#addd8e"],["4","#78c679"],["2","#41ab5d"],["0","#238443"],["-2","#9ecae1"],["-4","#6baed6"],["-6","#4292c6"],["-8","#2171b5"],["-10","#08519c"],["-12","#06407c"],["-14","#544082"],["-16","#6a51a3"],["-18","#807dba"],["-20","#9e9ac8"],["-22","#bcbddc"],["-24","#dadaeb"],["-26","#efedf5"],["-28","#bdbdbd"],["-30","#969696"],["<-30","#737373"]],

    "hr_p": [["16","#c51b7d"],["12","#de77ae"],["8","#f1b6da"],["6","#fde0ef"],["4","#08306b"],["3","#08519c"],["2","#2171b5"],["1.5","#4292c6"],["1","#6baed6"],["0.5","#9ecae1"],["0.2","#c6dbef"],["0.1","#deebf7"]],

    "hr_sn": [["16","#f311e8"],["12","#f353ec"],["8","#f79df3"],["6","#019ccf"],["4","#19bbf1"],["2","#59ccf2"],["1","#a0e0f6"],["0.3","#c6ecf9"],["0.1","#ffffff"]],

    "tcdcprs": [["87.5","#4c4c4c"],["75","#666666"],["62.5","#7f7f7f"],["50","#999999"],["37.5","#b2b2b2"],["25","#cccccc"]],

    "wind": [["100","#ffffff url('img/270n/wind-100-270.png') no-repeat center; width: 59px; height: 33px;"],["75","#ffffff url('img/270n/wind-75-270.png') no-repeat center; width: 54px; height: 30px;"],["50","#ffffff url('img/270n/wind-50-270.png') no-repeat center; width: 48px; height: 28px;"],["40","#ffffff url('img/270n/wind-40-270.png') no-repeat center; width: 43px; height: 24px;"],["30","#ffffff url('img/270n/wind-30-270.png') no-repeat center; width: 36px; height: 20px;"],["20","#ffffff url('img/270n/wind-20-270.png') no-repeat center; width: 29px; height: 16px;"],["10","#ffffff url('img/270n/wind-10-270.png') no-repeat center; width: 23px; height: 13px;"],["5","#ffffff url('img/270n/wind-5-270.png') no-repeat center; width: 18px; height: 10px;"],["2","#ffffff url('img/270n/wind-2-270.png') no-repeat center; width: 13px; height: 8px;"],["0","#ffffff url('img/270n/wind-0-270.png') no-repeat center; width: 8px; height: 8px;"]]
  }
```

## Examples

Our weather maps may be integrated into your website or app, using different software frameworks. Some of them are:

|  **Web**  |  **Apps**  |
|-----------|------------|
| [Leaflet](http://leafletjs.com/) | [Google Maps Android API](https://developers.google.com/maps/documentation/android/) |
| [OpenLayers](http://openlayers.org/) | [Google Maps SDK for iOS](https://developers.google.com/maps/documentation/ios/) |
| [Google Maps API](https://developers.google.com/maps/) | Apple's [Maps for Developers](https://developer.apple.com/maps/) |
|  | [Mapbox SDK](https://www.mapbox.com/mobile/) |

#### Example of integration with Leaflet

```javascript
var temperatureLayer = L.tileLayer( 'http://t1.metgis.com/tmp2m_5/{z}/{x}/{y}.png?key=YOUR_API_KEY' );
```

The following examples show possible implementations with different frameworks.

### Leaflet

#### Example of temperature integration:

```javascript
var tmp2mUrl = 'http://{s}.metgis.com/tmp2m_';
var urlSuffix = '/{z}/{x}/{y}.png?key=YOUR-API-KEY';
var timestep = 1; // a value between 1 and 25

var layertmp2m = L.tileLayer( tmp2mUrl + timestep + urlSuffix, {
  maxZoom: 15,
  attribution: "Weather Forecast &copy; <a title='MetGIS Professional Weather Service' href='http://www.metgis.com/' target='_blank'>MetGIS</a>",
  opacity:0.7,
  subdomains: ["t1", "t2", "t3"]
});
```



#### Add UTFGrids


```javascript

// To integrate UTFGrids with Leaflet you have to use the Leaflet.utfgrid plugin.
// https://github.com/danzel/Leaflet.utfgrid

var tmp2mGridUrl = 'http://{s}.metgis.com/tmp2m_';
var gridUrlSuffix = '_grid/{z}/{x}/{y}.json?callback={cb}';
var timestep = 1; // a value between 1 and 25

var layertmp2mGrid = new L.UtfGrid(tmp2mGridUrl + timestep + gridUrlSuffix, {
  resolution: 4,
  pointerCursor: false,
  maxRequests:8,
  subdomains: ["t1", "t2", "t3"]
});
layertmp2mGrid.on('mouseover', function(e){console.log(e.data);});
```

Please refer to our [demo page](http://tiles.metgis.com/tiles-demo/) to view a fully functional example.

### OpenLayers 3

#### Simple example of temperature:

```javascript
var map = new ol.Map({
  target: 'map',
  layers: [
    new ol.layer.Tile({
      source: new ol.source.MapQuest({layer: 'sat'})
    }),
    new ol.layer.Tile({
      source: new ol.source.XYZ({
        url: 'http://t{1-3}.metgis.com/tmp2m_7/{z}/{x}/{y}.png',
        maxZoom:15,
        attributions: [
          new ol.Attribution({
            html: "Weather Forecast &copy; <a title='MetGIS Professional Weather Service' href='http://www.metgis.com/' target='_blank'>MetGIS</a>"
        })]
      }),
      opacity: 0.7
    })
  ],
  view: new ol.View({
    center: ol.proj.transform([12.41, 47.82], 'EPSG:4326', 'EPSG:3857'),
    zoom: 5
  })
});
```

#### With UTFGrids:

```javascript
var view =  new ol.View({
    center: ol.proj.transform([12.41, 47.82], 'EPSG:4326', 'EPSG:3857'),
    zoom: 5
  });
var gridSource = new ol.source.TileUTFGrid({
  url: 'http://tiles.metgis.com/meta/0.6/metgis_tilejsonwrapper.php?parameter=hr_p&timestep=7&key=YOUR-API-KEY'
});
var gridLayer = new ol.layer.Tile({source: gridSource});

var map = new ol.Map({
  target: 'map',
  layers: [
    new ol.layer.Tile({
      source: new ol.source.MapQuest({layer: 'sat'})
    }),
    new ol.layer.Tile({
      source: new ol.source.XYZ({
        url: 'http://t{1-3}.metgis.com/hr_p_7/{z}/{x}/{y}.png',
        maxZoom:15,
        attributions: [
          new ol.Attribution({
            html: "Weather Forecast &copy; <a title='MetGIS Professional Weather Service' href='http://www.metgis.com/' target='_blank'>MetGIS</a>"
        })]
      }),
      opacity: 0.7
    }),
    gridLayer
  ],
  view: view
});

var displayCountryInfo = function(coordinate) {
  var viewResolution = /** @type {number} */ (view.getResolution());
  gridSource.forDataAtCoordinateAndResolution(coordinate, viewResolution,
      function(data) {
        console.log(data);
      });
};

map.on('pointermove', function(evt) {
  if (evt.dragging) {
    return;
  }
  var coordinate = map.getEventCoordinate(evt.originalEvent);
  displayCountryInfo(coordinate);
});
```

**Note:** the `ol.source.TileUTFGrid` expects a [TileJSON](https://github.com/mapbox/tilejson-spec) file. We provide a PHP script that returns the appropriate data. Please submit the required fields to the query string to get the correct version.

`http://tiles.metgis.com/meta/0.6/metgis_tilejsonwrapper.php?parameter={parameter}&timestep={timestep}&key={YOUR-API-KEY}&callback={cb}`

Please refer to the official OpenLayers [TileUTFGrid Example](http://openlayers.org/en/v3.6.0/examples/tileutfgrid.html) for more information regarding the `ol.source.TileUTFGrid`.
