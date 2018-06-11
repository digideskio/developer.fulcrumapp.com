---
layout: default
section: data_events
title: "ISPORTRAIT"
description: "Returns true if the photo or video is in portrait mode. This is intended to be used inside of the add-photo and add-video events and operate on the parameter passed to the event handler."
category: section
permalink: /data-events/reference/isportrait/
---

### Parameters

`value` Object (__required__) - The value of the photo or video

### Examples

```js
ON('add-photo', 'photos', function (event) {
   if (ISPORTRAIT(event.value)) {
     ALERT('Photo is portrait!');
   }
});
```