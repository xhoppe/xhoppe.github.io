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

'GET /messages'

The Api accepts a parameter **since**, for example

'GET /messages?since=2014-11-22T12:00:00Z'

And it will return only the new messages since that time.

The response will be like following 

[
  {
    id: 1,
    title: 'Free ice cream',
    body: 'ljsfjsfjslfjs ljdslj ljdsflsjdf ',
    created_at: '2014-11-22T12:00:00Z',
    image: 'http://test.xhop.pe/aaa/aaa.jpg',
    ms_id: 3
  },

  {
    id: 2,
    title: 'Free ice cream',
    body: 'ljsfjsfjslfjs ljdslj ljdsflsjdf ',
    created_at: '2014-11-22T12:00:00Z',
    image: 'http://test.xhop.pe/aaa/aaa.jpg',
    ms_id: 
  }
]

The ms_id is the moodstocks image id, it could be nil, if it's not nil, then when the user clicks the message, the client can invoke the scannable api to get the product or coupon.

## Delete message


**This API needs authentication**

'DELETE /messages/#{id}'

The id is message id.

## New messages count

**This API needs authentication**

'GET /messages/new_messages_count'

This message accepts a parameter **since**

'GET /messages/new_messages_count?since=2014-11-22T12:00:00Z'

It returns the count of new message.

{% codeblock %}

{
  new_messages_count: 7
}

{% endcodeblock %}