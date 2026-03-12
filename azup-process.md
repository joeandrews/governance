# AZUP Process

*Note: AZIPs are improvement proposals (specifications). AZUPs are upgrade proposals (onchain execution).*

---

The AZUP (AZtec Upgrade Proposals) process defines how protocol upgrades are proposed, evaluated, and adopted by the network. AZIPs approved for inclusion by Core Contributors are packaged into AZUPs - Aztec Upgrade Proposals. Each AZUP is composed of a single onchain “payload” that sequencers signal support for. Payloads that meet the support quorum are promoted into formal proposals that tokenholders vote on. If approved, the upgrades are executed after built-in safety delays.

# Core Contributors

Once an AZIP reaches `RFD` status (see the [AZIP Process](azip-process.md)), it is submitted to Core Contributors for evaluation and potential inclusion in an AZUP.

Core Contributors review AZIPs, evaluate their design and implementation implications, and decide which proposals advance onchain. They ensure AZIPs preserve coherence, security, and alignment with the network's long-term roadmap. See the [Governance Manual](governance-manual.md) for full details on Core Contributor responsibilities.

### Evaluation Criteria

**Security Considerations**

- The proposal has been thoroughly audited by a qualified team, with all reported issues of medium or higher severity either resolved or acknowledged.
- The proposal has undergone comprehensive testing.
- An operational plan, including automated testing and safety checks, is in place (e.g. automated fork tests verifying state transitions, canary deployments, and rollback procedures).
- All known potential risks have been provably mitigated.

**Strategic & Operational Considerations**

- The proposal advances the protocol and responsibly supports the growth of the Aztec ecosystem.
- The impact of the proposal has been fully and transparently considered by and communicated to relevant ecosystem stakeholders.
- The proposal minimizes complexity, encapsulates new features as much as possible, and maintains credible neutrality.
- The proposal does not compromise the overall security or reliability of the protocol.
- The proposal has adhered to the established protocol governance process.
- The development of the proposal has been conducted transparently alongside the broader Aztec ecosystem.

### Reporting

Core Contributors will be responsible for publishing AZUPs to the governance repository after they are scheduled for inclusion. Each AZUP must follow the [AZUP Template](./AZUPs/template.md).

# Stages

## Proposing

Once Core Contributors approve AZIPs for inclusion, they are bundled into an AZUP. Each AZUP includes:

- List of AZIPs scheduled for inclusion
- Payload for any onchain changes

### Payload

For each scheduled AZUP, Core Contributors deploy a payload contract on Ethereum mainnet that encodes the exact onchain changes (via functions such as `getActions()`). The payload's `getActions()` function describes the exact sequence of contract calls that will occur if governance approves the upgrade. The payload is the single source of truth for what an approved proposal will actually do onchain. Once the payload is deployed and announced to the network, the AZUP is considered formally proposed and ready for sequencer signaling. Each payload must be merged into the governance repository in the `/AZUPs` folder and tagged with the corresponding AZIP/AZUP before deployment.

