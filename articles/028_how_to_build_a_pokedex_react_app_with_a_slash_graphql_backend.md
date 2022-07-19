# How to Build a Pokédex React App with a Slash GraphQL Backend

Frontend developers want interacting with the backend of their web application to be as painless as possible. Requesting data from the database or making updates to records stored in the database should be simple so that frontend developers can focus on what they do best: creating beautiful and intuitive user interfaces.

[GraphQL](https://graphql.org/) makes working with databases easy. Rather than relying on backend developers to create specific API endpoints that return pre-selected data fields when querying the database, frontend developers can make simple requests to the backend and retrieve the exact data that they need—no more, no less. This level of flexibility is one reason why GraphQL is so appealing.

Even better, you can use a *hosted* GraphQL backend—[Slash GraphQL](https://dgraph.io/cloud/) (by [Dgraph](https://dgraph.io/)). This service is brand new and was [publicly released on September 10, 2020](https://dgraph.io/blog/post/announcing-slash-graphql/). With Slash GraphQL, I can create a new backend endpoint, specify the schema I want for my [graph database](https://en.wikipedia.org/wiki/Graph_database), and—*voila!*—be up and running in just a few steps.

The beauty of a hosted backend is that you don't need to manage your own backend infrastructure, create and manage your own database, or create API endpoints. All of that is taken care of for you.

In this article, we're going to walk through some of the basic setup for Slash GraphQL and then take a look at how I built a [Pokémon Pokédex app](http://tylerhawkins.info/pokedex-slash-graphql/build/) with React and Slash GraphQL in just a few hours!

You can [view all of the code here on GitHub](https://github.com/thawkin3/pokedex-slash-graphql).

*Update: On April 16th, 2021, Slash GraphQL was officially renamed Dgraph Cloud. The information below still applies, and you can still build the app as described.*

---

## Overview of the Demo App

![Pokémon Pokédex app](https://dev-to-uploads.s3.amazonaws.com/i/3wsl2z1898zywow4ywvl.png)
<figcaption>Pokémon Pokédex app</figcaption>

What 90s child (or adult, for that matter) didn't dream of catching all 150 original Pokémon? Our demo app will help us keep track of our progress in becoming Pokémon masters.

As we build out our app, we'll cover all the CRUD operations for working with an API: create, read, update, and delete.

We'll start by adding all our Pokémon to the database online in Slash GraphQL's API Explorer. Then, in the Pokédex app UI, we'll display all 151 Pokémon queried from the database. (Hey, I couldn't leave out Mew, could I?) At the top of the screen, we'll show two dropdown menus that will allow us to filter the shown results by Pokémon type and by whether or not the Pokémon has been captured. Each Pokémon will also have a toggle switch next to it that will allow us to mark the Pokémon as captured or not. We won't be deleting any Pokémon from our database via the app's UI, but I'll walk you through how that could be done in the event that you need to clean up some data.

Ready to begin our journey?

---

## Getting Started with Slash GraphQL

### Creating a New Backend

Once you've created your [Slash GraphQL](https://dgraph.io/cloud/) account, you can have your GraphQL backend up and running in just a few steps:

1. Click the "Create a Backend" button.
2. Give it a name. (For example, I chose "pokedex".)
3. Optionally, give the API endpoint URL a subdomain name. (Again, I chose "pokedex".)
4. Optionally, choose a provider and a zone. (This defaults to using AWS in the US West 2 region.)
5. Click the "Create New Backend" button to confirm your choices.
6. Get your backend endpoint. (Mine looks like this: https://pokedex.us-west-2.aws.cloud.dgraph.io/graphql.)
7. Click the "Create your Schema" button.

That's it! After creating a new backend, you'll have a live GraphQL database and API endpoint ready to go.

![Creating a new backend](https://dev-to-uploads.s3.amazonaws.com/i/kjm4yfsx03f3qca2cjt7.png)
<figcaption>Creating a new backend</figcaption>

---

### Creating a Schema

Now that we have our backend up and running, we need to create the schema for the type of data we'll have in our database. For the Pokédex app, we'll have a `Pokémon` type and a `PokémonType` enum.

{% gist https://gist.github.com/thawkin3/629f3befd8810141e271f551af076cd0 %}

There's a lot to unpack in that small amount of code! The `PokémonType` enum is straightforward enough—it's a set of all the Pokémon types, including Fire, Water, Grass, and Electric. The `Pokémon` type describes the shape of our data that we'll have for each Pokémon. Each Pokémon will have an ID, a name, an image URL for displaying the Pokémon's picture, the types of Pokémon it is, and a status indicating whether or not the Pokémon is captured.

You can see that each field has a data type associated with it. For example, `id` is an `Int` (integer), `name` and `imgUrl` are `String` types, and `captured` is a `Boolean`. The presence of an exclamation point `!` means the field is required. Finally, adding the `@search` keyword makes the field searchable in your queries and mutations.

To test out working with our database and newly created schema, we can use the API Explorer, which is a neat feature that allows us to run queries and mutations against our database right from within the Slash GraphQL web console.

---

## Populating Our Database

Let's use the API Explorer to insert all of our Pokémon into the Pokédex database. We'll use the following mutation:

{% gist https://gist.github.com/thawkin3/ad84f247a28e9c01e9243e5d1f4db137 %}

For brevity I've only shown the first nine Pokémon in the snippet above. Feel free to check out the [full code snippet for adding all the Pokémon](https://gist.github.com/thawkin3/bc69df149c8e8cb004a9575fc43b1297).

![Adding all the Pokémon via the API Explorer](https://dev-to-uploads.s3.amazonaws.com/i/9eweys4yx4sgrwh4knm5.png)
<figcaption>Adding all the Pokémon via the API Explorer</figcaption>

Now, for a quick sanity check, we can query our database to make sure that all our Pokémon have been added correctly. We'll request the data for all our Pokémon like so:

{% gist https://gist.github.com/thawkin3/524e73922fe1a9f2372557c42bb5b658 %}

Here's what it looks like in the API Explorer:

![Querying for all Pokémon in the API Explorer](https://dev-to-uploads.s3.amazonaws.com/i/7p9ex8j7cu13er03qnz6.png)<figcaption>Querying for all Pokémon in the API Explorer</figcaption>

We could also write a similar query that only returns the Pokémon names if that's all the data we need. Behold, the beauty of GraphQL!

{% gist https://gist.github.com/thawkin3/97dbfb86de2df2aa5c795f8fe2cec0a8 %}

![Querying for all Pokémon names in the API Explorer](https://dev-to-uploads.s3.amazonaws.com/i/ojil7j2qqthn3t5e4sfe.png)
<figcaption>Querying for all Pokémon names in the API Explorer</figcaption>

---

## Fetching Data in the App

Now that we've added our Pokémon to the Pokédex and verified the data is in fact there, let's get it to show up in our app. Our app was built with [React](https://reactjs.org/) and [Material UI](https://material-ui.com/) for the frontend and was bootstrapped using [create-react-app](https://github.com/facebook/create-react-app). We won't be going through step-by-step how to build the app, but we'll highlight some of the key parts. Again, the [full code is available on GitHub](https://github.com/thawkin3/pokedex-slash-graphql) if you'd like to clone the repo or just take a look.

When using Slash GraphQL in our frontend code, we essentially just make a POST request to our single API endpoint that we were provided when creating the backend. In the body of the request, we provide our GraphQL code as the `query`, we write a descriptive name for the query or mutation as the `operationName`, and then we optionally provide an object of any `variables` we reference in our GraphQL code.

Here's a simplified version of how we follow this pattern to fetch our Pokémon in the app:

{% gist https://gist.github.com/thawkin3/d3836e03674eeeb21b02f6f58f100144 %}

We then take that data and loop over it using the Array `map` helper function to display each Pokémon in the UI.

The filters at the top of the page are hooked up to our API as well. When the filter values change, a new API request kicks off, but this time with a narrower set of search results. For example, here are all the Fire type Pokémon that we've captured:

![Captured Fire type Pokémon](https://dev-to-uploads.s3.amazonaws.com/i/msc6r9035cp7ns13iu25.png)
<figcaption>Captured Fire type Pokémon</figcaption>

The JavaScript for making an API request for Pokémon filtered by type and captured status looks a little like this:

{% gist https://gist.github.com/thawkin3/22f8c0e1d89aecddb8ce42db439e9404 %}

---

## Updating Data in the App

At this point we've sufficiently covered creating Pokémon from the API Explorer and fetching Pokémon within our Pokédex app via JavaScript. But what about updating Pokémon? Each Pokémon has a toggle switch that controls the Pokémon's captured status. Clicking on the toggle updates the Pokémon's captured status in the database and then updates the UI accordingly.

Here is our JavaScript to update a Pokémon:

{% gist https://gist.github.com/thawkin3/37bad118a2d018b1a126074b86ac664a %}

We then call the `updatePokemonCapturedStatus` function when the toggle value changes. This kicks off the API request to update the value in the database. Then, we can either optimistically update the UI without waiting for a response from the backend, or we can wait for a response and merge the result for the single Pokémon into our frontend's larger dataset of all Pokémon. We could also simply request all the Pokémon again and replace our frontend's stored Pokémon info with the new result, which is what I chose to do.

---

## Deleting Data from the Database

The last of the CRUD operations is "delete". We won't allow users to delete Pokémon from within the app's UI; however, as the app admin, we may need to delete any mistakes or unwanted data from our database. To do so, we can use the API Explorer again.

For example, if we found that we have an extra Bulbasaur in our Pokédex, we could delete all the Bulbasaurs:

{% gist https://gist.github.com/thawkin3/529a7e2acf4b39e3d857d562481712e0 %}

![Deleting all Bulbasaur Pokémon via the API Explorer](https://dev-to-uploads.s3.amazonaws.com/i/xxo60g46dtd9nrtghvfw.png)
<figcaption>Deleting all Bulbasaur Pokémon via the API Explorer</figcaption>

Then, we could add one Bulbasaur back:

{% gist https://gist.github.com/thawkin3/5d91284bc400f67a2cbb911c0806a125 %}

---

## Wrapping Up

So, what did we learn? By now we should understand how to work with Slash GraphQL in the context of a React app. We've covered all the CRUD operations to make a pretty sweet Pokédex app. We may have even caught a few Pokémon along the way.

Hopefully we didn't... hurt ourselves in confusion... [*cue audible groans from the readers*].

We haven't yet covered [how to add authentication](https://dgraph.io/docs/graphql/authorization/authorization-overview/) to secure our app or how to use the [Apollo client](https://www.apollographql.com/) when making our GraphQL requests, but those are important topics for another article!

As an experienced frontend developer but without much experience using GraphQL, working with Slash GraphQL was refreshingly easy. Getting set up was a breeze, and the API Explorer along with the documentation played a crucial role in helping me explore the various queries and mutations I could make with my data.

Slash GraphQL, I choose you! [*more audible groans from the readers*]
