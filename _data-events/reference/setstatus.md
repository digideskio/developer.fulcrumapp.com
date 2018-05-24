---
layout: default
section: data_events
title: "SETSTATUS"
description: "Set the status of a record."
category: section
permalink: /data-events/reference/setstatus/
---

### Parameters

`status` String (__required__) - The status value for the record. Must be a valid status option!

### Examples

```js
// Sets the status of a record
SETSTATUS('inspection_pending');
```