Once the payload is deployed, Core Contributors publish a forum post following the AZUP Template. For more on how payloads are structured, see [sequencer documentation](https://docs.aztec.network/network/operation/sequencer_management/creating_and_voting_on_proposals).

## Signaling

Signaling is an onchain mechanism for sequencers to express coordinated support for a specific upgrade candidate before a formal proposal is created. Sequencers signal support for payloads by configuring their nodes to signal for a specific payload address. A signal can be made for each slot that a sequencer is selected to propose checkpoints. Signaling is separate from checkpoint proposals. Each successful signal increments that payload's support count for the current round.

A payload reaches signaling quorum when it accumulates at least **`QUORUM_SIZE` signals within a `ROUND_SIZE`-slot round** (`GovernanceProposer.sol`). Payloads that meet this threshold are eligible to be promoted into formal proposals. Once a payload reaches quorum, it must be promoted to a formal proposal within `LIFETIME_IN_ROUNDS` (`GovernanceProposer.sol`). If not promoted in time, the quorum expires and signals must be gathered again.

For more on how signaling works, see [sequencer documentation](https://docs.aztec.network/network/operation/sequencer_management/creating_and_voting_on_proposals).

### Alternative Proposal Path

In addition to the sequencer signaling path, the Governance contract supports an alternative proposal mechanism (`proposeWithLock`) that bypasses sequencer signaling. This path requires the proposer to lock `proposeConfig.lockAmount` tokens for `proposeConfig.lockDelay` (both from `getConfiguration()` in `Governance.sol`). Once submitted, the proposal follows the same voting and execution process as any other proposal. This mechanism exists as a safeguard in case sequencer signaling is unable to function as intended.

The rollup does **not** automatically vote "yea" on proposals submitted via `proposeWithLock`. Sequencers and tokenholders must actively vote to pass these proposals.

## Voting

Once a payload reaches quorum among sequencers, it must be voted on by tokenholders to be approved for execution. To do so, the payload must be turned into an onchain proposal.

### Creating the Proposal

After signaling meets quorum with sequencers, any account can call a function such as `submitRoundWinner(roundNumber)` on the Governance Proposer contract. The Governance Proposer verifies the winning payload for that round and forwards it to the Governance contract.

The Governance contract:

- Registers the new proposal with a reference to the payload.
- Records the creation timestamp.
- Derives and stores the voting start and end times according to protocol parameters.

At this point, the payload transitions from a “signaled upgrade candidate” to a tracked proposal within the governance system.

### Preparing for the Vote

Following proposal creation, there is a **`votingDelay`** period in which the proposal is pending and votes cannot yet be cast. This delay is designed to:

- Give the community time to review the payload and its implications.
- Allow node operators to assess operational impact.
- Allow tokenholders to stake and/or deposit into governance to participate in the upcoming vote. Staking (for running a sequencer) deposits tokens into governance by default and delegates voting power to the rollup (which automatically votes "yea" on proposals from the `GovernanceProposer`). Depositing into governance without staking allows tokenholders to vote directly.
- Provide sequencers with an opportunity to adjust their delegation if they want to vote differently from the default rollup behavior.

**Custom Voting via Delegation to a Controlled Address**

During this time, any sequencer who would like to vote *without following the canonical rollup* (i.e. vote “nay”) must delegate their stake to an address they control. This removes their stake's voting power from the rollup's control and gives it to the chosen address.

See [Custom Voting](https://docs.aztec.network/network/operation/sequencer_management/creating_and_voting_on_proposals#custom-voting-delegating-to-your-own-address) for instructions on how to re-delegate stake.

### Voting

After the waiting period expires, the proposal enters the `votingDuration` voting period. At the moment voting begins:

- The system snapshots voting power derived from staked tokens.
- The delegation state at that time determines who holds voting rights for the proposal.
- Any delegation changes made *after* voting has started do not affect voting power for that active proposal.

To participate in voting, eligible tokenholders can stake and lock their tokens to vote. All staked tokens will be locked for the minimum lock period (`votingDelay + votingDuration + executionDelay` from `getConfiguration()` in `Governance.sol`) to ensure the proposal can be executed before the voter exits.

**Default Voting via Rollup Delegation**

By default, tokenholders staked as sequencers delegate their voting power to the rollup contract through the GSE (Governance Staking Escrow). The rollup automatically votes "yea" on proposals created through the `GovernanceProposer` using **all** delegated stake from **all** sequencers in that rollup. To vote “nay” on a proposal, sequencers must delegate their stake to an address they control **before** the snapshot is taken.

**Note:** Because the rollup always votes "yea" on proposals that came through the sequencer signaling path, proposals that reach the voting stage require active opposition to fail rather than active support to pass. Sequencers and tokenholders who wish to oppose a proposal must explicitly re-delegate their stake before the voting snapshot is taken.

**Voting Requirements**

A proposal is approved only if it satisfies all of the following:

- **Quorum:** At least **`quorum`** of the total voting power in governance must participate.
- **Supermajority:** The difference between yea and nay votes must meet the **`requiredYeaMargin`** threshold.
- **Minimum Votes:** At least **`minimumVotes`** tokens must be deposited in governance for any vote to pass.

*(All fields from `getConfiguration()` in `Governance.sol`)*

If these thresholds are not met, the proposal fails and does not proceed to execution.

### Execution

When a proposal receives sufficient support, it passes. After passing, there's another mandatory `executionDelay` (`getConfiguration()` in `Governance.sol`) before the proposal becomes executable. 

This delay serves several purposes:

- Gives node operators time to deploy and test updated node software that is compatible with the upcoming changes.
- Allows the community to perform final checks and monitoring.
- Reduces the risk of sudden, uncoordinated changes that could disrupt the network.

In practice, operators may run multiple node versions during this window (for example, one on the current version and one on the upcoming version) to ensure a smooth transition.

Once executable, anyone can trigger execution. See [Executing Proposals](https://docs.aztec.network/network/operation/sequencer_management/creating_and_voting_on_proposals#executing-proposals) for instructions on how to execute proposals. If not executed within the `gracePeriod` (`getConfiguration()` in `Governance.sol`), the proposal expires.

### Failed Proposals

If a proposal fails to meet the voting requirements, Core Contributors should review the outcome and engage with stakeholders who voted against or declined to signal support. A failed AZUP cannot be resubmitted — a new AZUP must be created that addresses the feedback received during the signaling or voting process.

### Dropped Proposals

If the Governance Proposer contract is changed (e.g. via a governance proposal), all pending proposals that were created by the previous proposer become invalid. These proposals can be permanently dropped by anyone. This is a safety mechanism that prevents outdated proposals from executing after the governance infrastructure itself has been upgraded.

## Core Components

### Payload

A payload is an Ethereum smart contract that encodes a specific protocol upgrade. It implements a function (e.g. `getActions()`) that returns a list of onchain actions the governance contract should perform if the proposal passes. These actions may include updating protocol contract references, modifying registries, or enabling new features.

Before a payload is used in governance, participants are expected to:

- Verify the payload contract address on a block explorer.
- Inspect the `getActions()` implementation to understand exactly what will change.
- Check whether the payload has undergone audits where relevant.
- Discuss the proposed upgrade in public community channels.

The payload is the single source of truth for what an approved proposal will actually do onchain.

### Governance Proposer

The Governance Proposer contract manages the signaling phase and the transition from informal support to formal proposals. It:

- Accepts signals from sequencer nodes for specific payloads.
- Aggregates signal counts within fixed-length rounds.
- Identifies payloads that reach quorum within a round.
- Allows anyone to promote a round's winning payload to a proposal in the Governance contract.

The Governance Proposer acts as the bridge between sequencer sentiment and the formal governance layer.

### Governance Contract

The Governance contract is the canonical record of proposals and their state. It is responsible for:

- Tracking proposal metadata (payload address, creation time, voting schedule).
- Enforcing voting delays and voting periods.
- Recording vote tallies (“yea” vs “nay”).
- Evaluating whether proposals have met quorum and supermajority requirements.
- Executing an approved proposal's payload actions after the required delay.

All governance parameters — including voting delay, voting duration, execution delay, and quorum thresholds — can be modified through the governance process itself via the Governance contract's `updateConfiguration()` function. Changes to governance parameters follow the same proposal, signaling, voting, and execution process as any other upgrade.

### Governance Staking Escrow (GSE)

The Governance Staking Escrow holds staked tokens and manages voting power. It:

- Custodies stake used for securing the network.
- Tracks and updates delegation relationships: which address controls voting power for a given stake.
- Exposes voting functions that allow delegatees to vote on proposals using their delegated voting power.

GSE is the mechanism that binds stake to governance rights.

### Rollup Contract

Sequencers stake into a rollup contract. By default, a sequencer's voting power is delegated from the GSE to the rollup. The rollup can then cast votes on governance proposals on behalf of all stakers who remain delegated to it.

This default delegation path provides a straightforward way for sequencers to participate in governance without managing votes directly, while still retaining the option to override via custom delegation.

## Resources

[AZUP Template](./AZUPs/template.md)