# 🏹 Tracking API

Welcome to the Tracking API.&#x20;

This API should be used when there is no module available for your language or they doesn't satisfy your needs.

We strive to keep it as simple as possible.

Is something missing from the API? Let us know [hello@nucleus.sh](mailto:hello@nucleus.sh)

### Event Types <a href="#events-types" id="events-types"></a>

| Type      | Description                                                                                     | Requires extra data |
| --------- | ----------------------------------------------------------------------------------------------- | ------------------- |
| init      | First event to send when starting the app                                                       | **yes**             |
| event     | Default type, for reporting actions or anything else                                            | no                  |
| error     | Submit an error, `name` should contain the error type                                           | **yes**             |
| userid    | Set a new user ID for this user                                                                 | no                  |
| props     | Set properties for this user ID based on the `data` field.                                      | no                  |
| heartbeat | _Websocket only_: send every minute to keep the connection on. **machineId** field is required. | no                  |

The first thing you need to send when the user opens the app is an **init** event upon which most of the analytics relies on.&#x20;

**If you don't send it first, no data will appear in your dashboard.**

Nucleus uses the **init** event to track the number of sessions. Make sure you send a new **init** event whenever a new session is created. We know it might be difficult to track this properly if you're only sending events from your backend. One solution could be to save the last event timestamp of each session in an in-memory storage and create a new session if more than 30 minutes passed since the last request.

### Events Data <a href="#events-data" id="events-data"></a>

Nucleus expects to receive the analytics data as a JSON object, containing a `data` array property.

This array should contain one or multiple events you want to report.

_Basic events data_

| Parameter | Type    | Optional                                         | Description                                                                                                                                                           |
| --------- | ------- | ------------------------------------------------ | --------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| type      | String  | optional                                         | Event type, see below for all the possible values (default: "event")                                                                                                  |
| name      | String  | **required** if `type` is **event** or **error** | Name of the event                                                                                                                                                     |
| sessionId | Integer | **required**                                     | 4-digits number that identifies the current session                                                                                                                   |
| date      | String  | optional                                         | Date/time of the event (ISO 8601 or milliseconds since 1970). If not provided we'll use the time the server receives the event.                                       |
| id        | String  | optional                                         | Short ID that will be returned in confirmation                                                                                                                        |
| userId    | String  | optional                                         | Identify the user with user-facing ID like email, username..                                                                                                          |
| anonId    | String  | optional                                         | Unique user identifier. Ideally should be a [nanoid](https://github.com/ai/nanoid) of 12 chars. More reliable than _userId_ and can be used to track anonymous users. |
| deviceId  | String  | **required**                                     | Hashed identifier of the machine (ie mac address)                                                                                                                     |
| payload   | Object  | optional                                         | Additional data attached to the event                                                                                                                                 |

_Extra events data_

If you are reporting either an **error** or the first **init** event, you can attach the following extra data:

| Parameter | Type    | Optional | Example  | Description                                            |
| --------- | ------- | -------- | -------- | ------------------------------------------------------ |
| platform  | String  | required | _Darwin_ | Usually 'win32', 'windows', 'mac', 'darwin' or 'linux' |
| osVersion | String  | required | _18.2.0_ | Current installed version of the OS                    |
| totalRam  | Integer | required | 8        | Total RAM available on the user device                 |
| version   | String  | required | _0.1.0_  | Version of the app installed                           |
| language  | String  | required | _en-US_  | Locale of the user                                     |

They are not required with regular events to save bandwidth.

Example:

```json
{    
    "data": [{       
     "event": String, // Name of the event        
     "id": String, // OPTIONAL A random id for the event that will be returned when the query succeeds, can be used to make sure no events are reported two times.        
     "userId": String, // A string to identify the user      
     "anonId": String, // 12 chars nanoid identifying the user          
     "deviceId": String, // A hashed identifier of the machine        
     "sessionId": Int, // A random 4-digits number that identifies the current session
     "platform": String, //        
     "osVersion": String, // The version of the OS        
     "totalRam": Int, // The total number of RAM available, in GB        
     "version": String, // Installed version of the app        
     "language": String, // Locale of the user (i.e. 'en-US')        
     "payload": {} // Any additionnal data that you want to report along the event    }]
  }
```

### Track via WebSockets <a href="#track-via-websockets" id="track-via-websockets"></a>

This is the recommended protocol to submit data.&#x20;

It is the most efficient in terms of bandwidth and battery. This is what the modules use behind the scenes.

Latency will be vastly better compared to normal HTTP requests.

**Endpoint:** `wss://app.nucleus.sh/:appId/`

Send your data as a JSON serialized string message.

To prevent data lost due to network errors, when Nucleus receives an event it will send your client a message containing an array `reportedIds` of the previously reported events so you can safely assume they were handled by the server.

### Track via HTTP <a href="#track-via-http" id="track-via-http"></a>



{% swagger method="post" path="" baseUrl="https://app.nucleus.sh/app/:appId/track" summary="" %}
{% swagger-description %}
Use this if you'd like to report data where Websockets aren't available.

Keep in mind that with the HTTP method the "Live view" in the dashboard won't work.

This endpoint doesn't require authentication but is **subject to IP rate limiting**.&#x20;

If you expect lots of events to be reported within a short time interval, you should condense them under one request. For example, save the events in memory (with their correct date), and every 30 seconds report them to the server.

Nucleus will respond with an array `reportedIds` containing the IDs of the events just reported.
{% endswagger-description %}

{% swagger-parameter in="path" name="appId" required="true" %}
Your app Id
{% endswagger-parameter %}

{% swagger-parameter in="body" required="true" name="data" type="Array" %}
Array of 

`events`

 objects
{% endswagger-parameter %}
{% endswagger %}

