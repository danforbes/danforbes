# Building the Ecosystem: There is no limit.

> The following document was written by @SBalaguer.

The Polkadot Ecosystem provides endless development possibilities. Depending on their business case,
teams can decided to either build new blockchains leveraging on the Substrate Framework, or go the
Smart Contract route, where Ink! and Solidity Smart Contracts are possible. No matter how you build
it, Polkadot then connects it all, making interoperability a reality.

> Why are teams using Substrate?

> There are several reasons why teams are choosing Substrate. But think of the following: If you are
> going to buy a computer from scratch, which computer will you choose? I do believe that this
> answer will depend on the actual usage of that device. If you are planning to work mostly with the
> Office Package, then getting a Windows Machine is the best way to go. However, if it's for design,
> then MacOs could probably be the best route.

> Same exact thing happens with blockchains. blockchains that process smart contracts are big,
> distributed computers. But these networks have been designed to process information in a
> particular way, and most commonly in a very broad way to fit as many uses cases as possible. That
> being said, they are not optimal for a project's specific use case. Teams have seen this in the
> past, so they decided to build their own blockchains. This is extremely hard.

> Substrate was specifically designed to make it easy for teams to develop their own blockchains
> with their own custom logic that fits their specific business case. All the security, message
> passing and base layer part of a blockchain has already been developed in Substrate and it's
> available to everyone (teams can change it if they need to), leaving the Runtime, the _brains_ of
> the blockchain, ready like a white canvas for teams to paint their own picture.

Easiness and flexibility to develop together with true interoperability is why Polkadot has the
second largest -and growing fast 🚀!- ecosystem in the blockchain space after Ethereum.

This guide will serve the sole purpose of outlying the blockchain space in different verticals, with
ideas of projects that can be built. Although extensive, this is **a guide** limited by the
imagination and knowledge of who is writing it. Please take it as such, and let your imagination and
business goals guide your development.

## Blockchain Verticals

The blockchain space is re-thinking the way that people communicate between each other. When
societies started, small groups of people interacted between each other and almost everyone knew
everyone, making trust processes easy to track. As societies grew, people stopped knowing each
other, but still needed to trust other people in order to interact. This is when big companies
stepped in: they would be the trusting layers of societies. Bob didn't know Alice and Alice didn't
know Bob, but they needed to conduct business. Both Bob and Alice knew and trusted the Bank in their
town. Therefore, this Bank was the trusted layer between Bob and Alice. Charlie decided to fly
abroad to visit some friends. When entering the new country, the Immigration Officer didn't know who
Charlie was, but Charlie had a passport issued by his home country. This Immigration Officer trusts
the government of Charlie's home country, and can therefore attest that Charlie is Charlie through
Charlie's passport.

Trust is everywhere people interact. But what if this centralised third parties don't act
truthfully? Who really prevents that from happening? blockchain is a solution that removes the need
of people trusting each other or centralised entities. It is the interconnected network of nodes
that keeps track of what is true, and no one single person owns that. As Gavin Wood -Co-Founder of
Ethereum and Founder of Polkadot- says: `Less trust, more truth`.

### TL;DR

The ecosystem is infinite. Think of what are people trusting on, and think of how to transform that
intro a trustless blockchain based project. Then, think of how blockchain technology can enhance
that. That is the end goal.

### Index

