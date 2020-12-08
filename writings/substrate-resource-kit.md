# Substrate Builder's Resource Kit

The purpose of this guide is to provide a clear set of steps that any developer can take to get
started building blockchains with [Substrate](https://www.substrate.io/), as well as some suggested
paths for next-steps for intermediate Substrate developers. This resource kit is meant to supplement
the [Polkadot & Substrate: Zero to Hero](substrate.md) guide.

## Getting Started

Complete the first four tutorials to get acquainted with Substrate and the FRAME runtime library.
These [tutorials](https://substrate.dev/en/tutorials) will introduce several important tools for
Substrate builders, including the
[Node Template](https://github.com/substrate-developer-hub/substrate-node-template), the
[Front-End Template](https://github.com/substrate-developer-hub/substrate-front-end-template), and
the [Polkadot{JS}](https://polkadot.js.org/) family of client-side utilities.

1. [Your First Chain](https://substrate.dev/docs/en/tutorials/create-your-first-substrate-chain/) -
   Setup a local
   [development environment](https://substrate.dev/docs/en/knowledgebase/getting-started/) and learn
   the basics of building and starting the Node Template, and using the Front-End Template to
   interact with it. _Related video_:
   [Introduction to Substrate](https://www.crowdcast.io/e/xzdm2hyq)
1. [Add a Pallet](https://substrate.dev/docs/en/tutorials/add-a-pallet/) - Extend the capabilities
   of the Node Template by adding a prebuilt
   [runtime module](https://substrate.dev/docs/en/knowledgebase/runtime/). _Related video_:
   [Design of a Pallet](https://www.crowdcast.io/e/substrate-seminar/4)
1. [Build a dApp](https://substrate.dev/docs/en/tutorials/build-a-dapp/) - Create a custom
   Substrate-based application by writing a
   [FRAME](https://substrate.dev/docs/en/knowledgebase/runtime/frame) pallet and a custom front-end.
   _Related video_: [Custom UIs for Substrate](https://www.crowdcast.io/e/substrate-front-ends)
1. [Upgrade a Chain](https://substrate.dev/docs/en/tutorials/upgrade-a-chain/) - Perform a forkless
   [runtime upgrade](https://substrate.dev/docs/en/knowledgebase/runtime/upgrades) and learn more
   about access control and runtime development with FRAME. _Related video_:
   [FRAME's Origin Primitive](https://www.crowdcast.io/e/substrate-seminar/24)

## Next Steps

Understanding the basics of Substrate and FRAME is just the beginning. There are some other things
that all Substrate developers should be familiar with:

- Understand the
  [structure](https://github.com/substrate-developer-hub/substrate-node-template#template-structure)
  of the Node Template.
- [Awesome Substrate](https://github.com/substrate-developer-hub/awesome-substrate) is an awesome
  list of all the awesome developer resources in the Substrate ecosystem.
- Dig deep into the design and implementation of Substrate by exploring the
  [Knowledge Base](https://substrate.dev/docs/en/).
- Find working examples of common Substrate development patterns with
  [Substrate Recipes](https://substrate.dev/recipes/).
- The Substrate [Rustdocs](https://substrate.dev/rustdocs/) are well-maintained and contain a wealth
  of information related to the design, implementation, and usage of Substrate's capabilities.
- Get comfortable browsing Substrate's source code, especially the
  [core FRAME pallets](https://github.com/paritytech/substrate/tree/v2.0.0/frame) and
  [example node](https://github.com/paritytech/substrate/tree/v2.0.0/bin/node).
- Get to know Polkadot{JS} by checking out its [docs](https://polkadot.js.org/docs/). Explore the
  [Polkadot{JS} Apps UI](https://polkadot.js.org/apps/?rpc=wss%3A%2F%2Fkusama-rpc.polkadot.io#/explorer),
  which is the _de facto_ front-end for Substrate-based chains.
- Learn about [Subkey](https://substrate.dev/docs/en/knowledgebase/integrate/subkey), the official
  Rust-based command line interface for working with cryptographic keys for Substrate-based chains.
- Run the [Sidecar](https://github.com/paritytech/substrate-api-sidecar) REST service alongside a
  locally running development node. Learn more about Sidecar by watching this
  [recording of Substrate Seminar](https://www.crowdcast.io/e/substrate-seminar/2) that features
  Sidecar's lead developer.
- Explore the
  [Substrate Enterprise Sample](https://github.com/substrate-developer-hub/substrate-enterprise-sample),
  a fully-featured demonstration of a decentralized supply chain application built on Substrate.
  This is a great reference for exploring advanced runtime development with FRAME and front-end
  development with Polkadot{JS}. It incorporates a number of powerful capabilities, like an
  [off-chain worker](https://substrate.dev/docs/en/knowledgebase/runtime/off-chain-workers) for data
  integration,
  [signed extensions](https://substrate.dev/docs/en/knowledgebase/learn-substrate/extrinsics#signed-extension)
  for scalable access-control, and integrating existing specifications, like the
  [Decentralized Identifier spec](https://w3c.github.io/did-core/).
- Watch a [demonstration of Cumulus](https://www.crowdcast.io/e/zpnjlj0r) to learn more about
  parachains. Read the
  [Parachain Implementer's Guide](https://w3f.github.io/parachain-implementers-guide/) to dig deep
  into the protocols that power parachains.

## Want More?

This is just the beginning!

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

### Build with Smart Contracts

Follow the [Substrate Contracts Workshop](https://substrate.dev/substrate-contracts-workshop/#/) for
an end-to-end smart contract demonstration that uses the FRAME
[Contracts pallet](https://github.com/paritytech/substrate/tree/master/frame/contracts) and the
[ink! smart contract language](https://github.com/paritytech/ink). The Contracts Workshop uses the
[Canvas Node](https://github.com/paritytech/canvas-node) and
[Canvas UI](https://github.com/paritytech/canvas-ui), which can be used as starting points to build
a custom Substrate-based blockchain with smart contract capabilities. Follow the
[Add the Contracts Pallet tutorial](https://substrate.dev/docs/en/tutorials/add-contracts-pallet/)
to learn how to enhance the Node Template by adding smart contract capabilities.

Check out [Moonbeam's docs](https://docs.moonbeam.network/) for best-in-class support of EVM smart
contracts on Substrate-based chains.

### Maintain the Polkadot Network

Decentralized networks require a vibrant
[community](https://wiki.polkadot.network/docs/en/community) of user-maintainers and the Polkadot
Network is no different. Visit the information-packed
[Polkadot Wiki](https://wiki.polkadot.network/en/) to learn how to support the Polkadot Network by
[running a validator](https://wiki.polkadot.network/docs/en/maintain-guides-how-to-validate-polkadot),
[participating in governance](https://wiki.polkadot.network/docs/en/maintain-guides-democracy), and
more.
