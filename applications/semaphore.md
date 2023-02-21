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

You can watch a [video explanation](https://youtu.be/tXhwmqeeuto) of this process. Additionally, the full documentation on the Semaphore protocol and its security can be found in the [Semaphore White Paper](https://github.com/SirLemmings/Semaphore-Demo/blob/main/Semaphore%20White%20Paper.pdf).

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

Building a network like Semaphore requires solving a lot of operations research style problems to allocate resources, validate network security, etc. We both have strong academic backgrounds in operations research and computer science. Alex has been programming for a very long time. In the past, he developed new software tools to solve Markov decision process problems (github link?), designed and built custom 3d printers, and designed a more comfortable and effective mask for a hackathon. Additionally, he has spent far too much time in undergrad learning the crypto stuff necessary to build a novel system like this. Oscar has similar programming experience, having worked with evolutionary algorithms, CNNs, and reinforcement learning agents in the context of academic, research, and industry environments. His journey into blockchains started with this project and continued through his graduate coursework at Stanford. Before working together on Semaphore, we collaborated on the simulation of honeybee and termite colonies for research in the application of information theory to eusocial insects, and managed an entrepreneurial capstone project. In summation, our team has the academic background required to design a novel blockchain, the practical experience to implement the design, the track record to manage the project from start to finish, and the industry/academic connections needed to refine and deploy the early stages of the protocol.

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

Thus far, the project has produced two MVPs, with largely complementary functionality. The first is a proof-of-concept for the consensus mechanisms outlined in our white paper. This node software is implemented in Python. It performs the basic functions of P2P connectivity, public key/ private key encryption, memory commitments, and gossiping. In addition, it maintains consensus over a synchronized network clock, broadcast relays, chain commitments, epoch voting, simple proof of engagement, reorgs, and fork requests. It builds blocks in parallel epoch processes and is robust to 33% attacks on safety and liveness. The current version can be found [here](https://github.com/SirLemmings/Semaphore), and a demo of the older version can be seen [here](https://youtu.be/gjo0V2Z3iJE).
	The second MVP is a sequencer version of the protocol which implements the more advanced parts of the chain infrastructure, but in a centralized way. It tracks state transitions, mints aliases, and produces blocks. The client software is able to submit broadcasts and request the state of the chain, as a basic backend for apps building on top of the protocol. Both are running on AWS. The current version can be found [here](https://github.com/SirLemmings/semaphore_sequencer).
	Most parts of the core Semaphore node functions are implemented in the proof-of-concept and/or sequence version. There are a few missing elements, such as peer bootstrapping and discovery, bandwidth optimization, extra primitives, blockspace allocation, and alias tokenization/checkpointing via Polkadot. A full initial testnet would combine the functionality from the two MVPs along with these elements. A mature version of the Semaphore network would require a full rewrite in Rust along with several planned optimizations.

## Development Roadmap :nut_and_bolt:

This scope of the grant will have an estimated timeline of four months. Combining, publishing, and launching a full Python node implementation and test net will take two months. Next, designing, writing, and deploying a Polkadot smart contract to manage aliases would take 2 months. Paying for 8 part-time man-months (FTE of 1 person) would amount to $26K plus an overhead of 15%. Factoring in miscellaneous costs would likely add another $1K. Given extra resources, we would also seek to expand our team from two to four people and publicize/market the project more aggressively. In total, the requested amount in USD for the project would be $30K.

### Overview

- **Total Estimated Duration:** 4 months
- **Full-Time Equivalent (FTE):**  1 FTE
- **Total Costs:** 30,000 USD.
### Milestone 1 — Python Testnet

- **Estimated duration:** 2 months
- **FTE:**  1
- **Costs:** 15,000 USD

| Number | Deliverable | Specification |
| -----: | ----------- | ------------- |
| **0a.** | License | MIT |
| **0b.** | Documentation | We will provide both **inline documentation** of the code and a basic **tutorial** that explains how a user can (for example), request an alias, spin up one of our Semaphore nodes, and send broadcasts, which will show how the new functionality works. |
| **0c.** | Testing and Testing Guide | Core functions will be fully covered by comprehensive unit tests to ensure functionality and robustness. In the guide, we will describe how to run these tests. |
| **0d.** | Docker | We will provide a Dockerfile that can be used to test all the functionality delivered with this milestone. |
| 0e. | Article | We will publish a series of **Medium posts** that explain what Semaphore is, what it solves, and how it can be used in a blog format targeting tech-focused users dissatisfied with the current social media landscape. |
| 1. | Semaphore module: Networking | We will implement a peer-to-peer networking protocol into our nodes to support bootstrapping and consensus for remote nodes. |
| 2. | Semaphore module: Consensus | We will complete the consensus mechanisms outlined in the Semaphore white paper and show guarantees of safety and liveness. |
| 3. | Semaphore module: State Transitions | We will integrate the state transition functionality from the sequencer into the Semaphore node software |
| 4. | Semaphore module: Alias Distribution | We will enable a central node with the ability to mint new aliases and bind them to pubkeys for external parties to request and use. |
| 5. | Semaphore chain | Modules 1,2, and 3 will combine to form a completed Python implementation of nodes in the Semaphore network, and will be used to launch a testnet for any party to join and participate in broadcasting and consensus. Module 4 will serve as a centralized placeholder for the second milestone. |

### Milestone 2 Example — Additional features

- **Estimated Duration:** 2 months
- **FTE:**  1
- **Costs:** 15,000 USD

| Number | Deliverable | Specification |
| -----: | ----------- | ------------- |
| **0a.** | License | MIT |
| **0b.** | Documentation | We will provide both **inline documentation** of the code and a **light paper** detailing the design for alias distribution. |
| **0c.** | Testing and Testing Guide | The contract will be first deployed on the Polkadot testnet and will be fully covered by unit tests to ensure functionality and robustness. In the guide, we will describe how to run these tests. |
| **0d.** | Docker | We will provide a Dockerfile that can be used to test all the functionality delivered with this milestone. |
| 0e. | Article | We will publish a **Medium post** that explains how Semaphore alias distribution works and its pivotal role in the security of the network. |
| 1. | Substrate module: (De)activate | We will implement a peer-to-peer networking protocol into our nodes to support bootstrapping and consensus for remote nodes. |
| 2. | Substrate module: Mint | We will complete the consensus mechanisms outlined in the Semaphore white paper and show guarantees of safety and liveness. |
| 3. | Substrate module: Increase Supply | We will integrate the state transition functionality from the sequencer into the Semaphore node software |
| 4. | Semaphore module: Alias Checkpointing | We will enable a central node with the ability to mint new aliases and bind them to pubkeys for external parties to request and use. |
| 5. | Semaphore Alias Smart Contract | Modules 1,2, and 3 will be implemented as functions of the smart contract, following the system we design to manage Semaphore aliases. Module 4 will switch the Python test net from the centralized alias production to using the alias management contract deployed on Polkadot. |

## Future Plans

Upon completion of the grant, we plan to launch the testnet and smart contracts, and immediately start distributing aliases. We also aim to write a full, optimized implementation of the protocol in Rust. After doing this, the goal is to decentralize and test the protocol as quickly as possible. This also means that we will be heavily publicizing the projects through channels such as online forums, academic conferences, hackathons, etc. Finally, we think it is crucial for us to build our own first app on top of the network. This will serve the basis for adding early users and allow us to provide third-party developers with the tools to build on top of Semaphore.

## Additional Information :heavy_plus_sign:

**How did you hear about the Grants Program?**

Web3 Foundation Website 

We have received no other financial investments at this time. We have received a research grant from Stanford IORH to design the karma system for feeless block space allocation. We have also applied for a grant from ESP.
