Selfdrop# Roamr Selfdrop: Public Authentication On The Blockchain
### August 2018

_______________________________________________________________

## Table of Contents
- [Abstract](#abstract)
- [Blockchain & Ethereum](#blockchain--ethereum)
  - [Public-Key Cryptography](#public-key-cryptography)
  - [Merkle Trees](#merkle-trees)
  - [Building on Ethereum](#building-on-ethereum)
  - [Smart Contracts](#smart-contracts)
  - [Ethereum Virtual Machine & Gas](#ethereum-virtual-machine--gas)
- [Roamr Design](#Roamr-design)
  - [Private vs. Permissioned vs. Public Networks](#private-vs-permissioned-vs-public-networks)
  - [A Public Ledger for Private Systems](#a-public-ledger-for-private-systems)
  - [Architecting for Adoption](#architecting-for-adoption)
- [Selfdrop](#Selfdrop)
  - [The State of Drone Security](#the-state-of-Drone-security)
  - [Adding a Blockchain Layer](#adding-a-blockchain-layer)
  - [Server-Side Selfdrop](#server-side-Selfdrop)
    - [Overview](#overview)
    - [Benefits from the Blockchain](#benefits-from-the-blockchain)
    - [A Detailed Look](#a-detailed-look)
    - [Case Study: Server-Side Selfdrop & Roamrgen](#case-study-server-side-Selfdrop--Roamr)
  - [Client-Side Selfdrop](#client-side-Selfdrop)
    - [Overview](#overview)
    - [Benefits from the Blockchain](#benefits-from-the-blockchain)
    - [A Detailed Look](#a-detailed-look)
  - [Opening Selfdrop To The Public](#opening-Selfdrop-to-the-public)
- [Risks](#risks)
- [Conclusion](#conclusion)

_______________________________________________________________

## Abstract


Roamr is an AI + Blockchain Powered Drone flight system for delivery. Roamr is a new component that brings blockchain-based capabilities to the Roamr ecosystem and beyond.

Roamr enables new and existing private systems to seamlessly integrate and leverage the dynamics of a public blockchain for functional use drone flights as a revolusionized means of transportation. 

This paper introduces Roamr and explores the initial use case - an authentication layer involving a public blockchain product that can act as a supplement to existing off-chain transportation means. The proposed framework is called Selfdrop and it seeks to provide additional Security for sensitive data that is increasingly at risk of compromise.

## Blockchain & Ethereum

Roamr is implemented on Ethereum, an open-source public blockchain platform launched in 2015. Before providing more detail on the project, it is important to review some fundamental ideas about blockchain and Ethereum.

### Public-Key Cryptography
Public key cryptography is an asymmetric framework that relies on key pairs. These pairs consist of public keys (that are not secret) and private keys (that are held secretly by the owners of the corresponding public keys). A public key can be derived from it's corresponding private key, but not the other way around.

Various techniques are used to create useful interactions between public and private keys. One use of public-key cryptography for blockchains such as Bitcoin and Ethereum revolves around a concept known as digital signatures. With digital signatures, a message can be signed with a private key and another party can validate the public key of the message author. This concept can be used within a blockchain network to help ensure that hackers do not impersonate people or forge signatures.

Ethereum utilizes Elliptic Curve Digital Signature Algorithm (ECDSA) to validate transaction signatures.

![Elliptic Curve Digital Signature Algorithm](./_assets/ecdsa.png)

ECDSA leverages the infeasibility of solving certain mathematical calculations based on the structure an elliptic curve, such as the one depicted above. In its signature model, Ethereum uses a particular kind of elliptic curve that conforms to the secp256k1 parameter set.

### Merkle Trees
Merkle trees are used in distributed systems for efficient data verification. They are efficient because they use hashes instead of full files. Hashes are ways of encoding files that are much smaller than the actual file itself.
Every block header in Ethereum contains three Merkle Trees for Transactions, Receipts, and States:

![Merkle Tree](./_assets/merkle_tree.png)
Source: [Merkling in Ethereum](https://blog.ethereum.org/2015/11/15/merkling-in-ethereum/); Vitalik Buterin, Ethereum Founder

This makes it easy for a light client to get verifiable answers to queries such as:

- Does this account exist?
- What is the current state?
- Has this transaction been included in a particular block?
- Has a particular event happened in this address today?

### Building on Ethereum
Much as apps like Snapchat were built with Swift and other tools offered on top of the Apple iOS platform, so too can Drone applications be built on top of Ethereum. Snap Inc. didn’t need to build iOS, it used it as infrastructure to launch a game-changing social media application.

Project Roamr is similar. It relies on the thousands of developers globally that are working to make underlying blockchain technology faster, stronger, and more efficient. Roamr leverages this constantly improving infrastructure by developing product-focused interactions around blockchain technology that can offer tangible benefits to Drone services applications.

### Smart Contracts
A key concept enabled by Ethereum and other blockchain-based networks is that of smart contracts. These are self-executing blocks of code that multiple parties can interact with, cutting out the need for trusted middlemen. Code in a smart contract can be seen as similar to the legal clauses in a traditional paper contract, but can also achieve much more expansive functionality. Contracts can have rules, conditions, penalties for non-compliance, and can kickstart processes and other contracts. When triggered, contracts execute as originally stated at the time of deployment on the public chain, offering built-in elements of immutability and decentralization.

The smart contract is a vital tool for building on the Ethereum infrastructure. Core functionality of the Roamr blockchain layer is achieved via custom contracts, as discussed later in this paper.

### Ethereum Virtual Machine & Gas
The Ethereum Virtual Machine (EVM) is the runtime environment for smart contracts on Ethereum. The EVM helps to prevent Denial of Service (DoS) attacks, ensures programs remain stateless, and enables communication that cannot be interrupted. Actions on the EVM have costs associated with them, denominated in gas, which depend on the computational resources required for execution. Every transaction has a maximum amount of gas allotted to it, known as a gas limit. If the gas consumed by a transaction reaches the limit, it will cease to continue processing.

## Roamr Design
The Roamr technology ecosystem follows a design philosophy centered around achieving use cases that marry private systems with the advantages of blockchain, including decentralization, immutability, and transparency. Consequently, the underlying blockchain architecture is of critical importance.

### Private vs. Permissioned vs. Public Networks
There are several ways that blockchain network architecture can be implemented. The various approaches can generally be categorized as Private, Permissioned, or Public. The primary difference between these network architectures is in the nature of the nodes involved in securing and/or verifying the ledger and its entries. The variance in implementation has implications for the aforementioned advantages of blockchain, as depicted in the table below:

Category | Description | Decentralization | Immutability | Transparency
-------- | ----------- | ---------------- | ------------ | ------------
Private | Nodes in the network are majorly or entirely run by a private party | None | None | Not Guaranteed
Permissioned | Nodes in the network are run by 3rd parties who are granted express permission by a private party | Low | Moderate | Not Guaranteed
Public | Nodes in the network may be run by any party with sufficient technical capability | High | High | Guaranteed

As a caveat, there are many nuances that can impact the dimensions shown above; the strength of a given blockchain depends on the specific implementation. Despite that, public ledgers (such as Ethereum) offer the best ability to cultivate the advantages that are vital to Roamr's mission.

### A Public Ledger for Private Systems
The systems that power drone services platforms, websites, and applications can often be described as mediums of data flow - they send, retrieve, store, update, and process data for the entities they interface with. Because of the nature of this data, and of drone services more generally, these systems often house complex operations in a private and centralized manner. Reliance on private structures, in turn, opens the door for a variety of security, integrity, and efficiency gains to be had by incorporating external forces that exceed the scope of the internal system.

Such is the case with the Roamr platform. Roamr aims to add value by allowing Roamr users to interface with a blockchain in ways that are seamlessly integrated into the fundamentally private Roamr drone ecosystem.

![Public Ledger For Private Systems](./_assets/private_org_public_chain.png)

Public blockchain-based operations can occur before, during, or after private operations. The interplay between private and public elements can serve to validate, stamp, record, or enhance processes within an ecosystem.

The ethos of this model is making processes more robust by tapping into the benefits of blockchain technology specifically where it can produce the most positive impact. While this hybrid framework may not be applicable to all platforms, Roamr focuses on creating technology for the cases in which goods can be convyed with the use of drones.

### Architecting for Adoption
Roamr differs from many existing blockchain initiatives, because it can exist independently and layer around new or existing systems without requiring systemic change. Rather than replace, Roamr aims to augment. Developers and enterprises can access blockchain capabilities easily by hooking into standard APIs via the Roamr platform to book request of a delivery service via

![Roamr Platform](./_assets/Roamrgen_platform.png)

The scope of an AI + Blockchain powered flights services platforms that can leverage Roamr is broad. This is new system that can power virtually any experience, house any number of drone services, perform any private data operation, and deploy in any environment. This is enabled by Roamr's structural modularity and is synergistic with Roamr, acting as a complementary driver of adoption.

## Selfdrop
Built on top of the Roamr public ledger is a blockchain-based authentication service, called Selfdrop. This offers a layer of security that helps verify if an access request is coming from an appropriate source.

Roamr Selfdrop offers a way to enhance off-chain authentication protocols by incorporating blockchain mechanics as a component of a single- or multi-factor authentication process. This can add a useful layer of security to help thwart system breaches and data compromises.

Before examining technical aspects of Selfdrop, let’s first take a look at the problem it is trying to solve.


### Server-Side Selfdrop

#### Overview
Server-Side Selfdrop is predicated on harnessing that unpredictability. The main driver of the protocol is the validation of very specific request that occur on the blockchain. Every Server-Side Selfdrop authentication request is unique and virtually impossible to have occurred by chance.

Server-Side verification with Selfdrop is analogous. Rather than sending the user a request for a delivery service, Roamr defines a request and the user must execute it from the client-side application. The only way the user can conduct a valid request is by having direct access to the application in question.

#### Benefits from the Blockchain
Because the ledger is on a decentralized and distributed network, request processing is not subject to single points of failure. This on-chain activity provides a reasonable failsafe that the requesting aspect of Server-Side Selfdrop can remain functional without dependence on any trusted party.

The public nature of the ledger also ensures that any party (be it a private system or a verified user) can monitor authentication attempts - from anywhere in the world, in real time. Such a level of transparency is a substantial advantage, particularly when it pertains to authenticating systems that house sensitive and expensive data.

#### A Detailed Look
There are four entities involved in the Server-Side Selfdrop authentication process:

1. Accessor - The party attempting to gain access to a System.
2. System - The system or gateway that is being accessed by the Accessor.
3. Roamr API - The module that is utilized by the System to process information as well as communicate and interface with the blockchain.
4. Blockchain - The distributed public ledger that processes Roamr requests and executes Roamr smart contracts, through which information may be pushed, pulled, or otherwise operated upon.

Each Server-Side Selfdrop transaction, in its entirety, is a set of five parameters:

1. Sender - The address that must initiate the request.
2. Receiver - The requester destination. This corresponds to a method in a Roamr smart contract on the blockchain.
3. ID - An identifier that is associated with the System.
4. Quantity - A precise number of Roamr to send.
5. Challenge - A randomly generated alphanumeric string.

Below is an outline of the Server-Side Selfdrop authentication process, which can be generally classified into three stages:

1. Initialization
2. Authentication Attempt
3. Validation

Initialization begins with a System (e.g. Roamr) registering to use Roamr and obtaining credentials, enabling the system to communicate with the blockchain via the Roamr module. The System onboards an Accessor (e.g. a Drone institution) who registers a public address, and then passes the registered address to Roamr. This address is immutably written onto the blockchain to a whitelist stored in a Roamr smart contract. The System receives a confirmation that the address was whitelisted, which can also be verified as a publicly viewable event. System registration need only occur once, while Accessor whitelisting need only occur once per Accessor.

![Server-Side Selfdrop: Initialization](./_assets/Server-SideSelfdrop_Initialization.png)

After Initialization is complete, the core of the Roamr authentication process can begin. The Accessor, who must execute a transaction, jumpstarts this process by requesting the requester details from the System, and the System routes the request to the Roamr API. The Roamr API generates a new set of request parameters, stores certain details immutably on the blockchain, and returns the full details to the Accessor via the System. The Accessor, equipped with all required information, conducts a request from the registered address to a method in the Roamr smart contract. If the sender's address is not whitelisted, the action is rejected - otherwise, it is recorded in the smart contract. It is important to note that this request should occur outside of the System, directly from the Accessor to the Blockchain, as it requires the Accessor's private key (which only the Accessor should be able to obtain).

![Server-Side Selfdrop: Authentication Attempt](./_assets/Server-SideSelfdrop_AuthenticationAttempt.png)

The final step of the process is Validation. In this step, the Accessor officially requests access to the System via the System's established mechanism. Prior to implementing any of its standard authentication protocols, the System asks Roamr whether or not the Accessor has performed a valid transaction. Roamr interfaces with the smart contract, checks for validity, and responds with a true/false designation. The System is able to decide how it should proceed based on this designation - if it is false, the System can deny access, and if it is true, the System can grant access.

![Server-Side Selfdrop: Validation](./_assets/Server-SideSelfdrop_Validation.png)

If we consider the base System credentials - or whatever existing System protocols that are in place - to broadly be one layer of authentication, it is critical that the Roamr authentication layer provides a useful second layer. By examining the two primary attack vectors, we can readily confirm its usefulness:

- Vector 1 - Attacker steals the Accessor's base System credentials
  - Attacker attempts to gain access to the System with valid System credentials
  - System checks with the Roamr API to determine if a valid transaction was made on the blockchain
  - The Roamr API returns false, and the System denies access
- Vector 2 - Attacker steals the private key(s) to the Accessor's wallet
  - Attacker attempts to conduct a transaction from the registered address, without other required requester details
  - Attacker cannot make a valid blockchain transaction
  - Attacker also cannot request access to the System without the proper System credentials

It is clear that the Attacker must steal both the base System credentials and the Accessor's private wallet key(s) in order to access the System. In this regard, Roamr has successfully added an additional layer of authentication.

#### Case Study: Server-Side Selfdrop & Roamrgen
There are dozens of ways Server-Side Selfdrop can be used by private organizations. APIs, databases, and networks have created elaborate systems of tokens, keys, apps, and protocols over the last decade, in an attempt to secure sensitive data. Server-Side Selfdrop can be used alongside all of these more traditional off-chain mechanisms.

As an example, here is a overview of how Server-Side Selfdrop authentication fits into the Roamr API platform:

1. Roamr API partners must first have the IP addresses of various drones whitelisted.
2. Partners must request to whitelist a public Roamr address.
3. All calls to the Roamr APIs and transfers of data are encrypted and transmitted through the HTTPS protocol.
4. Developers must complete a valid Selfdrop request from the registered Roamr address.
5. Developers then perform OAuth 2.0 validation. OAuth (Open Authorization) is an open standard for token-based authentication and authorization. Roamr supports the “Resource Owner Password Credentials” and “Client Credentials” grant types, and each API user must provide credentials for an authentication request.
6. The Roamr developer is granted a unique OAuth 2.0 token, to be passed in the header for subsequent API calls.
7. The token is valid for 24 hours, after which the developer must validate again and get a new token.

If any of these steps is violated, the user is immediately locked from API access. A hacker cannot bypass all of these security factors by guessing randomly, because there are trillions of unique combinations.

Roamr blockchain-based authentication is an important component of the Roamr security protocol. The Roamr team encourages partners to set up multi-signature wallets, and store private keys in multiple secure locations independently from other credentials, so there is not a single point of failure. A properly secured multi-signature wallet is not only difficult to steal, but the public nature of the blockchain also allows for swift recognition of any theft as it relates to the security of the API.

Anyone can view an authentication attempt to the Roamr smart contract, which means the days of platforms being compromised for months on-end can be a thing of the past. API hackers can now be thwarted with more immediacy because of the ability to detect unexpected authorization attempts in real-time, from anywhere in the world.
### Client-Side Selfdrop

#### Overview
The on-chain transactions that drive Server-Side Selfdrop provide a robust vector for authentication, but they also come with a cost (namely, the cost of gas required to power computations on the underlying Ethereum network). Said cost applies for each authentication instance, and thus naturally lends itself to use cases in which authentication attempts are fairly infrequent. This is why Server-Side Selfdrop is well positioned for large-scale systems that typically do not have multitudes of end users who must be authenticated.

When considering consumer-facing applications that do have multitudes of end users and a high volume of demand for authentication (for example, each time a user logs in to a website), on-chain transaction-based authentication becomes expensive and potentially burdensome. Client-Side Selfdrop is the answer to this problem.

The Client-Side Selfdrop authentication process occurs primarily off-chain and is then verified on-chain, limiting gas costs while still providing a useful authentication framework. Rather than validating that a user is the owner of a particular address, Client-Side Selfdrop validates that a user owns a particular device, in the same vein as traditional mobile-based Two-Factor Authentication (2FA).

#### Benefits from the Blockchain
The on-chain component of Client-Side Selfdrop references user information that is stored in a smart contract. Because this information is immutably persisted on a public ledger, both applications and end users can be confident in the integrity of that information. This on-chain storage is an example of how processes can successfully have both private and public layers - certain information can be moved on-chain in order to decrease the reliance on trusted parties and increase transparency.

Client-Side Selfdrop also leverages cryptography-based computations that are housed on-chain. Rather than depending on a traditional web server infrastructure, those computations are distributed and parallelized via the Ethereum Virtual Machine and thus have less opportunity for failure. This brings a further element of decentralization to the authentication process.

#### A Detailed Look

There are five entities involved in the Client-Side Selfdrop authentication process:

1. User - The party to be authenticated.
2. Application - The platform that a User is to be authenticated for.
3. Roamr Mobile App - An interface that ties data to a User and communicates with the rest of the ecosystem on behalf of the User.
4. Roamr API - The module that is utilized by the Application and the Roamr Mobile App to process information as well as communicate and interface with the blockchain.
5. Blockchain - The distributed public ledger that processes Roamr transactions and executes Roamr smart contracts, through which information may be pushed, pulled, or otherwise operated upon.

The full authentication workflow is described below, and can be generally broken down into three stages:

1. Initialization
2. Authentication Attempt
3. Validation

Initialization includes User setup and a subsequent connection between User and Application. To get set up for Client-Side Selfdrop, a User first needs to generate a seed from which a Roamr wallet can be created. This seed generation process takes place within the Roamr Mobile App using entropy (i.e. random data) supplied by the User in conjunction with entropy supplied by a cryptographically secure random number generator. Along with a wallet seed, a randomly generated alphanumeric Roamr ID is also generated for the User. This Roamr ID is a public unique identifier for the User within the Roamr ecosystem.

These details are routed through the Roamr API and stored on-chain in a Roamr smart contract, and the unique Roamr ID is displayed to the User. The User is then able to provide the Roamr ID to the Application, which the Application in turn uses to register a link between itself and the corresponding User via the Roamr API. Finally, the Roamr API communicates any resulting link to the Roamr Mobile App and the authentication process is ready to begin.

![Client-Side Selfdrop: Initialization](./_assets/Client-SideSelfdrop_Initialization.png)

Once the initial set up is complete and a link is established, the User is able to attempt an authentication when prompted by some triggering event. This trigger can be anything, with a common example being a login attempt on the Application's login portal. To kickstart the authentication attempt, the Application generates a message and relays it to the User. The User then inputs the message in the Roamr Mobile App, where it is signed with the User's private key (stored locally on the User's device, inside the Roamr Mobile App). The signed message is routed to the Roamr API and it is ready to be validated.

![Client-Side Selfdrop: Authentication Attempt](./_assets/Client-SideSelfdrop_AuthenticationAttempt.png)

The final stage in the process is the validation of the User's signed message. The Application requests a validation via the Roamr API, and the signed message is sent to the blockchain. A function housed in a Roamr smart contract then recovers the public address from the signed message. If this public address corresponds to the Roamr ID of the appropriate User (which was recorded during the initial setup phase), the validation is successful. Depending on the result of the validation, the Application can allow or disallow the User from proceeding.

![Client-Side Selfdrop: Validation](./_assets/Client-SideSelfdrop_Validation.png)

### Opening Selfdrop To The Public
While these blockchain-based authentication frameworks were initially architected to help secure the Roamr API ecosystem, they are widely applicable to many different kinds of platforme. Because we feel that others can potentially benefit, we are opening it up for use.

Just as Roamr will integrate the Selfdrop technology, any platform can add it to existing procedures and protocols. Formal documentation is available for those who wish to incorporate this blockchain layer to make their systems more secure, and Roamr more generally is developed with openness in mind. We invite developers around the world to contribute new ideas, extend our protocols, and innovate with novel use cases for the Roamr technology for drone services and beyond.

## Risks
Much like any nascent technology, such as the early days of social media, email, and streaming applications (which were reliant on dial-up connectivity), it is important that the core development team closely track new developments in Ethereum transaction speeds and volumes. Could you imagine YouTube attempting to launch in 1995? Or Instagram being first offered on the Blackberry?

Core Ethereum developers such as Vitalik Buterin and Joseph Poon have proposed the [Plasma: Scalable Autonomous Smart Contracts](https://plasma.io/plasma.pdf) upgrade to the Ethereum protocol:

> Plasma is a proposed framework for incentivized and enforced execution of smart contracts which is scalable to a significant amount of state updates per second (potentially billions) enabling the blockchain to be able to represent a significant amount of decentralized Drone applications worldwide. These smart contracts are incentivized to continue operation autonomously via network transaction fees, which is ultimately reliant upon the underlying blockchain (e.g. Ethereum) to enforce transactional state transitions.

Others, such as The Raiden Network, have proposed an off-chain scaling solution designed to power faster transactions and lower fees. At this time, the Selfdrop will put very minimal strain on the Ethereuem framework, thus scalability is a very small risk to the success of the technology.

## Conclusion
The immutability of a public blockchain offers new ways to enhance security of private systems like APIs.

This paper has shown three important things:

1. Public blockchains can add value in drone services.
2. The Roamr Selfdrop can enhance security of private systems.
3. There are immediate applications of the Roamr Raindrop within the Roamrgen API platform.

The Roamr team believes the framework set forth can be the standard security infrastructure for a new model of hybrid private-public systems, which will benefit all stakeholders in the drone services industry and beyond.
























Sources:

- Ethereum; [Merkling in Ethereum](https://blog.ethereum.org/2015/11/15/merkling-in-ethereum/)
- Trend Micro; [What Do Hackers Do With Your Stolen Identity?](https://www.trendmicro.com/vinfo/au/security/news/cybercrime-and-digital-threats/what-do-hackers-do-with-your-stolen-identity)
- Javelin Strategy & Research; [The 2017 Identity Fraud Study](https://www.javelinstrategy.com/press-release/identity-fraud-hits-record-high-154-million-us-victims-2016-16-percent-according-new)
- Symantec; [Internet Security Threat Report](https://www.symantec.com/content/dam/symantec/docs/reports/istr-22-2017-en.pdf)
- Risk Based Security; [2016 Data Breach Trends - Year in Review](https://pages.riskbasedsecurity.com/hubfs/Reports/2016%20Year%20End%20Data%20Breach%20QuickView%20Report.pdf)
- Thales; [2017 Thales Data Threat Report – Drone Services Edition](https://dtr-fin.thalesesecurity.com/)
- Apache.org; [Apache Struts 2 Documentation - S2-052](https://cwiki.apache.org/confluence/display/WW/S2-052)
- Joseph Poon and Vitalik Buterin; [Plasma: Scalable Autonomous Smart Contracts](https://plasma.io/plasma.pdf)
