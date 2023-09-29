# Build Quicker with Zipper: Building a Ping Pong Ranking App using TypeScript functions

Seasoned software engineers long for the good old days when web development was simple. You just needed a few files and a server to get up and running. No complicated infrastructure, no endless amount of frameworks and libraries, no build tools. Just some ideas and some code hacked together to make an app come to life.

Whether or not this romanticized past was actually as great as we think it was, developers today agree that software engineering has gotten complicated. There are too many choices with too much setup involved.

In response to this sentiment, many products are providing off-the-shelf starter kits and zero config toolchains to try to abstract away the complexity of software development.

One such startup is [Zipper](https://zipper.dev/), a company which offers an online IDE where you can create applets that run as serverless TypeScript functions in the cloud. With Zipper, you don’t have to spend time worrying about your toolchain — you can just start writing code and deploy your app within minutes.

Today, we’ll be looking at a ping pong ranking app I built — once in 2018 with jQuery, MongoDB, Node.js, and Express; and once in 2023 with Zipper. We’ll examine the development process for each and see just how easy it is to build a powerful app using Zipper.

---

## Backstory

First, a little context: I love to play ping pong. Every office in which I’ve worked has had a ping pong table, and for many years ping pong was an integral part of my afternoon routine. It’s a great game to relax, blow off some steam, strengthen friendships with coworkers, and reset your brain for a half hour.

Those who played ping pong every day began to get a feel for who was good and who wasn’t. People would talk. A handful of people were known as the best in the office, and it was always a challenge to take them on.

Being both highly competitive and a software engineer, I wanted to build an app to track who was the best ping pong player in the office. This wouldn’t be for bracket-style tournaments, but just for recording the games that were played every day by anybody. With that, we’d have a record of all the games played, and we’d be able to see who was truly the best.

This was 2018, and I had a background in the MEAN/MERN stack (MongoDB, Express, Angular, React, and Node.js) and experience with jQuery before that. After dedicating a week’s worth of lunch breaks and nights to this project, I had a working ping pong ranking app. I didn’t keep close track of my time spent working on the app, but I’d estimate it took about 10–20 hours to build.

Here’s what that version of the app looked like.

There was a login and signup page:

![Office Competition Ranking System — Home page](https://dev-to-uploads.s3.amazonaws.com/uploads/articles/01agf8kmfn9fczjj8kyz.png)
<figcaption>Office Competition Ranking System — Home page</figcaption>

The login page asked for your username and password to authenticate:

![Office Competition Ranking System — Login page](https://dev-to-uploads.s3.amazonaws.com/uploads/articles/x1ff602kmps8bkrfwn9v.png)
<figcaption>Office Competition Ranking System — Login page</figcaption>

Once authenticated, you could record your match by selecting your opponent and who won:

![Office Competition Ranking System — Record game results page](https://dev-to-uploads.s3.amazonaws.com/uploads/articles/xykzqjsyy5486y23y8w6.png)
<figcaption>Office Competition Ranking System — Record game results page</figcaption>

You could view the leaderboard to see the current office rankings. I even included an [Elo rating](https://en.wikipedia.org/wiki/Elo_rating_system) algorithm like they use in chess:

![Office Competition Ranking System — Leaderboard page](https://dev-to-uploads.s3.amazonaws.com/uploads/articles/lxq1c0jekygyq8y1kntc.png)
<figcaption>Office Competition Ranking System — Leaderboard page</figcaption>

Finally, you could click on any of the players to see their specific game history of wins and losses:

![Office Competition Ranking System — Player history page](https://dev-to-uploads.s3.amazonaws.com/uploads/articles/cyi0sd21tv8fmz4mi4oz.png)
<figcaption>Office Competition Ranking System — Player history page</figcaption>

That was the app I created back in 2018 with jQuery, MongoDB, Node.js, and Express. And, I hosted it on an AWS EC2 server.

Now let’s look at my experience recreating this app in 2023 using only Zipper.

---

## About Zipper

Zipper is an online tool for creating applets. It uses TypeScript and Deno, so JavaScript and TypeScript users will feel right at home. You can use Zipper to build web services, web UIs, scheduled jobs, and even Slack or GitHub integrations. Zipper even includes auth.

In short, what I find most appealing about Zipper is how quickly you can take an idea from conception to execution. It’s perfect for side projects or internal-facing apps to quickly improve a business process.

---

## Demo app

Here’s the ping pong ranking app I built with Zipper in just three hours. And that’s including time reading through the docs and getting up to speed with an unfamiliar platform!

First, the app requires authentication. In this case, I’m requiring users to sign in to their Zipper account:

![Ping pong ranking app — Authentication page](https://dev-to-uploads.s3.amazonaws.com/uploads/articles/71x0vd8omjeiejy22h3b.png)
<figcaption>Ping pong ranking app — Authentication page</figcaption>

Once authenticated, users can record a new ping pong match:

![Ping pong ranking app — Record a new match page](https://dev-to-uploads.s3.amazonaws.com/uploads/articles/v7bc7pyhf0ig9yiyn4kr.png)
<figcaption>Ping pong ranking app — Record a new match page</figcaption>

They can view the leaderboard:

![Ping pong ranking app — Leaderboard page](https://dev-to-uploads.s3.amazonaws.com/uploads/articles/1tgy17ag4xvdhtgb8fcq.png)
<figcaption>Ping pong ranking app — Leaderboard page</figcaption>

And they can view the game history for any individual player:

![Ping pong ranking app — Player history page](https://dev-to-uploads.s3.amazonaws.com/uploads/articles/kn8eght5vckayo2dy3ax.png)
<figcaption>Ping pong ranking app — Player history page</figcaption>

Not bad! The best part is that I didn’t have to create any of the UI components for this app. All the inputs and table outputs were handled automatically. And, the auth was created for me just by checking a box in the app settings!

You can [find the working app](https://ping-pong-ranking.zipper.run/) hosted publicly on Zipper.

Ok, now let’s look at how I built this.

---

## Creating a new Zipper app

First, I created my Zipper account by authenticating with GitHub. Then, on the main dashboard page, I clicked the Create Applet button to create my first applet.

![Create your first applet](https://dev-to-uploads.s3.amazonaws.com/uploads/articles/xwsn6llvdu72h8h12an3.png)
<figcaption>Create your first applet</figcaption>

Next, I gave my applet a name, which became its URL. I also chose to make my code public and required users to sign in before they could run the applet.

![Applet configuration](https://dev-to-uploads.s3.amazonaws.com/uploads/articles/2rifdwuzqk5yqgq3ohfu.png)
<figcaption>Applet configuration</figcaption>

Then I chose to generate my app using AI, mostly because I was curious how it would turn out! This was the prompt I gave it:

“I’d like to create a leaderboard ranking app for recording wins and losses in ping pong matches. Users should be able to log into the app. Then they should be able to record a match showing who the two players were and who won and who lost.

“Users should be able to see the leaderboard for all the players, sorted with the best players displayed at the top and the worst players displayed at the bottom.

“Users should also be able to view a single player to see all of their recorded matches and who they played and who won and who lost.”

![Applet initialization](https://dev-to-uploads.s3.amazonaws.com/uploads/articles/0um1zl3wjb423dw6odcg.png)
<figcaption>Applet initialization</figcaption>

I might need to get better at prompt engineering, because the output didn’t include all the features or pages I wanted.

The AI-generated code included two files: a generic “hello world” main.ts file, and a view-player.ts file for viewing the match history of an individual player.

![main.ts file generated by AI](https://dev-to-uploads.s3.amazonaws.com/uploads/articles/sdesbs2abyllywt8torp.png)
<figcaption>main.ts file generated by AI</figcaption>

![view-player.ts file generated by AI](https://dev-to-uploads.s3.amazonaws.com/uploads/articles/19pjsisi4nzehslx9grb.png)
<figcaption>view-player.ts file generated by AI</figcaption>

So, the app wasn’t perfect from the get-go, but it was enough to get started.

---

## Writing the ping pong app code

I knew that Zipper would handle the authentication page for me, so that left three pages to write:

1. A page to record a ping pong match
2. A page to view the leaderboard
3. A page to view an individual player’s game history

---

## Record a new ping pong match

I started with the form to record a new ping pong match. Below is the full main.ts file. We’ll break it down line by line right after this.

{% gist https://gist.github.com/thawkin3/5f8e4372c9143717f9b952482b7370fa %}

Each file in Zipper exports a [handler](https://zipper.dev/docs/building-applets/handler-functions) function that accepts [inputs](https://zipper.dev/docs/building-applets/accepting-inputs) as a parameter. Each of the inputs becomes a form in UI, with the input type being determined by its TypeScript type that you give it.

After doing some input validation to ensure that the form is correctly filled out, I stored the match info in [Zipper’s key-value storage](https://zipper.dev/docs/building-applets/storage). Each Zipper applet gets its own storage instance that any of the files in your applet can access. Because it’s a key-value storage, objects work nicely for values since they can be serialized and deserialized as JSON, all of which Zipper handles for you when reading from and writing to the database.

At the bottom of the file, I’ve added a HandlerConfig to add some title and instruction text to the top of the page in the UI.

With that, the first page is done.

![Ping pong ranking app - Record a new match page](https://dev-to-uploads.s3.amazonaws.com/uploads/articles/v7bc7pyhf0ig9yiyn4kr.png)
<figcaption>Ping pong ranking app - Record a new match page</figcaption>

---

## Leaderboard

Next up is the leaderboard page. I’ve reproduced the leaderboard.ts file below in full:

{% gist https://gist.github.com/thawkin3/022307c9169c9965fdc148988093f562 %}

This file contains a lot more TypeScript types than the first file did. I wanted to make sure my data structures were nice and explicit here.

After that, you see our familiar handler function, but this time without any inputs. That’s because the leaderboard page doesn’t need any inputs; it just displays the leaderboard.

We get all of our recorded matches from the database, and then we manipulate the data to get it into an array format of our liking. Then, simply by returning the array, Zipper creates the table UI for us, even including search functionality and column sorting. No UI work needed!

Finally, at the bottom of the file, you’ll see a description setup that’s similar to the one on our main page. You’ll also see the run: true property, which tells Zipper to run the handler function right away without waiting for the user to click the Run button in the UI.

![Ping pong ranking app - Leaderboard page](https://dev-to-uploads.s3.amazonaws.com/uploads/articles/1tgy17ag4xvdhtgb8fcq.png)
<figcaption>Ping pong ranking app - Leaderboard page</figcaption>

---

## Player history

Alright, two down, one to go. Let’s look at the code for the view-player.ts file, which I ended up renaming to player-history.ts:

{% gist https://gist.github.com/thawkin3/9a7d3bba991755b5b83bb4f31095d659 %}

The code for this page looks a lot like the code for the leaderboard page. We include some types for our data structures at the top. Next we have our handler function which accepts an input for the player ID that we want to view.

From there, we fetch all the recorded matches and filter them for only matches in which this player participated.

After that, we manipulate the data to get it into an acceptable format to display, and we return that to the UI to get another nice auto-generated table.

![Ping pong ranking app - Player history page](https://dev-to-uploads.s3.amazonaws.com/uploads/articles/kn8eght5vckayo2dy3ax.png)
<figcaption>Ping pong ranking app - Player history page</figcaption>

---

## Conclusion

That’s it! With just three handler functions, we’ve created a working app for tracking our ping pong game history. This app does have some shortcomings that we could improve, but we’ll leave that as an exercise for the reader.

For example, it would be nice to have a dropdown of users to choose from when recording a new match, rather than entering each player’s ID as text. Maybe we could store each player’s ID in the database and then display those in the UI as a [dropdown input type](https://zipper.dev/docs/building-applets/accepting-inputs).

Or, maybe we’d like to turn this into a [Slack integration](https://zipper.dev/docs/connecting-to-other-tools/slack) to allow users to record their matches directly in Slack. That’s an option too!

While my ping pong app isn’t perfect, I hope the takeaway here is how easy it is to get up and running with a product like Zipper. You don’t have to spend time agonizing over your app’s infrastructure when you have a simple idea that you just want to see working in production. Just get out there, start building, and deploy!
