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

`GET /api/v1/scannable/#{moodstocks_image_id}/by_moodstocks_image`

This method either returns a coupon or a product. In the response there is a **type** attribute, which tells you it's a coupon or product

If the result is a coupon, it could return response like,

{% codeblock lang:json %}
{
  id: 29,
  name: "test2",
  type: "Coupon",
  description: "test2",
  images: [
    {
    url: "/uploads/image/image/63/20140424085015281.jpg",
    medium_url: "/uploads/image/image/63/medium_20140424085015281.jpg",
    thumb_url: "/uploads/image/image/63/thumb_20140424085015281.jpg"
    }
  ]
}

{% endcodeblock %}

If the result is a product, it could return response like

{% codeblock lang:json %}

{
  id: 2,
  sku: "",
  type: "Product",
  description: "",
  display_price: "$100",
  price: 100,
  price_cents: 10000,
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


The client should use the **type** attribute in response to react differently.

