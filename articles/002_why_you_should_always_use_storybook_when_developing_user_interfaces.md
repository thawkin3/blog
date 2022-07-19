# Why You Should Always Use Storybook When Developing User Interfaces

Storybook makes testing, documentation, and knowledge-sharing easy

![Photo by [Amélie Mourichon](https://unsplash.com/@amayli?utm_source=medium&utm_medium=referral) on [Unsplash](https://unsplash.com?utm_source=medium&utm_medium=referral)](https://cdn-images-1.medium.com/max/8064/0*ms7b4ZKLk1o1p9aI)*Photo by [Amélie Mourichon](https://unsplash.com/@amayli?utm_source=medium&utm_medium=referral) on [Unsplash](https://unsplash.com?utm_source=medium&utm_medium=referral)*

## The Problem

As a frontend software engineer, developing clean and functional user interfaces is a big part of what I do. Making sure the UI works flawlessly involves a lot of testing: unit testing, manual testing, cross-browser testing, mobile responsiveness testing, accessibility (a11y) testing, and plenty more.

There’s also the need for consistency in design across multiple components. All the individual pieces need to work well together, and they need to be visually pleasing to the end user. And, the components we as developers create need to conform to the designs and standards set forth by our team’s designers.

So, how do we achieve all this in an effective way?

---

## The Solution: Storybook

Enter Storybook. Their [home page](https://storybook.js.org/docs/basics/introduction/) starts off with the following snippet of text:

> Storybook is a user interface development environment and playground for UI components. The tool enables developers to create components independently and showcase components interactively in an isolated development environment.
> Storybook runs outside of the main app so users can develop UI components in isolation without worrying about app specific dependencies and requirements.

In my mind, Storybook provides solutions in four main ways:

1. Makes it easy to build components in isolation, outside of your app.
2. Increases developer awareness of existing components.
3. Serves as a living style guide and documentation.
4. Makes it easy to visually test edge cases.

Let’s discuss each of these in detail.

---

## Makes it easy to build components in isolation, outside of your app.

Have you ever been working on a new feature that requires new UI components, but haven’t had a place in your app to put them yet? Maybe the main page doesn’t even exist yet. Maybe the backend API endpoints haven’t been fully flushed out yet. Maybe you’re building a component library and don’t even have an app to demo them in.

Storybook is a development tool, and it runs completely independently of your actual app. When doing local development, you can run Storybook on its own port on your localhost.

Within Storybook, you create what are called “stories”, which are essentially just mini examples or demos. For example, if you were developing a button component, you could create a story for that. If you have multiple variations of that button component, you could create stories for that too. For instance, a story for a button with an outline focus style and a story for a button with an underline focus style.

![Storybook examples for a button component](https://cdn-images-1.medium.com/max/2448/1*Oc7px7evb91eVqispaNlLQ.png)*Storybook examples for a button component*

---

## Increases developer awareness of existing components.

Have you ever been working on creating a new component, only to find out that the exact same component (or something really similar) already exists in your code base? And then find that three months later another developer has created yet another component just like the first two?

This problem stems largely from a lack of documentation. Developers that are new to the code base may not know what resources are already out there for them to use. While they could ask their co-workers for this information, wouldn’t it be nice if all your components were documented somewhere?

Storybook can help you with that!

By creating stories in Storybook, you are creating documentation for your fellow software engineers. Anyone that looks at the Storybook app can get a bird’s eye view of what you and your team have already created.

This is especially helpful if you maintain your own component library at your company. If you use Storybook, it’s really easy to show other developers what you’ve created for them: buttons, cards, modals, menus, tooltips, etc. All there and ready for them to use!

Now instead of accidentally creating many components that all do the same thing, developers can browse your Storybook app, find what they’re looking for, and use that existing component.

![A button with an underline focus style! Just what I needed!](https://cdn-images-1.medium.com/max/2148/1*kPf5imbdb8PghybPqtNHvw.png)*A button with an underline focus style! Just what I needed!*

---

## Serves as a living style guide and documentation.

Most companies have a formal style guide that designers and developers are expected to follow. The style guide could include things like fonts, approved branding colors, icons, and various components (for example, what your company’s modals should look like).

If your style guide is just a PDF of mockups, it’s easy for the designs and the reality of what you’ve actually delivered to diverge. And no one likes misleading or outdated documentation.

Storybook examples are actual code examples, so what you see is what you get.

If you see a modal component in your Storybook app, when you go to implement that same modal component in your actual app, the modal will look just like it did in Storybook. No more confusion!

---

## Makes it easy to visually test edge cases.

This one, for me, is the biggest game changer. Have you ever had a complex component that renders differently depending on things like its loading state or the data it is given? And have you ever had to manually test that component in your app for all those different states? Are some of those states nearly impossible to replicate? Or do they involve complex setup within your app to get them into the state you want to test?

Storybook helps simplify all of that!

Back to the button example: you’re creating a button component. It’s just a button. How hard could it be? Well, there might be more to it than you initially think!

For instance, what happens if your button is passed a really long string for its text?

![Button with a really long text label](https://cdn-images-1.medium.com/max/4684/1*f2nlkfymnDOTLTUzQeLVdg.png)*Button with a really long text label*

Oh no! That’s far too wide of a button. Maybe we should set a `max-width` on that.

![Button with a max-width set](https://cdn-images-1.medium.com/max/2336/1*JEvWDefPnOSOK-vGgsglvQ.png)*Button with a max-width set*

There we go, that’s much more manageable. Now a user could actually read this button if for whatever reason you decided you truly needed this much text to go into a button (but in practice, seriously, don’t do this).

Ok, another example: what if your button text didn’t have any spaces in it? (Don’t ask me why.)

Let’s say your button had the text: “ClickHereToHaveSomethingReallyReallyAwesomeHappen”.

![Button with text that has no spaces](https://cdn-images-1.medium.com/max/3012/1*WRDae8OGmxgUE9gJf6P85g.png)*Button with text that has no spaces*

Oh no! The text is overflowing outside of the button! Let’s fix that by making the word wrap and break properly.

![Button with text that wraps properly](https://cdn-images-1.medium.com/max/2528/1*a6YHIaVqiLkuJE9bPMAidQ.png)*Button with text that wraps properly*

Much better! No more text overflow.

Ok, this example was a bit contrived. But in reality, if your app is translated into multiple different languages, you might be surprised by the length of some of the words you see in German or Italian, so you really might run into some text overflow issues if your button widths are smaller.

---

## Bonus: Storybook Add-ons

In addition to what we’ve looked at so far, Storybook also has a rich ecosystem of supported add-ons that extend Storybook’s functionality.

One really useful add-on is called [Knobs](https://github.com/storybookjs/storybook/tree/master/addons/knobs). Knobs allow you to make examples that the person viewing your Storybook app can modify and configure directly in the UI without making changes in the code itself. The add-on turns Storybook into a really neat playground.

Another handy add-on is called [Viewport](https://github.com/storybookjs/storybook/tree/master/addons/viewport). This add-on works similar to the device simulator in the Google Chrome developer tools and allows you to view your components in various screen sizes and layouts. This is great for doing some responsiveness testing.

There are lots more add-ons, but the last one I’ll mention is the [a11y](https://github.com/storybookjs/storybook/tree/master/addons/a11y) add-on. This add-on does an accessibility audit on each of your examples and displays the results in Storybook. It can check for things like having an adequate color contrast ratio or using appropriate aria roles and labels.

---

## Conclusion

Given all the benefits of using Storybook, I’ll never do UI development without Storybook again. Its ability to help with testing, documentation, and knowledge-sharing is unmatched by any other tool I’ve come across so far. I’d highly recommend it to any frontend developer out there who wants to take their app’s UI to the next level.
