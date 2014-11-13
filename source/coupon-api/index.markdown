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

### Get my Coupons


`GET /api/v1/user_coupons`

without parameters, it get all coupons user added

This API can accepts parameters,

- type: accepts 1 values: expiring, redeemed

for example,

`GET /api/v1/coupons?type=redeemed`
`GET /api/v1/coupons?type=expiring`

返回的结果是分页的，每页最多25条记录, 默认是返回第一页（page=0 是第一页），用户可以用page参数来返回别的页，比如返回第二页，

`GET /api/v1/coupons?type=redeemed&page=1`

返回结果如下, 下面结果中total_pages为1，说明只有一页， coupons返回结果，在每个coupon里面如果用户已经加了这个coupon，added_by_user为true，如果用户已经redeem了这个coupon，redeemed_by_user返回true

{% codeblock lang:json %}

{
 "total_pages"=>1,
 "coupons"=>
  [
    {"id"=>23,
      "sku"=>nil,
      "name"=>"Discount 10% for TShirt",
      "type"=>"Coupon",
      "start_date"=>"2014-11-12",
      "end_date"=>"2014-11-16",
      "description"=>"Discount 10% for TShirt for storewide",
      "date_valid"=>true,
      "added_by_user"=>true,
      "redeemed_by_user"=>false,
      "images"=>[]},
     {"id"=>22,
      "sku"=>nil,
      "name"=>"Discount 10% for TShirt",
      "type"=>"Coupon",
      "start_date"=>"2014-11-13",
      "end_date"=>"2014-11-21",
      "description"=>"Discount 10% for TShirt for storewide",
      "date_valid"=>true,
      "added_by_user"=>true,
      "redeemed_by_user"=>false,
      "images"=>[]},
     {"id"=>21,
      "sku"=>nil,
      "name"=>"Discount 10% for TShirt",
      "type"=>"Coupon",
      "start_date"=>"2014-11-13",
      "end_date"=>"2014-11-19",
      "description"=>"Discount 10% for TShirt for storewide",
      "date_valid"=>true,
      "added_by_user"=>true,
      "redeemed_by_user"=>false,
      "images"=>[]}
    ]
  }

{% endcodeblock %}

### Add coupon

用户用下面api添加coupon

{% codeblock lang:json %}

`POST /api/v1/user_coupons`

{ coupon_id: 1234 }

{% endcodeblock %}

coupon_id 为coupon id，如果添加成功，response的status为201（created），body为空

### Delete Coupon

用户用下面api添加coupon

{% codeblock lang:json %}

`DELETE /api/v1/user_coupons/1,2,3`

{% endcodeblock %}

1,2,3为coupon id用`,`隔开

如果删除成功，response的status为200（created），body为空



