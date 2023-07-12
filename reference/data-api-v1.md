---
description: >-
  The Data API v1 is now available. Almost all the data obtained by Nucleus can
  be retrieved using this API.
---

# 🔗 Data API V1

{% hint style="info" %}
This API is in beta for now, please let us know if you encounter any issue or if you need to fetch a specific data that isn't documented here, we strive to be fast to respond. We will add more endpoints over time.
{% endhint %}

### Authentication <a href="#authentication" id="authentication"></a>

Nucleus expects the API key to be included in all requests to the server in a auth header:

`Authorization: your_access_token`

Example:

```bash
curl "api_endpoint_here" h -H "Authorization: your_access_token"
```

You may also send it in the body of a POST request as the parameter `token`.

### Quick Glance <a href="#quick-glance" id="quick-glance"></a>

{% swagger method="get" path="" baseUrl="https://app.nucleus.sh/api/v1/apps/:appId/analytics/quickglance" summary="" %}
{% swagger-description %}
Fetch the number of users, installs, sessions and errors during the last 24 hours (and during the previous 24h period for comparison).
{% endswagger-description %}

{% swagger-parameter in="path" name="appId" required="true" %}
Your app ID
{% endswagger-parameter %}

{% swagger-response status="200: OK" description="" %}
```javascript
{ 
    "data": {    
        "installs": 35,    
        "users": 15,    
        "appStarts": 205,    
        "errors": 526,    
        "previousInstalls": 26,    
        "previousUsers": 21,    
        "previousAppStarts": 46,    
        "previousErrors": 256  
    }
}
```
{% endswagger-response %}
{% endswagger %}

Example:

```bash
curl "https://app.nucleus.sh/api/v1/apps/:appId/analytics/quickglance" -H "Authorization: your_access_token"
```

### Daily Analytics <a href="#daily-analytics" id="daily-analytics"></a>

{% swagger method="get" path="" baseUrl="https://app.nucleus.sh/api/v1/apps/:appId/analytics" summary="" %}
{% swagger-description %}
The analytics data of your application grouped by day.

You need to supply a date interval as timestamps.
{% endswagger-description %}

{% swagger-parameter in="path" name="appId" required="true" %}
Your app ID
{% endswagger-parameter %}

{% swagger-parameter in="query" name="start" required="true" %}
Timestamp for beginning of interval
{% endswagger-parameter %}

{% swagger-parameter in="query" name="end" required="true" %}
Timestamp for end of interval
{% endswagger-parameter %}

{% swagger-parameter in="query" name="utcOffset" %}
Your timezone UTC offset (

**in minutes**

) so we can return the appropriate dates
{% endswagger-parameter %}

{% swagger-response status="200: OK" description="" %}
```javascript
// It will return various data on your app like the following. Where each object contains a key/value pair for every day in the period you supplied.
{
  "data": {
    "totalNumbers": {
      "users": 89,
      "installs": 48
    },
    "usage": {
      "2021-04-09": 8,
      "2021-04-10": 25,
      "2021-04-11": 65
    },
    "newUsers": {
      "2021-04-09": 6,
      "2021-04-10": 15,
      "2021-04-11": 25
    },
    "activeUsers": {
      "2021-04-09": 5,
      "2021-04-10": 12,
      "2021-04-11": 21
    },
    "nonNewUsers": {
      "2021-04-09": -1,
      "2021-04-10": 3,
      "2021-04-11": -3
    },
    "hours": {
      "2021-04-09": [
        1,
        1,
        2
      ],
      "2021-04-10": [
        2,
        1,
        1,
        1
      ],
      "2021-04-11": [
        1,
        1,
        1,
        1,
        2,
        2,
        3,
        3
      ]
    },
    "platforms": [
      {
        "value": "linux",
        "count": 12
      },
      {
        "value": "mac",
        "count": 8
      },
      {
        "value": "win",
        "count": 17
      }
    ],
    "ram": [],
    "languages": [
      {
        "value": "en",
        "count": 15
      },
      {
        "value": "fr",
        "count": 8
      }
    ],
    "versions": [
      {
        "value": "0.2.1",
        "count": 55
      }
    ],
    "countries": [
      {
        "value": "BG",
        "count": 6
      },
      {
        "value": "US",
        "count": 20
      }
    ],
    "avgSessionDuration": {
      "2021-04-10": 1789,
      "2021-04-11": 2589.5
    }
  }
}
```
{% endswagger-response %}
{% endswagger %}

