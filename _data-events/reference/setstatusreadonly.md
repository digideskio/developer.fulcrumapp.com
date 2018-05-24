---
layout: default
section: data_events
title: "SETSTATUSREADONLY"
description: "Sets the status field to be read-only or editable."
category: section
permalink: /data-events/reference/setstatusreadonly/
---

### Parameters

`readOnly` boolean,null (__required__) - Boolean value representing whether the field should be read-only, or `null` to restore the original state

### Examples

```js
SETSTATUSREADONLY(true);

// Sets the status field to read only, not editable by the user
```