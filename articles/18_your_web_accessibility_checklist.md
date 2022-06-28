# Your Web Accessibility Checklist

Web accessibility is important. Making your app accessible is morally the right thing to do, it helps you win more business, and it may even be legally or contractually required. However, not many software engineers or web designers know how to make their apps accessible.

The W3C has published its [Web Content Accessibility Guidelines](https://www.w3.org/TR/WCAG21/), and these recommendations have been publicly available for a long time. However, this document is long and complex, and it’s difficult to get through and to understand.

---

## The Solution

Checklists are easy. They give you actionable things to do.

So, without further ado, I’ve compiled a **web accessibility checklist** for complying with the standards found in **WCAG 2.1 AA**. This checklist can be used by software engineers, web designers, QA engineers, and anyone else that has an interest in making their web applications more accessible.

*See something missing? Let me know in the comments below.*

---

## Web Accessibility Checklist

*Note: I’ve grouped the checklist items into related sections in as logical a way as I could. There may be some overlap between sections.*

---

### Buttons

✅ Buttons are used for triggering actions, not for navigating.

✅ All buttons have clear labels explaining their purpose.

✅ Icon-only buttons have an `aria-label` attribute that provides additional information for screen reader users.

✅ Buttons have contextual labels that provide information for screen reader users about which item this action is being performed on. (For example, if you have a list of 10 items in a to-do list, and each one has a Delete button, you need to provide a contextual label in the form of an `aria-label` on each button so that the screen reader will see something like “Delete item: walk the dog”.)

✅ `div` and `span` elements with click handlers are not used in place of button elements.

---

### Links

✅ Links are used for navigating, not for triggering actions.

✅ Links have contextual labels that provide more information for screen reader users. (For example, if your text is simply “Read more”, either change the visible text to “Read more about web accessibility” or else leave the text as “Read more” and add an `aria-label` that says “Read more about web accessibility”.)

---

### Images

✅ Text is not used in images.

✅ Each image has an `alt` attribute. (Background images or other images that you want the screen reader to ignore can have an empty `alt` attribute like `alt=""`.)

✅ Alt text describes the intent of the image, not describes the image literally.

---

### Headings

✅ The page has one and only one `h1` element.

✅ The first heading on the page is an `h1` element. (Usually. If you have heading elements inside the navigation, you may make an exception.)

✅ Heading levels are not skipped. (For example, an `h2` element must not be followed by an `h4` element without an `h3` element between them).

---

### Forms

✅ All inputs have associated labels.

✅ Placeholder text is used for example input and not as a label.

✅ Formatting expectations for user input are displayed.

✅ Required fields are easily identified.

✅ Color is not the only indicator for feedback. (You should use a combination of color, icons, and text.)

✅ Error messages are helpful and appear close to the input.

✅ Every element is reachable by the keyboard.

✅ Inputs and buttons have focus indicators.

✅ Tab order makes sense.

✅ `fieldset` and `legend` elements are used to group inputs.

---

### Modals

✅ When closing a menu or modal, focus is returned to where it was previously (for instance, back to the button that the user clicked to open the modal).

✅ When a modal is open, hitting the Escape key closes the modal.

✅ When a modal opens, focus starts on the Close button or on the first element inside the modal.

✅ When a modal is open, keyboard-only users and screen reader users are restricted to only interacting with the content inside of the modal. (They should not be able to focus on, read, or interact with content outside of the modal in any way).

---

### Tables

✅ Tables are not used for layout purposes. Tables are used to present data.

✅ Tables have clear headings on every column or row (use the `scope` attribute).

---

### Toasts

✅ Users have enough time to read and respond to toasts or other messages on the screen. (For example, toasts generally shouldn’t disappear automatically after a few seconds, especially if there are buttons in the toast that a user may need to interact with. It’s a much better practice to allow the user to dismiss a toast when they are ready.)

---

### Drawers/Panels

✅ Content that expands and collapses allows the screen reader to read the current state for whether the content is expanded or collapsed.

---

### Color

✅ Text has a color contrast ratio of at least 4.5:1 between foreground and background. (Note: Logos and disabled interactive content have no color contrast requirements.)

✅ Large text has a color contrast ratio of at least 3:1 between foreground and background.
Skip Links

✅ The very first element on the page is a visually hidden button that appears when focused on and allows the user to skip past the navigation to the main page content.

---

### Touch Targets

✅ Touch targets are at least the size of an average person’s finger (at least 44x44px).

✅ There is adequate space between touch targets.

---

### Focus Indicators

✅ It is easy to see what element is currently active or focused.

✅ Focus indicators are not color-only.

---

### Typography

✅ The base font size is around 16–20px.

✅ `em` or `rem` is used instead of `px` to create a more responsive experience when changing the font size.

✅ Text that is in all caps is cased normally in the HTML and transformed to all caps using CSS.

✅ Content is not aligned “justified”.

---

### Language

✅ The `lang` attribute is present on the `html` tag.

✅ Content that is in a different language than the rest of the page is wrapped in its own `lang` attribute.

✅ Abbreviations are inside `abbr` tags and also have a `title` attribute included which contains the full name (for instance, the full name of an organization if only the acronym is shown on the page).

---

### Layout

✅ The user can increase the font size of the text on the page by up to 200% without loss of content or functionality.

✅ The page layout doesn’t require the user to scroll both horizontally and vertically. (Preferably, only vertical scrolling is needed.)

---

### Audio

✅ Audio has a transcript provided either inline or as a link to a text file.

✅ Audio is not autoplayed.

✅ Audio longer than three seconds can be paused or stopped by the user.

---

### Video

✅ Video is captioned or has a transcript provided either inline or as a link to a text file.

✅ Captions can be toggled on and off.

✅ Non-visual cues are narrated/captioned (for example, when a character laughs).

✅ Video is not autoplayed.

✅ Video longer than three seconds can be paused or stopped by the user.

---

### Screen Readers

✅ All visible text can be read by a screen reader ([VoiceOver](https://www.apple.com/voiceover/info/guide/_1124.html), [JAWS](https://www.freedomscientific.com/products/software/jaws/), [NVDA](https://www.nvaccess.org/)).

✅ All interactions can be completed using only a screen reader.

✅ The screen reader makes an announcement if the page content changes significantly (for instance, new search results have loaded or page content has been filtered by some criteria).

✅ All error messages are announced by the screen reader.

✅ When content is deleted or removed from the page, focus is moved backwards to something previously seen rather than forwards.

✅ Screen readers can focus and read disabled elements (for example, a disabled button).

✅ Content that is inherently inaccessible, like drag and drop functionality that requires a mouse, has an alternative implementation that screen reader users can use.

---

### Keyboard-Only

✅ All interactions can be completed using only the keyboard.

✅ There are no keyboard traps (places that you can get into via the keyboard but can’t get out of via the keyboard).

✅ There are visual focus indicators for every focusable element on the page.

✅ The tab order is consistent, and focus moves through the page in a way that makes sense.

✅ Tabbing with the keyboard does not focus on disabled elements (for example, a disabled button).

✅ Content that is inherently inaccessible, like drag and drop functionality that requires a mouse, has an alternative implementation that keyboard-only users can use.

---

## Conclusion

I hope this checklist greatly simplifies your discussions as a software company regarding making your application accessible. Again, if you think something is missing or incorrect, please let me know in the comments!

If you’d like to learn more, I’ve included links to more great resources below.

---

## Resources for Further Learning

- [Web Content Accessibility Guidelines (WCAG) 2.1](https://www.w3.org/TR/WCAG21/)
- [How to Meet WCAG (Quick Reference)](https://www.w3.org/WAI/WCAG21/quickref/)
- [Developing Websites for Accessibility: Getting Started (Pluralsight course)](https://app.pluralsight.com/library/courses/developing-websites-accessibility-getting-started/table-of-contents)
- [Meeting Web Accessibility Guidelines (Section 508/ WCAG 2.1) (Pluralsight course)](https://app.pluralsight.com/library/courses/web-accessibility-meeting-guidelines/table-of-contents)
- [Develop Accessible Web Apps with React (egghead.io course)](https://egghead.io/courses/develop-accessible-web-apps-with-react)
- [WCAG 2.1, Simplified: How to Make Your Website Accessible (Medium article)](https://levelup.gitconnected.com/wcag-2-1-simplified-how-to-make-your-website-accessible-1cfadd03d20d)
- [Designing accessible forms: the 10 foundational rules (Medium article)](https://uxdesign.cc/10-tips-for-designing-accessible-forms-a016dae8e9aa)

---

## Resources for Accessibility Testing

- [eslint-plugin-jsx-a11y](https://www.npmjs.com/package/eslint-plugin-jsx-a11y)
- [react-axe](https://www.npmjs.com/package/react-axe)
- [axe Chrome extension](https://chrome.google.com/webstore/detail/axe-web-accessibility-tes/lhdoppojpmngadmnindnejefpokejbdd)
- [Accessibility Insights for Web Chrome extension](https://chrome.google.com/webstore/detail/accessibility-insights-fo/pbjjkligggfmakdaogkfomddhfmpjeni)
- [tota11y Chrome extension](https://chrome.google.com/webstore/detail/tota11y-plugin-from-khan/oedofneiplgibimfkccchnimiadcmhpe)
- [High Contrast Chrome extension](https://chrome.google.com/webstore/detail/high-contrast/djcfdncoelnlbldjfhinnjlhdjlikmph)
- [Color Contrast Checker](https://webaim.org/resources/contrastchecker/)
- [Color Review](https://color.review/)
- [Accessible Color Picker Chrome extension](https://chrome.google.com/webstore/detail/accessible-color-picker/bgfhbflmeekopanooidljpnmnljdihld)
