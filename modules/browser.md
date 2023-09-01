# Browser

![Astrolytics](https://intriguing-lemonade-efa.notion.site/image/https%3A%2F%2Fs3-us-west-2.amazonaws.com%2Fsecure.notion-static.com%2Fb00319ab-5801-40dc-b0f4-5de683b11d61%2Fgithub\_browser\_sdk\_banner.jpg?id=9fabfb89-61d3-4d87-97ff-ef88c9bd8947\&table=block)

## Table of Contents

1. [Getting Started](broken-reference)
2. [Usage](broken-reference)
3. [How to Contribute](broken-reference)

## Getting Started

To get started with Astrolytics, create an account at [Astrolytics](https://dash.astrolytics.io/signup) and grab the App ID, then use the SDK to start tracking events.

### Installation

As NPM package (recommended)

```bash
# with npm
npm install astrolytics-browser

# or with yarn
yarn add astrolytics-browser
```

or as browser script

```
<script src="https://cdn.jsdelivr.net/npm/astrolytics-browser/dist/bundle.iife.js"></script>
```

we recommend using a specific version in case a non-backwards compatible change is introduced

```
<script src="https://cdn.jsdelivr.net/npm/astrolytics-browser@<version>/dist/bundle.iife.js"></script>
```

### Usage

```javascript
import Astrolytics from 'astrolytics-browser';

Astrolytics.init('YOUR_APP_ID');
```

Replace `'YOUR_APP_ID'` with the unique ID of your app. You can get it [here](https://dash.nucleus.sh/account).

You can check examples with different frameworks here.

## API

Astrolytics supports passing the following options as second argument to the `Astrolytics.init()` method:

```js
Astrolytics.init('APP_ID', {
  endpoint: 'wss://app.astrolytics.io', // only option, we don't allow self hosting yet :(
  disableInDev: true, // disable in development mode. We recommend not to call
                      // `init` method, as that will be more reliable.
  debug: false, // if set to `true`, will log a bunch of things.
  disableTracking: false, // will not track anything. You can also use `Astrolytics.disableTracking()`.
                          // note that some events will still be added to the queue, so if you call
                          // Astrolytics.enableTracking() again, they will be sent to the server.
  automaticPageTracking: true, // will track all page changes.
  reportInterval: 2 * 1000, // at which interval the events are sent to the server.
  sessionTimeout: 60 * 30 * 1000, // time after which the session is ended
  cutoff: 60 * 60 * 48 * 1000, // time after which event that were not sent yet are deleted
  disableErrorReports: false, // wether to disable error tracking
})
```

### Tracking

Track events with optional custom data

```javascript
Astrolytics.track("click", { foo: 'bar' });
```

### Error Tracking

Track errors with a name and the Error object.

```javascript
Astrolytics.trackError(name, error);
```

By default Astrolytics will listen for `window.onerror` and `window.onunhandledrejection` events and send them to the API. If you want to disable this behaviour, you can set `disableErrorReports` to `true`:

```js
Astrolytics.init('APP_ID', { disableErrorReports: true })
```

### User Identification

Identify a user by a unique ID and optionally set custom properties.

```javascript
Astrolytics.identify('04f8846d-ecca-4a81-8740-f6428ceb7f7b', { firstName: 'Brendan', lastName: 'Eich' });
```

### Page Tracking

Track page views with the page name and optional parameters. If the page name is not provided, the current window's pathname is used.

```javascript
Astrolytics.page('/about', { foo: 'baz' });
```

By default, Astrolytics will track any page change by polling the url every 50 ms. If you prefer to manually track page changes, set `automaticPageTracking` to false and call `Astrolytics.page()` on every page change.

### Disabling Tracking

To disable tracking

```javascript
Astrolytics.disableTracking();
```

### Enabling Tracking

To enable tracking

```javascript
Astrolytics.enableTracking();
```
