---
layout: default
section: data_events
title: "PROMPT"
description: "Display a text field to get input from the user and a callback to respond to the result"
category: section
permalink: /data-events/reference/prompt/
---

### Parameters

`title` String (__required__) - A short title for the alert

`message` String (__required__) - The message content for the alert

`callback` function (__required__) - invoked when the message box is dismissed

### Examples

```js
PROMPT('Please enter the current year', function (result) {
  if (result.input === new Date().getFullYear()) {
    // Correct
  } else {
    // Incorrect
  }
});
```