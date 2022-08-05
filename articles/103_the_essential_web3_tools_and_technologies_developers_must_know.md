# The Essential Web3 Tools and Technologies Developers Must Know

![Decentralized app architecture](https://cdn-images-1.medium.com/max/2000/0*PtfMDdUCTp1rENzK)*Decentralized app architecture*

Web3 is the next iteration of the web. As opposed to Web1, which consisted of static web pages, and Web2, which brought us web apps and the web as a platform, Web3 consists of decentralized networks built on blockchains.

There is a high demand for Web3 developers, as relatively few current developers are experts in this field. So if you’re an established Web2 engineer wanting to break into Web3, where would you start? What are the fundamental concepts to know, and what tools and technologies would you need to learn?

In this article, we’ll explore Web3, why it matters, and how it differs from Web2. We’ll then look at the tech stack that aspiring Web3 developers should become familiar with to get started.

---

## Web3 101

Before we dig into the tech used in Web3 apps, let’s first begin by understanding what Web3 is and why it’s important.

Five key features of Web3 are decentralization, blockchains, security, scalability, and privacy. In the decentralized world of Web3, blockchain technology and other protocols fundamentally change how data is stored, distributed, and accessed while providing a native transaction layer. Popular use cases for Web3 are decentralized finance (DeFi and cryptocurrencies), voting through decentralized governance known as DAOs, and non-fungible tokens (NFTs) as proof of ownership.

It may sound cynical, but a lot of the motivation behind Web3 is based on the erosion of trust between users, companies, and governments.

In decentralized finance, users store their funds in their private wallets and make transactions without ever interacting with a centralized institution or relying on a state’s fiat currency.

A voting app built on a blockchain would make all voting data transparent and easily verifiable by anyone, so you no longer have to trust those in power that an election is being handled fairly. This is what makes Web3 “trustless”: the technology is transparent and secured by cryptography, so there is no need for blind trust in institutions.

NFTs can be used to provide proof of ownership of any given digital asset like music or art and can allow you to support creators more directly.

All of these examples involve core activities that no longer have to rely on central authorities or intermediaries.

It’s important to note that Web3 is not meant to replace Web2, just like Web2 hasn’t replaced Web1. There is still an important place on the web for static websites like those that came out of Web1. Even as Web3 grows in popularity and usage, there will still be a place for Web2 apps as well.

---

## Decentralized Apps (Dapps)

We now have a general idea of what Web3 is and why the concept of decentralization is important, but what does a Web3 app actually look like?

Well, they look a lot like Web2 apps! Decentralized apps, also called “dapps” (or “dApps”), consist of a frontend UI that interacts with a “smart contract” (a small program of code) that is deployed on the blockchain. The frontend can also interact with a user’s wallet when making transactions or writing data to the blockchain. The key difference from a Web2 app is that the smart contract and blockchain replace a typical server and database owned and maintained by a single person or company.

![Decentralized app architecture](https://cdn-images-1.medium.com/max/2000/0*PtfMDdUCTp1rENzK)*Decentralized app architecture*

---

## Technologies Defining the Web3 Tech Stack

So, how do you actually build a decentralized app (dapp)? The good news is that you can start with the programming skills and experience you already have! We already know that a dapp has a frontend, so that means you’ll need to know HTML, CSS, and JavaScript. Unless you like building your apps in vanilla JavaScript, you’ll probably also want to use a framework or library like Angular, React, or Vue. This is great news for front-end developers who are already well-versed in these technologies.

Now let’s look at some of the languages, tools, and frameworks you’ll need to learn specifically for Web3:

[**Solidity**](https://soliditylang.org/) is a programming language used to write smart contracts that run on the Ethereum blockchain. It looks like a mix of C++, Python, and JavaScript. If you’ve learned a few programming languages by now, then you know that picking up a new language gets easier every time. Since most smart contracts involve some sort of monetary exchange, following [proper standards](https://docs.openzeppelin.com/) and [security best practices](https://consensys.net/blog/developers/solidity-best-practices-for-smart-contract-security/) is essential.

[**Truffle**](https://trufflesuite.com/docs/truffle/) is a framework that helps you write, test, and deploy smart contracts. The Truffle website describes it as a “development environment, testing framework and asset pipeline for blockchains using the Ethereum Virtual Machine (EVM).” In the same way that React helps you build JavaScript apps, Truffle helps you with your smart contracts. It’s not strictly necessary to use Truffle, but this framework will help you immensely as it abstracts away some of the development complexity. For VS Code users, [the Truffle for VS Code extension](https://trufflesuite.com/blog/build-on-web3-with-truffle-vs-code-extension/) makes the development lifecycle even easier.

[**Ganache**](https://trufflesuite.com/docs/ganache/) is a personal blockchain for local development and testing of smart contracts. It enables developers to create a local instance of the Ethereum blockchain in a few simple commands. Just like you develop a Web2 app on your localhost or a test environment rather than in production, Ganache lets you do your Web3 development locally as well.

[**Web3.js**](https://web3js.readthedocs.io/) is a JavaScript library used to interact with Ethereum. You would use web3.js in your frontend app to do things like connect to a user’s wallet, grant access to a smart contract, and call functions on the smart contract. Smart contracts can be accessed through the CLI or through a UI, so web3.js is what helps you work with smart contracts from the UI.

[**MetaMask**](https://metamask.io/) is a Web3 wallet that you can use with its browser extension or mobile app. We’ve alluded to wallets before but haven’t really described what they are yet. A wallet provides an interface to your digital assets. You protect the contents with a private key that only you know. MetaMask provides a secure way for users to connect to blockchain-based apps and interact with them with their wallets. For developers, wallets are required to deploy and interact with smart contracts. Normally you would have to do this by placing your private keys in your code, but the [Truffle Dashboard](https://trufflesuite.com/blog/introducing-truffle-dashboard/) enables you to connect your MetaMask wallet to your project without ever exposing your keys.

[**Infura**](https://infura.io/) is an infrastructure provider for connecting to Ethereum and other blockchains as well as decentralized storage networks such as IPFS. Without going into too much detail, any interactions with the blockchain require access to a node via JSON-RPC or websockets. Infura provides the infrastructure so you don’t have to spin up your own node on your machine. Infura can also serve as a fallback in case you do want to run your own node. Infura also provides a development suite and toolkit including monitoring, metrics, logging, transaction management, and other features for building dapps. This is a further abstraction on top of some of the other technologies we’ve already discussed to make Web3 development even easier.

Excited to dig into all of these yet? With a brief introduction to each of these technologies, you’re ready to start building your first dapp! There are [many great tutorials](https://www.useweb3.xyz/tutorials) out there, and each of them will likely use most (or all) of these technologies. To help you get started, the ConsenSys team has dozens of “[Truffle Boxes](https://trufflesuite.com/boxes/)” available, which are boilerplate templates you can use to bootstrap your first project. If you are looking for a [structured way of ramping up for Web3, check out ConsenSys Academy](https://consensys.net/academy/).

---

## Conclusion

Web3 is the next evolution of the internet that supports the next generation of software. Blockchains are more transparent technologies that are already going mainstream through not just consumer adoption but also [major institutional adoption](https://consensys.net/reports/web3-report-q3-2021/).

Understanding what the Web3 technology is and how to build it will give you an advantage to break into the market.

For those aspiring Web3 developers that already have a strong Web2 foundation, I hope by now you feel confident that you’re off to a good start with what you already know! Take some time to learn the technologies above, and you’ll be ready sooner than you may think.
