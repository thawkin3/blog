# Key Application Performance Metrics From the Viewpoint of a Statistician-Turned-Developer

You've just released your new app into the wild, live in production. Success! Now what? Your job is done, right? Wrong. Now that you've deployed your code, it's time to monitor it, collect data, and analyze your metrics.

Without application performance monitoring in place, you can't accurately determine how well things are going. Are people using your app? Is the app performant? Do the pages load quickly? Are your users experiencing any errors? If so, where? How often?

The first step to gather this type of data is with application monitoring. Once you have data though, it's important to analyze it correctly. Numbers can lie, and it's essential to ensure that you are drawing accurate conclusions.

In this article, we'll discuss some key application performance metrics and how to think about them with a statistics mindset.

---

## A Little About Me

Before we begin, a little about me. I began my college education as a psychology major. After a couple of years I realized that I enjoyed the hard science part of psychology more than the social science aspect, so I changed my major to statistics. During my time studying statistics, I took several programming classes learning various languages that statisticians learn, such as R, SAS, and SQL. I fell in love with programming and have pursued that path ever since. Even though I graduated with a Bachelor's degree in Statistics with an emphasis in Applied Statistics, I've spent my entire career working as a software engineer.

The reason I tell you this is that I still have the mindset of a statistician. I love a good chart and an interesting study. But misleading data, faulty reasoning, poor survey methodology, and unsupported conclusions bother me like no other!

So as we explore some of these key application performance metrics, let's make sure that we measure and understand them correctly.

---

## Key Application Performance Metrics

So, what are "key application performance metrics"? In short, these are essential metrics that help give you a good idea of how well your app is performing. None of them tell the whole story by themselves, but by looking at the metrics together holistically, we start to see the bigger picture. When considering these metrics, it's helpful to view them as a proactive and preventive tool rather than only as a reactive tool. By actively monitoring your application, you can stay on top of potential problems and nip them in the bud before they spiral into larger customer issues.

If you Google "key application performance metrics", there is a general consensus among experts on what you should be measuring. We'll look at these five areas:

1. Response Time
2. Error Rate
3. Request Rate
4. Application Availability
5. Slow Transactions and Expensive Queries

Your Platform as a Service (Paas) or Infrastructure as a Service (IaaS) provider can often provide you with a view into these metrics. In this article, we'll show some examples from the [Heroku Metrics Dashboard](https://devcenter.heroku.com/articles/metrics).

