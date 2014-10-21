---
layout: page
title: "checkout api"
date: 2014-09-30 21:40
comments: true
sharing: true
footer: true
---

Here we describe the checkout API. All Order API requires authentication.

An order is a list of line items. Each line item has a variant and a quantity. For example, one order could have 2 line items: 1 iphone 6 and 2 LG G3.

When the user adds a product into the shopping cart, the cart is stored at the client side. And then the user clicks the **Checkout** button, here is to create an order.

To checkout involves 2 steps, according to previous discussion: 

1. Create an order and add line items into it and set the delivery address, this step will calculate the shipment fee
2. Pay the order

## Create an order

To create an empty order

`POST /api/v1/orders.json`

The body could be the line items like this,

{% codeblock lang:json %}

{  
  "line_items": 
    [
      { "variant_id": 33,
        "quantity": 2
      },
      { "variant_id": 34,
        "quantity": 3
      }
    ],
  "ship_address": 
    { "firstname": "Noemi",
      "lastname": "Ullrich",
      "address1": "2315 Molly Hill",
      "address2": "Suite 329",
      "zipcode": "47546",
      "phone": "(684) 555-8450"
    }
}

{% endcodeblock %}

In line_items the **variant_id** and **quantity** are mandatory, in the ship_address, firstname, lastname, address1, zipcode and phone are mandatory.

The expected response is as following,

{% codeblock lang:json %}

{
 "id"=>8,
 "state"=>"payment",
 "all_paid"=>false,
 "display_item_total"=>"$400",
 "item_total"=>400.0,
 "item_total_cents"=>40000,
 "display_ship_total"=>"$0",
 "ship_total"=>0.0,
 "ship_total_cents"=>0,
 "display_total"=>"$400",
 "total"=>400.0,
 "total_cents"=>40000,
 "ship_address"=>
  {
   "firstname"=>"Noemi",
   "lastname"=>"Ullrich",
   "full_name"=>"Noemi Ullrich",
   "address1"=>"2315 Molly Hill",
   "address2"=>"Suite 329",
   "zipcode"=>"47546",
   "phone"=>"(684) 555-8450"
  },

 "line_items"=>
  [
    {
      "quantity"=>2,
      "variant"=>
     {"id"=>33,
      "options_text"=>"",
      "display_price"=>"$50",
      "price"=>50.0,
      "price_cents"=>5000,
      "images"=>
       [{"url"=>"/uploads/image/image/19/gshock.jpg",
         "medium_url"=>"/uploads/image/image/19/medium_gshock.jpg",
         "thumb_url"=>"/uploads/image/image/19/thumb_gshock.jpg"}]}
    },

   {
    "quantity"=>3,
    "variant"=>
     {"id"=>34,
      "options_text"=>"",
      "display_price"=>"$100",
      "price"=>100.0,
      "price_cents"=>10000,
      "images"=>
       [{"url"=>"/uploads/image/image/20/gshock.jpg",
         "medium_url"=>"/uploads/image/image/20/medium_gshock.jpg",
         "thumb_url"=>"/uploads/image/image/20/thumb_gshock.jpg"}]}
    }]
 }

{% endcodeblock %}

Pay attention to the **state** in above response, if it's successful, the order's state becomes **payment**, which means the order expects a payment


## Payment

I checked the paypal msdk, if we use this, the payment is done between the mobile and paypal server, and paypal server returns the client a transaction id, the mobile sends this id to the server, and the server will do the validation to the paypal server.

After the mobile finishes the payment, it post the paypal reference id to the server,

`POST /api/orders/#{order.id}/payments`

The #{order.id} is the order ID which involves the payment.

The body is like following,

{% codeblock lang:json %}

{
  "reference_id":"123456789"
}

{% endcodeblock %}

The reference_id is the ID received from the paypal server. And Xhoppe server will validates this id and get the amount paid from the paypal. If it's successful, it will return a response like this

{% codeblock lang:json %}

{"id"=>1,
 "state"=>"completed",
 "all_paid"=>true,
 "display_item_total"=>"$400",
 "item_total"=>400.0,
 "item_total_cents"=>40000,
 "display_ship_total"=>"$0",
 "ship_total"=>0.0,
 "ship_total_cents"=>0,
 "display_total"=>"$400",
 "total"=>400.0,
 "total_cents"=>40000,
 "ship_address"=>
  {"firstname"=>"Brice",
   "lastname"=>"Larson",
   "full_name"=>"Brice Larson",
   "address1"=>"1509 Abdiel Views",
   "address2"=>"Apt. 656",
   "zipcode"=>"69331",
   "phone"=>"696.649.5404 x13459"},
 "line_items"=>
  [{"quantity"=>2,
    "variant"=>
     {"id"=>3,
      "options_text"=>"",
      "display_price"=>"$50",
      "price"=>50.0,
      "price_cents"=>5000,
      "images"=>
       [{"url"=>"/uploads/image/image/1/gshock.jpg",
         "medium_url"=>"/uploads/image/image/1/medium_gshock.jpg",
         "thumb_url"=>"/uploads/image/image/1/thumb_gshock.jpg"}]}},
   {"quantity"=>3,
    "variant"=>
     {"id"=>4,
      "options_text"=>"",
      "display_price"=>"$100",
      "price"=>100.0,
      "price_cents"=>10000,
      "images"=>
       [{"url"=>"/uploads/image/image/2/gshock.jpg",
         "medium_url"=>"/uploads/image/image/2/medium_gshock.jpg",
         "thumb_url"=>"/uploads/image/image/2/thumb_gshock.jpg"}]}}]
}

{% endcodeblock %}

It returns the order json, notice the **state** is completed, and the **all_paid** is true, it means the payment is done and the order's state is completed, which means the merchant could process the order now.

## My orders
To get the orders of a user, 

`GET /api/orders.json`

This method will return all orders of a user.

