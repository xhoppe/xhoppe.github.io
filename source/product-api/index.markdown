---
layout: page
title: "product api"
date: 2014-09-28 19:08
comments: true
sharing: true
footer: true
---

Here we describe the API for products. The mobile client get the products through the Moodstocks Images.

Firstly we need to create moodstocks images for products. Like the following figure,

{% img /images/moodstocks_imgs.png  %}

When you create a moodstocks image, the server will upload this image to moodstocks server and add it to offline. Then the client could call moodstocks API to match an image, if it finds the match, it could get the moodstocks image id. And then it calls the following API to get product information,

`GET /api/v1/products/#{moodstocks_image_id}/by_moodstocks_image`

replace the #{moodstocks_image_id} by the real moodstocks image id. For example, if the image id is 123, then the URL will be,

`GET /api/v1/products/123/by_moodstocks_image`

A sample response is like following,

{% codeblock lang:json %}
{
  id: 2,
  sku: "",
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


## Filter options
When 