If you are using [Heroku](http://heroku.com/) to deploy and host your code, you can find the Metrics Dashboard for your own project by navigating to your app's Heroku Dashboard and then clicking on the Metrics tab. (Note, the Metrics Dashboard is not available for free accounts.)

Alright, let's get started.

---

## 1. Response Time

The first metric we'll look at is response time. Response time measures how long your app takes to respond to a request. You could view response times for your API, or you could also view response times for how long it takes your page to load when a user visits your site.

---

### The Statistician's View

The important thing to remember here is to not just look at the **average** response time. When you have data that is normally distributed, looking at the averages can sometimes be sufficient. However, when you have skewed data, averages can be misleading. For example, consider two datasets:

Dataset 1: [5, 5, 5, 5, 5]

Dataset 2: [1, 1, 1, 1, 21]

The average of both of those sets of numbers is 5. Imagine that those are response times. In the first dataset, your app consistently responds in 5 seconds. In your second dataset, your app generally responds in 1 second, but in one instance it took 21 seconds. These two datasets tell very different stories that you would miss if you just looked at the average!

For this reason, it's better to look at the **median**. In this example, the median of the first dataset is 5, and the median of the second dataset is 1. It can also be helpful to look at **percentiles** like the 95th or 99th percentile. In plain English, looking at the response time for the 95th percentile of your users tells you that your app responds in X amount of time or faster for 95% of users. Using these metrics can help you better identify **outliers** in your data, or data points that are far away from the rest of the body of data.

[Heroku's response time dashboard](https://devcenter.heroku.com/articles/reviewing-your-key-application-performance-metrics?preview=1#record-your-response-times-and-throughput) does a great job at exposing the metrics for average, median, and percentiles, so shout out to them for getting it right.

![Response time report displayed in Heroku](https://cdn-images-1.medium.com/max/1600/0*gOYwbl_ZqNEAFx3Q.png)
<figcaption>Response time report displayed in Heroku</figcaption>

---

## 2. Error Rate

Next, let's talk about error rates. If the name isn't self-explanatory enough, the error rate is the number of errors that your application generates over a given period of time. You should care about what errors your users see, what pages they see these errors on, and how frequently these errors occur. There are several great error monitoring tools out there that can provide you with this kind of information in great detail, but [New Relic](https://newrelic.com/) and [Sentry](https://sentry.io/) are two of my favorites.

---

### The Statistician's View

When viewing errors, it's important to view these in **time series graphs**. To put it simply, time series graphs show you how data changes over time. For instance, it should be obvious that you wouldn't just look at the total number of errors in your app for all time. It makes more sense to view the errors over a specified period of time, such as over the last 24 hours or over the last week.

![Error rates shown in New Relic](https://cdn-images-1.medium.com/max/1600/0*-LP3Mb3o0IucasC-.png)
<figcaption>Error rates shown in New Relic</figcaption>

Viewing your error rates in a time series graph helps you identify patterns. For example, are you seeing more errors this week than last week? What could have caused the error rate to spike? A new release? A drop in overall quality in your app due to poor code reviews and testing? Or is your error rate going down over time? If so, keep up the good work!

As an extra "gotcha", to avoid being misled, it can also be useful to view your error rates in the context of the total number of active users or active sessions during a given time frame. If there were 100 errors recorded in 24 hours and 50 active user sessions during that time, you're averaging about two errors per session. Now, let's say the next day you see 5,000 errors but have 2,500 active user sessions that day. While it may be tempting to fret about the dramatic increase in errors, it's wise to take a step back and understand that 5,000 errors divided by 2,500 sessions is still two errors per session. So in this scenario, it's likely that you haven't actually introduced a ton of new bad code; rather, more people are just using your app. They are likely just seeing those same errors that existed the day before. (You still need to fix those errors. Just don't panic.)

---

## 3. Request Rate

Next, let's look at request rates. This metric measures the number of requests from your users over a given period of time.

In addition to being a useful metric for monitoring your application's performance, request rates are beneficial to you from a cyber security standpoint as well. For example, if you see a spike in request rates, and your login API is being hammered by a single IP address, then it's very possible someone is trying to brute force their way into an account.

In another scenario, if a heavily-used API endpoint is suddenly reporting zero requests, something may be wrong. Perhaps an essential service has gone down. Time to fight a fire!

---

### The Statistician's View

The advice for evaluating and understanding request rates is very similar to what we've discussed with error rates: Use time series graphs, look for trends, and don't view your request rate data in isolation from other app usage statistics.

To help you monitor this, [Heroku's throughput dashboard](https://devcenter.heroku.com/articles/reviewing-your-key-application-performance-metrics?preview=1#record-your-response-times-and-throughput) allows you to view the requests per minute over a given timeframe and even breaks down the responses by HTTP status codes.

![Throughput report displayed in Heroku](https://cdn-images-1.medium.com/max/1600/0*qPiei_n7Vm7vRJd8.png)
<figcaption>Throughput report displayed in Heroku</figcaption>

---

### 4. Application Availability

Fourth, let's discuss application availability. This is often used in service level agreements (SLAs) with customers. For example, you might have an SLA of 99% uptime. Therefore, if your app is unavailable for more than 1% of the time, you may owe your client some money.

The metric for application availability is often implemented via a simple health check endpoint. To achieve a [systematic random sample](https://en.wikipedia.org/wiki/Systematic_sampling), a request to this endpoint is made at some interval, and if a 200 response is returned, the application is considered healthy and available. The uptime is then just a simple calculation of the total number of "OK" responses divided by the total number of requests to the health check endpoint. Simple enough.

---

### The Statistician's View

The key here is to choose an appropriate **sampling interval**, or how frequently you collect a data sample by hitting the health check endpoint and recording its response.

As an example of what *not* to do, suppose that you were to make a request to your health check endpoint every day at 9am. You do this for 10 days and receive 10 "OK" responses. Great! A perfect score, 10/10. But, what if your application had gone down for 12 hours from 10am to 10pm? Because of how infrequent your sampling interval was, you never collected any data during that downtime. Due to that misuse of sampling methodology, your application availability statistic is a bit of a lie.

Now, collecting only one response per day is a bit extreme. So, how often should you check your application health? Every hour? Every minute? Every second? I'd recommend checking your application health every second if possible, or as frequently as you can.

Ultimately the sampling interval you choose will largely be driven by client requirements and the nature of your application. For example, a fintech app will likely have higher availability requirements than a company blog. However, keep in mind that any gap in time between your data samples creates guesswork as you don't truly know if  your application was or wasn't available during that time.

---

## 5. Slow Transactions and Expensive Queries

Finally, let's examine slow transactions and expensive queries. Be on the lookout for bottlenecks in your application that result in slowness for your users. These bottlenecks could be API endpoints that take awhile to respond, inefficient database queries, or a combination of both. Your provider can often give you insights into what these bottlenecks may be, like in the [Heroku Diagnose tab](https://devcenter.heroku.com/articles/reviewing-your-key-application-performance-metrics?preview=1#identify-your-expensive-queries) or in [New Relic's APM solution](https://newrelic.com/products/application-monitoring).

![Slow transactions shown in New Relic](https://devcenter2.assets.heroku.com/article-images/1582147533-slowest-avg-response-time.png)
<figcaption>Slow transactions shown in New Relic</figcaption>

---

### The Statistician's View

When searching for ways to optimize your app, the [Pareto principle](https://en.wikipedia.org/wiki/Pareto_principle) (also known as the 80/20 rule) is important to remember: "roughly 80% of the effects come from 20% of the causes." In the context of application slowness, this can be interpreted as "roughly 80% of the slowness in your application is due to inefficiencies in 20% of your code."

The actual numbers may vary, but the principle is that there are probably a few core pieces of your application that need to be optimized to give you the greatest performance boost. The more parts of your application that you try to optimize, the more the [law of diminishing returns](https://en.wikipedia.org/wiki/Diminishing_returns) applies. In other words, it's not worth spending time optimizing every line of code in your app just to save three milliseconds. Because of these two principles, identifying the bottlenecks that will yield a high return on investment is key. 

---

## Conclusion

To summarize, you need application performance monitoring, and you need to pay attention to the metrics your monitoring tool provides. However, don't just collect the data and take the metrics at face value. Make sure you understand what you're really measuring and that you're interpreting the data correctly. By applying a stats mindset, you can turn data into insights.
