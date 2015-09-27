---
title: API Reference

language_tabs:
  - JavaScript

toc_footers:
  - <a href='http://github.com/tripit/slate'>Documentation Powered by Slate</a>

includes:
  - errors

search: true
---

# Introduction

Welcome to the Mapn RESTful API! You can use our API to access Mapn API endpoints.

** Geo
All coordinates use [GeoJSON](http://geojson.org/) format

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

## Get all pins near a location

```Angular
$http.get('http://example.com/api/pins');
```

> The above command returns JSON structured like this:

```json
[
  {
    "pin_id": 1356674,
    "coordinates": [-77.69531, 33.578],
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
        "hashtags": ["burger"]
      },
      { 
        "video_id": 128,
        "title": "Don't Eat Here",
        "url": "https://www.youtube.com/watch?v=sz9ncrDlALQ",
        "description": null,
        "likes": 0,
        "dislikes": 1,
        "hashtags": null
      }
    ],
    "bounties": null
  },
  { 
    "pin_id": 1356676,
    "coordinates": [-77.69535, 33.57823],
    "name": "Super Zoo",
    "hashtags": ["zoo", "lions", "tigers", "bears" ],
    "entries": null,
    "bounties": [
      {
        "title": "Make a video featuring our lions",
        "reward": 5.00,
        "active": true
      }
    ]
  }
]
```

This endpoint retrieves all pins by searching in a given area.

### HTTP Request

`GET http://example.com/api/pins`

### Query Parameters

Parameter | Default | Description
--------- | ------- | -----------
location | [0,0] | [lat, long] coordinates of the center of search area
radius | 25000 | A integer that represents the radius of the search area. Maximum is 25000

**This endpoint does not need to be authenticated**

## Get a Specific Pin

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
