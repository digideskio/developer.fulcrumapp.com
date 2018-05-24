---
layout: default
section: data_events
title: "CLEARTIMEOUT"
description: "Clears a timeout that was previously started with SETTIMEOUT."
category: section
permalink: /data-events/reference/cleartimeout/
---

### Description

The CLEARTIMEOUT function clears a timeout that was previously started with [SETTIMEOUT](/data-events/reference/settimeout/).

### Parameters

`timerID` Number (__required__) - The timer ID to clear

### Examples

```js
// Starts a timer to alert after five minutes, and another timeout that clears the first one after four minutes.
// No alert is ever displayed.
ON('load-record', function(event) {
  var fiveMinutes = 1000 * 60 * 5;
  var fourMinutes = 1000 * 60 * 4;

  var timer = SETTIMEOUT(function() {
    ALERT("You've been editing this record for 5 minutes.");
  }, fiveMinutes);

  SETTIMEOUT(function() {
    CLEARTIMEOUT(timer);
  }, fourMinutes);
});
```