---
layout: page
title: "checkout api"
date: 2014-09-30 21:40
comments: true
sharing: true
footer: true
---

Here we describe the checkout API.

An order is a list of line items. Each line item has a variant and a quantity. For example, one order could have 2 line items: 1 iphone 6 and 2 LG G3.

When the user adds a product into the shopping cart, the cart is stored at the client side. And then the user clicks the **Checkout** button, here is to create an order.

To checkout involves 3 steps:

1. Create an order and add line items into it
2. Set the delivery address, this step will calculate the shipment fee
3. Pay the order

## Create an order

To create an empty order

`POST /api/orders.json`

The body could be the line items like this,

{% codeblock lang:json %}

{
  "line_items": [ {
    "variant_id": 1,
    "quantity": 5
  },  {
    "variant_id": 2,
    "quantity": 3
  }
  ]
}

{% endcodeblock %}

The expected response is as following,

{% codeblock lang:json %}

{
  "id": 4,
  "number": "R307128032",
  "item_total": "0.0",
  "total": "0.0",
  "ship_total": "0.0",
  "state": "cart",
  "adjustment_total": "0.0",
  "user_id": 1,
  "created_at": "2014-07-06T18:52:33.724Z",
  "updated_at": "2014-07-06T18:52:33.752Z",
  "completed_at": null,
  "payment_total": "0.0",
  "shipment_state": null,
  "payment_state": null,
  "display_item_total": "$0.00",
  "display_total": "$0.00",
  "display_ship_total": "$0.00",
  "display_tax_total": "$0.00",
  "bill_address": null,
  "ship_address": null,
  "line_items": [],
  "payments": []
}

{% endcodeblock %}

## Update delivery address

To update the delivery address, use the following

`POST /api/orders/#{id}.json`

The #{id} is the order id.

An expected request is,

{% codeblock lang:json %}

{
  "delivery_address": {
    "firstname": "John",
    "lastname": "Doe",
    "address1": "7735 Old Georgetown Road",
    "phone": "3014445002",
    "zipcode": "139941"
  }
}

{% endcodeblock %}


## Payment

The payment needs to be discussed

TBD.


## My orders
To get the orders of a user,

`GET /api/orders.json`

This method will return all orders of a user.


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