Example:

```bash
curl "https://app.nucleus.sh/api/v1/apps/:appId/analytics?start=1618010248&end=1618211950&utcOffset=480"    -H "Authorization: your_access_token"
```

### Events <a href="#events" id="events"></a>

{% swagger method="get" path="" baseUrl="https://app.nucleus.sh/api/v1/apps/:appId/analytics/events" summary="" %}
{% swagger-description %}
The daily stats of your application's events.

You need to supply a date interval as timestamps.
{% endswagger-description %}

{% swagger-parameter in="query" name="start" required="true" %}
Timestamp for beginning of interval
{% endswagger-parameter %}

{% swagger-parameter in="query" name="end" required="true" %}
Timestamp for end of interval
{% endswagger-parameter %}

{% swagger-parameter in="query" name="utcOffset" %}
Your timezone UTC offset (

**in minutes**

) so we can return the appropriate dates
{% endswagger-parameter %}

{% swagger-parameter in="path" name="appId" required="true" %}
Your app ID
{% endswagger-parameter %}

{% swagger-response status="200: OK" description="It will return details about custom events that you have set in your app tracking." %}
```javascript
{
  "data": {
    "ITEM_PLAYED": {
      "2021-04-11": 256,
      "2021-04-10": 152
    },
    "ITEM_SAVED": {
      "2021-04-11": 314,
      "2021-04-10": 124
    },
    "ITEM_CHECKOUT": {
      "2021-04-11": 289,
      "2021-04-10": 325
    }
  }
}
```
{% endswagger-response %}
{% endswagger %}

Example:

```bash
curl "https://app.nucleus.sh/api/v1/apps/:appId/analytics/events?start=1618010248&end=1618211950&utcOffset=480" -H "Authorization: your_access_token"
```

### Event Properties <a href="#events-properties" id="events-properties"></a>

{% swagger method="get" path="" baseUrl="https://app.nucleus.sh/api/v1/apps/:appId/analytics/eventsprops" summary="" %}
{% swagger-description %}
Get details on the custom data reported alongside your events.

You need to supply a date interval as timestamps.
{% endswagger-description %}

{% swagger-parameter in="query" name="start" required="true" %}
Timestamp for beginning of interval
{% endswagger-parameter %}

{% swagger-parameter in="query" name="end" required="true" %}
Timestamp for end of interval
{% endswagger-parameter %}

{% swagger-parameter in="query" name="utcOffset" %}
Your timezone UTC offset (

**in minutes**

) so we can return the appropriate dates
{% endswagger-parameter %}

{% swagger-parameter in="path" name="appId" required="true" %}
Your app ID
{% endswagger-parameter %}

{% swagger-response status="200: OK" description="It will return a list of events and attributes (custom data you reported) like the following:" %}
```javascript
{
  "data": [
    {
      "event": "init",
      "keys": [
        "plan",
        "displayName"
      ],
      "type": "string"
    }
  ]
}
```
{% endswagger-response %}
{% endswagger %}

Example:

```bash
curl "https://app.nucleus.sh/api/v1/apps/:appId/analytics/eventsprops?start=1618010248&end=1618211950&utcOffset=480" -H "Authorization: your_access_token"
```

### Event Attributes <a href="#event-attributes" id="event-attributes"></a>

