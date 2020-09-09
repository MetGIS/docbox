## HistAPI

The MetGIS HistApi provides historical weather data.

### Request Data


```endpoint
GET https://api.hist.metgis.com/histapiserverRest/histdata?key={your-user-key}&lat={latitude}&lon={longitude}&time={date}&v={version}
```

#### Example Request 

```curl
curl "https://api.hist.metgis.com/histapiserverRest/histdata?key={your-user-key}&lat={latitude}&lon={longitude}&time={date}&v={version}"
```

##### Browser

```javascript
fetch('https://api.hist.metgis.com/histapiserverRest/histdata?key={your-user-key}&lat={latitude}&lon={longitude}&time={date}&v={version}')
  .then(response => response.json())
  .then(data => console.log(data))
  .catch(err => console.error(err))
```

##### Node

```javascript
const got = require('got')

got('https://api.hist.metgis.com/histapiserverRest/histdata?key={your-user-key}&lat={latitude}&lon={longitude}&time={date}&v={version}')
  .json()
  .then(res => console.log(res))
  .catch(err => console.error(err))
```

| Query Param | Description |
|---------|-------------|
| **key** | your access token with valid access to MetGIS Hist API |
| **lat** | latitude: a range defined by two lat values for ex.: "`50_50.5`" or a single lat value for ex.: "`50`" |
| **lon** | longitude: a range defined by two lon values for ex.: "`12_12.5`" or a single lon value for ex.: "`12`"|
| **time** | date: a range defined by two date strings for ex.: "`201305030000_201305100000`" or a single date string for ex.: "`201305030000`"
| **v** | Requested version (see chapter [HistAPI Versions](#available-histapi-versions) for options)|


### Available HistAPI Versions

| Query Param | Description |
|--------------|------------|
| **hitemp** | [Temperature](#hitemp) |
| **hitempsum** | [Daily Min/Max/Mean Temperature](#daily-min/max/mean-temperature) |
| **hirh** | [Relative Humidity](#relative-humidity) |
| **hihrp** | [Precipitation](#precipitation) |
| **hiwind** | [Wind](#wind) |
| **hidswr** | [Downward Short Wave Radiation Flux](#downward-short-wave-radiation-flux) |
| **hicld** | [Total Cloud Cover](#total-cloud-cover) |
| **hidsum** | [Weather Data Daily Summary](#weather-data-daily-summary) |

### Temperature

Json result with hourly temperature data.

#### Example Request

```javascript
fetch("https://api.hist.metgis.com/histapiserverRest/histdata?key={token}&lon=12&lat=50&time=201305030000_201305030200&v=hitemp")
  .then(response => response.json())
  .then(data => console.log(data))
```

```curl
curl "https://api.hist.metgis.com/histapiserverRest/histdata?key={token}&lon=12&lat=50&time=201505040000_201505040300&v=hiwind"
```

#### Example Result

```json
{
  "Info": "Historical Wind Data",
  "data": [
    {
      "lon": 12,
      "lat": 50,
      "alt": 658,
      "date": "20150504 08:00",
      "wdir": 243,
      "wspd": 4.5
    }
  ],
  "Info_wdir": "Wind Direction (in °)",
  "Info_wspd": "Wind Speed (in m/s)",
  "Info_lat": "latitude of the data point (in degrees)",
  "Info_lon": "longitude of the data point (in degrees)",
  "Info_alt": "altitude of the data point (in m above mean sea level)"
}
```

The entries of the json result data array contain the following properties:

| Property | Description                                      |
|----------|--------------------------------------------------|
| **lon**  | geographical longitude of point                  |
| **lat**  | geographical latitude of point                   |
| **alt**  | altitude of point in meters                      |
| **date** | the wind values correspond to this date          |
| **wdir** | wind direction in °                              |
| **wspd** | wind speed in m/s                                |


### Daily Min/Max/Mean Temperature

Json result with daily minimum, maximum and mean temperature data.

#### Example Request

```javascript
fetch("https://api.hist.metgis.com/histapiserverRest/histdata?key={token}&lon=12&lat=50&time=201305030000_201305040000&v=hitempsum")
  .then(response => response.json())
  .then(data => console.log(data))
```

```curl
curl "https://api.hist.metgis.com/histapiserverRest/histdata?key={token}&lon=12&lat=50&time=201305030000_201305040000&v=hitempsum"
```

#### Example Result

```json
{
  "Info": "Historical Temperature Data",
  "data": [
    {
      "lon": 12.0,
      "lat": 50.0,
      "alt": 658,
      "date": "20130503",
      "tmp_mean": 8.0,
      "tmp_max": 10.6,
      "tmp_min": 5.2
    },
    {
      "lon": 12.0,
      "lat": 50.0,
      "alt": 658,
      "date": "20130504",
      "tmp_mean": 8.0,
      "tmp_max": 9.8,
      "tmp_min": 6.7
    }
  ],
  "Info_tmp_mean": "mean temperature of the day (in °C)",
  "Info_tmp_min": "minimum temperature of the day (in °C)",
  "Info_tmp_max": "maximum temperature of the day (in °C)",
  "Info_lat": "latitude of the data point (in degrees)",
  "Info_lon": "longitude of the data point (in degrees)",
  "Info_alt": "altitude of the data point (in m above mean sea level)"
}
```

The entries of the json result data array contain the following properties:

| Property      | Description                                       |
|---------------|---------------------------------------------------|
| **lon**       | geographical longitude of point                   |
| **lat**       | geographical latitude of point                    |
| **alt**       | altitude of point in meters                       |
| **date**      | the temperature values corresponds to this date   |
| **tmp_mean**  | mean temperature of the day in °C                 |
| **tmp_max**   | maximum temperature of the day in °C              |
| **tmp_min**   | minimum temperature of the day in °C              |

### Relative Humidity

Json result with hourly relative humidity data.

#### Example Request

```javascript
fetch("https://api.hist.metgis.com/histapiserverRest/histdata?key={token}&lon=12&lat=50&time=201305030000_201305030200&v=hirh")
  .then(response => response.json())
  .then(data => console.log(data))
```

```curl
curl "https://api.hist.metgis.com/histapiserverRest/histdata?key={token}&lon=12&lat=50&time=201305030000_201305030200&v=hirh"
```

#### Example Result

```json
{
  "Info": "Historical Relative Humidity Data",
  "data": [
    {
      "lon": 12.0,
      "lat": 50.0,
      "alt": 658,
      "date": "20130503 00:00",
      "rh": 98
    },
    {
      "lon": 12.0,
      "lat": 50.0,
      "alt": 658,
      "date": "20130503 01:00",
      "rh": 97
    },
    {
      "lon": 12.0,
      "lat": 50.0,
      "alt": 658,
      "date": "20130503 02:00",
      "rh": 98
    }
  ],
  "Info_rh": "Humidity (in %)",
  "Info_lat": "latitude of the data point (in degrees)",
  "Info_lon": "longitude of the data point (in degrees)",
  "Info_alt": "altitude of the data point (in m above mean sea level)"
}
```

The entries of the json result data array contain the following properties:

| Property      | Description                                       |
|---------------|---------------------------------------------------|
| **lon**       | geographical longitude of point                   |
| **lat**       | geographical latitude of point                    |
| **alt**       | altitude of point in meters                       |
| **date**      | the humidity value corresponds to this date       |
| **rh**        | Humidity in %                                     |

### Precipitation

Json result with hourly precipitation data.

#### Example Request

```javascript
fetch("https://api.hist.metgis.com/histapiserverRest/histdata?key={token}&lon=12&lat=50&time=201305040800_201305041000&v=hihrp")
  .then(response => response.json())
  .then(data => console.log(data))
```

```curl
curl "https://api.hist.metgis.com/histapiserverRest/histdata?key={token}&lon=12&lat=50&time=201305040800_201305041000&v=hihrp"
```

#### Example Result

```json
{
  "Info": "Historical Precipitation Data",
  "data": [
    {
      "lon": 12.0,
      "lat": 50.0,
      "alt": 658,
      "date": "20130504 08:00",
      "hrp": 0.0
    },
    {
      "lon": 12.0,
      "lat": 50.0,
      "alt": 658,
      "date": "20130504 09:00",
      "hrp": 0.1
    },
    {
      "lon": 12.0,
      "lat": 50.0,
      "alt": 658,
      "date": "20130504 10:00",
      "hrp": 1.1
    }
  ],
  "Info_hrp": "Hourly Precipitation (in mm)",
  "Info_lat": "latitude of the data point (in degrees)",
  "Info_lon": "longitude of the data point (in degrees)",
  "Info_alt": "altitude of the data point (in m above mean sea level)"
}
```

The entries of the json result data array contain the following properties:

| Property      | Description                                       |
|---------------|---------------------------------------------------|
| **lon**       | geographical longitude of point                   |
| **lat**       | geographical latitude of point                    |
| **alt**       | altitude of point in meters                       |
| **date**      | the end date of the corresponding one hour range  |
| **hrp**       | precipitation amount in mm of the last hour       |

### Wind

Json result with hourly wind data.

#### Example Request

```javascript
fetch("https://api.hist.metgis.com/histapiserverRest/histdata?key={token}&lon=12&lat=50&time=201505040000_201505040300&v=hiwind")
  .then(response => response.json())
  .then(data => console.log(data))
```

```curl
curl "https://api.hist.metgis.com/histapiserverRest/histdata?key={token}&lon=12&lat=50&time=201305030000_201305030200&v=hiwind"
```

#### Example Result

```json
{
  "Info": "Historical Temperature Data",
  "data": [
    {
      "lon": 12.0,
      "lat": 50.0,
      "alt": 658,
      "date": "20130503 00:00",
      "tmp": 5.2
    },
    {
      "lon": 12.0,
      "lat": 50.0,
      "alt": 658,
      "date": "20130503 01:00",
      "tmp": 5.3
    },
    {
      "lon": 12.0,
      "lat": 50.0,
      "alt": 658,
      "date": "20130503 02:00",
      "tmp": 5.3
    }
  ],
  "Info_lat": "latitude of the data point (in degrees)",
  "Info_lon": "longitude of the data point (in degrees)",
  "Info_alt": "altitude of the data point (in m above mean sea level)",
  "Info_tmp": "Temperature (in °C)"
}
```

The entries of the json result data array contain the following properties:

| Property | Description                                      |
|----------|--------------------------------------------------|
| **lon**  | geographical longitude of point                  |
| **lat**  | geographical latitude of point                   |
| **alt**  | altitude of point in meters                      |
| **date** | the temperature value corresponds to this date   |
| **tmp**  | temperature value in °C                          |

### Downward Short Wave Radiation Flux

Json result with hourly "downward short wave radiation flux" data.

#### Example Request

```javascript
fetch("https://api.hist.metgis.com/histapiserverRest/histdata?key={token}&lon=12&lat=50&time=201305030900_201305031100&v=hidswr")
  .then(response => response.json())
  .then(data => console.log(data))
```

```curl
curl "https://api.hist.metgis.com/histapiserverRest/histdata?key={token}&lon=12&lat=50&time=201305030900_201305031100&v=hidswr"
```

#### Example Result

```json
{
  "Info": "Historical Downward Short Wave Radiation Flux Data",
  "data": [
    {
      "lon": 12.0,
      "lat": 50.0,
      "alt": 658,
      "date": "20130503 09:00",
      "dswr": 398
    },
    {
      "lon": 12.0,
      "lat": 50.0,
      "alt": 658,
      "date": "20130503 10:00",
      "dswr": 432
    },
    {
      "lon": 12.0,
      "lat": 50.0,
      "alt": 658,
      "date": "20130503 11:00",
      "dswr": 298
    }
  ],
  "Info_dswr": "Downward Short Wave Radiation Flux (in W/m^2)",
  "Info_lat": "latitude of the data point (in degrees)",
  "Info_lon": "longitude of the data point (in degrees)",
  "Info_alt": "altitude of the data point (in m above mean sea level)"
}
```

The entries of the json result data array contain the following properties:

| Property      | Description                                       |
|---------------|---------------------------------------------------|
| **lon**       | geographical longitude of point                   |
| **lat**       | geographical latitude of point                    |
| **alt**       | altitude of point in meters                       |
| **date**      | the data value corresponds to this date           |
| **dswr**      | downward short wave radiation flux in w/m^2       |

### Total Cloud Cover

Json result with hourly cloud cover data.

#### Example Request

```javascript
fetch("https://api.hist.metgis.com/histapiserverRest/histdata?key={token}&lon=12&lat=50&time=201305030000_201305030200&v=hicld")
  .then(response => response.json())
  .then(data => console.log(data))
```

```curl
curl "https://api.hist.metgis.com/histapiserverRest/histdata?key={token}&lon=12&lat=50&time=201305030000_201305030200&v=hicld"
```

#### Example Result

```json
{
  "Info": "Historical Total Cloud Cover Data",
  "data": [
    {
      "lon": 12.0,
      "lat": 50.0,
      "alt": 658,
      "date": "20130503 00:00",
      "cld": 80
    },
    {
      "lon": 12.0,
      "lat": 50.0,
      "alt": 658,
      "date": "20130503 01:00",
      "cld": 100
    },
    {
      "lon": 12.0,
      "lat": 50.0,
      "alt": 658,
      "date": "20130503 02:00",
      "cld": 97
    }
  ],
  "Info_cld": "Total Cloud Cover (in %)",
  "Info_lat": "latitude of the data point (in degrees)",
  "Info_lon": "longitude of the data point (in degrees)",
  "Info_alt": "altitude of the data point (in m above mean sea level)"
}
```

The entries of the json result data array contain the following properties:

| Property      | Description                                       |
|---------------|---------------------------------------------------|
| **lon**       | geographical longitude of point                   |
| **lat**       | geographical latitude of point                    |
| **alt**       | altitude of point in meters                       |
| **date**      | the data value corresponds to this date           |
| **cld**       | total cloud cover in %                            |

### Weather Data Daily Summary

Json result with a daily weather data summary.

#### Example Request

```javascript
fetch("https://api.hist.metgis.com/histapiserverRest/histdata?key={token}&lon=12&lat=50&time=201305030000_201305040000&v=hidsum")
  .then(response => response.json())
  .then(data => console.log(data))
```

```curl
curl "https://api.hist.metgis.com/histapiserverRest/histdata?key={token}&lon=12&lat=50&time=201305030000_201305040000&v=hidsum"
```

#### Example Result

```json
{
  "Info": "Historical Weather Data Daily Summary",
  "data": [
    {
      "lon": 12.0,
      "lat": 50.0,
      "alt": 658,
      "date": "20130503",
      "tmp_mean": 8.0,
      "tmp_max": 10.6,
      "tmp_min": 5.2,
      "prec_sum": 0.0,
      "wind_dir_weighted": 33.0,
      "wind_dir_max": 18.0,
      "wind_speed_mean": 5.0,
      "wind_speed_max": 6.3,
      "wind_speed_min": 3.2,
      "cloud_cover_mean": 89.0,
      "cloud_cover_max": 100.0,
      "cloud_cover_min": 26.0,
      "sun_dur": 1.47,
      "rel_hum_mean": 82.0,
      "rel_hum_max": 99.0,
      "rel_hum_min": 66.0
    },
    {
      "lon": 12.0,
      "lat": 50.0,
      "alt": 658,
      "date": "20130504",
      "tmp_mean": 8.0,
      "tmp_max": 9.8,
      "tmp_min": 6.7,
      "prec_sum": 6.8,
      "wind_dir_weighted": 117.0,
      "wind_dir_max": 124.0,
      "wind_speed_mean": 2.0,
      "wind_speed_max": 3.6,
      "wind_speed_min": 0.0,
      "cloud_cover_mean": 81.0,
      "cloud_cover_max": 100.0,
      "cloud_cover_min": 0.0,
      "sun_dur": 2.19,
      "rel_hum_mean": 85.0,
      "rel_hum_max": 94.0,
      "rel_hum_min": 75.0
    }
  ],
  "Info_tmp_mean": "mean temperature of the day (in °C)",
  "Info_tmp_min": "minimum temperature of the day (in °C)",
  "Info_tmp_max": "maximum temperature of the day (in °C)",
  "Info_prec_sum": "total precipitation accumulated during the day (in mm)",
  "Info_wind_dir_max": "wind direction of maximum wind (in degrees clockwise from north)",
  "Info_wind_dir_weighted": "prevailing wind direction during day (in degrees clockwise from north, -9999 means rotating)",
  "Info_wind_speed_mean": "mean wind speed (in m/s)",
  "Info_wind_speed_max": "maximum wind speed (in m/s)",
  "Info_wind_speed_min": "minimum wind speed (in m/s)",
  "Info_cloud_cover_mean": "mean cloud cover (in %)",
  "Info_cloud_cover_max": "maximum cloud cover (in %)",
  "Info_sun_dur": "accumulated sunshine duration (in h)",
  "Info_rel_hum_mean": "mean relative humidity (in %)",
  "Info_rel_hum_max": "maximum relative humidity (in %)",
  "Info_rel_hum_min": "minimum relative humidity (in %)",
  "Info_lat": "latitude of the data point (in degrees)",
  "Info_lon": "longitude of the data point (in degrees)",
  "Info_alt": "altitude of the data point (in m above mean sea level)",
  "Info_cloud_cover_min": "minimum cloud cover (in %)"
}
```

The entries of the json result data array contain the following properties:

| Property              | Description                                       |
|-----------------------|---------------------------------------------------|
| **lon**               | geographical longitude of point                   |
| **lat**               | geographical latitude of point                    |
| **alt**               | altitude of point in meters                       |
| **date**              | the data values correspond to this date           |
| **tmp_mean**          | mean temperature in °C                            |
| **tmp_max**           | maximum temperature in °C                         |
| **tmp_min**           | minimu temperature in °C                          |
| **prec_sum**          | accumulated precipitation in mm                   |
| **wind_dir_weighted** | prevailing wind direction                         |
| **wind_dir_max**      | wind direction of maximum wind                    |
| **wind_speed_mean**   | mean wind speed in m/s                            |
| **wind_speed_max**    | maximum wind speed in m/s                         |
| **wind_speed_min**    | minimum wind speed in m/s                         |
| **cloud_cover_mean**  | mean cloud cover in %                             |
| **cloud_cover_max**   | maximum cloud cover in %                          |
| **cloud_cover_min**   | minimum cloud cover in %                          |
| **sun_dur**           | accumulated sunshine duration in h                |
| **rel_hum_mean**      | mean relative humidity in %                       |
| **rel_hum_max**       | maximum relative humidity in %                    |
| **rel_hum_min**       | minimum relative humidity in %                    |

