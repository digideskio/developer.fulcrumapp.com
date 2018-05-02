---
layout: default
section: query_api
order: 2
title: "Query API Reference"
description: "Reference Information for the Fulcrum Query API"
category: section
menu:
  - "Authentication": authentication
  - "Endpoints": endpoints
  - "Query Parameters": query-parameters
  - "Formatting Calls": formatting-calls-to-the-query-api
  - "Response Formats": response-formats
  - "Fulcrum Table Types": fulcrum-table-types
  - "Form Tables": form-tables
  - "Helper Functions": fulcrum-sql-helper-functions
---

## Authentication

Fulcrum enforces secure API access that requires you to authorize your queries. In order to authorize queries, you can use an API token.

The API token offers the simplest way to access private data or perform actions to your public data. Your API token protects access to your data, keeps it confidential and only permits sharing if you want others to have this access. If necessary, you can reset your API key in the Fulcrum admin settings.

## Endpoints

{:.table.table-striped}
| Method | Endpoint | Description |
|--------|----------|-------------|
| GET/POST | /api/v2/query | Fetch objects. Additional parameters must be supplied (see query parameters table below). |

## Query Parameters

Below are the available parameters for the query endpoint. Unless stated otherwise, the parameters can be combined to perform more complex queries.

{:.table.table-striped}
| Parameter | Type | Description | Required |
|-----------|------|-------------|----------|
| token | string | Your Fulcrum API token. | yes |
| q | string | The SQL query. | yes |
| format | string | The format of the results returned by the query. Valid formats include `csv`, `json`, and `geojson`. Defaults to `csv`. | no |
| headers | string | Include headers in CSV format. Defaults to yes for `csv` format. | no |
| page | integer | The page number requested. | no |
| per_page | integer | Number of results per page. | no |
| sort_column | string | The name of the column used to sort on. | no |
| sort_direction | string | The sort direction (asc, desc). Defaults to unspecified order. | no |

## Formatting Calls to the Query API

All Query API requests should follow this general pattern:

```
https://api.fulcrumapp.com/api/v2/query?token={API Token}&q={SQL Statement}
```

If you encounter errors, double-check that you are using the correct API token, and that your SQL statement is valid. A common use case example is to get a count of the records in a form, grouped by status:

**Example**

```
https://api.fulcrumapp.com/api/v2/query/?token=abc-123&q=SELECT _status, COUNT(*) FROM "Park Inventory" GROUP BY _status ORDER BY COUNT(*) DESC
```

**Result**

```
_status,count
City,19
Private,5
Local,3
County,3
State,1
```

## Response Formats

The default response format is CSV, but data can also be returned as JSON or GeoJSON. The lightweight JSON format allows you to easily integrate your Fulcrum data into web or mapping applications. The same results can be returned in JSON format by including the `format=json` parameter.

**County Parks (JSON)**

```
https://api.fulcrumapp.com/api/v2/query/?token=abc-123&format=json&q=SELECT park_name, address FROM "Park Inventory" WHERE _status = 'County'
```

**JSON Result**

```json
{
  "fields": [{
    "name": "park_name",
    "type": "string"
  }, {
    "name": "address",
    "type": "string"
  }],
  "rows": [{
    "park_name": "Philippe Park",
    "address": "2525 Philippe Parkway 2525 Safety Harbor FL 34695 US"
  }, {
    "park_name": "Sawgrass Lake Park",
    "address": "7400 25th St N 7400 Saint Petersburg FL 33702 US"
  }, {
    "park_name": "Eagle Lake Park",
    "address": "1800 Keene Rd 1800 Clearwater FL 33764 US"
  }],
  "time": 0.001
}
```

*Note that the JSON format includes additional useful information on the fields and time taken to execute the query.*

**County Parks (GeoJSON)**

```
https://api.fulcrumapp.com/api/v2/query/?token=abc-123&format=geojson&q=SELECT park_name, address, _geometry FROM "Park Inventory" WHERE _status = 'County'
```

**GeoJSON Result**

