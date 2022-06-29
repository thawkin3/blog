# Web Component Solutions: A Comparison

"Don’t repeat yourself." Every programmer has this concept drilled into their head when first learning to code. Any time you have code you find yourself duplicating in several places, it’s time to abstract that code away into a class or a function. But how does this apply to user interfaces? How do you avoid re-writing the same HTML and CSS over and over again?

If you’re using a UI framework like Angular or a UI library like React, the answer is simple: you build a component. Components are bits of HTML, CSS, and JavaScript put together in a way that they can be easily reused.

But what if you’re not using Angular, React, Vue, or whatever else is the latest and greatest new JavaScript framework? What if you’re writing plain vanilla HTML, CSS, and JavaScript? Or what if you want to write a component that is framework-agnostic and can be used in any web app regardless of what it’s written in?

---

## Web Components

Enter [web components](https://developer.mozilla.org/en-US/docs/Web/Web_Components). Web components allow you to create custom elements with encapsulated functionality that can be reused anywhere. They’re created using [templates](https://developer.mozilla.org/en-US/docs/Web/HTML/Element/template) and [slots](https://developer.mozilla.org/en-US/docs/Web/HTML/Element/slot) and are defined in the [shadow DOM](https://developer.mozilla.org/en-US/docs/Web/Web_Components/Using_shadow_DOM), which isolates your element’s styles and scripts from the rest of the DOM to avoid collisions.

Web components can be built using the native browser APIs provided by most major browsers, or they can be created using what are called web component libraries: solutions that serve as an abstraction on top of the browser APIs to help make writing web components easier.

In this article, we’ll compare a few different web component solutions: native [web components](https://developer.mozilla.org/en-US/docs/Web/Web_Components), [Svelte](https://svelte.dev/), [Stencil](https://stenciljs.com/), [LitELement](https://lit-element.polymer-project.org/), and [Lightning Web Components (LWC)](https://developer.salesforce.com/docs/component-library/documentation/lwc).

---

## The Criteria

In evaluating these solutions, it’s helpful to have a defined set of criteria. We’ll look at each solution while keeping an eye out for the following:

- year released
- popularity
- license
- syntax style (declarative vs. imperative)
- compiler or runtime required
- browser support
- testing strategy
- quality of documentation
- relative bundle size

---

## Native Web Components

Let’s first start with [native web components](https://developer.mozilla.org/en-US/docs/Web/Web_Components)—that is, web components built using the browser APIs and no additional frameworks or libraries.

Web components were first introduced in 2011. As with every new technology, web browsers needed time to catch up and implement the new proposed APIs and standards, so web components took a while to gain traction. Today, web components are supported in most evergreen browsers. Chrome, Firefox, Edge, and Opera all support web components. Safari provides partial support. In Internet Explorer, web components are not supported (surprise, surprise).

Since this is native web functionality we’re talking about, the documentation is excellent. You can find [resources on MDN](https://developer.mozilla.org/en-US/docs/Web/Web_Components) for specifications and tutorials on how to build and implement web components.

Another pro of using vanilla web components is you don’t need to introduce another library, compiler, runtime, or any other build tools. Web components just work (as long as the browser supports them).

In addition to lacking full browser support, one con of native web components is that they are written using an imperative style. In other words, you must tell the component how to execute each step, including re-rendering or updating content in the DOM. Those who enjoy the declarative style of writing React components will likely become frustrated with native web components.

To alleviate this pain, many web component libraries have emerged to provide an abstraction over the native browser APIs. These libraries offer a better developer experience when creating new web components and often include polyfills that allow the web components to work in browsers that wouldn't support them out of the box. We’ll consider some of these web component libraries in the next few sections of this article.

You can find an [example of a native web component implementation here](https://webcomponents.dev/edit/ssbYKxUnqWP2AdOWvV1O). The code is reproduced below in full:

{% gist https://gist.github.com/thawkin3/c1de9c89f254421ff352bb713a2df0bc %}

First, you define a custom web component by extending the `HTMLElement` class. HTML and CSS are defined inline in the class body and then get inserted into the shadow DOM by modifying the HTML content of the `shadowRoot`. Since the code is written imperatively, you can see an update method defined that handles updating the DOM content when necessary. Lifecycle callback methods are also available for setup and teardown, which you can see when attaching and removing event listeners in the `connectedCallback` and `disconnectedCallback` methods. Finally, the component is registered with the rest of the application using the `customElements.define` method, which allows you to provide an HTML tag name and link it to your class.

---

## Svelte

[Svelte](https://svelte.dev/) was released in 2016 as a simple and elegant way to write web components. It allows you to write your components in a declarative style and handles the imperative step-by-step instructions for updating the DOM for you. Svelte components are written in files ending in the `.svelte` extension, a custom file type that allows you to include HTML, CSS, and JavaScript all in the same file. Svelte includes no runtime, meaning it builds the components during compile time into code that browsers can understand. This provides the benefit of little-to-no overhead added to your app’s bundle size.

At the time of writing, Svelte boasts 65,043 weekly downloads from NPM, making it one of the most popular web component libraries right now. Its documentation is also excellent, including [interactive tutorials](https://svelte.dev/tutorial/basics) that walk you through everything you’d ever want to know. Svelte even comes with its own animation utilities!

Svelte has a growing community, which means there are many people learning Svelte and creating third-party components and plugins for others to use. You can find a list of [Svelte open-source projects here](https://svelte-community.netlify.app/code/).

For all its benefits, Svelte does have some weak spots it needs to iron out, as covered in their [FAQs](https://svelte.dev/faq). Syntax highlighting with `.svelte` files in your IDE still isn’t perfect. They also don’t provide a recommended testing strategy yet—the current approach is to essentially compile each component and then mount it to the DOM using your test library of choice. Additionally, [Svelte doesn’t advertise what browsers it supports](https://github.com/sveltejs/svelte/issues/558). It appears you’ll need to figure this out on your own and provide whatever polyfills you end up needing, especially if you plan to support IE11.

You can find an [example of a Svelte web component implementation here](https://webcomponents.dev/edit/c1lCr4jc91cI3wpH5lt6). The code is reproduced below in full:

{% gist https://gist.github.com/thawkin3/4c512d0109db701c26faa3cecd556813 %}

As mentioned earlier, all of the HTML, CSS, and JavaScript are included in the same `.svelte` file and look very much like normal HTML, as opposed to a JSX-like syntax. The component tag name is defined on the first line. Event handlers are attached to HTML elements with the `on:event-name` syntax, and the UI is reactively updated when the state changes—nice and declarative!

---

## Stencil

[Stencil](https://getstencil.com/) is an online graphic design tool commonly used by UI/UX designers. In 2017, the [Ionic Framework](https://ionicframework.com/) team released a tool, `@stencil/core`, for developers. Like Svelte, Stencil is a compiler only, so no runtime needed. The compiler creates web components that browsers can understand and even includes polyfills as needed so that your code can run in every major browser, including IE11.

Stencil components are written in TypeScript, which may be either exciting for you or a huge turn-off, depending on your opinion of adding types to JavaScript. They are also written using JSX and a declarative style, so it feels very much like writing components in React.

Stencil currently shows 25,568 weekly downloads from NPM, making it less popular than Svelte, but still a popular choice. Stencil brags that it is used by companies like Apple, Amazon, and Microsoft, implying that it’s a battle-proven solution. [Stencil’s docs](https://stenciljs.com/docs/introduction) are excellent as well, even providing instructions on how to incorporate components generated by Stencil into Angular, React, or Vue apps.

To [test Stencil components](https://stenciljs.com/docs/unit-testing), their docs recommend using Jest and Stencil testing utility methods found in the `@stencil/core/testing` package.

You can find an [example of a Stencil web component implementation here](https://webcomponents.dev/edit/1beAhuWbhB2THaFGLI7q). The code is reproduced below in full:

{% gist https://gist.github.com/thawkin3/3e6a30e2c0d960344a0fa319435a5761 %}

The web element is defined through a class, but it doesn't extend any base class like the native web component implementation did. Instead, a `@Component` decorator is used, which provides the tag name, where the styles can be found, and whether or not the component should be placed in the shadow DOM. Component state is implemented using the `@State` decorator, and the HTML content is written inside a `render` method.

---

## LitElement

Next, let’s look at [LitElement](https://lit-element.polymer-project.org/), an offering from Google’s [Polymer Project](https://www.polymer-project.org/). LitElement was released in 2018 and currently has 95,643 weekly downloads from NPM—an impressive statistic for an offering only two years old—making it more widely used than both Svelte and Stencil combined.

LitElement offers many of the same benefits we’ve discussed before, such as using declarative syntax, compiling down to code complying with web component standards, and working in all major browsers including IE11.

LitElement is licensed under the BSD-3-Clause license, which is a fairly permissive license (not to be confused with the BSD+Patents license that created the [controversy regarding React](https://www.freecodecamp.org/news/facebook-just-changed-the-license-on-react-heres-a-2-minute-explanation-why-5878478913b2/) until Facebook changed React’s license to the MIT license in 2017).

It’s also important to note that unlike Svelte and Stencil, LitElement is not a compiler itself. The docs describe LitElement as a library for building web components using lit-html templates ([lit-html](https://lit-html.polymer-project.org/guide) is another offering from the Polymer Project that serves as an HTML templating library). So in order to use web components created with LitElement, you first need to [compile them using Babel and Rollup or Webpack](https://github.com/PolymerLabs/lit-element-build-rollup).

For testing, the LitElement docs recommend using the [Open WC testing library](https://open-wc.org/testing/testing.html), a general library used to test web components.

You can find an [example of a LitElement web component implementation here](https://webcomponents.dev/edit/YtTsAs9pYkRTrA4sTwrA). The code is reproduced below in full:

{% gist https://gist.github.com/thawkin3/e13758b6071db568cf50cc4558488fb9 %}

The code style here looks like a cross between native web components and Stencil components. A class is defined which extends the base `LitElement` class. The HTML content is provided in a `render` method and is wrapped in a template literal used by the `lit-html` package. Event handlers are attached using the `@event-name` syntax. Finally, just like native web components, the new components get registered via the `customElements.define` method.

---

## Lightning Web Components (LWC)

Finally, let’s consider Lightning Web Components, or LWC. LWC is the new kid on the block, an offering that [Salesforce open sourced in 2019](https://www.salesforce.com/company/news-press/press-releases/2019/05/192915-e/). Being newer, LWC only has 1,383 weekly downloads from NPM, far fewer than the other web component solutions we’ve considered so far.

LWC looks similar to other solutions we’ve explored in that the code is written declaratively. It also supports the latest version of all major browsers, including IE11.

One difference from the other libraries is that LWC includes a runtime, meaning you have an additional script that runs on the page to help your app work, similar to how you need to include the React library alongside a React app in the browser. This means extra code for your users to download, but at only 7kB, the LWC runtime is pretty small.

[Their documentation](https://lwc.dev/guide/introduction) comes with some great explanations and explicitly states how you can [test your LWC apps](https://lwc.dev/guide/test), which is incredibly helpful. They also include a [guide on accessibility](https://lwc.dev/guide/accessibility). While not necessary, it's nice to see that accessibility is something that the LWC development team values and feels is worth noting in their docs. Overall, LWC looks like a fine choice for organizations looking for a stable web component library. As time goes on, it will be interesting to see adoption rates and if LWC can catch up in popularity with the other web component solutions.

You can find an [example of an LWC web component implementation here](https://webcomponents.dev/edit/jLMrZzrJGpD6lVk9pJtg). The code is reproduced below in full:

{% gist https://gist.github.com/thawkin3/6d0340bdd42f39d4c50fbf2826fa778c %}

Note the use of three separate files for the HTML, CSS, and JavaScript. The files don't explicitly reference each other anywhere. Instead, LWC has an implicit contract that files with the same name but different extension are used together. The HTML is wrapped in a `template` tag, and the event handlers are written using the all-lowercase `oneventname` syntax you'd see in regular HTML. The JavaScript defines a class that extends `LightningElement` and then implements the state and any methods. Interestingly, there is no `render` method, as the HTML is magically linked to the JavaScript. Just like native web components and LitElement, LWC web components are then registered using the `customElements.define` method at the bottom of the file.

---

## Conclusion

So which web component solution should you use? It's important to evaluate these solutions for yourself in the context of the needs of your organization.

In comparing these web component libraries, **Svelte** feels like more of an experimental library for now, probably not something ready for enterprise applications. Stencil, LitElement, and LWC all present themselves as more enterprise-ready solutions, with their focus on cross-browser support and recommended testing strategies when writing unit tests.

**LitElement** seems like an excellent choice, with no readily apparent drawbacks besides its young age.

**Stencil** seems on par with LitElement and would be a great choice, especially if you’re already using Stencil for design or enjoy working with TypeScript.

Finally, if you are using, or might use, Salesforce in the future, **LWC** is the obvious choice because of its easy integration with other Salesforce workflows and frameworks such as building UIs with the Lightning App Builder or implementing security with Lightning Locker. LWC is also a great choice for enterprise, as it's open source but also backed by the power of a large corporation. You might also consider LWC if you enjoy being an early adopter of new web component technology trends, don't like JSX syntax, or have a preference for keeping your HTML, CSS, and JavaScript code in separate files. 

One thing seems clear: using a web component library rather than the native browser APIs will provide a better development experience as well as a more robust and cross-browser-friendly solution.

---

## A Final Note

In researching each solution, I’ve tried to be as impartial as possible, using the predefined set of criteria to evaluate each one. For a quick reference, I’ve included a chart summarizing my findings below.

If you’d like to explore even more web component solutions, [this blog post](https://webcomponents.dev/blog/all-the-ways-to-make-a-web-component-april2020/) provides an in-depth look into thirty web component implementations.

Thanks for reading!

---

## Web Component Solution Comparisons

|  | Native web components | Svelte | Stencil | LitElement | Lightning Web Components |
| --- | --- | --- | --- | --- | --- |
| Year released | 2011 | 2016 | 2017 | 2018 | 2019 |
| Popularity (weekly downloads) | N/A | 65,043 | 25,568 | 95,643 | 1,383 |
| License | N/A | MIT | MIT | BSD-3-Clause | MIT |
| Syntax style | Imperative | Declarative | Declarative | Declarative | Declarative |
| Compiler or runtime required | None | Compiler only | Compiler only | Needs Babel and webpack or Rollup | Runtime only |
| Browser support | Supported - Chrome, Firefox, Opera, Edge; Partial support - Safari; Unsupported - Internet Explorer | Unclear, nothing official in the docs | Chrome, Firefox, Safari, Edge, IE11 | Chrome, Firefox, Safari, Opera, Edge, IE11 | Chrome, Firefox, Safari, Edge, IE11 |
| Testing strategy | No official recommendation | No official recommendation | Jest and Stencil test utils | Karma and Open WC | Jest and LWC presets |
| Quality of documentation | Excellent | Excellent | Excellent | Excellent | Excellent |
| Relative bundle size based on a single component example* | 558 B | 1.68 kB | 3.47 kB | 6.55 kB | 12.35 kB |
| Relative bundle size based on a 30-component example library* | 16.35 kB | 20.09 kB | 15.94 kB | 19.38 kB | 30.30 kB |

*The bundle size benchmark comparisons can be found here: [https://webcomponents.dev/blog/all-the-ways-to-make-a-web-component-april2020/](https://webcomponents.dev/blog/all-the-ways-to-make-a-web-component-april2020/).

*Edit: WebComponents.dev have recently updated their blog post that I've referenced throughout this article. You can check out their latest version here: [https://webcomponents.dev/blog/all-the-ways-to-make-a-web-component/](https://webcomponents.dev/blog/all-the-ways-to-make-a-web-component/).*
