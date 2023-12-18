# Accessibility Audit Prioritization Levels

As accessibility experts, we often perform accessibility audits on web content and then send a punch list of items that need to be fixed back to the developers.

As developers, it can be discouraging to receive a long list of items to remediate. In the back of our head we may be thinking, “Great, this will take weeks to fix. How am I supposed to slot this into my current workload? And which items should I fix first?”

To enable accessibility subject matter experts and developers to work well together, it’s helpful to think about the feedback in terms of importance.

## Priority Levels

I often rank my accessibility feedback issues on a scale of P0-P3 (priority 0–3). You can come up with your own definitions, but often it’s something like:

**P0: Blocker.** A user with a certain disability can’t use this feature at all.

**P1: Partial blocker.** A user with a certain disability can use this feature, but it’s very difficult or unintuitive.

**P2: Minor improvement.** A user with a certain disability can use this feature, but there are some small improvements that could be made.

**P3: Best practice, but not required.** The feature is usable, but we could implement it differently to provide an even better user experience. This often means adding additional functionality rather than just fixing existing content.

Adding these priority levels to each ticket makes it crystal clear which items are most important and should be worked on first.

(I should add that this is helpful not just for accessibility audits, but for any kind of bug bash in which a long list of issues will be created. Let people know which items are critical to fix and which ones can wait.)

## Some Examples

Let’s look at a few example issues for each priority level.

### P0: Blocker

* Some drag and drop functionality has no alternative method for moving content using the keyboard, so keyboard and screen reader users can’t use this feature at all.

* An unintended focus trap exists on an element, so once a keyboard user tabs to it, they can’t move away or get out of the focus trap without refreshing their page.

* Video content has no captions, so users who are deaf or hard of hearing have no way of knowing what is being said.

### P1: Partial blocker

* The hit areas of clickable buttons are too small. The buttons are clickable, so they’re not unusable, but mouse users with hand tremors or other motor disabilities may have a difficult time clicking them.

* Focus is not sent into a modal when it opens. Keyboard and screen reader users can still get to the modal content by tabbing or navigating through the entire page until they reach the modal, but doing so is tedious and not a great user experience.

### P2: Minor improvement

* The color contrast ratio on some content is barely under the required 4.5:1 color contrast ratio.

* A dropdown menu trigger button is not indicating its expanded or collapsed state due to a missing `aria-expanded` attribute.

* The heading levels used on the page do not properly represent the page’s hierarchy of information.

### P3: Best practice, but not required

* A skip link could be added to the site to allow users to skip past the navigation links and header content that is repeated on every page.

* An avatar image for a user has an alt attribute containing the user’s name, and the user’s name is displayed next to the avatar, resulting in redundant information provided to the screen reader.

* A complex table could be redesigned to present the data in a simpler way.

You may not agree with the priority level I’ve given each specific example, but hopefully the idea is clear.

## Conclusion

Not all accessibility issues are created equal. Have empathy for your users and seek to understand which issues are causing the most frustration and which fixes are only nice to have.

Let’s work together in such a way that we prioritize the most important fixes first.

And as always, thanks for being an accessibility ally.
