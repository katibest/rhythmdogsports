---
layout: post
post_author: Joel Turnbull
current_gaslighter: false
categories:
- Development
tags: []
permalink: "/blog/:slug"
post_title: Circumventing Same-Origin Policy Using a Proxy Server
publish_date: 2015-07-14 14:00:00 +0000
feature_post_image: ''
post_images: []
slug: circumventing-same-origin-policy-using-a-proxy-server

---
I had problems wrapping my head around same-origin policy the first time I encountered it, and I’ve found a lack of in-depth articles about how to deal with it. What follows is a small discussion of what same-origin policy is, and one simple solution for getting around it.

[Same-origin policy](https://en.wikipedia.org/wiki/Same-origin_policy) restricts scripts contained in a web page from accessing data in a second web page unless they share the same origin. It’s a security measure enforced by browsers that restricts malicious websites from running JavaScript inside of websites they don’t own. This [article](http://security.stackexchange.com/questions/8264/why-is-the-same-origin-policy-so-important) has a good example of the kinds of problems same-origin policy is meant to address:

> Assume you are logged into Facebook and visit a malicious website in another browser tab. Without the same origin policy JavaScript on that website could do anything to your Facebook account that you are allowed to do. For example read private messages, post status updates, analyse the HTML DOM-tree after you entered your password before submitting the form. 

To protect against these scenarios, browsers restrict this kind of access by default. It is possible, using [CORS](https://en.wikipedia.org/wiki/Cross-origin_resource_sharing), to configure a server to allow requests from client-side applications running on outside domains. But this means that if your app uses JavaScript in the browser to interact with a third-party server, you need to have some level of administrative control over that API server. This is often not the case!

## The Solution

One common situation is fetching third-party API data with your client side JavaScript application. Same-origin restricts this, so how do you do it? One solution is to use a proxy. The concept is simple: You insert your own server in-between your app and the third-party server. There is no browser enforced same-origin limitation on server-to-server communication, and you do have the administrative control necessary to configure your proxy server to allow requests from your app’s domain. 

Your proxy server sits between your JavaScript application and the third-party API server. It takes requests from your application, fetches data from the third-party API, and then forwards that data back to your application in a response that authorizes it using the `Access-Control-Allow-Origin` header.

Ruby folks can set up a simple proxy using Sinatra and Faraday like so:

````ruby

require 'sinatra'
require 'faraday'

ALLOWED_REFERRERS = ['http://mydomain.com','http://localhost:3000']

get '/foo/bar' do
  response = do_request(“foo/bar”)
  set_access_control
  response.body
end

def do_request(endpoint)
  url = "https://api.thirdparty.com/#{endpoint}"
  conn = Faraday.new(url)
  conn.get(url)
end

def set_access_control
  request_referrer = request.env['HTTP_REFERER'] || request.env['REQUEST_URI']

  referrer = ALLOWED_REFERRERS.detect do |allowed_referrer|
    request_referrer =~ /#{allowed_referrer}/i
  end

  headers "Access-Control-Allow-Origin" => referrer
end
````

Your Sinatra server defines a route, `/foo/bar`, that your JavaScript application can hit. You can name it yourself, or just match the endpoint defined by the third-party like I did. The route action does three things. First, it fetches the data from the third-party API. Next it sets the ‘Access-Control-Allow-Origin’ header in the response that it formulates. Lastly, it forwards the data back to the JavaScript application in the response body.

The ‘Access-Control-Allow-Origin’ header does not support multiple values. The ‘Access-Control-Allow-Origin’ does support a wildcard `*` value that will allow requests from every origin, but unless your data is truly meant for public consumption, you'll only want to feed requests that come from trusted parties. 

Even though it doesn't support multiple values, you will most likely want to serve requests from multiple trusted domains. In the example above, I allow access from the domain where my app runs in "production" `mydomain.com`, and my local rails domain `localhost:3000` so that I can do local development against it.

The `set_access_control` method above handles this by programmatically checking that the request came from one of those domains, and sending that domain back as the value of your “Access-Control-Allow-Origin” header. This is the strategy recommended by the W3C.

Using a proxy, you've effectively fetched the data you need from the third party API, and have told the browser to allow access to the API from our current domain. Voila!

