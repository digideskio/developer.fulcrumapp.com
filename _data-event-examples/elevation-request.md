---
layout: default
section: data_events
title: "Get elevation information"
description: "Determine the elevation of the location you are collecting data."
category: section
tags:
  - request
  - set value
---

Similar to automating weather collection, data events allow you to tap into any API that supports point coordinates. This example uses the [MapQuest Open Elevation API](https://developer.mapquest.com/documentation/open/elevation-api/elevation-profile/get/) to determine the elevation at the point you are collecting data. It assumes you have a numeric (integer) field called `mq_elevation`.

<img src="../media/fulcrum-elev-data-event.gif" alt="Elevation Query" style="float: right; margin-left: 40px;" />

``` js
function getElevation() {
  var key = 'your_api_key';
  var mqURL = 'https://open.mapquestapi.com/elevation/v1/profile?key=' + key + '&shapeFormat=raw&latLngCollection=' + LATITUDE() + ',' + LONGITUDE();

  var options = {
    url: mqURL
  };

  REQUEST(options, function(error, response, body) {
    if (error) {
      ALERT('Error with request: ' + INSPECT(error));
    } else {
      elevation = JSON.parse(body);
      SETVALUE('mq_elevation', elevation.elevationProfile[0].height);
    }
  });
}

ON('change-geometry', getElevation);
```

Alternatively, altitude can be determined (as shown in the .gif animation as 'Device Elevation') with a Calculation Expression as shown in [this example](http://developer.fulcrumapp.com/expressions/examples/altitude/).
