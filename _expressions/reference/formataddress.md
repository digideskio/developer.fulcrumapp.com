---
layout: default
section: expressions
title: "FORMATADDRESS"
description: "Formats an address field object into a string"
category: section
permalink: /expressions/reference/formataddress/
---

### Parameters

`address` Object (__required__) - the address field object

### Returns

String - formatted string

### Examples

```js
FORMATADDRESS({sub_thoroughfare: '360', thoroughfare: 'Central Avenue', suite: '200', locality: 'St. Petersburg', sub_admin_area: 'Pinellas', admin_area: 'FL', postal_code: '33701'})

// returns "360 Central Avenue #200\nSt. Petersburg FL 33701"
```


```js
FORMATADDRESS({sub_thoroughfare: '360', thoroughfare: 'Central Avenue', suite: '200', locality: 'St. Petersburg', sub_admin_area: 'Pinellas', admin_area: 'FL', postal_code: '33701'}, {lineSeparator: ', '})

// returns "360 Central Avenue #200, St. Petersburg FL 33701"
```