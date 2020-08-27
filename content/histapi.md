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

```javascript
fetch("https://api.hist.metgis.com/histapiserverRest/histdata?key={your-user-key}&lat={latitude}&lon={longitude}&time={date}&v={version}")
  .then(response => response.json())
  .then(data => console.log(data))
```

| Query Param | Description |
|---------|-------------|
| **key** | user key with valid access to MetGIS Hist API |
| **lat** | latitude: a range defined by two lat values for ex.: "`50_50.5`" or a single lat value for ex.: "`50`" |
| **lon** | longitude: a range defined by two lon values for ex.: "`12_12.5`" or a single lon value for ex.: "`12`"|
| **time** | date: a range defined by two date strings for ex.: "`201305030000_201305100000`" or a single date string for ex.: "`201305030000`"
| **v** | Requested version (see chapter [HistAPI Versions](#available-histapi-versions) for options)|


### Available HistAPI Versions

| Query Param | Description |
|--------------|------------|
| **hitemp** | [Temperature](#hitemp) |
| **hitempsum** | Daily Min/Max/Mean Temperature |
| **hirh** | Relative Humidity |
| **hihrp** | Precipitation |
| **hiwind** | --- |
| **hidswr** | Downward Short Wave Radiation Flux |
| **hicld** | Total Cloud Cover |
| **hidsum** | Weather Data Daily Summary |

### hitemp

### hitempsum

### hirh

### hihrp

### hiwind

### hidswr

### hicld

### hidsum



