---
layout: post
post_author: Peter Kananen
current_gaslighter: true
categories:
- Development
- Business
tags: []
permalink: "/blog/:slug"
post_title: ActiveAdmin is the New Spreadsheet
publish_date: 2014-03-14T13:32:00.000+00:00
feature_post_image: ''
post_images: []
slug: activeadmin-is-the-new-spreadsheet

---
If your company is anything like Gaslight, it's pretty common to have a bunch of spreadsheets in Dropbox or Google Drive to keep track of random operational details. We have had spreadsheets tracking time off requests, birthdays, G-Days (employment anniversaries), blog writing schedules, and contact information for team members. Spreadsheets are easy to create and edit by everyone, so they make sense to use for these functions. However, they also tend to not be maintained well over time, and they also lock in data and make it hard to share with other systems.

Because we're a development shop, we've always had a Rails app to play with and house tools we've created to help run our business. Recently, I've been taking some of these crufty spreadsheets and creating simple admin interfaces around this data. I've been using [ActiveAdmin](http://activeadmin.info/) for this because it's just so easy.

Here are the columns our "Birthdays and G-Days" spreadsheet had:

<table>
<thead>
<td>Name</td>
<td>Birthday</td>
<td>G-Day</td>
</thead>
</table>

It'd probably take 10 minutes to create this spreadsheet and enter data for all 19 people at Gaslight. To get the same data in our Rails app and viewable through the ActiveAdmin interface, it'd probably take no more than 20 minutes (assuming ActiveAdmin is already installed). Here's what we need to model the data:

```
rails g model Person name:string birthday:date gday:date
rails g active_admin:resource Person
```

If you want, you can do a bit of configuration in app/admin/people.rb to tweak your index view or form. I changed my date input fields to datepickers:

```
f.input :birthday, as: :datepicker
```

It takes just a bit more time to enter data in ActiveAdmin than in a spreadsheet, but it's not too bad for 19 people.

So what do we do with birthday and G-Day information for everyone at Gaslight? All of those dates sound like great excuses for office cupcakes to me! Of course, in order for cupcakes to be purchased, we need to know about the event. I created a simple ActiveMailer and accompanying Rake task that runs on a daily Heroku scheduler job. An email goes out at 5am notifying everyone of the day's (or upcoming weekend's) events. For example, on March 4th we got this email:

![G-Day email](https://draftin.com:443/images/12776?token=TTtNmunkVVr5MkghRiCyDgC6BDx9Fbn8ODW2qcjWQQplSbF6oeYKVPvGLy9AYy9ogLXhuDFnAejZcircSrDPhdA)

Not only are these notifications fun to get, they help to improve the general happiness level. People like being noticed, and automating these notifications means no one gets left out!

Another spreadsheet I replaced with ActiveAdmin was used to track our time off requests. Similar to our birthday and G-Day mailer, I created a mailer task to notify everyone of current and upcoming time off requests. This eliminates the "where's Chris today?" question that is asked when he's out of the office, and also serves as a reminder to follow up with someone before they're gone for a week on vacation.

![Time Off email](https://draftin.com:443/images/12790?token=TI1RKebs79KEWY5f_5wscpm99h6NqXurPkjmLoKRTlcUOB2zbNk7uxTa4DsVZYOP0-RQe_0HbIPDe-FWCQ3c2gg) 

These examples are simple ways to do more interesting things with organizational data. ActiveAdmin makes it almost painless to capture this information, and Rails provides a platform to extend whatever functionality you'd like. And if you ever miss your spreadsheets, you can just export a CSV from ActiveAdmin!