{% swagger method="get" path="" baseUrl="https://app.nucleus.sh/api/v1/apps/:appId/analytics/events/:event/:attr" summary="" %}
{% swagger-description %}
This will fetch data about a specific event and its attribute.

You need to supply a date interval as timestamps.
{% endswagger-description %}

{% swagger-parameter in="query" name="start" required="true" %}
Timestamp for beginning of interval
{% endswagger-parameter %}

{% swagger-parameter in="query" name="end" required="true" %}
Timestamp for end of interval
{% endswagger-parameter %}

{% swagger-parameter in="query" name="utcOffset" %}
Your timezone UTC offset (

**in minutes**

) so we can return the appropriate dates
{% endswagger-parameter %}

{% swagger-parameter in="path" name="appId" required="true" %}
Your app ID
{% endswagger-parameter %}

{% swagger-parameter in="path" name="event" required="true" %}
The name of the event to retrieve data
{% endswagger-parameter %}

{% swagger-parameter in="path" name="attr" %}
The name of the attribute of the event to retrieve data for
{% endswagger-parameter %}

{% swagger-response status="200: OK" description="It will return data for a specific event and event attribute that is requested" %}
```javascript
{
  "data": [
    {
      "value": "pro",
      "count": 35
    },
    {
      "value": "free",
      "count": 24
    }
  ]
}
```
{% endswagger-response %}
{% endswagger %}

Example:

```bash
curl "https://app.nucleus.sh/api/v1/apps/appId/analytics/events/init/plan?start=1617358132&end=1617962932&utcOffset=480" -H "Authorization: your_access_token"
```

### Errors <a href="#errors" id="errors"></a>

{% swagger method="get" path="" baseUrl="https://app.nucleus.sh/api/v1/apps/:appId/analytics/errors" summary="" %}
{% swagger-description %}
This is your application's error records.

You need to supply a date interval as timestamps.
{% endswagger-description %}

{% swagger-parameter in="query" name="start" required="true" %}
Timestamp for beginning of interval
{% endswagger-parameter %}

{% swagger-parameter in="query" name="end" required="true" %}
Timestamp for end of interval
{% endswagger-parameter %}

{% swagger-parameter in="query" name="utcOffset" %}
Your timezone UTC offset (

**in minutes**

) so we can return the appropriate dates
{% endswagger-parameter %}

{% swagger-parameter in="path" name="appId" required="true" %}
Your app ID
{% endswagger-parameter %}

{% swagger-response status="200: OK" description="It will return the data for all uncaughtException and unhandledRejection reports as well as any custom error reports that you have set." %}
```javascript
{
  "data": {
    "unhandledRejection": {
      "2021-04-09": 52,
      "2021-04-10": 28,
      "2021-04-11": 75
    },
    "uncaughtException": {
      "2021-04-09": 26,
      "2021-04-10": 37,
      "2021-04-11": 69
    }
  }
}
```
{% endswagger-response %}
{% endswagger %}

Example:

```bash
curl "https://app.nucleus.sh/api/v1/apps/:appId/analytics/errors?start=1618010248&end=1618211950&utcOffset=480"    -H "Authorization: your_access_token"
```

### Users <a href="#users" id="users"></a>

{% swagger method="get" path="" baseUrl="https://app.nucleus.sh/api/v1/apps/:appId/users" summary="The Users list for your application." %}
{% swagger-description %}

{% endswagger-description %}

{% swagger-parameter in="path" name="appId" required="true" %}
Your app ID
{% endswagger-parameter %}

{% swagger-response status="200: OK" description="It will return all users data" %}
```javascript
{
  "total": 1,
  "data": [
    {
      "_id": "xxxxxxxxxx",
      "userId": "test_user",
      "createdAt": "2021-03-23T06:51:38.633Z",
      "devices": [
        {
          "lastSeen": "2021-03-23T07:42:47.379Z",
          "_id": "xxxxxxxxxx",
          "platform": "win",
          "osVersion": "6.3.9600",
          "version": "0.0.0"
        },
        {
          "lastSeen": "2021-03-23T08:23:44.497Z",
          "_id": "xxxxxxxxxx",
          "platform": "win",
          "osVersion": "8.1",
          "version": "0.0.0"
        }
      ],
      "lastSeen": "2021-03-23T08:23:44.497Z",
      "props": {
        "country": "US",
        "locale": "en"
      }
    }
  ]
}
```
{% endswagger-response %}
{% endswagger %}

