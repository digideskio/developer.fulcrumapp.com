---
layout: default
section: general
order: 7
title: "Webhooks Overview"
description: "Overview of Using Webhooks In Fulcrum"
category: section
---

## Getting Ready

We assume you've read the general [webhooks overview](http://help.fulcrumapp.com/setting-up-webhooks/webhooks-for-push-notifications), and will build on top of that knowledge here.

## Introduction

Say you are a developer for a large organization that uses Fulcrum for data collection. Coworkers have collected data in the field, but now you need to analyze that data using other tools. To begin, you need a local copy of the data collected with Fulcrum. For this, you download all the existing data using the Fulcrum API. But what about keeping your database up to date as forms, records, and other pieces of data are created, updated, and deleted?

The naive solution is to poll the API every few seconds for changes. Now, however, using webhooks for push notifications is much preferred to polling. To take advantage of webhooks, you set up one or more servers that act as endpoints and register their URLs with Fulcrum. Then, when an event occurs within Fulcrum, we push that event to your webhook endpoints in the form of an HTTP POST request with a JSON payload of the event's data.  Your code only concerns updating your database and only runs when an event occurs.  Your copy of the data will stay up to date with Fulcrum using minimal effort.

This is only a single use case. One could build webhook endpoints for other uses such as integrating with another service, tracking statistics, enabling workflows, managing tasking of employees, or sending out emails - all in real time. Webhooks open a lot of possibilities for integration and additional functionality that extend Fulcrum.

## Authorization

