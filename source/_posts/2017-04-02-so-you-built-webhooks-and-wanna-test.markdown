---
layout: post
title: "So, you built webhooks? And wanna test "
date: 2017-04-02 23:28
comments: true
categories: [ruby]
keywords: ruby,rspec,webhooks,ngrok
description: "Ruby library to test webhook calling code."
---

When you build public facing APIs, there are operations that could not fit enough into request-response cycle. Especially, when it involves user interaction.

Take an example of sending email. You can expose an API endpoint for clients to send email. But whenever the email is opened, the client want to do some action. They can keep polling you and get the status of the email of course. But, that's suboptimal on both sides. So you want to tell the client when the email is opened. Now, that's when you do webhooks ([ref](https://webhooks.pbworks.com/w/page/13385124/FrontPage)).

Webhooks are easier to build. After all, it's just one http call you will have to make.

But, testing it is not so.

<!-- more -->

**Public URL:** We need the webhook URL to be accessible from the application server. For local testing `localhost` would do. But when you want to test the behaviour in CI ot other enviroments, it requires to be a public URL.

**Recorded requests:** Once the application is invoked and it called the webhook URL, we need to assert on the request made by application. So we need a way to record those requests and assert later.

So [webhook_recorder](https://github.com/siliconsenthil/webhook_recorder) is born. It uses `WEBrick` server to run locally and  uses [ngrok](https://ngrok.com) to expose that.

<iframe src="https://docs.google.com/presentation/d/1-Ic9WCukD9D1FGYISvo1TAfa5hW5YsN0zCF9QotGujQ/embed?start=false&loop=false&delayms=3000" frameborder="0" width="600" height="460" allowfullscreen="true" mozallowfullscreen="true" webkitallowfullscreen="true"></iframe>

<br>
<br>

_PS: We at [Simpl](https://getsimpl.com) are in a mission of making money simple. We value quality, talent and most importantly culture. Wanna join hands? Let me know at [@siliconsenthil](https://twitter.com/siliconsenthil)_