- [DeFi](#defi)
- [Privacy / Identity](#privacy--identity)
- [File Storage](#file-storage)
- [Gaming](#gaming)
- [Gambling](#gambling)
- [NFTs](#nfts)
- [Bridges](#bridges)
- [Oracles](#oracles)
- [Supply Chain](#supply-chain)
- [IoT](#iot)
- [Wallets](#wallets)
- [Social](#social)
- [Real State](#real-state)
- [Scaling](#scaling)
- [Government](#government)
- [Energy](#energy)
- [Enterprise](#enterprise)
- [Others](#Others)

## DeFi

DeFi is short for Decentralized Finance, and it is a collection of projects that aim to develop, in
a decentralized and trustless manner, different financial applications. DeFi is the largest growing
category in blockchain, and as such it has various verticals inside of it (please see below).
According to [DeFi Pulse](https://defipulse.com/) there is now $22.2 Billion Dollars locked in DeFi
Applications. A year ago, that number was 675 Million Dollars.

### Stablecoins

Stablecoins aim to produce a decentralized token that has the ability to keep it's value pegged to a
reference asset. This removes the fluctuations that the overall blockchain industry has in the price
of it's tokens.

Stablecoins can be categorized in three verticals:

- **Token-Backed Stablecoins**. These stablecoins are backed by another token that has value.
  Generally speaking, and in order to counter the price fluctuations of the underlying token, these
  projects tend to over-collateralized their supply. A good example of this is
  [Acala](https://acala.network/) with the aUSD stablecoin (1 aUSD = 1 USD). Acala is actually
  developing the DeFi specific Substrate blockchain for the Polkadot Ecosystem. The Acala Team has
  designed their blockchain specifically for the DeFi use case, and they also support Solidity Smart
  Contracts. In fact, Acala was part of the Berkley Xcelerator last year.
- **Fiat-Backed Stablecoins**. These Stablecoins are usually minted in the same proportion as they
  have it's underlying fiat currency. [USDT](https://tether.to/) is an example of this category.
- **Algorithm Stablecoins or non-collateralized Stablecoins**. These Stablecoins don't have
  collateral, but rather manage to keep the price pegged to their underlying asset by changing the
  supply of the token. An example here is [AMPL](https://www.ampleforth.org/).

> Can a Stablecoin backed by a group of other tokens be created? Changing the mix of coins would
> change the value of the stablecoin, thus keeping it...stable.

_[Back to DeFi](#defi)_

_[Back to Index](#index)_

### CBDCs

CBDC stands for Central Bank Digital Currency, and it is something that most central banks are
starting to evaluate. Different from other tokens, CBDCs are supposed to be controlled by Central
Banks or by Government Entities, therefore regulating the supply and demand of the token. The
underlying technology to CBDCs would be a blockchain, although it needs to consider that all
information cannot be accessible by everyone, as this ledger would contain the balances of people
and companies.

> Is there market fit for CBDCs in Developing Countries?

_[Back to DeFi](#defi)_

_[Back to Index](#index)_

### Exchanges - DEX / CEX

Exchanges are platforms for people to exchange cryptocurrencies, and are a very important piece of
the blockchain Ecosystem, since they allow liquidity to flow into the ecosystem and within different
projects. Generally speaking, exchanges can be classified in two categories:

- **Decentralized Exchanges (DEX)**. This category allows for direct P2P exchanges with no need of
  personal information sharing. The main issue with DEX is fostering the availability of tokens for
  people to exchange. [Uniswap](https://uniswap.org/), the most popular DEX today, managed to solve
  the token availability issue by creating the concept of Liquidity Pools and Automatic Market
  Makers. The idea behind this concept is that the platform would incentivize liquidity in relation
  to the demand of tokens and the number of tokens available in the platform. If a token is very
  demanded and there is low liquidity, then Uniswap would offer a high interest rate for people
  depositing their tokens in that pool. A drawback of DEXs is that they cannot offer fiat on/off
  ramps. It is only possible to transfer blockchain Tokens. In fact, Logan Saether (@lsaether) has
  created a [pallet to be used on Substrate that mimics the Uniswap Functionality](https://github.com/lsaether/pallet-swaps).

- **Centralized Exchanges (CEX)**. This category allows for people to exchange directly with a
  platform and not P2P. A good example of this is [Binance](https://www.binance.com/). CEXs require
  licenses to operate, and therefore they will require several KYC steps with there users. CEX's
  don't usually have liquidity issues since they are managed by a central entity and they can
  provide fiat on/off ramps thanks to the compliance with regulations and KYC.

Apart from these, there are projects like [1Inch](https://1inch.exchange/) that serve as DEX
Aggregators. These projects scout the different DEX platforms looking for the best possible price
for a user's exchange.

Lastly, there is a risk with this category, specially with DEX's. blockchains generate pools of
transactions that miners select from, validate, and then produce a block with. But what if a miner
selects the transactions that it wants in order to change the price of an asset in the future?
Substrate allows for teams to design the Runtime (a.k.a the _brains_ of the blockchain) in the way
that they need, therefore allowing for these issues to be prevented if the team wanted to.

_[Back to DeFi](#defi)_

_[Back to Index](#index)_

### Borrowing and Lending

These encompass all the projects that provide borrowing and lending capabilities with
cryptocurrencies. These platforms usually pay an interest rate to those how provide liquidity, and
charge for those that require liquidity. Apart from this, when a user wants to ask for liquidity,
the protocol with ask for some collateral which is retain if the loan is not repaid after the agreed
term. An example of a Borrowing and Lending project is [Aave](https://aave.com/). When holding a
token, a user can deposit it on Aave for lending. Aave will issue 1:1 aTokens (if you deposit BAT
you would get aBAT) which value starts compounding from the moment you deposit. Holding those
aTokens allows the user to withdraw the underlying token. On the other hand, if borrowing, a user
will need to deposit a collateral which collateralization rate will depend on the collateral asset
deposited.

A very interesting property that this category has is that it managed to create something called
**Flash Loans**. This type of Loans are exclusive for the blockchain ecosystem, and do not exist in
any other financial system. Flash loans allow users to take a loan with no collateral as long as
they repay it in **the same transaction**. This sounds strange, but given that different function
calls can happen in the same transaction in blockchain, then a user could request a Flash Loan, then
call a smart contract to leverage an opportunity, get the money back, and then repay the first loan.

> How would house -or any other- mortgages work?

_[Back to DeFi](#defi)_

_[Back to Index](#index)_

### Yield Aggregators

Yield Aggregators are programmed to get the best deal out of a specific token for a user. They
always search for what is the highest return that token can have in the ecosystem and directly
invest there. Given the exponential growth of the DeFi ecosystem and with so many teams providing
returns to get more liquidity into, these Yield Aggregators became very popular. A good example for
this is [Yearn.Finance](https://yearn.finance). In the Polkadot ecosystem, Yield becomes even more
interesting than in Ethereum, since there are more sources to generate Yield from. For example, DOT
Holders generate Yield by staking their DOTs.

_[Back to DeFi](#defi)_

_[Back to Index](#index)_

### Synthetic Assets

Synthetic Assets are a token representation of real life assets like USD, Gold, or Tesla Stocks.
They allow users to hold a token that represents the asset -with it's changes in price-, without the
need of having that actual asset. An example of a protocol like this is
[Synthetix](https://www.synthetix.io/).

> Could something be done here to tokenize other assets like cars or houses? Could these assets be
> used for borrowing and lending?

_[Back to DeFi](#defi)_

_[Back to Index](#index)_

### Crowdfunding

Crowdfunding projects assists teams to raise capital in a decentralized way. A good example for this
is [Polkastarter](https://www.polkastarter.com/), that allows projects to build pools, set a target
of investment and timeframe and aim to get that money. If they manage to do it, then the money goes
to the team, and project tokens flow to the investors. If not, the investors receive their original
tokens back.

> Besides crowdfunding, what about crowd-collaboration in a decentralized way? How would a group of
> people working together to get a project done look like?

_[Back to DeFi](#defi)_

_[Back to Index](#index)_

### Prediction Markets

Prediction Markets intend to provide an estimation of the outcome of an event by having people
investing on that outcome. A user can create a scenario for other users to vote on depending on the
possible outcome. A good example of this is [Polymarket](https://polymarket.com/)

_[Back to DeFi](#defi)_

_[Back to Index](#index)_

### Derivatives

_[Back to DeFi](#defi)_

_[Back to Index](#index)_

### Insurance

blockchains and Decentralized Applications are still on a very early stage, and therefore error
prone. This gave way to a new projects to be created that help user protect their assets. Insurance
protocols therefore offer insurance in different blockchain spaces. A good example of this is
[Tidal](https://tidal.finance/) that allows users to create pools of tokens to be insured.

> Could their be a blockchain based insurance project that insures real-life assets?

_[Back to DeFi](#defi)_

_[Back to Index](#index)_

### Security Tokens

These projects tokenize real life security assets (equity, debt, real estate, etc.). A good example
of this is [Polymath](https://polymath.network/), that started the Security Token as a Smart
Contract project and has now moved on to [Polymesh](https://polymath.network/polymesh) which is
their very own custom blockchain.

_[Back to DeFi](#defi)_

_[Back to Index](#index)_

### Other Ideas

Other avenues worth exploring in the DeFi vertical are Margin Trading Projects, Staking Liquidity
(could it be possible for people to remain liquid while staking their tokens?), Fixed Interest Rates
projects (having this decentralized would be awesome!), Payment platforms
([Valiu](https://www.valiu.com/) is doing something interesting, even more for the World Region they
are targeting), and much more!

_[Back to DeFi](#defi)_

_[Back to Index](#index)_

## Privacy / Identity

Privacy is one of the most controversial topics in the current Web2 Ecosystem. Big centralised
organizations have a lot of information from users and profit from that. Think Facebook is free?
Well, not really. They justs make it look like it is, but you gave something very valuable to them:
all your personal data. Still, it is sometimes important to understand who is interacting on the
other side, because it could be the case that you want users to have accounts (web3 accounts, not
web2 accounts). Privacy and Identity projects try to solve this problem of understanding who is who
without actually asking who is who. A very good project working on this is
[Kilt](https://www.kilt.io/). Through certifications validated by attesters, users (or claimers) can
claim information about themselves.

Another interesting discussion to follow is what is going on in the
[Substrate Open Working Groups](https://github.com/paritytech/substrate-open-working-groups/discussions/2)
regarding how to deal with Identity and Privacy on the Polkadot Ecosystem. The SOWGs have been
created so that the community can develop what the community needs.

_[Back to Index](#index)_

## File Storage

File Storage solutions intend to allow users to save their files in a decentralized manner, without
the need to trust big companies to save (and possibly tamper) the data. The underlying method of
achieving this is by generating a incentivize network where users that have free disk space share
that with the ecosystem, and users that need to save data pay for that space. All in all, a file of
a user is distributed encrypted in different nodes of a blockchain instead of being in a sever owned
by a company.

Examples of File Storage Blockhains are [IPFS](https://ipfs.io/), [FileCoin](https://filecoin.io/),
[Equilibrium](https://equilibrium.co/) and DatDot.

> How would a decentralized Dropbox look like? Could I share pictures of my Dog with my Mom in a
> decentralized way?

_[Back to Index](#index)_

## Gaming

Several projects have aimed to decentralized Gaming, where no central party controls the actual
status of the game and of the players in that game. Blockchains in itself are not well prepared for
transactions taking milliseconds: imagine if you want to move a character and whenever you press the
left arrow it takes seconds for the player to move. That would not be viable! Teams have gotten
smart on how to use the blockchain level for keeping track of things.

[Xaya](https://xaya.io/) has been building games from the beginning of blockchain, and they are a
good example of this category.

> I don't think of any game that can't be re-created. Sims? Poker? Really, anything.

_[Back to Index](#index)_

## Gambling

There are teams that are creating decentralized Gambling solutions, where Privacy is key.
[BetProtocol](https://www.betprotocol.com/) has come up with a very interesting approach: they
didn't create a specific Gaming blockchain, they created a platform for people to build their own
blockchain-based gambling platform.

_[Back to Index](#index)_

## NFTs

NFT stands for Non-Fungible Token, which means that is a token that it can not be divided. Some
people would argue that NFTs is not a category in itself, but rather a way of dealing with tokens in
all of the other categories. Given that there is a big bet on NFTs for 2021, I will be leaving this
a a separate category for now.

The first example of an NFT was [Cryptokitties](https://www.cryptokitties.co/), a game that was
created for users to generate unique Kittens with unique features that could then mated with another
Kittens or traded for money. Cryptokitties generated the very first wave of expensive gas fees on
Ethereum since it collapsed the network at its peak.

NFTs are used for a lot of different cases. There are Art Galleries, Marketplaces, and others. A
good example here is what [UseTech](https://usetech.com/blockchain/) through their
[Unique Network](https://usetech.com/usetech-announces-unique-network-nft-chain-on-substrate/) has
[done in Kusama](https://usetech.com/ksm-bridge-for-nft-markeplace/).

> A Masters Degree diploma is unique: It has the students own courses and grades. How would a system
> of NFT Diploma's work? What would people use it for?

> An NFT Marketplace: a Uniswap like template with NFT's standrds. You can leverage [this pallet](https://github.com/lsaether/pallet-swaps).

_[Back to Index](#index)_

## Bridges

Bridges aim to connect different blockchains. There are mainly two types of bridges:

- **Custodian Bridges**. In this case, the team behind the bridge solution keeps custody of the
  interactions happening in the bridge. For instance if a user wants to use ETH on the Polkadot
  Ecosystem, they could go through a Custodian Bridge, where the user would deposit ETH, and it
  would get a wrapped version of that ETH that in value equals the same ETH he put in, but that can
  be used in Polkadot.

- **Trustless Bridges**. Trustless bridges do not require any third party to control what is going
  through the bridge. It automatically bridges both blockchains in a trustless way. Two good
  examples of these are: [Interlay](https://www.interlay.io/) which is a BTC<>DOT bridge and
  [Snowfork](https://snowbridge.snowfork.com/) which is a ETH<>DOT Bridge. These two bridges allow
  for any Substrate-based chain connected to Polkadot to interact trustlessly with BTC and with ETH.

_[Back to Index](#index)_

## Oracles

Oracles are one of the gateway that brings the outside world into the blockchain space. Oracles are
services that verify and send real world occurrences to the blockchain space. As such, they are a
big source of truth for the ecosystem, but what if the 'outside' source of data is wrong? What would
happen next? Oracles are both extremely important and dangerous. A good example of Oracles is
[Chainlink](https://chain.link/).

There is for sure a lot to be done with Oracles. These have seen a huge growth with DeFi so
everything related to prices, volume and other market related information has been the first to
develop.

> Data Marketplaces, decentralized, true, for everyone 💭. With Substrate.

_[Back to Index](#index)_

## Supply Chain

These blockchains aim to provide a trust layer for critical Supply Chain Systems. Food traceability,
clothes traceability and much more. Check out [Origin Trail](https://origintrail.io/), which is a
Substrate based chain creating Supply Chain solutions.

> 💡 Pharmaceutical Industry. Full traceability.

_[Back to Index](#index)_

## IoT

Projects are leveraging blockchain as a trustless layer of communication between IoT devices. This
increases the security of the services, since data in devices is not easily hackable, as well as
privacy, given that no IoT company owns (and therefore can sell) the data generated by the
individual devices.

Checkout [Noddle](https://nodle.io/) who was part of the Berkeley Xcelerator last year, and is
working with IoT devices.

_[Back to Index](#index)_

## Wallets

Wallets are an extremely important component of the blockchain ecosystem: they allow users to keep
their tokens safe, and to use them in the different applications. Wallets come in lots of different
flavours.

- **Cold Wallet**.
- **Hot Wallet**.
- **Browser Wallet**.

> A wallet for substrate chains that allows staking and generates tax reports for staking earnings.

_[Back to Index](#index)_

## Social

Social blockchain projects have as an objective to connect different people around the world in a
decentralized way. There are different projects building social interactions, and each of them have
their own objective. [Social.Network](https://social.network/) is connecting people and helping them
create societies for specific purposes, whereas [SubSocial](https://subsocial.network/) has
something similar to a Blogging Platform.

The key about Social blockchains is to figure out why you need people to connect between each other.
Re-doing Social networking as we know it from Web2 (Facebook, Instagram, TikTok, etc), might not be
the best strategy.

_[Back to Index](#index)_

## Real Estate

One could argue that everyone in the world needs a house to live in. Real estate impacts everyone's
lives. Still, the entire process of getting a house/apartment is kind of strange. Buying, selling
knowing why that piece of land or house or apartment is worth that, is rather strange. It kind of
lacks a bit of transparency. Check out what [Propy](https://propy.com/browse/) is doing as a good
example.

_[Back to Index](#index)_

## Scaling

Scaling solutions are one of the most look at things right now on the blockchain space. How to get
the security and privacy of the blockchain, with the speed that we are used to right now with
centralised servers.

_[Back to Index](#index)_

## Government

Decentralised Autonomous Organizations (DAOs) have become popular in blockchain, to the point that
protocols have included in their own solutions DAOs for protocol users to be able to decide upon the
future of that same protocol. You can check out what [SubDAO](https://www.subdao.network/) is doing
as a good example.

Still, there are more things on to Government in general that can benefit of blockchain. Public
lease process, voting, and much more.

> Elections with blockchains. Only 1 vote per person, no possible fraud.

_[Back to Index](#index)_

## Energy

Teams are working on decentralising energy, one can hardly imagin what would happen if users can benefit from renewable energy for private usage as well as selling energy back to the grid for other users consumption in a fully peer-to-peer manner? Check out what [Mpowa](https://mpowa.io/), [d3a](https://www.d3a.io/) and [energy web](https://www.energyweb.org/) is doing. Other concepts can also [include decentralizing investments](https://www.beautiful.ai/player/-MQaMo8Ogw69fZFJkAji) for renewable energy systems, with Tesla spear heading the decentralisation of energy production and consumption, having decentralized systems in the context of [Teslas Autobidder](https://www.tesla.com/support/autobidder) would just fit in naturally.

_[Back to Index](#index)_

_[Back to Index](#index)_

## Enterprise

Rather than a specific vertical, this category aims to give perspective on the fact that any
solution can be though with enterprises or regular users in mind. Ideas in this scenario could be
vast, given that enterprises usually take longer to adopt cutting edge technology. Apart from that,
enterprises will probably want to keep things private, but with the ability to interact with other
public blockchains,

> Private enterprise blockchain that can communicate with the public ecosystem.

_[Back to Index](#index)_

## Others

There are teams like [Deeper Network](https://deeper.network/) that allow users to share their
internet and profit from that, or teams like [Centrifuge](https://centrifuge.io/) that are working
on digitalizing in a decentralised manner very specific company assets.

_[Back to Index](#index)_
