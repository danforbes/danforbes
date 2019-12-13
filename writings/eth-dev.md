# Ethereum Smart Contract Development Reference

The purpose of this document is to collect notes and resources related to Ethereum smart contract development. Primarily, it is a personal tool for its author, [Dan Forbes](https://github.com/danforbes/danforbes/blob/master/README.md); however, it is being made publicly available in the hopes that others find it useful. Please contact Dan ([dan@danforbes.dev](mailto:dan@danforbes.dev?subject=Ethereum%20Smart%20Contract%20Development%20Reference)) or use a GitHub Issue or Pull Request if you have any questions or would like to suggest any changes.

- :nerd_face: [Smart Contracts](#nerd_face-Smart-Contracts)
- :blue_car: [Gas](#blue_car-Gas)
- :floppy_disk: [Storage, Memory & the Stack](#floppy_disk-Storage-Memory--the-Stack)
- :computer: [The Ethereum Virtual Machine (EVM)](#computer-The-Ethereum-Virtual-Machine-EVM)
  - [Accounts](#Accounts)
- [Application Binary Interface (ABI)](#Application-Binary-Interface-ABI)
- [Solidity](#Solidity)
  - [Data Types](#Data-Types)
  - [Functions](#Functions)
  - [Events](#Events)
- :cd: [Remix Integrated Development Environment (IDE)](#cd-Remix-Integrated-Development-Environment-IDE)
  - [Deploy & Run](#Deploy--Run)
  - [Debugger](#Debugger)
- [web3.js](#web3js)
- :mushroom: [Truffle](#mushroom-Truffle)
- [OpenZeppelin](#OpenZeppelin)
- :bulb: [Ethereum Improvement Proposals (EIPs)](#bulb-Ethereum-Improvement-Proposals-EIPs)
  - [EIP 20: ERC-20 Token Standard](#EIP-20-ERC-20-Token-Standard)
  - [EIP 721: ERC-721 Non-Fungible Token Standard](#EIP-721-ERC-721-Non-Fungible-Token-Standard)
  - [EIP 777: ERC777 Token Standard](#EIP-777-ERC777-Token-Standard)
  - [EIP 165: ERC-165 Standard Interface Detection](#EIP-165-ERC-165-Standard-Interface-Detection)
  - [EIP 1820: Pseudo-introspection Registry Contract](#EIP-1820-Pseudo-introspection-Registry-Contract)
  - [EIP 1504: ERC-1504 Upgradable Smart Contract](#EIP-1504-ERC-1504-Upgradable-Smart-Contract)
  - [EIP 1538: Transparent Contract Standard](#EIP-1538-Transparent-Contract-Standard)
  - [EIP 1822: Universal Upgradeable Proxy Standard (UUPS)](#EIP-1822-Universal-Upgradeable-Proxy-Standard-UUPS)
- :rocket: [The Ethernaut](#rocket-The-Ethernaut)

## :nerd_face: Smart Contracts

Smart contracts are programs that run on the Ethereum blockchain and read and modify its state.

### [Overview](https://solidity.readthedocs.io/en/develop/introduction-to-smart-contracts.html#)

- Ethereum smart contracts are powerful because they are bound by the rules of the Ethereum blockchain.
  - Deterministic - a smart contract must always provide the same output for any given set of inputs
  - Terminable - the execution of a smart contract must always terminate
  - Isolated - smart contracts must be kept in a “sandbox” to prevent bugs from propagating

## :blue_car: Gas

Gas is a unit that measures the amount of computational effort it will take to execute operations on the Ethereum blockchain.

### [Overview](https://solidity.readthedocs.io/en/latest/introduction-to-smart-contracts.html#gas)

- Gas is integral to ensuring Ethereum smart contracts are terminable as well as incentivizing [miners](https://ethereum-homestead.readthedocs.io/en/latest/mining.html) to participate in the blockchain.
- Every single Ethereum operation, be it a transaction or smart contract execution, requires some amount of gas.
- Miners get paid an amount in Ether, which is directly related to the total amount of gas it took them to execute Ethereum operations.
- Transaction senders specify a transaction’s **gas limit**, which is the maximum amount of gas they are willing to pay for a transaction.
  - If **the limit is not high enough** to complete the transaction’s execution, the state changes caused by the transaction are **reverted** and the **sender is charged 100% of the gas limit**.
  - If **the limit is sufficient** to complete the transaction’s execution, the state changes caused by the transaction are **persisted** and the **sender is refunded any unused gas**.
  - There is a limit on the total amount of gas a miner can accept, so transaction senders should not set artificially high gas limits.
- The sender of a transaction specifies the **gas price** (expressed in [gwei](https://ethgasstation.info/blog/gwei/)).
  - Miners are more likely to pick up transactions that have higher gas prices.
- Some operations (deletion) **refund** gas; refunds are limited to 50% of the gas used by a transaction.
- Anecdotally, [gas is extremely expensive](https://hackernoon.com/ether-purchase-power-df40a38c5a2f); Ethereum is “400 million times” more expensive than AWS.

### Further Reading

- [What is Ethereum Gas?](https://blockgeeks.com/guides/ethereum-gas/)
- [ETH Gas Station](https://ethgasstation.info/calculatorTxV.php)
- [Ethereum Gas, Fuel & Fees (Joseph Chow, YouTube)](https://www.youtube.com/watch?v=dd-ajiMl4HY)
- [How to Optimize Gas Cost](https://eattheblocks.com/how-to-optimize-gas-cost-in-a-solidity-smart-contract-6-tips/)
- [How to Reduce Gas Cost](https://medium.com/layerx/how-to-reduce-gas-cost-in-solidity-f2e5321e0395)
- [Tips & Tricks to Save Gas](https://blog.polymath.network/solidity-tips-and-tricks-to-save-gas-and-reduce-bytecode-size-c44580b218e6)

## :floppy_disk: [Storage, Memory & the Stack](https://solidity.readthedocs.io/en/develop/introduction-to-smart-contracts.html#storage-memory-and-the-stack)

_Ethereum smart contracts have three areas where they can store data - storage, memory and the stack._

### Storage

- **Persistent** between function calls and transactions
- Comparatively **high gas cost** to read, and even higher to initialize and modify

### [Memory](https://github.com/ethereum/wiki/wiki/Subtleties#memory)

- Contract obtains a freshly cleared instance for each message call
- Memory is more costly the larger it grows (resizable array); scales quadratically

### The Stack

- Runtime state; the program stack
- Access to the stack is limited to copying one of the topmost 16 elements to the top of the stack or swapping the topmost element with one of the 16 elements below it

### Further Reading

- [What is the memory keyword? What does it do? (Solidity v0.4)](https://solidity.readthedocs.io/en/v0.4.25/frequently-asked-questions.html#what-is-the-memory-keyword-what-does-it-do)
- [Ethereum Solidity: Memory vs Storage & When to Use Them](https://medium.com/coinmonks/ethereum-solidity-memory-vs-storage-which-to-use-in-local-functions-72b593c3703a)
- [Data Location (Solidity Documentation)](https://solidity.readthedocs.io/en/latest/types.html#data-location)

## :computer: The Ethereum Virtual Machine (EVM)

_The EVM is the [runtime environment](https://en.wikipedia.org/wiki/Runtime_system) for Ethereum smart contracts. It is a simple yet powerful Turing Complete 256-bit [virtual machine](https://en.wikipedia.org/wiki/Virtual_machine#Process_virtual_machines)._

### [Overview](https://solidity.readthedocs.io/en/latest/introduction-to-smart-contracts.html#index-6)

- Turing Complete - a system that can be mathematically proven to have the capability of performing any possible calculation or computer program
- In order to interact with the EVM, users must provide [EVM bytecode](https://github.com/crytic/evm-opcodes).
  - High-level languages like [Solidity](#Solidity) are compiled to EVM bytecode.
- The EVM is a transaction-based state machine; transactions are submitted and validated before the state is updated.
- The EVM manages the state of a number of components.
  - Accounts - user accounts or smart contracts
  - Blocks - information like the hash of the last block
  - Runtime - information used to execute a transaction

### [Accounts](https://solidity.readthedocs.io/en/latest/introduction-to-smart-contracts.html#accounts)

- User (“externally-owned”) accounts are associated with private keys.
- Contract accounts are associated with the code that defines them.
- The EVM has [message-passing capabilities](#Application-Binary-Interface-ABI) that allow accounts to interact.
  - User accounts can send messages by signing them with their private key.
  - Passing a message to a contract account will execute the contract’s code and allow it to manipulate its storage.
- All Ethereum accounts are comprised of four elements.
  - Nonce - a number that increases with account activity
    - User - the number of transactions sent from the account address
    - Contract - the number of contracts created by the account
  - Balance - the amount of [wei](https://ethgasstation.info/blog/gwei/) controlled by the account
  - Hash - 160-bit account address
    - User - calculated with public/private key pair
    - Contract - calculated with creator address and nonce
  - [Storage](#Storage) - a mapping of 256-bit words to 256-bit words

### Further Reading

- [Ethereum Yellow Paper](http://paper.gavwood.com/)
- [Ethereum Virtual Machine Explained](https://www.mycryptopedia.com/ethereum-virtual-machine-explained/)
- [EVM Design Rationale](https://github.com/ethereum/wiki/wiki/Design-Rationale#virtual-machine)
- [EVM State Machine (Ethereum Development Tutorial)](https://github.com/ethereum/wiki/wiki/Ethereum-Development-Tutorial#state-machine)

## Application Binary Interface (ABI)

The Ethereum ABI is the standard way to interact with EVM smart contracts, both from outside the blockchain and for contract-to-contract interaction.

### [Overview](https://solidity.readthedocs.io/en/latest/abi-spec.html#)

- A formal specification for encoding smart contract function selectors and arguments.
- There is a JSON format for describing a contract’s interface.

## Solidity

_Solidity is an object-oriented, high-level language for implementing smart contracts. Solidity was influenced by C++, Python and JavaScript, and is designed to target the EVM._

### Overview

- Solidity contracts are similar to classes and can declare and define a number of types.
  - [State variables](https://solidity.readthedocs.io/en/latest/structure-of-a-contract.html#state-variables) - values stored in the contract’s persistent storage
  - [Functions](#Functions) - executable units of code
  - [Function modifiers](https://solidity.readthedocs.io/en/latest/structure-of-a-contract.html#function-modifiers) - declarative mechanisms for amending function semantics
  - [Events](#Events) - convenience interfaces to EVM logging capabilities
  - [Structs](https://solidity.readthedocs.io/en/latest/structure-of-a-contract.html#struct-types) - custom types that can be used to group related values
  - [Enums](https://solidity.readthedocs.io/en/latest/structure-of-a-contract.html#enum-types) - custom types that define a constant set of values
- Solidity supports a number of common language features.
  - Common [expressions and control structures](https://solidity.readthedocs.io/en/latest/control-structures.html)
  - [Inheritance and polymorphism](https://solidity.readthedocs.io/en/latest/contracts.html#inheritance)
  - [Abstract](https://solidity.readthedocs.io/en/latest/contracts.html#abstract-contracts) and [interface](https://solidity.readthedocs.io/en/latest/contracts.html#interfaces) contracts
  - [Libraries](https://solidity.readthedocs.io/en/latest/contracts.html#libraries)
- Solidity also has many [Ethereum-specific features](https://solidity.readthedocs.io/en/latest/units-and-global-variables.html).
- Solidity developers must take [a number of unique security considerations](https://solidity.readthedocs.io/en/latest/security-considerations.html) into account.

### [Data Types](https://solidity.readthedocs.io/en/latest/types.html)

- No concept of `null` or `undefined` in Solidity; variables are initialized with [default values](https://solidity.readthedocs.io/en/latest/control-structures.html#default-value)
- Value types - passed by value (copied) when used in function arguments or assignments
  - [Booleans](https://solidity.readthedocs.io/en/latest/types.html#booleans) - 8-bit values representing `true` or `false`
  - [Integers](https://solidity.readthedocs.io/en/latest/types.html#integers) - signed and unsigned integers in various sizes (8- to 256-bits)
    - There are a number of [operators](https://solidity.readthedocs.io/en/latest/types.html#integers) that apply to integers.
  - [Addresses](https://solidity.readthedocs.io/en/latest/types.html#address) - payable and non-payable 160-bit Ethereum addresses
  - [Contracts](https://solidity.readthedocs.io/en/latest/types.html#contract-types) - 160-bit pointers to contract code
  - [Fixed-Size Byte Arrays](https://solidity.readthedocs.io/en/latest/types.html#fixed-size-byte-arrays) - sequences of bytes in various sizes (1 to 32)
  - [Address Literals](https://solidity.readthedocs.io/en/latest/types.html#address-literals) - hexadecimal literals that pass the address checksum test
  - [Rational and Integer Literals](https://solidity.readthedocs.io/en/latest/types.html#rational-and-integer-literals) - number literals
  - [String Literals](https://solidity.readthedocs.io/en/latest/types.html#string-literals-and-types) - non-null-terminated arrays of bytes
  - [Hexadecimal Literals](https://solidity.readthedocs.io/en/latest/types.html#hexadecimal-literals) - behave like string literals
  - [Enums](https://solidity.readthedocs.io/en/latest/types.html#enums) - integer values representing user-defined constants
  - [Functions](https://solidity.readthedocs.io/en/latest/types.html#function-types) - used to pass functions as arguments and use them as return values
- Reference types - passed by reference when used in function arguments or assignments
  - Changes will propagate to the original since no copy is made
  - Reference types are annotated with a data location
    - [Memory](#Memory)
    - [Storage](#Storage)
    - Calldata - non-modifiable, non-persistent
  - [Structs](https://solidity.readthedocs.io/en/latest/types.html#structs) - custom types that can be used to group related values
  - [Arrays](https://solidity.readthedocs.io/en/latest/types.html#arrays) - collections with compile-time fixed size or dynamic size
    - The types `bytes` and `string` are special arrays
  - [Mappings](https://solidity.readthedocs.io/en/latest/types.html#mapping-types) - associate elementary type values with other values; like hash tables

### [Functions](https://solidity.readthedocs.io/en/latest/contracts.html#functions)

- Solidity functions can [return multiple values](https://solidity.readthedocs.io/en/latest/control-structures.html#destructuring-assignments-and-returning-multiple-values).
- Solidity functions can be modified with keywords.
  - `public` - can be called either internally or externally
  - `internal` - can only be called from within the contract or its descendants
  - `private` - can only be called from within the contract
  - `external` - cannot be called from within the contract
  - `view` - will not modify the state of the contract
  - `pure` - will not read or modify the state of the contract
  - `payable` - will accept any Ether sent to it
- Each contract can have one [fallback function](https://solidity.readthedocs.io/en/latest/contracts.html#fallback-function).
  - Cannot take any arguments
  - Must be marked `external`
  - Executed if the contract is invoked without a function selector (as in a transfer of Ether) or if the function selector is invalid
  - Must be marked `payable` if the contract is to be able to receive Ether through regular transactions

### [Events](https://solidity.readthedocs.io/en/latest/contracts.html#events)

- Event types are defined in contracts and are inheritable.
- Event instances are stored on the blockchain in transaction logs.
- External applications can listen to contract events via the RPC log interface of an Ethereum client.
  - Log data is not accessible from within contracts.
- Events can have up to three indexed attributes, which will be easily accessible via logs.
  - Attributes that are not indexed will be [ABI-encoded](#Application-Binary-Interface-ABI) into the log data.
- A hash of the event’s signature will be written to the logs unless the event is anonymous.
  - Anonymous events are cheaper to deploy and call but cannot be filtered.
- Log subscribers can filter by contract, event signature or indexed attributes.

### Further Reading

- [Solidity by Example](https://solidity.readthedocs.io/en/latest/solidity-by-example.html)
- [Solidity Style Guide](https://solidity.readthedocs.io/en/latest/style-guide.html)
- [Common Patterns in Solidity](https://solidity.readthedocs.io/en/latest/common-patterns.html)

## :cd: Remix Integrated Development Environment (IDE)

[Remix](https://remix.ethereum.org/) is a powerful, open-source web IDE that allows users to write, debug, deploy and interact with Ethereum smart contracts straight from the browser.

### [Overview](https://remix-ide.readthedocs.io/en/latest/index.html)

- The Remix [text editor](https://remix-ide.readthedocs.io/en/latest/solidity_editor.html) provides in-line support for Solidity development.
- There is a [terminal](https://remix-ide.readthedocs.io/en/latest/terminal.html) that displays messages and exposes a command-line interface.
- Remix is comprised of a number of modules that are accessible as discrete portals.
  - [File explorer](https://remix-ide.readthedocs.io/en/latest/file_explorer.html) - view and manage files in the browser and [local file system](https://remix-ide.readthedocs.io/en/latest/remixd.html)
  - [Solidity compiler](https://remix-ide.readthedocs.io/en/latest/compile.html) - compile Solidity contracts and manage compiler settings
  - [Deployer and runner](#Deploy--Run) - deploy and run contracts in a number of environments
  - [Debugger](#Debugger) - debug Ethereum transactions
- There are a number of community-provided modules that provide capabilities like [static analysis](https://remix-ide.readthedocs.io/en/latest/static_analysis.html), [gas profiling](https://github.com/EdsonAlcala/remix-gas-profiler) and [unit testing](https://remix-ide.readthedocs.io/en/latest/unittesting.html).

### [Deploy & Run](https://remix-ide.readthedocs.io/en/latest/run.html)

- Remix supports a number of environments to deploy and run smart contracts.
  - Javascript VM - Remix provides and can connect to a sandboxed, ephemeral, in-browser blockchain for development purposes
  - Injected provider - Remix can use injected capabilities from a provider like [Metamask](https://metamask.io/) or the integrated wallet of [the Brave web browser](https://brave.com/)
  - Web3 provider - Remix can connect directly to Ethereum nodes, like [Parity](https://www.parity.io/ethereum/) (for production) or [Ganache](https://www.trufflesuite.com/ganache) (for development)
- Remix can [instantiate compiled contracts](https://remix-ide.readthedocs.io/en/latest/run.html#initiate-instance) by deploying them to the selected environment or by accessing a previously-deployed contract at a user-provided address.
- Once a contract has been instantiated, Remix exposes [a friendly UI to interact with it](https://remix-ide.readthedocs.io/en/latest/udapp.html).
- Users can [save and reuse contract interactions](https://remix-ide.readthedocs.io/en/latest/run.html#using-the-recorder).
- This module exposes [a number of settings related to sending transactions](https://remix-ide.readthedocs.io/en/latest/run.html#run-setup), like the Ethereum account to use, the gas limit for a transaction and the amount of Ether to send with a transaction.

### [Debugger](https://remix-ide.readthedocs.io/en/latest/debugger.html)

- The Remix debugger only works when the environment selected in [Deploy & Run]((#Deploy-%26-Run)) supports it. The Javascript VM supports the Remix debugger.
- Debug a transaction by [providing its hash](https://remix-ide.readthedocs.io/en/latest/tutorial_debug.html#initiate-debugging-from-the-debugger) or clicking [the “Debug” button that appears in the terminal after Remix has been used to make a transaction](https://remix-ide.readthedocs.io/en/latest/tutorial_debug.html#initiate-debugging-from-the-transaction-log-in-the-terminal).
- Once the debugger has been activated, it exposes [a similar interface to other debuggers](https://remix-ide.readthedocs.io/en/latest/tutorial_debug.html#using-the-debugger).

### Further Reading

- [Creating & Deploying a Contract](https://remix-ide.readthedocs.io/en/latest/create_deploy.html)
- [Importing Source Files](https://remix-ide.readthedocs.io/en/latest/import.html)
- [Remix Tutorials](https://remix-ide.readthedocs.io/en/latest/remix_tutorials_github.html)
- [Build Artifact](https://remix-ide.readthedocs.io/en/latest/contract_metadata.html)

## `web3.js`

`web3.js` is the Javascript API for the Ethereum ecosystem.

### [Overview](https://web3js.readthedocs.io/en/v1.2.4/index.html)

- `web3.js` is a collection of libraries for interacting with Ethereum nodes and related services.
  - [`web3.eth`](https://web3js.readthedocs.io/en/v1.2.4/web3-eth.html) - tools for interacting with the Ethereum blockchain and Ethereum smart contracts
  - [`web3.eth.subscribe`](https://web3js.readthedocs.io/en/v1.2.4/web3-eth-subscribe.html) - tools for subscribing to Ethereum [events](#Events)
  - [`web3.eth.Contract`](https://web3js.readthedocs.io/en/v1.2.4/web3-eth-contract.html) - an object to abstract Ethereum smart contracts and expose their capabilities
  - [`web3.eth.accounts`](https://web3js.readthedocs.io/en/v1.2.4/web3-eth-accounts.html) - functions to generate accounts and use them to sign transactions and data
  - [`web3.eth.abi`](https://web3js.readthedocs.io/en/v1.2.4/web3-eth-abi.html) - a library for [ABI](#Application-Binary-Interface-ABI) encoding and decoding
  - [`web3.utils`](https://web3js.readthedocs.io/en/v1.2.4/web3-utils.html) - common utilities for Ethereum development, like hexadecimal encoding and decoding

## :mushroom: Truffle

Truffle is framework for developing Ethereum smart contract projects.

### [Overview](https://www.trufflesuite.com/truffle)

- Truffle is a Javascript framework and is [available via NPM](https://www.trufflesuite.com/docs/truffle/getting-started/installation).
- Truffle [projects](https://www.trufflesuite.com/docs/truffle/getting-started/creating-a-project) are comprised of a number of components.
  - [Contracts](https://www.trufflesuite.com/docs/truffle/getting-started/compiling-contracts) - Solidity smart contracts
  - [Migrations](https://www.trufflesuite.com/docs/truffle/getting-started/running-migrations) - Javascript files that are used to deploy contracts
  - [Scripts](https://www.trufflesuite.com/docs/truffle/getting-started/writing-external-scripts) - Javascript files that used to interact with deployed contracts
  - [Tests](https://www.trufflesuite.com/docs/truffle/testing/testing-your-contracts) - Solidity unit tests and Javascript integration tests
- The Truffle framework includes [a CLI for interacting with Truffle projects](https://www.trufflesuite.com/docs/truffle/getting-started/using-truffle-develop-and-the-console).
- Truffle includes [an integrated debugger](https://www.trufflesuite.com/docs/truffle/getting-started/debugging-your-contracts).

### Further Reading

- [`@truffle/contract`](https://github.com/trufflesuite/truffle/tree/master/packages/contract)

## OpenZeppelin

OpenZeppelin provides tools to write, deploy and operate decentralized applications on the Ethereum blockchain.

### [Overview](https://openzeppelin.com/)

- OpenZeppelin maintains a number of products for Ethereum development.
  - [Contracts](https://openzeppelin.com/contracts/) - an open-source collection of commonly reusable contracts
  - [SDK](https://openzeppelin.com/sdk/) - a toolkit for compiling, upgrading, deploying, and interacting with smart contracts
  - [Starter Kits](https://openzeppelin.com/starter-kits/) - templates for scaffolding dApps

## :bulb: Ethereum Improvement Proposals (EIPs)

_Ethereum Improvement Proposals (EIPs) describe standards for the Ethereum platform, including core protocol specifications, client APIs, and contract standards._

### [Overview](https://eips.ethereum.org/)

- The EIP program is described in [EIP-1](https://eips.ethereum.org/EIPS/eip-1).
- EIPs are divided into types (and in some cases, sub-types).
  - Standard Track - improvements that affect Ethereum implementations or the interoperability of applications using Ethereum
    - Core - improvements related to “core dev” discussions including those requiring a consensus fork
    - Networking - improvements to networking protocols
    - Interface - improvements such as those to API/RPC specification or language-level standards
    - ERC (Ethereum Request for Comment) - application-level standards and conventions
  - Meta - official EIPs that do not relate to the Ethereum protocol or codebase
  - Informational - used to provide general information, like Ethereum design issues
  - There is an EIP life cycle comprised of multiple stages.
    - Draft - open EIPs undergoing rapid change
    - Last Call - a “finished” EIP that is ready for wide review
    - Accepted - an EIP that has been in the Last Call stage for at least two weeks and whose author has addressed any requested technical changes
      - For non-core EIPs this is the Final stage
    - Final - core EIPs that the core devs have decided will be (or already have been) released in a hard fork

### [EIP 20: ERC-20 Token Standard](https://eips.ethereum.org/EIPS/eip-20)

- An interface that describes basic functionality to transfer tokens as well as approve and facilitate transfers initiated by third-parties
- Example: [OpenZeppelin ERC20.sol](https://github.com/OpenZeppelin/openzeppelin-contracts/blob/master/contracts/token/ERC20/ERC20.sol)
- The ERC-20 standard defines several [log event types](#Events).
  - `Transfer(address indexed, address indexed, uint256)` - log a transfer of a specified token amount from one account to another
    - Token contracts should log a `Transfer` event from account `0x0` when tokens are created.
  - `Approval(address indexed, address indexed, uint256)` - log an approval for one account to spend tokens from another account, up to some specified maximum value
- The ERC-20 standard defines several **required** functions.
  - `totalSupply() public view returns (uint256)` - returns the total token supply
  - `balanceOf(address) public view returns (uint256)` - returns the token balance of the supplied account
  - `transfer(address, uint256) public returns (bool)` - transfers the specified amount of the token to the provided account
    - Must fire a `Transfer` event (even if amount is `0`)
    - Should raise an error if sender’s balance is insufficient
  - `transferFrom(address, address, uint256) public returns (bool)` - transfers the specified amount of the token from one account to another
    - Should raise an error if the account to send funds from has not provided authorization to the caller of this function
  - `approve(address, uint256) public returns (bool)` - approve another account to withdraw tokens from your account, up to some provided maximum value
    - Must fire an `Approval` event
  - `allowance(address, address) public view returns (uint256)` - returns the amount of the token that one account is approved to withdraw on behalf of another
- The ERC-20 standard defines several **optional** functions.
  - `name() public view returns (string)` - returns the name of the token
  - `symbol() public view returns (string)` - returns the symbol of the token
  - `decimals() public view returns (uint8)` - returns the maximum decimal precision supported by the token

#### [EIP 223: ERC223 Token Standard](https://github.com/ethereum/EIPs/issues/223)

- This proposal, which was superseded by [EIP 777](#EIP-777-ERC777-Token-Standard), is motivated by one of the limitations of the EIP 20 specification.
  - The ERC-20 standard defines different semantics for transferring tokens to user and contract accounts.
  - To transfer ERC-20 tokens to a user account, the `transfer` function is used.
  - To transfer ERC-20 tokens to a contract account, a combination of the `approve` and `transferFrom` functions are used in order to approve the contract to transfer tokens on behalf of a user account.
  - If the `transfer` function is incorrectly used to send ERC-20 tokens to a contract account, those tokens may be "lost" forever because the contract may not have any capabilities to handle the tokens.
  - This is because, unlike a transfer of Ether, which invokes a smart contract function (the `fallback` function), transfers of ERC-20 tokens do not actually involve the receiver in any way.
  - Anecdotally, this limitation to the ERC-20 specification has resulted in hundreds-of-thousands in US dollars of ERC-20 tokens being rendered inaccessible.
- In order to receive ERC-223 tokens, smart contract accounts must implement a function that is specified by this EIP.
  - `tokenFallback(address, uint256, bytes)` - meant to emulate the `fallback` function that is used for Ether transfers
  - When the `tokenFallback` function is invoked, `msg.sender` should refer to the address of the ERC-223 token.
  - The function's `address` parameter specifies the sender of the transfer and the `uint` parameter specifies the amount of the token to be transferred.
  - The final `bytes` parameter is optional and can be used to include data with a transfer, although this will cost additional gas.
- EIP 223 is backwards-compatible with EIP 20.
- ERC-223 token contracts must also implement a `transfer(address, uint, bytes)` function.
  - This function must always be called as part of a token transfer.
  - If the receiver address belongs to a contract account, this function must invoke the `tokenFallback` function on the receiver. If the `tokenFallback` function does not exist at the receiver address, then the transfer must fail.
  - The EIP provides a recommended way to check if an address belongs to a contract account.
- Example: [ERC-223 Reference Implementation](https://github.com/Dexaran/ERC223-token-standard/blob/development/token/ERC223/ERC223.sol)

### [EIP 721: ERC-721 Non-Fungible Token Standard](https://eips.ethereum.org/EIPS/eip-721)

- The [ERC-20](#EIP-20-ERC-20-Token-Standard) standard defines an interface for _fungible_ tokens. An asset is referred to as "fungible" if all instances of the asset are equal, like a one-dollar bill.
  - For fungible assets, it does not matter _which_ instances of that asset someone controls; what matters is _how much_ of that asset they control.
- An example of a non-fungible asset is a property deed; although all instances of a given property deed type are "the same", the difference in value between two deeds may be enormous.
  - For non-fungible assets, it not only matters _how many_ someone controls, but also _which ones_.
- Instances of a non-fungible token (NFT) are _distinguishable_ from one another and the ownership of each one must be tracked separately.
- EIP 721 specifies an interface for NFTs that builds on the ERC-20 token standard.
  - In addition to providing capabilities to track individual tokens, the standard is designed to allow applications to efficiently handle large quantities of NFTs.
- The ERC-721 standard specifies the use of a `uint256` unique identifier for NFTs.
- Although EIP 721 requires [EIP 165](#EIP-165-ERC-165-Standard-Interface-Detection), this requirement comes with a caveat and is not included in any of the proposal's reference implementations. The authors of EIP 721 explicitly state their support for an interface registry, such as that defined by [EIP 1820](#EIP-1820-Pseudo-introspection-Registry-Contract).
- Example: [ERC-721 Reference Implementation](https://github.com/0xcert/ethereum-erc721/blob/master/src/contracts/tokens/nf-token.sol)

#### Receiver Function

- Similar to [EIP 223](#EIP-223-ERC223-Token-Standard), EIP 721 specifies that contract accounts must implement a function in order to receive _safe_ transfers of ERC-721 tokens.
- `onERC721Received(address, address, uint256, bytes) external returns(bytes4)` - handles the receipt of an NFT
  - This interface specifies the account initiating the transfer as well as the account that previously owned the NFT, since these two values may not be equal.
  - When this function is invoked, `msg.sender` is always the address of the ERC-721 contract.
  - This function may throw an error in order for the contract to reject a transfer.
  - This function must return `bytes4(keccak256("onERC721Received(address,address,uint256,bytes)"))` or else the transfer will fail.
- Note that contract accounts can still receive transfers of ERC-721 tokens if the `transferFrom` function is used (see below).

#### [Log Events](#Events)

- `Approval(address indexed, address indexed, uint256 indexed)` - logs that the account authorized to manage this NFT on behalf of its owner has been changed or reaffirmed
  - A zero-valued address indicates that no account is authorized to manage the NFT on behalf of its owner.
- `ApprovalForAll(address indexed, address indexed, bool)` - logs that permission to manage all NFTs for a specified account has been enabled or disabled for another account
- `Transfer(address indexed, address indexed, uint256 indexed)` - log a transfer in ownership of a specified NFT from one account to another
  - This event _may_ be emitted when NFTs are created (transfers _from_ account `0x0`) and _must_ be emitted when NFTs are destroyed (transfers _to_ account `0x0`).
  - A `Transfer` event implies that there are no longer any accounts authorized to manage the specified NFT on behalf of its owner.

#### Functions

- `balanceOf(address) external view returns (uint256)` - returns the amount of an ERC-721 token owned by an account
  - If the address parameter is `0x0`, this function will fail.
- `ownerOf(uint256) external view returns (address)` - returns the account that owns a specified NFT
  - If the ID is that of an NFT that has been destroyed (owned by account `0x0`), this function will fail.
- `setApprovalForAll(address, bool) external` - enables or disables permission for an account to manage all NFTs on behalf of the account corresponding to `msg.sender`
  - This function must fire an `ApprovalForAll` event.
  - An account that has been authorized to manage all NFTs for another account is known as an "operator" of that account.
  - EIP 721 specifies that ERC-721 contracts must support multiple operators per account.
- `isApprovedForAll(address, address) external view returns (bool)` - returns whether or not one account is an authorized operator for another
- `approve(address, uint256) external payable` - change or reaffirm the account that is authorized to manage an NFT on behalf of its owner
  - If `msg.sender` is not equal to the current owner of the NFT or one of their authorized operators, this function will fail.
  - This function must log an `Approval` event.
- `getApproved(uint256) external view returns (address)` - returns the account that is approved to manage the specified NFT on behalf of its owner
  - This function will fail if the token ID is invalid.
- `safeTransferFrom(address, address, uint256, bytes) external payable` - transfers the ownership of a specified NFT from one account to another
  - This function will fail in the following scenarios:
    - `msg.sender` is not equal to the current owner of the NFT, the account specified as the "approved" account for this NFT or an authorized operator account for the NFT's owner
    - The parameter that specifies the account _sending_ the transfer is not equal to the account that owns the NFT
    - The parameter that specifies the account _receiving_ the transfer is `0x0`
    - The token ID is invalid
  - This function will check if the receiving account is a contract account after it has completed the transfer and, if so, will call the `onERC721Received` function on that contract.
    - If `onERC721Received` does not return `bytes4(keccak256("onERC721Received(address,uint256,bytes)"))` this function will fail.
  - This function must log a `Transfer` event.
- `safeTransferFrom(address, address, uint256) external payable` - an alias for the other `safeTransferFrom` function that sets the final parameter to `""`
- `transferFrom(address, address, uint256) external payable` - similar to `safeTransferFrom(address, address, uint256)` but without invoking `onERC721Received` on receivers that are contracts

#### Optional Metadata Extension

- This extension to EIP 721 allows ERC-721 contracts to be interrogated for details about the token type as well as for details about individual tokens.
- The EIP 721 Metadata Extension defines several functions.
  - `name() external view returns (string)` - returns a descriptive name for the collection of NFTs defined by the contract
  - `symbol() external view returns (string)` - returns an abbreviated name for the collection of NFTs defined by the contract
  - `tokenURI(uint256 _tokenId) external view returns (string)` - returns a distinct [Uniform Resource Identifier (URI)](https://wikipedia.org/wiki/Uniform_Resource_Identifier) for the specified NFT
    - If the token ID is invalid this function will fail.
    - The URI for an NFT may change from time-to-time.
    - The return value of this function is not usable on-chain, but the EIP authors justify that decision in the proposal.
- This extension defines a JSON schema, which may be the target of URIs returned by the `tokenURI` function.
- Example: [Reference Implementation](https://github.com/0xcert/ethereum-erc721/blob/master/src/contracts/tokens/nf-token-metadata.sol)

#### Optional Enumeration Extension

- This extension to EIP 721 allows ERC-721 contracts to publish their full collection of NFTs and make them discoverable.
- The EIP 721 Enumeration Extension defines several functions.
  - `totalSupply() external view returns (uint256)` - returns the count of valid NFTs tracked by the contract
    - A valid NFT is one with an assigned and queryable owner that is not equal to `0x0`.
  - `tokenByIndex(uint256) external view returns (uint256)` - returns the ID of the NFT at the specified index
    - If the specified index is greater than or equal to the value returned by `totalSupply`, this function will fail.
    - Note that this extension to the EIP 721 standard does _not_ specify the sort order which is to be used for NFTs.
  - `tokenOfOwnerByIndex(address, uint256) external view returns (uint256)` - returns the ID of the NFT at the specified index for the specified owner
    - If the specified index is greater than or equal to the value returned by `balanceOf(address)`, this function will fail.
    - If the specified owner account is `0x0`, this function will fail.
- Example: [Reference Implementation](https://github.com/0xcert/ethereum-erc721/blob/master/src/contracts/tokens/nf-token-enumerable.sol)

### [EIP 777: ERC777 Token Standard](https://eips.ethereum.org/EIPS/eip-777)

- This proposal defines a new way to interact with a non-fungible token contract while remaining backwards-compatible with [EIP 20](#EIP-20-ERC-20-Token-Standard).
  - This EIP states that ERC-20 functions should only be called from old contracts.
  - Applications must take care to not double-count token movement based on the ERC-20 and ERC777 log events of tokens that implement both standards.
  - Applications should consider tokens _either_ ERC777 _or_ ERC-20 but not _both_.
- EIP 777 improves on EIP 20 by introducing _operators_ and _hooks_.
  - These enhancements address the same issue as [EIP 223](#EIP-223-ERC223-Token-Standard), and this EIP supersedes the former EIP.
  - **Operators** are accounts that are authorized to manage tokens on behalf of another account.
    - Accounts may specify multiple operators.
    - ERC777 contracts may specify default operators.
      - If an ERC777 token specifies default operators, this value must be set when the token contract is created and may not ever be modified.
    - Account owners may authorize, revoke and reauthorize operators (including default operators).
    - Accounts are always considered operators for themselves and this cannot be revoked.
  - **Hooks** are function interfaces that are intended to offer token holders more control over their tokens.
- This EIP requires [EIP 1820](#EIP-1820-Pseudo-introspection-Registry-Contract).
  - ERC777 contracts must use the EIP 1820 registry to register the `ERC777Token` interface with their own address.
  - If an ERC777 contract has a switch to enable and disable the capabilities described by EIP 777, it must update the EIP 1820 registry accordingly.
  - If an ERC777 contract implements the ERC-20 standard, it must use the EIP 1820 registry to register the `ERC20Token` interface with its own address.
  - If an ERC777 contract has a switch to enable and disable its ERC-20 capabilities, it must update the EIP 1820 registry accordingly.
- The design for this standard takes several considerations into account.
  - Token Life Cycle - a clearly-defined life cycle is important for consistency and accuracy, especially when token value is derived from scarcity
    - The ERC777 standard includes consideration for the minting, managing and destruction (burning) of tokens.
  - Data - allow token transfers to include a data component like the transfer of Ether
  - Hooks - offer token holders more control over their tokens
  - EIP 1820 Integration - EIP 1820 was, in fact, written to support this EIP
  - Operators - an idiomatic way to allow one account to control the tokens of another
- The ERC777 standard requires that tokens support 18-points of decimal precision.
  - The ERC-20 `decimals` function is **optional** for ERC777 tokens, since the value returned by this function will be `18` for **all** ERC777 tokens.
- EIP 777 specifies functions for managing account operators as well as sending and burning tokens. ERC777 tokens may expose other functions to address these features.
- This EIP specifies that dApps and wallets should first estimate the gas required when sending, minting, or burning tokens by using [`eth_estimateGas`](https://github.com/ethereum/wiki/wiki/JSON-RPC#eth_estimategas) to avoid running out of gas during the transaction.
- This EIP includes a logo that may be used to promote valid implementations of the ERC777 standard.
- One of the authors of this EIP submitted a more detailed version of this standard as his [master thesis](https://github.com/0xjac/master-thesis).
- Example: [OpenZeppelin ERC777](https://github.com/OpenZeppelin/openzeppelin-contracts/blob/master/contracts/token/ERC777/ERC777.sol)

#### Hooks

- `tokensToSend(address, address, address, uint256, bytes, bytes) external` - notifies the receiver of a request to decrease the balance of their account
  - This function is specified by the `ERC777TokensSender` interface.
  - This function specifies three addresses: `from`, `operator` and `to`.
    - If the `to` account is `0x0` then the tokens are being burned.
  - The final two parameters are for `data` and `operatorData`, respectively.
  - The value of `msg.spender` will be the address of an ERC777 token contract.
    - This function may be called from multiple token contracts.
  - This hook is optional for both user and contract accounts.
  - Accounts that wish to be notified of token debits may register the address of a contract implementing the `ERC777TokensSender` interface with the EIP 1820 registry.
    - If an account has registered an implementation of this function, the token contract must call it when sending or burning tokens from the account.
    - An account may block a send or burn of tokens by throwing an error in this hook.
  - This function will be called _before_ updating the state of the token contract, i.e. before the account balance of the receiver has been decreased.
  - This function, if specified, must be called when ERC777 tokens are sent by way of the ERC-20 `transfer` or `transferFrom` functions.
  - This function must not be called outside of a burn or send (or ERC-20 transfer) process.
  - One implementation of this function may be shared by multiple accounts.
- `tokensReceived(address, address, address, uint256, bytes, bytes) external` - notifies the receiver of an increase to the balance of their account
  - This function is specified by the `ERC777TokensRecipient` interface.
  - This function specifies three addresses: `from`, `operator` and `to`.
    - If the `from` account is `0x0` then the tokens are being minted.
  - The final two parameters are for `data` and `operatorData`, respectively.
  - The value of `msg.spender` will be the address of an ERC777 token contract.
    - This function may be called from multiple token contracts.
  - Contract accounts must implement this function if they wish to receive ERC777 tokens when they are minted or transferred using the `send` function.
    - Contract accounts that do not implement this function may still receive ERC777 tokens by other means (e.g. ERC-20 `transfer`).
  - Accounts that wish to be notified of token transfers may register the address of a contract implementing the `ERC777TokensRecipient` interface with the EIP 1820 registry.
    - If an account has registered an implementation of this function, the token contract must call it when sending or minting tokens to the account.
    - An account may block a send or mint of tokens by throwing an error in this hook.
  - This function will be called _after_ updating the state of the token contract, i.e. after the account balance of the receiver has been increased.
  - This function, if specified,  must be called when ERC777 tokens are sent by way of the ERC-20 `transfer` or `transferFrom` functions.
  - This function must not be called outside of a mint or send (or ERC-20 transfer) process.
  - One implementation of this function may be shared by multiple accounts.

#### [Log Events](#Events)

- `Sent(address indexed, address indexed, address indexed, uint256, bytes, bytes)` - logs that tokens have been sent from one account to another
  - This event specifies three addresses: `from`, `operator` and `to`.
  - This event must not be logged outside of a send (or ERC-20 transfer) process.
  - ERC777 tokens must log `Sent` events from the ERC-20 `transfer` and `transferFrom` functions.
  - A single operation on the token contract may result in the logging of multiple `Sent` events.
    - An example use case that involves logging multiple `Sent` events is a token contract that applies a fee when tokens are sent; in this case, there must be a `Sent` event for the recipient of the transfer as well as one for the recipient of the transfer fee.
    - A `Sent` event must be logged for each `from`-`to` pair with a value that is equal to amount for that pair.
    - The sum of the values specified by all `Sent` events must be equal to the total amount sent.
- `Minted(address indexed, address indexed, uint256, bytes, bytes)` - logs that tokens have been minted and sent from an account by an operator account
  - This event must not be logged outside of the minting process.
  - A single operation on the token contract may result in the logging of multiple `Minted` events.
    - A `Minted` event must be logged for each token recipient.
    - The sum of the values specified by all `Minted` events must be equal to the total amount minted.
- `Burned(address indexed, address indexed, uint256, bytes, bytes)` - logs that tokens have been burned from an account by an operator account
  - A single operation on the token contract may result in the logging of multiple `Burned` events.
    - A `Burned` event must be logged for each token holder.
    - The sum of the values specified by all `Burned` events must be equal to the total amount burned.
- `AuthorizedOperator(address indexed, address indexed)` - logs that one account has been authorized as an operator for another account
  - This event will not be logged if default operators are set when an ERC777 contract is created.
  - This event must not be logged outside of the operator authorization process.
- `RevokedOperator(address indexed, address indexed)` - logs that one account has had its authorization as operator for another account revoked
  - This event must not be logged outside of the operator revocation process.

#### Functions

- `name() external view returns (string)` - returns the name of the token
- `symbol() external view returns (string)` - returns the symbol of the token
- `totalSupply() external view returns (uint256)` - returns the total number of tokens in existence
  - The value returned by this function must equal the sum of the balances of all accounts, which must be equal to the sum of all minted tokens minus the sum of all burned tokens.
- `balanceOf(address) external view returns (uint256)` - returns the amount of the token owned by an account
- `granularity() external view returns (uint256)` - returns the maximum granularity supported by the token
  - The granularity is a constant, positive number that is set when an ERC777 contract is constructed
  - Tokens must always be minted and burned in amounts that are multiples of the granularity.
  - Any operation that would result in an account having a token balance that is not a multiple of the granularity must result in error.
  - This EIP specifies that tokens should use a granularity of `1` unless there is a compelling reason otherwise.
- `defaultOperators() external view returns (address[])` - returns the list of default operators; a token contract with no default operators will return an empty list
- `isOperatorFor(address, address) external view returns (bool)` - returns whether one account is an operator for another
  - In order for an application to determine all authorized operators for an account, it must call this function once for each default operator **and** parse all `AuthorizedOperator` and `RevokedOperator`events for the account.
- `authorizeOperator(address) external` - authorize an account as an operator for the account specified by `msg.sender`
  - If the address parameter is equal to the value of `msg.sender`, this function will fail.
  - This function may be used to reaffirm the authorization of an operator for the account of the message sender.
  - This function must log an `AuthorizedOperator` event.
- `revokeOperator(address) external` - revoke authorization from an account to act as operator for the account specified by `msg.sender`
  - If the address parameter is equal to the value of `msg.sender`, this function will fail.
  - This function may be used to reaffirm that an account is not authorized as an operator for the account of the message sender.
  - This function must log an `RevokedOperator` event.
- `send(address, uint256, bytes) external` - sends a specified amount of tokens from the account specified by `msg.sender` to another account
  - Calling this function will cause the balance of the sender's account to _decrease_ by the specified amount and the balance of the recipient's account to _increase_ by the same amount.
  - The semantics of this function are meant to emulate a `fallback` function and the final parameter can be used to include data with a token transfer.
  - If the sender has specified a `tokensToSend` hook per the EIP 1820 registry, this function must invoke it _before_ updating the state of the token contract.
  - If the receiver has specified a `tokensReceived` hook per the EIP 1820 registry, this function must invoke it _after_ updating the state of the token contract.
  - This function will fail in the following scenarios:
    - Transferring the specified amount of tokens would cause any account balance to no longer be a multiple of the token's granularity
    - The recipient of the transfer is a contract that does not specify an implementation of the `ERC777TokensRecipient` interface per the EIP 1820 registry
    - The recipient of the transfer is account `0x0`
    - Transferring the specified amount of tokens would cause the balance of the sender's account to become negative
    - Any of the hooks invoked by this function fail
  - This function must log a `Sent` event.
  - For tokens that also implement the ERC-20 standard, this function must log a `Transfer` event.
- `operatorSend(address, address, uint256, bytes, bytes) external` - sends a specified amount of tokens from one account to another
  - This function is only different from the `send` function in a few ways.
    - The account from which tokens are sent may not be the same as that specified by `msg.sender`.
    - The function signature includes an additional parameter for `operatorData`.
    - If the account specified by `msg.sender` is not an authorized operator for the account to send tokens from, this function will fail.
  - If the account specified by `msg.sender` is equal to the account to send tokens from, invoking this function must have the same effects as invoking the `send` function.
  - This EIP specifies that the value of `operatorData` must only be provided by the operator. Furthermore, it states that it is intended mostly for logging purposes.
- `burn(uint256, bytes) external` - burn a specified amount of tokens from the account specified by `msg.sender`
  - If the account from which tokens are being burned has specified a `tokensToSend` hook per the EIP 1820 registry, this function must invoke it _before_ updating the state of the token contract.
  - This function must log a `Burned` event.
  - For tokens that also implement the ERC-20 standard, this function should log a `Transfer` event.
- `operatorBurn(address, uint256, bytes, bytes) external` - burn a specified amount of tokens from a specified account
  - This function is only different from the `burn` function in a few ways.
    - The account from which tokens are burned may not be the same as that specified by `msg.sender`.
    - The function signature includes an additional parameter for `operatorData`.
    - If the account specified by `msg.sender` is not an authorized operator for the account to burn tokens from, this function will fail.
  - If the account specified by `msg.sender` is equal to the account to send tokens from, invoking this function must have the same effects as invoking the `burn` function.
  - This EIP specifies that the value of `operatorData` must only be provided by the operator.

#### Minting Tokens

- In order to support the greatest number of use cases, this EIP does not define any functions that mint tokens.
- Tokens must be minted in quantities that are multiples of the token contract's granularity.
- Tokens may be minted for any account except `0x0`.
- Minting tokens may not decrease the balance of account `0x0`.
- Minting a given amount of tokens must result in an equivalent increase in the value returned by the `totalSupply` function.
- Minting a given amount of tokens must result in an equivalent increase in the balances of the recipient accounts.
- The token contract must log one `Minted` event for each recipient account.
- Tokens that also implement the ERC-20 standard should log at least one `Transfer` event when tokens are minted.
- The token contract must call the `tokensReceived` hook of each of the recipients if they have registered an `ERC777TokensRecipient` implementation per the EIP 1820 registry.
  - If a recipient account is a contract, it must provide an `ERC777TokensRecipient` implementation.
  - If a `tokensReceived` hook fails, so will the minting function.
- The values of `data` and `operatorData` must be equal for all `tokensReceived` hooks and `Minted` log events that are part of a single mint operation.
- When an ERC777 contract is created, it must log `Minted` events and invoke `tokensReceived` hooks for all recipients of its initial supply.
- EIP 777 specifies that minting an amount of zero tokens is valid.
- Unlike sending or burning a token, tokens that are minted do not originate from a holder account, so the data sent to the `tokensReceived` hook and `Minted` event may be interpreted as originating from the token contract or operator.

### [EIP 165: ERC-165 Standard Interface Detection](https://eips.ethereum.org/EIPS/eip-165)

- A method to publish and detect what interfaces a smart contract implements by establishing several standards:
  - Interface identification
  - Publication of implemented interfaces
  - Detection of support for the ERC-165 standard
  - Detection of support for any given interface
- An interface’s identifier is calculated as the XOR (exclusive-or) of the function selectors that define the interface.
- Smart contracts that are compliant with the ERC-165 standard must implement a function that is specified in the EIP.
  - `function supportsInterface(bytes4 interfaceID) external view returns (bool);`
  - Must return `true` when called with the ID of any interface the implementing smart contract supports, including the ID of the detection function (`0x01ffc9a7`)
  - Must return `false` when called with the ID of any interface the implementing smart contract does not support, or the special value `0xffffffff`
  - Must not use more than 30,000 [gas](#Gas)
- The EIP specifies a procedure for checking if a smart contract implements this standard as well as example implementations of the `supportsInterface` function.

### [EIP 1820: Pseudo-introspection Registry Contract](https://eips.ethereum.org/EIPS/eip-1820)

- Defines a fully-decentralized universal registry smart contract where any Ethereum account can register which interfaces it supports and which smart contracts are responsible for their implementations
- Addresses limitations of previous approaches: [EIP165](#EIP-165-ERC-165-Standard-Interface-Detection), [EIP672](https://github.com/ethereum/EIPs/issues/672)
  - Backwards-compatible with EIP165
- The registry associates Ethereum addresses with the interfaces that they implement and the addresses of the implementations.
  - Interface identifers are generated by hashing the interface name.
  - The EIP specifies standards for interface names, as well as a hashing standard (`keccak256`).
  - The registry contract provides a helper function to generate the hash for an interface name.
- For any given address, there is exactly one address that can update its corresponding values in the registry; the latter address is referred to as the "manager" of the former address.
  - Initially, an address is its own manager.
  - The registry exposes a function that allows an address's manager to transfer this role at any time.
- Any contract that implements an interface on behalf of another address must implement an additional function: `canImplementInterfaceForAddress(bytes32, address) external view returns(bytes32)`.
  - In order to indicate that the contract implements the provided interface for a given address, this function must return a special sentinel value, which the EIP defines as `keccak256(abi.encodePacked("ERC1820_ACCEPT_MAGIC"))`.
- The EIP specifies a standard deployment protocol for the registry contract along with the definition of the registry.
  - The registry is deployed using a single-use address, for which no one knows the private key.
  - The address of the registry contract is identical for every chain on which it is deployed, [0x1820a4B7618BdE71Dce8cdc73aAB6C95905faD24](https://etherscan.io/address/0x1820a4b7618bde71dce8cdc73aab6c95905fad24).

### [EIP 1504: ERC-1504 Upgradable Smart Contract](https://eips.ethereum.org/EIPS/eip-1504)

```js
// TODO
```

### [EIP 1538: Transparent Contract Standard](https://eips.ethereum.org/EIPS/eip-1538)

```js
// TODO
```

### [EIP 1822: Universal Upgradeable Proxy Standard (UUPS)](https://eips.ethereum.org/EIPS/eip-1822)

```js
// TODO
```

## :rocket: The Ethernaut

_The Ethernaut is a Web3/Solidity based wargame inspired on [overthewire.org](https://overthewire.org/), played in the Ethereum Virtual Machine. Each level is a smart contract that needs to be 'hacked'._

### [Overview](https://solidity-05.ethernaut.openzeppelin.com/)

- The following Ethernaut levels were completed using [the account 0x0a8395c74b4048f1a40ea8c11b7e946a5ff93561on the Ropsten Ethereum network](https://ropsten.etherscan.io/address/0x0a8395c74b4048f1a40ea8c11b7e946a5ff93561).
  - [00 - Hello, Ethernaut!](https://solidity-05.ethernaut.openzeppelin.com/level/0xc8bAE5314C82d94858fE916067FbFCFE9F32097b)
    - Simple “Hello, World!” example to introduce the Ethernaut interface as well as the technologies that support it (e.g. [`web3.js`](#web3js) and [Truffle](#mushroom-Truffle)).
    - Instance address: [0x473773cCa24487DE241Aca80e5456C6C08B8855e](https://ropsten.etherscan.io/address/0x473773cca24487de241aca80e5456c6c08b8855e)
  - [01 - Fallback](https://solidity-05.ethernaut.openzeppelin.com/level/0xD95B091f19946d6ef0c88f8CD360c0d6E408876E)
    - Interact with a deployed contract (including [`fallback`](https://solidity.readthedocs.io/en/latest/contracts.html#fallback-function) and [`payable`](https://solidity.readthedocs.io/en/latest/contracts.html#modifiers) functions); introduces the concept of [contract ownership](https://solidity.readthedocs.io/en/latest/common-patterns.html#restricting-access).
    - Instance address: [0x4d6E5F42E6012fae3A370Bb01886C30423310a04](https://ropsten.etherscan.io/address/0x4d6e5f42e6012fae3a370bb01886c30423310a04)
  - [02 - Fallout](https://solidity-05.ethernaut.openzeppelin.com/level/0xAcdB8f800D8fE8c743f15Df8D41f9A91EFb54ed1)
    - Introduces vulnerabilities that can be exploited if explicit [constructors](https://solidity.readthedocs.io/en/latest/contracts.html#constructor) are not used.
    - Instance address: [0x04b378A754CB1e4A6878aCE3b3A9C64CafdFA632](https://ropsten.etherscan.io/address/0x04b378A754CB1e4A6878aCE3b3A9C64CafdFA632)
  - [03 - Coin Flip](https://solidity-05.ethernaut.openzeppelin.com/level/0xef9B87A4666fA565f9Be9b14fE6A34037caDCF16)
    - Explains inability of smart contracts to natively generate [randomness](https://solidity.readthedocs.io/en/latest/miscellaneous.html#global-variables).
    - Instance address: [0x86Cd8571CF3F67A9c62f658C65836A4B1ccbf5Fb](https://ropsten.etherscan.io/address/0x86Cd8571CF3F67A9c62f658C65836A4B1ccbf5Fb)
  - [04 - Telephone](https://solidity-05.ethernaut.openzeppelin.com/level/0x05911b4a9a392E6E59226AaaFe44701De970492D)
    - Illustrates the difference between [a transaction’s origin and the sender of an Ethereum message](https://solidity.readthedocs.io/en/latest/units-and-global-variables.html#block-and-transaction-properties).
    - Instance address: [0xf959C3FcF449100c06afEc7438F4Df66A9Cf4A42](https://ropsten.etherscan.io/address/0xf959C3FcF449100c06afEc7438F4Df66A9Cf4A42)
  - [05 - Token](https://solidity-05.ethernaut.openzeppelin.com/level/0x6E0B06770144b7b5923f3d759C19E1938Fe67807)
    - Demonstrates [overflow errors](https://solidity.readthedocs.io/en/latest/security-considerations.html#two-s-complement-underflows-overflows).
    - Instance address: [0xb96C6aA8A5BB211A6121f2b10759653cC9bF8106](https://ropsten.etherscan.io/address/0xb96C6aA8A5BB211A6121f2b10759653cC9bF8106)
  - [06 - Delegation](https://solidity-05.ethernaut.openzeppelin.com/level/0x6Ea2A13523bDbB97ED54bF4892A2ec82dE117Fd9)
    - Introduces delegation and the built-in [`delegatecall`](https://solidity.readthedocs.io/en/latest/units-and-global-variables.html#members-of-address-types) function.
    - Instance address: [0xF6E3d450F1DF7B37dDC867Ce7CdcdB8cc221EDA0](https://ropsten.etherscan.io/address/0xF6E3d450F1DF7B37dDC867Ce7CdcdB8cc221EDA0)
  - [07 - Force](https://solidity-05.ethernaut.openzeppelin.com/level/0xC624F5f9cd437ca9cefeF7672F3e951b2B27A42b)
    - Illustrates using the built-in [`selfdestruct`](https://solidity.readthedocs.io/en/latest/units-and-global-variables.html#contract-related) function to pay non-payable contracts.
    - Instance address: [0x847df6CE83D263B3097D0Abd360A3fdf88d110B1](https://ropsten.etherscan.io/address/0x847df6CE83D263B3097D0Abd360A3fdf88d110B1)
  - [08 - Vault](https://solidity-05.ethernaut.openzeppelin.com/level/0xe2F72aa61fD6322C6c4d22227a594391E051F990)
    - Demonstrates that `private` state variables are [only private from other contracts](https://web3js.readthedocs.io/en/v1.2.1/web3-eth.html#getstorageat).
    - Instance address: [0x809BeBdcbe8c5aDa2b513e50ca1518eFB254a9d1](https://ropsten.etherscan.io/address/0x809bebdcbe8c5ada2b513e50ca1518efb254a9d1)
  - [09 - King](https://solidity-05.ethernaut.openzeppelin.com/level/0xb29b192Ffaa3aa7cE40030b3eB1DC003a07195d0)
    - An example of how an error in [a downstream contract](https://github.com/OpenZeppelin/ethernaut/blob/master/contracts/levels/King.sol#L17) can produce unexpected consequences [upstream](https://github.com/OpenZeppelin/ethernaut/blob/master/contracts/levels/KingFactory.sol#L20), as in [the King of the Ether bug](http://www.kingoftheether.com/postmortem.html#Issue). See also: [Check-Effects-Interaction Pattern](https://solidity.readthedocs.io/en/develop/security-considerations.html#use-the-checks-effects-interactions-pattern)
    - Instance address: [0xEe2B4BE3D2d8FeEFF4048D69e1641024E35688A4](https://ropsten.etherscan.io/address/0xee2b4be3d2d8feeff4048d69e1641024e35688a4)
  - [10 - Re-Entrancy](https://solidity-05.ethernaut.openzeppelin.com/level/0x1f64228447c3A44E5e57672a31B005d6338c0D84)
    - Introduces re-entrancy and simulates [TheDAO hack](https://blog.openzeppelin.com/15-lines-of-code-that-could-have-prevented-thedao-hack-782499e00942/).
    - Instance address: [0xA5FD4b812E63B96db44e84997Bca8baa8C2F30eb](https://ropsten.etherscan.io/address/0xa5fd4b812e63b96db44e84997bca8baa8c2f30eb)
  - [11 - Elevator](https://solidity-05.ethernaut.openzeppelin.com/level/0xA122Fa37E1d89Edf3827560E1474987787DB0893)
    - Illustrates the fact that [interfaces](https://solidity.readthedocs.io/en/v0.5.13/contracts.html#interfaces) don’t enforce implementation.
    - Instance address: [0x8A2008E1b2816d2Fd79F21f884E16C8A1c4b3f8f](https://ropsten.etherscan.io/address/0x8a2008e1b2816d2fd79f21f884e16c8a1c4b3f8f)
  - [12 - Privacy](https://solidity-05.ethernaut.openzeppelin.com/level/0x63A43a71667126D6FEA382A53CdDAc252723bCC6)
    - A more convoluted demonstration of the concepts illustrated in Level 08 - Vault (e.g. [`web3.eth.getStorageAt`](https://web3js.readthedocs.io/en/v1.2.1/web3-eth.html#getstorageat)).
    - Instance address: [0xedbd38c3ccCE87B6125ADF6DD3fD0f49543C3792](https://ropsten.etherscan.io/address/0xedbd38c3ccCE87B6125ADF6DD3fD0f49543C3792)
  - [13 - Gatekeeper One](https://solidity-05.ethernaut.openzeppelin.com/level/0xfe1B0cb67F95Ab51e9052e70424A49A6d34769ed)
    - A bit-shifting and gas estimation puzzle.
    - Instance address: [0x30015cB14bf4886494554aD5db93AE8ED08eDEe2](https://ropsten.etherscan.io/address/0x30015cB14bf4886494554aD5db93AE8ED08eDEe2)
  - [14 - Gatekeeper Two](https://solidity-05.ethernaut.openzeppelin.com/level/0x6b5caFcdf28d6d4c27269E41A89F14A5e7250e90)
    - Another bit-shifting puzzle.
    - Instance address: [0xF912dE5f8d0dac8cA70c9F4a0d460950e9537850](https://ropsten.etherscan.io/address/0xf912de5f8d0dac8ca70c9f4a0d460950e9537850)
  - [15 - Naught Coin](https://solidity-05.ethernaut.openzeppelin.com/level/0xA683a179074f2f501Fb6b952E2EFF35860187F93)
    - Introduces [the ERC-20 specification](https://github.com/ethereum/EIPs/blob/master/EIPS/eip-20.md) and illustrates how inheritance can be a vector for attacks.
    - _Note_: As of 2019-12, the UI for this level appears to be broken (UI believes player has passed level before level instance has been created).
    - Instance address: [0x66F6470875C688132EDc3F55eCeC7795cBDc9E25](https://ropsten.etherscan.io/address/0x66F6470875C688132EDc3F55eCeC7795cBDc9E25)
  - [16 - Preservation](https://solidity-05.ethernaut.openzeppelin.com/level/0xa47AFD98aBba00C731C08F470890dCF6e42d8f27)
    - Another demonstration of the dangers of using the low-level [`delegatecall`](https://solidity.readthedocs.io/en/latest/units-and-global-variables.html#members-of-address-types) function; introduces the [`library`](https://solidity.readthedocs.io/en/develop/contracts.html#libraries) keyword.
    - Instance address: [0x62B79E8457844cadA1318d5B82d87E1C85677EB2](https://ropsten.etherscan.io/address/0x62B79E8457844cadA1318d5B82d87E1C85677EB2)
  - [17 - Recovery](https://solidity-05.ethernaut.openzeppelin.com/level/0x96077905c21208D86e46d3ccED03f8ce3D3d24E0)
    - Introduces some [interesting Ethereum quirks and vulnerabilities](https://swende.se/blog/Ethereum_quirks_and_vulns.html).
    - Instance address: [0x0a6Cbd7de1B523cD68a00f484ba04C5516c0B8AA](https://ropsten.etherscan.io/address/0x0a6Cbd7de1B523cD68a00f484ba04C5516c0B8AA)
  - [18 - Magic Number](https://solidity-05.ethernaut.openzeppelin.com/level/0x66CDc3A10fdE88AB05A7e6015b9E2Bd892a890Af)
    - Introduces [raw EVM bytecode](https://github.com/crytic/evm-opcodes) and [assembly](https://solidity.readthedocs.io/en/v0.5.13/assembly.html).
    - Instance address: [0x20012c88C6B95c0ef4C14E250482667F5f3F2FE2](https://ropsten.etherscan.io/address/0x20012c88c6b95c0ef4c14e250482667f5f3f2fe2)
  - [19 - Alien Codex](https://solidity-05.ethernaut.openzeppelin.com/level/0xf0D6F7dA4ed4Ff54761841e497F5aFc795F04688)
    - Illustrates an underflow attack.
    - _Note_: As of 2019-12, the UI for this level appears to be broken (UI loops on level instance creation).
    - Instance address: [0x48090Ea6d3f7Cf593Ba135C2b01Da9900d3ea311](https://ropsten.etherscan.io/address/0x48090ea6d3f7cf593ba135c2b01da9900d3ea311)
  - [20 - Denial](https://solidity-05.ethernaut.openzeppelin.com/level/0x7cbdE530fE990147c935D3B4594D458633871177)
    - Another re-entrancy attack.
    - Instance address: [0x9cD11a156D621222C7f59027F22634981F6744a6](https://ropsten.etherscan.io/address/0x9cd11a156d621222c7f59027f22634981f6744a6)
  - [21 - Shop](https://solidity-05.ethernaut.openzeppelin.com/level/0x57b61ae45d8Ec9778Cb7DCDEFfcaF515267daDbD)
    - Demonstrates that changes to state should be atomic.
    - Instance address: [0x596BB1BB9031D6b1F8041431eC1F95a87c91f5E9](https://ropsten.etherscan.io/address/0x596BB1BB9031D6b1F8041431eC1F95a87c91f5E9)
