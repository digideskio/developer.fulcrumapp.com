---
layout: default
section: endpoints
order: 15
title: "Memberships"
description: "Read, update, or delete project, layers, and form memberships"
category: section
img: /assets/img/guides/
difficulty: advanced
menu:
  - "Endpoints": endpoints
  - "Query Parameters": query-parameters
  - "Properties": membership-properties
  - "Notes": notes
  - "Examples": examples
---

The Memberships API allows you to view, add, or remove non-owner users associated with the projects, forms, and layers in your Fulcrum account.

## Endpoints

{:.table.table-striped}
| Method | Endpoint | Description |
|--------|----------|-------------|
| GET | /api/v2/memberships.json | Fetch all members in your organization |
| POST | /api/v2/memberships/change_permissions.json | Add or remove membership permissions from layers, forms, or projects |

## Query Parameters

Available parameters to query the members associated with your account. All of the parameters may be used together to filter your data for more accurate results.

{:.table.table-striped}
| Parameter | Type | Description |
|-----------|------|-------------|
| form_id | string | Fetch all members for a given form. |
| project_id | string | Fetch all members for a given project. |
| layer_id | string | Fetch all members for a given layer. |

## Membership Properties

{:.table.table-striped}
| Property | Type | Required | Readonly | Description |
|----------|------|----------|----------|-------------|
| id | string | no | yes | The member ID |
| created_at | timestamp | no | yes | When the member was created |
| updated_at | timestamp | no | yes | Last time the member was updated |
| gravatar_email | string | no | yes | Email used for gravatar image |
| gravatar_image_url | string | no | yes | URL to gravatar image |
| user_id | string | no | yes | User ID |
| user | string | no | yes | User full name |
| first_name | string | no | yes | User first name |
| last_name | string | no | yes | User last name |
| email | string | no | yes | User email |
| role_id | string | no | yes | ID of the user's role |
| image_small | string | no | yes | User profile image (300px)|
| image_large | string | no | yes | User profile image (600px)|

## Notes
 - Users with a role of _Owner_ always have access to all resources within the organization.
 - The `change permissions` endpoint accepts a JSON object labeled `change` with three parameters.
 - The first parameter is `type`, which indicates which object you want to change the membership permissions of. Valid type values are `project_members`, `form_members`, and `layer_members`.
 - The second parameter is the id of the object you want to change the permission of. Valid ID keys are `project_id`, `form_id` and `layers_id`. Values must be valid resource IDs.
 - The third parameter is the changeset you would like to add or remove. This is a key of either `add` or `remove` with the value an array of valid member IDs that you would like to alter.

## Examples

### Valid Membership Response
```json
{
    "memberships": [
         {
            "id": "49ee5c1a-220f-4deb-8a9b-775706f66433",
            "created_at": "2018-01-19T23:38:39Z",
            "updated_at": "2018-01-19T23:38:39Z",
            "gravatar_email": null,
            "gravatar_image_url": "https://s.gravatar.com/avatar/d41d8cd98f00b204e9800998ecf8427e?s=80",
            "user_id": "8e20fc7e-33c3-4a34-9b13-167b24ebe89c",
            "user": "Robb Stark",
            "first_name": "Robb",
            "last_name": "Stark",
            "email": "robbstark@harrenhall.com",
            "role_id": "5a326552-865c-4c91-a45b-4262d4b6aedf",
            "image_small": "https://s.gravatar.com/avatar/d41d8cd98f00b204e9800998ecf8427e?s=300",
            "image_large": "https://s.gravatar.com/avatar/d41d8cd98f00b204e9800998ecf8427e?s=600"
        },
        {
            "id": "b21cc100-08b1-41ba-8353-d6e7a97debee",
            "created_at": "2018-01-19T23:38:39Z",
            "updated_at": "2018-01-22T18:48:46Z",
            "gravatar_email": null,
            "gravatar_image_url": "https://s.gravatar.com/avatar/d41d8cd98f00b204e9800998ecf8427e?s=80",
            "user_id": "c535156f-2871-4084-a399-d3061b25fae4",
            "user": "Walder Frey",
            "first_name": "Walder",
            "last_name": "Frey",
            "email": "walderfrey@thetwins.com",
            "role_id": "5696b7e9-765f-42b4-a61c-2895f257aab3",
            "image_small": "https://s.gravatar.com/avatar/d41d8cd98f00b204e9800998ecf8427e?s=300",
            "image_large": "https://s.gravatar.com/avatar/d41d8cd98f00b204e9800998ecf8427e?s=600"
        }
    ]
}
```

### Get all memberships for a particular project
```sh
curl -H "X-ApiToken: token" -H "Content-Type: application/json" https://api.fulcrumapp.com/api/v2/memberships.json?project_id=3751f03b-043d-48bf-a6cc-747ddd36b777
```

### Add a new member to a form
```sh
curl -XPOST -H "X-ApiToken: token" -H "Content-Type: application/json" -d@- https://api.fulcrumapp.com/api/v2/memberships/change_permissions.json <<EOF
{
  "change": {
    "type": "form_members",
    "form_id": "292a22c5-456a-4deb-a7d6-42ac409897b5",
    "add": [ "49ee5c1a-220f-4deb-8a9b-775706f66433" ]
  }
}
EOF
```

### Remove an existing member from a form
```bash
curl -XPOST -H "X-ApiToken: token" -H "Content-Type: application/json" -d@- https://api.fulcrumapp.com/api/v2/memberships/change_permissions.json <<EOF
{
  "change": {
    "type": "form_members",
    "form_id": "292a22c5-456a-4deb-a7d6-42ac409897b5",
    "remove": [ "49ee5c1a-220f-4deb-8a9b-775706f66433" ]
  }
}
EOF
```
