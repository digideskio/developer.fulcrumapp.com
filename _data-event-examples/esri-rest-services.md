---
layout: default
section: data_events
title: "Query Esri REST services"
description: "How to query an Esri ArcGIS Server or ArcGIS Online REST Service using Data Events in Fulcrum."
category: section
tags:
  - request
  - esri
---

This example passes your Fulcrum latitude and longitude as a point parameter in an Intersect query against data hosted on an Esri REST Service. Three text fields are required in the app, for the particular properties we're interested in here.

<img src="../media/flood-info.gif" alt="USGS Elevation Query" style="float: right; margin-left: 40px;" />

``` js
function getFloodInfo() {

  var options = {
    url: 'https://hazards.fema.gov/gis/nfhl/rest/services/public/NFHLWMS/MapServer/28/query',
    qs: {
      geometry: LONGITUDE() + ',' + LATITUDE(),
      geometryType: 'esriGeometryPoint',
      inSR: '4326',
      spatialRel: 'esriSpatialRelIntersects',
      outFields: '*',
      returnGeometry: false,
      f: 'pjson'
    }
  };

  REQUEST(options, function (err, res, body) {
    if (err) {
      ALERT('Error: ' + err.message);
    } else {
      var result = JSON.parse(body);
      if (result && result.features[0]) {
        SETVALUE('flood_zone', result.features[0].attributes['FLD_ZONE']);
        SETVALUE('flood_zone_subtype', result.features[0].attributes['ZONE_SUBTY']);
        SETVALUE('dfirm_id', result.features[0].attributes['DFIRM_ID']);
      } else {
        SETVALUE('flood_zone', 'NA');
        SETVALUE('flood_zone_subtype', 'NA');
        SETVALUE('dfirm_id', 'NA');
      }
    }
  });
}

ON('change-geometry', getFloodInfo);
```
