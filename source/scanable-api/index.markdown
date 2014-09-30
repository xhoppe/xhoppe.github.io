---
layout: page
title: "scanable api"
date: 2014-09-30 22:16
comments: true
sharing: true
footer: true
---

When the user scans an image, it could return either a product or a coupon. Either a product or a coupon is called a scanable object.


The user uses the following API to get a scanable

`GET /api/v1/scanable/#{moodstocks_image_id}/by_moodstocks_image`

If the result is a product, it could return response like,

{% codeblock lang:json %}
{
  id: 2,
  type: "product",
  sku: "",
  description: "",
  price: "$668",
  price_amount: 668,
  has_variants: true,
  images: [
    {
      image_url: "/uploads/image/image/4/iphone6-gold-early-release.jpg",
      image_medium_url: "/uploads/image/image/4/medium_iphone6-gold-early-release.jpg",
      image_thumb_url: "/uploads/image/image/4/thumb_iphone6-gold-early-release.jpg"
    }
  ],

  product_properties: [
    {
      display_name: null,
      value: "4.7 inch"
    }
  ],

  variants: [
    {
      id: 4,
      options_text: "Color: Black",
      price: "$668",
      price_amount: 668
    },
    {
      id: 5,
      options_text: "Color: White",
      price: "$668",
      price_amount: 668
    },
    {
      id: 6,
      options_text: "Color: Gold",
      price: "$668",
      price_amount: 668
    }
  ]
}

{% endcodeblock %}


In the above response, there is a type field whose value is **product**. If it's a coupon, then the type value will be coupon.

