Kona Widget Documentation
=======================

The Kona Widget allows you to embed Kona into your own applications for a more seemless integration. To learn more on how to get your client id and authenticate using OAuth 2.0, please see our [Kona API documentation](https://github.com/KonaTeam/kona-api/blob/production/sections/authentication.md)

The Kona Widget is available to Kona Business accounts. To learn more about Kona Business, please visit [Kona.com](https://kona.com/business) or, if you are interested in other integrations or extensibility, feel free to email us at support@kona.com.

##Adding a Widget Container
```html
<div id="myContainer" class="container-style"></div>
```

##Configuration

```javascript
window._konaWidgetConfig = {
  id: "widgetiFrame", // the id you want to give the widget iframe
  key: 'clientID', // (optional) the application specific key you have been provided or genereated
  domain: 'io.kona.com', // the kona production domain to call
  oauth_token: 'access_token', // (optional) the OAuth 2.0 access token of the user to log as into the widget
  container_id: 'myContainer', // the id of element to embed the widget_in
  default_space: 'spaceId', // (optional) the id of the space you want to show by default
  disable_spaces: 'true', // (optional) set to 'true' if do not want to show the space drawer at the left side of the widget; valid values are 'true' and 'false'
  disable_people: 'true', // (optional) set to 'true' if do not want to show the people drawer at the right side of the widget; valid values are 'true' and 'false'
  theme: 'themeId' // (optional) the id of the theme to use; valid values are 'light', 'dark', and 'kona'
};
```
**Note:** If the `oauth_token` is invalid or already expired, the widget will show the login page instead. The widget does not auto-refresh the token - the application has to validate and/or refresh the token before loading the widget. Refer to our API documentation for more details on how to do this.

##Programatically Change Spaces
```javascript
//get the contentWindow of our iframe
var win = document.getElementById(window._konaWidgetConfig.id).contentWindow;
//send a postMessage with the space ID as the first parameter and url as the second parameter
win.postMessage(
  "1",
  "https://" + window._konaWidgetConfig.domain
);
```

##Loading The Widget
```javascript
var script = document.createElement('script');
script.src = 'https://' + window._konaWidgetConfig.domain + '/kona/widgets/kona_widget_loader.js?conversations';
var insertLocation = document.getElementsByTagName('script')[0];
insertLocation.parentNode.insertBefore(script, insertLocation);
```
##Notes
* When embedding the widget on an installed application using a browser control and a web page, the widget may not load properly at times. This is possibly due to browser compatibility issue with the browser control. Try to include a DOCTYPE directive in your webpage (e.g. `<!DOCTYPE HTML PUBLIC "-//W3C//DTD HTML 4.01//EN" "http://www.w3.org/TR/html4/strict.dtd">`) and check the compatibility mode setting of the browser control. Kona supports Internet Explorer 9 & up and other modern, evergreen browsers.