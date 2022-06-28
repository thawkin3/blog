# WCAG 2.1, Simplified: How to Make Your Website Accessible

Web accessibility is an incredibly vital aspect to consider when developing a website or web application, yet so many companies either ignore accessibility guidelines or don’t understand how to implement them properly.

Some argue that making your site accessible is a moral obligation. All people, regardless of ability or disability, deserve to be able to use the internet and to access the wealth of knowledge that can be found there.

Others take a more pragmatic approach and argue that accessibility can be treated as a financial problem: is the return on investment for making your site accessible worth it? By not following accessibility guidelines, you risk alienating users with disabilities and losing their business. So, is the engineering cost worth the potential lost market share?

Personally, I fall into the first camp. I believe we all should feel morally obligated to make our sites as accessible as possible. Not necessarily because it is the most financially sensible thing to do, but because it is the right thing to do.

(Especially if your business is in the education space or provides an essential service. By not creating accessible content, you are in a sense saying, “Those with disabilities are not worth educating. You are not worth our time or attention.”)

---

I think, deep down, we as developers want to do the right thing. The problem then stems largely from the fact that many developers do not have any experience making their sites accessible or do not know how.

For instance, have you ever tried to use a screen reader on your site? Or do you know how to use a screen reader at all? Do you know what tools are out there to help audit your site for accessibility issues? Do you know and understand what guidelines you should be following?

---

## Web Content Accessibility Guidelines (WCAG)

