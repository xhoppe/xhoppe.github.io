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
  "valid_for_authentication": false
}

{% endcodeblock %}

If register successful, the server returns the following attributes,

- msisdn: The phone number of the user
- valid_for_authentication: Will be false if the user is locked.

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

## Verification code
After the user is registered, a SMS will be send to the user, the user needs to input this verification code to be enabled.

`POST /api/v1/users/verify`

Expected parameters in the body

- code:  The verification code received by the user.

### Sample Request

{% codeblock %}

POST /api/v1/users/verify

{
  "code": "323445"
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

If the verification is successful, it will return the authentication_token to the client.

## Forget password
If the user forgets his password, he could recover by call **Forget password** API to recover. 


`PUT /api/v1/users/forget_password`

Expected parameters in the body

- msisdn:  The phone number of the user

If successful, it returns a 200 response.

## Recover password
TODO: Current mechanism seems not secure.


