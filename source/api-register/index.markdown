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

{% img [class names] /images/Sign_up_screen.png 400x200 %}

Here is the URL for user registration,

`POST /api/v1/users`

Expected parameters in the body

- msisdn:  The phone number of the user
- username: The username
- password: The password of the user

### Sample Request

{% codeblock %}

POST /api/v1/users


{
  "msisdn": "84932466",
  "username": "Mike Jackson"
  "password": "12345678"
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
- valid_for_authentication: Will be false if the user is locked or if the user is not verified yet.

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

{% img [class names] /images/Sign_up_verification.png 400x200 %}

After the user is registered, a SMS will be send to the user, the user needs to input this verification code to be enabled.

`POST /api/v1/users/verify`

Expected parameters in the body

- code:  The verification code received by the user.

### Sample Request

{% codeblock %}

POST /api/v1/users/verify

{
  "msisdn": "84932466"
  "verification_code": "323445"
}

{% endcodeblock %}

### Sample Response
{% codeblock %}

{
  "msisdn": "84932466",
  "username": "Mike Jackson",
  "authentication_token": "h-jkDAMwJzrsm-4bvgzw",
  "valid_for_authentication": true
}

{% endcodeblock %}

If the verification is successful, it will return the authentication_token to the client.

### Sample error response

{% codeblock %}

{
  "error": "Invalid resource. Please fix errors and try again.",
  "errors": {
    "verification_code": [
      "not valid"
    ]
}
}

{% endcodeblock %}


## Forget password

{% img [class names] /images/Forgot_password.png 400x200 %}

If the user forgets his password, he could recover by call **Forget password** API to recover. 


`PUT /api/v1/users/forget_password`

Expected parameters in the body

- msisdn:  The phone number of the user

If successful, it returns a 200 response.

After calling this API, the server will send a verification code to the user by SMS.

## Recover password

After the user receives the SMS, he should enter the verification code and new password

## Reset password

{% img [class names] /images/reset_password_dialog_box.png 400x200 %}

The user can reset his password by this API.

`PUT /api/v1/users/reset_password`

**This API needs authentication**

Expected parameters in the body
- current_password: the current password
- password: new password

{% codeblock %}

POST /api/v1/users/reset_password

{
  "current_password": "12345678"
  "password": "87654321"
}

{% endcodeblock %}

if the change is successful, the server return response like

{% codeblock %}

{
  "msisdn": "84932466",
  "username": "Mike Jackson",
  "authentication_token": "h-jkDAMwJzrsm-4bvgzw",
  "valid_for_authentication": true
}

{% endcodeblock %}

**After the user updates his password, the authentication_token will be regenerated. So the user need to login again.**




