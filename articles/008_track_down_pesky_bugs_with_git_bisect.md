# Track Down Pesky Bugs with `git bisect`

Recently I’ve come across a couple dev issues that were incredibly difficult to track down.

Normally, fixing a bug in JavaScript is as simple as reproducing the issue, viewing the error and the stack trace, finding the appropriate source file, and fixing the code.

But what if the error for whatever reason isn’t helpful? Or what if there is no error? Maybe nothing is technically “broken”, but the behavior that’s occurring in your app is not at all what you expected or intended. Or maybe the error is coming from a third-party dependency that you recently upgraded.

Whatever the case may be, if you aren’t able to easily track down where in your source code the error is coming from, git has a really neat command that can help.

---

## Git Bisect

`git bisect` is a command that uses a binary search to identify the commit that introduced the issue. Using it is really straightforward.

To begin the process, you enter `git bisect start`.

---

Next, you need to identify two commits, one in which the issue exists and one in which the issue does not exist.

So, maybe you’re on your `master` branch right now with the latest code pulled down, and you’re seeing the bug. So you can tell git that this current commit is bad. To do that, you enter `git bisect bad`.

---

Now, you need to find a commit where the bug doesn’t exist. To do that, you can look through your repo’s recent commits by entering `git log --oneline`. This will give you a list of past commits, working its way backwards from the commit you’re currently on, with each commit being on its own line.

Use your own judgment to pick a commit that you think might not have the bug. If you’re not sure, it’s ok to check out a commit, compile your code, look for the bug, and then try again with a different commit if you haven’t found a good commit yet.

Once you’ve found a good commit, you can tell git by entering `git bisect good <good commit hash here>`.

---

Now, git starts its binary search. What it does is essentially grab the middle commit between your good and bad commits and checks it out. Now that the commit is checked out, you can compile your code and test to see if the issue exists or not.

If the issue exists on this commit, you tell git with `git bisect bad`. If the issue does not exist on this commit, you tell git with `git bisect good`.

---

Upon receiving this feedback, git checks out another commit in the middle of this commit and the good/bad starting points, depending on the feedback you gave it.

Again, you compile your code, test it, and let git know if you see the issue or not with `git bisect bad` or `git bisect good`.

This process repeats, and it’s in the most efficient way possible with the least number of steps because of the binary search.

---

Once you’ve finished, you’ll have identified the commit that introduced the issue. To tell git you’re done bisecting, you enter `git bisect reset`.

At that point, you can go check out the pull request in GitHub (or whatever you’re using), read through the code, and get a better idea for where exactly the problem lies.

---

## Conclusion

![Person happily jumping](https://miro.medium.com/max/6016/0*IaTCMflAptMyBjrR)
<figcaption>Photo by Andre Hunter on Unsplash</figcaption>

`git bisect` has been a lifesaver for me in these instances. By using it, I’ve been able to track down the offending commit, get a better idea of what the root cause of the problem could be, and then start fixing the issue.

Thanks git!

---

You can read the [full documentation from git here](https://git-scm.com/docs/git-bisect).
