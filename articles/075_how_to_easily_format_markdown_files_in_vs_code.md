# How to Easily Format Markdown Files in VS Code

Every respectable software project needs a `README`. This file provides crucial information about what the project is, how to work with it, and other relevant information for developers. `README` files are written in *markdown*, a special markup syntax. The syntax for markdown is simple enough, but it can be a pain to manually type out, and it’s easy to make simple mistakes and typos.

Wouldn’t you like to just use the `Cmd+B` keyboard shortcut to bold some text instead of typing `**` around your text? Or what about creating a nicely formatted table in your `README`, especially when editing an existing table? Wouldn’t it be nice if the table formatting and column width adjustments were taken care of for us? Markdown is wonderful, but it’s not exactly as easy as working with a Google doc when applying formatting.

The [SFDocs Markdown Assistant VS Code extension](https://marketplace.visualstudio.com/items?itemName=salesforce.sfdocs-markdown-assistant) is here to help!

---

## Common Use Cases

In this article, we’ll look at some common use cases when writing a markdown file. We’ll first look at simple text formatting like bold, italic, or strikethrough. Next, we’ll look at writing numbered lists. Finally, we’ll look at creating and modifying tables.

Let’s get started!

---

## Bold, Italic, and Strikethrough Text

Let’s start with the simple stuff. In markdown syntax, you can make your text bold by wrapping your text in `**`, italic by wrapping your text in `_`, and strikethrough by wrapping your text in `~~`. It’s not a huge burden to type out these characters, but it’d be really nice if we could just use keyboard shortcuts to format the text in the same way that we can when working with a Google doc.

Here’s the markdown we’ll use to apply these styles:

![Markdown for bold, italic, and strikethrough text](https://dev-to-uploads.s3.amazonaws.com/uploads/articles/13zc90lapnyvrtmux2l7.png)
<figcaption>Markdown for bold, italic, and strikethrough text</figcaption>

And here’s what the output looks like when viewing the markdown file:

![Output for bold, italic, and strikethrough text](https://dev-to-uploads.s3.amazonaws.com/uploads/articles/gmlngvy5s8c511vvvxg0.png)
<figcaption>Output for bold, italic, and strikethrough text</figcaption>

Now, let’s show how easy it is to apply these various styles when using the VS Code extension! We can make our text bold using `Cmd+B`, italic using `Cmd+I`, and strikethrough using `Option/Alt+S`.

![Demo for writing bold, italic, and strikethrough text](https://dev-to-uploads.s3.amazonaws.com/uploads/articles/8wnfjrhjrducxu75q3mc.gif)
<figcaption>Demo for writing bold, italic, and strikethrough text</figcaption>

---

## Numbered Lists

Next, let’s take a look at lists. When creating a numbered list in a Google doc, hitting the `Enter` key after typing your first list item creates the second list item with the `2.` prefix already in place. In a markdown file, however, you have to manually type the number prefix for each item. It’d be nice if we could have that done for us automatically!

Here’s the markdown we’ll use to create a numbered list:

![Markdown for a numbered list](https://dev-to-uploads.s3.amazonaws.com/uploads/articles/bli1psf7xj3vjhg7tkms.png)
<figcaption>Markdown for a numbered list</figcaption>

And here’s what the output looks like when viewing the markdown file:

![Output for a numbered list](https://dev-to-uploads.s3.amazonaws.com/uploads/articles/3b6am9i7lp234mqbt108.png)
<figcaption>Output for a numbered list</figcaption>

Now, let’s show how easy it is to create a numbered list when using the VS Code extension! We start by typing `1. Bread` to create the first item in our list. Then when we hit the `Enter` key, the next number prefix is added for us automatically!

![Demo for creating a numbered list](https://dev-to-uploads.s3.amazonaws.com/uploads/articles/khwurd50yl811be8esrx.gif)
<figcaption>Demo for creating a numbered list</figcaption>

---

## Tables

Finally, let’s look at creating and modifying tables in markdown. Tables are easy enough to create, but a pain to modify, especially as the column widths change.

Here’s the markdown we’ll use to create a table:

![Markdown for a table](https://dev-to-uploads.s3.amazonaws.com/uploads/articles/z1kwg95vp0kpkogpmb0b.png)
<figcaption>Markdown for a table</figcaption>

And here’s what the output looks like when viewing the markdown file:

![Output for a table](https://dev-to-uploads.s3.amazonaws.com/uploads/articles/pzf5i8wvrwik9wabebr8.png)
<figcaption>Output for a table</figcaption>

Now, let’s see a demo of how long it takes to create the table from scratch without using the VS Code extension. Note how much time we spend modifying the column widths, and we’re only doing this for a table with three rows! Imagine doing this with a much larger dataset and how much of a headache this would be.

![Demo for manually creating a table](https://dev-to-uploads.s3.amazonaws.com/uploads/articles/4nh7re61rgrssoeys2tg.gif)
<figcaption>Demo for manually creating a table</figcaption>

Now, let’s show how easy it is to create and modify a table when using the VS Code extension. For starters, we can insert an empty table by right-clicking in our file to open the context menu and then selecting “Table >> Table: Add.” We can then specify the number of columns and rows and hit `Enter` to create the dummy layout.

Then, as we enter text into the table, we can navigate from cell to cell using `Tab` and `Shift+Tab`. Whenever those keys are pressed to navigate to a different table cell, the column widths automatically adjust. This is probably my favorite feature!

![Demo for creating a table using the VS Code extension to help](https://dev-to-uploads.s3.amazonaws.com/uploads/articles/gkm8y27bx0eijms6vdwv.gif)
<figcaption>Demo for creating a table using the VS Code extension to help</figcaption>

You can find even more cool features in the right-click context menu or by reading the [extension’s docs](https://marketplace.visualstudio.com/items?itemName=salesforce.sfdocs-markdown-assistant).

---

## Conclusion

This markdown tool is incredibly simple, but it’s often the simple things that make the developer experience better. The SFDocs Markdown Assistant looks like a VS Code extension I’ll be keeping around!
