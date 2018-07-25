---
layout: default
section: expressions
title: "Hours between pair of date and time fields"
description: "Find the total number of hours between pair of date and time fields."
category: section
---

This code will calculate the hours between a pair a start date and start time field and a end date and end time field.

```js
function diff_hours(dt2, dt1) {
  var diff =(dt2.getTime() - dt1.getTime()) / 1000;
  diff /= (60 * 60);
  return Math.abs(Math.round(diff));
}

var startDate = $start_date_field;
var startTime = $start_time_field;
var endDate = $end_date_field;
var endTime = $end_time_field;

var startTimeStamp = new Date(startDate.getFullYear() + '-' + startDate.getMonth() + '-' + startDate.getDate() + ' ' + startTime + ':00');
var endTimeStamp = new Date(endDate.getFullYear() + '-' + endDate.getMonth() + '-' + endDate.getDate() + ' ' + endTime + ':00');

SETRESULT(diff_hours(endTimeStamp, startTimeStamp));
```

Copy and paste the entire code block above into the expression section of your calculation field, making sure to replace `$start_date_field`, `$start_time_field`,  `$end_date_field`, and `$end_time_field` with the data names of your date and time fields.
