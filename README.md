Kona Widget Documentation
=======================

The Kona Widget allows you to embed Kona into your own applications for a more seemless integration. It is available to Kona business accounts users - to learn more about Kona business, please visit our [product introduction page](https://www.deltek.com/en/products/project-and-portfolio-management/kona) and our [pricing page](https://www.kona.com/business/). Or, if you are interested in other integrations or extensibility, feel free to email us at support@kona.com.

To run the widget, it would need a Client ID from your account that you can generate from Kona's web application. The widget also needs to be authenticated using the OAuth 2.0 protocol. To learn more on how to get your client id and authenticate using OAuth 2.0, please see our [Kona API documentation](https://github.com/KonaTeam/kona-api/blob/production/sections/authentication.md)

##Adding a widget container
```html
<div id="myContainer" class="container-style"></div>
```

##Configuration

```javascript
window._konaWidgetConfig = {
  id: "widgetiFrame", // the id attribute you want to give the widget iframe
  key: 'clientID', // (optional) the application specific key you have been provided or genereated
  domain: 'io.kona.com', // the Kona production domain to call
  oauth_token: 'access_token', // the OAuth 2.0 access token of the user to log as into the widget
  container_id: 'myContainer', // the id of element to embed the widget_in
  default_space: 'spaceId', // (optional) the space ID you want to show by default
  disable_spaces: 'true', // (optional) set to 'true' to remove the space drawer at the left side of the widget; valid values are 'true' and 'false'
  disable_people: 'true', // (optional) set to 'true' to remove the people drawer at the right side of the widget; valid values are 'true' and 'false'
  disable_header: 'true', // (optional) pass 'true' to this parameter to remove the blue header of the widget and the navigation tabs; valid values are 'true' and 'false'
  disable_folders: 'true', // (optional) set to 'true' if you do not want to show folders in the space; valid values are 'true' and 'false'
  disable_paperclip: 'true', // (optional) set this to 'true' to hide the folders of a space; valid values are 'true' and 'false'
  show_conversations: 'true', // (optional) if this is 'true' it shows the conversation list only and adjusts other UI elements like search accordingly; valid values are 'true' and 'false'
};
```
**Note:** If the `oauth_token` is invalid or already expired, the widget will show the login page instead. The widget does not auto-refresh the token - the application has to validate and/or refresh the token before loading the widget. Refer to our API documentation for more details on how to do this.

##Programatically change spaces
```javascript
//get the contentWindow of our iframe
var win = document.getElementById(window._konaWidgetConfig.id).contentWindow;
//send a postMessage with the space ID as the first parameter and url as the second parameter
win.postMessage(
  "1",
  "https://" + window._konaWidgetConfig.domain
);
```

##Loading the widget
```javascript
var script = document.createElement('script');
script.src = 'https://' + window._konaWidgetConfig.domain + '/kona/widgets/kona_widget_loader.js?conversations';
var insertLocation = document.getElementsByTagName('script')[0];
insertLocation.parentNode.insertBefore(script, insertLocation);
```
###Notes
When embedding the widget on an installed application using a browser control and a web page, the widget may not load properly at times. This is possibly due to browser compatibility issue with the browser control. Try to include a DOCTYPE directive in your webpage (e.g. `<!DOCTYPE HTML PUBLIC "-//W3C//DTD HTML 4.01//EN" "http://www.w3.org/TR/html4/strict.dtd">`) and check the compatibility mode setting of the browser control. Kona supports Internet Explorer 9 & up and other modern, evergreen browsers.

## Getting the unread count
The widget can also notify the parent iframe of the current count of unread conversations via a postMessage. The integrating web application needs to add an event listener to capture the data and parse the JSON payload. The `data` attribute of the message contains the unread conversations count of all active spaces of the user (`count`), and optionally, if they are currently in a specific space, the unread count of the space (`projectCount`). 
```javascript
    window.addEventListener("message", receiveMessage, false);

    function receiveMessage(event)
    {
        var countData = jQuery.parseJSON(event.data);
        if (countData.projectCount !== null) {
            var unreadCount = "Overall Unread: " + countData.count + "; Current Unread: " + countData.projectCount;
        } else {
            var unreadCount = "Overall Unread: " + countData.count;
        }
        $("#widgetTitle").html("Widget (" + unreadCount + ")");
    }

```
As an example, `event.data` can have the following string value `"{"type":"conversationCountUpdate","count":5,"projectCount":null}"` if the widget is in MyKona; or `"{"type":"conversationCountUpdate","count":5,"projectCount":1}"` if the widget is in a specific space.