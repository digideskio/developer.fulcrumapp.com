---
layout: default
section: data_events
title: "Require project"
description: "Use this custom validation logic to ensure every record has a project selected before saving."
category: section
tags:
  - validation
  - qa/qc
---

This example uses the `validate-record` event in conjunction with the [INVALID]({{ site.url }}/data-events/reference/invalid) function and [PROJECTNAME]({{ site.url }}/expressions/reference/projectname/) expression to prevent saving if the user has not associated a [project](http://help.fulcrumapp.com/web-app/what-are-projects) with the record.

```js
ON('validate-record', function (event) {
  if (!PROJECTNAME()) {
    INVALID('Please select a project before saving.');
  }
});
```
