---
layout: default
description: Scalable & confidential smart contracts for Bitcoin & lightning network
---

# RGB Blueprint

RGB is a suite of protocols for scalable & confidential smart contracts for Bitcoin & lightning network. They embrace concepts of private & mutual ownership, abstraction and separation of concerns and represent "post-blockchain", Turing-complete form of trustless distributed computing which does not require introduction of "tokens".

As a smart contract system RGB is quite different from previous approaches, both Bitcoin-based \(Colored coins, Counterparty, OMNI\) and non-bitcoin \(Ethereum, EOS and others\):

* RGB separates concept of smart contract **issuer**, **state owners** and **state evolution**
* RGB keeps the smart contract code and data offchain
* RGB uses blockchain as a state commitment layer and Bitcoin script as an ownership control system; while smart contract evolution is defined by off-chain **schema**

More about these concepts can be read in [this presentation](https://github.com/LNP-BP/FAQ/blob/master/Presentation%20slides/RGB%20%26%20Spectrum%20explanation%20for%20business.pdf).

## Core underlying concepts

In order to understand technical details behind RGB one have to become familiar with the following concepts, which are hevily used in RGB design:

* Distributed systems \(replicated state machines\), including
  * PRISM \(partially-replicated infinite state machines\) computing
  * AluVM instruction set architecture
* Non-imperative computing, including
  * Declarative programming
  * Cellular automation
* Zero knowledge protocols, including
  * Confidential transactions
  * Bulletproofs
* Cryptographic commitment schemes, including
  * BIP-340 tagged hashes
  * Advanced merklization schemes \(LNPBP-\)
  * Multi-message commitments
  * Deterministic bitcoin commitments
* Client-side-validation, including:
  * Strict encoding \(LNPBP-7\)
  * Commit-conceal schemes
  * Single-use-seals
* Bitcoin transactions, including
  * PSBTs v1 and v2
  * Bitcoin TxO2 single-use-seals
* Lightning network protocol, including
  * Lightning P2P message extensions
  * Generalized lightning channels

## Design

Briefly, RGB smart contracts operate with **client-side validation** paradigm, meaning that all the data is kept outside of the bitcoin transactions, i.e. bitcoin blockchain or lightning channel state. This allows the system to operate on top of Lightning Network without any changes to the LN protocols and also gives a foundation for a high level of protocol scalability and privacy.

As a security mechanism RGB uses **single-use seals** defined over bitcoin transaction outputs, which provides ability for any party having smart contract state history to verify its uniqueness. In other words, RGB leverages Bitcoin script for its security model and definition of the **ownership** and **access rights**.

Each RGB smart contract is represented by some **genesis state**, created by **smart contract issuer** \(or, put simply, issuer\) and a directed acyclic graph \(DAG\) of **state transitions** kept in form of _client-validated data_ \(i.e. this data is not stored on blockchain or within LN transactions/channel state\). The state is **assigned** to unspent bitcoin transaction outputs, which defines them as _single-use seals_. The party that is able to spend corresponding transaction output is named a party **owning state**: it is a party that has the right to change the corresponding part of the smart contract state by creating a new _state transition_ and committing to it in a transaction spending the output containing previous state. This procedure represents **closing of a seal over state transition**, and a pair of spending transaction and corresponding extra-transaction data on the state transition are named **witness**.

_State transition_ **assigns** _state_ to a set of defined **single-use seals**. Each smart contract may maintain different forms of state and define different kinds of single-use seals with different validation rules. Additionally to this, state transition may contain different metadata and _scripts_, defining parts of its business logic.

Which types of state, seals, metadata and which script extensions are allowed within state transitions is defined by **schema**. Thus, schema can be seen as validation rules for _client-side validation_; schema is always defined by the issuer in state genesis. Schema also may contain Turing-complete _scripts_ defining parts of the business logic for _client-side validation_.

RGB operates in “shards”, where each contract has a separate **state history** and data; different smart contracts never intersect in their histories directly. This allows another level of scalability; and while the term “shard” is incorrect, we use it to demonstrate that RGB actually achieves what was planned to be achieved with “Ethereum shards”.

While being separately maintained, RGB contracts may interact via **Bifrost** protocol over the Lightning Network, allowing multiparty **coordinated state changes**, which, for instance, enables functionality like DEX over Lightning etc.

Thus, by their abilities RGB smart contracts go beyond what is possible with Ethereum-like smart contract system, providing more layered, scalable, private and safe approach, where the ownership of the smart contract state is separated from the smart contract creation.

## Compatibility and interoperability

* SegWit v0
* Taproot \(SegWit v1\), Tapscript
* Schnorr signatures
* Ed19255 signatures and Curve19255 keys
* Miniscript
* Eltoo \(SIGHASH\_ANYPREVOUT\)
* OP\_CHECKTEMPLATEVERIFY
* Lightning network
* Atomic swaps
* UTXO-based blockchains with Bitcoin script
* Tor
* Internet2

## History & Acknowledgements

RGB was originally envisioned in 2016 by _Giacomo Zucco_ \(**BHB Network**\) as a "non-blockchain based asset system" basing on earlier ideas of _Peter Todd_ about client-side-validation and single-use-seals and implemented as original MVP around 2017 by BHB Network with support from **Poseidon Group**. Since 2019, _Dr Maxim Orlovsky_, **Pandora Core AG**, acts as the main designer and lead contributor to RGB protocol, designing and implementing more than 95% of its current code and underlying standards. Since 2019 RGB was restructured and re-thought from scratch by him as a generic form of computing and confidential smart contracting system. This refactoring happened as a part of **LNP/BP Standards** effort, created in 2019 and initially funded by **iFinex Inc** and **Fulgur Ventures** \(2019H2-2020H1\), and, later \(from 2020H2\), by **Pandora Core AG**, personal funds of _Dr Maxim Orlovsky_ and community donations. A lot of input into RGB design, re-design and protocol peer-review came from a broader community, which included contributions from more than 50 people and organizations, including:

* _Christophe Diederichs_, 
* _Cláudio de Castro,_
* _Chris Stewart,_
* _Emil Bayes_, 
* _Fabrizio Armani_, 
* _Federico Tenga_, 
* _Juraj Bednar,_
* _Martino Salvetti_, 
* _Max Hillebrand_, 
* _Marco Amadori_, 
* _Martin Habovštiak_, 
* _Nadav Kohen,_
* _Nicola Busanello_,
* _Rajar Shimaitra,_
* _Rene Pickhardt_, 
* _Reza Bandegi_, 
* _[Sosthene](https://github.com/Sosthene00)_,
* _Stefano Pellegrini_, 
* _[yojoe](https://github.com/yojoe)_,
* _ZmnSCPxj_, 
* _Zoe Faltiba,_
* _[zkao](https://github.com/zkao)_,

The community and contribution management since 2019 was performed by _Olga Ukolova_, Pandora Core AG.

Many input into protocol design ideas and suggestions came from personal conversations of Dr Maxim Orlovsky and Giacomo Zucco with notable cryptographers, specialists on distributed systems and game theorists, including:

* _Adam Back,_
* _Andrew Poelstra,_
* _Christian Decker_, 
* _Christopher Allen,_
* _Pieter Wuille,_
* _Peter Todd,_
* _Sabina Sachtachtinskagia,_ 

We are thankful to the early adopters of RGB protocols, who invested and continue to invest into RGB integration and independent peer reviews:

* HodlHodl
* Bitfinex & Tether
* Condensat Technologies
* inbitcoin
* SuredBits
* Blockchain Of Things
* Atomic Loans
* Farcaster project
* Sphinx
* Nym


# Contacts

Basic information about the project can be provided by the developers in the
[Telegram Group](https://t.me/rgbtelegram).

# License

![Creative Commons License](https://i.creativecommons.org/l/by/4.0/88x31.png "License CC-BY")

This work is licensed under a [Creative Commons Attribution 4.0 International
License](http://creativecommons.org/licenses/by/4.0/).
