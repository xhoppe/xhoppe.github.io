---
layout: page
title: "messages api"
date: 2014-11-05 23:06
comments: true
sharing: true
footer: true
---

Here we describe the messages API.

## My messages

{% img [class names] /images/My_Messages_Home_Screen_minimized_gaussian_blur_bg.png 400x200 %}

**This API needs authentication**

'GET /message_copies'

The Api accepts a parameter **since**, for example

'GET /message_copies?since=2014-11-22T12:00:00Z'

And it will return only the new messages since that time.

返回结果分页，每页显示25条记录，用户可以用page参数指示第几页，page=0为第一页

'GET /message_copies?since=2014-11-22T12:00:00Z&page=2'

上面返回第三页

只所以叫message_copy而不是message主要是为了支持群发考虑，如果群发的时候消息只有一条，每个接受者有一个message_copy,这样可以节约资源

The response will be like following

**Updated on Nov 25**

注意下面id=17的例子，sender_avatar可能为空，image中的url, medium_url, thumb_url也可能为空

{% codeblock %}

{"total_pages"=>1,

 "message_copies"=>
  [
   {"id"=>17,
 "subject"=>"Aperiam autem modi ut.",
 "body"=>
  "Vitae quo eum nostrum rerum nihil. Repellendus beatae omnis tempora vero ipsam. Recusandae blanditiis debitis consequatur qui dolore. Nihil ipsam vel dolorem ratione.",
 "read"=>true,
 "resource_type"=>nil,
 "resource_id"=>nil,
 "sender_name"=>"Xhopper",
 "updated_at"=>"2014-11-25T10:23:27Z",
 "sender_avatar"=>nil,
 "image"=>{"url"=>nil, "medium_url"=>nil, "thumb_url"=>nil}},

   {"id"=>31,
 "subject"=>"Eaque at perferendis non aut aut tempora deserunt.",
 "body"=>"Perspiciatis omnis ex. Numquam tempora est omnis aliquid voluptas impedit. At quia accusantium.",
 "read"=>true,
 "resource_type"=>nil,
 "resource_id"=>nil,
 "sender_name"=>"Kiehn Inc",
 "updated_at"=>"2014-11-25T10:27:28Z",
 "sender_avatar"=>
  {"url"=>"/uploads/merchant/avatar/1/girl.jpeg",
   "medium_url"=>"/uploads/merchant/avatar/1/medium_girl.jpeg",
   "thumb_url"=>"/uploads/merchant/avatar/1/thumb_girl.jpeg"},
 "image"=>
  {"url"=>"/uploads/message/image/31/gshock.jpg",
   "medium_url"=>"/uploads/message/image/31/medium_gshock.jpg",
   "thumb_url"=>"/uploads/message/image/31/thumb_gshock.jpg"}
   }
  ]
}

{% endcodeblock %}

The resource_type and resource_id could be different kinds of objects. For example, resource_type can be `Coupon`, and resource_id can be the coupon id.

这个api会返回一个Last-Modified http header，格式为iso8601格式。客户端可以缓存这个时间戳，以后的请求可以用这个作为since param

## Delete message


**This API needs authentication**

'DELETE /message_copies/#{id}'

The id is message id.

This API returns an empty body. If the HTTP status is 200, the message is deleted

## New messages count

**This API needs authentication**

'GET /message_copies/unread_count'

It returns the count of new message.

{% codeblock %}

{
  unread_count: 7
}

{% endcodeblock %}

## Read message

**This API needs authentication**

'GET /message_copies/#{id}'

id is the message_copy id.

Sample response is


{% codeblock %}


{"id"=>31,
 "subject"=>"Eaque at perferendis non aut aut tempora deserunt.",
 "body"=>"Perspiciatis omnis ex. Numquam tempora est omnis aliquid voluptas impedit. At quia accusantium.",
 "read"=>true,
 "resource_type"=>nil,
 "resource_id"=>nil,
 "sender_name"=>"Kiehn Inc",
 "updated_at"=>"2014-11-25T10:27:28Z",
 "sender_avatar"=>
  {"url"=>"/uploads/merchant/avatar/1/girl.jpeg",
   "medium_url"=>"/uploads/merchant/avatar/1/medium_girl.jpeg",
   "thumb_url"=>"/uploads/merchant/avatar/1/thumb_girl.jpeg"},

 "image"=>
  {"url"=>"/uploads/message/image/31/gshock.jpg",
   "medium_url"=>"/uploads/message/image/31/medium_gshock.jpg",
   "thumb_url"=>"/uploads/message/image/31/thumb_gshock.jpg"}
}

{% endcodeblock %}

调用这个api后read属性会变为true


