---
title: "Introducing personal sender emails"
subtitle: "Keendly is back!"
type: "post"
date: "2017-03-02T02:06:04-07:00"
categories: [ "Product" ]
---

After more than a week of being down, we're back!


What happened?
-------------
So far the only information we shared was that we got blocked by Amazon from using the Send To Kindle service. Let us explain it a bit further. As you know, we are generating ebooks from articles. Those ebooks are pretty much just files in a format that Kindle devices understand. In order to make them appear on Kindle, we are sending them by email using the [Send To Kindle](https://www.amazon.com/gp/sendtokindle) service.  
As more and more people was using Keendly, Amazon detected that single sender email (kindle@keendly.com) is sending documents to many different recipients, which made them block this sender.

What did we do to solve it?
-------------------------
During the discussion with the Amazon support, we were suggested that it would be fine if each user had his own sender email address. This is what we did. We create a send-only email for every Keendly user and use it to deliver his documents to Kindle. This way there is not gonna be a single email address used to sent content to all the Keendly users, so we won't be blocked and you will still receive your articles directly to the Kindle device :)

OK, but what do I need to do?
-------------------
Actually there is one thing. If you already have an account in Keendly (new users will have it created automatically), you will need to generate your personal sender email address. Simply go to the [settings page](https://app.keendly.com/user) and click *Save*:  
![Config](/img/personal-sender-emails/config.png)

Once done, you will get the information with the sender email address that got generated for you:  
![Email](/img/personal-sender-emails/email.png)

Last step would be to change the approved email in [Amazon](https://www.amazon.com/mn/dcw/myx.html/ref=kinw_myk_surl_2#/home/settings/), go to the **Settings** tab, scroll down to the **Approved Personal Document E-mail List** section, click **Add a new approved e-mail address**:  
![Amazon](/img/personal-sender-emails/amazon.png)

Once you added the address, feel free to remove the old one *kindle@keendly.com*, it is not gonna be used anymore.  
Well done! You are now all set to continue receiving articles from Keendly :-)

As always, if you have any questions, bring it!
