# Rust Syntax for Substrate and FRAME

This document is being written to prepare for (and accompany)
[an upcoming Substrate Seminar](https://www.crowdcast.io/e/substrate-seminar/28). The purpose of
this document and the upcoming Seminar is to make it easier for developers that are working with
[Substrate](https://www.substrate.io/) and
[FRAME](https://substrate.dev/docs/en/knowledgebase/runtime/) to understand the advanced
[Rust](https://www.rust-lang.org/) syntax that is used by these frameworks. In particular,
[the `Trait`](https://github.com/substrate-developer-hub/substrate-node-template/blob/v2.0.0/pallets/template/src/lib.rs#L16)
configuration [trait](https://doc.rust-lang.org/book/ch10-02-traits.html) (and related code) from
the Node Template's pallet template will be used to guide this discussion.

## Introduction

Substrate and FRAME are heavyweight frameworks that define a number of conventions. Furthermore, the
Rust programming language is a next-generation
[system programming language](https://en.wikipedia.org/wiki/System_programming_language) that
introduces novel concepts for solving problems that many _application_ developers may not be
familiar with thinking about. Developers learning Substrate should always make sure they understand
the "layer of abstraction" (Rust versus Substrate versus FRAME) on which any given concept resides.
Consider the `Trait` configuration trait:

- **Rust**: The `Trait` configuration trait is
  [a Rust trait](https://doc.rust-lang.org/book/ch10-02-traits.html). Traits in Rust are like
  _interfaces_ in languages like [C++](https://www.tutorialcup.com/cplusplus/interfaces.htm),
  [Java](https://docs.oracle.com/javase/tutorial/java/concepts/interface.html),
  [Python](https://realpython.com/python-interface/),
  [TypeScript](https://www.typescriptlang.org/docs/handbook/interfaces.html), or
  [Go](https://tour.golang.org/methods/9). Like an interface, a trait is used to specify the
  behavior of a type without specifying how that behavior is implemented (i.e. function signatures).
  Unlike the interface primitives in many other languages, though, Rust traits allow developers to
  define
  "[associated types](https://doc.rust-lang.org/book/ch19-03-advanced-traits.html#specifying-placeholder-types-in-trait-definitions-with-associated-types)",
  which are similar to generic types that can be used to parameterize the trait.
- **Substrate**: The Substrate framework uses a number of
  [core runtime interfaces](https://substrate.dev/docs/en/knowledgebase/runtime/primitives#core-primitives)
  (traits) to define the expected capabilities and type bounds of a chain's runtime system (e.g.
  [`Hash`](https://substrate.dev/rustdocs/v2.0.0/sp_runtime/traits/trait.Hash.html) and
  [`Header`](https://substrate.dev/rustdocs/v2.0.0/sp_runtime/traits/trait.Hash.html)). These
  interfaces are satisfied, in part, by way of the core
  [FRAME System `Trait`](https://substrate.dev/rustdocs/v2.0.0/frame_system/trait.Trait.html)
  configuration trait (e.g.
  [`Hash`](https://substrate.dev/rustdocs/v2.0.0/frame_system/trait.Trait.html#associatedtype.Hash)
  and
  [`Header`](https://substrate.dev/rustdocs/v2.0.0/frame_system/trait.Trait.html#associatedtype.Header)).
- **FRAME**: Like the core FRAME System itself, individual FRAME pallets must define a Rust trait
  named `Trait` that is used to define and configure the capabilities upon which the pallet depends.
  The fact that the terms "trait" and "`Trait`" are overloaded is an unfortunate design decision
  that has already been rectified (the trait has been
  [renamed to `Config`](https://github.com/paritytech/substrate/pull/7599)) and is scheduled for
  imminent "official" release. For the purposes of consistency and clarity, this document will use
  the currently supported terminology (i.e. `Trait`). FRAME's `Trait` configuration trait is _only_
  used to define associated types - it should not be used to specify function signatures. However,
  each of the `Trait` configuration trait's associated types are themselves described as a set of
  "[trait bounds](https://doc.rust-lang.org/rust-by-example/generics/bounds.html)", and these traits
  are often used to specify available function. For example, the
  [`frame_system::Trait::Hash` _type_](https://substrate.dev/rustdocs/v2.0.0/frame_system/trait.Trait.html#associatedtype.Hash)
  is "bound by" the
  [`core::hash::Hash` _trait_](https://doc.rust-lang.org/nightly/core/hash/trait.Hash.html), which
  provides the
  [`hash` function](https://doc.rust-lang.org/nightly/core/hash/trait.Hash.html#tymethod.hash) and
  the
  [`hash_slice` function](https://doc.rust-lang.org/nightly/core/hash/trait.Hash.html#method.hash_slice).

In addition to understanding the above, it's important for new Substrate and FRAME developers to
simply accept that some thing just are the way they are. This is no different than how every
programming language makes certain design decisions that developers using that language must accept.
An example of such a design decision in FRAME is the fact that pallets that wish to emit
[events](https://substrate.dev/docs/en/knowledgebase/runtime/events) and
[errors](https://substrate.dev/docs/en/knowledgebase/runtime/errors) are only required to define an
[`Event` type](https://github.com/substrate-developer-hub/substrate-node-template/blob/v2.0.0/pallets/template/src/lib.rs#L18)
within their trait configuration trait but are not required to do so for errors, despite the fact
that events and errors are both "just enums". The technical reason for this distinction is
_probably_ that events may be parameterized with respect to certain pallet-specific types whereas
errors may not. However, the fact that events may be parameterized with respect to such types while
errors may not is a design decision that FRAME developer must "just accept".
