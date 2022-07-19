# How to Implement a GraphQL API on Top of an Existing REST API

Where do you keep your dad jokes? In a *dadabase* of course! Let’s imagine that you are a site maintainer for the world’s best dad joke database. Your app communicates with the database using a REST API that allows you to retrieve jokes and post ratings for those jokes. Visitors to your site can rate each joke they see via a simple user interface.

Recently you heard of a fancy new technology called GraphQL that provides the flexibility to request only the data that you need using a single API endpoint. It sounds neat, and you’d like to start using it in your app. But, you’d really prefer not to make any breaking changes to the existing REST API. Is it possible to support both the REST API and the GraphQL API in your app? You’re about to find out!

In this article we’ll explore what it takes to implement a GraphQL API on top of an existing REST API. This strategy allows you to start using GraphQL in legacy portions of your app without breaking any existing contracts with functionality that may still rely on the original REST API.

If you’d like to see the end result, you can find the [code for the REST API here](https://github.com/thawkin3/dad-joke-dadabase-rest-api) and the [code for the frontend and GraphQL API here](https://github.com/thawkin3/dad-joke-dadabase). Don’t forget to [visit the app](https://dad-joke-dadabase.herokuapp.com/) as well to groan at some jokes.

---

## The Initial Architecture

The app’s backend was originally built using [Node](https://nodejs.org/en/) and [JSON Server](https://github.com/typicode/json-server). JSON Server utilizes [Express](https://expressjs.com/) to provide a full REST API to a mock database generated from a simple JSON file. A separate Express server takes care of serving the static HTML, CSS, and JavaScript assets for the frontend. The frontend is implemented in vanilla JS and uses the browser’s built-in [Fetch API](https://developer.mozilla.org/en-US/docs/Web/API/Fetch_API/Using_Fetch) to make the API requests. The app is hosted on [Heroku](https://devcenter.heroku.com/) to make deployment and monitoring a breeze.

Our JSON file contains information for a few jokes as well as some ratings. It’s reproduced in full below:

{% gist https://gist.github.com/thawkin3/72ebaf757118ac8a268753c20461c355 %}

JSON Server takes that file as a starting point for the database and then implements a REST API that includes support for GET, POST, PUT, PATCH, and DELETE requests. The magic of JSON Server is that using this API really does modify the underlying JSON file, so the database is fully interactive. JSON Server can be started directly from an npm script without any additional setup, but in order to provide a little more configuration and a dynamic port, we can instead write a few lines of code like so:

{% gist https://gist.github.com/thawkin3/bb384c7c32d3299c4634a7519b8ec852 %}

You can test out our mock database by cloning the [repo for the API](https://github.com/thawkin3/dad-joke-dadabase-rest-api), running `npm install`, and then running `npm start`. If you navigate to [http://localhost:3000/jokes](http://localhost:3000/jokes) you'll see all of the jokes. Navigating to [http://localhost:3000/ratings](http://localhost:3000/ratings) will display all the ratings.

![/jokes API endpoint returns all the jokes when running the app locally](https://cdn-images-1.medium.com/max/3524/0*hKZlLEM_mzlVLnLE.png)
<figcaption>/jokes API endpoint returns all the jokes when running the app locally</figcaption>

Wonderful! We can run our app’s backend locally in the browser. Now let’s get our API hosted on Heroku. First, we need to [install the Heroku CLI](https://devcenter.heroku.com/articles/heroku-cli). After that, we can log in, create the app, push it to Heroku, and open the new app in our browser in four easy steps:

```sh
# log in to your Heroku account
heroku login

# create the Heroku app
heroku create dad-joke-dadabase-rest-api

# deploy the code to Heroku
git push heroku master

# open the Heroku app on your machine
heroku open
```

And look, now we have a publicly available API out on the web!

![/jokes API endpoint returns all the jokes when hosting the API on Heroku](https://cdn-images-1.medium.com/max/3500/0*UG1tnsWGg6C_EyoX.png)
<figcaption>/jokes API endpoint returns all the jokes when hosting the API on Heroku</figcaption>

---

## Building the User Interface

Now that we have a working REST API, we can build the frontend to consume that API and display the user interface for viewing and rating jokes. The HTML provides a shell of the page with containers into which the JavaScript will insert content for each joke.

{% gist https://gist.github.com/thawkin3/490d11976b1684152b8a978e78fe1cf1 %}

The JavaScript is shown below. The key pieces that interact with the REST API are the two fetch requests. The first fetches all of the jokes from the database by hitting the `/jokes?_embed=ratings` endpoint. The second makes a POST request to the `/ratings` endpoint to submit a new rating for each joke you rate.

{% gist https://gist.github.com/thawkin3/e7515c76f421e82eb4050c63c1db0b1c %}

![Dad joke "dadabase" user interface allows you to rate each joke](https://dev-to-uploads.s3.amazonaws.com/i/lf0remusyakuq9icvmu7.png)
<figcaption>Dad joke "dadabase" user interface allows you to rate each joke</figcaption>

---

## Setting Up Apollo Server

So, that’s the existing app architecture: a simple frontend that interacts with the database via a REST API. Now how can we begin using GraphQL? We’ll start by installing [apollo-server-express](https://www.npmjs.com/package/apollo-server-express), which is a package that allows us to use [Apollo Server](https://www.apollographql.com/docs/apollo-server/getting-started/) with Express. We'll also install the [apollo-datasource-rest](https://www.npmjs.com/package/apollo-datasource-rest) package to help us integrate the REST API with Apollo Server. Then we'll configure the server by writing the following code:

{% gist https://gist.github.com/thawkin3/ce839aba676c2e0486d3626d4361b70f %}

As you can see, we configure Apollo Server with type definitions (`typeDefs`), `resolvers`, and `dataSources`. The `typeDefs` contain the [schema](https://www.apollographql.com/docs/apollo-server/schema/schema/) for our GraphQL API. In it, we'll define types for our jokes and ratings as well as how to query and mutate them. The `resolvers` tell the server how to handle various queries and mutations and how those link to our [data sources](https://www.apollographql.com/docs/apollo-server/data/data-sources/). And finally, the `dataSources` outline how the GraphQL API relates to the REST API.

Here are the type definitions for the `Joke` and `Rating` types and how to query and mutate them:

{% gist https://gist.github.com/thawkin3/3186d8d87eec04b3bfc2d1c1a5df8846 %}

The jokes data source defines methods for calling the original REST API endpoint to create, read, update, and delete jokes from the database:

{% gist https://gist.github.com/thawkin3/612304526e8776776b42db294335b909 %}

The ratings data source looks nearly identical, but with “rating” substituted for “joke” in every instance. ([Refer to the GitHub repo](https://github.com/thawkin3/dad-joke-dadabase/blob/master/src/ratingsAPI.js) if you’d like to see the code for this.)

Finally, we set up our resolvers to show how to use the data sources:

{% gist https://gist.github.com/thawkin3/2b85dbf41e1d68dd63ecc6f16fb3762e %}

With that, we have everything in place we need in order to start using our GraphQL API through Apollo Server. To get our new frontend and GraphQL API hosted on Heroku, we’ll create and deploy a second app like so:

```sh
# create the Heroku app
heroku create dad-joke-dadabase

# deploy the code to Heroku
git push heroku master

# open the Heroku app on your machine
heroku open
```

---

## Replacing the Endpoint to Fetch Jokes

You’ll recall that we have two endpoints used by the frontend: one to fetch jokes and one to post ratings. Let’s swap out the REST API for our GraphQL API when we fetch the jokes. The code previously looked like this:

{% gist https://gist.github.com/thawkin3/4d6a5c8cbc6ec6cac3df47721b593820 %}

Now to use the GraphQL endpoint, we can write this instead:

{% gist https://gist.github.com/thawkin3/e47dbaae2c44bd8d2bd597d1b53cde7c %}

We can run the app locally now and verify that the user experience still works properly. In fact, from the user’s point of view, nothing has changed at all. But if you look at the network requests in your browser’s developer tools, you’ll see that we’re now fetching our jokes from the `/graphql` endpoint. Amazing!

![The Network tab shows a request is being made to the /graphql endpoint now](https://cdn-images-1.medium.com/max/2520/0*ketnaG9b4tR0O0O4.png)
<figcaption>The Network tab shows a request is being made to the /graphql endpoint now</figcaption>

---

## Replacing the Endpoint to Submit Ratings

One API request down, one to go! Let’s swap out the ratings submission functionality now. The code to post a new joke rating previously looked like this:

{% gist https://gist.github.com/thawkin3/f5c4b3d7781c88d731bec7d8a2954fa1 %}

To use our GraphQL API, we’ll now use the following:

{% gist https://gist.github.com/thawkin3/f2458fa32c3317374c2b62e1d43d7fdd %}

A quick test gives us some promising results. Once again, the user experience remains unchanged, but now we’re fully using the `/graphql` endpoint for both our requests!

---

## Conclusion

We did it! We successfully wrote a GraphQL API endpoint on top of an existing REST API. This allows us to use GraphQL to our heart’s content without breaking existing functionality and without modifying the original REST API. Now we can deprecate the REST API or get rid of it completely at a later date.

While our dad joke database is entirely fictional, nearly every technology company that existed prior to GraphQL’s release in 2015 will find themselves in this same position of migrating to GraphQL if and when they choose to do so. The good news is that Apollo Server is flexible enough to pull data from a variety of sources, including existing REST API endpoints.
