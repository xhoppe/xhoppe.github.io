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

`GET /api/v1/user_info`

The server will return information like following,

{% codeblock %}
{
  "msisdn"=>"100001",
   "username"=>"Mary Jason",
   "authentication_token"=>"fciVr7iis9ZvJvSoCKxP",
   "gender"=>"female",
   "email"=>"aaa@example.com",
   "ship_address_same_as_billing_address"=>true,
   "valid_for_authentication"=>true,
   "ship_address"=>
    {
      "firstname"=>"Mary",
     "lastname"=>"Ann",
     "full_name"=>"Mary Ann",
     "address1"=>"12 Ayer rajah cresent",
     "address2"=>nil,
     "zipcode"=>"139941",
     "phone"=>"98723734"
    },
   "billing_address"=>nil,
   "avatar"=>
    {
      "url"=>nil, 
      "medium_url"=>nil, 
      "thumb_url"=>nil
   }
}
{% endcodeblock %}

or

{% codeblock %}
{
  "msisdn"=>"100005",
 "username"=>"Mary Ann",
 "authentication_token"=>"ynzWsj3dRaw31q34NBG8",
 "gender"=>"female",
 "email"=>"aaa@example.com",
 "ship_address_same_as_billing_address"=>false,
 "valid_for_authentication"=>true,

 "ship_address"=>
  {"firstname"=>"Mary",
   "lastname"=>"Ann",
   "full_name"=>"Mary Ann",
   "address1"=>"12 Ayer rajah cresent",
   "address2"=>nil,
   "zipcode"=>"139941",
   "phone"=>"98723734"},

 "billing_address"=>
  {
    "firstname"=>"AAA",
   "lastname"=>"BBB",
   "full_name"=>"AAA BBB",
   "address1"=>"12 Ayer rajah cresent",
   "address2"=>nil,
   "zipcode"=>"139941",
   "phone"=>"98723734"},

  "avatar"=>{"url"=>nil, "medium_url"=>nil, "thumb_url"=>nil}}

{% endcodeblock %}

## Update User info
To Update User info, the client uses following API,

`PUT /user_info`

The parameter is like following,

{% codeblock %}

PUT /api/v1/user_info

{
   :email=>"aaa@example.com",
   :gender=>"female",
   :username=>"Mary Jason",
   :ship_address_same_as_billing_address=>false,

   :ship_address=> {
    :firstname=>"Mary", 
    :lastname=>"Jason", 
    :address1=>"12 Ayer rajah cresent",
    :address2=>"Level 5",  
    :zipcode=>"139941",
    :phone=>"98723734"
   },

   :billing_address=>{
    :firstname=>"AAA", 
    :lastname=>"BBB", 
    :address1=>"12 Ayer rajah cresent", 
    :zipcode=>"139941", 
    :phone=>"98723734"
  }
}
{% endcodeblock %}

If successful, the server will return updated infomation same as Get user info.

** The server should not store user's credit card number**

当输入地址的时候 firstname, lastname, address1, zipcode, phone是必填项


{% img [class names] /images/My_Profile.png 400x200 %}

## Avatar

update avatar 下面的api

{% codeblock %}

PUT /api/v1/avatar

{
   :avatar => 'lsjfsljfsldfj02348029384234......'
}
{% endcodeblock %}

客户端的请求应该设为multipart，avatar参数为上传的图片文件