The [Web Content Accessibility Guidelines](https://www.w3.org/TR/WCAG21/), commonly referred to just by its acronym, WCAG, are a set of guidelines and recommendations for how to make your web content more accessible. The current version of this document at the time of this writing is WCAG 2.1.

WCAG is published by the Web Accessibility Initiative (WAI) group, which is part of the World Wide Web Consortium (W3C).

Like most things from the W3C, the document is quite meaty. It’s a very technical document, very long, and potentially exhausting to read.

---

WCAG 2.1 offers 4 principles and 13 guidelines for how to improve your site’s accessibility. In this article, we’ll briefly go through each of these points.

By the end of reading this article, you should hopefully have a good idea of what guidelines you should be following and be able to see some areas of “low-hanging fruit” that would be quick wins for you to implement on your own site.

---

## WCAG 2.1 Principles

![Pour](https://miro.medium.com/max/8000/0*qvVDijgsWu0mZkuT)
<figcaption>Photo by Sarah Shaffer on Unsplash</figcaption>

WCAG 2.1 outlines four principles of accessibility. Websites must be **P**erceivable, **O**perable, **U**nderstandable, and **R**obust (**POUR**).

For content to be **perceivable**, users must be able to perceive that the content exists. It cannot be hidden from users that aren’t able to use all of their senses.

For a user interface to be **operable**, users must be able to operate and navigate the site, regardless of whether they are using a mouse, keyboard, or screen reader.

For information to be **understandable**, users must be able to understand the content on the page and also understand how to operate the page.

For a website to be **robust**, users must be able to use the site on a wide variety of devices, screen sizes, and browsers, including when used with assistive technology.

---

Now that we understand at a high level what the guiding principles of web accessibility are, let’s dive into the details of each principle. That’s where the 13 guidelines come in.

---

## 1. Perceivable

![Perceivable](https://miro.medium.com/max/10944/0*Q1XuzRyupK6AYSAa)
<figcaption>Photo by Yassine Khalfalli on Unsplash</figcaption>

To make your content perceivable, here are some guidelines to follow:

### Guideline 1.1 Text Alternatives

Provide text alternatives for any non-text content on the page. For example, non-decorative images should have an `alt` attribute that describes the image. (Decorative images like background images can just have an `alt` attribute with an empty string as its value.)

### Guideline 1.2 Time-based Media

Audio and video clips should include captions or transcripts to help the blind and deaf.

### Guideline 1.3 Adaptable

Content should be able to be displayed in different ways and in different layouts without losing information. For example, the HTML structure of your website should read in a sensible order if you were to strip away all the CSS. When a user is viewing your site on a mobile device or tablet, the viewport orientation shouldn’t be restricted to landscape-only or portrait-only.

### Guideline 1.4 Distinguishable

When presenting information such as errors while a user is filling out a form, use a combination of color and icons so that colorblind users can still understand your messages. Color should not be the only visual means of displaying information. Color contrast between foreground and background should be appropriate (in general, at least a 4.5 contrast ratio is what you should be shooting for). If audio plays on your site for more than three seconds, the user should have the ability to stop the audio or to change the volume. A user should be able to change the font size on the screen without breaking the site layout. A user shouldn’t have to scroll both vertically and horizontally to see content on the page. Buttons and inputs should have hover and focus indicators so the user knows what element is currently active. (This guideline covers a ton of stuff!)

---

## 2. Operable

![Operable](https://miro.medium.com/max/9792/0*EStnaYftAVwLUtgW)
<figcaption>Photo by Nhu Nguyen on Unsplash</figcaption>

To make your UI operable, here are some guidelines to follow:

### Guideline 2.1 Keyboard Accessible

A user should be able to use and navigate your website using only their keyboard. You can easily test this yourself by just using the Tab, Enter, Space, Escape, and arrow keys on your keyboard when visiting your own site. Can you reach every button and input on the page? Can you click every button on the page by using the Enter or Space key? (Please don’t do silly things like add click listeners to divs and spans!) Do you fall into any focus traps where you have no way to get out of a specific area of the page? Remember that not everyone uses a mouse.

### Guideline 2.2 Enough Time

Users need enough time to read the content on the page and to respond to notifications. If you have toasts that appear in your application, don’t make them disappear after a few seconds; let them stay on the page until a user dismisses them. If a user’s session is about to expire, warn them in advance and give them a way to extend their session. Unless time-based functionality is absolutely crucial to your application (for example, a ticket reservation site that only allows you to hold tickets before purchasing them for so long), don’t use timers or time limits at all.

### Guideline 2.3 Seizures and Physical Reactions

This one should be fairly straightforward. Don’t do things on your site that are known to cause seizures. Don’t have content that flashes more than three times. Don’t include frivolous animations just for fun. Any sort of movement on the page should serve a purpose and should be carefully considered.

### Guideline 2.4 Navigable

Make it easy for users to navigate your site. Add a “skip to content” button for keyboard users that allows them to skip the nav links and go straight to the main page content. Use proper heading levels, and don’t skip heading levels! (An `h3` element should not follow an `h1` element without an `h2` element first.) The headings are incredibly important for screen reader users. Make sure when tabbing through your site, the focus order makes sense. As mentioned earlier, focus indicators should be visible so that the user knows what element they are currently focused on. Your page should have a title. Breadcrumbs should be used so the user knows where they are in your site and how they got there.

### Guideline 2.5 Input Modalities

Users should be able to operate inputs with their keyboard on a desktop/laptop as well as with a touch screen on a mobile device or tablet. Don’t make buttons too small on mobile device screens so that it is difficult to press the correct button. A good minimum size for touch targets is at least 44 x 44 pixels.

---

## 3. Understandable

![Understandable](https://miro.medium.com/max/11520/0*JI-O-0tQacnPpAam)
<figcaption>Photo by bruce mars on Unsplash</figcaption>

To make your information understandable, here are some guidelines to follow:

### Guideline 3.1 Readable

Your target audience should be able to read and understand the text on the page. Avoid jargon without including explanations. Use a proper `lang` attribute on your `html` tag to define the language in which you are writing. Use the `abbr` tag for abbreviations.

### Guideline 3.2 Predictable

Make sure your website behaves in a predictable way. Be consistent in what your buttons look like and what your links look like. If you have a global navigation component that displays on every page, make sure the links are always in the same order and in the same place. If components look the same, they should behave the same.

### Guideline 3.3 Input Assistance

You should help users avoid and correct mistakes when using your application. Use labels on all your inputs. Use placeholder text where appropriate, but don’t use placeholder text in place of a label! When a user is filling out a form, provide helpful feedback messages as soon as possible. Identify exactly which input has the error and how the user can correct the error. For long forms or multi-page forms, allow users to review their data before the final submission step.

---

## 4. Robust

![Robust](https://miro.medium.com/max/10368/0*8C40FkO0psy5-66-)
<figcaption>Photo by Hal Gatewood on Unsplash</figcaption>

To make your website robust, here are some guidelines to follow:

### Guideline 4.1 Compatible

Your site should be compatible with a wide range of user agents and should be as future-proof as possible. Use correct HTML so that your content is parsed correctly. Use semantic HTML elements so that screen readers can better understand your page structure. Use elements with `role=status` and `role=alert` to provide screen readers with updates on the success or failure of actions as a user is interacting with your app.

---

## Bonus: How to Test for Accessibility

At this point, you know all the principles and guidelines contained in WCAG 2.1. But now what? Where do you start? How do you go about evaluating how well you are doing at making your website accessible?

Below are some ideas, tools, and extensions that I’ve found helpful:

---

### Manual Testing for Keyboard-Only Navigation

As suggested earlier, imagine you don’t have a mouse. Try to navigate your site using only your keyboard. This should provide some quick insights into which areas of your site are not navigable or operable for keyboard users.

---

### Manual Testing with a Screen Reader

If you don’t know how to use a screen reader, get acquainted with one. Popular choices include [JAWS](https://www.freedomscientific.com/products/software/jaws/), [VoiceOver](https://www.apple.com/voiceover/info/guide/_1124.html) for iOS, and [NVDA](https://www.nvaccess.org/) for Windows. It can be difficult at first to learn how to properly use a screen reader, but it is well worth the effort.

Once you understand the basic commands for your chosen screen reader, try using it on your site. You will quickly learn how frustrating it must be to be a blind user of the internet.

---

### Chrome Extensions

The [axe Chrome extension](https://chrome.google.com/webstore/detail/axe-web-accessibility-tes/lhdoppojpmngadmnindnejefpokejbdd) can provide analysis of any page and helps catch many accessibility issues. A complete list of their rules can be found in their GitHub repo.

The [Accessibility Insights for Web Chrome extension](https://chrome.google.com/webstore/detail/accessibility-insights-fo/pbjjkligggfmakdaogkfomddhfmpjeni) is a similar extension that can also analyze your site for accessibility issues.

---

### eslint-plugin-jsx-a11y

You can use the ESLint plugin `eslint-plugin-jsx-a11y` to lint your JSX for accessibility issues. Simply add `eslint` and this plugin to your package’s dev dependencies, along with any other plugins you typically use, and you’re all set. Like every other ESLint plugin, this one can be configured to produce either warnings or errors based on accessibility rules that you choose to follow.

---

## Conclusion

If you’ve made it this far, thanks for reading! I hope by now you understand the motivation for making your site accessible, the principles and guidelines you should follow, and how you can do some accessibility testing.

Now get out there and make the world a better and more accessible place!
