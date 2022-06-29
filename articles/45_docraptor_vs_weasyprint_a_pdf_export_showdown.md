# DocRaptor vs. WeasyPrint: A PDF Export Showdown

I recently published an [article comparing HTML-to-PDF export libraries](https://levelup.gitconnected.com/how-to-convert-html-tables-into-beautiful-pdfs-eac2ce4c77de). In it, I explored options like the native [browser print](https://developer.mozilla.org/en-US/docs/Web/API/Window/print) functionality, open-source libraries [jsPDF](https://github.com/MrRio/jsPDF) and [pdfmake](http://pdfmake.org/), and the paid service [DocRaptor](https://docraptor.com/). Here's a quick recap of my findings:

> If you want the simplest solution and don’t need a professional-looking document, the native browser print functionality should be just fine. If you need more control over the PDF output, then you’ll want to use a library.

> jsPDF shines when it comes to single-page content generated based on HTML shown in the UI. pdfmake works best when generating PDF content from data rather than from HTML. DocRaptor is the most powerful of them all with its simple API and its beautiful PDF output. But again, unlike the others, it is a paid service. However, if your business depends on elegant, professional document generation, DocRaptor is well worth the cost.

In the comment section for my article on Dev.to, [one person suggested](https://dev.to/thawkin3/how-to-convert-html-tables-into-beautiful-pdfs-1k08/comments) I take a look at [Paged.js](https://www.pagedjs.org/) and [WeasyPrint](https://weasyprint.org/) as additional alternatives to consider. (This person is Andreas Zettl by the way, and he has an awesome demo site full of [Print CSS examples](https://printcss.live/).)

So today we'll explore the relative strengths and weaknesses of DocRaptor and WeasyPrint.

---

## WeasyPrint Overview

Let's start with WeasyPrint, an open-source library developed by [Kozea](https://kozea.fr/) and supported by [Court Bouillon](https://www.courtbouillon.org/). For starters, **it's free**, which is a plus. It's licensed under the [BSD 3-Clause License](https://github.com/Kozea/WeasyPrint/blob/master/LICENSE), a relatively permissive and straightforward license. WeasyPrint allows you to generate content as either a PDF or a PNG, which should adequately cover most use cases. It's built for Python 3.6+, which is great if you're a Python developer. If Python is not your forte or not part of your company's tech stack, then this may be a non-starter for you.

One of the biggest caveats to be aware of is that [WeasyPrint does not support JavaScript-generated content](https://github.com/Kozea/WeasyPrint/issues/454)! So when using this library, you'll need to be exporting content that is generated server-side. If you are relying on dynamically generated content or charts and tables powered by JavaScript, this library is not for you.

---

## Installing WeasyPrint

Getting up and running with WeasyPrint is fairly easy. They provide [installation instructions](https://weasyprint.readthedocs.io/en/latest/install.html) on their website, but I use `pyenv` to [install and manage Python](https://opensource.com/article/19/5/python-3-default-mac) rather than Homebrew, so my installation steps looked more like this:

Installing `pyenv` and Python:

```sh
# install pyenv using Homebrew
brew install pyenv

# install Python 3.7.3 using pyenv
pyenv install 3.7.3

# specify that I'd like to use version 3.7.3 when I use Python
pyenv global 3.7.3

# quick sanity check
pyenv version

# add `pyenv init` to my shell to enable shims and autocompletion
echo -e 'if command -v pyenv 1>/dev/null 2>&1; then\n  eval "$(pyenv init -)"\nfi' >> ~/.zshrc
```

Installing WeasyPrint and running it against the WeasyPrint website:

```sh
pip install WeasyPrint

weasyprint https://weasyprint.org/ weasyprint.pdf
```

As you can see, the simplest way to use WeasyPrint from your terminal is to run the `weasyprint` command with two arguments: the URL input and the filename output. This creates a file called `weasyprint.pdf` in the directory from which you run the command. Here's a screenshot of the PDF output when viewed in the Preview app on a Mac:

![Sample PDF output from WeasyPrint](https://dev-to-uploads.s3.amazonaws.com/i/yvfjqcuajrw24qxepweo.png)
<figcaption>Sample PDF output from WeasyPrint</figcaption>

Looks great! WeasyPrint also has a full [page of examples](https://weasyprint.org/samples/) you can check out which showcases [reports](https://weasyprint.org/samples/report/report.pdf), [invoices](https://weasyprint.org/samples/invoice/invoice.pdf), and even [event tickets](https://weasyprint.org/samples/ticket/ticket.pdf) complete with a barcode.

---

## DocRaptor Overview

Now let's consider DocRaptor. [DocRaptor](https://docraptor.com/) is closed-source and is available through a paid license subscription (although you can generate test documents for free). It uses the PrinceXML HTML-to-PDF engine and is the only API powered by this technology.

Unlike WeasyPrint's Python-only usage, DocRaptor has SDKs for PHP, Python, Node, Ruby, Java, .NET, and JavaScript/jQuery. It can also be used directly via an HTTP request, so you can generate a PDF right from your terminal using cURL. This is great news if you're someone like me who doesn't have Python in their arsenal.

DocRaptor can export content as a PDF, XLS, or XLSX document. This can come in handy if your content is meant to be a table compatible with Excel. For the time being though, we'll just look at PDFs since that's something both WeasyPrint and DocRaptor support.

One relative strength of DocRaptor compared to WeasyPrint is that it *can* wait for JavaScript on the page to be executed, so it's perfect for use with dynamically generated content and charting libraries.

---

## Getting Started with DocRaptor

DocRaptor has [guides for each of their SDKs](https://docraptor.com/documentation/jquery) that are well worth reading when first trying out their service. Since we ran the WeasyPrint example from the command line, let's also [run DocRaptor in our terminal](https://docraptor.com/documentation) by using cURL to make an HTTP request. DocRaptor is API-based, so there's no need to download or install anything.

Here's their example you can try:

```sh
curl http://YOUR_API_KEY_HERE@docraptor.com/docs \
  --fail --silent --show-error \
  --header "Content-Type:application/json" \
  --data '{"test": true,
           "document_url": "http://docraptor.com/examples/invoice.html",
           "type": "pdf" }' > docraptor.pdf
```

And here's the output after running that code snippet in your terminal:

![Sample PDF output from DocRaptor](https://dev-to-uploads.s3.amazonaws.com/i/hecltzcc7xfarminmsiy.png)
<figcaption>Sample PDF output from DocRaptor</figcaption>

Voila: a nice and simple invoice. DocRaptor's example here isn't as complex as WeasyPrint's was, so let's try generating a PDF from one of DocRaptor's more advanced examples.

```sh
curl http://YOUR_API_KEY_HERE@docraptor.com/docs \
  --fail --silent --show-error \
  --header "Content-Type:application/json" \
  --data '{"test": true,
           "document_url": "https://docraptor.com/samples/cookbook.html",
           "type": "pdf" }' > docraptor_cookbook.pdf
```

Here's the output for this cookbook recipe PDF:

![Sample PDF output from DocRaptor using their Cookbook Recipe example](https://dev-to-uploads.s3.amazonaws.com/i/j20ps32u6n34ss6lejh1.png)
<figcaption>Sample PDF output from DocRaptor using their Cookbook Recipe example</figcaption>

Pretty neat! Just like WeasyPrint, DocRaptor can handle complex designs and full-bleed layouts that extend to the very edge of the page. One important callout here is that DocRaptor supports footnotes, as seen in this example. WeasyPrint, on the other hand, has not yet fully implemented the [CSS paged media specifications](https://www.w3.org/TR/css-page-3/), so it can't handle footnote generation.

You can view more DocRaptor [examples on their site](https://docraptor.com/samples) including a [financial statement](https://docraptor.com/samples/statement.pdf), a [brochure](https://docraptor.com/samples/brochure.pdf), an [invoice](https://docraptor.com/samples/invoice.pdf), and an [e-book](https://docraptor.com/samples/ebook.pdf).

---

## JavaScript Execution

So far we've seen the powers and similarities of both DocRaptor and WeasyPrint. But one core difference we touched on above is that WeasyPrint does not wait for JavaScript to execute before generating the PDF. This is crucial for applications built with a framework like React. By default, React apps contain only a root container `div` in the HTML, and then JavaScript runs to inject the React components onto the page.

So if you try to generate a PDF from the command line for an app built with React, you won't get the actual app content! Instead, you'll likely see the content of the `noscript` tag, which typically contains a message stating something like "You need to enable JavaScript to run this app."

This is also the case for applications that rely on charting libraries like [Google Charts](https://developers.google.com/chart), [HighCharts](https://www.highcharts.com/), or [Chart.js](https://www.chartjs.org/). Without the JavaScript running, no chart is created.

As an example, [consider this simple web page](http://tylerhawkins.info/docraptor-js-demo/) I've put together. It contains a page header, a paragraph included in the HTML source code, and a paragraph inserted into the DOM by JavaScript. You can find the [code on GitHub](https://github.com/thawkin3/docraptor-js-demo). Here's what the page looks like:

![DocRaptor JS demo web page](https://dev-to-uploads.s3.amazonaws.com/i/az5421zkewrtl5ql66tv.png)
<figcaption>DocRaptor JS demo web page</figcaption>

Now, let's use WeasyPrint to generate a PDF from the web page by running the following command in the terminal:

```sh
weasyprint http://tylerhawkins.info/docraptor-js-demo/ weasyprint_js_demo.pdf
```

Here's the output:

![JS demo PDF output from WeasyPrint](https://dev-to-uploads.s3.amazonaws.com/i/5kd2os3p0cpwggts5h1m.png)
<figcaption>JS demo PDF output from WeasyPrint</figcaption>

Oh no! Where's the second paragraph? It's not there, because the JavaScript was never executed.

Now let's try again, but this time with DocRaptor. In order to have JavaScript run on the page, we must provide DocRaptor with the `"javascript": true` argument in our options object. Here's the code:

```sh
curl http://YOUR_API_KEY_HERE@docraptor.com/docs \
  --fail --silent --show-error \
  --header "Content-Type:application/json" \
  --data '{"test": true,
           "javascript": true,
           "document_url": "http://tylerhawkins.info/docraptor-js-demo/",
           "type": "pdf" }' > docraptor_js_demo.pdf
```

And the output:

![JS demo PDF output from DocRaptor](https://dev-to-uploads.s3.amazonaws.com/i/sml1n9q1p483oq5pks7x.png)
<figcaption>JS demo PDF output from DocRaptor</figcaption>

Tada! The JavaScript has been successfully executed, leading to the insertion of the second paragraph.

---

## Conclusion

So, which should you use, WeasyPrint or DocRaptor? It depends on your use case. 

If your app contains static content that doesn't rely on JavaScript, if Python is part of your tech stack, or if you need PNG image output, then WeasyPrint is an excellent choice. It's open source, free, and flexible enough to handle visually complex output.

If you need to use a programming language other than Python, or you rely on the execution of JavaScript to render the content you need exported, DocRaptor is the right choice.

---

## Table of Comparisons

As an added bonus, here's a comparison table for a quick summary of these two libraries:

![DocRaptor vs. WeasyPrint comparison table](https://dev-to-uploads.s3.amazonaws.com/i/mgms7ei6rr7fe83h37oh.png)
<figcaption>DocRaptor vs. WeasyPrint comparison table</figcaption>

Happy coding!
