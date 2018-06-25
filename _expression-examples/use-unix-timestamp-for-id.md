---
layout: default
section: expressions
title: "Unix timestamp for unique record value"
description: "Using the Unix timestamp to generate a unique record value."
category: section
---

While Fulcrum generates a record id for every record in Fulcrum, this id is quite long and sometimes too long to use. What you can do is use the [Unix Timestamp](https://en.wikipedia.org/wiki/Unix_time) to generate a unique numeric record value that can be more manageable to use.

The [ONCE()](/expressions/reference/once/) expression is used to ensure that the expression will only be run one time.

The expression below will generate unique value with 13 characters.

```js
ONCE(Date.now());
```
The example below divides the unix timestamp by 1000 and drops the decimals places using the [FLOOR()](/expressions/reference/floor/) expression. This generates a unique value with 10 characters.

```js
ONCE(FLOOR(Date.now()/1000));
```
The example below reduces the unique value to 8 characters.

```js
ONCE(FLOOR((Date.now() - 1451606400000) / 1000));
```
