---
layout: page
title: "api-register"
date: 2014-09-21 21:04
comments: true
sharing: true
footer: true
---

Here we describe the API for user registration. The registration enables a user to register an account in Xhoppe.

## User registration

Here is the URL for user registration,

`POST /api/v1/users`

Expected parameters in the body

- msisdn:  The phone number of the user
- password: The password of the user
- password_confirmation: The password confirmation, should be same as user

### Sample Request

{% codeblock %}

POST /api/v1/users


{
  "msisdn": "84932466",
  "password": "12345678",
  "password_confirmation": "12345678"
}

{% endcodeblock %}

### Sample Response
{% codeblock %}

{
  "msisdn": "84932466",
  "authentication_token": "h-jkDAMwJzrsm-4bvgzw",
  "valid_for_authentication": true
}

{% endcodeblock %}

If register successful, the server returns the following attributes,

- msisdn: The phone number of the user
- authentication_token: A token for the following user access
- valid_for_authentication: Usually be true. Will be false if the user is locked.

### Sample error response

If the register is failed, the server will return an error response, for example, if the email is already taken,

{% codeblock %}

{
  "error": "Invalid resource. Please fix errors and try again.",
  "errors": {
    "msisdn": [
      "has already been taken"
    ]
  }
}

{% endcodeblock %}
