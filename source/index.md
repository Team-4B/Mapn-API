---
title: API Reference

language_tabs:
  - JavaScript

toc_footers:
  - <a href='http://github.com/tripit/slate'>Documentation Powered by Slate</a>
  - <a href='https://github.com/Team-4B/Mapn-API'>Documentation Source</a>

includes:
  - errors

search: true
---

# Introduction

Welcome to the Mapn RESTful API! You can use our API to access Mapn API endpoints.

### For geo locations, use [GeoJSON](http://geojson.org/) format

`"geometry": { "type": "Point", "coordinates": [125.6, 10.1]}`

Where coordinates are in the format `[lat, lng]`

### Please URL encode all parameters.

| Search query | URL encoded query |
| ------------ | ----------------- |
| #grocery #food | %23grocery+%23food |
| hello world :) | hello%20word%20%3A%29 |
| 2015-09-26T19:47:03Z | 2015-09-26T19%3A47%3A03Z |

Note: space character can be represented by + or %20.

### All datetime objects are UTC values represented in ISO-8601 format

`"2015-09-26T19:47:03Z"`

# Authentication

> To login, send a post request to `/auth/local` :

```JavaScript
$http.post('https://api.example.com/auth/local', {
          email: example@email.com,
          password: 12345
        }).
        success(function(data) {
          $cookieStore.put('token', data.token);
        });

```

Mapn uses authentication tokens to allow access to authenticated users. Guests have limited access to Mapn data.

Mapn expects for the token to be included in all authenticated API requests to the server in a header that looks like the following:

`Authorization:Bearer <token>`

<aside class="notice">
You must replace <code>&lt;token&gt;</code> with your session token returned by /auth
</aside>

# Pins

## Get all pins

```Angular
$http({
    method: 'GET',
    url: 'https://www.example.com/api/pins',
    params: {
        "limit": 10,
        "location": "125.6,10.2",
        "radius": 50
    },
    headers: {"Authorization": "Bearer xxxxYYYYZzzz"}
}).success(function(data){
    // With the data succesfully returned, do something
});
```

> The above command returns JSON structured like this:

```json
[
  {
    "pin_id": 1356674,
    "geo": { "type": "Point", "coordinates": [125.6, 10.1] },
    "name": "Bob's Burgers",
    "hashtags": ["burger", "restaurant"],
    "entries": [
      {
        "video_id": 123,
        "title": "Best Burger in Town",
        "url": "https://www.youtube.com/watch?v=GDcOfvVVyzE",
        "description": "I had a #burger",
        "likes": 523,
        "dislikes": 1,
        "hashtags": ["burger"],
        "posted_on": "2015-09-26T19:47:03Z"
      },
      { 
        "video_id": 128,
        "title": "Don't Eat Here",
        "url": "https://www.youtube.com/watch?v=sz9ncrDlALQ",
        "description": null,
        "likes": 0,
        "dislikes": 1,
        "hashtags": null,
        "posted_on": "2015-09-26T20:47:03Z"
      }
    ],
    "bounties": null
  },
  { 
    "pin_id": 1356676,
    "coordinates": { "type": "Point", "coordinates": [125.62, 10.1] },
    "name": "Super Zoo",
    "hashtags": ["zoo", "lions", "tigers", "bears" ],
    "entries": null,
    "bounties": [
      {
        "title": "Make a video featuring our lions",
        "reward": 5.00,
        "active": true,
        "posted_on": "2015-09-26T19:47:03Z"
      }
    ]
  }
]
```

This endpoint retrieves all pins by searching via parameters. 
If no parameters are specified, the 25 newest pins are returned.
It is highly recommended to specify a location and radius on every request.

Please be aware that a max of 200 pins can be returned in a single request.

### HTTP Request

`GET http://example.com/api/pins`

### Query Parameters

Parameter | Default | Description
--------- | ------- | -----------
location  |         | Used to search via lat,lng. **Example** 37.781157,-122.398720
radius    |         | A integer in meters that is used with `location` to search via lat,lng. **Maximum is 25000**
hashtag   |         | Search by hashtag. **Example** #hello #world
since     |         | Show items with timestamp *after* an ISO-8601 datetime 
until     |         | Show items with timestamp *before* an ISO-8601 datetime
count     |   25    | Specify number of pins to return. **Maximum is 200**
bounties_only | false | When set to `1` or `true`, only returns pins with an active bounty

**This request does not need to be authenticated**

## Get a Specific Pin

```Angular
$http.get('http://example.com/api/pins/1356674');
```

> The above command returns JSON structured like this:

```json
{
  "pin_id": 1356674,
  "coordinates": [-77.69531, 33.578],
  "name": "Bob's Burgers",
  "hashtags": ["burger", "restaurant"],
  "entries": null,
  "bounties": null
}
```

This endpoint retrieves a specific pin.

<aside class="warning">Note that some pins will return 403 Forbidden if they are hidden for admins only.</aside>

### HTTP Request

`GET http://example.com/api/pin/<ID>`

### URL Parameters

Parameter | Description
--------- | -----------
ID | The ID of the pin to retrieve

## Create or update a pin

This endpoint allows you to create or updated pins.
The payload must be a valid JSON object.
The payload is returned on success with an id

```Angular
payload = {
  "coordinates": [-77.69531, 33.578],
  "name": "Bob's Burgers",
  "hashtags": ["burger", "restaurant"]
};
$http.post('http://example.com/api/pins/1356674', payload);
```

> The above command returns JSON structured like this:

```json
{
  "pin_id": 1356674,
  "coordinates": [-77.69531, 33.578],
  "name": "Bob's Burgers",
  "hashtags": ["burger", "restaurant"],
  "entries": null,
  "bounties": null
}
```


### HTTP Request

`POST http://example.com/api/pin/<ID>`

### URL Parameters

Parameter | Description
--------- | -----------
ID | (optional) The ID of the pin to edit. If none is specified, creates a new pin

<aside class="warning">
    Note that some pins will return 403 Forbidden if they are hidden
    or you don't have permission to edit them
</aside>
