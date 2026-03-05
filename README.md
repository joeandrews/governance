# Aztec Governance

This is the canonical repository for all things Aztec Governance. It is the home to the AZIP and AZUP process:

- **[AZIPs (AZtec Improvement Proposals)](azip-process.md)** – the *off-chain* process for proposing, iterating on, and socially validating changes to Aztec's protocols, standards, and governance.
- **[AZUPs (AZtec Upgrade Proposals)](azup-process.md)** – the *on-chain* process for taking approved AZIPs through sequencer signaling, tokenholder voting, and onchain execution, turning proposed changes into live protocol upgrades.

Together, AZIPs and AZUPs form a structured pipeline for implementing changes within the Aztec Network in a transparent, controlled way.

## What's an AZIP?

AZIPs (AZtec Improvement Proposal) are a tool that can be used by anyone to propose informal changes, technical changes or to propose standards that can be used within the Aztec Network. AZIPs are version-controlled design documents that detail the motivation, technical specification, rationale, implementation path, and impact evaluation of proposed protocol-level, system-level, and application-level changes for the Aztec network, ranging from high-level informational context to core infrastructure updates and application standards. AZIPs are used to:

- Track progress while designing, building, and implementing new features
- Publicly communicate new features, designs, and create space for community input
- Propose new upgrades for approval & execution

For the full process, see the [AZIP Process](azip-process.md). To start drafting, use the [AZIP Template](AZIPs/template.md).

## What's an AZUP?

AZUPs (AZtec Upgrade Proposals) bundle the implementations of one or more AZIPs that Core Contributors have approved for inclusion in the Aztec Network. For proposed changes to be considered for signalling by sequencers and voting by token holders, it must be formally proposed via the AZUP process.

Once an AZIP has been deemed properly specified, it is submitted by AZIP editors to the Core Contributors for final review. Core Contributors discuss AZIPs on a weekly basis during the Core Contributor Weekly call.

AZIPs are bundled together by Core Contributors into AZUPs. AZUPs include:

- List of AZIPs scheduled for inclusion
- Payload(s) for any onchain changes

For the full process, see the [AZUP Process](azup-process.md). To start drafting, use the [AZUP Template](AZUPs/template.md).

## AZIP Editors

AZIP editors are responsible for moving proposals from stage to stage, ensuring each AZIP meets the criteria required to progress through the [AZIP Process](azip-process.md). Editors are administrators, not decision-makers.

TBD

## Contributing

Anyone can propose changes to the Aztec Network by submitting an AZIP. To get started:

1. Post your idea in GitHub Discussions (under the "AZIP Ideas" category) to gather initial feedback
2. Use the [AZIP Template](AZIPs/template.md) to draft your proposal
3. Open a pull request to this repository — an editor will assign a number and review begins
4. Once peer review is complete, the PR is merged and the AZIP enters the repository

See the [AZIP Process](azip-process.md) for the full lifecycle and requirements.
