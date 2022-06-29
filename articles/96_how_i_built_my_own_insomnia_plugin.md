# How I Built My Own Insomnia Plugin

[Insomnia](https://insomnia.rest/) is an open source API client that helps you test and debug GraphQL, gRPC, and REST API endpoints. As powerful as Insomnia is, sometimes you want to extend its functionality even further, which you can do with [plugins](https://docs.insomnia.rest/insomnia/introduction-to-plugins).

There are many great plugins available on the [Insomnia Plugin Hub](https://insomnia.rest/plugins), all contributed by the open source community. Even more exciting, if you’d like to contribute, you can build your own plugin!

In this article, we’ll explore a plugin I built and then show you how you can build an Insomnia plugin of your own.

---

## Plugin Demo

My plugin is a simple addition that allows you to initiate all the requests in a folder and then displays a desktop notification once all the requests have been completed.

![Context menu item allows you to “Send All Requests” inside the Insomnia app](https://cdn-images-1.medium.com/max/2000/0*JorzLprjgQcnPryz)
<figcaption>Context menu item allows you to “Send All Requests” inside the Insomnia app</figcaption>

If you’re a habitual multitasker like myself, you probably rapidly cycle through several apps at a time while you’re working. Your IDE, your terminal, your browser, and Insomnia may be a few. In the event that you have a large number of requests in your folder and don’t want to wait for them to all finish, you can move on to something else while you wait, and then be notified once the job is complete.

![Desktop notification appears once all requests have been sent](https://cdn-images-1.medium.com/max/2000/0*-QUTNGN6QkWddSVz)
<figcaption>Desktop notification appears once all requests have been sent</figcaption>

You can [find my plugin’s package on npm](https://www.npmjs.com/package/insomnia-plugin-requests-desktop-notification) or [view the GitHub repo here](https://github.com/thawkin3/insomnia-plugin-requests-desktop-notification).

---

## Plugin Installation

You can install Insomnia plugins through the settings page in your Insomnia desktop app. Simply click the gear icon in the top-right corner of the app, then choose the “Plugins” tab. Search for any plugin name in the text input, and then click the “Install Plugin” button to add the plugin to your app.

In the case of my plugin, the name is `insomnia-plugin-requests-desktop-notification`.

![Installing the requests-desktop-notification plugin](https://cdn-images-1.medium.com/max/3200/0*wzP8X2M2AgXzAuSs)
<figcaption>Installing the requests-desktop-notification plugin</figcaption>

---

## Plugin Usage

Once you have the plugin installed, you’re ready to start using it — no need to restart your Insomnia app. There are several variations of plugins. Some add behavior to various hooks, like [request hooks or response hooks](https://docs.insomnia.rest/insomnia/hooks-and-actions). Others [add new items to dropdown menus](https://docs.insomnia.rest/insomnia/hooks-and-actions#folder-actions) in the app. You can even install a [custom theme](https://docs.insomnia.rest/insomnia/custom-themes) via a plugin.

My plugin adds a new item to the dropdown menu for a request folder. To see it in action, you can navigate to the “Debug” area of your Insomnia app. Then, create a new folder and add some requests to it (or use an existing folder if you already have one).

Next, click the dropdown trigger button to the right of the folder name to open the dropdown menu. Among the default menu items, you’ll also now see a custom menu item that my plugin adds: “Send All Requests.”

![Context menu item allows you to “Send All Requests” inside the Insomnia app](https://cdn-images-1.medium.com/max/2000/0*2picN2e-5Z9KuGIi)
<figcaption>Context menu item allows you to “Send All Requests” inside the Insomnia app</figcaption>

Select that menu item to send all the requests in your folder. Once all the requests have been completed, a desktop notification will appear on your machine. Ta-da!

![Desktop notification appears once all requests have been sent](https://cdn-images-1.medium.com/max/2000/0*XxWXtc5hsd87Gr1a)
<figcaption>Desktop notification appears once all requests have been sent</figcaption>

---

## How to Build an Insomnia Plugin

So, how did I build this? Well, it’s actually quite simple! The plugin is a small JavaScript app that consists of a `main.js` file and a `package.json` file. For the desktop notification functionality, I used the [node-notifier](https://www.npmjs.com/package/node-notifier) npm package.

When [creating a new Insomnia plugin](https://docs.insomnia.rest/insomnia/introduction-to-plugins#create-a-plugin), you can bootstrap your plugin by navigating to the settings screen in your Insomnia app, clicking the “Plugins” tab, and then clicking the “Generate New Plugin” button.

![Use the “Generate New Plugin” button to bootstrap your plugin](https://cdn-images-1.medium.com/max/3200/0*5P9Q3BzJZ_iVrzEm)
<figcaption>Use the “Generate New Plugin” button to bootstrap your plugin</figcaption>

This will create a new directory for your plugin and place it in a folder in which Insomnia keeps all of its plugins locally. It also creates the `main.js` and `package.json` files for you automatically.

From there, I wrote the following code in my `main.js` file. Look how short it is!

{% gist https://gist.github.com/thawkin3/c6419cc972b8ae9060cece4b64b73e55 %}

Let’s walk through this code together. First, it requires the two dependencies that we rely on: `path` and `node-notifier`. `path` is a built-in module, but I needed to install `node-notifier` by running `yarn add node-notifier` in my terminal.

The main part of the code adds a new entry to the `requestGroupActions` array. This is what creates the new menu item in the dropdown menu for our request folder. The `label` is the menu item’s text, and the `action` is what code we want to run when someone selects that menu item.

In the action function, we do the following:

1. Gather all the requests in the folder.
2. Send them.
3. Call `notifier.notify` once all the requests have been completed. The `title`, `message`, `icon`, and `sound` properties all configure our desktop notification.

That’s it! The plugin really is that simple.

Once I wrote this code, I was able to navigate to the settings page of my Insomnia app, then the “Plugins” tab, then click the “Reload Plugins” button to reload the latest changes. After that, the menu item appeared in my request folder’s dropdown menu.

---

## Publishing to npm

After writing my plugin, I had everything working nicely locally. But I also wanted to share this plugin with the broader community. In order to do that, I needed to publish it to npm. To do so, I pushed the latest code to my GitHub repo, logged into my npm account with `npm login`, and then published my package using `npm publish`.

After publishing the initial version of the package, I was able to make a few updates to the code, generate a new package version with `npm version <major|minor|patch>`, push the tags to GitHub with `git push --tags`, push the code to GitHub with `git push`, and then publish the new package version to npm with `npm publish`.

Once my package was published in the npm registry, it was automatically added to the Insomnia Plugin Hub within the next 24 hours.

---

## Conclusion

There you have it — how I built my very own Insomnia plugin and how you can too. To recap, Insomnia is great for building, testing, and debugging APIs. Plugins let you extend Insomnia’s functionality. Building your own custom plugin is easy — you can add new functionality with just a few lines of code!
