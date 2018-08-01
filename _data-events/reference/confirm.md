---
layout: default
section: data_events
title: "CONFIRM"
description: "Display a question to the user with an &#39;Okay or Cancel&#39; response and a callback to respond to the result"
category: section
permalink: /data-events/reference/confirm/
---

### Description

CONFIRM displays a message to the user and allows a callback function that will be invoked to respond to the result of the question.

<img src="../media/confirm.gif" alt="CONFIRM() Data Event example" style="float: right; margin-left: 40px;" />

### Parameters

`title` String (__required__) - A short title for the alert

`message` String (__required__) - The message content for the alert

`callback` function (__required__) - invoked when the message box is dismissed

### Examples

```js
CONFIRM('Confirm', 'You have selected a critical safety violation. Are you sure?', function (result) {
  if (result.value === 'Okay') {
    // Selected Okay
  } else {
    // Selected Cancel
  }
});
```