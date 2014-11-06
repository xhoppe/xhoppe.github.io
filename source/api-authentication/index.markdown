---
layout: page
title: "api-authentication"
date: 2014-09-21 21:19
comments: true
sharing: true
footer: true
---

Here we describe the API for user authentication.

{% img [class names] /images/Log_in_screen.png 400x200 %}

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
  "username": "Mike Jackson"
  "authentication_token": "Uzzuxk4yz1_4sCX_hM86",
  "valid_for_authentication": true
}
{% endcodeblock %}

If authentication is successful, the server returns the following attributes,

- msisdn: The phone number of the user
- username: the username
- authentication_token: A token for the following user access
- valid_for_authentication: Usually be true. Will be false if the user is locked or unverified.

## Facebook authentication
When the user authenticate with facebook, the client will authenticate with Facebook client and send a facebook token
to the server like following,

`POST /api/v1/facebook/login`

{% codeblock %}

POST /api/v1/facebook/login

{
  token: '23497joseu234'
}


{% endcodeblock %}

The token is facebook token, then the server will check with facebook if the token is correct or not, if yes, then it will
create a user in database if the user doesn't exist.

Then it returns same information as normal authentication,

{% codeblock %}
{
  "msisdn": "",
  "username": "Mike Jackson"
  "authentication_token": "Uzzuxk4yz1_4sCX_hM86",
  "valid_for_authentication": true
}
{% endcodeblock %}


**When the user authenticate with facebook, he doesn't necessarily input a mobile number, so the msisdn may be empty**

