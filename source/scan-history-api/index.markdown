---
layout: page
title: "scan_history_api"
date: 2014-11-13 22:54
comments: true
sharing: true
footer: true
---

The user uses following API to get scan histories

`GET api/v1/scan_histories`

scan history也分页，默认为第一页，用户可以带page参数来制定返回第几页。（page=0 为第一页）

返回结果如下，

{% codeblock lang:json %}

{"total_pages"=>1,
 "scan_histories"=>
  [
   {
    "moodstocks_id"=>1,
    "resouce_type"=>"Product",
    "created_at"=>"2014-11-13T12:13:01Z",
    "scannable"=>
     {"id"=>3,
      "sku"=>nil,
      "name"=>"Rustic Concrete Shirt",
      "type"=>"Product",
      "description"=>
       "Et rem magnam odio. Ut voluptatem corporis delectus doloribus. Quo id sit laboriosam dolorem suscipit. Vitae porro ut aut quasi ducimus.",
      "sold_by"=>"Pacocha-Konopelski",
      "display_price"=>"$0",
      "price"=>0.0,
      "price_cents"=>0,
      "has_variants"=>false,
      "available_options"=>[],
      "images"=>[],
      "product_properties"=>[],
      "master"=>{"id"=>1, "stocks_count"=>0}}},

   {"moodstocks_id"=>2,
    "resouce_type"=>"Coupon",
    "created_at"=>"2014-11-13T12:13:01Z",
    "scannable"=>
     {"id"=>2,
      "sku"=>nil,
      "name"=>"Discount 10% for TShirt",
      "type"=>"Coupon",
      "start_date"=>"2014-11-13",
      "end_date"=>"2014-11-21",
      "description"=>"Discount 10% for TShirt for storewide",
      "date_valid"=>true,
      "added_by_user"=>false,
      "redeemed_by_user"=>false,
      "images"=>[]}},

   {"moodstocks_id"=>3,
    "resouce_type"=>"Coupon",
    "created_at"=>"2014-11-13T12:13:01Z",
    "scannable"=>
     {"id"=>1,
      "sku"=>nil,
      "name"=>"Discount 10% for TShirt",
      "type"=>"Coupon",
      "start_date"=>"2014-11-13",
      "end_date"=>"2014-11-19",
      "description"=>"Discount 10% for TShirt for storewide",
      "date_valid"=>true,
      "added_by_user"=>false,
      "redeemed_by_user"=>false,
      "images"=>[]
      }
    }
  ]
}
{% endcodeblock %}

在scan_histories中，moodstocks_id为用户扫描时的id，scannable要么是product，要么是coupon

