---
layout: default
section: endpoints
order: 7
title: "Projects"
description: "Create, read, update, or delete projects"
category: section
img: /assets/img/guides/
difficulty: advanced
menu:
  - "Endpoints": endpoints
  - "Query Parameters": query-parameters
  - "Properties": project-properties
  - "Validations": validations
  - "Notes": notes
  - "Examples": examples
---

The Projects API gives you access to the [projects](http://help.fulcrumapp.com/web-app/what-are-projects) within your Fulcrum account. Projects can be used to separate your data into different groups for access, filtering, and exporting purposes.

## Endpoints

{:.table.table-striped}
| Method | Endpoint | Description |
|--------|----------|-------------|
| GET | /api/v2/projects.json | Fetch all projects. |
| GET | /api/v2/projects/**:id**.json | Fetch a single project. |
| POST | /api/v2/projects.json | Create a new project. |
| PUT | /api/v2/projects/**:id**.json | Update a single project. |
| DELETE | /api/v2/projects/**:id**.json | Delete a single project. |

## Query Parameters

{:.table.table-striped}
| Parameter | Type | Description |
|-----------|------|-------------|
| page | integer | The page number requested. |
| per_page | integer | Number of records per page. By default, all requests are paginated to the maximum value of 20,000 items per request. |

## Project Properties

{:.table.table-striped}
| Property | Type | Required | Readonly | Description |
|----------|------|----------|----------|-------------|
| name | string | yes | no | The name of the project. |
| description | string | no | no | Optional project description. |
| id | string | no | yes | The id of the project. |
| created_at | string | no | yes | Timestamp when the project was created. |
| updated_at | string | no | yes | Timestamp when the project was last updated. |

## Validations

The following properties must be included in order to create/update a project object in our system. Any validation errors will return a `422` and an object with a list of validation errors.

### Required Properties

{:.table.table-striped}
| Property | Type | Description | Example |
|----------|------|-------------|---------|
| name | string | The name of the project. | `"Pinellas County"` |

Example validation response if `name` is not included:

```json
{
  "project": {
    "errors": {
      "name": ["can't be blank"]
    }
  }
}
```

## Notes

* The entire project object is required when making an update. Omitting fields with existing data will result in data loss! The typical workflow for updating an existing project is to fetch the project object, modify it, and then submit the PUT request.

## Examples

### Valid project response

```json
{
  "project": {
    "name": "Pinellas County",
    "description": "For records in Pinellas County",
    "id": "60786571-46b4-43b8-8f49-7d8f0f45987e",
    "created_at": "2013-10-29T15:49:09Z",
    "updated_at": "2014-11-18T15:23:48Z"
  }
}
```

### Get all projects

#### cURL
```sh
curl --request GET 'https://api.fulcrumapp.com/api/v2/projects.json' \
--header 'Accept: application/json' \
--header 'X-ApiToken: my-api-key'
```

#### jQuery
```js
$.ajax({
  type: "GET",
  url: "https://api.fulcrumapp.com/api/v2/projects.json",
  contentType: "application/json",
  dataType: "json",
  headers: {
    "X-ApiToken": "my-api-key"
  },
  success: function (data) {
    // do something!
    console.log(data);
  }
});
```

### Get a single project by ID

#### cURL
```sh
curl --request GET 'https://api.fulcrumapp.com/api/v2/projects/my-project-id.json' \
--header 'Accept: application/json' \
--header 'X-ApiToken: my-api-key'
```

#### jQuery
```js
$.ajax({
  type: "GET",
  url: "https://api.fulcrumapp.com/api/v2/projects/my-project-id.json",
  contentType: "application/json",
  dataType: "json",
  headers: {
    "X-ApiToken": "my-api-key"
  },
  success: function (data) {
    // do something!
    console.log(data);
  }
});
```

### Create a new project

#### cURL
```sh
curl --request POST 'https://api.fulcrumapp.com/api/v2/projects.json' \
--header 'Accept: application/json' \
--header 'Content-Type: application/json' \
--header 'X-ApiToken: my-api-key' \
--data '{"project": {"name": "Pinellas County","description": "For records in Pinellas County"}}'
```

#### jQuery
```js
$.ajax({
  type: "POST",
  url: "https://api.fulcrumapp.com/api/v2/projects.json",
  data: JSON.stringify({
      "project": {
        "name": "Pinellas County",
        "description": "For records in Pinellas County"
      }
  }),
  contentType: "application/json",
  dataType: "json",
  headers: {
    "X-ApiToken": "my-api-key"
  },
  success: function (data) {
    // do something!
    console.log(data);
  }
});
```

### Update a project

#### cURL
```sh
curl --request PUT 'https://api.fulcrumapp.com/api/v2/projects/my-project-id.json' \
--header 'Accept: application/json' \
--header 'Content-Type: application/json' \
--header 'X-ApiToken: my-api-key' \
--data '{"project": {"name": "Pinellas County, FL","description": "For records in Pinellas County, FL"}}'
```

#### jQuery
```js
$.ajax({
  type: "PUT",
  url: "https://api.fulcrumapp.com/api/v2/projects/my-project-id.json",
  data: JSON.stringify({
    "project": {
      "name": "Pinellas County, FL",
      "description": "For records in Pinellas County, FL"
    }
  }),
  contentType: "application/json",
  dataType: "json",
  headers: {
    "X-ApiToken": "my-api-key"
  },
  success: function (data) {
    // do something!
    console.log(data);
  }
});
```

### Delete a project

#### cURL
```sh
curl --request DELETE 'https://api.fulcrumapp.com/api/v2/projects/my-project-id.json' \
--header 'Accept: application/json' \
--header 'X-ApiToken: my-api-key'
```

#### jQuery
```js
$.ajax({
  type: "DELETE",
  url: "https://api.fulcrumapp.com/api/v2/projects/my-project-id.json",
  contentType: "application/json",
  dataType: "json",
  headers: {
    "X-ApiToken": "my-api-key"
  },
  success: function (data) {
    // do something!
    console.log(data);
  }
});
```
