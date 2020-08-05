# Substrate v2.0.0-rc5+2

The latest version of Substrate
([v2.0.0-rc5+2 - River Dolphin](https://github.com/paritytech/substrate/releases/tag/v2.0.0-rc5%2B2)) was released on
July 24th. Here is an overview of some of the new capabilities it included. The ones I have chosen to highlight are
those that I found particularly interesting in my role as Parity Developer Advocate :nerd_face:

## Client Service Refactor

Substrate core developer Ashley ([@expenses](https://github.com/expenses) on GitHub) made significant enhancements to
the usability and maintainability of Substrate’s client (blockchain node) layer, spread across two pull requests.
[The first PR](https://github.com/paritytech/substrate/pull/6352) removed the relatively opaque
[`Service`](https://github.com/paritytech/substrate/blob/v2.0.0-rc4/client/service/src/lib.rs#L109) struct and replaced
it with the more straightforward
[`ServiceComponents`](https://github.com/paritytech/substrate/blob/v2.0.0-rc5+2/client/service/src/lib.rs#L154) struct;
this makes it easier for developers to understand the important capabilities provided by the client, such as
peer-to-peer networking, an RPC server, and a task manager to orchestrate the components of the client.
[The second PR](https://github.com/paritytech/substrate/pull/6557) replaced the
[`ServiceBuilder`](https://github.com/paritytech/substrate/blob/v2.0.0-rc4/client/service/src/builder.rs#L87) struct
with a new [`build`](https://github.com/paritytech/substrate/blob/v2.0.0-rc5+2/client/service/src/builder.rs#L407)
function that takes a single argument in the form of a
[`ServiceParams`](https://github.com/paritytech/substrate/blob/v2.0.0-rc5+2/client/service/src/builder.rs#L375) struct
and has the above-mentioned `ServiceComponents` type as its return value. This new pattern provides a more idiomatic and
declarative mechanism for instantiating client capabilities as opposed to relying on a disjointed set of builder
methods. Check out
[the changes to the Node Template’s client service declaration](https://github.com/substrate-developer-hub/substrate-node-template/compare/v2.0.0-rc4...v2.0.0-rc5?diff=split#diff-e85d0410af2924eec622c5f1b079831f)
to see Ashley’s awesome changes in action!

## EVM Compatibility Enhancements

Wei Tang ([@sorpaas](https://github.com/sorpaas) on GitHub) authored three pull requests that enhanced the capabilities
of the [EVM (Ethereum Virtual Machine) pallet](https://substrate.dev/rustdocs/v2.0.0-rc5/pallet_evm/index.html).
[The first PR](https://github.com/paritytech/substrate/pull/6493) changed the pallet’s API so that failing calls to EVM
contracts return `Ok`; this allows EVM gas fees to be applied to the chain state. Wei’s
[second PR](https://github.com/paritytech/substrate/pull/6537) allows FRAME developers that include the EVM pallet in
their runtime to configure it with a chain ID. Moonbeam, the cutting-edge Substrate-based network for Ethereum
interoperability,
[selected 43](https://github.com/PureStake/moonbeam/blob/650281213d5469596ae83211ad18f4b3a3655cac/runtime/src/lib.rs#L278)
as their chain ID. Finally, and perhaps most significantly, Substrate system storage is now used to maintain the balance
and nonce for an EVM address. [The third PR](https://github.com/paritytech/substrate/pull/6659) provides two important
capabilities: 1) it gives developers the ability to use Substrate’s powerful
[`Origin`](https://substrate.dev/docs/en/knowledgebase/runtime/origin) primitive to control access to EVM operations
such as withdrawing balances and calling contracts; 2) it makes it easier for Ethereum interoperability chains like
Moonbeam to use a single account ID for Substrate and EVM accounts.

## Deterministic Wasm Builds

Ben Kampmann, better known as [@gnunicorn](https://github.com/gnunicorn) on GitHub, included tests to ensure that
Substrate’s build mechanisms produce deterministic Wasm output. Given the importance of Wasm build artifacts to the
Substrate ecosystem, this is more than just a nice-to-have; it allows users of Substrate-based chains to use utilities
such as [Chevdor’s `srtool`](https://gitlab.com/chevdor/srtool) to verify the Wasm blobs used to enable forkless
upgrades. Ben published an
[excellent write-up](https://dev.to/gnunicorn/hunting-down-a-non-determinism-bug-in-our-rust-wasm-build-4fk1) that
explains how he was able to work with some of the core maintainers of the Rust language to bring deterministic Wasm
builds back to the Substrate community.

## FRAME System No Longer Aliased to `system`

A [pull request](https://github.com/paritytech/substrate/pull/6500) from Substrate community member
[Shaun Wang](https://github.com/shaopengw) of Laminar included a major readability improvement. Shaun removed the alias
used by FRAME’s [`decl_module!`](https://substrate.dev/docs/en/knowledgebase/runtime/macros#decl_module) macro that
renamed the [`frame_system`](https://substrate.dev/docs/en/knowledgebase/runtime/frame#system-library) crate to
`system`. This reinforces the idiom that crates that begin with `frame_` are part of the mandatory core FRAME framework,
while those that begin with `pallet_` are optional modules that may be used to extend a FRAME-based runtime with
additional capabilities.
