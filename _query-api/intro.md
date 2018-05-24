---
layout: default
section: query_api
order: 1
title: "Query API Introduction"
description: "Introduction to the Fulcrum Query API"
category: section
---

The Query API provides read-only access to interact with your Fulcrum tables and data (records, repeatables, choice lists, members, media, etc.). This endpoint supports most standard [PostgreSQL](https://www.postgresql.org/) database functions, including many advanced spatial types supported by the [PostGIS](http://postgis.net/) extension. The API enables real-time data analytics for integrating Fulcrum with a variety of Business Intelligence (BI) services and platforms, as well as permits more programmatic scripting for custom data exports.

The SQL syntax is PostgreSQL, so knowledge of other SQL dialects means you are probably already familiar with the basics of the Query API. Once enabled, every app in your account is accessible as a SQL table that you can query using its table name. For example, a query can be as simple as `SELECT * FROM Inspections` where `Inspections` is the name of the app (for table names longer than one word wrap quotations around the table name). The output can be controlled using standard URL parameters, including CSV, JSON and GeoJSON formats. When you make changes to the structure of your app definition in the builder, changes to the SQL tables are reflected automatically in the Query API.

The Query API also fully supports all field types and form configurations, including Repeatable fields and Record Link fields. In other words, sections of the form-- like photos, users, projects and more-- are queryable. Repeatable data can be accessed using the form name and data name, e.g. `SELECT * FROM "Inspections/rooms"` will return all of the child records in a `rooms` repeatable. Record links use the same technique and return a join table that you can use to join with the other app.

Check out the [Reference](/query-api/reference/) for more information on the specifics of the API. Using these basic building blocks, you can build up complex queries to return composite data to drive aggregation and analytics.

We currently have two open source repositories on GitHub for working with the Query API:

* [FQ](https://github.com/fulcrumapp/fq) is a Command Line Interface (CLI) built on Node.js. This can be used for programmatically exporting data and scripting custom workflows.
* [Fulcrum Query Utility](https://github.com/fulcrumapp/fulcrum-query-utility) is responsive web application for writing and executing queries and viewing/exporting the returned data.

## Reference

Check out the full documentation on required parameters, formatting calls, responses and functions by browsing the [Reference](/query-api/reference/) section.

## Functional Examples

We have a [library of examples](/query-api/examples/) available for learning what you can do with the Query API.

## Limitations

- The Query API is not currently enabled by default. If you are on a Professional plan and would like to try it out, please [contact us](http://help.fulcrumapp.com/).
- API calls are limited to 5,000 calls an hour per user.
- Queries cannot exceed 10 seconds of processing time to complete.

## SQL Reference Material

 - [SQL Tutorial](http://sqlzoo.net/)
 - [A Primer on SQL](https://leanpub.com/aprimeronsql/read)
 - [Learn SQL the Hard Way](http://sql.learncodethehardway.org/)
 - [A beginners guide to thinking in SQL](http://www.sohamkamani.com/blog/2016/07/07/a-beginners-guide-to-sql/)
 - [PostgreSQL JSON Functions and Operators](https://www.postgresql.org/docs/current/static/functions-json.html)
