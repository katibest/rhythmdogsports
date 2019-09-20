---
post_author: Michael Guterl
categories:
- Development
tags: []
post_title: Measuring Engagement with Ruby
publish_date: 2013-08-14T14:47:00.000+00:00
feature_post_image: ''
post_images: []
layout: post
current_gaslighter: false
slug: measuring-engagement-with-ruby
permalink: "/blog/:slug"

---
We're making a huge effort to be more active on our blog. We want to measure how
much activity our blog posts are generating on various websites. This led to the
creation of [engagement](https://github.com/gaslight/engagement).

Engagement is a Ruby Gem that will help you find out the number of comments a URL
is receiving on Reddit, HackerNews, Disqus or Twitter. You can find out more by
checking out the [README](https://github.com/gaslight/engagement/blob/master/README.md).

## Try it Out

```bash
gem install engagement
```

```ruby
hacker_news = Engagement::CommentCounter::HackerNews.new
reddit      = Engagement::CommentCounter::Reddit.new
counter     = Engagement::CommentCounter::Threaded.new([hacker_news, reddit])

counter.comments_count('http://gaslight.co/blog/measuring-engagement-with-ruby')
```

View the source on [GitHub](https://github.com/gaslight/engagement). Hop in and help out, too! Happy commenting!
