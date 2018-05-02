---
layout: default
section: data_events
title: "Add current weather to a record"
description: "Use this to fetch weather data from the wunderground.com API and add it to the record."
category: section
tags:
  - request
  - set value
---

This example assumes you've signed up for an API key from [wunderground.com](https://www.wunderground.com/weather/api) and have a text field in your app (`weather_summary` below) to store the current weather summary.

Here we're listening for the `'change-geometry'` event for a record, and then using the [REQUEST]({{ site.url }}/data-events/reference/request) function to make an API call to wunderground.com. Once we get the response we parse it as JSON and use [SETVALUE]({{ site.url }}/data-events/reference/setvalue) to update the form value.


```js
function getWeather() {
  var apiKey = 'your_api_key';

  var options = {
    url: 'https://api.wunderground.com/api/' + apiKey + '/conditions/q/' + LATITUDE() + ',' + LONGITUDE() + '.json'
  };

  REQUEST(options, function(error, response, body) {
    if (error) {
      ALERT('Error with request: ' + error);
    } else {
      var data = JSON.parse(body).current_observation;
      SETVALUE('weather_summary', data.weather + ', ' + data.wind_string);
    }
  });
}

ON('change-geometry', getWeather);
```

If you only want to fetch weather when explicitly requested by the end user, you can listen for the `'click'` event for a hyperlink field. Just add a new [hyperlink field](http://help.fulcrumapp.com/field-types/how-do-hyperlink-fields-work) to your app and give it a descriptive label, "Tap to Add Weather Data" for example. Leave the default url blank and set up your data event like so.

```js
// This assumes you've still got the getWeather function defined from the example above

ON('click', 'your_hyperlink_field', getWeather)
```

These examples add basic weather metrics, but many others [are available](https://www.wunderground.com/weather/api/d/docs?d=data/conditions) and could be added to multiple fields in your app.
