# Hacking the Hackathon

![Flappy Bird knockoff game](https://cdn-images-1.medium.com/max/2000/0*svgWxV8S9tfBU8Yb)*Flappy Bird knockoff game*

Developers love to hack, but software engineering as a profession can actually be quite mundane. We attend hours of meetings, give status reports, fix bugs, maintain legacy software, and work on less-than-exciting projects. Not all of us are out there changing the world and building the next hottest new technology.

To satisfy our desire to build, we participate in hackathons, where the primary goal is simply to build something cool. We hack it together, so to speak. Smart companies regularly host internal hackathons, and that’s where some of their best ideas are generated. Sometimes a company will sponsor a public-facing hackathon as well, in which anyone can participate. Winners of these hackathons are often rewarded with prize money, swag, and the respect of their peers.

I recently came across this pretty awesome [Ethereum Hackathon Survival Guide](https://consensys.net/developers/ultimate-hackathon-survival-guide/) from ConsenSys (the company that makes web3 tools such as [Infura](https://www.infura.io/), [MetaMask](https://metamask.io/), [Truffle](https://trufflesuite.com/), and [Diligence](https://consensys.net/diligence/)). The guide covers workflows, prepping for a web3 hackathon, best practices, Ethereum resources to have with you during the hackathon, etc.

It’s a thorough guide. And it got me thinking about *my own* past hackathon experiences. I haven’t actually participated in any public hackathons yet (having kids will do that to you), but I’ve done plenty of company-wide hackathons over the last nine years of my career.

For those looking to make the most of their next hackathon, read the above guide for web3 hackathon tips — and then, in addition to that, here are my words of advice.

---

## Explore Something New

Hackathons are a great time to explore something new. When choosing a project for your next hackathon, don’t just do what you already do every day at work. Find something new! Hackathons are a time to branch out and to get out of your comfort zone.

Is there a framework, library, or API you’ve been dying to try out? Do you have a harebrained idea that you just haven’t had time to start building? Is there a problem that’s been nagging you and keeping you up at night? Is there something you’d like to simplify, either in your personal life or at work? These can all serve as inspiration when brainstorming topic ideas.

When I wanted to learn GraphQL, I built [Puppy Playdate](http://tylerhawkins.info/puppy-playdate/build/), the [Tinder app for dogs](https://github.com/thawkin3/puppy-playdate).

![Puppy Playdate, the Tinder app for dogs](https://cdn-images-1.medium.com/max/3200/0*3xd6YfwQrjLaEJvj)*Puppy Playdate, the Tinder app for dogs*

When I was first learning about web sockets and WebRTC, I built [Chat Sockets](http://tylerhawkins.info:3017/), a real-time [chat app](https://github.com/thawkin3/chat-sockets) that included a Giphy integration, just like Slack has.

![Chat Sockets, a WebRTC chat app](https://cdn-images-1.medium.com/max/2700/0*pPoeCiqRpFFHODOF)*Chat Sockets, a WebRTC chat app*

And when I wanted to learn more about machine learning and similarity search, I built a [plagiarism checker](https://github.com/thawkin3/plagiarism-checker) backed by the Pinecone SDK.

![Plagiarism checker built with the Pinecone SDK](https://cdn-images-1.medium.com/max/2000/0*prFimfgVXrQB5umX)*Plagiarism checker built with the Pinecone SDK*

Keep in mind that your project doesn’t have to be production ready by the end of the hackathon. The idea is to build a proof of concept you can then explore further after the hackathon is over.

---

## Meet New People

Some developers participate in hackathons with the intent to win. They’re super competitive and have their sights set on that first-prize money. If that doesn’t sound like you, don’t worry — your goal doesn’t have to be to win.

Your goal might just be to experience what a hackathon is like. The high energy or late night might be just what you needed to rekindle the spark and remind you why you love programming.

Or, your goal might be to network. Hackathons are a great place to meet new people and to learn from those around you. You might be looking forward to getting to know your teammates better. Or perhaps there’s someone in your group who you look up to and have always wanted to work alongside.

Just like pair programming or mob programming, hackathons allow you to peer a little deeper into the minds of your colleagues to see how they work. You may pick up a few tips and tricks along the way that will boost your productivity for years to come.

A couple years ago, I did a week-long mob programming exercise with my teammates at work. We built a minesweeper game together using Tailwind CSS for the first time. During that week, I showed my teammates how to implement some build tools like Prettier, Commitizen, lint-staged, and Husky. One of my teammates showed me how to create static sites hosted on GitLab Pages. It was a win-win for everyone!

In a subsequent hackathon, I later turned that project into a micro frontend that lived alongside an ecosystem of other micro frontends in our larger company app.

![Minesweeper, a React app turned into a microfrontend](https://cdn-images-1.medium.com/max/3200/0*kXP5DAsoQXzzxLj_)*Minesweeper, a React app turned into a microfrontend*

---

## Come Prepared

You should have an idea in mind before the hackathon actually begins. Some hackathons require you to submit your project idea beforehand, but others do not. Regardless, you need to have a rough idea of what you want to build. The first “pro tip” from the Ethereum hackathon guide is “[have a bulletproof plan](https://consensys.net/developers/ultimate-hackathon-survival-guide/#:~:text=Have%20a%20Bulletproof%20Plan)” — and I agree. If you don’t come prepared, you’ll spend the first half of the hackathon just figuring out what you actually want to build, and by then you’ll have lost a lot of time.

For my last company hackathon (or “Garage Week”, as we call it), I switched between a few topics. I first added some excitement to our product by including a burst of confetti whenever a user marks a task as complete. Gotta get that hit of dopamine! I then explored the process of bootstrapping a brand new micro frontend. After that I began implementing drag-and-drop functionality for reordering columns within our complex tables. It was a fun week, and I accomplished a lot, but the scope of each project was much smaller than many of the other project submissions.

The same advice applies to finding your team. Make sure you know in advance who you’ll be working with. You may want to start recruiting people to help you long before the actual hackathon starting date.

Finally, it’s important that you know what your goals are for the hackathon, which we’ve touched on briefly already. Are you here to win? To learn something new? To meet people? All of the above? And if you’re in it to win it, do you know the rules of the contest or the criteria by which submissions will be judged?

---

## Past Projects

If you need any more inspiration for your next hackathon, I have a [web portfolio](http://tylerhawkins.info/201R/) full of goofy ideas you can check out.

Need a [Flappy Bird knockoff](http://tylerhawkins.info/201R/QrappyBird/#/)? I’ve got you covered.

![Flappy Bird knockoff game](https://cdn-images-1.medium.com/max/2000/0*svgWxV8S9tfBU8Yb)*Flappy Bird knockoff game*

How about a [Corporate BS Generator](http://tylerhawkins.info:3009/) to help you better communicate with upper management?

![Corporate BS Generator](https://cdn-images-1.medium.com/max/2000/0*sHQYCVhtFIUa5ytq)*Corporate BS Generator*

Or perhaps you’d like to play an [addicting ninja cat game](http://tylerhawkins.info/stacks-on-stacks-on-stacks/)?

![Stacks on Stacks on Stacks, a ninja cat game](https://cdn-images-1.medium.com/max/2000/0*GAtw1dj8TpS0KD31)*Stacks on Stacks on Stacks, a ninja cat game*

You can find all of the code repos in [my GitHub profile](https://github.com/thawkin3) as well.

---

## Wrapping Up

Hackathons can be a great experience, and one of the world’s largest hackathons is about to happen at [ETHDenver](https://www.ethdenver.com/buidlweek) very soon: February 24th to March 1st this year. If it’s your first one, don’t stress too much. Come prepared, know what your goals are, meet some people, and learn something new. But most of all, have fun.
