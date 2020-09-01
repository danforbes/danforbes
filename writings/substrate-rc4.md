# Substrate v2.0.0-rc4

The latest version of Substrate
([v2.0.0-rc4 - Rhinoceros](https://github.com/paritytech/substrate/releases/tag/v2.0.0-rc4)) was released on June 25.
Here is an overview of some of the new capabilities it included. The ones I have chosen to highlight are those that I
found particularly interesting in my role as Parity Developer Advocate :nerd_face:

## RPC Endpoint for Extrinsic Dry Runs

Substrate community member Bryan Chen, better known as [@xlc](https://github.com/xlc) on GitHub and
[Riot](https://riot.im/app/#/room/!HzySYSaIhtyWrwiwEV:matrix.org), added a new RPC endpoint to the
[FRAME System](https://substrate.dev/rustdocs/v2.0.0-rc4/frame_system/index.html) module that allows users to dry run an
extrinsic. This means that extrinsics can be verified before being executed and making state changes. Users submit an
extrinsic to this endpoint and receive a response in the form of the
[`ApplyExtrinsicResult` type](https://substrate.dev/rustdocs/v2.0.0-rc4/sp_runtime/type.ApplyExtrinsicResult.html),
which indicates whether or not the extrinsic was valid, as well as information about the outcome of the dry run (if the
extrinsic would have succeeded or failed, and why it would have failed if so). Bryan created
[a GitHub Issue](https://github.com/paritytech/substrate/issues/6298) for this awesome feature and closed it with
[a PR of his own design](https://github.com/paritytech/substrate/pull/6300) within the span of about a week. Bryan’s day
job is Chief Technology Officer for [Laminar](https://laminar.one/), which makes his amazing contributions even more
impressive!

## Atomic Swap Pallet

A new [FRAME pallet for atomic swaps](https://substrate.dev/rustdocs/v2.0.0-rc4/pallet_atomic_swap/index.html) has been
added to Substrate’s set of [core FRAME pallets](https://substrate.dev/docs/en/knowledgebase/runtime/frame). This pallet
was inspired by [the atomic swap protocol](https://en.bitcoin.it/wiki/Atomic_swap) described in the Bitcoin Wiki. With
this pallet, two parties can easily exchange assets without needing to trust each other or a centralized third-party.
Although this pallet’s [initial implementation](https://github.com/paritytech/substrate/pull/6349) only supported
swapping assets that implemented the
[`ReservableCurrency`](https://substrate.dev/rustdocs/v2.0.0-rc4/frame_support/traits/trait.ReservableCurrency.html)
trait, its author, Wei Tang/[@sorpaas](https://github.com/sorpaas), quickly followed up with
[a second PR](https://github.com/paritytech/substrate/pull/6421) that generalizes the atomic swap capability. The
version of this pallet that appears in the v2.0.0-rc4 release allows users to atomically swap any asset type for which
the [`SwapAction`](https://substrate.dev/rustdocs/v2.0.0-rc4/pallet_atomic_swap/trait.SwapAction.html) trait is
implemented, and also includes an
[implementation for `ReservableCurrency`](https://github.com/paritytech/substrate/blob/v2.0.0-rc4/frame/atomic-swap/src/lib.rs#L124)
types. Wei is one of the primary contributors to the
[EVM pallet](https://substrate.dev/rustdocs/v2.0.0-rc4/pallet_evm/index.html).

## Transactional Storage Updates

Substrate core developer [Alexander Theißen](https://github.com/athei) authored a PR that introduces
[a new `with_transaction` function](https://substrate.dev/rustdocs/v2.0.0-rc4/frame_support/storage/fn.with_transaction.html).
[Alexander’s PR](https://github.com/paritytech/substrate/pull/6269) is a shining example of the hard work that the
Substrate core devs put into the usability of the code they write, and readers are encouraged to refer to the original
PR for a clear and thorough description of the capabilities it introduces. In addition to the excellent PR
documentation, Alex included
[some great tests](https://github.com/paritytech/substrate/blob/v2.0.0-rc4/frame/support/test/tests/storage_transaction.rs#L41)
that demonstrate how storage transactions can be used to delineate a set of storage modifications that will be
guaranteed to succeed or fail as a unit.

## Updates to `ink!` Smart Contracts

Substrate v2.0.0-rc4 included enhancements to improve the experience around
[the Contracts pallet](https://substrate.dev/rustdocs/v2.0.0-rc4/pallet_contracts/index.html). Check them out by
completing [the awesome `ink!` Smart Contracts Tutorial](https://substrate.dev/substrate-contracts-workshop/#/). You can
also follow the [Add the Contracts Pallet Tutorial](https://substrate.dev/docs/en/tutorials/add-contracts-pallet/) to
learn how to add the Contracts pallet to a runtime.