```json
{
  "type": "FeatureCollection",
  "features": [{
    "type": "Feature",
    "properties": {
      "park_name": "Philippe Park",
      "address": "2525 Philippe Parkway 2525 Safety Harbor FL 34695 US"
    },
    "geometry": {
      "type": "Point",
      "coordinates": [-82.6788122579455, 28.0060460720531]
    }
  }, {
    "type": "Feature",
    "properties": {
      "park_name": "Sawgrass Lake Park",
      "address": "7400 25th St N 7400 Saint Petersburg FL 33702 US"
    },
    "geometry": {
      "type": "Point",
      "coordinates": [-82.6685400679708, 27.8407134982167]
    }
  }, {
    "type": "Feature",
    "properties": {
      "park_name": "Eagle Lake Park",
      "address": "1800 Keene Rd 1800 Clearwater FL 33764 US"
    },
    "geometry": {
      "type": "Point",
      "coordinates": [-82.7624716982245, 27.9324083125492]
    }
  }]
}
```

*Note that the GeoJSON format requires the `_geometry` column in the query.*

## Fulcrum Table Types

The basic `SELECT * FROM tables;` query will return all of the tables available for use and is a good place to start when exploring the capabilities of the Query API.

{:.table.table-striped}
| Type | Description |
|------|-------------|
| `form` | These tables contain the records for each form. These are the primary tables used in the Query API and contain all the main record data. |
| `repeatable` | These tables contain child records. Child tables have some additional columns, `_child_record_id` and `_parent_id` that can be used to link child records to the parent records. Note that because Fulcrum supports nested child records, it's possible that child records have their own child records. Using `_child_record_id`, `_parent_id` and `_record_id` it's possible to construct queries that can link records together going down or up repeatable levels.  |
| `link` | These tables help you join linked records. They are join tables for connecting linked apps. For each Record Link field, there will be a join table that contains a row for each link between records. You can think of this table as representing the "arrows" between related records when using Record Link fields. There will be a row for each "arrow" and `source_record_id` is the start of the arrow and `linked_record_id` is the arrowhead. |
| `media` | These tables help you join media with records. Each photo, video, and audio field will have its own associated table that can be used to connect the record data with the `photos`, `audio` and `videos` system tables. Each media field also has an array column on the record table containing the ID's, but these `media` tables let you use more conventional SQL joins to connect the data rather than more complex array operations to link multiple photos in a single photo to the `photos` table. |
| `system` | Fulcrum system tables: audio, changesets, choice_lists, classification_sets, forms, memberships, photos, projects, roles, signatures, videos |

## Form Tables

Fulcrum form tables hold the records for each form in your account.

* You can query a form by its name (`SELECT * FROM "Park Inventory";`) or its ID (`SELECT * FROM "19da1bbc-43e4-49b3-b70f-a63ec680268e";`).

* Repeatable records can be accessed with the following table name pattern: `"Table Name/repeatable_column"`.

* Record links can be accessed with the following table name pattern: `"Table Name/record_link_column"`.

* Media records (photos, video, audio, signatures) can be accessed with the following table name pattern: `"Table Name/media_column"`.

### System Columns

Every Fulcrum form contains standard system columns, in addition to your custom fields. System columns are prepended with an underscore (`_`) to help distinguish them easier.

