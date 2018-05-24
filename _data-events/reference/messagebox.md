---
layout: default
section: data_events
title: "MESSAGEBOX"
description: "Display a message box with configurable title, message, buttons and optional text input."
category: section
permalink: /data-events/reference/messagebox/
---

### Description

MESSAGEBOX displays a message to the user. You can provide both the title and message of the alert box. Using the `buttons` parameter you can specify the button titles that are displayed in the message box.

### Parameters

`options` Object (__required__) - The options for the message box

`callback` function (__required__) - invoked when the message box is dismissed

### Examples

```js
MESSAGEBOX({title: 'Confirm', message: 'You have selected a critical safety violation. Are you sure?', buttons: ['Yes', 'No']}, function (result) {
  if (result.value === 'Yes') {
    // Selected Yes
  } else {
    // Selected No
  }
});
```