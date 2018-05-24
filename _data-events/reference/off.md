---
layout: default
section: data_events
title: "OFF"
description: "Detaches an event handler set by ON."
category: section
permalink: /data-events/reference/off/
---

### Parameters

`event` string (__required__) - The event name

`callback` function (__required__) - The function to detach

### Examples

```js
OFF('validate-record', callback);

// Detaches an event handler from the validate-record event
```


```js
OFF('validate-record');

// Detaches all event handlers listening to the 'validate-record' event
```