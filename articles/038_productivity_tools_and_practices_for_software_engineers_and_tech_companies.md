# Productivity Tools and Practices for Software Engineers and Tech Companies

Everyone wants to be more productive without burning out. So, how do you get more done without working more hours? And how do you help the rest of your team improve without taking on the role of taskmaster? The answer: use effective tools. 

In this article, we'll look at five effective tools software engineers and tech companies can use to speed up their development lifecycle without sacrificing quality. Design systems, code linters, code formatters, continuous integration, and IaaS/PaaS providers are all tools that allow software engineers to streamline mundane work and, in turn, prioritize building their products.

---

## Design Systems

![Palantir's Blueprint component library](https://dev-to-uploads.s3.amazonaws.com/i/158m043u687j1yp7rrpo.png)
<figcaption>Palantir's Blueprint component library</figcaption>

A design system can be boiled down to a component library that is used to create a product out of reusable building blocks.

*(Although in practice, it is much more than that! A design system also includes the design patterns, usage guidelines, documentation, ownership model, communication methods, product roadmap, and much more.)*

These building blocks might consist of components like avatars, badges, buttons, dropdowns, form inputs, icons, links, modals, progress indicators, and tooltips. Like Lego pieces, these components can be assembled to create all the pages and features that your app needs.

Design systems provide massive benefits that allow your organization's UI to scale as the company (and product) grows.

1. First, the design system helps you **create a consistent UI** since you're using the same building block components throughout your app.
2. Second, your designers and software engineers can **develop faster** since they don't have to spend hours or days reinventing the wheel rebuilding things like modals. Instead, the design system can provide one generic modal that can be implemented everywhere.
3. Third, using a shared set of components makes it much easier to **roll out style changes throughout the app all at once**. If your app's button styles need to be changed, rather than tweaking every individual button in the app, you can just adjust the button styles in the design system and then watch the changes take effect everywhere else in the app too!
4. Fourth, design systems allow you to **focus on the hard UX problems**. Rather than spending time deciding how dropdowns and modals should work for every new feature your company develops, UX designers can instead focus on the experience as a whole to make sure each feature is sensible and user-friendly.

If you decide to build your own design system, be advised that they take a lot of work! Design systems are a product, not a side project. Otherwise, if you recognize that you don't have the time or resources to build your own design system, there are plenty of good options like Google's [Material-UI](https://material-ui.com/), Adobe's [Spectrum](https://spectrum.adobe.com/), or [Ant Design](https://ant.design/).

---

## Code Linters

![ESLint example output](https://dev-to-uploads.s3.amazonaws.com/i/h4qai5fyzdmqrwwejfnn.png)
<figcaption>ESLint example output</figcaption>

Code linters like [ESLint](https://eslint.org/) for JavaScript do static analysis of your code. They help catch syntax errors and even best-practice issues automatically and can be directly included in your build process or git hooks. Code linters are handy because they automate the kinds of things humans are bad at but that machines are good at – no more searching for the missing curly brace on line 245!

ESLint is also highly configurable and has a vast ecosystem of plugins. You can install ESLint plugins like [eslint-plugin-jsx-a11y](https://www.npmjs.com/package/eslint-plugin-jsx-a11y) to help catch accessibility violations in your app or [eslint-plugin-react](https://www.npmjs.com/package/eslint-plugin-react) to help enforce React best practices. There are also presets you can use if you don't want to pick and choose the various plugins yourself. One popular preset is the [eslint-config-airbnb](https://www.npmjs.com/package/eslint-config-airbnb) package that includes Airbnb's recommended ESLint configuration options.

---

## Code Formatters

Formatters like [Prettier](https://prettier.io/) can format your JavaScript, HTML, CSS, and even markdown files. Similar to code linters, code formatters help automate what would otherwise be a painfully manual task.

No more spending time arguing about whether you should use spaces or tabs, semicolons or no semicolons, trailing commas or not – just set up your Prettier config and allow it to format your code. The formatter will maintain consistency and team standards throughout your repository for you. This also means no more spending time in code reviews saying things like “missing semicolon here” or “add a new line to the end of the file.” Prettier allows you to focus on the functionality and maintainability of the code itself.

Here's my preferred Prettier configuration setup:

```js
{
  "tabWidth": 2,
  "useTabs": false,
  "printWidth": 80,
  "semi": false,
  "singleQuote": true,
  "trailingComma": "es5",
  "quoteProps": "as-needed",
  "jsxSingleQuote": false,
  "jsxBracketSameLine": false,
  "bracketSpacing": true,
  "arrowParens": "avoid",
  "endOfLine": "auto",
  "proseWrap": "preserve",
  "htmlWhitespaceSensitivity": "css"
}
```

Using this Prettier config can take ugly, inconsistently formatted code like this:

```js
function HelloWorld({greeting = "hello", greeted = '"World"', silent = false, onMouseOver,}) {

  if(!greeting){return null};

     // TODO: Don't use random in render
  let num = Math.floor (Math.random() * 1E+7).toString().replace(/\.\d+/ig, "")

  return <div className='HelloWorld' title={`You are visitor number ${ num }`} onMouseOver={onMouseOver}>

    <strong>{ greeting.slice( 0, 1 ).toUpperCase() + greeting.slice(1).toLowerCase() }</strong>
    {greeting.endsWith(",") ? " " : <span style={{color: '\grey'}}>", "</span> }
    <em>
	{ greeted }
	</em>
    { (silent)
      ? "."
      : "!"}

    </div>;

}
```

And turn it into beautiful code that looks like this!

```js
function HelloWorld({
  greeting = 'hello',
  greeted = '"World"',
  silent = false,
  onMouseOver,
}) {
  if (!greeting) {
    return null
  }

  // TODO: Don't use random in render
  let num = Math.floor(Math.random() * 1e7)
    .toString()
    .replace(/\.\d+/gi, '')

  return (
    <div
      className="HelloWorld"
      title={`You are visitor number ${num}`}
      onMouseOver={onMouseOver}
    >
      <strong>
        {greeting.slice(0, 1).toUpperCase() + greeting.slice(1).toLowerCase()}
      </strong>
      {greeting.endsWith(',') ? (
        ' '
      ) : (
        <span style={{ color: 'grey' }}>", "</span>
      )}
      <em>{greeted}</em>
      {silent ? '.' : '!'}
    </div>
  )
}
```

---

## Automated Tests and Continuous Integration

As any app grows in complexity and size, it becomes impossible for a single person to remember how everything works. It also becomes infeasible to manually test everything in the app, not to mention cost prohibitive.

Unit tests, integration tests, and end-to-end (e2e) tests make sure your code does what you think it does, serve as documentation, and guard against future regressions. If you ever feel like writing tests is a pointless endeavor, remember – [tests are for the future](https://dev.to/thawkin3/tests-are-for-the-future-3a4d).

Continuous integration (CI) ensures that your master branch of code stays in a workable state (in theory). You can use a service like [Travis CI](https://travis-ci.org/), [CircleCI](https://circleci.com/), [GitLab CI/CD](https://docs.gitlab.com/ee/ci/), or [Heroku CI](https://devcenter.heroku.com/articles/heroku-ci) to set up continuous integration for your repository. You can then configure your CI pipeline to run your linters and automated tests after each commit and also require that everything passes before code can be merged.

By having tests and running them often – both during local development and as part of the CI pipeline – you can save hours of time you'd otherwise spend manually testing the app.

---

## IaaS and PaaS providers

Both infrastructure as a service (IaaS) providers and platform as a service (PaaS) providers manage infrastructure for you so that you don't have to. Common IaaS providers include [Amazon Web Services](https://aws.amazon.com/), [Google Cloud Platform](https://cloud.google.com/), and [Microsoft Azure](https://azure.microsoft.com/). PaaS providers would be solutions like [Heroku](https://www.heroku.com/) or [Netlify](https://www.netlify.com/).

For example, using a managed database like [Amazon Relational Database Service (RDS)](https://aws.amazon.com/rds/) means you don't have to worry about performing database upgrades or installing security patches. Using a notification service like [Amazon Simple Notification Service (SNS)](https://aws.amazon.com/sns/?whats-new-cards.sort-by=item.additionalFields.postDateTime&whats-new-cards.sort-order=desc) means you don't have to create your own services for sending emails or text messages.

Deploying your app to the Heroku platform means, among other things, that you don't have to worry about scaling your app as usage increases. [Horizontal and vertical scaling](https://devcenter.heroku.com/articles/scaling) can happen automatically.

![Heroku logo](https://dev-to-uploads.s3.amazonaws.com/i/lhcssbaavlyzgdbir11z.png)
<figcaption>Heroku logo</figcaption>

When your infrastructure is managed for you, you can spend more time on your product and less time on [toil](https://landing.google.com/sre/workbook/chapters/eliminating-toil/).

---

## Conclusion

Each of the tools we've covered helps take care of the mundane work inherent in software engineering. Design systems, code linters, code formatters, tests, continuous integration, and IaaS/PaaS providers can drastically speed up your software development lifecycle. Still, it's up to you to start using them. Once you've taken care of the initial configuration for these tools, you'll be amazed at how efficient you and your team can become.

If you want to be more productive, use the tools at your disposal to automate what you can. Then you can focus on what you love – building your world-changing app.
