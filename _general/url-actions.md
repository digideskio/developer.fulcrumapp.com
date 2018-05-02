---
layout: default
section: general
order: 5
title: "URL Actions"
description: "Using URL Actions for direct record access on web and mobile"
category: section
---

## Introduction

You can directly create new records and update existing records via URL parameters on both the web and mobile apps. This allows you to easily integrate Fulcrum with other applications and services to build custom workflows, such as:

- Scheduling work outside of Fulcrum and sending daily worksheets to your field crews.

- Adding record links to calendar events for scheduling and notification.

- Sending a text message with a record link for immediate inspection.

- Passing coordinates from another application or web map to create a new Fulcrum record.

## Web Actions

Records can be created and edited directly on the web using the following actions:

{:.table.table-striped}
| Action | Description |
|--------|-------------|
|`https://web.fulcrumapp.com/records/new`|Create a new record|
|`https://web.fulcrumapp.com/records/[record_id]?mode=edit`|Edit an existing record|


## Mobile Actions

Both the Android and iOS apps support opening the app using the `fulcrumapp://` URL scheme.

{:.table.table-striped}
| Action | Description |
|--------|-------------|
|`fulcrumapp://new-record`|Create a new record|
|`fulcrumapp://edit-record`|Edit an existing record|

## New Record Parameters

{:.table.table-striped}
| Parameter | Required | Description |
|-----------|----------|-------------|
| `form_id` | yes | The form ID to activate and use for the new record |
| `project_id` | no | The project ID of the new record |
| `status` | no | The status of the new record |
| `latitude` | no | The latitude of the new record |
| `longitude` | no | The latitude of the new record |
| `...attributes` | no | Any other URL encoded attributes should be `data_name=value` pairs to set on the new record |

## Edit Record Parameters

{:.table.table-striped}
| Parameter | Required | Description |
|-----------|----------|-------------|
| `form_id` | yes | The form ID to activate and use for the new record |
| `record_id` | yes | The record ID to edit |
| `project_id` | no | The project ID to set on the new record |
| `status` | no | The status of the record |
| `latitude` | no | The latitude of the record |
| `longitude` | no | The longitude of the record |
| `...attributes` | no | Any other URL encoded attributes should be `data_name=value` pairs to set on the record |

## Supported Field Types

{:.table.table-striped}
| Field | Description |
|-------|-------------|
| Text | _Uri encoded text value_ |
| Yes/No | _Uri encoded text value_ |
| Barcode | _Uri encoded text value_ |
| Hyperlink | _Uri encoded text value_ |
| SingleChoice ('Other' is not supported) | _Uri encoded text value_ |
| Classification | _Uri encoded, comma delimited list of values. ex: `1%2C2%2Chello%2C4`_ |
| Date | _YYYY-MM-DD_ |
| Time | _HH:MM_ |

## Validation

- `form_id` must exist for `new-record` action
- `record_id` must exist for `edit-record` action
- If `project_id` is passed
  - the project must exist
  - the active account's role must have permission to change projects
- If `status` is passed, the active account's role must have permission to change the status
- `latitude` must be a number and must be: -90 <= latitude <= 90
- `longitude` must be a number and must be: -180 <= longitude <= 180

## Examples

```
fulcrumapp://new-record?form_id=c55adab9-916d-46e9-98aa-7a2388a77b24&number_of_floors=3&sq_footage=2300
```

```
fulcrumapp://new-record?form_id=c55adab9-916d-46e9-98aa-7a2388a77b24&status=incomplete&sq_footage=2300&name=My%20Awesome%20Building&number_of_floors=3&latitude=28.038046&longitude=-81.952514
```

```
fulcrumapp://edit-record?record_id=11fb2a54-5158-4848-8695-c405c54525e4&status=incomplete&sq_footage=2300&name=SNI&number_of_floors=3&latitude=28.038046&longitude=-81.952514
```

## Notes

Several core Android apps, including Gmail and Messenger do not support custom schemes and will not correctly link to Fulcrum if you try to use the following `<a href="fulcrumapp://new-record?form_id=123-xyz">Create new record</a>`. We have setup a dedicated http page to help overcome this issue, by providing a web link, which opens the browser and redirects to open the Fulcrum app. If you are dealing with this issue, try the following:

* `<a href="https://www.fulcrumapp.com/action/#new-record?form_id=123-xyz">Create new record</a>` for new records
* `<a href="https://www.fulcrumapp.com/action/#edit-record?form_id=123-xyz&record_id=xyz-123">Edit record</a>` for existing edits

This solution should work for both Android and iOS.
