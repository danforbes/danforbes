# Substrate: Zero to Hero

[Substrate](https://www.substrate.io/) is an [open-source](https://github.com/paritytech/substrate)
[Rust](https://www.rust-lang.org/) framework for building blockchains that is curated by
[Parity Technologies](https://www.parity.io/). Substrate is designed for building blockchains for
[Polkadot](https://polkadot.network/)-like networks, which implement protocols designed by
researchers at the [Web3 Foundation](https://web3.foundation/). The purpose of this guide is to
provide a clear entry point to make it easy to understand the admittedly complex web of concepts and
technologies that informs the Substrate and Polkadot ecosystems.

## Parity Technologies

Parity Technologies was founded in 2016 by [Dr. Gavin Wood](https://www.parity.io/gavin-wood/) and
[Dr. Jutta Steiner](https://www.parity.io/jutta-steiner/) two years after they helped found
[Ethereum](https://ethereum.org/), the ground-breaking smart contract platform that actualized the
promise of a decentralized "world computer". Parity was initially known for the Rust-based
[Parity Ethereum client](https://www.parity.io/ethereum/), but went on to develop infrastructure for
a range decentralized protocols, such as the
[Parity Zcash client](https://github.com/paritytech/parity-zcash).

## Web3 Foundation

The Web3 Foundation is a research organization that was also founded by Dr. Gavin Wood. Its mission
is to nurture cutting-edge applications of decentralized protocols in order to create a more fair
Internet where users control their own identity and data. The Web3 Foundation's flagship project is
[Polkadot](https://research.web3.foundation/en/latest/polkadot/overview.html) - a next-generation
protocol for trustless, scalable, decentralized networking.

### Why Blockchain?

A blockchain is a means to an end; it is a platform upon which novel mechanisms for trustless human
interaction can be built. Blockchain technology is an enablement layer for the Web 3.0 vision.

- Web 1.0: the original incarnation of the Internet that existed to allow researchers to freely
  communicate and share their research
- Web 2.0: the modern Internet that relies heavily on broken trust models and layers of legacy
  protocols to provide critical infrastructure like commerce and healthcare
- Web 3.0: a trustless, egalitarian Internet that uses cryptography to allow individuals to maintain
  custody of their identity and personal data

## Polkadot

The [Polkadot Paper](https://polkadot.network/PolkaDotPaper.pdf) (a playful reference to the term
"whitepaper") describes a multi-blockchain network with a central "relay chain" at its core. The
purpose of a relay chain is to provide shared infrastructure and security for the
application-specific "parachains" that it supports. This design support two important capabilities:
_scalability_ & _interoperability_. Scalability is provided by decoupling the concerns of
infrastructure and application logic - by focusing solely on infrastructure, the relay chain is able
to support a relatively large number of parachains that are each free to focus on specialized
capabilities. Furthermore, the relay chain provides trustless message-passing capabilities among all
the parachains it supports, which means that the blockchains in a Polkadot-like network are
interoperable by default. Another key capability supported by the design of the Polkadot protocol is
_flexibility_. Although the Polkadot protocol specifies that the relay chain implement a
[nominated proof-of-stake](https://research.web3.foundation/en/latest/polkadot/NPoS/index.html)
consensus mechanism, parachains are free to implement their own consensus mechanisms. This is why
Polkadot is referred to as a "heterogenous" multi-blockchain network, because there is no
requirement for homogeneity with respect to consensus mechanisms. Last, but by no means least,
Polkadot specifies a mechanism by which blockchains may be _upgraded_ over time, which is a
groundbreaking capability that allows network maintainers to improve their blockchains to serve the
needs of the network.

It is worth reiterating and enumerating these characteristics of Polkadot-like networks:

- **Scalability** through separation of concerns
- **Interoperability** through trustless message passing
- **Flexibility** through separation of concerns & upgradeability

### A Note About Smart Contracts

Many people who have explored blockchain technologies are familiar with the concept of
[smart contacts](https://en.wikipedia.org/wiki/Smart_contract) and may even consider them to be a
necessary component of a blockchain ecosystem. This is not the case in a Polkadot-like network.
Although the Polkadot protocol supports smart contract platforms, it is only one mechanism by which
blockchain developers and users can interact with blockchains in a Polkadot-like network. Keep
reading to learn how smart contracts fit in to the Polkadot ecosystem.

## Substrate

Substrate was created in order to accelerate the development of multiple Polkadot-like networks.
Substrate is being used by Parity Technologies to implement the relay chains of Polkadot-like
networks, including the eponymous Polkadot Network, as well as others like the boundary-pushing
[Kusama Network](https://kusama.network/). Furthermore, Substrate is used by Parity and dozens of
other teams to build application-specific parachains for Polkadot-like networks or even standalone
blockchains that do not require the services of a relay chain. The Substrate framework for building
blockchains focuses on flexibility, and it provides this capability by pushing the Rust programming
language to its limits through advanced use of
[macros](https://doc.rust-lang.org/reference/macros.html) and
[generics](https://doc.rust-lang.org/book/ch10-00-generics.html). Substrate breaks a blockchain down
into a number of modular components that can be configured and customized to meet a variety of
needs:

- Runtime: defines state transition logic that informs a blockchain's user-facing capabilities
- Networking: allows blockchain clients to interact with one another
- Consensus: the mechanism by which network participants come to agreement on the state of the
  runtime and its history
- Storage: represents the current state of the runtime and maintains an archive of its history
- Remote Procedure Call (RPC) Server: allows users to interact with the blockchain

### FRAME

FRAME is Substrate's standard library for runtime development. It is not the only way to build
Substrate runtimes, but it is the method that Parity supports and that is used to build the runtimes
of relay chains like the Polkadot and Kusama Networks. FRAME defines a number of concepts and
primitive types for runtime development, and provides a framework that allows developers to create a
custom runtime by creating, composing, and configuring runtime modules, known as "pallets". It's
easy to write a pallet to encapsulate custom business logic or reuse existing pallets, many of which
are maintained by Parity and undergo third-party audits before being battle-tested on relay chains
like Polkadot and Kusama. The standard FRAME pallets cover a range of capabilities:

- Utilities like on-chain randomness & timestamps, multi-signature accounts, and batch transactions
- System administration like mechanisms for managing transaction payments and access to the root
  user
- Consensus mechanism that use on-chain data for improved security and decentralization
- Governance capabilities that provide realistic, well-researched approaches to managing things like
  network upgrades
- Smart contracts to allow users to securely deploy custom logic to the runtime

### Back to Smart Contacts

With Substrate & FRAME, it's easy to build a wide range of capabilities directly into the runtime of
an application-specific blockchain. With Polkadot, it's easy to deploy a custom-built blockchain
into a secure network of interoperable blockchains. This means it is no longer necessary to rely on
monolithic, legacy smart contract platforms that require all users to draw from a single pool of
shared resources. An example of a blockchain-based application that may not require smart contract
capabilities is a supply chain consortium, which has a clear group of users with a common set of
needs. In cases such as this, the application logic can be programmed directly into the blockchain's
runtime, which avoids the overhead of an additional smart contract execution environment.
Furthermore, thanks to Polkadot's groundbreaking concept of forkless runtime upgrades, it's even
possible to extend and maintain a blockchain's runtime capabilities as the network's needs change
over time. In other cases, it may be desireable to provide blockchain users with the ability to
deploy smart contracts, such as a gaming chain that may wish to allow users to define character
customizations or even create unique in-game resources. The power of Polkadot and Substrate is that
blockchain application developers now have the _choice_ of when and how to use smart contracts.

## Other Terms

Believe it or not, this is just the start. Here are some other concepts and terms to keep in mind:

- [Polkadot{JS}](https://polkadot.js.org/): the official Javascript client library for interacting
  with Substrate-based chains
- [WebAssembly (Wasm)](https://webassembly.org/): a portable binary format (like Java bytecode); it
  enables Polkadot's runtime upgrade capabilities
- [Cumulus](https://github.com/paritytech/cumulus): a toolkit for turning a standalone
  Substrate-based chain into a Polkadot-ready parachain
- [Bridge](https://github.com/paritytech/parity-bridges-common): a collection of technologies and
  protocols that allow isolated blockchains to communicate; blockchains within a Polkadot-like
  network do not require bridges to communicate with one another, but may use bridges to communicate
  with blockchains outside of their network
- [Frontier](https://github.com/paritytech/frontier): a collection of utilities that makes it easy
  to create a Substrate-based chain that implements Ethereum's virtual machine, block processing,
  and RPC interfaces
- [libp2p](https://libp2p.io/): the multi-protocol networking stack used by Polkadot-like networks
- [Off-chain worker](https://substrate.dev/docs/en/knowledgebase/runtime/off-chain-workers):
  Substrate's mechanism for reliably introducing off-chain data into a runtime
- [ink!](https://paritytech.github.io/ink/): Parity's own Wasm-based smart contract programming
  language
- [SCALE](https://substrate.dev/docs/en/knowledgebase/advanced/codec): Substrate's compact encoding
  scheme

Review the official
[Substrate glossary](https://substrate.dev/docs/en/knowledgebase/getting-started/glossary) to learn
more.

## Getting Started

There are a number of ways to participate in the Polkadot community.

### Build a Chain

Go to the [Substrate Developer Hub](https://substrate.dev/) to learn how to write a custom
blockchain with Substrate and FRAME. The
[first tutorial](https://substrate.dev/docs/en/tutorials/create-your-first-substrate-chain/) is a
guided walk through of getting started with the official Substrate Developer Hub
[Node Template](https://github.com/substrate-developer-hub/substrate-node-template) and
[Front-End Template](https://github.com/substrate-developer-hub/substrate-front-end-template). While
the Node Template compiles, read the introduction to the Substrate Developer Hub
[Knowledge Base](https://substrate.dev/docs/) and get acquainted with its structure and content.
Complete the next three [tutorials](https://substrate.dev/en/tutorials) to get hands-on experience
with Substrate and FRAME. Check out the
[Awesome Substrate](https://github.com/substrate-developer-hub/awesome-substrate) repository for
lots more resources.

### Build on an Existing Chain

Explore the [teams building with Substrate](https://www.substrate.io/substrate-users/) and
contribute to what they're building. Here are some chains that provide excellent developer
experiences:

- [Acala](https://acala.network/): decentralized finance
- [Centrifuge](https://centrifuge.io/): on-chain assets represented as non-fungible tokens (NFTs)
- [KILT](https://www.kilt.io/): self-sovereign identity
- [Kulupu](https://kulupu.network/): smart contract platform with on-chain governance
- [Kusama](https://kusama.network/): experimental network that
  [sponsors](https://app.subsocial.network/@RelayChainGovernance/how-to-submit-a-bounty-proposal-on-kusama-and-polkadot-316)
  innovative applications of blockchain technologies
- [Moonbeam](https://moonbeam.network/): supports Ethereum applications with the next-generation
  capabilities of Substrate and Polkadot
- [Nodle](https://nodle.io/): decentralized Internet of Things
- [OriginTrail](https://origintrail.io/): supply chain management
- [Polkaswap](https://polkaswap.io/): automated market maker
- [Subsocial](https://subsocial.network/): decentralized social network

### Maintain the Polkadot Network

Decentralized networks require a vibrant
[community](https://wiki.polkadot.network/docs/en/community) of user-maintainers and the Polkadot
Network is no different. Visit the information-packed
[Polkadot Wiki](https://wiki.polkadot.network/en/) to learn how to support the Polkadot Network by
[running a validator](https://wiki.polkadot.network/docs/en/maintain-guides-how-to-validate-polkadot),
[participating in governance](https://wiki.polkadot.network/docs/en/maintain-guides-democracy), and
more.
