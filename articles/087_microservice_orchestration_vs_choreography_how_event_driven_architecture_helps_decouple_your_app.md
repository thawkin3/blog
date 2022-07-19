# Microservice Orchestration vs. Choreography: How event-driven architecture helps decouple your app

Microservices are all the rage right now. They allow you to split apart your app into small chunks with clear domain boundaries. Microservices are decoupled from each other, allowing them to be changed and deployed independently of one another, which provides greater autonomy to the teams working on each microservice. They also provide easier ways to scale heavily-used parts of the app without having to scale up the entire monolith.

It’s easy to see why microservices are appealing. However, microservices also bring with them a lot of complexity, from data consistency to communication to logging to service discovery.

It’s the challenge of communication between microservices that I’d like to address today.

---

## Two Modes of Collaboration

What do you do when microservices need to talk to each other? Ideally all communication should be asynchronous. Additionally, any sort of interactions between microservices should not tightly couple them together. Otherwise, you lose out on the benefits of independent changes and deployments.

There are two main modes of collaboration we can reach for: **orchestration** (request/response) or **choreography** (event-driven).

With orchestration, or the request/response pattern, a central service tells everything what to do. It makes requests to other downstream services to ensure that all the right things happen, and the downstream services respond to these requests.

With choreography, or event-driven architecture, the first service simply emits an event. Various services can subscribe to these event channels, and when they see a particular event emitted, they can respond however they would like.

---

## An Example: E-Commerce Checkout

Let’s consider an example to better understand these two paradigms.

Imagine we have an e-commerce site. When a customer completes the checkout process, a few things need to happen:

* We need to send an email to the customer with their order confirmation.

* We need to send the order to our warehouse so we can begin the shipment.

* If the customer signed up for our rewards program, we need to create a new points balance for them.

It’s important to note that once an order is placed, these next three tasks don’t necessarily need to happen in any particular order, they just need to happen.

Our checkout service will handle the initial interaction with the customer. Our three downstream services are an email service, a warehouse service, and a rewards service. Our email service will handle sending the email, our warehouse service will handle fulfilling the order, and our rewards service will handle creating the new points balance.

So what would the process look like for orchestration and for choreography?

---

## Orchestration

![Checkout process with orchestration](https://dev-to-uploads.s3.amazonaws.com/uploads/articles/zbvtp90y9h3sgn531b2q.png)
<figcaption>Checkout process with orchestration</figcaption>

With an orchestrated process, the checkout service would act as our central service. When the customer submits their order, the checkout service would then make three API requests to the email service, warehouse service, and rewards service. The checkout service would tell the email service to send an email, the warehouse service to started preparing the shipment, and the rewards service to create the new points balance.

In this sense, the checkout service acts as the conductor of an orchestra, telling all the players what to do and when.

---

## Choreography

![Checkout process with choreography](https://dev-to-uploads.s3.amazonaws.com/uploads/articles/l3flvi8g8vr8ye6wd35p.png)
<figcaption>Checkout process with choreography</figcaption>

With a choreographed or event-driven architecture, the business process is the same, but the way our services handle it is slightly different. When the customer submits their order, the checkout service would emit an event. This even would be called something like “OrderCreated” and would include the order details.

Our three downstream services (email, warehouse, and rewards) would be subscribed to an event channel and listening for events. When the “OrderCreated” event is emitted, our three services would see the event and respond accordingly by sending the email, preparing the shipment, and creating the new points balance.

In this sense, our services act as dancers in a ballet, each independently dancing their part as they respond to the music.

---

## Downsides of Orchestration

Did you catch the difference here? It’s subtle, but important. The main difference is that choreography, or event-driven architecture, provides us with more flexibility when our app needs to change.

What if, for example, later down the road we’ve created a fourth service, and this also needs to do something when an order is placed? In an orchestrated model, in order to change our app, we would need to modify our checkout service to make a fourth API request to this new service.

![Adding a fourth service with orchestration](https://dev-to-uploads.s3.amazonaws.com/uploads/articles/awz736ompd5rkrptxbu7.png)
<figcaption>Adding a fourth service with orchestration</figcaption>

Or what if the API endpoint to one of our existing three services has changed? Maybe rather than calling the original endpoint, we need it to call a different endpoint? Or maybe we need it to make two API requests to the service rather than one? In an orchestrated model, changes of this sort would again require us to modify the checkout service.

We’ve inadvertently coupled our microservices together!

---

## Benefits of Choreography

Now, think back to the choreographed architecture. The checkout service simply emits an event, and all the downstream services simply respond to the event. 

So, need to add a fourth service to the mix? No problem, just have it subscribe to that event.

![Adding a fourth service with choreography](https://dev-to-uploads.s3.amazonaws.com/uploads/articles/l002a1qkll6kovod1h7h.png)
<figcaption>Adding a fourth service with choreography</figcaption>

Need to change how an existing service responds to the event? No problem, we are capable of changing a downstream service’s behavior without affecting any of the other services like the checkout service.

Choreography allows us greater flexibility and helps us keep our services decoupled.