Your [plan must enable webhooks](http://help.fulcrumapp.com/setting-up-webhooks/how-to-enable-webhooks). You must also have an account with the [right role/permission](http://help.fulcrumapp.com/setting-up-webhooks/roles-and-permissions) to manage webhooks.

Now that you know your plan enables webhooks, and you have the right role/permission, you can use your API token to manage webhooks through the API.

**Note:** Accessing the webhooks API without a valid API token will result in a `401 Unauthorized` error. Using an API token without the proper subscription or role/permission will result in a `403 Forbidden` error.

See [Developers Authentication]({{ site.url }}/api/intro/#authentication) on how to use this API token to access the API.

## Manage Webhooks In Fulcrum

Webhooks can be managed programmatically through the [Webhook API]({{ site.url }}/endpoints/webhooks/).

One can also manage webhooks through [the Fulcrum site](http://help.fulcrumapp.com/setting-up-webhooks/getting-started-with-webhooks).

## Choose an Endpoint

Since webhooks are HTTP-based callbacks, our Fulcrum servers make HTTP POST requests to web servers whose URLs you give us. This means you need an endpoint that can receive these POST requests and conform to the responsibilities outlined below.

You can write your own endpoint, or use an existing application.

### Write an Endpoint

It's likely you will want to write your own endpoint to implement functionality custom to your organization. Below is a very simple example of a starter endpoint written in PHP.

```php
<?php
  $input = file_get_contents('php://input'); # POST data from webhook
  $payload = json_decode($input, true);

  // do something with the webhook payload...
  print $payload['type']; // record.create
  print $payload['data']['form_values']['9272']; // 2018-01-19
?>
```

The details you'll need for implementing your own endpoint are discussed below, but can be helpful to use a service such as [RequestBin](https://requestb.in/) for inspecting the actual webhook payloads sent by Fulcrum, while developing your endpoints.

### Use an Existing Endpoint

In the process of adding the webhooks functionality to Fulcrum, we built several applications that are webhook endpoints. You can refer to the list of guides and applications on our [Getting Started](http://help.fulcrumapp.com/setting-up-webhooks/getting-started-with-webhooks) page.

## Details

You may register multiple webhooks for the same organization. This allows for multiple points of integration with your Fulcrum data.

Each webhook event will initiate a separate HTTP POST request to each webhook URL registered within the organization.

Your webhook endpoint is free to do anything with the response, as long as it complies with the Responsibilities section below. Update a database record, send an email, kick off some custom analysis, or anything else you can imagine.

## Events

The following table describes all the events that can be raised and the data that will be sent to your webhook.

{:.table.table-striped}
| Event Type                | Event Data                                   | Examples                                                     |
|---------------------------|----------------------------------------------|--------------------------------------------------------------|
| form.create               | Data of the form just created.               | [Example payload](https://gist.github.com/kyletolle/6597024) |
| form.update               | Data of the form just updated.               | [Example payload](https://gist.github.com/kyletolle/6597137) |
| form.delete               | Data of the form just deleted.               | [Example payload](https://gist.github.com/kyletolle/6597307) |
|---------------------------|----------------------------------------------|--------------------------------------------------------------|
| record.create             | Data of the record just created.             | [Example payload](https://gist.github.com/kyletolle/6597187) |
| record.update             | Data of the record just updated.             | [Example payload](https://gist.github.com/kyletolle/6597221) |
| record.delete             | Data of the record just deleted.             | [Example payload](https://gist.github.com/kyletolle/6597279) |
|---------------------------|----------------------------------------------|--------------------------------------------------------------|
| choice_list.create        | Data of the choice list just created.        | [Example payload](https://gist.github.com/kyletolle/6598016) |
| choice_list.update        | Data of the choice list just updated.        | [Example payload](https://gist.github.com/kyletolle/6598075) |
| choice_list.delete        | Data of the choice list just deleted.        | [Example payload](https://gist.github.com/kyletolle/6598114) |
|---------------------------|----------------------------------------------|--------------------------------------------------------------|
| classification_set.create | Data of the classification set just created. | [Example payload](https://gist.github.com/kyletolle/6597717) |
| classification_set.update | Data of the classification set just updated. | [Example payload](https://gist.github.com/kyletolle/6597806) |
| classification_set.delete | Data of the classification set just deleted. | [Example payload](https://gist.github.com/kyletolle/6597894) |

Notice that each resource type is singular. The event type itself is present tense.

### Data Format

The relevant HTTP headers in the request are:

    Content-Type: application/json

The event data is delivered as a JSON-encoded string in the POST request. This is indicated by the Content-Type header. And after parsing the payload as JSON, it will look something like:

```json
{
  "id":       "<event id>",
  "type":     "<event type>",
  "owner_id": "<id of the owning organization>",
  "data":     "<JSON representation of the resource>"
}
```

See the above example payloads for details of what each event will look like.

### Active/Inactive

A webhook can be either active or inactive. By default, a webhook is active.

An active webhook will receive requests for events as they occur within your Fulcrum organization.

A webhook marked as inactive will stop receiving event requests.  If an event's request is processing while the webhook is made inactive, this request will finish. Any other existing, unprocessed event requests (including retries) will be canceled and not sent to your endpoint.

If an inactive webhook is made active, it will begin receiving event requests once more, as soon as new event requests are created for that organization.

## Responsibilities

Fulcrum's webhooks are responsible for delivering your data to your endpoint. Your endpoint's primary responsibility is to receive this data. We expect an HTTP `200` - `207` response code to indicate successful receipt of the request. Even if you're not interested in this event or data, for whatever reason, we expect a successful response, since you have, indeed, received the data.

Any response outside the `200` - `207` range will be treated as a failure, and the request retried later.

To keep the webhooks as fast as possible, we require a successful response code within 20 seconds of initiating the HTTP POST request to your server. After 20 seconds, the request will time out and be retried again later.

After receiving the data, you can perform further actions on it. Some of these actions may be fast (e.g. inserting the data into a database) while others may be slow (e.g. building a PDF report, or sending an email). Perform all your heavy lifting either on another thread or after you've successfully responded. This will avoid your processing running long enough to cause the request to time out.

## Failures and Retries

It's understood that networks and servers go down. To accommodate that, we've made the webhook event requests fault tolerant.

A request made to a webhook URL that cannot connect, times out, returns a status code outside the `200` - `207` range, or experiences some other error will fail and retried again later.

The frequency of the retries is subject to exponential backoff. Meaning that the more times a request fails, the longer we'll wait until we retry again. In the worst case, we'll retry the request a maximum of 25 times over roughly 20 days. After this point it will be assumed that the server will never respond and the request will not be retried again.

## Security

There are no features built into Fulcrum's webhooks to verify the content or validate the authenticity of the event payload being delivered.

For example, it could be possible, for some 3rd party to know of your webhook endpoint's URL and forge a payload. Or there may be other considerations that make you wary of trusting data given to you.

If this level of security is needed, it seems practical that one use the webhook as an alert. You can then initiate a request to the Fulcrum API to retrieve the relevant data. Once you've securely initiated the request on your own, with your credentials, to the endpoint you know is the Fulcrum service, you can be confident the data is valid and authentic.

## Notes

The organization which owns the resource in the payload is designated by the `owner_id` attribute in the event data.  This is useful in the case of a service that provides multiple clients a service built on top of Fulcrum. This single webhook can be used to receive event payloads for multiple organizations and still determine to which organization the event belongs.
