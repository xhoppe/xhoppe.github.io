---
layout: page
title: "api-doc"
date: 2014-09-21 20:54
comments: true
sharing: true
footer: true
---

Here we cover the inner working of Xhoppe's RESTful API. It assumes a basic understanding of the principles of REST.

## Version

All APIs are versioned. And the current version is v1.

## API URL

All API access is over HTTPS. And the entry point for all API is under the URL http://test.xhop.pe/api/v1/ . The v1 represents the API version number.

## JSON

The REST API uses JSON format to encode the HTTP request and response. When the client sends a HTTP request to the server, the server expects the body is of JSON format. And when the client receives the HTTP response, if the response has body, it should also parse the body as JSON format.

And when the client sents a HTTP request, it MUST add the following HTTP headers,

{% codeblock %}

CONTENT_TYPE: application/json
ACCEPT: application/json

{% endcodeblock %}

## HTTP Method

The REST API uses different HTTP verbs to differentiate the actions the client wants to perform. 

- GET
- POST
- PUT
- DELETE

## Datetime
All DATETIME and timestamps are returned in ISO 8601 format:  `YYYY-MM-DDTHH:MM:SSZ`

## Authentication

Some API needs authentication. The authentication is first logon through the authentication API. If authentication is successful, it will return an authentication_token to the client. For the API that requires authentication, the client should set the authentication token as a custom HTTP header, the header name is `X-Xhoppe-Token`,for example, 

`X-Xhoppe-Token: Uzzuxk4yz1_4sCX_hM86`

If the authentication is failed, the server will return a 401 Unauthorized HTTP error code.



