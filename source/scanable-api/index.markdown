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


## variants的select

product增加了两个elements: available_options 和 filtered_variants.

比如我们有一个product，他有两个option types：

* Size: S, L
* Color: Red, Green, White

他有四个variant，

* Size: S, Color: Red
* Size: L, Color: Red
* Size: S, Color: Green
* Size: L, Color: Green

如果访问scannable不带参数的话，访问这个product会返回如下的json

{% codeblock lang:json %}

{"id"=>1,
 "sku"=>nil,
 "name"=>"Fantastic Wooden Hat",
 "type"=>"Product",
 "description"=>
  "Quaerat magni odio quas illum culpa commodi. Voluptates necessitatibus eius soluta voluptatem porro aut. Soluta id sit aut quis. Velit est odit similique nostrum tempore ducimus deleniti.",
 "display_price"=>"$0",
 "price"=>0.0,
 "price_cents"=>0,
 "has_variants"=>true,


 "available_options"=>
  [{"id"=>1, "presentation"=>"Size", "available_values"=>[{"id"=>1, "presentation"=>"S"}, {"id"=>2, "presentation"=>"L"}]},
   {"id"=>2, "presentation"=>"Color", "available_values"=>[{"id"=>3, "presentation"=>"Red"}, {"id"=>4, "presentation"=>"Green"}]}],


 "images"=>[],
 "product_properties"=>[],
 "variants"=>
  [{"id"=>2, "options_text"=>"Size: L, Color: Green", "display_price"=>"$0", "price"=>0.0, "price_cents"=>0},
   {"id"=>3, "options_text"=>"Size: S, Color: Green", "display_price"=>"$0", "price"=>0.0, "price_cents"=>0},
   {"id"=>4, "options_text"=>"Size: S, Color: Red", "display_price"=>"$0", "price"=>0.0, "price_cents"=>0},
   {"id"=>5, "options_text"=>"Size: L, Color: Red", "display_price"=>"$0", "price"=>0.0, "price_cents"=>0}],


 "filtered_variants"=>
  [{"id"=>2, "options_text"=>"Size: L, Color: Green", "display_price"=>"$0", "price"=>0.0, "price_cents"=>0},
   {"id"=>3, "options_text"=>"Size: S, Color: Green", "display_price"=>"$0", "price"=>0.0, "price_cents"=>0},
   {"id"=>4, "options_text"=>"Size: S, Color: Red", "display_price"=>"$0", "price"=>0.0, "price_cents"=>0},
   {"id"=>5, "options_text"=>"Size: L, Color: Red", "display_price"=>"$0", "price"=>0.0, "price_cents"=>0}]}

{% endcodeblock %}

当不带任何参数式， filtered_variants 和 variants的内容一样，available_options返回variants里面用到的option_types和对应的option_values.

根据available_options, 客户端可以创建一个类似如下的界面，

{% img [class names] /images/select_variant1.png %}

当用户选择Size为L的时候，客户端向服务端重新发送同样的请求，但是加上参数 `?Size=L`, 这时服务端会返回，


{% codeblock lang:json %}

{"id"=>7,
 "sku"=>nil,
 "name"=>"Fantastic Wooden Pants",
 "type"=>"Product",
 "description"=>
  "Tenetur cum autem est. Blanditiis sequi aut distinctio qui repellendus quibusdam est. Nulla aliquam qui corporis inventore.",
 "display_price"=>"$0",
 "price"=>0.0,
 "price_cents"=>0,
 "has_variants"=>true,


 "available_options"=>
  [{"id"=>13, "presentation"=>"Size", "available_values"=>[{"id"=>32, "presentation"=>"L"}]},
   {"id"=>14, "presentation"=>"Color", "available_values"=>[{"id"=>33, "presentation"=>"Red"}, {"id"=>34, "presentation"=>"Green"}]}],


 "images"=>[],
 "product_properties"=>[],
 "variants"=>
  [{"id"=>32, "options_text"=>"Size: L, Color: Green", "display_price"=>"$0", "price"=>0.0, "price_cents"=>0},
   {"id"=>33, "options_text"=>"Size: S, Color: Green", "display_price"=>"$0", "price"=>0.0, "price_cents"=>0},
   {"id"=>34, "options_text"=>"Size: S, Color: Red", "display_price"=>"$0", "price"=>0.0, "price_cents"=>0},
   {"id"=>35, "options_text"=>"Size: L, Color: Red", "display_price"=>"$0", "price"=>0.0, "price_cents"=>0}],


 "filtered_variants"=>
  [{"id"=>32, "options_text"=>"Size: L, Color: Green", "display_price"=>"$0", "price"=>0.0, "price_cents"=>0},
   {"id"=>35, "options_text"=>"Size: L, Color: Red", "display_price"=>"$0", "price"=>0.0, "price_cents"=>0}]}

{% endcodeblock %}

注意上面的available_options经过了过滤，只返回Size为L的variants的options，客户端收到response后可以过滤Color，上面的response中，filtered_variants也过滤为只包含Size L的variants

接下来如果用户再选择Color为Green，客户端向服务端再次发送请求，并且加上参数`?Size=L&Color=Green`,这时服务器返回结果

{% codeblock lang:json %}
{"id"=>10,
 "sku"=>nil,
 "name"=>"Gorgeous Concrete Shirt",
 "type"=>"Product",
 "description"=>
  "Quasi dolores id. Expedita deserunt quos commodi aut quod. Consequuntur placeat non voluptate ea nisi est minus. In aut neque aspernatur accusantium ducimus ut amet.",
 "display_price"=>"$0",
 "price"=>0.0,
 "price_cents"=>0,
 "has_variants"=>true,
 "available_options"=>
  [{"id"=>19, "presentation"=>"Size", "available_values"=>[{"id"=>47, "presentation"=>"L"}]},
   {"id"=>20, "presentation"=>"Color", "available_values"=>[{"id"=>49, "presentation"=>"Green"}]}],
 "images"=>[],
 "product_properties"=>[],
 "variants"=>
  [{"id"=>47, "options_text"=>"Size: L, Color: Green", "display_price"=>"$0", "price"=>0.0, "price_cents"=>0},
   {"id"=>48, "options_text"=>"Size: S, Color: Green", "display_price"=>"$0", "price"=>0.0, "price_cents"=>0},
   {"id"=>49, "options_text"=>"Size: S, Color: Red", "display_price"=>"$0", "price"=>0.0, "price_cents"=>0},
   {"id"=>50, "options_text"=>"Size: L, Color: Red", "display_price"=>"$0", "price"=>0.0, "price_cents"=>0}],



 "filtered_variants"=>[{"id"=>47, "options_text"=>"Size: L, Color: Green", "display_price"=>"$0", "price"=>0.0, "price_cents"=>0}]}

{% endcodeblock %}

可以看到filtered_variants只含一个element，即为用户最后选择的variant