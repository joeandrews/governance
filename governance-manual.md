# Aztec Governance Manual

# Background

The Aztec Network is a fully decentralized L2 on top of Ethereum, run by a permissionless network of sequencers who propose and attest to transactions, and provers who submit rollup proofs. Sequencers also serve as core governance actors, proposing, signaling, voting on, and executing network upgrades.

The purpose of Aztec governance is to remove control over the network from the hands of any single entity or individual. Decentralized sequencing, proving, and governance are hard-coded into the base protocol so that no central actor can unilaterally change the rules, censor transactions, or appropriate user value. Multi-stakeholder governance reduces platform risk, improves capture-resistance, and gives builders and users credible assurances that the network will not turn against them as it grows. By distributing upgrade authority across sequencers and tokenholders and grounding all decisions in open, transparent processes (AZIPs and AZUPs), Aztec aims to align long-term protocol stewardship with the people who depend on it, while still enabling efficient, scalable decision-making about upgrades, parameters, and resources.

# Governance Components

- **AZIPs** – Offchain, version-controlled design documents that specify proposed changes to Aztec’s protocols, standards, or governance processes, serving as the canonical record of what should change and why.
- **AZUPs** – Onchain upgrade bundles that package one or more AZIPs, plus their associated payload, into a concrete proposal for execution on the Aztec Network.
- **Payloads** – Series of onchain commands that execute against protocol contracts (or update contract references) detailed by an approved AZUP.
- **Signaling** – The process by which sequencers express support for an AZUP’s payload onchain; once a payload receives signals in at least 600 of 1,000 eligible slots, it is promoted to a formal onchain proposal.
- **Onchain Proposals** – Governance objects created once a payload meets sequencer signaling thresholds, defining a specific upgrade that tokenholders can vote to accept or reject.
- **Voting** – The onchain decision process in which eligible tokenholders lock tokens to cast “yea” or “nay” votes on proposals, with quorum and supermajority requirements determining whether an upgrade is authorized to execute.

# Governance Bodies

## Sequencers

Sequencers in Aztec are block producers and core governance actors. By running a sequencer node and staking into a rollup, they both build blocks and help steer protocol upgrades. 

Before AZUPs are presented to tokenholders for voting, they must gather enough support from sequencers. Sequencers vote on the support of AZUPs via onchain signaling. 600 of 1,000 signals are needed from sequencers to advance an AZUP’s payload into the voting phase. This means sequencers collectively decide which AZUPs reach the formal voting stage.

A sequencer’s staked capital is its voting power. By default, their stake is delegated through the Governance Staking Escrow (GSE) to the rollup contract, which automatically votes “yea” on AZUPs that came through the sequencer signaling path. This means sequencers passively support all AZUPs unless they explicitly delegate away from the rollup to an address they control and vote directly via the GSE.

Sequencers are expected to monitor proposals from signaling through execution and upgrade their node software in sync with approved changes so that the network’s block production and protocol logic remain consistent.

## Core Contributors

Core Contributors are individuals who actively and meaningfully contribute to the development, security, or governance of the Aztec Network. Participation is informal and merit-based — there is no fixed list or formal appointment process. Core Contributors coordinate around reviewing AZIPs, evaluating the implications of their design and implementations, and deciding which proposals should advance onchain to sequencers for signaling and to tokenholders for voting. They help ensure that AZIPs preserve coherence, security, and alignment with the network’s long-term roadmap prior to network-wide ratification.

### Core Contributor Weekly

