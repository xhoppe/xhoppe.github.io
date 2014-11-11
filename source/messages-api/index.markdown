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

只所以叫message_copy而不是message主要是为了支持群发考虑，如果群发的时候消息只有一条，每个接受者有一个message_copy,这样可以节约资源

The response will be like following

{% codeblock %}

[
  {
    "id"=>8,
    "subject"=>"Eos deserunt hic repudiandae.",
    "body"=>
     "Velit ratione aut libero aut et eum. Dignissimos alias pariatur est. Deserunt perspiciatis corporis natus. Repellendus est fuga ab eaque consequuntur voluptates rerum. Et adipisci totam vitae vel.",
    "read"=>false,
    "resource_type"=>nil,
    "resource_id"=>nil,
    "updated_at"=>"2014-11-11T09:58:44Z"
  },
  {
    "id"=>7,
    "subject"=>"Delectus consequatur expedita eum aut.",
    "body"=>
     "Aut possimus rerum odit quis. Aliquid enim quis dignissimos aut vitae sint. Sed aut rem amet sequi. Rerum et modi est adipisci sint eos et. Fuga ullam occaecati sit ipsa ea.",
    "read"=>false,
    "resource_type"=>nil,
    "resource_id"=>nil,
    "updated_at"=>"2014-11-11T09:58:44Z"
  }
]

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


{
  "id"=>8,
  "subject"=>"Eos deserunt hic repudiandae.",
  "body"=>
   "Velit ratione aut libero aut et eum. Dignissimos alias pariatur est. Deserunt perspiciatis corporis natus. Repellendus est fuga ab eaque consequuntur voluptates rerum. Et adipisci totam vitae vel.",
  "read"=>true,
  "resource_type"=>nil,
  "resource_id"=>nil,
  "updated_at"=>"2014-11-11T09:58:44Z"
}

{% endcodeblock %}

调用这个api后read属性会变为true


