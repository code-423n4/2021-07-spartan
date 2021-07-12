# ✨ So you want to sponsor a contest

This `README.md` contains a set of checklists for our contest collaboration.

Your contest will use two repos: 
- **a _contest_ repo** (this one), which is used for scoping your contest and for providing information to contestants (wardens)
- **a _findings_ repo**, where issues are submitted. (We'll set that one up later.) 

Ultimately, when we launch the contest, this contest repo will be made public and will contain the smart contracts to be reviewed and all the information needed for contest participants. The findings repo will be made public after the contest is over and your team has mitigated the identified issues.

Some of the checklists in this doc are for **C4 (🐺)** and some of them are for **you as the contest sponsor (⭐️)**.

---

# Contest scoping

## 🐺 C4: Set up repos
- [ ] Create a new private repo named `YYYY-MM-sponsorname` using this repo as a template.
- [ ] Get GitHub handles from sponsor.
- [ ] Add sponsor to this private repo with 'maintain' level access.
- [ ] Send the sponsor contact the url for this repo to follow the instructions below and add contracts here. (Example message below)

> I just sent the invitation to access a private repo we'll use to scope your contest. Once you accept the invite (it should be in your email), the repo will walk you through what we need step-by-step.

- [ ] Delete this checklist and wait for sponsor to complete their scoping checklist.

# ⭐️ Sponsor: Provide marketing details

- [x] Your logo (URL or add file to this repo - SVG or other vector format preferred)
See SPARTA.svg in the root folder
- [x] Your primary Twitter handle
https://twitter.com/SpartanProtocol
@SpartanProtocol
- [x] Your Discord URI
Telegram is our main channel: 
https://t.me/SpartanProtocolOrg
We have a community-curated Discord but its in early stages:
https://discord.gg/wQggvntnGk
- [x] Your website
https://spartanprotocol.org/
- [x] Optional: Do you have any quirks, recurring themes, iconic tweets, community "secret handshake" stuff we could work in? How do your people recognize each other, for example? 
Anything Spartan themed. 'Join the shield wall'
- [x] Optional: your logo in Discord emoji format
We dont have one

Docs (we have community-curated documentation):
https://docs.spartanprotocol.org/

---

# Contest scope information

- Pool.sol : 365 lines : Holds Protocol Liquidity : High Importance
- Router.sol : 358 lines : Routes funds, Incentivises LP's : Medium Importance
- Dao.sol : 721 lines : Protocol Governance : High Importance
- Synth.sol : 229 lines : Holds Pool Tokens As Collateral : High Importance
- daoVault.sol : 95 lines : Holds User Funds : High Importance
- synthVault.sol : 257 lines : Holds User Funds : High Importance
- bondVault.sol : 158 lines : Holds User Funds : High Importance
- poolFactory.sol : 150 lines : Deploys Pools : Low Importance
- synthFactory.sol : 78 lines : Deploys Synths : Low Importance
- Utils.sol : 217 lines : Core Maths and helper functions: Medium Importance

The Pool contracts use Thorchain's continuous liquidity pools (CLP) model. https://docs.thorchain.org/thorchain-finance/continuous-liquidity-pools
Contract is designed with a security model of "Money in - Money Out"
Pool contains the core design for synthetic assets. Sparta into the pool and call mintSynth(), pool sends LP tokens to the synth contract. Synth contract will mint the relevant requested amount of synths and attribute that to the user, via mint(). Synths are swapped back to layer 1 assets via POOL function: burnSynth() by sending synth tokens to the SYNTH then calling burnSynth(). It will find all spare synth tokens on its address, burn them, then send the LP tokens back to the pool to also be burnt and attribute the user their fair share of the requested BEP20 asset. A realise() function burns excess LP tokens to ensure the revenue is going to the liquidity providers in the underlying pools instead of the un-owned LP tokens held on the SYNTH contract

Router contract facilitates movement of funds from users into pools, containing business logic for adding/removing liquidity, swapping and managing synths. Router also distributes fee rewards to curated pools with swaps.

Dao contract is the source-of-truth for the location of the ROUTER, UTILS, DAOVAULT, POOLFACTORY, SYNTHFACTORY as well as distributing rewards and managing how the system upgrades itself. It has goverance features that use a member's claim on BASE in each pool to attribute voting weight. The DAO can upgrade itself, as well as ammend some features in the BASE contract.

Synth contract contains logic and holds LP tokens and state. Minting synths requires the relevant POOL to send LP units to the SYNTH contract.

DaoVault contract holds user's funds and state for members

SynthVault contract holds user's funds and state for members

BondVault contract holds user's funds and state for members

Utils contract works as both a web3 aggregrator (one call that makes several EVM calls, returning objects), as well as the core arithmetic of the system.

Three Protocol Tokens:
- SPARTA as the base asset : ERC-20 + ERC-677 Standard
- Pool LP units issued as ERC-20 tokens. Identified by "tokenSymbol-SPP"
- Synthetic assets issued as ERC-20 tokens. Identified by "tokenSymbol-SPS"

Important Areas Of Concern:
- Pool contract - Draining liquidity
- Dao contract - Protocol manipulation
- SynthVault - Abusing rewards - can it be manipulated by flash loans? can funds be stuck?
- DaoVault - Abusing rewards and gaining protocol level control, can funds be stuck?
- BondVault - Leaving vault before bonding period is finished, can funds be stuck?
- Router - Fee Manipulation

---

# Contest prep

## 🐺 C4: Contest prep
- [ ] Rename this repo to reflect contest date (if applicable)
- [ ] Rename contest H1 below
- [ ] Add link to report form in contest details below
- [ ] Update pot sizes
- [ ] Fill in start and end times in contest bullets below.
- [ ] Move any relevant information in "contest scope information" above to the bottom of this readme.
- [ ] Add matching info to the [code423n4.com public contest data here](https://github.com/code-423n4/code423n4.com/blob/main/_data/contests/contests.csv))
- [ ] Delete this checklist.

---

# Sponsorname contest details
- TBD main award pot
- TBD gas optimization award pot
- Join [C4 Discord](https://discord.gg/EY5dvm3evD) to register
- Submit findings [using the C4 form](https://code423n4.com/YYYY-MM-sponsorName-contest/submit)
- [Read our guidelines for more details](https://code423n4.com/compete)
- Starts TBD XXX XXX XX 00:00 UTC
- Ends TBD XXX XXX XX 23:59 UTC

This repo will be made public before the start of the contest. (C4 delete this line when made public)
