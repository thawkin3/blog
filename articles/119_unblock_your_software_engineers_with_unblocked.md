# Unblock Your Software Engineers with Unblocked

Developers spend weeks or even months onboarding at a new company. Getting up to speed in a new codebase takes time. During this time, the developer will have many questions (as they should)! However, those questions interrupt other team members who must stop what they’re doing to provide answers.

Most engineering organizations face the dilemma of ensuring the new developer gets the support they need without slowing down the rest of the team too much.

A culture of documentation is an excellent step in the right direction. However, this documentation is often fragmented across Slack messages, Notion and Confluence wikis, GitHub pull requests, and Jira tickets. How do you successfully navigate this endless sea of information?

An AI startup called [Unblocked](https://getunblocked.com/) is seeking to solve this problem. They’ve created a chatbot-like interface where you can ask questions and get answers to unblock yourself without interrupting anyone else. Most importantly, Unblocked can be connected to all the data sources your company uses, so the answers are tailored for you based on your actual company resources, as opposed to generic advice.

I recently tried Unblocked to see how well it could help someone like me. In this article, we’ll look at some example scenarios, the questions I asked, and the answers I received.

We’ll explore three general categories of information seeking:

1. Getting a general understanding of the architecture of a new codebase
2. Trying to understand how a feature works
3. Troubleshooting and fixing a bug

Ready to get unblocked?

---

## Example Repo

In order to use Unblocked with my current employer’s data, I would need to work through our security process and get permission. Unblocked is SOC 2 compliant and isolates customer data. For now, I decided to try Unblocked with my personal projects to get a sense of its capabilities.

I turned to one of my largest repos from my college years. I have one repo that contains dozens of projects I worked on throughout my computer science courses. Most of these projects I haven’t looked at in over eight years. If you’re like me, you’ve likely forgotten the details of the code you worked on even a few months ago, so coming back to this repo felt similar to re-onboarding!

You can [find the entire repo we will use on GitHub](https://github.com/thawkin3/201R). (Please don’t judge too harshly on the code quality. I was learning for the first time!)

---

## Scenario One: Can You Give Me an Overview of the Codebase?

As I re-acquainted myself with some of the projects in this old repo, I asked Unblocked about my repo.

I started with a very generic question: “What does this app do?”

Unblocked responded by telling me that this repo contains many different projects. It even described some of the projects to me. It knew I had apps about pet adoption, photography, fitness, and movie streaming. It also correctly identified that one of my apps was a web-based game.

![Question and answer for “What does this app do?”](https://cdn-images-1.medium.com/max/2712/0*TzMUFvmHZmJUTS1U)
<figcaption>Question and answer for “What does this app do?”</figcaption>

We were off to a great start. I asked a second question: “What languages, libraries, or frameworks does this repo use?”

Unblocked responded with many tools I listed in my main portfolio file. It correctly called out that each project used different technologies. You’ll note at the bottom of the screenshot that Unblocked cites its sources, so you know where this information comes from.

![Question and answer for “What languages, libraries, or frameworks does this repo use?”](https://cdn-images-1.medium.com/max/2716/0*L0RJ9X1ITnYxwjlb)
<figcaption>Question and answer for “What languages, libraries, or frameworks does this repo use?”</figcaption>

---

## Scenario Two: How Does This Thing Work?

Alright, that was a good enough intro for me. Next, I asked a specific question about one of my projects: a [Connect Four game](http://tylerhawkins.info/201R/ConnectFour/) built with jQuery.

Remember, the point of this trial was to see how I could use Unblocked in my day-to-day job. So, I imagined myself as a developer onboarding to a new codebase working on this game. I had a question about how the game worked. Rather than bugging one of my coworkers, I decided to ask Unblocked.

I wanted to make sure there weren’t opportunities for players to cheat in my game. I asked, “In the ConnectFour app, is it possible for a player to play two pieces in a row without waiting for the other person to take their turn?”

Unblocked’s response here was impressive. It was able to reference a specific code snippet that showed how the turn-taking behavior worked in the game.

![Question and answer for “In the ConnectFour app, is it possible for a player to play two pieces in a row without waiting for the other person to take their turn?”](https://cdn-images-1.medium.com/max/2716/0*v382-0tqaJn0CnqE)
<figcaption>Question and answer for “In the ConnectFour app, is it possible for a player to play two pieces in a row without waiting for the other person to take their turn?”</figcaption>

However, I wasn’t convinced that a player couldn’t find some way to cheat. I asked a follow-up question: “What if someone clicked the button twice really quickly before the animation finished? Then could they cheat and play two pieces at once?”

Again, I was impressed with Unblocked’s response. It highlighted another code snippet that showed how I had disabled the click handler to prevent someone from clicking twice to play two pieces at once.

It even found a closed GitHub issue referencing a similar possible problem but assured me that it has since been resolved.

![Question and answer for “What if someone clicked the button twice really quickly before the animation finished? Then could they cheat and play two pieces at once?”](https://cdn-images-1.medium.com/max/2620/0*zpGNmQNS2wpObc7k)
<figcaption>Question and answer for “What if someone clicked the button twice really quickly before the animation finished? Then could they cheat and play two pieces at once?”</figcaption>

---

## Scenario Three: Can You Help Me Fix This Bug?

Let’s switch gears and consider another scenario. Let’s imagine that I’m working in a new codebase and need help fixing a bug. I might turn to a coworker to ask for help, but this seems like something Unblocked could help with, too.

For the next few questions, I referenced a multiplayer game called [Pixel Mania](http://tylerhawkins.info:3004/login), which I built as one of my capstone projects many years ago. This game is built with JavaScript and uses web sockets to communicate information from peer to peer. In the game, each player is a dot. They move around the screen, eating food to grow in size. Players can eat one another as well. And they have to do this while avoiding obstacles that will cut their size in half.

This game works really well when two to four players are online. However, the game begins to lag when the number of players increases.

In this scenario, let’s imagine I’m a developer working on this project and am noticing these performance issues. I need some help. Who should I ask? Unblocked, of course!

My first question was, “In PixelMania, I’m seeing performance issues when a large number of players are playing. Why is that?”

Unblocked responded with a few initial thoughts. The game manages the position information of all the players, food, and balls. Unblocked theorized that the operations would take longer as the number of items increased.

It’s correct, by the way. Many of the operations in the game involve looping over all the items, looking for collisions to know if you’ve eaten a piece of food, eaten another player, or been hit by a ball. Thinking about Big O Notation, these operations are at least O(n) time.

Unblocked then suggested that some possible optimizations could be achieved by using “techniques such as rate limiting updates, using delta compression for updates, or implementing an area of interest management system where clients only receive updates for objects near their player.”

![Question and answer for “In PixelMania, I’m seeing performance issues when a large number of players are playing. Why is that?”](https://cdn-images-1.medium.com/max/2720/0*PhDg0DjPppI1uIOM)
<figcaption>Question and answer for “In PixelMania, I’m seeing performance issues when a large number of players are playing. Why is that?”</figcaption>

The nice thing about Unblocked is that you can have a conversation with it, similar to ChatGPT. I had follow-up questions and wanted to treat this like a pair programming brainstorming session.

I asked, “Could you explain more about the optimization techniques you suggested?”

Unblocked went into detail about its five suggestions:

* Rate limiting updates
* Delta compression
* Areas of interest management
* Spatial partitioning
* Optimizing data structures

![Question and answer for “Could you explain more about the optimization techniques you suggested?”](https://cdn-images-1.medium.com/max/2692/0*RQvmd5FUnTSgkUl1)
<figcaption>Question and answer for “Could you explain more about the optimization techniques you suggested?”</figcaption>

I wanted to dig in even further. I asked, “Spatial Partitioning sounds like a good approach to me. Can you give me some advice on how I would implement that in PixelMania?”

It gave me even more detailed advice! Note that it’s not just generic info about how spatial partitioning works, but it applied the advice to specific files in my app, like `game.js` and `Player.js`.

![Question and answer for “Spatial Partitioning sounds like a good approach to me. Can you give me some advice on how I would implement that in PixelMania?”](https://cdn-images-1.medium.com/max/2620/0*UeG5DSDa2z1xThuE)
<figcaption>Question and answer for “Spatial Partitioning sounds like a good approach to me. Can you give me some advice on how I would implement that in PixelMania?”</figcaption>

After that, I asked just one more question about the data structures I used: “You also mentioned Optimizing Data Structures. Are there any data structures used in PixelMania that are used inefficiently or incorrectly and could be optimized?”

Unblocked responded with some specific instances of design choices I had made and highlighted some potential drawbacks. Many of my operations were done in O(n) time, and it’s possible I could use different data structures and make better use of objects to achieve O(1) time. This could potentially improve some of the performance issues.

![Question and answer for “You also mentioned Optimizing Data Structures. Are there any data structures used in PixelMania that are used inefficiently or incorrectly and could be optimized?”](https://cdn-images-1.medium.com/max/2636/0*fGa-4JhOWpG7A22t)
<figcaption>Question and answer for “You also mentioned Optimizing Data Structures. Are there any data structures used in PixelMania that are used inefficiently or incorrectly and could be optimized?”</figcaption>

By now, I had a pretty good idea of where to go next. If I were working on this in my job, I’d be well equipped to start making changes in the code.

---

## Conclusion

Finding the right balance between asking questions and being self-reliant can be difficult. Interruptions lead to context switching, and that can be a time sink. We all want to be helpful to our coworkers, but we also need to protect our time.

AI is playing an ever-increasing role in our field as developers, and it has the capacity to significantly increase our productivity in ways we’ve never seen before.

Unblocked is one of these tools. By making it easier for developers to find the answers to their questions on their own, Unblocked enables us to get the right help we need when we need it.
