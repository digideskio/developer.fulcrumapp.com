---
layout: default
section: expressions
title: "ISBLANK"
description: "Checks whether the field&#39;s value is empty."
category: section
permalink: /expressions/reference/isblank/
---

## ISBLANK

Checks whether the field's value is empty.

### Parameters

`value` String (__required__) - Field value to check.

### Returns

Boolean

### Examples

~~~
ISBLANK("")

// returns true
~~~
{: .language-js}


~~~
ISBLANK("Test")

// returns false
~~~
{: .language-js}