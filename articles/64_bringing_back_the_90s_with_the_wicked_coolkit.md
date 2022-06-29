# Bringing Back the 90s with the Wicked Coolkit

Remember the 90s? Pokémon, Beanie Babies, Crazy Bones, Super Nintendo, Pogs, and neon windbreakers… Those were the good old days. The web was a simpler place too, with barebones websites composed of mostly text and hyperlinks. I remember it like it was yesterday. Or, wait — 30 years ago?

I recently found [Wicked Coolkit](https://wickedcoolkit.com/) — a nifty retro-themed toolkit — and I thought it would be fun to play around with it to briefly relive those years. The toolkit includes a hit counter, webrings, and developer trading cards.

If you’re feeling nostalgic too, let’s explore some web development trends from the 90s by exploring Wicked Coolkit, just for kicks.

---

## Hit Counters

Before software like Google Analytics, [hit counters](https://en.wikipedia.org/wiki/Web_counter) were a fun way to track your site’s popularity while also flaunting your success (or lack thereof) to everyone who visited your site. Hit counters were usually clunky widgets that simply displayed a number that incremented after every page load. Now that website analytics software is readily available and affordable, hit counters have almost entirely disappeared from the modern web. But that doesn’t mean we can’t make one just for fun!

The [Wicked Coolkit](https://wickedcoolkit.com/) setup for hit counters requires you to deploy a Heroku app and create a Salesforce developer account, both of which have free tiers that I used. Once the app is up and running, it handles the backend server keeping track of your website’s hits. After that, it’s as simple as including on your site a code snippet that contains a `script` tag and a custom web component for the hit counter.

The code snippet looks like this, though the `host` attribute will be different for your app:

```html
<script type="module" async src="https://unpkg.com/wicked-coolkit@^1.1.2/dist/hitCounter.js"></script>
<wck-hit-counter host="tyler-wicked-coolkit.herokuapp.com"></wck-hit-counter>
```

Here’s my hit counter that I added to an HTML5 game I built called [Stacks on Stacks on Stacks](http://tylerhawkins.info/stacks-on-stacks-on-stacks-2/):

![Stacks on Stacks on Stacks HTML5 game, now with a hit counter](https://dev-to-uploads.s3.amazonaws.com/uploads/articles/6zxh26cysn9w2s8ttbv3.png)
<figcaption>Stacks on Stacks on Stacks HTML5 game, now with a hit counter</figcaption>

---

## Webring

Now that we have a hit counter in place, let’s make a webring! [Webrings](https://en.wikipedia.org/wiki/Webring) were another concept popular in the 90s that linked together a group of sites. These sites would form a chain or a ring of curated content and allowed users to explore cool new sites they probably never would have found on their own. Webrings were a neat way to stumble upon new pages back before the days of recommendation engines, social media, and customized news feeds.

The Wicked Coolkit provides us with another handy code snippet to include on any website that wants to participate in the webring. It looks like this, again with a `script` tag and a web component, with the `host` attribute being unique to my app:

```html
<script type="module" async src="https://unpkg.com/wicked-coolkit@^1.1.2/dist/webring.js"></script>
<wck-webring host="tyler-wicked-coolkit.herokuapp.com"></wck-webring>
```

The list of sites is managed in Salesforce for an added measure of security and to prevent any old site from being included in the webring. This allows you, the administrator, to fully control what sites you want to showcase in your webring.

I created my own webring and recruited a few friends to participate. For some fun apps, you can [find our webring here](http://tylerhawkins.info/BubbleBall2/)!

![Bubble Ball HTML5 game, now part of the webring](https://dev-to-uploads.s3.amazonaws.com/uploads/articles/3z1dpyix10j8aatfhdls.png)
<figcaption>Bubble Ball HTML5 game, now part of the webring</figcaption>

---

## Developer Trading Card

Finally, we would be remiss if we didn’t talk about the 90s trading card phase. Who among us didn’t have beloved Pokémon, Digimon, Yu-Gi-Oh!, Magic: The Gathering, or sports cards? Wouldn’t you like to be on a trading card yourself? Well, now you can with Wicked Coolkit’s developer trading cards. You can build your own and manage the content with the same Heroku and Salesforce app you used for the hit counter and webring.

Here’s what [my developer trading card](https://tyler-wicked-coolkit.herokuapp.com/) looks like:

![Personalized developer trading card](https://dev-to-uploads.s3.amazonaws.com/uploads/articles/vnuxzjosnbwxu6f4yrab.png)
<figcaption>Personalized developer trading card</figcaption>

Clicking on the various stickers takes you to another person’s trading card, giving you a unique opportunity to connect with other developers with similar interests.

---

## Conclusion

Well, that’s it for now. The 90s were a long time ago, but it was fun to reminisce. History tends to repeat itself, so who knows? Maybe one day some of these retro trends will make a comeback.
