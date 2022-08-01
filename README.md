 <img src="https://user-images.githubusercontent.com/136583/182129623-3bab6cb3-ef97-41bb-bfd1-39500e2bc3f5.png" width="120">
<br>

# Handshake Decentralized SLDs
```
Authors: Aaron Oxborrow, Chris Moos, Sam Ward
Published: 2022-08-01
```



    
## Abstract

Handshake is a decentralized naming protocol designed for managing TLDs (Top Level Domains). This document describes a new protocol for decentralized second-level domains (SLDs) anchored to the HNS root zone. These domains are based on the ERC-721 NFT standard and deployed on a secure, scalable, and inexpensive EVM L2 blockchain. 

The goal of this proposal is to define a new standard for Handshake SLDs that offers compelling advantages over traditional registry-based domains. Tokenized names allow novel ownership models including self custody, multi-signature security, DAOs, and fractionalization. NFT domains can be efficiently traded on NFT marketplaces resulting in an active aftermarket without the need for auth codes or escrow middlemen. These domains support all traditional DNS records but can also resolve new types of resources like crypto wallets and decentralized content storage (e.g. IPFS). This enables innovative new use cases for domains such as personal digital identities and payment address identifiers. 

This document also describes a method in which existing domain registrars like Porkbun, Namecheap, and Encirca can begin offering tokenized domains to their customers with little change to their existing systems. The enhanced utility of NFT domains combined with appealing new Handshake TLDs, widely promoted by the popular domain registrars should drive increased end user adoption of Handshake domains.



## Motivation

This proposal addresses many of the issues with existing non-blockchain SLD registry solutions:

* Registry SLDs have the same or lesser functionality of ICANN domains but without any protections
* Registry SLDs are subject to legal exposure and censorship
* Registry SLDs rely on centralized servers to resolve
* Registry SLDs have limited or non-existent aftermarket options
* Registry SLDs are missing the added functionality and use cases of NFT domains

This proposal offers many benefits to Handshake TLD owners:

* TLD owners get full control of pricing and distribution methods
* TLD owners can benefit from sales by traditional registrars
* TLD owners get recurring SLD fees streamed directly to their wallet
* TLD owners can configure SLD aftermarket royalties using EIP-2981
* TLD owners do not have to lock their TLDs forever, preventing upgrades

<br>

# Fees & Dev Fund

A multisig wallet controlled by trusted Handshake developers and stakeholders will be designated as the Handshake Dev Fund. Fees collected from SLD registrations, renewals, and aftermarket royalties will be collected into the wallet to fund Handshake core development. The following rules are enforced where possible: 


* **SLDs must cost at least $1/yr** - this hopefully reduces the number of spammy registrations, and ensures Dev Fund always gets something.
* **5% or $0.50 (whichever is greater) of SLD registration/renewal to Dev Fund**
* **(Optional) 5% royalties from aftermarket sales to Dev Fund.** This will be the default, but TLD owners can override this in their TldPricingStrategy. Since EIP-2981 royalties cannot be split, Dev Fund will not receive any royalties in that case. 
* **Claiming and Minting TLDs cost $100/ea** - this hopefully increases the quality of TLDs that are bridged and reduces the number of TLDs that need to be managed and renewed on Handshake side 

<br>

# NFT Metadata

Metadata is additional info provided in JSON format which is easily accessible by traditional web apps. For visual NFTs this includes the URL to the images themselves, along with the various "traits" it might possess. For domain NFTs, this includes registration and expiration dates as well as an avatar image that is used by marketplaces. 

For Handshake NFTs, the default metadata will be served fully on-chain with an SVG representation of the avatar like those below. TLD owners can optionally specify their own Metadata implementation, which opens up the possibilities for custom avatars for TLD communities or even full NFT collections based around a TLD. The default avatars will use the strong black and white Handshake branding:

**<sub>Handshake SLD Avatar</sub>**
 
 <img src="https://user-images.githubusercontent.com/136583/182129858-444164e5-1025-42ab-81b1-7b13f132b3b6.png" width="295">

<br>

**<sub>Handshake TLD Avatar</sub>**
 
 <img src="https://user-images.githubusercontent.com/136583/182129887-c158eada-b80f-464b-b6be-a575844ef287.png" width="300">