The Core Contributor Weekly is a call held once a week to discuss various initiatives involving the Aztec Network. The facilitator creates a GitHub Issue for each weekly call as the agenda. Anyone can propose agenda items by commenting on the issue. Agendas can be found on the [Issues tab](https://github.com/AztecProtocol/governance/issues).

If you would like to join the governance process and participate in the calls, join the [Aztec Governance Signal group](https://signal.group/#CjQKIMpN-zgok2fW3PQQlv8PovOvHl47t8_P8dUpN1_SvMBpEhC6zgiyP2IiI6cbQtsN6dle).

*Facilitator* → [Koen van Marrewijk](https://github.com/koenmtb1) (Mar ‘26) [koen@aztec.foundation](mailto:koen@aztec.foundation)

## Tokenholders

Tokenholders play a direct role in Aztec governance by staking their tokens and participating in onchain votes. Any tokenholder may participate in governance votes by connecting their wallet to the governance dashboard, depositing into the Governance contract, and locking tokens for at least 38 days to ensure proposals can be executed before they exit. 

Once a proposal is created by sequencers, all eligible stakers can vote during a 7-day voting window, following a fixed timeline that includes an initial waiting period, a voting period, an execution delay, and a final grace period for execution. For a proposal to pass, at least 20% of the total voting power in governance must participate, at least 66% of votes cast must be yea, and at least 100,000,000 tokens must be deposited in governance.

Aztec Labs and Aztec Foundation teams, as well as investors, are excluded from staking and governance for the first 12 months (including the TGE vote), with their tokens locked for 1 year and then vesting over the following 2 years. 

# Governance Process

## AZIPs

AZIPs (AZtec Improvement Proposals) are version-controlled design documents that anyone can use to propose changes to the Aztec Network.

### Process

The current AZIP process is defined by the [AZIP Process](azip-process.md).

## AZUP

AZUPs (AZtec Upgrade Proposals) bundle the implementations of one or more AZIPs that Core Contributors have approved for inclusion in the Aztec Network. Once an AZUP is scheduled, its onchain payload is deployed to mainnet, where it is submitted to sequencers for signaling.

Once sequencers have signaled support for the deployed payload, anyone can promote it to a formal onchain governance proposal. After a 3-day review delay, the proposal enters a 7-day voting window where staked tokenholders vote “yea” or “nay”. If quorum, supermajority, and participation thresholds are met, the proposal passes. After a final 30-day execution delay, the proposal can then be executed by anyone within the grace period (`getGracePeriod()` in `Governance.sol`), implementing the AZUP onchain. If not executed within the grace period, the proposal expires.

### Process

The current AZUP process is defined by the [AZUP Process](azup-process.md).

```mermaid
flowchart LR
    A["AZIP\n(Specification)"] --> B["AZUP\n(Upgrade Proposal)"]
    B --> C["Payload Deployed\n(Ethereum Mainnet)"]
    C --> D["Sequencer Signaling\n(600/1,000 slots)"]
    D --> E["Proposal Created"]
    E --> F["Voting Delay\n(3 days)"]
    F --> G["Voting Period\n(7 days)"]
    G --> H{"Quorum 20%\nSupermajority 66%\nMin 100M tokens"}
    H -- Pass --> I["Execution Delay\n(30 days)"]
    H -- Fail --> J["Failed"]
    I --> K["Execution\n(Grace Period)"]
    K --> L["Upgrade Live"]
```

## Disputes and Disagreements

Disagreements are a natural part of governance. The AZIP and AZUP processes are designed to surface conflicts early and resolve them through transparent, structured mechanisms.

### Decision-Making in Core Contributor Calls

Core Contributors make decisions using rough consensus. If no significant objections remain after discussion, the proposal moves forward. When consensus cannot be reached, the facilitator may call a simple majority vote among participants present, recorded in the call notes. Decisions with significant impact should be deferred if key stakeholders are absent or discussion is unresolved. Disagreements may be revisited at a subsequent call if new information or arguments are brought forward.

### AZIP Disputes

AZIP editors are administrators, not decision-makers. They may not block proposals on substantive grounds. Substantive disagreements are resolved by Core Contributors during RFD review. When contributors fundamentally disagree on direction, they are encouraged to submit competing AZIPs. Rejected AZIPs cannot be reopened; a new AZIP must be submitted that references the original and explains what has changed.

### AZUP Disputes

If an AZUP fails signaling or voting, Core Contributors should engage with opposing stakeholders. A failed AZUP cannot be resubmitted. A new AZUP must address the feedback received.

The governance process provides multiple checkpoints for opposition: sequencers can withhold signals, tokenholders and sequencers can vote "nay" or re-delegate before the snapshot, and proposals that are not executed within the grace period expire. No single party can unilaterally force or block an upgrade.

### Undocumented Payloads

The governance process requires that all protocol upgrades follow the full AZIP and AZUP lifecycle before being submitted for sequencer signaling. Payloads that have not been reviewed by Core Contributors, documented as an AZUP, and published to the governance repository are not considered legitimate upgrade candidates. Sequencers who signal for undocumented or unreviewed payloads undermine the integrity of the governance process. If such a payload reaches signaling quorum and is promoted to a proposal, tokenholders are expected to vote "nay" and Core Contributors should publicly flag the proposal as having bypassed the established process. The governance framework exists to ensure that upgrades are transparent, well-reviewed, and broadly supported before reaching the network.

### General Principles

- Disputes should be conducted publicly in GitHub PRs, Discussions, or the Aztec forum.
- Decisions that reject or block a proposal must include documented rationale.
- The governance process, not any individual or group, is the final arbiter.

