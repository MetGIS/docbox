## Tiles API

The MetGIS Weather Maps API offers forecasts as a Tile Maps Service (TMS). This standardized format enables you to integrate our weather predictions for Central Europe into any dynamic map on your website or within your app. Check our [demo page](http://tiles.metgis.com/tiles-demo/) to see an example of weather data layers integrated in zoomable maps.

Our forecasts are updated every 24 hours. Once a day, at approximately 4:00 UTC, a new forecast is available, covering the following 75 hours in three-hour time intervals. Beside regular tiles (in PNG-format) we also provide UTFGrid Tiles to enable a fast numerical representation of our colored maps.

![Coverage of MetGIS Maps API](./img/coverage_thumb.jpg)

The following sections provide detailed information about our Maps API, with various [examples](#examples) showing how to include our weather in your maps.

### Weather Parameters

The following weather parameters are currently available:

| Code | Parameter |
|------|-----------|
| `tmp2m` | Temperature |
| `hr_p` | Precipitation |
| `hr_sn` | Fresh Snow |
| `tcdcprs` | Cloudiness |
| `wind` | Wind |

TODO: cumu??
Some more parameters (cumulative snow and precipitation for time spans of one to three days) are currently under development and will be available soon.

### API Call for Tiles

To view our weather tiles within your website or application you need to call the following address with parameters specified according to your requirements:

```json
http://{t1-t3}.metgis.com/tilestache/{parameter}_{timestep}/{z}/{x}/{y}.png?key={YOUR-API-KEY}
```

Parameters:

TODO: Find better word for parameter - property?

| Parameter | Description |
|-----------|-------------|
| `{t1-t3}` | subdomains to get around browser limitations on the number of simultaneous HTTP connections to each host |
| `{parameter}` | the code of the desired weather parameter as described in `meta.json` |
| `{timestep}` | value of the desired time step, starting at 1 which reflects the weather forecasts for the time  `forecastIssued`, see [meta.json](#meta.json) |
| `{x}`, `{y}`, `{z}` | x,y coordinates and zoom of a tile |
| `{YOUR-API-KEY}` | your unique API-Key, [more information](#api-key) |

Please check our [sample code](#examples) to see how to place the code line above in the right context.

### List wobbles

Lists all wobbles for a particular account.

```endpoint
GET /wobbles/v1/{username} wobbles:read
```

#### Example request

```curl
$ curl https://wobble.biz/wobbles/v1/{username}
```

```bash
$ wbl wobbles list
```

```javascript
client.listWobbles(function(err, wobbles) {
  console.log(wobbles);
});
```

```python
wobbles.list()
```

#### Example response

```json
[
  {
    "owner": "{username}",
    "id": "{wobble_id}",
    "created": "{timestamp}",
    "modified": "{timestamp}"
  },
  {
    "owner": "{username}",
    "id": "{wobble_id}",
    "created": "{timestamp}",
    "modified": "{timestamp}"
  }
]
```

### Create wobble

Creates a new, empty wobble.

```endpoint
POST /wobbles/v1/{username}
```

#### Example request

```curl
curl -X POST https://wobble.biz/wobbles/v1/{username}
```

```bash
$ wbl wobbles create
```

```javascript
client.createWobble({
  name: 'example',
  description: 'An example wobble'
}, function(err, wobble) {
  console.log(wobble);
});
```

```python
response = wobbles.create(
  name='example', description='An example wobble')
```

#### Example request body

```json
{
  "name": "foo",
  "description": "bar"
}
```

Property | Description
---|---
`name` | (optional) the name of the wobble
`description` | (optional) a description of the wobble

#### Example response

```json
{
  "owner": "{username}",
  "id": "{wobble_id}",
  "name": null,
  "description": null,
  "created": "{timestamp}",
  "modified": "{timestamp}"
}
```

### Retrieve a wobble

Returns a single wobble.

```endpoint
GET /wobbles/v1/{username}/{wobble_id}
```

Retrieve information about an existing wobble.

#### Example request

```curl
curl https://wobble.biz/wobbles/v1/{username}/{wobble_id}
```

```bash
$ wbl wobble read-wobble wobble-id
```

```python
attrs = wobbles.read_wobble(wobble_id).json()
```

```javascript
client.readWobble('wobble-id',
  function(err, wobble) {
    console.log(wobble);
  });
```

#### Example response

```json
{
  "owner": "{username}",
  "id": "{wobble_id}",
  "created": "{timestamp}",
  "modified": "{timestamp}"
}
```

### Update a wobble

Updates the properties of a particular wobble.

```endpoint
PATCH /wobbles/v1/{username}/{wobble_id}
```

#### Example request

```curl
curl --request PATCH https://wobble.biz/wobbles/v1/{username}/{wobble_id} \
  -d @data.json
```

```python
resp = wobbles.update_wobble(
  wobble_id,
  name='updated example',
  description='An updated example wobble'
  ).json()
```

```bash
$ wbl wobble update-wobble wobble-id
```

```javascript
var options = { name: 'foo' };
client.updateWobble('wobble-id', options, function(err, wobble) {
  console.log(wobble);
});
```

#### Example request body

```json
{
  "name": "foo",
  "description": "bar"
}
```

Property | Description
---|---
`name` | (optional) the name of the wobble
`description` | (optional) a description of the wobble

#### Example response

```json
{
  "owner": "{username}",
  "id": "{wobble_id}",
  "name": "foo",
  "description": "bar",
  "created": "{timestamp}",
  "modified": "{timestamp}"
}
```

### Delete a wobble

Deletes a wobble, including all wibbles it contains.

```endpoint
DELETE /wobbles/v1/{username}/{wobble_id}
```

#### Example request

```curl
curl -X DELETE https://wobble.biz/wobbles/v1/{username}/{wobble_id}
```

```bash
$ wbl wobble delete-wobble wobble-id
```

```python
resp = wobbles.delete_wobble(wobble_id)
```

```javascript
client.deleteWobble('wobble-id', function(err) {
  if (!err) console.log('deleted!');
});
```

#### Example response

> HTTP 204

### List wibbles

List all the wibbles in a wobble. The response body will be a
WobbleCollection.

```endpoint
GET /wobbles/v1/{username}/{wobble_id}/wibbles
```

#### Example request

```curl
curl https://wobble.biz/wobbles/v1/{username}/{wobble_id}/wibbles
```

```bash
$ wbl wobble list-wibbles wobble-id
```

```python
collection = wobbles.list_wibbles(wobble_id).json()
```

```javascript
client.listWobbles('wobble-id', {}, function(err, collection) {
  console.log(collection);
});
```

#### Example response

```json
{
  "type": "Wobble",
  "wibbles": [
    {
      "id": "{wibble_id}",
      "type": "Wobble",
      "properties": {
        "prop0": "value0"
      }
    },
    {
      "id": "{wibble_id}",
      "type": "Wobble",
      "properties": {
        "prop0": "value0"
      }
    }
  ]
}
```

### Insert or update a wibble

Inserts or updates a wibble in a wobble. If there's already a wibble
with the given ID in the wobble, it will be replaced. If there isn't
a wibble with that ID, a new wibble is created.

```endpoint
PUT /wobbles/v1/{username}/{wobble_id}/wibbles/{wibble_id}
```

#### Example request

```curl
curl https://wobble.biz/wobbles/v1/{username}/{wobble_id}/wibbles/{wibble_id} \
  -X PUT \
  -d @file.geojson
```

```bash
$ wbl wobble put-wibble wobble-id wibble-id 'geojson-wibble'
```

```javascript
var wibble = {
  "type": "Wobble",
  "properties": { "name": "Null Island" }
};
client.insertWobble(wibble, 'wobble-id', function(err, wibble) {
  console.log(wibble);
});
```

#### Example request body

```json
{
  "id": "{wibble_id}",
  "type": "Wobble",
  "properties": {
    "prop0": "value0"
  }
}
```

Property | Description
--- | ---
`id` | the id of an existing wibble in the wobble

#### Example response

```json
{
  "id": "{wibble_id}",
  "type": "Wobble",
  "properties": {
    "prop0": "value0"
  }
}
```

### Retrieve a wibble

Retrieves a wibble in a wobble.

```endpoint
GET /wobbles/v1/{username}/{wobble_id}/wibbles/{wibble_id}
```

#### Example request

```curl
curl https://wobble.biz/wobbles/v1/{username}/{wobble_id}/wibbles/{wibble_id}
```

```bash
$ wbl wobble read-wibble wobble-id wibble-id
```

```javascript
client.readWobble('wibble-id', 'wobble-id',
  function(err, wibble) {
    console.log(wibble);
  });
```

```python
wibble = wobbles.read_wibble(wobble_id, '2').json()
```

#### Example response

```json
{
  "id": "{wibble_id}",
  "type": "Wobble",
  "properties": {
    "prop0": "value0"
  }
}
```

### Delete a wibble

Removes a wibble from a wobble.

```endpoint
DELETE /wobbles/v1/{username}/{wobble_id}/wibbles/{wibble_id}
```

#### Example request

```javascript
client.deleteWobble('wibble-id', 'wobble-id', function(err, wibble) {
  if (!err) console.log('deleted!');
});
```

```curl
curl -X DELETE https://wobble.biz/wobbles/v1/{username}/{wobble_id}/wibbles/{wibble_id}
```

```python
resp = wobbles.delete_wibble(wobble_id, wibble_id)
```

```bash
$ wbl wobble delete-wibble wobble-id wibble-id
```

#### Example response

> HTTP 204
