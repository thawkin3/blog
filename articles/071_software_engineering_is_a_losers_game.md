# Software Engineering is a Loser’s Game

I’ve recently become fascinated by the idea of “winner’s games” and “loser’s games.” There are several great articles which explain the idea in depth, but here’s a quick summary:

An observation was made by Simon Ramo in 1973 that there is a big difference in how games are won in amateur tennis versus professional tennis.

When two amateur opponents are playing, the game is often won not through the winner’s great skill but because of the loser’s mistakes. The loser often commits unforced errors by hitting the ball out of bounds, missing easy shots, or double faulting. In other words, the loser beats himself. Points are “lost” by the loser more than they are “won” by the winner. This is a “loser’s game.”

When two professional opponents are playing, the game is won primarily due to the winner’s skill. Neither player commits many unforced errors. The winner places his shots well and outperforms his opponent to defeat him. Points in this kind of game are “won” by the winner more than they are “lost” by the loser. This is a “winner’s game.”

So, if you’re playing a loser’s game, a winning strategy is to simply try to avoid making mistakes and let your opponent beat himself.

(If you’ve ever played tennis or ping pong before, I hope at this point you’re nodding your head in recognition. As an avid ping pong player, I’ve seen this scenario play out in the office at work on a daily basis.)

The application of this observation is that you should attempt to understand whether any given activity you’re involved in is a winner’s game or a loser’s game. Gaining that understanding teaches you how you should play the game.

You can read more about these ideas in this [article by Charles Ellis](https://www.empirical.net/wp-content/uploads/2012/06/the_losers_game.pdf), this [article from the FS blog](https://fs.blog/2014/06/avoiding-stupidity/), or this [article from Ben Hosking](https://thehosk.medium.com/software-development-is-a-losers-game-fc68bb30d7eb).

---

## Parallels to Software Engineering

Now, what if we consider software engineering to be a loser’s game? That is to say, we often beat ourselves by committing unforced errors and making mistakes. If we are amateurs, so to speak, how can we keep the ball in play rather than hitting it into the net?

It’s a simple thing to say, “If you want to be good, just stop making mistakes.” But that’s somewhat unhelpful. That’s like saying to those in poverty, “Why don’t you just stop being poor?”

It’s also unhelpful if we take this analogy too far. If avoiding mistakes is the ultimate goal of software engineering, is the best software engineer the one who writes no code or does nothing? Obviously, no. Software engineers are paid to write code to help bring to life some product in order to achieve some vision (make the business money, solve a real-world problem, simplify a task, etc.), so that must be the real ultimate goal.

So it appears that we must balance producing valuable output with avoiding mistakes. This leads to an interesting thought experiment: In what ways do we beat ourselves, and how can we avoid making these amateur mistakes?

---

## Unforced Errors

Here’s a list of possible unforced errors we commit. I’m sure you may be able to add more to this list as well.

* Not understanding the problem before trying to code a solution

* Not understanding the tools or programming languages we use

* Not carefully reviewing our own code before asking for a code review

* Not manually testing our own code before asking for a code review

* Not writing unit tests

* Not following agreed-upon company standards

---

## Solving These Unforced Errors

Now that we’ve identified some potential unforced errors, how do we avoid making them?

For starters, we can put safeguards in place to help us catch and correct our mistakes before they become too costly. All code repos should be configured with code linters, code formatters, and a suite of automated tests. These safeguards can be run as part of a CI pipeline prior to allowing any code to be merged.

We can also be more thorough in our own attention to detail when writing code. After creating a merge request, we should always do a self code review before asking others to review our code. We should always manually validate our changes as well.

Nothing is more frustrating as a code reviewer than reviewing someone else’s code who clearly didn’t do these checks themselves. It wastes the code reviewer’s time when he has to catch simple mistakes like commented out code, bad formatting, failing unit tests, or broken functionality in the code. All of these mistakes can easily be caught by the code author or by a CI pipeline.

When merge requests are frequently full of errors, it turns the code review process into a gatekeeping process in which a handful of more senior engineers serve as the gatekeepers. This is an unfavorable scenario that creates bottlenecks and slows down the team’s velocity. It also detracts from the higher purpose of code reviews, which is knowledge sharing.

We can use checklists and merge request templates to serve as reminders to ourselves of things to double check. Have you reviewed your own code? Have you written unit tests? Have you updated any documentation as needed? For frontend code, have you validated your changes in each browser your company supports? Have you ensured that all user-facing text is translated? Have you ensured that the UI meets accessibility standards and guidelines?

By performing these checks ourselves, aided by automated tools, we show an added measure of professionalism and respect for our coworkers. Trust will grow and velocity will increase. The key is to be diligent and disciplined.

---

## Conclusion

Software engineering is a loser’s game. So let’s learn to play the game and stop losing to ourselves.
