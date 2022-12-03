# Create Dynamic Code Review Checklists with CodeSee Code Automation

I’ve been a longtime advocate of [pull request templates](https://levelup.gitconnected.com/managing-complexity-through-merge-request-templates-9a00cc9a5fb1). These templates allow you to provide instructions to developers creating pull requests in your repos so that they will be reminded to include all the relevant information you need to properly review their code. Pull request templates are a great place to include [checklists](https://betterprogramming.pub/a-checklist-manifesto-3f686ed135f8) as additional reminders. These might include to-dos like remembering to write unit tests, self-reviewing your code, or doing a security audit.

Sometimes though, these checklists can get long. You may have so many important items that developers begin to ignore them. Or perhaps some of the checklist items are only relevant in specific situations or when certain files are changed. While each item is important, they may not all always be applicable.

**Wouldn’t it be nice if your pull request checklists could be dynamic and only show relevant reminders?**

CodeSee Code Automation can help you do just that! With Code Automation, you can configure rule-based triggers to automatically add checklist comments to the pull request when certain conditions are met.

In this article, we’ll look at some of CodeSee’s Code Automation templates and show how we can incorporate one of them into a codebase of our own. Ready to get started?

---

## Automation Templates

There are several different [automation templates](https://www.codesee.io/code-automation) on the CodeSee site that we can use as our inspiration. For example, there are checklists for [adding icons](https://www.codesee.io/automation-templates/icon-checklist), [creating a migration](https://www.codesee.io/automation-templates/create-a-migration), [adding a new npm package](https://www.codesee.io/automation-templates/adding-a-new-package), or [creating a new API endpoint](https://www.codesee.io/automation-templates/endpoint-checklist).

For our demo, we’ll explore the icon checklist and modify it to meet our needs.

---

## Our Fictional Icon Repo

![Screenshot of our demo icon library](https://cdn-images-1.medium.com/max/3200/0*0qVvtzKwvCdwkyNi)*Screenshot of our demo icon library*

[Let’s imagine that we have a repo](https://github.com/thawkin3/codesee-automation-demo) containing hundreds of icons for developers at our company. When a developer needs a new icon not found in our icon library, they talk to a designer, and the designer creates an SVG file for the developer. The developer then takes the SVG file and adds it to the code repo.

Simple enough, right? The problem is that the SVG file from the designer is often messy. Usually these files will have random ID attributes included. There might be extra comments or unnecessary code in the file. The file name will probably not follow the same pattern as the other icon files. And, there might be additional code setup needed besides simply committing the new SVG file. The developer may need to update a master list where the icons are exported or update some documentation or Storybook examples.

Some developers might just add the SVG file without giving any of these additional steps any thought.

The solution, then, is to have a checklist that reminds developers what they need to do when adding a new icon. This checklist could exist in a README or in a pull request template, and I’d highly recommend you do both of these things.

But even then, developers sometimes just don’t read the docs. They ignore the instructions and blindly press forward.

To solve this, let’s add a Code Automation Trigger so that the developer is freshly reminded with a checklist comment when they create their pull request.

---

## Adding the Icon Checklist Automation Template

To begin, we’ll first need to give CodeSee access to our GitHub repo from the Settings page.

![Connect CodeSee to your GitHub repo](https://cdn-images-1.medium.com/max/3200/0*XNaktMim4tEwaGiI)*Connect CodeSee to your GitHub repo*

Once our repo is added, we can navigate to the Automate page to create our first Trigger.

![Add your first Trigger](https://cdn-images-1.medium.com/max/3200/0*pkMI0YbfkUs5eeBv)*Add your first Trigger*

We’ll click the **Add Trigger** button to access the New Trigger page.

![New Trigger page](https://cdn-images-1.medium.com/max/2148/0*l1TJMBAeHIMOwxT6)*New Trigger page*

On this page, we can add a name and repo to our Trigger and then specify the conditions and the actions. In our case, we want the condition to be whenever a new file is added to the `src/icons` directory.

![Add Conditions for your Trigger](https://cdn-images-1.medium.com/max/2712/0*6ggqO_7OAp150F5k)*Add Conditions for your Trigger*

When that condition is met, we want CodeSee to automatically comment on the pull request with a few checklist items. We’ve added three items here, but you can add as many items as you’d like.

![Add Actions for your Trigger](https://cdn-images-1.medium.com/max/2700/0*61COdsw7-OGaCZkr)*Add Actions for your Trigger*

After we’re done editing, we can click the Save button to save our changes. With that, we’ve just created our first dynamic checklist! Let’s see this automation in action.

In our code repo, let’s add a new SVG file to the `src/icons` directory. Then let’s create a new branch and submit a pull request to merge our branch into the master branch.

Once our pull request is created, you’ll notice that the CodeSee bot automatically comments on our pull request with a checklist of reminders. Good thing too, because we forgot to do these things!

![The CodeSee bot has added a comment on the pull request](https://cdn-images-1.medium.com/max/3200/0*N2-TX160ZljGICFu)*The CodeSee bot has added a comment on the pull request*

---

## Conclusion

That’s it! In just a few short minutes, we’ve added a checklist to help our developers have a better experience when adding icons. We’ve made code expectations clear and highlighted possible items that the developer may have missed.

And, best of all, we’ve added these comments dynamically, only showing them when actually needed. If a developer were to make a different contribution to this repo that didn’t involve adding a new icon, they wouldn’t see these comments at all.

The cool thing is that this is just the start. We can add all sorts of other automation templates to our repo. And, these Triggers can do more than just add checklist comments. They also allow you to automatically assign reviewers to the pull request in the event that the expertise of certain developers or teams is needed.

With CodeSee Code Automation, we can enforce best practices and share knowledge effortlessly.
