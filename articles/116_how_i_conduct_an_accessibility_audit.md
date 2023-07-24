# How I Conduct an Accessibility Audit

Screen reader testing, keyboard-only navigation, and more

![Photo by [Agence Olloweb](https://unsplash.com/@olloweb?utm_source=medium&utm_medium=referral) on [Unsplash](https://unsplash.com?utm_source=medium&utm_medium=referral)](https://cdn-images-1.medium.com/max/8576/0*eIE2BQ0yemhVw_iQ)*Photo by [Agence Olloweb](https://unsplash.com/@olloweb?utm_source=medium&utm_medium=referral) on [Unsplash](https://unsplash.com?utm_source=medium&utm_medium=referral)*

I’ve spent the greater part of my career thus far as a frontend software engineer with an emphasis on accessibility and design systems. As such, a frequent part of my job is conducting accessibility audits for various pages and features.

Sometimes I’ll be asked to conduct an audit of an existing set of pages so that we can remediate it. Other times I’ll be asked to audit a feature to check for accessibility issues before it’s released. Most frequently, I’m asked for accessibility code review feedback as part of the pull request process.

Whatever the circumstances may be, the way I perform the accessibility audit is roughly the same. I thought it’d be helpful to outline my process here so that others who are new to the accessibility world have somewhat of a template to follow.

---

## Main Areas of the Audit

I generally conduct my audit in a series of passes through the page or feature:

1. Manual testing using keyboard-only navigation

1. Manual testing using screen reader navigation

1. Automated testing using the axe DevTools browser extension and other accessibility tools

Let’s examine each of these phases.

---

### Keyboard-Only Navigation

The first time I pass through the page or feature, I try to operate the controls and navigate the page using only the keyboard.

This helps me check that all interactive content can be reached via the keyboard and that the tab order on the page makes sense. Sometimes I’ll find that the developers created an interface that can be easily used with a mouse but that can’t be used at all with a keyboard. Or, it may be mostly keyboard accessible, but some pieces are unreachable via keyboard and need to be fixed.

During this phase I also check that the various controls respond to the appropriate keypresses. For example, buttons should respond to the Enter and Space keys. Links should be activated using the Enter key. Menu items should be navigable using the Up and Down arrow keys.

While navigating with the keyboard, I check that focusable items have visible focus indicators with appropriate color contrast. Sometimes designers or developers will intentionally hide the focus outline because they don’t like how it looks, but this is unkind to keyboard-only users, as it creates confusion when trying to identify what the current active item is. Style your focus indicators however you’d like, but please ensure that they exist and can be easily seen.

As I use the app, I also ensure that the focus is sent to the right place when interacting with content like modals or dropdown menus. Modals, for example, should have the focus sent inside the modal when it opens, focus should be trapped inside the modal while it’s open, and focus should be sent back to the trigger button when the modal closes. Similarly, when interacting with menu items in a dropdown menu, if the user presses the Escape key, the menu should be closed and focus should return to the dropdown menu trigger button.

Finally, when using only the keyboard, I check for any unintentional focus traps. We don’t want a user to be able to navigate their way into an area that they have no way of navigating out of.

---

### Screen Reader Navigation

Next, I make a second pass through the page or feature using a screen reader. I primarily use a Mac, so my screen reader of choice is VoiceOver, the built-in screen reader for iOS devices.

As I use the screen reader, I check to make sure that information on the page is communicated clearly to the user. For example, non-decorative images should have alt text describing the image. Otherwise, the screen reader may simply read “image” or the file name, both of which are generally unhelpful. Similarly, icon-only buttons, like a Delete button which only displays a trash can icon without any text beside it, need to have an aria-label attribute so that the screen reader users know what the button is for.

I also check that the correct ARIA roles are communicated to the screen reader. For example, sometimes developers will use a link element when they really should have used a button element. In this scenario, the `href` attribute is overridden and an `onclick` handler is provided so that when the “button” is pressed, an action is taken but no navigation occurs. This is fine and dandy for a sighted mouse user, but when a screen reader user encounters the element, it will be read as “link” rather than “button”, which is misleading to the user.

The key here during this part of the audit is that all of the content needs to clearly and correctly communicate what it is and does. This is often achieved using semantic HTML elements along with occasional ARIA attributes.

If anything feels confusing or misleading while using a screen reader, it’s likely a usability issue.

---

### Automated Testing Tools

As you may have already noted, the first two phases of my audit are very manual. It’s nice to use automated testing tools, but the reality is that these automated testing tools are somewhat limited in the kinds of issues they can find.

For now, there really is no substitute for good manual accessibility testing. It’s important to have empathy for our users and make sure that the content is actually usable for humans.

That being said, automated accessibility testing tools are still an essential part of the audit. One free tool I like to use is the axe DevTools browser extension created by Deque. With this extension, you simply navigate to the page you want to audit, click the Scan button, and then view the results.

The axe DevTools extension uses the axe-core rules underneath the hood. These rules are great at finding things like incorrect ARIA attribute usage, duplicate element IDs, poor color contrast, and out-of-order heading levels.

The findings from this output help me catch any last issues that I didn’t already identify during my keyboard and screen reader testing.

---

## Conclusion

That’s it! The specifics of the accessibility audit may change depending on the feature or the goal of the audit, but the fundamentals are always the same. 

If the content can be used with a keyboard and screen reader, and automated accessibility tools aren’t reporting issues, then you’re well on your way to creating more accessible content.

Thanks for reading, and thanks for being an accessibility ally!