*Example ENS Metadata for Vitalik.eth*
[https://metadata.ens.domains/mainnet/0x57f1887a8bf19b14fc0df6fd9b2acc9af147ea85/79233663829379634837589865448569342784712482819484549289560981379859480642508](https://metadata.ens.domains/mainnet/0x57f1887a8bf19b14fc0df6fd9b2acc9af147ea85/79233663829379634837589865448569342784712482819484549289560981379859480642508)

<br>

# DNS Resolution

Handshake NFT domains will support all valid DNS records via EIP-1185, (e.g. A, TXT, NS, DS, etc.).   Domain owners can choose to manage their DNS records on-chain, obviating the need for DNSSEC, or delegate to external nameservers like traditional domains. 

DNS resolution from Handshake clients can be accomplished with HIP-005. A new suffix may be required to support SLD resolution on chains other than mainnet Ethereum, e.g `._evm`, and the NS delegation would contain the EIP-155 Chain ID. For example, with Handshake TLD .foo, using Chain ID 10 (Optimism L2), and contract address `0x36fc69f0983E536D1787cC83f481581f22CCA2A1`, the record might be:


```
foo.  3600  IN   NS   10.0x36fc69f0983E536D1787cC83f481581f22CCA2A1._evm.
```


<br>

# EPP Extension for Registry Custody

We are developing an extension to the EPP protocol that allows registrars to sell Handshake decentralized SLDs and utilize Namebase Registry for custody. This takes advantage of existing EPP rails and puts dSLDs right into the traditional checkout flow. The domains will be minted directly to a holding contract, where they can be remotely managed by the registrar through standard EPP calls. Registrars will have the ability to transfer the domain to an external wallet at any time, for example when a customer wants to take ownership. Namebase Registry abstracts away the blockchain integration and covers all L2 gas fees. For custodial service Namebase Registry will charge a small fee (TBD) via regular EPP channels.


<br>

# Layer 2 

In order to bring traditional registrars into the world of decentralized domains, we need to manage on-chain domains quickly and cost effectively. Ethereum mainnet is becoming the global settlement layer, but Layer 2s are a better fit for our needs. While the additional complexity can be a hurdle, we think domain owners will ultimately appreciate the speed and savings.

**L2 Pros**
* Fast and inexpensive transactions for massive scaling potential
* Registry can potentially subsidize on-chain activities
* Security inherited from mainnet Ethereum

**L2 Cons**
* Bridging can be confusing and cumbersome. For example, after acquiring ETH from a fiat on-ramp like Coinbase, a separate step is required to bridge those funds over to the L2. Example of a bridge: [ https://app.optimism.io/bridge](https://app.optimism.io/bridge)
* For Polygon users a separate gas token ($MATIC) adds to the confusion.
* UX Friction when using parts of NFT ecosystem. For example switching networks in wallet. ([Screenshot of Metamask asking to switch to Polygon on OpenSea](https://user-images.githubusercontent.com/136583/182131613-70448cfe-050c-4812-abfc-2d110028488f.png))

<br>

## Optimism **\*\*Recommended\*\***
**Optimism is our choice for Layer 2.** The ecosystem is growing and the DAO is geared for the long haul. The bridge UI and developer documentation are a joy to use. We’ve been told OpenSea is already on testnet, but Quixotic is a capable marketplace regardless. We have a lot of Handshake connections in the OP world, including co-founder and longtime $HNS fan Mark Tyneway. (He wrote the [first ENS resolver plugin for HSD](https://github.com/tynes/hsd-ens-resolution) way back in 2020.)

Optimism is an L2 rollup chain, fully secured by ETH. Cheap fees and multiple bridge options. Lower selection of fiat on-ramps than Polygon. Some NFT marketplace support, OpenSea currently on testnet.

**Markets:** Quixotic, OptiMarket  
[https://www.optimism.io/](https://www.optimism.io/)  
[https://community.optimism.io/docs/developers/bridge/messaging/](https://community.optimism.io/docs/developers/bridge/messaging/)


## Arbitrum
Largest layer 2 rollup, fully secured by ETH. Cheap fees. Lower selection of fiat on ramps vs. Polygon. Some NFT marketplace support.

**Markets:** Stratos, Treasure, Tofu  
[https://arbitrum.io/](https://arbitrum.io/)  
[https://developer.offchainlabs.com/docs/bridging_assets](https://developer.offchainlabs.com/docs/bridging_assets)


## Polygon
Not secured by ETH as this is actually a side chain. Good fiat on-ramps and various NFT marketplace support including OpenSea. Gas fees are cheap but NFTs have a reputation as lower quality (lots of spam).

**Markets:** OpenSea, NFTrade, Refinable  
[https://polygon.technology/](https://polygon.technology/)  
[](https://docs.polygon.technology/docs/develop/ethereum-polygon/mintable-assets)


## zkSync
Platform just not very mature yet. Cheap fees. No price feeds / oracles. Poor NFT marketplaces. No fiat on-ramps.

**Markets:** zkNft, Mintsquare  
[https://zksync.io/](https://zksync.io/)  
[https://docs.zksync.io/dev/nfts/](https://docs.zksync.io/dev/nfts/)




<br>

# TLD Staking & Bridging

The challenge of building fully trustless and cross-chain TLD staking has so far been a major road block to creating decentralized SLDs for Handshake. While we haven’t yet cracked that nut, we believe it’s only a matter of time before a reliable solution is achieved. In the meantime, we want to move forward with dSLDs and try to make progress on multiple fronts.


## Phase 1: Namebase TLD Staking

The first phase is a custodied and thus fairly centralized solution. While we want to move quickly to bring dSLDs to fruition, we need to get things right before they are written in stone (or blocks). Using staked TLDs from Namebase is just the shortest path to a first iteration. Once the next phase is ready, TLDs can be moved to a more decentralized solution.

<br>

## Phase 2: Multisig TLD Bridge

A bridge between Handshake and EVM that relies on multi-signature wallets on each side consisting of trusted stakeholders in a 3/5 configuration. A simple majority of signers are required to execute a transaction. The signatures can be gathered over the course of days, so no coordination is required. The multisig could also own the contracts and manage the Dev Fund.  

The signers should be trustworthy stakeholders in the Handshake ecosystem who are willing to bear the responsibility and practice good operational security. This is similar to how ENS started before eventually forming their DAO, and it can be considered a bootstrapping mechanism to move things forward until other bridging solutions can be developed. A multisig is highly secure, and at least partially decentralized, which makes it less of a legal target. This solution compares favorably to the "permanent" staking of .forever domains, as it still allows TLD record updates and even unstaking. 


### Multisig TLD Bridge Signers *(WeCann?)*

I am just throwing these names out there, but ultimately we need willing participants!



* Chris Moos (Namebase / Namecheap)
* Matthew Zipkin (Lead Handshake Dev)
* Mike Carson (Impervious) 
* Thomas Barrett (Encirca)
* Ray King (Porkbun)


### TLD Bridge Staking & Approval

We’ll create a portal that helps TLD owners configure and stake their TLD, and subsequently claim ownership of the TLD NFT on the EVM side. The configuration is similar to [Impervious Registry](https://registry.impervious.com/stake), with slight modifications for specifying L2 Chain ID:

**NS Record** (HIP-005 compatible, see DNS Resolution above)

```
10.0x36fc69f0983E536D1787cC83f481581f22CCA2A1._evm.
```


**TXT Record**


```
eth-claim:10.0xE3D2b657c45613d30292Fd67Ef40DAB043B70706
```


The portal will verify the correct TLD configuration on Handshake, and then show the multisig wallet address where the owner needs to send the TLD. Transfers take 2 days to finalize, and this is the last step on the Handshake side.

The portal will have a dashboard that tracks all the TLDs in the multisig and their approval and claim status. The dashboard can be filtered for the current user. Once a TLD gets approved by the multisig, a “Claim” button activates and allows the owner to mint the TLD NFT. The mint costs $100/ea which goes entirely to the Dev Fund. 

The signers need to approve TLDs sent to the multisig. They can use the same portal dashboard to identify which TLDs are configured correctly and in need of approval. We could use the Gnosis Safe transaction builder and initiate a bunch of approve transactions in bulk. Then the other signers just need to access the multisig portal whenever possible and sign any pending approvals. [https://help.gnosis-safe.io/en/articles/4680071-transaction-builder](https://help.gnosis-safe.io/en/articles/4680071-transaction-builder)


#### Additional Considerations

1. TLDs in the multisig will need to be renewed indefinitely, or at least as long as they have active SLDs. This Handshake renewal transaction is cheap now, but may grow in cost over time. We may want to consider the cost of these heartbeat transactions and adjust the staking fee accordingly.
2. The multisig should control the Dev Fund. They can choose to put some of those funds towards staked TLD renewals. Or maybe buy and burn $HNS to further secure the root blockchain.
3. The multisig might want to police the quality level of TLDs, simply for the overhead cost of administrative time in the initial stage, and avoiding paying renewal for poor quality TLDs that aren’t attracting SLD registrations.
4. The multisig is not really tenable at large scale. The task of approving hundreds or thousands of TLDs will be time consuming in addition to other administrative decisions that may require coordination.


<br>

<br>

# Some Important Contracts

### TldManager
This contract controls which TLDs can be minted and claimed. A whitelisted set of TldProvider(s) addresses are able to add new TLD claims. Initially, Namebase or Multisig will be the only trusted TldProvider(s). Later on this role can hopefully be replaced by a trustless oracle which monitors the Handshake chain directly.

### RegistrationManager

The RegistrationManager controls the fees and conditions under which SLDs can be registered and renewed. This contract combines the required BasePricingStrategy and user-controllable TldPricingStrategy to come up with the SLD registration/renewal costs and fee distribution. Once a domain has been registered, its annual renewal price will never change. TLDs can only increase the price of registration and renewal for *new* SLDs.


### BasePricingStrategy
This enforces the minimum pricing requirements (see Fees above).

### TldPricingStrategy

This delegated contract will have a simple interface which takes the requested domain, registration period, requesting wallet and destination wallet, and returns the registration cost and fee distribution. This gives the TLD owner flexibility to delegate to their own custom pricing strategy contract and go nuts. The default contract will provide many built-in options for the most common pricing scenarios:

* variable cost based on character count of domain
* discount based on length of registration
* list of premium names and prices
* names reserved for specific wallet(s)
* discounts for specific wallet(s)

We will see interesting implementations like exclusive TLDs that don’t allow public minting, membership-gated TLDs, etc. etc. 



<br>
<br>


# Login with Handshake SDK

An SDK code library with documentation and examples for implementing **Login with Handshake.** This is different than the existing implementations using Bob wallet as it will use EVM compatible wallets for authentication, e.g. MetaMask or WalletConnect. We can support TLDs as identity in addition to SLDs. We could also provide an example OAUTH identity provider for implementing SSO.  

* EIP-4361: Sign-In with Ethereum
[https://eips.ethereum.org/EIPS/eip-4361](https://eips.ethereum.org/EIPS/eip-4361)
[https://login.xyz/](https://login.xyz/)

* Login with Unstoppable
[https://docs.unstoppabledomains.com/login-with-unstoppable/](https://docs.unstoppabledomains.com/login-with-unstoppable/)

<br>

<br>

# TBD: W3C Decentralized Identifiers (DIDs)

Determine what if any design considerations are needed for Handshake domains to be compatible as W3C DIDs.

* W3C DID Spec
[https://www.w3.org/TR/did-core/](https://www.w3.org/TR/did-core/)

* Draft EIP-1056: Lightweight Identity 
[https://github.com/ethereum/EIPs/issues/1056](https://github.com/ethereum/EIPs/issues/1056)

* "Ethr" DID Resolver
[https://github.com/decentralized-identity/ethr-did-resolver/blob/master/doc/did-method-spec.md](https://github.com/decentralized-identity/ethr-did-resolver/blob/master/doc/did-method-spec.md)


<br>

<br>


# References

* HIP-0005: Alternative Namespace Resolution
[https://hsd-dev.org/HIPs/proposals/0005/](https://hsd-dev.org/HIPs/proposals/0005/)

* EIP-721: Non-Fungible Token Standard + Metadata
[https://eips.ethereum.org/EIPS/eip-721](https://eips.ethereum.org/EIPS/eip-721)

* EIP-137: Ethereum Domain Name Service
[https://eips.ethereum.org/EIPS/eip-137](https://eips.ethereum.org/EIPS/eip-137)

* EIP-634: Storage of text records in ENS
[https://eips.ethereum.org/EIPS/eip-634](https://eips.ethereum.org/EIPS/eip-634)

* EIP-1185: Storage of DNS Records in ENS
[https://eips.ethereum.org/EIPS/eip-1185](https://eips.ethereum.org/EIPS/eip-1185)

* IANA DNS Records
[https://www.iana.org/assignments/dns-parameters/dns-parameters.xhtml#dns-parameters-4](https://www.iana.org/assignments/dns-parameters/dns-parameters.xhtml#dns-parameters-4)

* EIP-2981: NFT Royalty Standard
[https://eips.ethereum.org/EIPS/eip-2981](https://eips.ethereum.org/EIPS/eip-2981)

* OpenSea Metadata Standards
[https://docs.opensea.io/docs/metadata-standards](https://docs.opensea.io/docs/metadata-standards)

* EIP-155: Chain ID Standards
[https://eips.ethereum.org/EIPS/eip-155](https://eips.ethereum.org/EIPS/eip-155)

* BIP-0044: CoinTypes
[https://github.com/satoshilabs/slips/blob/master/slip-0044.md](https://github.com/satoshilabs/slips/blob/master/slip-0044.md)