{:.table.table-striped}
| Column | Type | Description |
|--------|------|-------------|
| _record_id | string | The ID of the [record](/endpoints/records/). |
| _project_id | string | The ID of the [project](/endpoints/projects/). Can be joined to the `project_id` column in the `projects` table. |
| _assigned_to_id | string | The ID of the [member](/endpoints/users/) the record is assigned to.  Can be joined to the `user_id` column in the `memberships` table. |
| _status | string | The record [status](http://help.fulcrumapp.com/field-types/what-is-a-status-field). |
| _version | number | The record [version](http://help.fulcrumapp.com/web-app/how-does-version-history-work). |
| _title | string | The record [title](http://help.fulcrumapp.com/field-types/what-is-a-title-field). |
| _created_at | date | The timestamp when the record was created on the device. |
| _updated_at | date | The timestamp when the record was updated on the device. |
| _server_created_at | date | The timestamp when the record was created on the server. |
| _server_updated_at | date | The timestamp when the record was updated on the server. |
| _created_by_id | string | The ID of the [member](/endpoints/users/) the record was created by.  Can be joined to the `user_id` column in the `memberships` table. |
| _updated_by_id | string | The ID of the [member](/endpoints/users/) the record was updated by.  Can be joined to the `user_id` column in the `memberships` table. |
| _changeset_id | string | The ID of the last [changeset](/endpoints/changesets/). Can be joined to the `changeset_id` column of the changesets table. |
| _latitude | number | The record latitude. |
| _longitude | number | The record longitude. |
| _geometry | geometry | The record geometry as a native PostGIS geometry. When output to CSV, it's in Extended Well-Known Text ([EWKT](http://postgis.net/docs/ST_GeomFromEWKT.html)) format. |
| _altitude | number | The GPS altitude in meters above/below (+/-) sea level. |
| _speed | number | The GPS speed in meters per second. |
| _course | number | The GPS course in degrees (0.0-360), only recorded if the device is moving. |
| _horizontal_accuracy | number | The GPS latitude and longitude accuracy in meters. |
| _vertical_accuracy | number | The GPS altitude accuracy in meters. (Only reported on iOS devices) |

## Fulcrum SQL Helper Functions

In addition to the standard PostgreSQL & PostGIS functions, there are several Fulcrum-specific helper functions for formatting data and handling media attachments.

#### `FCM_ConvertToFloat(input_value text) RETURNS double precision`

```sql
FCM_ConvertToFloat('1.2')   -- 1.2
FCM_ConvertToFloat('1000')  -- 1000
FCM_ConvertToFloat('a')     -- NULL
```

Convert a textual value to a floating point value. This function is similar to a cast, except it gracefully fails to `NULL` when the input cannot be converted to a number. This is useful for text data that's mostly numbers but might have some invalid values in it.

<hr>

#### `FCM_FormatTimestamp(ts timestamp without time zone, tz text DEFAULT 'UTC') RETURNS text`

Convert a timestamp to a different time zone. This is useful for localizing your timestamps to your own time zone. The `tz` argument is a string representing the time zone to use. This string can be in any format supported by the PostgreSQL `AT TIME ZONE` construct.

```sql
SELECT FCM_FormatTimestamp(current_timestamp, 'EST');               -- Eastern Standard Time
SELECT FCM_FormatTimestamp(current_timestamp, 'EDT');               -- Eastern Daylight-Saving Time
SELECT FCM_FormatTimestamp(current_timestamp, '-04:00');            -- 4 hours behind UTC
SELECT FCM_FormatTimestamp(current_timestamp, '-05:00');            -- 5 hours behind UTC
SELECT FCM_FormatTimestamp(current_timestamp, 'America/New_York');  -- New York time, either EST or EDT depending on the timestamp
```

The above examples show some of the ways the function can be used. The last form using `America/New_York` is the most useful because it takes into account the changes in the timezones given the exact timestamp. For example, New York is sometimes on EST and sometimes on EDT.

<hr>

#### `FCM_Photo(id text, version text DEFAULT NULL) RETURNS fcm_file`

* `id` The photo ID
* `version` (optional, default `NULL`) The photo version. One of:
  * `NULL` (original)
  * `'large'` (jpg)
  * `'thumb'` (jpg)

Returns a photo URL in the output for a single photo.

The following will return a secure URL directly to the raw file, using its ID.

```sql
SELECT FCM_Photo('c515f1d6-e882-4027-9e5c-11e44b4c181c', 'thumb') AS photo_url;
```

The following will return a secure URL directly to the raw file for the second photo in a particular record.

```sql
SELECT FCM_Photo(photo_field[2], 'large') AS photo_url FROM "Building Inspections" WHERE _record_id='69daadc7-f68c-4c7c-8b40-7d9ea9e6d6c5';
```

The following will return secure URLs directly to the raw files for the first photos of all records in a table.

```sql
SELECT FCM_Photo(photo_field[1], 'thumb') AS photo_urls FROM "Building Inspections";
```

#### `FCM_Photo(ids text[], version text DEFAULT NULL) RETURNS fcm_file[]`

* `ids` An array of photo IDs, this works naturally with the way photo fields are stored.
* `version` (optional, default `NULL`) The photo version. One of:
  * `NULL` (original)
  * `'large'` (jpg)
  * `'thumb'` (jpg)

Returns an array of photo URLs in the output.

The following will return secure URLs directly to the raw files. The consumer of the output needs to be able to handle multiple URLs.

```sql
SELECT FCM_Photo(unnest(photo_field), 'thumb') AS photo_urls FROM "Building Inspections";
```

*Here we use `unnest` to expand an array to a set of rows.*

<hr>

#### `FCM_Video(id text, version text DEFAULT NULL) RETURNS fcm_file`

* `id` The video ID
* `version` (optional, default `NULL`) The video version. One of:
  * `NULL` (original)
  * `small` (mp4)
  * `medium` (mp4)
  * `preview_small` (gif)
  * `preview_medium` (gif)
  * `thumbnail_small` (jpg)
  * `thumbnail_medium` (jpg)
  * `thumbnail_large` (jpg)
  * `thumbnail_huge` (jpg)
  * `thumbnail_small_square` (jpg)
  * `thumbnail_medium_square` (jpg)
  * `thumbnail_large_square` (jpg)
  * `thumbnail_huge_square` (jpg)

Returns video URLs in the output for a single video.

The following will return a secure URL directly to the raw file.

```sql
SELECT FCM_Video(video_field[1]) AS video_url FROM "Building Inspections";
```

#### `FCM_Video(ids text[], version text DEFAULT NULL) RETURNS fcm_file[]`

* `ids` The video IDs
* `version` (optional, default `NULL`) The video version. One of:
  * `NULL` (original)
  * `small` (mp4)
  * `medium` (mp4)
  * `preview_small` (gif)
  * `preview_medium` (gif)
  * `thumbnail_small` (jpg)
  * `thumbnail_medium` (jpg)
  * `thumbnail_large` (jpg)
  * `thumbnail_huge` (jpg)
  * `thumbnail_small_square` (jpg)
  * `thumbnail_medium_square` (jpg)
  * `thumbnail_large_square` (jpg)
  * `thumbnail_huge_square` (jpg)

Returns video URLs in the output for multiple videos.

The following will return secure URLs directly to the raw video files.

```sql
SELECT FCM_Video(video_field) AS video_urls FROM "Building Inspections";
```

<hr>

#### `FCM_Audio(id text, version text DEFAULT NULL) RETURNS fcm_file`

* `id` The audio ID
* `version` (optional, default `NULL`) The audio version. One of:
  * `NULL` (original)
  * `small` (m4a)
  * `medium` (m4a)

Returns audio URLs in the output for a single audio file.

The following will return a secure URL directly to the raw audio file.

```sql
SELECT FCM_Audio(audio_field[1]) AS audio_url FROM "Building Inspections";
```

#### `FCM_Audio(ids text[], version text DEFAULT NULL) RETURNS fcm_file[]`

* `ids` The audio IDs
* `version` (optional, default `NULL`) The audio version. One of:
  * `NULL` (original)
  * `small` (m4a)
  * `medium` (m4a)

Returns audio URLs in the output for multiple audio files.

The following will return secure URLs directly to the raw audio files.

```sql
SELECT FCM_Audio(audio_field) AS audio_urls FROM "Building Inspections";
```

<hr>

#### `FCM_Signature(id text, version text DEFAULT NULL) RETURNS fcm_file`

* `id` The signature ID
* `version` (optional, default `NULL`) The signature version. One of:
  * `NULL` (original)
  * `'large'` (png)
  * `'thumb'` (png)

Returns a signature URL in the output for a single signature.

The following will return secure URLs directly to the raw signature files.

```sql
SELECT FCM_Signature(signature_field) AS signature_url FROM "Building Inspections";
```
