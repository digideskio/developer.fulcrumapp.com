---
layout: default
section: data_events
title: "Integrating Fulcrum with what3words"
description: "Use the what3words API to populate a record with the 3 word representation of its location or update its location from a known 3 word address."
category: section
tags:
  - request
  - what3words
---

[what3words](http://what3words.com/) is a unique combination of just 3 words that identifies a 3mx3m square, anywhere on the planet. The [what3words API](https://docs.what3words.com/api/v2/) provides programmatic access to convert a 3 word address to coordinates (forward geocoding) and to convert coordinates to a 3 word address (reverse geocoding). You can sign up for a free what3words API key at [https://map.what3words.com/register?dev=true](https://map.what3words.com/register).

The example below demonstrates how to listen for a `'change-geometry'` event to automatically update a text field (`w3w_address`) with the what3words address for the record's location. It also demonstrates using the [`SETLOCATION`](/data-events/reference/setlocation/) function to manually update the record's location from a known what3words address.

Both the `getw3w` and `setw3w` functions use the [REQUEST](/data-events/reference/request) function to make an API call to what3words to fetch the info we need. The JSON response from the API is parsed and used to update the Fulcrum record accordingly.

<img src="../media/fulcrum-w3w-integration.gif" alt="what3words Example" style="float: right; margin-left: 40px;" />

```js
var w3wApiKey = 'my_api_key';

function getw3w() {
  var options = {
    url: 'https://api.what3words.com/v2/reverse',
    qs: {
      key: w3wApiKey,
      coords: LATITUDE() + ',' + LONGITUDE()
    }
  };

  PROGRESS('Loading', 'Finding the right words...');

  REQUEST(options, function(error, response, body) {
    PROGRESS();
    if (error) {
      ALERT('Error with request: ' + INSPECT(error));
    } else {
      var result = JSON.parse(body);
      SETVALUE('w3w_address', result.words);
    }
  });
}

function setw3w() {
  if ($w3w_address && $w3w_address.split('.') && $w3w_address.split('.').length == 3) {
    options = {
      url: 'https://api.what3words.com/v2/forward',
      qs: {
        key: w3wApiKey,
        addr: $w3w_address
      }
    };

    PROGRESS('Loading', 'Finding the location...');

    REQUEST(options, function(error, response, body) {
      PROGRESS();
      if (error) {
        ALERT('Error with request: ' + INSPECT(error));
      } else {
        var result = JSON.parse(body);
        if (result.geometry) {
          SETLOCATION(result.geometry.lat, result.geometry.lng);
          ALERT('Succes!', 'Your position has been updated to: ' + result.geometry.lat + ', ' + result.geometry.lng);
        } else if (result.message) {
          ALERT('w3w message', result.message);
        }
      }
    });
  } else {
    ALERT('Error', 'A 3 word address must be provided in the following format: index.home.raft');
  }
}

ON('change-geometry', getw3w);
ON('click', 'update_location', setw3w);
```