Example:

```bash
curl "https://app.nucleus.sh/api/v1/apps/:appId/users"    -H "Authorization: your_access_token"
```

### User Details <a href="#user-details" id="user-details"></a>



{% swagger method="get" path="" baseUrl="https://app.nucleus.sh/api/v1/apps/:appId/users/:userId" summary="User data" %}
{% swagger-description %}

{% endswagger-description %}

{% swagger-parameter in="path" name="appId" required="true" %}
Your app ID
{% endswagger-parameter %}

{% swagger-parameter in="path" name="userId" required="true" %}
ID of the user to fetch details for
{% endswagger-parameter %}

{% swagger-response status="200: OK" description="It will return the user details and all its sessions" %}
```javascript
{
  "user": {
    "lastSeen": "2021-03-23T08:23:44.497Z",
    "_id": "xxxxxxxxx",
    "appId": "xxxxxxxxxxxxxxxxxxxxx",
    "userId": "test_user",
    "__v": 0,
    "createdAt": "2021-03-23T06:51:38.633Z",
    "devices": [
      {
        "lastSeen": "2021-03-23T07:42:47.379Z",
        "_id": "xxxxxxxxx",
        "platform": "win",
        "osVersion": "6.3.9600",
        "version": "0.0.0"
      },
      {
        "lastSeen": "2021-03-23T08:23:44.497Z",
        "_id": "xxxxxxxxx",
        "platform": "win",
        "osVersion": "8.1",
        "version": "0.0.0"
      }
    ],
    "props": {
      "country": "US",
      "locale": "en"
    },
    "updatedAt": "2021-03-23T08:23:44.497Z"
  },
  "sessions": [
    {
      "device": "xxxxxxxxx",
      "actions": 1,
      "errors": 0,
      "end": "2021-03-23T07:41:38.479Z",
      "start": "2021-03-23T07:41:38.479Z",
      "session": "xxxxxx"
    },
    {
      "device": "xxxxxxxxx",
      "actions": 1,
      "errors": 0,
      "end": "2021-03-23T06:51:39.980Z",
      "start": "2021-03-23T06:51:39.980Z",
      "session": "xxxxxx"
    }
  ]
}
```
{% endswagger-response %}
{% endswagger %}

Example:

```bash
curl "https://app.nucleus.sh/api/v1/apps/:appId/users/:userId"    -H "Authorization: your_access_token"
```

### User Session Events <a href="#user-session-events" id="user-session-events"></a>



{% swagger method="get" path="" baseUrl="https://app.nucleus.sh/api/v1/apps/:appId/users/:userId/session/:sessionId" summary="" %}
{% swagger-description %}
This will fetch events data for a specific user session.

You need to supply a device Id in the request parameters, that you can obtain in the previous User Details call.
{% endswagger-description %}

{% swagger-parameter in="path" name="appId" required="true" %}
Your app ID
{% endswagger-parameter %}

{% swagger-parameter in="path" name="userId" required="true" %}
ID of the user to fetch details for
{% endswagger-parameter %}

{% swagger-parameter in="path" name="sessionId" type="Number" required="true" %}
ID of the session to retrieve data for
{% endswagger-parameter %}

