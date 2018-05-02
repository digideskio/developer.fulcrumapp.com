---
layout: default
section: data_events
title: "Basic Timer"
description: "A simple stopwatch-like timer that allows you to start, pause, and reset."
category: section
tags:
  - set value
  - set interval
  - clear interval
---

This example can be utilized in situations where survey details need to be timed and documented, such as wildlife observations or recording various occurrence durations.

Please consider the [Record Auditing Duration]({{ site.url }}/blog/august-2016-updates/) metrics which are now captured in your Fulcrum records - if those features may meet your needs for time capture.

The `SETTIMEOUT` function can also be used for similar situations, such as [alerting a user with a message](http://developer.fulcrumapp.com/data-events/reference/settimeout/) after a specified amount of time.

<img src="../media/timer.gif" alt="Stopwatch/Timer Example" style="float: right; margin-left: 40px;" />

```js
// this Data Event utilizes a Yes/No field (timer) with the N/A choice enabled,
// respectively representing Start/Pause and Reset actions,
// and a readonly Text field (elapsed_time).

var seconds = 0, minutes = 0, hours = 0;

function timer(event) {
  if (VALUE('timer') === 'start') {
    interval = SETINTERVAL(function() {
      seconds++;
      if (seconds >= 60) {
        seconds = 0;
        minutes++;
        if (minutes >= 60) {
          minutes = 0;
          hours++;
        }
      }
      SETVALUE('elapsed_time', (hours ? (hours > 9 ? hours : "0" + hours) : "00") + ":" + (minutes ? (minutes > 9 ? minutes : "0" + minutes) : "00") + ":" + (seconds > 9 ? seconds : "0" + seconds));
    }, 1000);
  } else if (VALUE('timer') === 'pause') {
    CLEARINTERVAL(interval);
  } else if (VALUE('timer') === 'reset') {
    CLEARINTERVAL(interval);
    seconds = 0; minutes = 0; hours = 0;
    SETVALUE('elapsed_time', '00:00:00');
  }
}

ON('change', 'timer', timer);
```
