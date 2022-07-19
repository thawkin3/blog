# Designing accessible forms: the 10 foundational rules

How to design forms with every user in mind.

![Photo by [Mockaroon](https://unsplash.com/@mockaroon?utm_source=medium&utm_medium=referral) on [Unsplash](https://unsplash.com?utm_source=medium&utm_medium=referral)](https://cdn-images-1.medium.com/max/10248/0*e54SWxHTT2fXuUZB)
*Photo by [Mockaroon](https://unsplash.com/@mockaroon?utm_source=medium&utm_medium=referral) on [Unsplash](https://unsplash.com?utm_source=medium&utm_medium=referral)
*

I’m sure at some point in our lives we’ve all had a painful experience filling out a form on the web. Confusing inputs, unclear expected formats, cryptic error messages, lack of keyboard-accessibility… The list could go on and on.

Now imagine including a disability or impairment on top of that experience (blindness, color-blindness, motor skill impairments, cognitive disabilities, etc).

Forms are needed for all sorts of things online. You use them to sign up for a service, or to log in to your account, or to place an order on an e-commerce site. Forms are everywhere!

So as designers and developers, let’s make them good.

Below are 10 guidelines you can follow to make your forms more accessible and more user-friendly today.

---

## 1. All inputs should have associated labels

Labels are important for identifying your inputs. They help sighted users as well as blind users know what each input is for. Labels are especially important for screen readers so that they can correctly announce what each input is. You should use the actual `label` HTML element as well and not simply a `span` or `div` element.

### Good Example:

![Good: Inputs with associated labels](https://cdn-images-1.medium.com/max/2000/1*9RhCgCusCif2yStcks1s1g.png)
*Good: Inputs with associated labels*

### Bad Example:

![Bad: No labels. What am I filling out here?!](https://cdn-images-1.medium.com/max/2000/1*aIribzq-j7FzeVO6JR_PZw.png)
*Bad: No labels. What am I filling out here?!*

---

## 2. Placeholder text should be used for examples

Placeholder text is meant to be helpful as an example response. It should *not* be used as a replacement for a label. The problem with trying to use placeholder text as a label is that the placeholder text disappears once the user has entered some information, so the label is now effectively missing.

### Good Example:

![Good: Placeholder text as an example response](https://cdn-images-1.medium.com/max/2000/1*xynVCmifccsYBCuIank4sw.png)
*Good: Placeholder text as an example response*

### Bad Example:

![Bad: Placeholder text being used as a label](https://cdn-images-1.medium.com/max/2000/1*ncUuxoek__QmjgbqGO-9Qg.png)
*Bad: Placeholder text being used as a label*

Placeholder text also does not need to be used to repeat what the label says. If your label says “First Name”, it’s redundant to have placeholder text that also says “First Name”.

### Bad Example:

![Bad: Redundant placeholder text](https://cdn-images-1.medium.com/max/2000/1*DbCr-lrvZe6Z30PQesWREQ.png)
*Bad: Redundant placeholder text*

---

## 3. Formatting expectations should be displayed

If you are expecting the user to enter some information in a particular format, tell them up front! For example, if you need their birthdate to be entered in the `MM/DD/YYYY` format, display that somewhere near the input.

It’s incredibly frustrating to enter something that you think is valid input, only to receive an error message when submitting your form that you’ve provided an invalid format for a field.

### Good Example:

![Good: Clear formatting expectations for the birthdate](https://cdn-images-1.medium.com/max/2000/1*FehlZp-uiqARVaxd_Em3PQ.png)
*Good: Clear formatting expectations for the birthdate*

### Bad Example:

![Bad: How should I enter my birthdate?!](https://cdn-images-1.medium.com/max/2000/1*Lq9a400_D1x-TeipWECnMw.png)
*Bad: How should I enter my birthdate?!*

Or, even better, be flexible in what data you can accept. If you are asking for someone’s phone number, you could allow a wide variety of input values, such as:

* 8881234567
* 888 123 4567
* 888–123–4567
* (888) 123–4567

You can then parse the input yourself on the backend and strip away any non-digit characters and extra whitespace.

---

## 4. Required fields should be identified

This one is simple enough. If you have some fields that are required and some fields that are optional, make it clear which are which. Some designers prefer to mark the required fields with something like an asterisk or the text `required` near the label, and other designers prefer to mark the optional fields with something like the text (`optional`) near the label.

To me, it doesn’t seem to matter which of these approaches you choose as long as you are consistent.

### Good Example:

![Good: Required fields are clearly marked](https://cdn-images-1.medium.com/max/2000/1*r0BwF3zWr4DMbIT-JmmJKA.png)
*Good: Required fields are clearly marked*

### Bad Example:

![Bad: Assuming “Middle Name” is not required, this is not clearly shown](https://cdn-images-1.medium.com/max/2000/1*KjO6n8j0zgQOFQIABG5IUQ.png)
*Bad: Assuming “Middle Name” is not required, this is not clearly shown*

---

## 5. Color should not be the only indicator for feedback

When showing error messages, the color red is often used. For success messages, green is often used. However, color should not be the only indicator for feedback. If all you do is change the border on a text input when its value is valid or invalid, colorblind users may not be able to distinguish that difference. (In fact, red-green colorblindness is the [most common form of colorblindness](https://www.nei.nih.gov/learn-about-eye-health/eye-conditions-and-diseases/color-blindness).)

Rather, you should use a combination of color, text, and icons to display feedback such as error messages. A red X and a green checkmark are great to use, because even colorblind users can still note the obvious difference between the X and the checkmark.

### Good Example:

![Good: Feedback is a combination of color, text, and icon](https://cdn-images-1.medium.com/max/2000/1*A4VseNbF30qKmUSPfcY5Jg.png)
*Good: Feedback is a combination of color, text, and icon*

### Bad Example:

![Bad: Feedback is color-only, the red border](https://cdn-images-1.medium.com/max/2000/1*rhVgzQV95PUuECak3vmQTA.png)
*Bad: Feedback is color-only, the red border*

The same goes for buttons. Sometimes, you may have a button change color when it is hovered by the mouse or focused by the keyboard. However, something like an outline or underline indicator is much more effective at indicating focus since even users who can’t distinguish between your chosen colors will still be able to see your outlines and underlines.

### Good Example:

![Good: The focused button has an outline](https://cdn-images-1.medium.com/max/2000/1*fHGSfcL--Sz9rdEYNvUTgw.png)
*Good: The focused button has an outline*

### Bad Example:

![Bad: The focused button only changes color](https://cdn-images-1.medium.com/max/2000/1*1KS3_-dkyQ2ebpbj5rcRUA.png)
*Bad: The focused button only changes color*

---

## 6. Error messages should be helpful and close to the input

When validating a form, error messages should be displayed as soon as possible, preferably on the client-side rather than waiting until the whole form is submitted.

For example, if you have an email field, and the user enters something that is not an email, a helpful error message should be displayed right next to the form field when the input loses focus. Something like “Please provide a valid email address” should suffice for an error message.

### Good Example:

![Good: After submitting an invalid form, helpful error messages are shown](https://cdn-images-1.medium.com/max/2000/1*xNXlKxIbviDwOHajT0z8Gg.png)
*Good: After submitting an invalid form, helpful error messages are shown*

### Bad Example:

![Bad: After submitting an invalid form, an unhelpful error message is shown](https://cdn-images-1.medium.com/max/2000/1*9SYfsMLzHuuJMrsMF8O6Ag.png)
*Bad: After submitting an invalid form, an unhelpful error message is shown*

Error messages should always tell the user how to correct the error. Unhelpful error messages like “Invalid input” or “Error” are not useful to the user. Perhaps the user doesn’t understand why what they entered is not valid.

As noted in Guideline 3, if you have an input that requires a specific format for the response, make sure the user knows what the expected format is. In addition to displaying the expected format up front, you could also provide an error message such as “Please provide your birthdate in the following format: MM/DD/YYYY”.

---

## 7. Every element should be reachable by the keyboard

The keyboard is for more than just power users who want to quickly navigate between form fields. Some motor-impaired users aren’t able to use a mouse, so they solely rely on the keyboard. Screen reader users solely use the keyboard as well.

Inputs and buttons are keyboard-navigable by default, so as long as you are using the semantically correct HTML elements throughout your app, you should already get the correct functionality out the box.

The problem arises when developers do silly things like use a `div` or a `span` in place of a `button`. Please… don’t be that person. Using the correct HTML elements provides the correct handling for the keyboard and helps screen readers know what ARIA `role` each element has and how it should be treated.

---

## 8. Inputs and buttons should have focus indicators

We briefly touched on this back in Guideline 5. To reiterate, you need to clearly indicate to the user what element they are currently focused on. Rather than using color only, you should use something like an outline or an underline.

You should feel free to use CSS to disable the default browser focus indicators if you want a consistent focus indicator across each browser, but don’t simply disable these focus indicators without adding your own!

### Good Example:

![Good: The focused button has an outline](https://cdn-images-1.medium.com/max/2000/1*fHGSfcL--Sz9rdEYNvUTgw.png)
*Good: The focused button has an outline*

### Bad Example:

![Bad: Even though the second button is focused, there is no indicator to make that apparent](https://cdn-images-1.medium.com/max/2000/1*UJMZTgIDi6CuvTR7WXWjVw.png)
*Bad: Even though the second button is focused, there is no indicator to make that apparent*

---

## 9. Tab order should make sense

When a user is tabbing through your form, the tab order should be logical. For most Western languages that read left to right, your tab order should go left to right, top to bottom.

The tab order can get out of order by messing with the `tabindex` attribute on your HTML elements. Generally you’ll only want to use `-1` and `0` as values for `tabindex`.

`-1` removes the element from the natural tab order but still allows the element to be focused programmatically. `0` is good for elements that are inserted dynamically, since `0` places the element in the logical tab order according to its placement in the DOM.

Any other values where you try to specifically control the tab order of your application are probably unnecessary and may be a sign that you are doing something wrong.

And while we’re at it, don’t automatically shift the focus from one input to the next after the user has entered some information. For example, if you have a text input that asks for a two-letter state abbreviation (“UT” for Utah), don’t move the user to the next input after they’ve entered two characters.

You may think you’re being helpful, but really you’re just being annoying. Let the user control what they’re focused on.

---

## 10. Fieldsets and legends should be used to group inputs

Lengthy forms can often be broken up into logical sections. If your form is displayed on a single page, `fieldset` and `legend` HTML elements can help group your inputs and make your form more readable.

### Good Example:

![Good: Fieldsets are used to group sections of the form](https://cdn-images-1.medium.com/max/3488/1*ryfHzqahRkKiGg13ggg-Kw.png)
*Good: Fieldsets are used to group sections of the form*

### Bad Example:

![Bad: This long form has no groupings to make it more manageable](https://cdn-images-1.medium.com/max/3400/1*UwKn8iAI_a5h2_e3tH9q5w.png)
*Bad: This long form has no groupings to make it more manageable*

## Conclusion

To recap, the 10 guidelines you should follow to create accessible forms are:

1. All inputs should have associated labels
2. Placeholder text should be used for examples
3. Formatting expectations should be displayed
4. Required fields should be identified
5. Color should not be the only indicator for feedback
6. Error messages should be helpful and close to the input
7. Every element should be reachable by the keyboard
8. Inputs and buttons should have focus indicators
9. Tab order should make sense
10. Fieldsets and legends should be used to group inputs

By following these guidelines, you’ll create a much better experience for users with and without disabilities. Thanks for reading!
