# Build a Reddit Clone with React and Dgraph Cloud

Social media apps are perfect candidates for using graph databases and GraphQL APIs. The combinations of complex data queries and relationships are endless.

Take Reddit for example. The app consists of “subreddits”, or topics. Users can create posts in these subreddits, which means that there is a many-to-one relationship between posts and subreddits. Each post belongs to exactly one subreddit, and each subreddit can contain many posts. Users can comment on posts, leading to another many-to-one relationship between posts and comments. Each comment belongs to exactly one post, and each post can have many comments. There is also a many-to-one relationship between users and posts and between users and comments. Each comment and post is made by a single user, and a single user can have many comments and posts.

In an app like Reddit, each page of the app requires different subsets of this data. Using traditional REST API endpoints could mean developing several unique endpoints each tailored to meet the needs of a specific use case. GraphQL APIs, however, are based around the idea of having a single API endpoint that developers can use to pick and choose the relevant pieces of data they need for any given page.

This article will highlight the flexibility of GraphQL and how easy using a hosted backend from [Dgraph Cloud](https://dgraph.io/cloud) makes it for frontend developers to get exactly the data they need for each page of their app.

---

## Demo App — Readit

The [demo app](http://tylerhawkins.info/reddit-clone/build/) we’ll be using throughout the rest of the article is Readit, a Reddit clone, but for book lovers (…get it?). The app is built using:

* [React](https://reactjs.org/) for the UI
* [React Router](https://reactrouter.com/web/guides/quick-start) for the client-side routing
* [Dgraph Cloud](https://dgraph.io/cloud) for the GraphQL backend and database
* [Apollo Client](https://www.apollographql.com/docs/react/) for facilitating the communication between the frontend and the backend

As noted above, the basic data types in the app are subreddits (“subreadits”, ha…), posts, comments, and users. A diagram may be helpful to visually highlight the relationships between each of these nodes that make up our graph:

![Node relationships in the graph](https://dev-to-uploads.s3.amazonaws.com/uploads/articles/n5w9upylu0fkdsb8ruwn.png)
<figcaption>Node relationships in the graph</figcaption>

The app contains routes for viewing the home page, viewing a single subreadit, viewing a specific post, and viewing an individual user. Here we see the home page:

![Readit home page](https://dev-to-uploads.s3.amazonaws.com/uploads/articles/8z80erxdbdx1wprv6s8c.png)
<figcaption>Readit home page</figcaption>

If you’d like to follow along at home or try this out on your machine, all of the [code for this app can be found on GitHub](https://github.com/thawkin3/reddit-clone). You can also [view the demo app here](http://tylerhawkins.info/reddit-clone/build/).

---

## Configuring the Dgraph Cloud Backend

Now that we have an overview of the app, let’s get started. First, we’ll create a backend with [Dgraph Cloud](https://dgraph.io/cloud). For those not familiar with this service, Dgraph is a native GraphQL graph database built for the cloud.

With a little configuration, you get a graph database as well as an API endpoint for working with your database. Dgraph’s free tier is great for learning and getting started, so that’s what I used. More advanced features like shared and dedicated clusters are available on additional paid tiers if you need to make your backend production-ready.

After signing in to our account, we click the “Launch a new backend” button, which will bring up the following setup page:

![Configuring a new Dgraph Cloud backend](https://dev-to-uploads.s3.amazonaws.com/uploads/articles/2ljfzmg9m6l6uvyl0u7y.png)
<figcaption>Configuring a new Dgraph Cloud backend</figcaption>

Since this is a demo app, we can choose the Starter option for the product type. Production apps should use a higher tier with a shared or dedicated instance though. I left my region as “us-west-2”, since that’s the region closest to me. I used “reddit-clone” for the name, but feel free to use whatever you like.

After filling out all the options, we can click “Launch” to spin up the new backend. Once the backend has been created, we’ll see an overview page with the new backend API endpoint:

![New backend API endpoint](https://dev-to-uploads.s3.amazonaws.com/uploads/articles/9903nl5zyar01i02p35p.png)
<figcaption>New backend API endpoint</figcaption>

Now it’s time to build a schema. This schema declares the various types of data that we’ll be working with in our app and storing in our database. We can either enter our schema info directly in the Schema Editor, or, for a more interactive experience, use UI Mode. Let’s use UI Mode to create our schema. The GUI helps us configure our types, their fields, and even the relationship between various types and fields.

![Creating a schema in UI Mode](https://dev-to-uploads.s3.amazonaws.com/uploads/articles/bh7q5aw7vcir8h1d3xzq.png)
<figcaption>Creating a schema in UI Mode</figcaption>

After creating the schema, we can click the “Deploy” button to make it official. If we now look at the Schema Editor view, we’ll see the resulting GraphQL snippet:

```graphql
type Comment {
  id: ID!
  commentContent: String!
  user: User! @hasInverse(field:"comments")
  post: Post! @hasInverse(field:"comments")
  voteCount: Int
}

type Post {
  id: ID!
  title: String!
  subreadit: Subreadit! @hasInverse(field:"posts")
  user: User! @hasInverse(field:"posts")
  voteCount: Int
  comments: [Comment] @hasInverse(field:"post")
}

type Subreadit {
  id: ID!
  name: String! @search(by:[exact])
  description: String
  posts: [Post] @hasInverse(field:"subreadit")
}

type User {
  id: ID!
  userName: String! @search(by:[exact])
  bio: String
  comments: [Comment] @hasInverse(field:"user")
  posts: [Post] @hasInverse(field:"user")
}
```

As you can see, each field has an associated type. For instance, the `Comment` type we created has an `id` field that contains a unique identifier generated by Dgraph Cloud. It has a `commentContent` field which contains the string text entered by the user. It has a `voteCount` field which is an integer representing the number of votes the comment has received. Finally, the `user` field references the user that wrote the comment, and the `post` field references the post on which the comment was made.

The relationship between the comment and the user is designated by the `@hasInverse` directive which tells Dgraph Cloud that the `Comment` type is linked to the `User` type by the `comments` field on the `User` type. The same is true for the relationship between the comment and the post.

You’ll also notice that a few of our fields include the `@search` directive. This allows us to filter our queries by these searchable fields. For instance, we can find a specific subreddit by filtering our query results by a specific string of text for the `name` field. The same is true when filtering user results by their `userName` field.

The next step is to populate the database with some seed data, which we can do using the API Explorer. We won’t go through all the mutations necessary to populate the data in this article, but you can [view the GraphQL snippets here](https://github.com/thawkin3/reddit-clone/tree/master/snippets). These snippets are used to create the subreadits, users, posts, and comments.

For example, here’s what I used to create a few subreadits:

```graphql
mutation AddSubreadits {
  addSubreadit(
    input: [
      {
        name: "1984"
        description: "A dystopian social science fiction novel by English novelist George Orwell." 
      },
      {
        name: "fahrenheit451"
        description: "A future American society where books are outlawed and firemen burn any that are found." 
      },
      {
        name: "thecatcherintherye"
        description: "Holden Caulfield, an angry, depressed 16-year-old, lives in an unspecified institution in California after the end of World War II." 
      }
    ]
  ) {
    subreadit {
      id
      name
      description
    }
  }
}
```

---

## Configuring the Frontend

Now that we have the backend created, we can move along to building the frontend. We’ll use [create-react-app](https://github.com/facebook/create-react-app) to generate a skeleton app as a starting point and then continue to build upon the app from there.

```sh
yarn create react-app reddit-clone
cd reddit-clone
```

Next, we’ll install `react-router-dom` so that we can do client-side routing in the single page app with React Router:

 ```sh
yarn add react-router-dom
```

Using React Router, we can create routes for each of our pages: home, subreadit, post, and user. Below is the `App` component with each of its routes:

```js
import React from 'react'
import {
  BrowserRouter as Router,
  Switch,
  Route,
  Redirect,
} from 'react-router-dom'
import { Nav } from './components/Nav'
import { HomePage } from './pages/HomePage'
import { PostPageWithRouter } from './pages/PostPage'
import { SubreaditPageWithRouter } from './pages/SubreaditPage'
import { UserPageWithRouter } from './pages/UserPage'
import './App.css'

export function App() {
  return (
    <Router basename="/reddit-clone/build">
      <div>
        <Nav />
        <main>
          <Switch>
            <Route path="/subreadit/:id">
              <SubreaditPageWithRouter />
            </Route>
            <Route path="/post/:id">
              <PostPageWithRouter />
            </Route>
            <Route path="/user/:id">
              <UserPageWithRouter />
            </Route>
            <Route path="/">
              <HomePage />
            </Route>
            <Route path="*">
              <Redirect to="/" />
            </Route>
          </Switch>
        </main>
      </div>
    </Router>
  )
}
```

Then, we’ll install a couple of packages for [Apollo Client](https://www.apollographql.com/docs/react/), which is a JavaScript state management library for working with GraphQL. While it’s possible to make requests to a GraphQL API endpoint directly using something like the `fetch` API, Apollo Client makes this process even simpler.

```
yarn add @apollo/client graphql
```

(You’ll note that we’ve installed the `graphql` package as well as the `@apollo/client` package, even though we never directly use the `graphql` package in our code. This is because `graphql` is a `peerDependency` of `@apollo/client` and is used internally to facilitate working with GraphQL in JavaScript.)

Now that we have Apollo Client installed, we can easily query data from the GraphQL backend and consume it in our React components. We can first create the Apollo client like so:

```js
import { ApolloClient, InMemoryCache } from '@apollo/client'

export const apolloClient = new ApolloClient({
  uri: 'https://reddit-clone.us-west-2.aws.cloud.dgraph.io/graphql',
  cache: new InMemoryCache(),
})
```

And then we can wrap our main `App` component in the `ApolloProvider` in the `index.js` file:

```js
import React from 'react'
import ReactDOM from 'react-dom'
import { ApolloProvider } from '@apollo/client'
import './index.css'
import { App } from './App'
import { apolloClient } from './apolloClient'

ReactDOM.render(
  <React.StrictMode>
    <ApolloProvider client={apolloClient}>
      <App />
    </ApolloProvider>
  </React.StrictMode>,
  document.getElementById('root')
)
```

---

## Home Page

Now that we have our routing set up and Apollo ready to go, we can begin building the pages for each of our routes. The home page shows a list of popular subreadits and a list of popular users.

![Readit home page](https://dev-to-uploads.s3.amazonaws.com/uploads/articles/9dnhlbszuz7t4ksynlhg.png)
<figcaption>Readit home page</figcaption>

We can query our endpoint for that information and then use Apollo to declaratively handle the `loading`, `error`, and response `data` states. The code for the `HomePage` component is reproduced in full below:

```js
import React from 'react'
import { useQuery, gql } from '@apollo/client'
import { Subreadit } from '../components/Subreadit'
import { User } from '../components/User'
import { LoadingSpinner } from '../components/LoadingSpinner'
import { ErrorMessage } from '../components/ErrorMessage'
import './HomePage.css'

const FETCH_SUBREADITS_AND_USERS = gql`
  query FetchSubreaditsAndUsers {
    querySubreadit {
      name
      description
    }
    queryUser {
      userName
      bio
      postsAggregate {
        count
      }
      commentsAggregate {
        count
      }
    }
  }
`

export const HomePage = () => {
  const { loading, data, error } = useQuery(FETCH_SUBREADITS_AND_USERS)

  return (
    <div className="homePage">
      <h1 className="srOnly">Home</h1>
      <p>
        Welcome to Readit, a community of bookworms discussing their favorite
        books! Find a subreadit to browse or a user to follow below.
      </p>
      <h2>Popular Subreadits</h2>
      {loading && <LoadingSpinner />}
      {error && <ErrorMessage />}
      {data && (
        <div className="subreaditsSection">
          {data.querySubreadit.map(subreadit => (
            <Subreadit
              key={subreadit.name}
              isPreview
              title={subreadit.name}
              description={subreadit.description}
            />
          ))}
        </div>
      )}
      <h2>Popular Users</h2>
      {loading && <LoadingSpinner />}
      {error && <ErrorMessage />}
      {data && (
        <div className="usersSection">
          {data.queryUser.map(user => (
            <User
              key={user.userName}
              isPreview
              userName={user.userName}
              bio={user.bio}
              postCount={user.postsAggregate?.count}
              commentCount={user.commentsAggregate?.count}
            />
          ))}
        </div>
      )}
    </div>
  )
}
```

Note how when retrieving the user info, we don’t need to fetch all of the user’s posts and comments. The only thing we’re interested in for the home page is how many posts and how many comments each user has. We can use the `count` field from `postsAggregate` and `commentsAggregate` to find the relevant numbers.

---

## Subreadit Page

If we click on one of the subreadits from the home page, we’ll be taken to that particular subreadit’s page where we can see all the posts under that topic.

![Readit subreadit page for the 1984 subreadit](https://dev-to-uploads.s3.amazonaws.com/uploads/articles/x9rr3wzbvsjh9fbdxq1s.png)
<figcaption>Readit subreadit page for the 1984 subreadit</figcaption>

On this page, we need the data for the subreadit name and description, just like we did on the home page. We now also need to fetch all of the posts that are part of this subreadit. For each post, we need the post title, the number of votes and comments, and the username of the user who posted it. We don’t need the actual comments yet though since they’re not displayed on this page.

Here’s the code for the `SubreaditPage` component:

```js
import React from 'react'
import { useQuery, gql } from '@apollo/client'
import { withRouter } from 'react-router-dom'
import { Subreadit } from '../components/Subreadit'
import { Post } from '../components/Post'
import { LoadingSpinner } from '../components/LoadingSpinner'
import { ErrorMessage } from '../components/ErrorMessage'
import './SubreaditPage.css'

export const SubreaditPage = ({ match }) => {
  const FETCH_SUBREADIT_WITH_POSTS = gql`
    query FetchSubreaditWithPosts {
      querySubreadit(filter: { name: { eq: "${match.params.id}" } }) {
        name
        description
        posts {
          id
          title
          user {
            userName
          }
          voteCount
          commentsAggregate {
            count
          }
        }
      }
    }
  `

  const { loading, data, error } = useQuery(FETCH_SUBREADIT_WITH_POSTS)

  return (
    <div className="subreaditPage">
      {loading && <LoadingSpinner />}
      {error && <ErrorMessage />}
      {data &&
        (data?.querySubreadit.length ? (
          <>
            <Subreadit
              title={data.querySubreadit[0].name}
              description={data.querySubreadit[0].description}
            />
            <h2>Posts</h2>
            <div className="postsSection">
              {data.querySubreadit[0].posts.length ? (
                data.querySubreadit[0].posts.map(post => (
                  <Post
                    key={post.id}
                    isPreview
                    isOnSubreaditPage
                    id={post.id}
                    title={post.title}
                    voteCount={post.voteCount}
                    commentCount={post.commentsAggregate?.count}
                    subreaditName={data.querySubreadit[0].name}
                    userName={post.user.userName}
                  />
                ))
              ) : (
                <p>No posts yet!</p>
              )}
            </div>
          </>
        ) : (
          <ErrorMessage />
        ))}
    </div>
  )
}

export const SubreaditPageWithRouter = withRouter(SubreaditPage)
```

---

## Post Page

Once we’ve found an interesting post we’d like to view, we can click on the link to view the individual post page. This page shows us the original post as well as all of the comments on the post.

![Readit post page for Oscar’s post](https://dev-to-uploads.s3.amazonaws.com/uploads/articles/1v90yj5sw4x4sfvt7sgb.png)
<figcaption>Readit post page for Oscar’s post</figcaption>

Here we need all the same post data that we did on the subreadit page, but now we also need to know the subreadit it was posted on, and we need all of the comments on the post. For each comment, we need to know the username for the user who posted it, what the actual comment content was, and how many votes it has.

The code for the `PostPage` looks like this:

```js
import React from 'react'
import { useQuery, gql } from '@apollo/client'
import { withRouter } from 'react-router-dom'
import { Post } from '../components/Post'
import { Comment } from '../components/Comment'
import { LoadingSpinner } from '../components/LoadingSpinner'
import { ErrorMessage } from '../components/ErrorMessage'
import './PostPage.css'

export const PostPage = ({ match }) => {
  const FETCH_POST_WITH_COMMENTS = gql`
    query FetchPostWithComments {
      getPost(id: "${match.params.id}") {
        title
        user {
          userName
        }
        subreadit {
          name
        }
        voteCount
        commentsAggregate {
          count
        }
        comments {
          commentContent
          voteCount
          user {
            userName
          }
        }
      }
    }
  `

  const { loading, data, error } = useQuery(FETCH_POST_WITH_COMMENTS)

  return (
    <div className="postPage">
      {loading && <LoadingSpinner />}
      {error && <ErrorMessage />}
      {data &&
        (data.getPost ? (
          <>
            <Post
              title={data.getPost.title}
              voteCount={data.getPost.voteCount}
              commentCount={data.getPost.commentsAggregate?.count}
              subreaditName={data.getPost.subreadit.name}
              userName={data.getPost.user.userName}
            />
            <h2>Comments</h2>
            <div className="commentsSection">
              {data.getPost.comments.length ? (
                data.getPost.comments.map(comment => (
                  <Comment
                    key={comment.commentContent}
                    isOnPostPage
                    commentContent={comment.commentContent}
                    voteCount={comment.voteCount}
                    userName={comment.user.userName}
                  />
                ))
              ) : (
                <p>No comments yet!</p>
              )}
            </div>
          </>
        ) : (
          <ErrorMessage />
        ))}
    </div>
  )
}

export const PostPageWithRouter = withRouter(PostPage)
```

---

## User Page

Finally, if we decide to view a user’s profile, we can see all of their posts and comments they’ve made.

![Readit user page for Oscar Martinez](https://dev-to-uploads.s3.amazonaws.com/uploads/articles/7wj4i2tf3dq7ze5ts9pp.png)
<figcaption>Readit user page for Oscar Martinez</figcaption>

This page should show the user’s username, bio, number of posts, and number of comments. We also need all of their posts and all of their comments. On each post, we need to know the subreadit it was posted on, the post title, as well as the number of votes and comments. For each comment, we need to know which post it was a comment on, what the comment content was, and the number of votes it’s received.

The code for the `UserPage` is below:

```js
import React from 'react'
import { useQuery, gql } from '@apollo/client'
import { withRouter } from 'react-router-dom'
import { User } from '../components/User'
import { Post } from '../components/Post'
import { Comment } from '../components/Comment'
import { LoadingSpinner } from '../components/LoadingSpinner'
import { ErrorMessage } from '../components/ErrorMessage'
import './UserPage.css'

export const UserPage = ({ match }) => {
  const FETCH_USER = gql`
    query FetchUser {
      queryUser(filter: { userName: { eq: "${match.params.id}" } }) {
        userName
        bio
        posts {
          id
          title
          user {
            userName
          }
          subreadit {
            name
          }
          voteCount
          commentsAggregate {
            count
          }
        }
        postsAggregate {
          count
        }
        comments {
          id
          commentContent
          voteCount
          user {
            userName
          }
          post {
            title
            id
          }
        }
        commentsAggregate {
          count
        }
      }
    }
  `

  const { loading, data, error } = useQuery(FETCH_USER)

  return (
    <div className="userPage">
      {loading && <LoadingSpinner />}
      {error && <ErrorMessage />}
      {data &&
        (data?.queryUser.length ? (
          <>
            <User
              userName={data.queryUser[0].userName}
              bio={data.queryUser[0].bio}
              postCount={data.queryUser[0].postsAggregate?.count}
              commentCount={data.queryUser[0].commentsAggregate?.count}
            />
            <h2>Posts</h2>
            <div className="postsSection">
              {data.queryUser[0].posts.length ? (
                data.queryUser[0].posts.map(post => (
                  <Post
                    key={post.id}
                    isPreview
                    isOnUserPage
                    id={post.id}
                    title={post.title}
                    voteCount={post.voteCount}
                    commentCount={post.commentsAggregate?.count}
                    subreaditName={post.subreadit.name}
                    userName={post.user.userName}
                  />
                ))
              ) : (
                <p>No posts yet!</p>
              )}
            </div>
            <h2>Comments</h2>
            <div className="commentsSection">
              {data.queryUser[0].comments.length ? (
                data.queryUser[0].comments.map(comment => (
                  <Comment
                    key={comment.id}
                    isOnUserPage
                    postTitle={comment.post.title}
                    postId={comment.post.id}
                    commentContent={comment.commentContent}
                    voteCount={comment.voteCount}
                    userName={comment.user.userName}
                  />
                ))
              ) : (
                <p>No comments yet!</p>
              )}
            </div>
          </>
        ) : (
          <ErrorMessage />
        ))}
    </div>
  )
}

export const UserPageWithRouter = withRouter(UserPage)
```

This page is by far the most complex, as we need to query more than just summary data or aggregate count data.

---

## Conclusion

As we’ve seen, each page in our app requires unique portions of data. Some pages need only high-level summaries, like the number of comments or posts a user has made. Other pages need more in-depth results, like the actual comments and actual posts. Depending on the page, you may need more or less information.

The benefit of using GraphQL and Dgraph Cloud is the flexibility in querying exactly the data we need for each page — no more and no less. For each request, we used the same single API endpoint but requested different fields. This greatly simplifies the development work as we don’t need to create a new backend endpoint for each page. (Or worse, create a single endpoint that retrieves a monstrous payload of data we then have to sift through to find the minor subset of data that we need.)

GraphQL makes it easy to quickly and painlessly request exactly the right data exactly when you need it.
