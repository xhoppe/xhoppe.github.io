---
layout: page
title: "redeem api"
date: 2014-10-21 18:16
comments: true
sharing: true
footer: true
---

Here we describe the redeem API. All Redeem API requires authentication.

When create a redeem, the url is 

`POST /api/v1/redeems.json`

The body could be the line items like this,

{% codeblock lang:json %}

{  
  "coupon_id": 1,
  "redeem_code": '1234'
}

{% endcodeblock %}

There are two parameters,

* coupon_id: This is the coupon ID
* redeem_code: Each redeem is associated to a store, so you must create at least a store for the merchant of the coupon, and when create the store, you must assign a redeem_code to it. So in such case we can know this redeem is occured at which store.

A sample response is as following

{% codeblock lang:json %}

{
 "id": 3,
 "store": {
    "id": 3, 
    "name": "Gorgeous Granite Table", 
    "address": nil, 
    "zipcode": nil, 
    "phone": nil
  },

 "coupon": {
  "id": 3, 
  "name": "Discount 10% for TShirt", 
  "type": "Coupon", 
  "description"=>"Discount 10% for TShirt for storewide", 
  "images"=>[]
 }
}

{% endcodeblock %}

It returns the information of the coupon and store.

