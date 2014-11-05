---
layout: page
title: "user info api"
date: 2014-11-05 22:22
comments: true
sharing: true
footer: true
---

Here we describe User Info API

**User Info API needs authentication**

{% img [class names] /images/Enter_user_info.png 400x200 %}

## Get User Info

To get the user info the client should use following API

`GET /user_info`

The server will return information like following,

{% codeblock %}
{
  "username": "Mike Jackson",
  "msisdn": "84932466",
  "email": "aaa@example.com",
  "billing_address": {
    "address1": "12 Ayer Rajah Cresent",
    "address2": "",
    "zipcode": "139943"
  }
  "shipping_address_same_as_billing": true
}
{% endcodeblock %}

or 

{% codeblock %}
{
  "username": "Mike Jackson",
  "msisdn": "84932466",
  "email": "aaa@example.com",
  "billing_address": {
    "address1": "12 Ayer Rajah Cresent",
    "address2": "",
    "zipcode": "139943"
  }
  "shipping_address_same_as_billing": false,
  "shipping_address": {
    "address1": "12 Orchard Road",
    "address2": "",
    "zipcode": "130000"
  }
}
{% endcodeblock %}

## Update User info
To Update User info, the client uses following API,

`PUT /user_info`

The parameter is like following,

{% codeblock %}

PUT /user_info

{
  "username": "Mike Jackson",
  "msisdn": "84932466",
  "email": "aaa@example.com",
  "billing_address": {
    "address1": "12 Ayer Rajah Cresent",
    "address2": "",
    "zipcode": "139943"
  }
  "shipping_address_same_as_billing": false,
  "shipping_address": {
    "address1": "12 Orchard Road",
    "address2": "",
    "zipcode": "130000"
  }
}
{% endcodeblock %}

If successful, the server will return updated infomation same as Get user info.

** The server should not store user's credit card number**


{% img [class names] /images/My_Profile.png 400x200 %}

My profile seems could be unified with user info? (discuss)
