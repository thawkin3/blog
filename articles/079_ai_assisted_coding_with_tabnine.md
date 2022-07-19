# AI-Assisted Coding with Tabnine

AI-assisted coding is intended to help you as a developer be more productive, write code faster, make fewer mistakes, and have to do less context switching between other windows and your IDE. But is AI-assisted coding a silver bullet, snake oil, or something in between?

In this article, we’ll look at the benefits as well as the potential downsides of using AI-assisted coding. We’ll also show a brief demo of using the AI-assisted coding solution [Tabnine](https://www.tabnine.com/) to help us write some code in JavaScript and React.

---

## What is AI-Assisted Coding and How Does It Work?

AI-assisted coding is fueled by a machine learning model that is trained on other code. The best of these models are trained on billions of lines of code from respectable open-source projects around the world. Through this training, the model “learns” what characters and lines of code often come after one another. Then, as you write your code, it provides auto-suggest tab completions for you, directly in your IDE.

As an extremely simple example, if you type `import React` in your IDE, the autocomplete would provide something like `from ‘react’;` to finish your statement.

But AI-assisted coding goes much further than that; it also learns from the code *you* write. The model constantly examines how you write your code and what patterns you typically follow. You can also train a more sophisticated model on your team’s code repos so that it better understands how your company writes code, which helps you be more consistent as a team.

---

## What Are the Benefits?

So, why would you opt to use an AI-powered coding assistant? Perhaps you’d rather rely on your own brain and a less fancy auto-suggest feature.

For starters, AI-assisted coding keeps you in your IDE, reducing the context switching to other windows. If the autocomplete can provide you with the right syntax, you no longer have to do a quick Google search to remember how to use an API that you’re a bit rusty on. Less context switching leads to higher productivity.

These machine learning models can also prompt you to write better code, as the code that they’re trained on often follows best practices and well-known design patterns. These nudges can be a teaching opportunity to help make you a better developer.

---

## What Are the Potential Downsides?

Now, why would you *not* want to use AI-assisted coding?

The biggest concern is privacy. If the machine learning model is being trained not just on public code but also on the code you write, then that means your code is potentially being shared with others who also use the same machine learning model. So, before using an AI-assisted coding solution, you should always look into the product’s privacy statement to understand if or how the product collects or shares data.

The second concern is how well the model is trained. As mentioned before, most models are trained on billions of lines of code. But, what if those code repos are no good? You know the old saying: “garbage in, garbage out.” If the model is trained on poor code, then the suggestions you receive will be equally poor.

---

## Demo Time

So, how helpful is AI-assisted coding in practice? To find out, I did a test drive with Tabnine, a popular solution that supports over 30 programming languages and 21 IDEs. The [VS Code extension](https://www.tabnine.com/install/vscode), for example, has nearly three million downloads.

Installing the extension is as simple as clicking the Install button, waiting a few minutes for the model to be initialized, and then restarting VS Code on your machine. Tabnine has a [privacy statement](https://www.tabnine.com/code-privacy) featured prominently on their website which states that the model only runs locally on your machine and that your data is not shared. The cool thing about this setup is that, though your data is not shared with anyone else, the model is still being trained (locally) on your code, so the suggestions get better as you use it.

Once I had Tabnine installed and ready to go, I began my coding. For this demo, I decided to write a very simple login form in JavaScript and React. The form includes inputs for the username and password as well as a submit button. This UI is simple enough and is something just about every frontend engineer ends up building at some point.

---

## Creating the Structure of the Login Form

Below is a short screen recording of me writing the JSX for the login form:

![Screen recording of writing the JSX for the login form](https://dev-to-uploads.s3.amazonaws.com/uploads/articles/wpoiayyst1bp2q7zdg6p.gif)
<figcaption>Screen recording of writing the JSX for the login form</figcaption>

You’ll notice that some of the suggestions were helpful, and some were not. The import statement worked flawlessly, and the suggestion for the component name `LoginForm` to match the filename was helpful.

When setting up the boilerplate code for the function component and the return statement, I didn’t receive very many helpful suggestions.

I then created the two input elements, and that’s where the magic started to happen. After typing `<label`, the rest of that line was suggested for me. Knowing that I had a label element for the username and that I was then creating an input element, the AI assistant gave me most of the code for the text input. Afterward, I added the `id` attribute myself.

The same thing happened for the password input, which makes sense because a username input is commonly followed by a password input. But this time for the input, the AI assistant included an `id` attribute for me. It’s learning!

Finally, I added the `htmlFor` attribute to link the label to the input for each set of elements. As usual, the autocomplete struggled for me on the first usage for the username, but then when I added it for the password, the suggestion was right there. Magic!

---

## Making the Form Interactive

Now that I had the JSX in place, I needed to write the rest of the JavaScript to add the event handlers for the form’s submit event and the two inputs’ change events. Let’s take a look at how that went:

![Screen recording of making the login form interactive](https://dev-to-uploads.s3.amazonaws.com/uploads/articles/g5m7w0ao13xelhsw53x8.gif)
<figcaption>Screen recording of making the login form interactive</figcaption>

You’ll notice a similar pattern in this video. Initially, the suggestions weren’t very helpful. However, as I wrote more code, the suggestions improved as the model picked up on what I was doing.

When creating the `handleSubmit` function, I needed to supply the Event object `e` myself. But after I typed `e.pre`, the model knew I wanted to call `e.preventDefault();`. Then, when I needed to add this event handler to the form’s `onSubmit` method, the model knew exactly what I wanted to do.

When creating the two change handling functions, I had to create most of the setup for the username using the `useState` hook myself. But when I followed the same pattern again for the password, the AI assistant was right by my side and ready with the code I needed.

This trend seems to be consistent: Do something once on your own, and the model is silently observing and learning. Do that same thing a second time, and the model is ready to help!

---

## Conclusion

AI-assisted coding was an interesting experience. I’ve only spent a few hours playing around with Tabnine so far, and the value seems tangible. Once you get used to working with the auto-suggestions, I can imagine your productivity skyrocketing. The [experience of this engineering team at Cisco](https://blogs.cisco.com/developer/tabninepairprogramming01) can certainly attest to that.

The real power seems to lie in automating the tedium of writing the same patterns of code over and over. Why not let an AI assistant help with that?

AI-assisted coding solutions are becoming more mainstream, and I think that they’re here to stay. Privacy concerns are real, so be wise in choosing a solution that has a level of telemetry you’re comfortable with. Regardless of your stance, I think any developer who is serious about their productivity should at least give AI-assisted coding solutions a try.
