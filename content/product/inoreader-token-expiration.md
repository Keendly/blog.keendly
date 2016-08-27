+++
bigimg = ""
title = "Inoreader token expired notifications"
type = "post"
date = "2016-08-27T02:16:04-07:00"
+++

Quick announcement for Inoreader users today.
Keendly integration with Inoreader is based on [Oauth](https://www.inoreader.com/developers/oauth).
Once user authorizes Keendly to access his account, we are able to fetch feeds, unread articles, mark them as read etc.

The token returned by Inoreader, is valid for [30 days](https://www.inoreader.com/developers/oauth#comment-2708071395) though. So if you've set up an automatic delivery a month ago and didn't log in during that time to Keendly, we lose access to your Inoreader data. That basically means that we're not able to deliver you articles.

The solution is very easy, just log in to Keendly, we'll refresh the tokens and your subscription will automatically get restored.

Although it is easy to fix, users don't realize why they stopped receiving feeds. That is why we've implemented **notifications**.
Now, if we've lost access to your account and weren't able to deliver articles to your Kindle, we will send you an email explaining what happened and asking to solve the issue by logging in to Keendly.

We think that it will reduce uncertainty hence make our users' life better. If you have other ideas to improve our service, feel free to reach out!