{% swagger-response status="200: OK" description="It will return the list of events of a session made by a specific user" %}
```javascript
{
  "data": [
    {
      "app_id": "xxxxxxxxxxxxxxxxxxxxxxxx",
      "platform": "win",
      "user_id": "test_user",
      "machine_id": "xxxxxxxxxx",
      "version": "0.0.0",
      "locale": "en",
      "renderer": null,
      "module_version": "2.6.0",
      "avail_ram": 4,
      "type": "init",
      "time": "2021-03-23T09:31:49.647Z",
      "country": "US",
      "arch": "x64",
      "first_time": false,
      "session_id": "xxxx",
      "error_hash": null,
      "os_version": "8.1",
      "data": {
        "plan": "pro"
      },
      "duration": null
    }
  ]
}
```
{% endswagger-response %}
{% endswagger %}

```bash
curl "https://app.nucleus.sh/api/v1/apps/:appId/users/:userId/session/:sessionId?device=:deviceId"    -H "Authorization: your_access_token"
```

### Delete User <a href="#delete-user" id="delete-user"></a>



{% swagger method="delete" path="" baseUrl="https://app.nucleus.sh/api/v1/apps/:appId/users/:userId" summary="" %}
{% swagger-description %}
This will delete a specific user (by id) and it's associated data.
{% endswagger-description %}

{% swagger-parameter in="path" name="appId" required="true" %}
Your app ID
{% endswagger-parameter %}

{% swagger-parameter in="path" name="userID" required="true" %}
The ID of the user to delete
{% endswagger-parameter %}

{% swagger-response status="204: No Content" description="It will return a 204 code if the user deletion is successful." %}
```javascript
{
    // Response
}
```
{% endswagger-response %}
{% endswagger %}

Example:

```bash
curl -X DELETE "https://app.nucleus.sh/api/v1/apps/:appId/users/:userId"    -H "Authorization: your_access_token"
```

### Live Events <a href="#live-events" id="live-events"></a>



{% swagger method="get" path="" baseUrl="https://app.nucleus.sh/api/v1/apps/:appId/live/events" summary="" %}
{% swagger-description %}
This shows real time events data.
{% endswagger-description %}

{% swagger-parameter in="path" name="appId" required="true" %}
Your app ID
{% endswagger-parameter %}
{% endswagger %}

Example:

```bash
curl "https://app.nucleus.sh/api/v1/apps/:appId/live/events"    -H "Authorization: your_access_token"
```

### Live Users Count <a href="#live-users-count" id="live-users-count"></a>

{% swagger method="get" path="" baseUrl="https://app.nucleus.sh/api/v1/apps/:appId/live/users/count" summary="" %}
{% swagger-description %}
Get the real-time count of how many users are using your app
{% endswagger-description %}

{% swagger-parameter in="path" name="appId" required="true" %}
Your app ID
{% endswagger-parameter %}

{% swagger-response status="200: OK" description="It will return the real-time amount of users" %}
```javascript
{ "data": 234 }
```
{% endswagger-response %}
{% endswagger %}

Example:

```bash
curl "https://app.nucleus.sh/api/v1/apps/:appId/live/users/count"    -H "Authorization: your_access_tokenbh
```

### Live Users List <a href="#live-users-list" id="live-users-list"></a>



{% swagger method="get" path="" baseUrl="https://app.nucleus.sh/api/v1/apps/:appId/live/users" summary="" %}
{% swagger-description %}
Get a list of all the users on your app, right now.
{% endswagger-description %}

{% swagger-parameter in="path" name="appId" required="true" %}
Your app ID
{% endswagger-parameter %}

{% swagger-response status="200: OK" description="It will return all users data" %}
```javascript
{
  "data": [
    {
      "userDbId": "xxxxxxxxx",
      "userId": "xxxxxxxxx",
      "deviceId": "xxxxxxxxx",
      "platform": "win",
      "osVersion": "8.1",
      "country": "US",
      "lastSeen": "2021-04-12T10:26:08.867Z"
    }
  ]
}
```
{% endswagger-response %}
{% endswagger %}

Example:

```bash
curl "https://app.nucleus.sh/api/v1/apps/:appId/live/users"    -H "Authorization: your_access_token"
```
