# Protecting Against XSS Attacks in React

Cross-site scripting (XSS) attacks are a type of attack in which malicious code is injected into a web page and then executed. It’s one of the most common forms of cyber attacks that front-end web developers have to deal with, so it’s important to know how the attack works and how to protect against it.

In this article, we’ll look at a few code examples written in React so that you too can protect your site and your users.

---

## Example 1: Successful XSS Attack in React

For all of our examples, we’re going to implement the same basic functionality. We’ll have a search box on the page that a user can enter text into. Hitting the “Go” button will simulate running the search, and then some confirmation text will be displayed on the screen that repeats back to the user what term they searched for. Pretty standard behavior for any site that allows you to search.

![Search demo](https://dev-to-uploads.s3.amazonaws.com/i/7xqkoxkwas8uwa60vg75.png)
<figcaption>Search demo</figcaption>

Simple enough, right? What could go wrong?

Well, what if we entered some HTML into the search box? Let’s try out the following snippet:

```HTML
<img src="1" onerror="alert('Gotcha!')" />
```

What happens now?

![XSS attack is executed](https://dev-to-uploads.s3.amazonaws.com/i/l30x0pnnvwzan1jb7ads.png)
<figcaption>XSS attack is executed</figcaption>

Whoa, the `onerror` event handler was executed! That’s not what we want. We just unwittingly executed a script from untrusted user input.

![Broken image is rendered](https://dev-to-uploads.s3.amazonaws.com/i/skhzsgiqburobltpdub9.png)
<figcaption>Broken image is rendered</figcaption>

And then the broken image is rendered on the page. That’s not what we want either.

So how did we get here? Well, in the JSX for rendering the search results in this example, we’ve used the following code:

```JSX
<p style={searchResultsStyle}>
  You searched for: <b><span dangerouslySetInnerHTML={{ __html: this.state.submittedSearch }} /></b>
</p>
```

The reason the user input was parsed and rendered is because we used the `dangerouslySetInnerHTML` attribute, a feature in React which works just like the native `innerHTML` browser API, which is generally regarded as unsafe to use for this very reason.

---

## Example 2: Failed XSS Attack in React

Now let’s look at an example that successfully protects against the XSS attack. The fix here is pretty simple. To render the user input safely, we should just not use the `dangerouslySetInnerHTML` attribute. Instead, let’s write our output code like this:

```JSX
<p style={searchResultsStyle}>You searched for: <b>{this.state.submittedSearch}</b></p>
```

We’ll enter the same input, but this time, here’s the output:

![XSS attack is prevented](https://dev-to-uploads.s3.amazonaws.com/i/viiw3nnqhdplwceyqpga.png)
<figcaption>XSS attack is prevented</figcaption>

Nice! The user input was rendered to the screen as text only. Threat neutralized.

That’s good news! React by default will escape the content that it renders, treating all data like a text string. This is the equivalent of using the native `textContent` browser API.

---

## Example 3: Sanitizing HTML Content in React

So, the advice here seems pretty easy. Just don’t use `dangerouslySetInnerHTML` in your React code, and you’re golden. But what if you find yourself needing to use this feature?

For example, maybe you’re pulling in content from a content management system (CMS) like Drupal, and some of this content contains markup. (As an aside, I’d probably recommend against including markup in your text content and translations from a CMS in the first place, but for this example, we’ll assume that you’ve been overruled and that the content with markup in it is here to stay.)

In that case, you *do* want to parse the HTML and render it on the page. So how do you do that safely?

The answer is to *sanitize* your HTML before rendering it. Rather than escaping the HTML entirely, instead you’ll run the content through a function to strip out any potentially malicious code before rendering.

There are many good HTML sanitization libraries out there that you can use. As with anything cybersecurity-related, it’s a good idea to never write any of this yourself. There are people out there far smarter than you are, both good guys and bad guys, that have given this more thought than you have. Always go with a battle-proven solution.

One of my favorite sanitization libraries is called [sanitize-html](https://www.npmjs.com/package/sanitize-html), and it does exactly what the name implies. You start with some dirty HTML, run it through a function, and then you get some nice, clean, safe HTML as the output. You can even customize what HTML tags and attributes are allowed if you want more control than their default settings provide.

*Update: An even smaller library I'd recommend is [dompurify](https://www.npmjs.com/package/dompurify). It has a minified and gzipped size of only [6.4 kB](https://bundlephobia.com/result?p=dompurify@2.2.2), as opposed to sanitize-html's whopping [49.7 kB](https://bundlephobia.com/result?p=sanitize-html@2.1.2). The API follows the same format by taking dirty input and returning sanitized output using options that you can customize.*

![Sanitize your HTML](https://dev-to-uploads.s3.amazonaws.com/i/5zd194wsji1bk8r0x7fh.png)
<figcaption>Sanitize your HTML</figcaption>

---

## Conclusion

There you have it. How XSS attacks are executed, how you can prevent them, and how you can safely parse HTML content when necessary. Happy coding, and be safe out there!

*The full code examples can be [found on GitHub](https://github.com/thawkin3/xss-demo).*
