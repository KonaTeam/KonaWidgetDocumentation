Kona Widget Documentation
=======================

##Adding a Widget Container
```html
<div id="myContainer" class="container-style"></div>
```

##Configuration

```javascript
window._konaWidgetConfig = {
  id: "widgetiFrame", //the id you want to give the widget iframe
  key: 'afakeapplicationkey', //the application specific key you have been provided or genereated
  domain: 'alpha.kona.com', //the kona domain to call - only alpha at the moment
  auth_token: 'afaketoken', //the id of element to embed the widget_in
  container_id: 'myContainer' //the id of element to embed the widget_in
};
```

##Programatically Change Spaces
```javascript
//get the contentWindow of our iframe
var win = document.getElementById(window._konaWidgetConfig.id).contentWindow;
//send a postMessage with the space id as the first parameter and url as the second parameter
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
