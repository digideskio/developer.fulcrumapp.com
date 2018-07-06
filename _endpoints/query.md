---
layout: default
section: endpoints
order: 2
title: "Query "
description: "SQL read access to account tables and objects"
category: section
img: /assets/img/guides/
difficulty: advanced
menu:
  - "Endpoints": endpoints
  - "Query Parameters": query-parameters
  - "Notes": notes
---

The Query API provides read-only access to interact with your Fulcrum tables and data (records, repeatables, choice lists, members, media, etc.). This endpoint supports most standard [PostgreSQL](https://www.postgresql.org/) database functions, including many advanced spatial types supported by the [PostGIS](http://postgis.net/) extension. This enables real-time data analytics and integrating Fulcrum with a variety of Business Intelligence services and other platforms, as well as programmatically scripting custom data exports.

A more detailed [introduction](/query-api/intro/), [reference documentation](/query-api/reference/), and [examples](/query-api/examples/) have been included in a separate expanded section.

## Endpoints

{:.table.table-striped}
| Method | Endpoint | Description |
|--------|----------|-------------|
| GET/POST | /api/v2/query | Fetch objects. Additional parameters may be supplied (see query parameters table below). |

## Query Parameters

Available parameters to query the objects in your account. All of the parameters may be used together to filter your data for more accurate results.

{:.table.table-striped}
| Parameter | Type | Description | Required |
|-----------|------|-------------|----------|
| token | string | Your Fulcrum API token. | yes |
| q | string | The SQL query. | yes |
| format | string | The format of the results returned by the query. Options include csv, json, geojosn. Defaults to csv. | no |
| headers | string | Include headers for CSV format | no |
| page | integer | The page number requested. | no |
| per_page | integer | Number of results per page. By default, all requests are paginated to the maximum value of 20,000 items per request. | no |
| sort_column | string | The name of the column used to sort on. | no |
| sort_direction | string | The sort direction (asc, desc). | no |

## Notes

* The Query API is read-only and _cannot modify data_ in your account.
* You can use the Query API in conjunction with the [Records API](/endpoints/records/) to make updates to specific records by fetching the IDs of the records you want to PUT/DELETE.
* Be careful sharing queries that expose your API token. Even though the Query API is read-only, if a token with write privileges is used, your data could still be compromised. Consider creating a [custom role](http://help.fulcrumapp.com/user-management/how-can-i-create-custom-roles-with-specific-access-rights) with restricted permissions (uncheck all boxes for read-only).
