---
post_author: Joel Turnbull
categories:
- Development
- Design
tags: []
post_title: Adding a Share by Email Button to Your Tumblr Posts
publish_date: 2012-07-09T04:05:00.000+00:00
layout: post
current_gaslighter: false
feature_post_image: ''
post_images: []
slug: adding-a-share-by-email-button-to-your-tumblr-posts
permalink: "/blog/:slug"

---
Apparently email is still a valid way of sharing things. It may still be the
best way of sharing something with one person. Every social media app has an
api for "share" buttons, but there doesn't seem to be a whole lot of code out
there to steal in the way of email sharing buttons. So if you have a Tumblr
blog like this one, and can run some Ruby, steal this script if you like:

```ruby
require 'open-uri'

def main
  image_url = "https://s3.amazonaws.com/gaslight-blog/email_button.png"
  subject = "{Title}"
  body = <<-eos
Check out this article...

{Title} <{ShortURL}>
  eos

  puts "<a href='mailto: ?subject=#{subject}&body=#{encode(body)}'><img src='#{image_url}'></a>"
end

def encode(string)
  string = URI::encode(string)
  string.gsub!('%7B','{')
  string.gsub!('%7D','}')
  string
end

main
```

The output is html that you can paste into your custom Tumblr theme. An anchor
tag, with a mailto action, that wraps a button image. You'll need to replace
the value of "image_url" in the script ( the one here is locked down by s3 ).
The rest may work out just fine for you.

The email copy is uri-encoded and passed as the value to the "body" parameter
in the mailto query string. The interface to the mailto scheme is pretty
limited. I quit in my attempt to get any multipart html/text email stuff going
on. The RFC states "The mailto URL is primarily intended for generation of
short text messages that are actually the content of automatic processing
(such as "subscribe" messages for mailing lists), not general MIME bodies."
So, yeah.

I'm also pretty aware that I'm hacking the mailto scheme by putting a space
where the "to" value should go. The idea is that an email client will pop up
in a draft mode with the "to" field empty, so that the article sharer can fill
it in. I got the result I wanted, but the execution seems shady.

Leave comments. I'm interested in better ways to do this, or ideas around how
to achieve richer email messages from a share by email button.
