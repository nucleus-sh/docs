# Electron.js / Tauri

_If you're looking to integrate Astrolytics into a web application instead, check out the_ [_Browser SDK_](browser.md)_._

To start using this module, sign up and get an app ID on [Astrolytics.io](https://dash.astrolytics.io).

## Installation

Using npm or yarn (recommended):

```bash
$ npm install astrolytics-desktop
$ yarn add astrolytics-desktop
```

## Usage

First sign-up and get a tracking ID for your app [here](https://nucleus.sh).

With ES6 imports:

```javascript
import Astrolytics from "astrolytics-desktop"
```

Or with CommonJS imports:

```javascript
const Astrolytics = require('astrolytics-desktop')
```

Then:

```javascript
Astrolytics.init("<Your App Id>")

// Optional: sets an user ID
Astrolytics.setUserId("richard_hendrix")

// Report things
Astrolytics.track("PLAYED_TRACK", {
  trackName: "My Awesome Song",
  duration: 120,
})
```

You only need to call `init` once.

### Options

You can init Astrolytics with options:

```javascript
import Astrolytics from "astrolytics-desktop"

Astrolytics.init("<Your App Id>", {
  deviceId: null, // device id (optional)
  disableInDev: false, // disable module while in development (default: false)
  disableTracking: false, // completely disable tracking from the start (default: false)
  disableErrorReports: false, // disable errors reporting (default: false)
  sessionTimeout: 60 * 60, // in seconds, after how much inactivity a session expires
  useOldDeviceId: false, // use the legacy device ID (default: false)
  debug: true, // Show logs
})

```

**Each property is optional**. You can start using the module with just the app ID.

The module will try to autodetect a maximum of data as possible but some can fail to detect. It will tell you in the logs which one it failed to detect.

You can manually add data:

```javascript
Astrolytics.setProps({
  version: "0.3.1",
  locale: "fr",
  // ...
})
```

### Electron Version detection

Version detection only works on Electron's main process as newest Electron versions don't supply the remote module version.

To get the version programmatically in Electron's renderer process you need to [setup the remote object](https://github.com/electron/remote), you can then track the version with:

```javascript
const { app } = require('@electron/remote')

Astrolytics.setProps({
  version: app.getVersion(),
})
```

### Identify your users

You can track specific users actions on the 'User Explorer' section of your dashboard.

For that, you need to supply an `userId`, a string that will allow you to track your users.

It can be your own generated ID, an email, username... etc.

```javascript
Astrolytics.identify("someUniqueUserId")
```

You can also pass custom attributes to be reported along with it.

```javascript
Astrolytics.identify("someUniqueUserId", {
  age: 34,
  name: "Richard Hendricks",
  jobType: "CEO",
})
```

If you call `.identify()` multiple times, the last one will be remembered as the current user data (it will overwrite).

Later on, you can update the userId only (and keep the attributes) with this method:

```javascript
Astrolytics.setUserId("someUniqueUserId")
```

### Update user attributes

You can report custom user attributes along with the automatic data.

Those will be visible in your user dashboard if you previously set an user ID.

The module will remember past properties so you can use `Astrolytics.setProps` multiple times without overwriting past props.

Properties can either **numbers**, **strings** or **booleans**. Nested properties or arrays aren't supported at the moment.

```javascript
Astrolytics.setProps({
  age: 34,
  name: "Richard Hendricks",
  jobType: "CEO",
})
```

Overwrite past properties by setting the second parameter as true.

```javascript
Astrolytics.setProps({
  age: 23
}, true)
```

### Events

Send your own events and track user actions:

```javascript
Astrolytics.track("PLAYED_TRACK")
```

They are a couple event names that are reserved by Astrolytics: `init`, `error:` and `astrolytics:`. Don't report events containing these strings.

#### Attach custom data

You can also add extra information to tracked events, as a JSON object.

Properties can either **numbers**, **strings** or **booleans**. Nested properties or arrays aren't supported at the moment (they won't show in the dashboard).

Example

```javascript
Astrolytics.track("PLAYED_TRACK", {
  trackName: "My Awesome Song",
  duration: 120,
})
```

#### Pages and Screen Views

You can set up Astrolytics to track page visits and screen views in your app.

For that, whenever the user navigates to a different page, call the `.page()` method with the new view name.

```javascript
Astrolytics.page("View Name")
```

You can attach extra info about the view. Example:

```javascript
Astrolytics.page("Cart", {
  action: "addItem",
  count: 5
})
```

Params can either be **numbers**, **strings** or **booleans**. Nested params or arrays aren't supported at the moment (they won't show in the dashboard).

### Toggle tracking

This will completely disable any communication with Astrolytics' servers.

To opt-out your users from tracking, use the following methods:

```javascript
Astrolytics.disableTracking()
```

and to opt back in:

```javascript
Astrolytics.enableTracking()
```

This change won't persist after restarts so you have to handle the saving of the settings.

You can also supply a `disableTracking: true` option to the module on start if you want to directly prevent tracking.

### Error tracking

Astrolytics will by default report all `uncaughtException`, `unhandledRejection` and `windowError` events.

If you'd like to report another type of error, you can do so with:

```javascript
Astrolytics.trackError("myCustomError", err)
```

Contact **hello@astrolytics.io** for any inquiry
