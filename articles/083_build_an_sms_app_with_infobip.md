# Build an SMS App with Infobip

SMS is a powerful way to connect with your users. Businesses all over the world use SMS texts to send appointment reminders, shipping notifications, customer satisfaction surveys, and more. For countries or customers with slower internet speeds, SMS can even function as a viable alternative to something like an in-app chat feature.

In this article, we’ll demonstrate the power of SMS and showcase just how easy it is to get started. Together we’ll build a “Fun Fact of the Day” web app that allows users to enter their phone number to receive an SMS text with a fun fact. We’ll provide this functionality using the SMS API from [Infobip](https://www.infobip.com/), a cloud communications platform.

Let’s get started!

---

## Demo App Overview

![Demo app: Enter your phone number to receive an SMS text](https://dev-to-uploads.s3.amazonaws.com/uploads/articles/tkmvwzt6al3e82rddu3i.png)
<figcaption>Demo app: Enter your phone number to receive an SMS text</figcaption>

Our demo app is built with Node.js and Express on the backend and simple HTML, CSS, and JavaScript on the frontend.

![Demo app: Text message successfully sent](https://dev-to-uploads.s3.amazonaws.com/uploads/articles/g5eh1fjwxf6xrsm2rkl4.png)
<figcaption>Demo app: Text message successfully sent</figcaption>

Users can enter their phone number into this minimal interface and then click the submit button to receive a text triggered by the [Infobip API](https://www.infobip.com/docs/sms/api).

![Text message with a fun fact about penguins](https://dev-to-uploads.s3.amazonaws.com/uploads/articles/fozfeq29bx0odnhot4bv.png)
<figcaption>Text message with a fun fact about penguins</figcaption>

Simple as that!

Let’s walk through how we built this. We’ll include a few code snippets throughout the rest of this article, but feel free to [check out the GitHub repo](https://github.com/thawkin3/fun-facts-sms) for the full example code.

---

## Creating the Signup Form

Let’s start with the frontend code for the signup form. The form is constructed with your typical HTML form elements: `<form>`, `<label>`, `<input>`, and `<button>`:

{% gist https://gist.github.com/thawkin3/e4817de3a5c5ce36a2a36164d8c998f5 %}

When the user enters their phone number and submits the form, the JavaScript initiates an API request to an endpoint on our Node.js server:

{% gist https://gist.github.com/thawkin3/137d3b4a8ab03bfc53c0b31f14bb97f1 %}

---

## Using the Infobip SMS API

Heading over to our backend code now, our Express router receives the request from the frontend and initiates an API request of its own, this time to the Infobip SMS API:

{% gist https://gist.github.com/thawkin3/34589c9ad1a406ca6c01cd5e95136417 %}

Why make a server-side API request you ask? Primarily because we want to keep our API key a secret. The Infobip SMS API uses an [authorization header](https://www.infobip.com/docs/essentials/api-authentication#api-key-header) that requires us to provide our API key, and we wouldn’t want that to be fully visible to all users in their browser’s network requests. So instead, we can protect that API key by storing it in an `.env` file and only accessing it from the server, not the client.

With that, the Infobip SMS API sends a text to the phone number the user provided, and the browser’s UI displays a confirmation message. We’ve successfully texted someone a fun fact!

---

## Conclusion and Further Exploration

In our short time together, we’ve built a simple app, but there’s so much more we could do. Rather than just sending the one text, we could allow users to opt in to receive a fun fact every day. We could create a customer directory from everyone who signed up. We could even require two-factor authentication for users to verify their phone numbers before subscribing to our fun fact of the day service. The [options provided by the API](https://www.infobip.com/docs/api#channels/sms/send-sms-message) for SMS sending is extensive, and you can even set up [webhooks for reports](https://www.infobip.com/docs/api#channels/sms/receive-outbound-sms-message-report) on outbound messages.

The good news is that Infobip makes all of this easy. Whether you use their API directly, one of their SDKs, or their platform’s GUI, staying connected with your users can be a breeze.
