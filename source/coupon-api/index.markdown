---
layout: page
title: "coupon api"
date: 2014-09-30 22:27
comments: true
sharing: true
footer: true
---

The client get the coupon information through the scanable Api. And then redeem the coupon through the following API.



## Create redeem

{% img [class names] /images/My_coupon_merchant_redemption_screen_minimized_gaussian_blur_bg.png 400x200 %}

**This API needs authentication**

`POST /api/v1/redeem`

The redeem accepts following parameters,

- merchant_code:  The merchant code

{% img [class names] /images/My_Coupons_gaussian_blur_bg.png 400x200 %}

## My coupons

**This API needs authentication**

`GET /api/v1/coupons`

This API can accepts parameters,

- type: accepts 1 values: redeemed

for example,

`GET /api/v1/coupons?type=redeemed`

The type recent and expiring just stores at client side when the user scans moodstocks image and don't need to access server?





