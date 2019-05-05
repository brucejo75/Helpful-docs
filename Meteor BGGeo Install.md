## Installing `cordova-background-geolocation` for a Meteor application

There are a few steps in setting this up for the licensed version

### Install cordova plugin directly from the github repository
As part of licensing the released version of `cordova-background-geolocation` you get access to the **premium** version.
This access is provided by installing the plugin directly from the private repository, it is not available in `npm`.

Meteor instructions are [here](https://guide.meteor.com/mobile.html#installing-plugins)
under the subheading: "Installing a plugin from Git".

You need 3 `cordova-background-geolocation` parameters:
- **id**: for the premium version this is `cordova-background-geolocation`
- **git location**: `https://github.com/transistorsoft/cordova-background-geolocation`
- **commit id**:  This can be found under the [releases directory](https://github.com/transistorsoft/cordova-background-geolocation/releases)
click on the commit id and you will be able to capture the full commit id.  For v3.0.1 the id is `f2e4eceeb0b94d741ac56f07eda8e58de96c4a74`

Now you can assemble the full command for v3.0.1:

`meteor add cordova:cordova-background-geolocation@https://github.com/transistorsoft/cordova-background-geolocation#f2e4eceeb0b94d741ac56f07eda8e58de96c4a74`

### Configure the plugin

For Meteor the plugin can be configured in `mobile-config.js` with [`App.configurePlugin`](https://docs.meteor.com/api/mobile-config.html#App-configurePlugin):

```javascript
App.configurePlugin('cordova-background-geolocation', {
  'ACCOUNT_TYPE': `YOURAPP_ID.account`,
  'ACCOUNT_NAME': `YOURAPP_ID`,
  'CONTENT_AUTHORITY': `YOURAPP_ID`,
  'LICENSE': `YOUR_LICENSE`,
  'GOOGLE_API_VERSION': '16.0.0',
  'APPCOMPAT_VERSION': '27.0.1',
  'LOCATION_ALWAYS_AND_WHEN_IN_USE_USAGE_DESCRIPTION': 'Always use is required for background location tracking',
  'LOCATION_ALWAYS_USAGE_DESCRIPTION': 'Background location-tracking is required',
  'LOCATION_WHEN_IN_USE_USAGE_DESCRIPTION': 'Background location-tracking is required',
  'MOTION_USAGE_DESCRIPTION': 'Using the accelerometer increases battery-efficiency'
    + 'by intelligently toggling location-tracking only when the device is detected to be moving'
});
```

**Note**: I added 3 other settings: `ACCOUNT_TYPE`, `ACCOUNT_NAME` & `CONTENT_AUTHORITY`.  These settings essentially scope the installed
`cordova-background-geolocation` to `'YOURAP_ID'`, which means multiple apps with be able to share one installation of the plugin.

For me that means I can install multiple test versions of my app
on an Android device without plugin collisions when I use different application ids.
This has been helpful for development and testing.
