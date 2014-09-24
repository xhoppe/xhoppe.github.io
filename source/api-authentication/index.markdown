---
layout: page
title: "api-authentication"
date: 2014-09-21 21:19
comments: true
sharing: true
footer: true
---

Here we describe the API for user authentication.

Here is the URL for user authentication,

`POST api/v1/login`

Expected parameters in the body

- msisdn:  The phone number of the user
- password: The password of the user

### Sample Request

{% codeblock %}
{
  "msisdn": "84932466",
  "password": "12345678"
}
{% endcodeblock %}

### Sample Response

{% codeblock %}
{
  "msisdn": "84932466",
  "authentication_token": "Uzzuxk4yz1_4sCX_hM86",
  "valid_for_authentication": true
}
{% endcodeblock %}

If authentication is successful, the server returns the following attributes,

- msisdn: The phone number of the user
- authentication_token: A token for the following user access
- valid_for_authentication: Usually be true. Will be false if the user is locked.

