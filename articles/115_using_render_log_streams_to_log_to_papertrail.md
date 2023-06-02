# Using Render Log Streams to Log to Papertrail

![Render Log Streams demo app](https://cdn-images-1.medium.com/max/2352/1*slozILJkTUABe9pCAfBkTw.png)*Render Log Streams demo app*

The `console.log` function — the poor man’s debugger — is every JavaScript developer’s best friend. We use it to verify that a certain piece of code was executed or to check the state of the application at a given point in time. We may also use `console.warn` to send warning messages or `console.error` to explain what happened when things have gone wrong. Logging makes it easy to debug your app during local development.

But what about debugging your Node.js app while it’s running in a hosted cloud environment? The logs are kept on the server, to which you may or may not have access. How do you view your logs then?

Most companies use application performance monitoring tools and observability tools for better visibility into their hosted apps. For example, you might send your logs to a log aggregator like [Datadog](https://www.datadoghq.com/), [Sumo Logic](https://www.sumologic.com/), or [Papertrail](https://www.papertrail.com/) where logs can be viewed and queried.

In this article, we’ll look at how we can configure an app that is hosted on [Render](https://render.com/) to send its system logs to Papertrail by using [Render Log Streams](https://render.com/docs/log-streams). By the end, you’ll have your app up and running — and logging — in no time.

---

## Creating Our Node.js App and Hosting It with Render

Render is a cloud hosting platform made for developers by developers. With Render, you can easily host your static sites, web services, cron jobs, and more.

We’ll start with a simple Node.js and Express app for our demo. You can [find the GitHub repo here](https://github.com/thawkin3/render-log-stream-demo). You can also [view the app here](https://render-log-stream-demo.onrender.com/). To follow along on your machine, fork the repo so that you have a copy running locally. You can install the project’s dependencies by running `yarn install`, and you can start the app by running `yarn start`. Easy enough!

![Render Log Streams demo app](https://cdn-images-1.medium.com/max/2352/1*slozILJkTUABe9pCAfBkTw.png)*Render Log Streams demo app*

Now it’s time to get our app running on Render. If you don’t have a Render account yet, [create one now](https://render.com/). It’s free!

Once you’re logged in, click the “New” button and then choose the “Web Service” option from the menu.

![Creating a new web service](https://cdn-images-1.medium.com/max/3276/1*CH0yQIOftwIq_lQV8RvXVw.png)*Creating a new web service*

This will take you to the next page where you’ll select the GitHub repo you’d like to connect. If you haven’t connected your GitHub account yet, you can do so here. And if you have connected your GitHub account but haven’t given Render access to your specific repo yet, you can click the “Configure account” button. This will take you to GitHub, where you can grant access to all your repos or just a selection of them.

![Connecting your GitHub repo](https://cdn-images-1.medium.com/max/5296/1*pi_H09OXY7_2WvIATt3syw.png)*Connecting your GitHub repo*

Back on Render, after connecting to your repo, you’ll be taken to a configuration page. Give your app a name (I chose the same name as my repo, but it can be anything), and then provide the correct build command (`yarn`, which is a shortcut for `yarn install`) and start command (`yarn start`). Choose your instance type (free tier), and then click the “Create Web Service” button at the bottom of the page to complete your configuration setup.

![Configuring your app](https://cdn-images-1.medium.com/max/3868/1*B9i9wdC2jlb7IT7W3D-bdA.png)*Configuring your app*

With that, Render will deploy your app. You did it! You now have an app hosted on Render’s platform.

![Log output from your Render app’s first deployment](https://cdn-images-1.medium.com/max/5276/1*SjoTl37LLXko4pJW6G4DVQ.png)*Log output from your Render app’s first deployment*

---

## Creating Our Papertrail Account

Let’s now [create a Papertrail account](https://www.papertrail.com/). Papertrail is a log aggregator tool that helps make log management easy. You can create an account for free — no credit card required.

Once you’ve created your account, click on the “Add your first system” button to get started.

![Adding your first system in Papertrail](https://cdn-images-1.medium.com/max/4256/1*D-XRBZ1pERc0JeFcIyKDSg.png)*Adding your first system in Papertrail*

This will take you to the next page which provides you with your syslog endpoint at the top of the screen. There are also instructions for running an install script, but in our case, we don’t actually need to install anything! So just copy that syslog endpoint, and we’ll paste it in just a bit.

![Syslog endpoint](https://cdn-images-1.medium.com/max/4252/1*fk9cT2mvGPxc4X2xqz_x8A.png)*Syslog endpoint*

---

## Connecting Our Render App to Papertrail

We now have an app hosted on Render, and we have a Papertrail account for logging. Let’s connect the two!

Back in the Render dashboard, click on your avatar in the global navigation, then choose “Account Settings” from the drop-down menu.

![Render account settings](https://cdn-images-1.medium.com/max/4248/1*kiJa9GLSIhU4AloL8kHSRA.png)*Render account settings*

Then in the secondary side navigation, click on the “Log Streams” tab. Once on that page, you can click the “Add Log Stream” button, which will open a modal. Paste your syslog endpoint from Papertrail into the “Log Endpoint” input field, and then click “Add Log Stream” to save your changes.

![Adding your log stream](https://cdn-images-1.medium.com/max/2412/1*B1NkoGjx0HEIkV48EDrP5w.png)*Adding your log stream*

You should now see your Log Stream endpoint shown in Render’s dashboard.

![Render Log Stream dashboard](https://cdn-images-1.medium.com/max/4248/1*C902Vle2PXExmoUgE5ZofA.png)*Render Log Stream dashboard*

Great! We’ve connected Render to Papertrail. What’s neat is that we’ve set up this connection for our entire Render account, so we don’t have to configure it for each individual app hosted on Render.

---

## Adding Logs to Our Render App

Now that we have our logging configured, let’s take it for a test run. In our GitHub repo’s code, we have the following in our `app.js` file:

```js
app.get('/', (req, res) => {
    console.log('Log - home page');
    console.info('Info - home page');
    console.warn('Warn - home page');
    console.error('Error - home page');
    console.debug('Debug - home page');

    return res.sendFile('index.html', { root: 'public' });
});
```

When a request is made to the root URL of our app, we do a bit of logging and then send the `index.html` file to the client. The user doesn’t see any of the logs since these are server-side rather than client-side logs. Instead, the logs are kept on our server, which, again, is hosted on Render.

To generate the logs, open your demo app in your browser. This will trigger a request for the home page. If you’re following along, your app URL will be different from mine, but [my app is hosted here](https://render-log-stream-demo.onrender.com/).

---

## Viewing Logs in Papertrail

Let’s go find those logs in Papertrail. After all, they were logged to our server, but our server is hosted on Render.

In your Papertrail dashboard, you should see at least two systems: one for Render itself, which was used to test the account connection, and one for your Render app (“render-log-stream-demo” in my case).

![Papertrail systems](https://cdn-images-1.medium.com/max/2000/1*NaMKotS3ioqqH8I8n55j7w.png)*Papertrail systems*

Click on the system for your Render app, and you’ll see a page where all the logs are shown and tailed, with the latest logs appearing at the bottom of the screen.

![Render app logs in Papertrail](https://cdn-images-1.medium.com/max/5236/1*9IDpRToZ-2xMiKoLjhSb0A.png)*Render app logs in Papertrail*

You can see that we have logs for many events, not just the data that we chose to log from our `app.js` file. These are the syslogs, so you also get helpful log data from when Render was installing dependencies and deploying your app!

At the bottom of the page we can enter search terms to query our logs. We don’t have many logs here yet, but where you’re running a web service that gets millions of requests per day, these log outputs can get very large very quickly.

![Searching logs in Papertrail](https://cdn-images-1.medium.com/max/2248/1*zyCOuDBUD9tib0QbSgBkaQ.png)*Searching logs in Papertrail*

---

## Best Practices for Logging

This leads us to some good questions: Now that we have logging set up, what exactly should we be logging? And how should we be formatting our logs so that they’re easy to query when we need to find them?

What you’re logging and why you’re logging something will vary by situation. You may be adding logs after a customer issue is reported that you’re unable to reproduce locally. By adding logs to your app, you can get better visibility into what’s happening live in production. This is a reactive form of logging in which you’re adding new logs to certain files and functions after you realize you need them.

As a more proactive form of logging, there may be important business transactions that you want to log all the time, such as account creation or order placement. This will give you greater peace of mind that events are being processed as expected throughout the day. It will also help you see the volume of events generated in any given interval. And, when things do go wrong, you’ll be able to pinpoint when your log output changed.

How you format your logs is up to you, but you should be consistent in your log structure. In our example, we just logged text strings, but it would be even better to log our data in JSON format. With JSON, we can include key-value pairs for all of our messages. For each message, we might choose to include data for the user ID, the timestamp, the actual message text, and more.

The beauty of JSON is that it makes querying your logs much easier, especially when viewing them in a log aggregator tool that contains thousands or millions of other messages.

---

## Conclusion

There you have it — how to host your app on Render and configure logging with Render Log Streams and Papertrail. Both platforms only took minutes to set up, and now we can manage our logs with ease.

Keep in mind that Render Log Streams let you send your logs to any of several different log aggregators, giving you lots of options. For example, Render logs can be [sent to Sumo Logic](https://render.com/docs/log-streams#sumo-logic). You just need to create a Cloud Syslog Source in your Sumo Logic account. Or, you can [send your logs to Datadog](https://render.com/docs/datadog#setting-up-log-streams) as well.

With that, it’s time for me to *log* off.

Thanks for reading, happy coding, and happy logging!
