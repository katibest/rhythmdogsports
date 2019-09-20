---
layout: post
post_author: Doug Alcorn
current_gaslighter: true
categories:
- Development
tags: []
permalink: "/blog/:slug"
post_title: 'TIL: How to Explicitly Set Session Expiration in Phoenix'
publish_date: 2018-01-02 18:47:00 +0000
feature_post_image: ''
post_images: []
slug: til-how-to-explicitly-set-session-expiration-in-phoenix

---
By default Phoenix stores session data in browser cookies. I don’t know why, but I thought the default was for those cookies to never expire. It turns out by default they expire when the browser session ends. I found this out because my users weren’t able to stay logged in beyond a few days.


It took me a while to find it, but the [documentation for Plug.Session options](https://hexdocs.pm/plug/Plug.Session.html#module-options) shows you can set the `max_age` key to the number of seconds for it to expire. Typically, your `Plug.Session` is configured in your `lib/my_app_web/endpoint.ex` file:


```elixir

plug Plug.Session,
    store: :cookie,
    max_age: 24*60*60*37,       # 37 days
    key: "_my_app_key",
    signing_salt: "random signing salt"

```


Now my users will stay logged in for 37 days.

