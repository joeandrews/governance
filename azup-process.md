# AZUP Process

---

The AZUP (AZtec Upgrade Proposals) process defines how protocol upgrades are proposed, evaluated, and adopted by the network. AZIPs approved for inclusion by Core Contributors are packaged into AZUPs - Aztec Upgrade Proposals. AZUPs are composed of on-chain “payloads” that sequencers signal support for. Payloads that meet the support quorum are promoted into formal proposals that tokenholders vote on. If approved, the upgrades are executed after built‑in safety delays.

For proposed changes to be considered for signalling by sequencers and voting by token holders, it must be formally proposed via the AZUP process. This document provides an overview of that end‑to‑end process, focusing on the roles of key stakeholders and the lifecycle of a proposal.

# Core Contributors

The Core Contributors are a representative governance body that serves as the primary steward of the Aztec Network, providing a structured locus for review and coordination around proposed changes. Core Contributors are responsible for reviewing AZIPs, evaluating the implications of their design and implementations, and deciding which proposals should advance onchain to sequencers for signalling and to tokenholders for voting. Core Contributors are responsible for ensuring that AZIPs preserve coherence, security, and alignment with the network’s long‑term roadmap prior to network‑wide ratification.

### Evaluation Criteria

**Security Considerations**

- The proposal has been thoroughly audited by a qualified team, with all reported issues of medium or higher severity either resolved or acknowledged.
- The proposal has undergone comprehensive testing.
- An operational plan, including automated testing and safety checks, is in place.
- All known potential risks have been provably mitigated.

**Strategic & Operational Considerations**

- The proposal advances the protocol and responsibly supports the growth of the Aztec ecosystem
- The impact of the proposal has been fully and transparently considered by and communicated to relevant ecosystem stakeholders.
- The proposal minimizes complexity, encapsulates new features as much as possible, and maintains credible neutrality.
- The proposal does not compromise the overall security or reliability of the protocol.
- The proposal has adhered to the established protocol governance process.
- The development of the proposal has been conducted transparently alongside the broader Aztec ecosystem.

### Current Members

- Client teams → Aztec Labs (rep: TBD)
- Ecosystem → Alejo Amiras
- Noir → Savio Sou
- Sequencers → Koen van Marrewijk
- Founders → Joe Andrews and Zac Williamson

All representatives, including Founders, can be changed via an AZIP proposal put forward by anyone.

### Core Contributor Weekly

