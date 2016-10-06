---
title: "Migration to SWF"
subtitle: "First post about the tech behind Keendly"
type: "post"
date: "2016-10-06T00:45:04-07:00"
categories: [ "Technology" ]
---

In the first post about the technology behind Keendly, we're gonna tell about our migration from custom flow system to [Amazon Simple Workflow](https://aws.amazon.com/swf/). We will try to explain why we decided to replace working system for something we didn't understand and share our finding and experience.

Problem
------

As you know, we are generating ebooks. Generating each of them consists of several steps. We need to:

* fetch the article content from the website
* download, resize and compress images
* generate actual ebook
* send it via email
* mark delivery as finished
* mark articles as read

From the very beginning we wanted those tasks to be independent. To achieve that we needed something to orchestrate, a single place to define the flow.

First solution
----

Our first approach was to implement it using [Amazon Notification Service](https://aws.amazon.com/sns/) and [Lambda Function](https://aws.amazon.com/lambda/). It was working pretty well and had several advantages:

* _no servers whatsoever_
* _very simple implementation_
* _flow definition in single place_

But after few weeks we faced some issues that we didn't think about before:

* _lack of visibility_  
  We couldn't see how many flows are being executed at given moment. If for whatever reason flow failed, it was very difficult to find out which step crashed and why.
* _complicated testing_  
  System that relies on asynchronous messaging is difficult to test. We had it pretty well unit tested, but lack of an easy way to run the whole flow was complicating development and debugging.
* _no timeouts on tasks_  
  If the task crashed the flow will wait for the result forever. No way to timeout or recover.

Evaluation SWF
----

First looks at SWF were not very promising. The service didn't seem very intuitive. Flow implementation also seemed complex and involving. You need two types of nodes: decider and workers. Decider coordinates the flow and workers process tasks. It all seemed bit overwhelming. Everything changed when we discovered the [Flow Framework](https://aws.amazon.com/swf/details/flow/), it hides the details of SWF and allows to create asynchronous distributed workflow as it was a simple synchronous application.

Implementation
----

We chose to use Java version of the Flow Framework (there is also one for Ruby). It took quite some time to setup the project with maven. Mainly because asynchronous methods are implemented using aspects and you need to configure the plugin so that it can actually process the annotations.  
We decided not to use SWF activities to make the workers not to depend on SWF. Flow Framework generates classes for you to be used in activities and we wanted them to be totally decoupled. Instead we used integration with Lambda functions combined with signalling the workflow.
From the high level point of view our delivery workflow looks now like this:
![Flow diagram](/img/migration-to-swf/diagram.png)

With this approach the workers are independent from the flow and can scale easily based on the number of messages in the incoming queue. If the task is quick enough (like sending an email), there is no worker but instead all happens during the Lambda execution.  
Flow Framework comes with tools that allow you to run your flow locally, in the unit test. Although it is far from being perfect, it was a great improvement.  
The advantages of this solution over the initial one:

* _better visibility_  
  You get CloudWatch metrics for free. You can also easily see all executions on the SWF console. With time that each task took to complete etc. You can also re-run the flow if you want.  
* _easier testing_  
  With the flow JUnit runner, you can run the whole flow in an unit test.  
* _built-in timeouts_  
  If you use SWF activities, you can easily configure timeouts. We didn't use them, but with a bit of digging we were able to implement task timeouts quite easily.

Some cons:

* _the decider process is needed_  
  You need to run the Decider app. No support for decider in Lambdas (sort of what we had in the first version).
* _complex setup_  
  Even though it is easier than other workflow solution, especially combined with Java Flow Framework, it takes time to configure and understand all the things.
* _testing capabilities_  
  Even though there is a support for tests, it is not great. We had to create our own tooling around it.
* _lambda 1min timeout_  
  Yes, I know that there is 5min timeout for the function execution. But for some reason workflow step timeouts after 1 min. I tried reaching out AWS people on the [forum](https://forums.aws.amazon.com/thread.jspa?threadID=236001&tstart=0), but no luck.

Conclusion
----

We are very happy with the transition. Not only the delivery flow is now easier to manage, but also we can easily extend it or introduce even new workflows. Amazon Simple Workflow combined with the Flow Framework is a good option for creating distributed asynchronous flows. For the price of time spent on setup it gives you control over your workflows. It is worth giving a try if you want to decouple workflow and  track the flow state, be able to retry tasks etc.
