
# How to Build a Crowdfunding Web3 Dapp

Let’s buy Twitter!

![Crowdfunding web3 dapp — Let’s buy Twitter!](https://cdn-images-1.medium.com/max/3200/0*DM0wW3A-fR_F2A9H)
<figcaption>Crowdfunding web3 dapp — Let’s buy Twitter!</figcaption>

For the last several months, the tech world has been abuzz with the news that Elon Musk is buying Twitter. Whether or not the acquisition will actually happen still remains to be seen, but many Twitter employees and Twitter users are concerned about what this may mean for the company culture and for the app itself.

Jokingly I thought to myself, “What if we rallied together and bought Twitter instead?” I don’t have $44 billion, but maybe we could crowdfund it? Surely I could create a GoFundMe or Kickstarter project.

I’ve also recently been delving into the world of Web3, which is all about decentralization. So my next train of thought became, “What would it take to build a crowdfunding app using Web3 technology?”

This article will explore exactly that. We’ll consider how crowdfunding apps normally work, how they would work in the Web3 world, and how we could build our own crowdfunding Web3 decentralized app (“dapp”). We’ll even include some code samples to help you build your own decentralized crowdfunding platform.

Ready to take on Elon?

---

## How Crowdfunding Apps Work

Crowdfunding apps like [GoFundMe](https://www.gofundme.com/) or [Kickstarter](https://www.kickstarter.com/) allow users to create new fundraisers that anyone can contribute to. The fundraiser creator accepts the donations, usually under certain conditions, and then the crowdfunding platform takes a small percentage of the money as their share. Everybody wins.

For a platform like Kickstarter, the fundraising goal must be met by a deadline to release funds. If the goal is met in time, then the fundraiser creator receives the funds for their project, and all the contributors’ credit cards are charged for the amount they donated. If the deadline passes and the goal is not met, then everyone who contributed gets their money back (or rather, their credit cards are not charged).

This model works pretty well, and plenty of successful projects have been funded by platforms like Kickstarter. But what if we could cut out the middleman?

---

## How a Web3 Crowdfunding Dapp Could Work

Web3 comes with its own transaction layer that allows users to transfer funds held in their crypto wallets. Popular wallets include [Coinbase Wallet](https://www.coinbase.com/wallet) or [MetaMask](https://metamask.io/).

Web3 apps are commonly called “dapps,” due to the decentralized nature of the blockchain. Dapps are built with a frontend UI that interacts with a smart contract deployed to the blockchain, and this smart contract serves as the backend code and database that you’d see in a typical Web2 app.

For a web3 crowdfunding dapp, we could utilize a smart contract that allows people to pledge funds from their wallet toward a cause, just like a Kickstarter campaign. The smart contract could have logic built into it that only allows the crowdfunding project creator to withdraw the funds once the goal has been met. Until then, funds would be held in escrow on the smart contract. This means that donors would have the funds transferred from their wallets when they make their donations, but they could ask for a refund at any time as long as the goal has not yet been met.

Once the goal has been met and the funds have been withdrawn, the person who accepted the donations could do as they pleased with the money, so technically, they could take the money and run. If we wanted to take this idea one step further, we could explore [decentralized autonomous organizations (DAOs)](https://ethereum.org/en/dao/) and how they handle not just crowdfunding but collective ownership and collective decision making. For now, however, we’ll stick with a simple smart contract only.

So, with that high-level architecture in mind, let’s check out an actual Web3 crowdfunding dapp we built! You can find all of the [code for the demo app hosted on GitHub](https://github.com/thawkin3/coinbase-crowdfunding-app).

---

## Our Web3 Crowdfunding Dapp

![Crowdfunding web3 dapp — Let’s buy Twitter!](https://cdn-images-1.medium.com/max/3200/0*fO2AMOwJlxgwde_x)
<figcaption>Crowdfunding web3 dapp — Let’s buy Twitter!</figcaption>

Our dapp is fairly straightforward from a user standpoint. The user visits the page and clicks the button to connect their wallet. Again, this could be any crypto wallet the user chooses.

If a user does not have a crypto wallet browser extension, clicking the button will prompt Coinbase Wallet’s onboarding UI to pop up, enabling a new user to either connect an existing mobile wallet or create a new wallet in minutes.

![Coinbase Wallet’s onboarding UI](https://cdn-images-1.medium.com/max/2042/0*zyd6vxpCzeIXsxeF)
<figcaption>Coinbase Wallet’s onboarding UI</figcaption>

Once their wallet is connected, the user can submit a donation by modifying the value in the input field and then clicking the “Donate” button. (We’ve set a minimum donation amount of 0.01 ether and a fund goal of 10 ether in the smart contract, but those values are arbitrary.) They can also click two other buttons to see the total amount contributed toward the goal or to request a refund of the money they previously pledged. There is a button at the bottom of the UI to reset the wallet connection to start over, if needed.

![Crowdfunding web3 dapp — Make a donation](https://cdn-images-1.medium.com/max/3200/0*BQuAtWx8uF4ukLqD)
<figcaption>Crowdfunding web3 dapp — Make a donation</figcaption>

That’s really all there is to it, functionality-wise.

So, how did we build this? We used several technologies to create our dapp:

* [React](https://reactjs.org/) for the frontend UI
* [Solidity](https://soliditylang.org/) for the smart contract
* [Remix](https://remix-project.org/) for compiling and deploying the smart contract
* [Coinbase Wallet SDK](https://docs.cloud.coinbase.com/wallet-sdk/docs) for connecting to the user’s wallet
* [Coinbase Wallet](https://www.coinbase.com/wallet) and [MetaMask](https://metamask.io/) crypto wallets for sending and receiving funds
* [Infura](https://infura.io/) for a backup RPC endpoint

We’ve outlined all of the setup steps in the [README](https://github.com/thawkin3/coinbase-crowdfunding-app#readme), so we won’t go into step-by-step detail of how we built the app. If you’d like to follow along or build your own crowdfunding dapp, we’d highly recommend following the steps in the README file above!

Here we highlight two key files that supply the main functionality of the app: the `Crowdfunding.sol` file for the smart contract, and the `App.js` file for the React frontend UI.

The `Crowdfunding.sol` file is reproduced below in full:

{% gist https://gist.github.com/thawkin3/5d3c215b2ddb05d31062a92f5c094d8f %}

This file is what we compiled and deployed from within the Remix online IDE, so it’s not actually included in the project repo. Instead, we reference the address of where the contract was deployed and use the methods defined in the contract’s application binary interface (ABI).

Scanning through this file, you can see that we’ve defined methods for `donate`, `getBalance`, `withdraw`, and `returnFunds`. Each method does what its name implies.

* The `donate` method allows users to pledge donations.
* The `getBalance` method shows the current total amount of donations contributed.
* The `withdraw` method allows the funds to be withdrawn under the condition that the fundraiser goal has been met.
* The `returnFunds` method allows users to request a refund of their pledged amount if they change their minds after contributing.

Now let’s look at the frontend code with our `App.js` file, which is also reproduced in full below:

{% gist https://gist.github.com/thawkin3/69464b87f65b08f06d0afbbd5fefbbeb %}

There’s a lot of code in this file, but let’s discuss a few highlights. As you can see, we use the Coinbase Wallet SDK for connecting to the user’s wallet. We load our crowdfunding contract using the contract’s ABI and deployed address. We interact with the smart contract’s methods by using `.call()` and `.send()`, and we wire up click handlers to our buttons to make the app interactive.

At a high level, that is the magic behind how all this works. For more detailed setup instructions, we would again refer you to the step-by-step guide found in the [README on GitHub](https://github.com/thawkin3/coinbase-crowdfunding-app#readme).

---

## Conclusion

So, what have we learned today?

We’ve learned that Web3 technology allows for financial transactions without an intermediary institution. We’ve learned that besides transferring money from one individual to another, we can also use Web3 technology to support crowdfunding.

Finally, we’ve explored how a simple crowdfunding dapp might be built, the technologies behind it, and how using these technologies together can enable you to have an app up and running in a matter of hours.
