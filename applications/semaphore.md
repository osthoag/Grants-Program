# Semaphore

- **Team Name:** Semaphore
- **Payment Address:** BTC, Ethereum (USDT/USDC/DAI) or Polkadot/Kusama (aUSD) payment address. Please also specify the currency. (e.g. 0x8920... (DAI))
- **[Level](https://github.com/w3f/Grants-Program/tree/master#level_slider-levels):** 2

## Project Overview :page_facing_up:

### Overview

Semaphore: a decentralized social layer for the internet

We are building a new type of L2 blockchain network to be a neutral communication layer for composable social apps. Our blockchain design allocates blockspace without the use of fees by utilizing an NFT-style smart contract on the Polkadot blockchain (or an L2) to manage membership in the network. This enables on-chain social media, so interoperable apps can share the network effects of a unified pool of users and create a decentralized online social ecosystem. 

The primary interaction that the Semaphore chain will have with Polkadot is that of checkpointing. The smart contract that manages aliases on Semaphore must be synchronized (checkpointed) into the Semaphore chain. Additionally, this method of representing valid Semaphore identities will create a new type of nonfungible asset with more interesting properties and greater utility than those of the typical NFT. We think that creating a finite (yet expanding) set of aliases used for social interaction will have use cases beyond Semaphore, hopefully facilitating the development of other type 3 networks within the Web3 ecosystem that use the same set of identities in order to share network effects.

Semaphore will host an ecosystem of social apps that will solve the myriad of problems caused by corporate control of the modern public square. Ten years from now, people will look back on the current era of fragmented online spaces, monolithic moderation, and adversarial algorithms as bizarre and confusing. Semaphore will achieve this by being a reliable, decentralized protocol that enables developers to easily build composable social apps.

### Project Details

Semaphore is a blockchain without a cryptocurrency. Instead, Semaphore uses “crypto-identities” or “aliases.” Each alias has a unique integer, a pubkey, and a human readable “nym.” The set of aliases is maintained on a separate type 2 network, such as Polkadot. However, aliases are much more than simple NFT’s (non-functional tokens), nodes that run the semaphore network sync the state of the alias smart contract and only accept broadcasts/connections signed by pubkeys belonging to valid aliases. This means that the state of a smart contract on Polkadot can control the set of valid users on the Semaphore network. The rules for the total number of aliases able to be minted can follow a predetermined function, be controlled by an alias vote, etc.

**Protocol**
The lifecycle of a broadcast starts when it is signed by a user. The message includes the user’s alias, signature, the message itself, and a timestamp. Time on Semaphore exists as discrete epochs of 6 seconds, so the timestamp says which epoch the broadcast occurs in. Next, the broadcast is relayed to Semaphore nodes, who will relay the message to their peers. This process is called the “initial relay.” At the end of the initial relay, it is possible that nodes will have seen different sets of broadcasts. The epoch vote is a BFT process that will allow nodes to reach consensus on which broadcasts were seen by a majority of nodes during the initial relay. Once the epoch vote has terminated, a block containing each accepted broadcast is constructed by each node. To reach consensus, nodes will iteratively sample each other in a process similar to the Avalanche consensus mechanism. At the end of the epoch vote process, with very high probability all broadcasts with a correct timestamp will be included in the block and all nodes will have produced identical blocks. 

The epoch vote on its own is insufficient, however, because it is a subjective process: offline nodes cannot verify the output like they can with PoW or PoS. The missing piece is “proof of engagement.” Each block has a “chain commitment,” which is the hash of previous blocks. The chain commitment is analogous to the previous block hash in a typical blockchain. The chain commitment is also what broadcasts use as a timestamp. Chain commitments mean that if there is ever a blockchain fork, broadcasts are only valid on a single fork. When an alias makes a broadcast with a particular chain commitment, it is said to have committed to the fork that the chain commitment corresponds to. If there is a blockchain fork that has reached a sufficient depth, nodes will consider the chain that has more commitments after the fork to be valid, meaning it has the higher proof of engagement.

One of the purposes of Semaphore’s blockchain architecture is to enable spam prevention, which is achieved through Semaphore’s karma system. The karma system is a dynamic rate limit that also enables popular aliases to have greater access during times of congestion. The goal of  the karma system is to fairly allocate blockspace without the use of fees. A good design will ensure that the total throughput never exceeds the capacity of the network but never throttles users so much that blocks are not full even though users want to post.

Another purpose of Semaphore’s blockchain architecture is to enable moderation. Moderation is important on social media, especially when users are able to post to a decentralized, censorship resistant system. The Semaphore philosophy is that users should have maximum control over the interactions with their own content and that different users should have the freedom to implement different rules according to their own values. Each alias is able to set a blocklist.  Additionally, a broadcast that is not a reply to another broadcast, can define a “paradigm,” or set of rules that apply to it and all descendent broadcasts (replies, replies to replies, etc). This means that users can moderate the discussions that take place under their broadcasts. Paradigms enable precise control over how interactions can occur. We think it is likely that different organizations would maintain sets of aliases that they consider to be spam, misinformation, hateful, etc.

The full documentation on the Semaphore protocol and its security can be found in the [Semaphore White Paper](https://github.com/SirLemmings/Semaphore-Demo/blob/main/Semaphore%20White%20Paper.pdf).

**Technology Stack**
- ink! or Solidity
- Python
- AWS
- Rust

### Ecosystem Fit

Help us locate your project in the Polkadot/Substrate/Kusama landscape and what problems it tries to solve by answering each of these questions:

- Where and how does your project fit into the ecosystem?
- Who is your target audience (parachain/dapp/wallet/UI developers, designers, your own user base, some dapp's userbase, yourself)?
- What need(s) does your project meet?
- Are there any other projects similar to yours in the Substrate / Polkadot / Kusama ecosystem?
  - If so, how is your project different?
  - If not, are there similar projects in related ecosystems?

We are building a new type of L2 blockchain network to be a neutral communication layer for composable social apps. Our blockchain design allocates blockspace without the use of fees by utilizing an NFT-style smart contract on the Polkadot blockchain (more specifially one of its parachains) to manage membership in the network. Additionally, this method of representing valid Semaphore identities will create a new type of nonfungible asset with more interesting properties and greater utility than those of the typical NFT.  Our protocol targets for membership the growing user base for decentralized social media by enabling on-chain social media. This also allows for interoperable apps to share the network effects from this unified pool of users. Thus, we aim to incentivize third party app developers to build on the protocol, resulting in a decentralized online social ecosystem.

The related projects in other ecosystems take two approaches. Decentralized/ ”decentralized” competitors use network designs that are ill suited to social apps. Lens protocol, for instance, is a prominent project that stores social interactions on Polygon using NFTs. Designs like Lens subject users to competition for blockspace with financial transactions, resulting in unpredictable fees. Other competing protocols simply use a P2P network with no consensus, which means broadcasts cannot be time stamped, that different apps' states can disagree about which broadcasts exist, and that each app must implement its own spam prevention techniques. This is the case with federated networks like Farcaster. Although Farcaster makes the same design choice to put identities on-chain, its lack of shared state cannot deliver the experience of Twitter in a truly decentralized manner. Our protocol differentiates itself from the alternatives by being a lightweight blockchain that allocates its blockspace without the use of fees. As a result the chain is accessible to users and running a service is easier because there is a guarantee of a shared state and no spam.

## Team :busts_in_silhouette:

### Team members

- Alex Dulisse
- Oscar Aguilar

### Contact

- **Contact Name:** Alex Dulisse
- **Contact Email:** 37alexd@gmail.com
- **Website:** [Semaphore Demo Page](https://sirlemmings.github.io/Semaphore-Demo/)

### Legal Structure

- **Registered Address:**
- **Registered Legal Entity:**

### Team's experience

Please describe the team's relevant experience. If your project involves development work, we would appreciate it if you singled out a few interesting projects or contributions made by team members in the past. 

If anyone on your team has applied for a grant at the Web3 Foundation previously, please list the name of the project and legal entity here.

### Team Code Repos

- [Proof-of-Concept Implementation](https://github.com/SirLemmings/Semaphore)
- [Sequencer Version](https://github.com/SirLemmings/semaphore_sequencer)
- [Demo Repo](https://github.com/SirLemmings/Semaphore-Demo)

Team member's repos

- https://github.com/SirLemmings
- https://github.com/osthoag

### Team LinkedIn Profiles (if available)

- https://www.linkedin.com/<person_1>
- https://www.linkedin.com/oscar-aguilar-76bb15170/

## Development Status :open_book:

Thus far, the project has produced two MVPs, with largely complementary functionality. The first is a proof-of-concept for the consensus mechanisms outlined in our white paper. This node software is implemented in Python. It performs the basic functions of P2P connectivity, public key/ private key encryption, memory commitments, and gossiping. In addition, it maintains consensus over a synchronized network clock, broadcast relays, chain commitments, epoch voting, simple proof of engagement, reorgs, and fork requests. It builds blocks in parallel epoch processes and is robust to 33% attacks on safety and liveness. The current version can be found here: https://github.com/SirLemmings/Semaphore-Demo/, and a demo of the older version can be seen here: https://github.com/SirLemmings/Semaphore-Demo/.
	The second MVP is a sequencer version of the protocol which implements the more advanced parts of the chain infrastructure, but in a centralized way. It tracks state transitions, mints aliases, and produces blocks. The client software is able to submit broadcasts and request the state of the chain, as a basic backend for apps building on top of the protocol. Both are running on AWS. The current version can be found here: https://github.com/SirLemmings/Semaphore-Demo/.
	Most parts of the core Semaphore node functions are implemented in the proof-of-concept and/or sequence version. There are a few missing elements, such as peer bootstrapping and discovery, bandwidth optimization, extra primitives, blockspace allocation, and alias tokenization/checkpointing via Ethereum. A full initial testnet would combine the functionality from the two MVPs along with these elements. A mature version of the Semaphore network would require a full rewrite in Rust along with several planned optimizations.


## Development Roadmap :nut_and_bolt:

This section should break the development roadmap down into milestones and deliverables. To assist you in defining it, we have created a document with examples for some grant categories [here](../docs/Support%20Docs/grant_guidelines_per_category.md). Since these will be part of the agreement, it helps to describe _the functionality we should expect in as much detail as possible_, plus how we can verify and test that functionality. Whenever milestones are delivered, we refer to this document to ensure that everything has been delivered as expected.

Below we provide an **example roadmap**. In the descriptions, it should be clear how your project is related to Substrate, Kusama or Polkadot. We _recommend_ that teams structure their roadmap as 1 milestone ≈ 1 month.

> :exclamation: If any of your deliverables is based on somebody else's work, make sure you work and publish _under the terms of the license_ of the respective project and that you **highlight this fact in your milestone documentation** and in the source code if applicable! **Teams that submit others' work without attributing it will be immediately terminated.**

### Overview

- **Total Estimated Duration:** Duration of the whole project (e.g. 2 months)
- **Full-Time Equivalent (FTE):**  Average number of full-time employees working on the project throughout its duration (see [Wikipedia](https://en.wikipedia.org/wiki/Full-time_equivalent), e.g. 2 FTE)
- **Total Costs:** Requested amount in USD for the whole project (e.g. 12,000 USD). Note that the acceptance criteria and additional benefits vary depending on the [level](../README.md#level_slider-levels) of funding requested. This and the costs for each milestone need to be provided in USD; if the grant is paid out in Bitcoin, the amount will be calculated according to the exchange rate at the time of payment.

### Milestone 1 Example — Basic functionality

- **Estimated duration:** 1 month
- **FTE:**  1,5
- **Costs:** 8,000 USD

> :exclamation: **The default deliverables 0a-0d below are mandatory for all milestones**, and deliverable 0e at least for the last one. 

| Number | Deliverable | Specification |
| -----: | ----------- | ------------- |
| **0a.** | License | Apache 2.0 / GPLv3 / MIT / Unlicense |
| **0b.** | Documentation | We will provide both **inline documentation** of the code and a basic **tutorial** that explains how a user can (for example) spin up one of our Substrate nodes and send test transactions, which will show how the new functionality works. |
| **0c.** | Testing and Testing Guide | Core functions will be fully covered by comprehensive unit tests to ensure functionality and robustness. In the guide, we will describe how to run these tests. |
| **0d.** | Docker | We will provide a Dockerfile(s) that can be used to test all the functionality delivered with this milestone. |
| 0e. | Article | We will publish an **article**/workshop that explains [...] (what was done/achieved as part of the grant). (Content, language and medium should reflect your target audience described above.) |
| 1. | Substrate module: X | We will create a Substrate module that will... (Please list the functionality that will be implemented for the first milestone. You can refer to details provided in previous sections.) |
| 2. | Substrate module: Y | The Y Substrate module will... |
| 3. | Substrate module: Z | The Z Substrate module will... |
| 4. | Substrate chain | Modules X, Y & Z of our custom chain will interact in such a way... (Please describe the deliverable here as detailed as possible) |
| 5. | Library: ABC | We will deliver a JS library that will implement the functionality described under "ABC Library" |
| 6. | Smart contracts: ... | We will deliver a set of ink! smart contracts that will...


### Milestone 2 Example — Additional features

- **Estimated Duration:** 1 month
- **FTE:**  1,5
- **Costs:** 8,000 USD

...


## Future Plans

Please include here

- how you intend to use, enhance, promote and support your project in the short term, and
- the team's long-term plans and intentions in relation to it.

## Referral Program (optional) :moneybag: 

You can find more information about the program [here](../README.md#moneybag-referral-program).
- **Referrer:** Name of the Polkadot Ambassador or GitHub account of the Web3 Foundation grantee
- **Payment Address:** BTC, Ethereum (USDT/USDC/DAI) or Polkadot/Kusama (aUSD) payment address. Please also specify the currency. (e.g. 0x8920... (DAI))

## Additional Information :heavy_plus_sign:

**How did you hear about the Grants Program?** Web3 Foundation Website / Medium / Twitter / Element / Announcement by another team / personal recommendation / etc.

Here you can also add any additional information that you think is relevant to this application but isn't part of it already, such as:

- Work you have already done.
- If there are any other teams who have already contributed (financially) to the project.
- Previous grants you may have applied for.
