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
  "remarks": 'this is a remarks',

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

**2014-12-21更新，LineItems中的variant加入了merchant信息**

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
 "remarks" => 'this is a remarks',
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
      "merchant"=>
        {"name"=>"Considine-Ryan", 
          "avatar"=>
          {
          "url"=>"/uploads/image/image/19/gshock.jpg",
         "medium_url"=>"/uploads/image/image/19/medium_gshock.jpg",
         "thumb_url"=>"/uploads/image/image/19/thumb_gshock.jpg"
         }
        },
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
 "remarks" => 'this is a remarks',
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

This method will return all orders of a user. The result is ordered by update date

The result is paginated. The user can supply a parameter `state` to filter orders by state,

- state: can be `created`, `payment`, `completed`

For example, get orders that need payment,

`GET /api/orders?state=payment`

**2014-12-16 更新**

- type: 可以是completed或者not_completed

completed是已经付款的order，状态包括completed, partial_delivered, delivered
not_completed是没有付款或者付款数量不够order总数的订单，状态包括created, payment

`GET /api/orders?type=completed` 返回所有已经成功付款的订单

`GET /api/orders?type=not_completed` 返回所有已经还没有成功付款的订单


如果不带任何参数，将返回用户的所有订单，包括已付款和未付款订单

It will return following contents,

{% codeblock lang:json %}

{"total_pages"=>1,
 "orders"=>
  [{"id"=>40,
    "state"=>"payment",
    "all_paid"=>false,
    "display_item_total"=>"$36",
    "item_total"=>36.0,
    "item_total_cents"=>3600,
    "display_ship_total"=>"$10",
    "ship_total"=>10.0,
    "ship_total_cents"=>1000,
    "display_total"=>"$46",
    "total"=>46.0,
    "total_cents"=>4600,
    "ship_address"=>
     {"firstname"=>"Urban",
      "lastname"=>"VonRueden",
      "full_name"=>"Urban VonRueden",
      "address1"=>"939 Koelpin Curve",
      "address2"=>"Apt. 437",
      "zipcode"=>"19661",
      "phone"=>"603.676.5452 x17586"},
    "line_items"=>
     [{"quantity"=>2,
       "variant"=>
        {"id"=>251,
         "options_text"=>"",
         "stocks_count"=>8,
         "display_price"=>"$10",
         "price"=>10.0,
         "price_cents"=>1000,
         "images"=>
          [{"url"=>"/uploads/image/image/5/gshock.jpg",
            "medium_url"=>"/uploads/image/image/5/medium_gshock.jpg",
            "thumb_url"=>"/uploads/image/image/5/thumb_gshock.jpg"}]}},
      {"quantity"=>2,
       "variant"=>
        {"id"=>252,
         "options_text"=>"",
         "stocks_count"=>8,
         "display_price"=>"$8",
         "price"=>8.0,
         "price_cents"=>800,
         "images"=>
          [{"url"=>"/uploads/image/image/6/gshock.jpg",
            "medium_url"=>"/uploads/image/image/6/medium_gshock.jpg",
            "thumb_url"=>"/uploads/image/image/6/thumb_gshock.jpg"}]}}],
    "shipping_fee_items"=>
     [{"merchant"=>"Fadel and Sons", "display_shipping_fee"=>"$5", "shipping_fee"=>5.0, "shipping_fee_cents"=>500},
      {"merchant"=>"Marquardt-Murray", "display_shipping_fee"=>"$5", "shipping_fee"=>5.0, "shipping_fee_cents"=>500}]}]}

{% endcodeblock %}

注意，客户端应该检查total_pages，如果为0，orders属性将为空

## 库存的处理
目前的库存将采用下面的方式，当用户创建一个order时，如果库存不足，服务端会返回下面的错误,

{% codeblock lang:json %}

{
  "errors" : {
    "variants" : ["Can't fulfill stock of Product XXX Variant Size:L"]
  }
}

{% endcodeblock %}


当一个order创建后，该库存会保留，比如一个variant开始的库存是10， 如果用户下单买了3个，那么variant
的库存变为7，如果5小时内用户不付款的话该order将删除，订单删除的时候库存会复原为10个。

