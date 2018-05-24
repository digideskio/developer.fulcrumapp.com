---
layout: default
section: data_events
title: "SETDESCRIPTION"
description: "Set the description of a field."
category: section
permalink: /data-events/reference/setdescription/
---

### Parameters

`field` String (__required__) - The data name for the field

`value` String (__required__) - The value to set for the field's description, or `null` to restore the original description

### Examples

```js
SETDESCRIPTION('weather_summary', 'Could not automatically fetch weather data. Briefly describe the current weather.');

// Sets the description of a weather summary field
```