# Add a Pallet to a Substrate Node

So, you want to add a FRAME pallet to a Substrate node? This document is aimed at people with experience writing code
and interacting with decentralized technologies like blockchain, but who are still learning about the Substrate
ecosystem and who are not familiar with programming in Rust. The goal of this document is to make it easier for you to
understand the capabilities that a pallet exposes and determine the steps needed to add any pallet to your runtime. If
none of that makes sense to you, [go to substrate.dev](https://substrate.dev/) to learn more about how easy it can be
to write your own blockchain. In particular, you may want to get to the point that you're able to complete
[the excellent tutorial that shows you how to add a pallet to a Substrate node](https://substrate.dev/docs/en/tutorials/add-a-pallet-to-your-runtime/).

## Some Terminology

If you are new to Rust and Substrate, you are probably awash in
[a sea of unfamiliar terminology](https://substrate.dev/docs/en/knowledgebase/getting-started/glossary). If you feel
lost, you've come to the right place. This document will explain a number of terms, but to get started you should
understand:

* [**Rust**](https://www.rust-lang.org/) - a programming language from Mozilla and the creators of Javascript that is
  beloved for being fast, safe and concurrent; these capabilities come at the expense of unfamiliar programming paradigms
  and long compile times
* [**Crate**](https://crates.io/) - a Rust package
* [**FRAME**](https://substrate.dev/docs/en/knowledgebase/runtime/frame) - a component of the Substrate framework that is
  comprised of a number of Rust crates; the goal of FRAME is to provide a flexible set of building blocks across a number of
  abstraction layers by balancing reusability with opinionated design
* **Pallet** - a Rust crate that is part of FRAME; some of these crates _must_ be included to use FRAME and their
  package names will begin with `frame_`, optional FRAME pallets have package names that begin with `pallet_`

## Dusting Off the Rust

This document is not intended to be [a crash course in Rust](https://github.com/rust-lang/rustlings), but instead the
goal is to the give you the context you need to accomplish a pretty specific task: adding a FRAME pallet to a Substrate
node. One of the first things you'll want to be able to do is read the documentation for a FRAME crate, which are
written in Rust. Take a look at
[the documentation for the FRAME System crate (`frame_system`)](https://crates.parity.io/frame_system/index.html).
At the top of this page, you will see a helpful introduction that the crate's maintainers have written about the
interface of the crate; this is the best place to start understanding the types of capabilities that the crate exposes.
After this introduction you will see sections for each of the top-level components exposed by the crate; in this case,
these are: [Modules](https://crates.parity.io/frame_system/index.html#modules),
[Structs](https://crates.parity.io/frame_system/index.html#structs),
[Enums](https://crates.parity.io/frame_system/index.html#enums),
[Traits](https://crates.parity.io/frame_system/index.html#traits),
[Functions](https://crates.parity.io/frame_system/index.html#functions),
[Type Definitions](https://crates.parity.io/frame_system/index.html#types). Keep in mind that not all crates
define components of these types, so you may not always see each section when reading the Rust docs for other FRAME
pallets. Furthermore, there may be other types of top-level components that the System crate does not define and
therefore will not be discussed below. Rust docs use different colors to delineate these different types of top-level
components. Take note of these colors, since Substrate crates often have multiple components that share the same name.

### <font color='goldenrod'>Modules</font>

The System crate explicitly defines a single module,
[the `offchain` module](https://crates.parity.io/frame_system/offchain/index.html).
If you click into the documentation for the `offchain` module, you will notice that, like the `frame_system`
documentation, it also has headings for
[Structs](https://crates.parity.io/frame_system/offchain/index.html#structs) and
[Traits](https://crates.parity.io/frame_system/offchain/index.html#traits). Rust crates
implicitly expose a root module, but can also define sub-modules, such as the `offchain` module. Since the purpose of
this section is to help you understand how to read Rust module documentation, skip over the `offchain` module for now.
You should be able to make sense of the `offchain` module documentation by the time you're done reading this article.

### <font color='aqua'>Structs</font>

Along with [Enums](#Enums), the Structs section of a Rust doc enumerates the
[custom data types](https://doc.rust-lang.org/stable/rust-by-example/custom_types.html) defined by a crate.
[Rust structs](https://doc.rust-lang.org/book/ch05-00-structs.html) can have fields (i.e. attributes or properties),
define their own methods and implement externally-defined interfaces, which Rust calls [Traits](#Traits). The structs
exposed by a FRAME pallet may be defined explicitly, as in
[the `AccountInfo` struct](https://github.com/paritytech/substrate/blob/v2.0.0-rc2/frame/system/src/lib.rs#L329-L340) from the
System crate, or they may be defined by way of one of [Substrate's macros](https://substrate.dev/docs/en/knowledgebase/runtime/macros),
as in [the System crate's `Account` struct](https://github.com/paritytech/substrate/blob/v2.0.0-rc2/frame/system/src/lib.rs#L425-L427).
Many of the macro-defined structs in the System pallet, including `Account`, are defined by way of
[the `decl_storage` macro](https://crates.parity.io/frame_support/macro.decl_storage.html). Structs define
the data structures you will need to instantiate in order to interact with a FRAME pallet and consume the capabilities
it exposes. For instance,
[the System pallet's `GenesisConfig` struct](https://crates.parity.io/frame_system/struct.GenesisConfig.html)
allows you to configure the behavior of the System pallet by providing it with initial values in the genesis block of
your blockchain. Examine the System pallet's `GenesisConfig` struct and take note of the fields and methods it exposes
and the traits that it implements. The fields and methods encapsulate the struct's purpose-built capabilities. The
trait implementations represent the mechanism that allow the struct to interact with other Rust components that are
defined outside the struct's module. You should be familiar with the following macro-defined structs that are used
throughout Substrate and FRAME.

#### The `Module` Struct

A FRAME pallet's `Module` struct is created by
[the `decl_module` macro](https://crates.parity.io/frame_support/macro.decl_module.html). It is a
collection of functions that are meant to...

```js
// TODO: explain the purpose of the Module struct
```

#### The `GenesisConfig` Struct

This struct is created by the `decl_storage` macro and provides capabilities for configuring a pallet's storage values
within the genesis block of a blockchain. Learn
[more about Substrate storage genesis configuration](https://substrate.dev/docs/en/knowledgebase/runtime/storage#genesis-configuration)
at substrate.dev.

### <font color='lime'>Enums</font>

[Rust enums](https://doc.rust-lang.org/book/ch06-00-enums.html) are like structs, in that they allow you to define
custom data types with fields and methods, and can implement externally defined interfaces (traits). Enums, however,
introduce a semantic distinction from structs, which is that they are meant to define types whose instances will always
represent _exactly one_ of a number of _well-defined_ mutually exclusive _variants_.
[Rust's `switch`-like `match` operator](https://doc.rust-lang.org/book/ch06-02-match.html) takes advantage of the fact
that enums will always represent a single variant. Like structs, the enums that a FRAME pallet exposes may be defined
explicitly, like [the `InitKind` enum](https://github.com/paritytech/substrate/blob/v2.0.0-rc2/frame/system/src/lib.rs#L880-L893)
from the System crate, or they may be defined by way of a Substrate macro, such as the System crate's
[`Call` enum](https://github.com/paritytech/substrate/blob/v2.0.0-rc2/frame/system/src/lib.rs#L544). In fact, the Substrate
macro language makes extensive use of enums to delineate the interfaces exposed by a pallet and there are a number of
common macro-defined enums that you should be familiar with.

#### The `Call` Enum

Along with [the `Module` struct](#The-Module-Struct), this enum is created by
[the `decl_module` macro](https://crates.parity.io/frame_support/macro.decl_module.html) and lists
the callable/dispatchable pallet functions that can be invoked by way of an
[extrinsic](https://substrate.dev/docs/en/knowledgebase/learn-substrate/extrinsics). These functions differ from those
defined in [the `Module` struct](#The-Module-Struct) in that...

```js
// TODO: contrast the Call enum functions with the Module struct functions
```

#### The `Error` Enum

The `Error` enum is created by
[the `decl_error` macro](https://crates.parity.io/frame_support/macro.decl_error.html) and
represents the types of errors that may originate from a FRAME pallet. These errors are important because...

```js
// TODO: explain where developers will encounter errors and how they should be prepared to handle them
```

#### The `RawEvent` Enum

Events are the mechanisms that blockchains use to communicate with the outside world, and FRAME pallets can declare and
emit custom events. This enum describes the events a pallet defines and is created by way of
[the `decl_event` macro](https://crates.parity.io/frame_support/macro.decl_event.html).

#### The `RawOrigin` Enum

In Substrate, the term
["origin" is used to describe the entity that invoked a dispatchable call](https://substrate.dev/docs/en/knowledgebase/runtime/origin)
and the required System pallet defines
[the three default origins](https://crates.parity.io/frame_system/enum.RawOrigin.html): `Root`,
`Signed(AccountId)` and `None`. Other pallets may define their own custom origins, which will be described in their own
`RawOrigin` enum, such as
[that for the Collective pallet](https://crates.parity.io/pallet_collective/enum.RawOrigin.html).
The origins described by a pallet's `RawOrigin` enum will be exposed to the blockchain runtime by way of
[the `decl_module` macro](https://crates.parity.io/frame_support/macro.decl_module.html) that is
used to construct the pallet's module.

### <font color='purple'>Traits</font>

[Traits](https://doc.rust-lang.org/book/ch10-02-traits.html) are Rust's powerful interface system. You may be familiar
with interfaces from other programming languages like Java or C#, where they are used to encapsulate capabilities by
defining a set of methods. In Rust, however, traits go beyond encapsulating methods and also encapsulate, in a
[generic](https://doc.rust-lang.org/book/ch10-00-generics.html) way, the types of data on which the methods can
operate. This is because, like Google's Go programming language, Rust does not support
[inheritance, and instead uses composition](https://en.wikipedia.org/wiki/Composition_over_inheritance) to express
shared behavior and reduce code duplication; this places a greater burden on the interface system and requires it to
expose more capabilities. FRAME pallets define traits in order to abstract the capabilities that they expose and
describe the data types they need to provide those capabilities. Some of these traits will be used to describe the
behaviors of custom data types while others, notably the `Trait` trait, are used to declare a pallet's dependencies and
capabilities.

#### The `Trait` Trait

This trait is an important part of declaring a FRAME pallet's module with
[the `decl_module` macro](https://crates.parity.io/frame_support/macro.decl_module.html) as well as
adding a pallet to your Substrate node. It has been referred to as
["the configuration trait"](https://substrate.dev/recipes/2-appetizers/1-hello-substrate.html#configuration-trait). As
mentioned above, traits in Rust are the mechanism that data types use to express the types of data on which they depend.
This is especially important in Substrate, where many interleaved layers and components operate on shared primitives
within the runtime of your blockchain. The Rust components that inform Substrate do not explicitly define data types for
these primitives; instead, pallets describe their data type dependencies in terms of the capabilities that those data
types expose (their traits). For example, take a look at
[the `Origin` type of the System pallet's Trait trait](https://crates.parity.io/frame_system/trait.Trait.html#associatedtype.Origin)
as well as
[the way that this `Origin` type is implemented in the Substrate node template](https://github.com/paritytech/substrate/blob/v2.0.0-alpha.6/bin/node-template/runtime/src/lib.rs#L149).
[As described above](#The-RawOrigin-Enum), origins are Substrate's mechanism for describing the entity that invoked a
dispatchable call. Although the origins exposed by the required System pallet can be thought of as the "default" set of
origins, they still needed to be configured within the `runtime/lib.rs` file that defines a Substrate node's blockchain
runtime. You can think of this as binding the pallet's notion of an origin (the capabilities it exposes or its
interface) to that of the runtime. This allows these two Substrate components to operate on a shared notion of "origin"
without creating tight couplings between these two important code modules that would restrict the flexibility of
blockchain runtime developers.

### <font color='green'>Functions</font>

### <font color='orange'>Type Definitions</font>
