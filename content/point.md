## Point API

The following chapters describe the functions and usage of the MetGIS Point API. To request data from this API you need a valid user key. Click [here](#obtain-developer-key) for information on how to obtain a free dseveloper key. 

### Request Data

MetGIS forecasts can be retrieved from the point API via https with a request that follows this pattern:

```json
https://api.metgis.com/forecast?key={your-user-key}&lat={yy.yyyy}&lon={yyy.yyyy}&alt={hhhh}&v={version}&lang={language}
```
The curly brackets in this example have to be replaced by values according to the following table:

| Content | Description |
|---------|-------------|
|{your-user-key}|user key with valid access to MetGIS point API |
|{yy.yyyy}|Latitude of point for which the weather forecast is requsted (more than 4 fractional digits will be truncated)|
|{xxx.xxxx}|Longitude of point for which the weather forecast is requsted (more than 4 fractional digits will be truncated, accepted range:-180...360)|
|{hhhh}|Altitude of point for which the weather forecast is requested|
|{version}|Requested forecast version (see chapter [forecast versions](#available-forecast-versions) for options)|


---------------------------------------------

Forecasts are delivered are delivered in JSON or JSONP format.

### Available Forecast Versions

The following table lists all available forecast formats, and they will be described in detail in the following subsections.

| Name | Version Akronym | Description |
|---------|------------|------------|
|Complete3|a1|Three day forecast with a all basic weather Parameters and sunrise/sunset|
|Summary3|sum3|Three day forecast with dayly weather parameters and short description of weather|
|Summary4|sum4|Four day forecast with dayly weather parameters and short description of weather|
|Summary5|sum5|Five day forecast with dayly weather parameters and short description of weather|
------------------------------------------

### Forecast Complete3

This forecast version offers 3 hourly weather forecasts with sunrise and sunset, a data sample is shown on the right.

```json
{"Time_step (h)":3,"Description":"Metgis Point Forecast","Forecast_issued":"2016-07-14T06:00Z","Temperature (C)":[18,21,24,25,21,16,14,13,15,20,23,24,20,15,14,13,16,22,25,24,20,16,15,14,18,25,27,26],"Wind_direction_speed (m/s)":["NE3","NE3","NE2","NE2","NE3","NE3","NE4","NE4","NE3","NE3","NE4","NE4","N1","NE3","NE3","NE3","NE2","N1","SW2","SW3","W2","NE1","NE3","NE3","NE2","SW1","SW3","SW3"],"3hr_Snowfall (cm)":[null,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0],"h":200,"Weather_code":[null,4,1,2,2,1,2,2,2,2,2,2,2,1,2,2,2,2,2,2,1,1,1,1,1,1,1,1],"lon":13.567,"3hr_Precipitation (mm)":[null,1.1,0.1,0,0.1,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0],"Sunset":"2016-07-14T18:54Z","Total_cloud_cover (%)":[1,27,1,16,50,2,17,30,30,30,34,49,30,8,30,30,30,30,30,30,2,0,0,0,0,0,1,0],"Transition_height_snow_sleet (m)":[2600,2500,2600,2700,2600,2700,2700,2600,2600,2600,2500,2600,2600,2600,2700,2700,2800,2900,2700,2700,2800,2900,3000,3200,3300,3400,3500,3700],"Sunrise":"2016-07-14T03:28Z","lat":46.123}
```

Elements:
* Description: Description of service
* Forecast_issued: Starting time of forecast model run (UTC) 
* lat: Geographical latitude of point
* lon: Geographical longitude of point
* h: Height above sea level of point
* Sunrise: ISO-datestring (UTC) of sunrise for day
* Sunset: ISO-datestring (UTC) of sunset for day
* Time_Step (h): Length of time interval between forecast times
* Temperature (C): Temperature in ° Celsius, array with values for each forecast time, starting at the time the forecast was issued.
* 3hr_Precipitation (mm): Acuumulated precipitation of the 3 hour intervals, array with values for each forecast time. The value for the time the forecast is issued will always be null, and the second value denotes the precipitation for the interval [Forecast_issued Forecast_issued+3h], etc. 
* 3hr_Snowfall (cm): Acuumulated fresh snow of the 3 hour intervals, array with values for each forecast time. The value for the time the forecast is issued will always be null, and the second value denotes the precipitation for the interval [Forecast_issued Forecast_issued+3h], etc.
* Transition_height_snow_sleet (m): Height above sea level at which the snow turns into sleet when falling.
* Wind_direction_speed (m/s): Wind direction and wind speed in [m/s]. possible values for wind direction are: NW (north-west), N (north), NE (north-east), E (east), SE (south east), S (south), SW (south-west), W (west) and XX (calm or changeable).
* Total_cloud_cover (%): Cloud cover [%].
* Weather_code: Code of weather conditions. This value can be translated to icons or description of the weather conditions. The values are defined in the following table: 

| Weather Code | Precipitation [mm/3h] | Cloud cover [%] | Form of precipitation | Risk of thunderstorms |
|----|--------|---------|-------|-----------|
| 1 | 0 | N<10 | No precipitation |	No |
| 2 | 0	| 10<=N<70 | No precipitation | No |
| 3 | 0	| N>70	Kein Niederschlag	| 	No|
| 4 | <2 | 10<=N<70		| rain	| 	No|
| 5 | <2 | 	N>70		| rain	| 	No|
| 6 | <2	| 	10<=N<70	| 	rain	| 	Yes|
| 7 | <2	| 	N>70	| 	rain	| 	Yes|
| 8 | >=2	| 	10<=N<70	| 	heavy rain		| No|
| 9 | >=2	| 	N>70	| 	heavy rain	| 	No|
| 10 | >=2	| 	10<=N<70	| 	heavy rain		| Yes|
| 11 | >=2	| 	N>70		| heavy rain	| 	Yes|
| 12 | <2	| 	10<=N<70	| 	sleet	| 	No|
| 13 | <2	| 	N>70	| 	sleet	| 	No|
| 14 | <2	| 	10<=N<70	| 	sleet	| 	Yes|
| 15 | <2	| 	N>70	| 	sleet	| 	Yes|
| 16 | >=2	| 	10<=N<70		| heavy sleet		| No|
| 17 | >=2	| 	N>70	| 	heavy sleet	| 	No|
| 18 | >=2	| 	10<=N<70	| 	heavy sleet		| Yes|
| 19 | >=2	| 	N>70		| heavy sleet		| Yes|
| 20 | <2	| 	10<=N<70	| 	snow fall	| 	No|
| 21 | <2	| 	N>70		| snow fall		| No|
| 22 | <2	| 	10<=N<70		| snow fall	| 	Yes|
| 23 | <2	| 	N>70	| 	snow fall		| Yes|
| 24 | >=2	| 	10<=N<70		| heavy snow fall		| No|
| 25 | >=2	| 	N>70	| 	heavy snow fall	| 	No|
| 26 | >=2	| 	10<=N<70		| heavy snow fall		| Yes|
| 27 | >=2	| 	N>70		| heavy snow fall	| 	Yes|
----------------------------------------------------------

### Forecasts Summary3, Summary4 and Summary5

Json forecast files with dayly values describing the weather for 3, 4 or 5 days. A few optional parameters can ba appended to the request to choose different languages and units. The available optional parameters are shown in the following table:

| Optional parameter | Description | Possible options |
|---------|-------------|-----------|
|lang|Optional parameter to choose language of forecast file |'de' for german (default) or 'en' for english|
|tempU|Optional parameter to choose the temperature unit used in the forecast file|'c' for °C (default) or 'f' for °F|
|linU|Optional parameter to choose the length unit used in the forecast file|'m' yields precipitation in mm/day and fresh snow in cm/day (default) or 'ft' which yields precipitation and fresh snow in inch|
|windU|Optional parameter to choose the wind speed unit|'kmh' for km/h (default), 'ms' for m/s, 'mph' for miles/hour and 'bft' for beaufort|
------------------------------------------------------



An example for a request is shown on the right with the resulting JSON underneath. 
```json
https://api.metgis.com/forecast?key=0dfa6e5a4a672b3a7c79e7c2ef&lat=46.123&lon=13.567&alt=200&v=plugin3&lang=en&tempU=f&linU=m&windU=ms
```
```json
{"sunrise":["2016-07-18T03:32Z","2016-07-19T03:33Z","2016-07-20T03:34Z"],"dayCount":3,"weatherIcon":["sun_cloud_bright_rain_thunder","sun_cloud_bright_rain_thunder","sun_cloud_bright_rain_thunder"],"snowfall":[0,0,0],"unitLin":"m","maxTemp":[82,86,84],"description":"Metgis Point Forecast - Summary 3 Days","lon":13.567,"windDir":["SW","NO","SW"],"sunshineDuration":[14.2,15.2,15.2],"precipitation":[4.5,5.5,1.5],"windSpeed":[3,3,3],"lang":"en","lat":46.123,"rainfall":[4.5,5.5,1.5],"unitWind":"m/s","alt":200,"forecastIssued":"2016-07-18T06:00Z","unitTemp":"F","sunset":["2016-07-18T18:51Z","2016-07-19T18:50Z","2016-07-20T18:49Z"],"relativeHumidity":[65,72,75],"forecastShortText":["thundery showers possible","thundery showers possible","thundery showers possible"]}
```
The JSON file contains the following elements:
* sunrise: Array of ISO-datestrings showing time of sunrise for each day.
* dayCount: Integer showing how many days are described in the forecast file.
* weatherIcon: Name of weather icon that describes the weather conditions on the respective day.
* snowfall: Amount of fresh snow for each day (unit can be choosen via the parameter linU, default is cm).
* unitLin: Linear unit used in forecast file (see table above for options and description).
* maxTemp: Array of maximum temperatures for each day (see table above for units and options).
* description: Description of JSON file. 
* lon: Geographical longitude the forecast was calculated for.
* windDir: Array of wind directions for each day.
* sunshineDuration: Sunshine duration for each day in hours.
* precipitation: Árray of accumulated dayly precipitations (unit can be choosen via the parameter linU, default is mm)
* windSpeed: Array with wind speed for each day (see table above for units and options).
* lang: Shows the language used in forecast file (see table above for units and options).
* lat: Geographical latitude the forecast was calculated for.
* rainfall: Array of accumulated rainfall for each day (unit can be choosen via the parameter linU, default is mm)
* unitWind: Unit used for wind speed (see table above for options and description).
* alt: Altitude of the point the forecast was calculated for (m above sea level).
* forecastIssued: Initial time of forecast
* minTemp: Array of minimum temperatures for each day (see table above for units and options).
* unitTemp: Unit of temperatures (see table above for options and description).
* sunset: Array of ISO-datestrings showing time of sunset for each day.
* forecastShortText: Array of short texts describing the weather for each day.