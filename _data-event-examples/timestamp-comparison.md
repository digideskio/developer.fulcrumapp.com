---
layout: default
section: data_events
title: "Prevent editing an old record"
description: "Use this to prevent a user from editing a record that was created at some defined time in the past."
category: section
tags:
  - alert
  - timestamp
  - validation
---

This example listens for record edits and compares the current timestamp with the `created_at` timestamp of the record. If the record was created more than 30 days ago, the user receives an [ALERT](/data-events/reference/alert/) that the record can no longer be edited and prompts them to create a new record. The record is also marked as [INVALID](/data-events/reference/invalid/) so edits cannot be saved.

```js
function daysBetween(date1, date2) {
  // The number of milliseconds in one day
  var oneDay = 1000 * 60 * 60 * 24;
  // Convert both dates to milliseconds
  var date1MS = date1.getTime();
  var date2MS = date2.getTime();
  // Calculate the difference in milliseconds
  var differenceMS = Math.abs(date1MS - date2MS);
  // Convert back to days and return
  return Math.round(differenceMS/oneDay);
}

ON('edit-record', function (event) {
  var now = new Date();
  var createdAt = new Date(this.featureCreatedAt * 1000);
  var age = daysBetween(now, createdAt);
  if (age > 30) {
    var message = 'This record was created over 30 days ago and can no longer be edited. Please create a new record';
    ALERT('Warning!', message);
    ON('validate-record', function (event) {
     INVALID(message);
    });
  }
});
```
