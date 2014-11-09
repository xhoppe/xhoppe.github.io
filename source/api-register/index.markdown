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

- sms_confirmation_token:  The verification code received by the user.

### Sample Request

{% codeblock %}

POST /api/v1/users/verify

{
  "sms_confirmation_token": "abcd"
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

The SMS confirmation token is case insensitive.

### Sample error response

If the SMS confirmation token is invalid

{% codeblock %}

{
  "error": "Invalid resource. Please fix errors and try again.",
  "errors": {
    "sms_confirmation_token": [
      "is invalid"
    ]
}
}

{% endcodeblock %}

If the SMS confirmation token is expired

{% codeblock %}

{
  "error": "Invalid resource. Please fix errors and try again.",
  "errors": {
    "sms_confirmation_token": [
      "is expired"
    ]
}
}

{% endcodeblock %}

After the SMS is sent, the token is valid for 10 minutes.

## Resend Verify
If the SMS is sent but the user didn't confirm. He could invoke 'Resend Verify' to send another SMS.

{% codeblock %}

POST /api/v1/users/resend_verify

{
  "msisdn": "+6584932466"
}

{% endcodeblock %}

The response should be

{% codeblock %}

{
  "msisdn": "84932466",
  "username": "Mike Jackson",
  "authentication_token": "h-jkDAMwJzrsm-4bvgzw",
  "valid_for_authentication": false
}

{% endcodeblock %}

## Forget password

{% img [class names] /images/Forgot_password.png 400x200 %}

If the user forgets his password, he could recover by first call **Resend Verify** API to send a SMS to user.

After the user receives the SMS, he should enter the SMS in a dialog, and the client calls `check_sms_token` API to check if the token is good or not

{% codeblock %}

POST /api/v1/users/check_sms_token

{
  "sms_confirmation_token": "abcd"
}

{% endcodeblock %}

if the token is correct, he calls the `Recover Password` to update his password

if the token is correct, the response is as following,

{% codeblock %}

{
  "msisdn": "84932466",
  "username": "Mike Jackson",
  "authentication_token": "h-jkDAMwJzrsm-4bvgzw",
  "valid_for_authentication": true
}

{% endcodeblock %}

If the SMS confirmation token is invalid

{% codeblock %}

{
  "error": "Invalid resource. Please fix errors and try again.",
  "errors": {
    "sms_confirmation_token": [
      "is invalid"
    ]
}
}

{% endcodeblock %}

If the SMS confirmation token is expired

{% codeblock %}

{
  "error": "Invalid resource. Please fix errors and try again.",
  "errors": {
    "sms_confirmation_token": [
      "is expired"
    ]
}
}

{% endcodeblock %}

## Recover password

After the user receives the SMS, he should enter the verification code and new password

{% codeblock %}

POST /api/v1/users/change_password_by_sms

{
  "sms_confirmation_token": "abcd",
  "password": "12345678"
}

{% endcodeblock %}

If the update is successful, it returns the user information same as above.

## Reset password

{% img [class names] /images/reset_password_dialog_box.png 400x200 %}

The user can reset his password by this API.

`PUT /api/v1/users/change_password`

**This API needs authentication**

Expected parameters in the body
- current_password: the current password
- password: new password

{% codeblock %}

POST /api/v1/users/change_password

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


## Facebook signup

When the user authenticate with facebook, the client will authenticate with Facebook client and send a facebook token
to the server like following,

{% codeblock %}

POST /api/v1/facebook/signup

{
  "token": "12sdfs23423"
}

{% endcodeblock %}

After the server receives the token, it will get the user information from facebook, and then returns the client following
information as response


{% codeblock %}

{
  "username": "Mike Jackson",
  "male": true,
  "birthday": '1984-03-20',
  "msisdn": "84932466",
  "email": "aaa@example.com",
  "facebook_id": "2342lkjdfsf"
}
{% endcodeblock %}

The facebook_id is very important, and then the client let the user update information and posts to the server,

{% codeblock %}

POST /api/v1/users

{
  "msisdn": "84932466",
  "username": "Mike Jackson"
  "password": "12345678"
  "facebook_id": "2342lkjdfsf"

  "male": true,
  "birthday": '1984-03-20',
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


The **facebook_id** must be passed back to the server.