The Core Contributor Weekly is a call held once a week to discuss various initiatives involving the Aztec Network. Agendas can be found on the [Issues tab](https://github.com/AztecProtocol/governance/issues).

If you would like to join these calls as an observer please contact the current facilitator.

*Facilitator* → [Koen van Marrewijk](https://github.com/koenmtb1) (Oct ‘25) [koen@aztec.foundation](mailto:koen@aztec.foundation)

### Reporting

Core Contributors will be responsible for publishing AZUPs (using the AZUP Template) to the governance repository after they are scheduled for inclusion. These AZUPs should include:

- List of approved AZIPs
- Overview of payloads to be proposed
- Scheduled date for deployment

# Stages

## Proposing

AZUPs bundle the implementations of one or more AZIPs that Core Contributors have approved for inclusion in the Aztec Network. Once an AZIP has been deemed properly specified, it is submitted by AZIP editors to the Core Contributors for final review. Core Contributors discuss AZIPs on a weekly basis during the Core Contributor Weekly call. AZIPs are bundled together by Core Contributors into AZUPs. AZUPs include:

- List of AZIPs scheduled for inclusion
- Payload(s) for any onchain changes

Core Contributors will publicly report on the outcome of their weekly meetings and provide evaluations for each AZIP. 

### **Payloads**

For each scheduled AZUP, Core Contributors deploy one or more payload contracts on Ethereum mainnet that encode the exact on‑chain changes (via functions such as ‎`getActions()`). The payload’s `getActions()` function describes the exact sequence of contract calls that will occur if governance approves the upgrade. The payload is the single source of truth for what an approved proposal will actually do on-chain. Once the payloads are deployed and announced to the network, the AZUP is considered formally proposed and ready for sequencer signaling. Each payload must be merged into the AZIP repository in the `/AZUPs` folder and tagged with the corresponding AZIP/AZUP before deployment.

Once payloads are deployed, Core Contributors publish a forum post following the AZUP Template. For more on how payloads are structured, see [sequencer documentation](https://docs.aztec.network/network/operation/sequencer_management/creating_and_voting_on_proposals).

## Signalling

After Core Contributors have approved an AZUP for inclusion, the deployed payload is sent to sequencers for **signalling**. The AZUP process uses a signaling phase to **gauge sequencer support** before a formal proposal is created. Signalling serves as an on-chain mechanism for sequencers to express coordinated support for a specific upgrade candidate. Sequencers signal support for payloads by configuring their nodes to include the payload address in block proposals. Each successful signal increments that payload’s support count for the current round.

Sequencers are delegated through the Governance Staking Escrow (GSE) to the rollup contract by default, which automatically votes “yea” on AZUPs that came through the sequencer signaling path. Sequencers that want to vote “nay” or split their voting power must delegate away from the rollup to an address they control and vote directly via the GSE.

A payload reaches signaling quorum when it accumulates at least **600 signals within a 1,000‑slot round** (60% of the round’s total possible signals). Payloads that meet this threshold are eligible to be promoted into formal proposals.

For more on how signalling works, see [sequencer documentation](https://docs.aztec.network/network/operation/sequencer_management/creating_and_voting_on_proposals).

## Voting

Once a payload reaches quorum among sequencers, it must be voted on by tokenholders to be approved for execution. To do so, the payload must be turned into an onchain proposal.

### **Creating the Proposal**

After signalling meets quorum with sequencers, any account can call a function such as `submitRoundWinner(roundNumber)` on the Governance Proposer contract. The Governance Proposer verifies the winning payload for that round and forwards it to the Governance contract.

The Governance contract:

- Registers the new proposal with a reference to the payload.
- Records the creation timestamp.
- Derives and stores the voting start and end times according to protocol parameters.

At this point, the payload transitions from a “signaled upgrade candidate” to a tracked proposal within the governance system.

### **Preparing for the Vote**

Following proposal creation, there is a **3‑day voting delay** in which the proposal is pending and votes cannot yet be cast. This delay is designed to:

- Give the community time to review the payload and its implications.
- Allow node operators to assess operational impact.
- Provide sequencers with an opportunity to adjust their delegation if they want to vote differently from the default rollup behavior.

**Custom Voting via Delegation to a Controlled Address**

During this time, any sequencer who would like to vote *without following the canonical rollup* (e.g. vote “nay”) must delegate their stake to an address they control. This removes their stake's voting power from the rollup's control and gives it to the chosen address.

See [Custom Voting](https://docs.aztec.network/network/operation/sequencer_management/creating_and_voting_on_proposals#custom-voting-delegating-to-your-own-address) for instructions on how to re-delegate stake.

### **Voting**

After the waiting period expires, the proposal enters a 7-day voting period. At the moment voting begins:

- The system snapshots voting power derived from staked tokens.
- The delegation state at that time determines who holds voting rights for the proposal.
- Any delegation changes made *after* voting has started do not affect voting power for that active proposal.

To participate in voting, eligible tokenholders can stake and lock their tokens to vote via [the governance dashboard](https://stake.aztec.network/governance). All staked tokens will be locked for at least 15 days to ensure the proposal can be executed before the voter exits.

**Default Voting via Rollup Delegation**

By default, tokenholders staked as sequencers delegate their voting power to the rollup contract through the GSE (Governance Staking Escrow). The rollup automatically votes "yea" on proposals created through the `GovernanceProposer` using **all** delegated stake from **all** sequencers in that rollup. To vote “nay” on a proposal, sequencers must delegate their stake to an address they control **before** the snapshot is taken.

**Voting Requirements**

A proposal is approved only if it satisfies all of the following:

- **Quorum:** At least **100M tokens** must participate in the vote.
- **Supermajority:** At least **66%** of the participating voting power in the proposal votes “yea.”
- **Participation Floor:** An amount of voting power equivalent to at least **500 validators** participates, ensuring a broad enough security base.

If these thresholds are not met, the proposal fails and does not proceed to execution.

### Execution

When a proposal receives sufficient support, it passes. After passing, there's another mandatory 7‑day delay before the proposal becomes executable. 

This delay serves several purposes:

- Gives node operators time to deploy and test updated node software that is compatible with the upcoming changes.
- Allows the community to perform final checks and monitoring.
- Reduces the risk of sudden, uncoordinated changes that could disrupt the network.

In practice, operators may run multiple node versions during this window (for example, one on the current version and one on the upcoming version) to ensure a smooth transition.

Once executable, anyone can trigger execution. See [Executing Proposals](https://docs.aztec.network/network/operation/sequencer_management/creating_and_voting_on_proposals#executing-proposals) for instructions on how to execute proposals. If not executed during this window, the proposal may expire or require re‑submission, depending on protocol rules.

## Core Components

### Payload

A payload is an Ethereum smart contract that encodes a specific protocol upgrade. It implements a function (e.g. `getActions()`) that returns a list of on-chain actions the governance contract should perform if the proposal passes. These actions may include updating protocol contract references, modifying registries, or enabling new features.

Before a payload is used in governance, participants are expected to:

- Verify the payload contract address on a block explorer.
- Inspect the `getActions()` implementation to understand exactly what will change.
- Check whether the payload has undergone audits where relevant.
- Discuss the proposed upgrade in public community channels.

The payload is the single source of truth for what an approved proposal will actually do on-chain.

### Governance Proposer

The Governance Proposer contract manages the signaling phase and the transition from informal support to formal proposals. It:

- Accepts signals from sequencer nodes for specific payloads.
- Aggregates signal counts within fixed-length rounds.
- Identifies payloads that reach quorum within a round.
- Allows anyone to promote a round’s winning payload to a proposal in the Governance contract.

The Governance Proposer acts as the bridge between sequencer sentiment and the formal governance layer.

### Governance Contract

The Governance contract is the canonical record of proposals and their state. It is responsible for:

- Tracking proposal metadata (payload address, creation time, voting schedule).
- Enforcing voting delays and voting periods.
- Recording vote tallies (“yea” vs “nay”).
- Evaluating whether proposals have met quorum and supermajority requirements.
- Executing an approved proposal’s payload actions after the required delay.

### Governance Staking Escrow (GSE)

The Governance Staking Escrow holds staked tokens and manages voting power. It:

- Custodies stake used for securing the network.
- Tracks and updates delegation relationships: which address controls voting power for a given stake.
- Exposes voting functions that allow delegatees to vote on proposals using their delegated voting power.

GSE is the mechanism that binds stake to governance rights.

### Rollup Contract

Sequencers stake into a rollup contract. By default, a sequencer’s voting power is delegated from the GSE to the rollup. The rollup can then cast votes on governance proposals on behalf of all stakers who remain delegated to it.

This default delegation path provides a straightforward way for sequencers to participate in governance without managing votes directly, while still retaining the option to override via custom delegation.

AZUP Template