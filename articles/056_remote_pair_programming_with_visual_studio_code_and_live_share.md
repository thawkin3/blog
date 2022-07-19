# Remote Pair Programming with Visual Studio Code and Live Share

Remote work has killed pair programming. Or has it? If anything, it certainly presents an interesting challenge for pair programming when the two developers involved are no longer sitting side by side in the same room. But can you still successfully pair program in this world of remote work?

In this article, we’ll look at some of the benefits of pair programming and how you can use technology like the [Visual Studio Code Live Share extension](https://marketplace.visualstudio.com/items?itemName=MS-vsliveshare.vsliveshare) to continue to pair program remotely with your colleagues.

---

## What is Pair Programming?

Let’s start with a quick definition. We’ll define pair programming as a development technique in which two developers write code at a single workstation. Typically this means only one computer and one keyboard. The developer at the keyboard “drives” while the developer without the keyboard “observes” or “navigates”.

The driver provides the hands that write the code. The observer-navigator provides the strategic decision-making for which files to look at and which problems to tackle. The observer-navigator also reviews the code that is being written and helps catch simple typos and other mistakes in real time. The two developers switch roles at some agreed-upon interval, typically every 30 minutes or so.

---

## Benefits of Pair Programming

Now, why on earth would anyone do this? Two developers working on the same thing at the same time? Surely that’s a waste of time. Couldn’t you produce twice as much code if the developers were working independently on their own machines?

These are certainly valid arguments and ones worth examining. So let’s look at the benefits of pair programming. What good do we get from two developers working together?

### Teaching and Mentoring Opportunities

First off, pair programming provides a perfect teaching opportunity. It’s rare that the two developers are at exactly the same skill level. Instead, one is usually more senior and the other more junior. Pair programming allows the senior engineer to mentor the junior engineer as they work on real problems together. The junior engineer can learn how the senior engineer reasons through a problem, how they organize their code, how they name their variables, and how they troubleshoot issues when things go wrong. It’s also important to remember that the mentoring and teaching is a two-way street.

And seniors, don’t underestimate your junior engineers. They may have a fresh perspective on the problems at hand too. And who knows, you both may end up learning each other’s favorite IDE shortcuts and tricks.

### Knowledge Sharing

Second, pair programming allows you to share knowledge regarding how the system is architected. When developers work in individual silos, you’ll often have one engineer code a feature from start to finish. Even though code reviews are done throughout the development process, having a single engineer be the main point of contact on a project results in that one person having the most understanding and knowledge of the final solution. Pair programming, on the other hand, results in two developers both gaining a deeper understanding of their code.

### Error Prevention

Third, pair programming makes it easy to catch simple mistakes and typos faster with a second pair of eyes. You’ll be surprised how much faster the two of you can find the missing semicolon or why your code won’t compile when you have a second person watching every character you type.

### Interpersonal Communication Skill Improvement

Fourth, pair programming is a great opportunity for engineers to develop their interpersonal skills. Can you talk through a solution with another person? Can you bounce ideas off each other? Can you clearly communicate your ideas in a way that another engineer can understand? Can you tactfully guide someone away from a poor solution if you see a better one? And perhaps most importantly, how do you respond to feedback and correction on your own ideas? These soft skills are valuable for engineers at any level.

### Defect Reduction and Increased Productivity

Finally, and perhaps most interestingly, [studies have shown](https://collaboration.csc.ncsu.edu/laurie/Papers/XPSardinia.PDF) that when engineers pair program they write better code with fewer bugs. The extra development-time cost of having two people work together is estimated at 15%. But when you think about all the time saved through fewer customer support issues and less time spent debugging, you may find that pair programming actually *increases* productivity!

---

## Pair Programming Remotely

So, we’ve established that pair programming has several benefits. But how do we pair program remotely? COVID-19 has forced many companies to adopt a remote work policy, even those companies that may have initially been very averse to the idea of remote work.

As for me, I’ve typically worked at the office, but I’ve been working remotely since March 2020, and I’m loving it! Zoom meetings and Slack messages have become my default methods of communication. But what about for pair programming?

One option might look something like this:

1. Dev 1 and Dev 2 get on a video call together
2. Dev 1 starts as the driver, and Dev 2 starts as the observer-navigator
3. Dev 1 shares their screen and starts writing code while Dev 2 navigates
4. When it’s time to switch roles, Dev 1 commits what they’ve written and pushes it to a branch on the remote repository
5. Dev 2 pulls down the latest code from the branch
6. Dev 2 shares their screen and starts writing code while Dev 1 navigates
7. When it’s time to switch roles again, Dev 2 commits what they’ve written and pushes it to the branch
8. Repeat steps 3–7 until the pair programming session is over

Those steps look pretty similar to what you might see when pair programming in-person, with the exception of the video call and screen sharing. The other notable difference is the need to commit the code and push it up in order for the other person to have the latest code. When pair programming in person, you don’t have that same challenge, as you’re both working on the same computer.

So that begs the question: Is there any way to collaborate without having to commit your code in between switching roles?

The answer: Yes, there is! Visual Studio Code (VS Code) has a handy extension called Live Share that helps facilitate the remote pair programming process. Live Share allows you to share your code while others follow along in VS Code on their own computer. But that’s not all — you can even share your localhost server or a terminal instance!

---

## How to Use VS Code Live Share

You can find the VS Code Live Share extension for free on the [Visual Studio marketplace](https://marketplace.visualstudio.com/items?itemName=MS-vsliveshare.vsliveshare). After clicking the Install button and then restarting your VS Code application, you’re ready to start collaborating! They have excellent installation instructions and a quick start guide on their marketplace listing at the same link as above, so I’d highly recommend following along there.

To experiment with this extension, [I created a simple Node.js project](https://github.com/thawkin3/vs-code-live-share-demo) hosted on [Heroku](https://www.heroku.com/). I opened the project directory in VS Code on my own machine, clicked the Live Share icon on the left-hand side of the screen, and then clicked Share to create a new collaboration session.

![Click the Share button to start a new collaboration session](https://dev-to-uploads.s3.amazonaws.com/uploads/articles/q9ekasvti72kvf9p3rcr.png)
<figcaption>Click the Share button to start a new collaboration session</figcaption>

Since this was my first time using the extension, I was prompted to sign in, which I did through GitHub.

![Sign in with your GitHub or Microsoft account](https://dev-to-uploads.s3.amazonaws.com/uploads/articles/wcoyshlifqy6z9uy5ip2.png)
<figcaption>Sign in with your GitHub or Microsoft account</figcaption>

After signing in, I was given a collaboration session URL which I could then share with up to 30 people at a time.

![Send the collaboration session URL to anyone you’d like](https://dev-to-uploads.s3.amazonaws.com/uploads/articles/4ugme4jew2qdfkwjqyyf.png)
<figcaption>Send the collaboration session URL to anyone you’d like</figcaption>

I sent this URL to myself on a second computer and signed in as an anonymous John Doe user. After doing that, I received a notification on my first machine that John Doe was attempting to join my collaboration session. At that point, I could grant read-only access to let him follow along, read-write access to let him write code along with me, or reject his request and keep him out of the session. I clicked the “Accept read-write” button.

![Accept or reject requests to join your session](https://dev-to-uploads.s3.amazonaws.com/uploads/articles/7rkl5ldvpvswjhlgt2tu.png)
<figcaption>Accept or reject requests to join your session</figcaption>

Over on my second computer, once access was granted, VS Code pulled open the project directory on my machine. This is really neat! I didn’t even need to pull down the code locally or clone the GitHub repo.

Once multiple people are in the session, each person’s cursor is shown, and you can collaborate in real time, much like editing a Google doc together. As you can see below, I was able to write a line of code from both computers, and I can see two cursors on the screen.

![Multiple cursors can be seen in VS Code, one for each participant](https://dev-to-uploads.s3.amazonaws.com/uploads/articles/6h9gz5axert3k3y7dpcp.png)
<figcaption>Multiple cursors can be seen in VS Code, one for each participant</figcaption>

I was running the app locally on my first machine by using the `npm start` command. The app output running on localhost:5000 is seen below:

![Demo app running locally on localhost:5000](https://dev-to-uploads.s3.amazonaws.com/uploads/articles/kl1jtpyq8cr72h0f1oot.png)
<figcaption>Demo app running locally on localhost:5000</figcaption>

Now, here’s where it gets really exciting. Beyond just writing code together, I can also [share my localhost server](https://docs.microsoft.com/en-us/visualstudio/liveshare/use/vscode#share-a-server) with others in the session. I chose to share localhost:5000. Then, on my second computer, I navigated to localhost:5000, and I could see the app there too! As a collaborator, not only did I not need to clone the repo, I also didn’t need to install any dependencies or run the app locally on my own machine. All of that was handled by the first user!

![Share a session, servers, or terminals with others](https://dev-to-uploads.s3.amazonaws.com/uploads/articles/835nyft5jzpwuwb9q7du.png)
<figcaption>Share a session, servers, or terminals with others</figcaption>

In addition to sharing servers, you can also [share terminals with collaborators](https://docs.microsoft.com/en-us/visualstudio/liveshare/use/vscode#share-a-terminal). I shared a terminal from my first computer, and then on my second computer I was able to enter the `npm test` command in an attempt to run the project's test suite. (But alas – there are no tests to run.)

![Shared terminal instance](https://dev-to-uploads.s3.amazonaws.com/uploads/articles/iu3rhi7lyx2ceuunb3qe.png)
<figcaption>Shared terminal instance</figcaption>

During the collaboration session I added a new section to the app titled “Features to Try.” Our Live Share collaboration session was a great success!

![End result for the demo app](https://dev-to-uploads.s3.amazonaws.com/uploads/articles/8h454eignifn1dvoi8gw.png)
<figcaption>End result for the demo app</figcaption>

---

## Conclusion

I was honestly really impressed with how smoothly things went using Live Share and all the additional features it offered beyond simple code-editing collaboration. The only real challenges presented would be if VS Code was not your preferred IDE or if anyone in the pair programming session experiences internet connectivity issues.

Pair programming yields many benefits: fewer defects, higher productivity, and excellent mentoring opportunities. COVID-19 may have forced us all to work remotely, but that doesn’t mean we can’t still pair program. With tools like VS Code Live Share, remote pair programming can be just as easy as pair programming in the office.

---

*What has your remote pair programming experience been like? I’d love to hear from you in the comments below.*
