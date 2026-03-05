# AZIP Process

## Abstract

The AZIP (Aztec Improvement Proposal) process is the standard process for specifying, discussing, and proposing changes to the Aztec network. AZIPs are version-controlled design documents that detail the motivation, technical specification, rationale, implementation path, and impact evaluation of proposed protocol-level, system-level, and application-level changes for the Aztec network, ranging from high-level informational context to core infrastructure updates and application standards. AZIPs are maintained in a dedicated, version-controlled GitHub repository, which serves as the canonical record of all active, implemented, and archived AZIPs. When relevant, AZIPs include concise technical specifications, and the author is responsible for building community consensus and documenting any dissenting views. This document defines the AZIP lifecycle, required structure, categories, and editorial workflow that govern how proposals are authored, reviewed, standardized, and recorded.

## Rationale

AZIPs are intended to be the canonical way to propose and coordinate changes to the Aztec network, replacing ad‑hoc discussions with a structured, transparent process. By requiring proposals to be captured as version‑controlled design documents, AZIPs create a public and auditable record of motivations, design iterations, community feedback and implementation details of every proposed change to the Aztec network, documenting the full lifecycle of how a change is evaluated, refined, and ultimately adopted or rejected. This shared format gives contributors a common language for evaluating ideas, helps implementers track which features their software supports, and provides users with a clear view of the current and planned behavior of the network. The AZIP framework is deliberately modeled on the well‑tested [EIP](https://github.com/ethereum/EIPs/blob/master/EIPS/eip-1.md) and [BIP](https://bips.dev/) processes, adapted to Aztec’s architecture and governance needs to give the ecosystem a battle‑tested and predictable process that brings proposals from idea to implementation.

## Scope

AZIPs are design documents that detail the motivation, technical specification, rationale, implementation path, and impact evaluation of proposed protocol-level, system-level, and application-level changes for the Aztec network, ranging from high-level informational context to core infrastructure updates and application standards. AZIPs are used to:

- Track progress while designing, building, and implementing new features
- Publicly communicate new features, designs, and create space for community input
- Propose new upgrades for approval & execution

All AZIPs must be grouped into one of the following categories:

1. **Core** - Core System AZIPs, including improvements to the protocol specifications for public, private, sequencer and prover components, and networking protocols.
2. **Treasury** - Requesting funds from the governance treasury or minting more $AZTEC tokens. Grant requests are not entertained as part of this process.
3. **Standard** - Application layer AZIPs, e.g. defining standards for contracts written in Noir or a token standard etc. A separate standards process may be defined in the future.
4. **Informational** - Provide general guidelines or information to the Aztec community, but does not propose a new feature. Includes high level descriptions of the system, architecture, etc.

AZIPs that require onchain implementation are bundled into AZUPs by Core Contributors and submitted to Sequencers & Tokenholders for voting. See the AZUP process for more details. 

### **Process**

Each AZIP progresses through a defined set of stages that govern how it is proposed, reviewed, finalized, and, where applicable, implemented onchain. 

All AZIPs start as informal ideas discussed in GitHub Discussions on the AZIP repository. Once properly specified, they are submitted as pull requests to the official AZIP GitHub repository. The PR serves as the primary venue for technical review and feedback. Once review is complete, the PR is merged and the AZIP becomes part of the canonical record. From there, an AZIP progresses through the following stages:

1. `Idea`: Proposal is in pre‑draft concept form and being discussed in GitHub Discussions on the AZIP repository. AZIPs in `Idea` stage are not tracked in the AZIP repository.
    - Criteria:
        - Post in GitHub Discussions (under the "AZIP Ideas" category)
        - Gather initial feedback from the community
2. `Draft`: Author opens a pull request with a fully specified AZIP. The PR remains open for peer review. An editor assigns an AZIP number and reviews formatting. All technical discussion and peer review happens in PR comments.
    - Criteria:
        - All fields in AZIP template are completed (no TBDs)
        - Full specification draft, including technical implementation
    - Process:
        - Author opens a PR with the complete AZIP
        - The AZIP number is derived from the PR number (e.g. PR #7 becomes AZIP-7)
        - An editor reviews formatting and confirms the number
        - The PR remains open for peer review — all feedback happens in PR comments
3. `RFD (Ready for Discussion)`: Peer review is complete and the PR is merged into the repository. The AZIP now represents the final standard; only minor errata or non‑normative clarifications may be added. Core Contributors review the AZIP for AZUP inclusion.
    - Criteria:
        - Peer review has been completed and documented in the PR, with provable engagement from impacted stakeholders
        - Security Considerations discussion deemed sufficient by the reviewers (for `Core` and `Standard` AZIPs)
    - Process:
        - An editor reviews the PR and associated discussion to verify sufficient peer review
        - The PR is merged — the AZIP is now in the repository
4. `Accepted`: AZIP has been accepted and implemented onchain via an AZUP (if applicable).
5. `Rejected`: Core Contributors have reviewed the AZIP and decided not to progress it for implementation. The rejection rationale must be documented by Core Contributors. A rejected AZIP remains in the repository as a permanent record. If the idea is pursued at a later date with a substantially different approach, it requires a new AZIP number referencing the original.
6. `Cancelled`: The AZIP author(s) or editors have withdrawn the proposed AZIP. An AZIP may be cancelled at any point after it has entered `Draft`. If the idea is pursued at a later date it is considered a new proposal.

If an existing implementation does not conform to the specification as described in an accepted AZIP, the implementation may be corrected without a new AZIP. AZIPs govern the specification, not the implementation — fixes that bring code in line with an existing spec are not considered protocol changes.

An AZIP's status does not enforce when development must begin. For example, work may start as early as the Draft stage, but remain heavily subject to change. Aztec engineers building reference implementations may begin before an AZIP reaches `RFD` status, but external developers working on their own implementations will likely wait for `RFD` or `Accepted` status.

Throughout the process, the proposal author(s) works to build community consensus, collect technical feedback, and, where needed, provide reference implementations. Because AZIPs are version-controlled files, their history serves as the permanent record of design decisions, and they act as the primary coordination mechanism for proposal authors and the wider community. AZIP editors are responsible for moving proposals from stage to stage, ensuring each AZIP meets the set criteria required to progress through the AZIP process.

## Content Requirements

AZIPs should be written in [markdown](https://github.com/adam-p/markdown-here/wiki/Markdown-Cheatsheet) format. All AZIPs must follow the official AZIP Template.

Each AZIP should have the following parts:

- **Preamble:**
    - Headers containing metadata about the AZIP, including the AZIP number, a short descriptive title (limited to a maximum of 44 characters), a description (limited to a maximum of 140 characters), and the author details. Irrespective of the category, the title and description should not include AZIP number. See below for details.
- **Abstract:**
    - Abstract is a multi-sentence (short paragraph) technical summary. This should be a very terse and human-readable version of the specification section. Someone should be able to read only the abstract to get the gist of what this specification does.
- **Motivation:**
    - A motivation section is critical for AZIPs that want to change the protocol. It should clearly explain why the existing protocol specification is inadequate to address the problem that the AZIP solves. This section may be omitted if the motivation is evident.
- **Specification** (required for `Core` and `Standard` AZIPs):
    - The technical specification should describe the syntax and semantics of any new feature. The specification should be detailed enough to allow competing, interoperable implementations.
- **Rationale:**
    - The rationale fleshes out the specification by describing what motivated the design and why particular design decisions were made. It should describe alternate designs that were considered and related work, e.g. how the feature is supported in other languages. The rationale should discuss important objections or concerns raised during discussion around the AZIP.
- **Backwards Compatibility** (if applicable):
    - All AZIPs that introduce backwards incompatibilities must include a section describing these incompatibilities and their consequences. The AZIP must explain how the author proposes to deal with these incompatibilities. This section may be omitted if the proposal does not introduce any backwards incompatibilities, but this section must be included if backward incompatibilities exist.
- **Test Cases** (if applicable):
    - Tests should either be inlined in the AZIP as data (such as input/expected output pairs, or included in `../assets/azip-###/<filename>`.
- **Reference Implementation** (optional)
    - An optional section that contains a reference/example implementation that people can use to assist in understanding or implementing this specification. This section may be omitted for all AZIPs.
- **Treasury Considerations** (required for any proposals that mint new Aztec tokens or spend protocol controlled funds):
    - All proposals that spend protocol funds should include a full economic analysis of the long-term effects on sequencing and proving the network for operators. Proposals that request funds from the treasury must clearly detail where the funds will go and how they will be spent. Any funds going to smart contracts must meet the Security Considerations below.
- **Security Considerations** (required for `Core` and `Standard` AZIPs):
    - All `Core` and `Standard` AZIPs must contain a section that discusses the security implications/considerations relevant to the proposed change. Include information that might be important for security discussions, surfaces risks and can be used throughout the life-cycle of the proposal. E.g. include security-relevant design decisions, concerns, important discussions, implementation-specific guidance and pitfalls, an outline of threats and risks and how they are being addressed. AZIP submissions missing the “Security Considerations” section will be rejected. An AZIP cannot proceed to status `RFD` without a Security Considerations discussion deemed sufficient by the reviewers.
- **Copyright Waiver:**
    - All AZIPs must be in the public domain. The copyright waiver MUST link to the license file and use the following wording: `Copyright and related rights waived via [CC0](/LICENSE).`

## AZIP Authors

Throughout the process, AZIP author(s) are responsible for building community consensus, collecting and implementing technical feedback, and interfacing with AZIP editors. 

### Transferring AZIP Ownership

Transferring ownership of an AZIP to a new champion may occasionally be necessary. In general, the original author should remain a co‑author, but this is at the discretion of the original author. Appropriate reasons for a transfer include the original author no longer having the time or interest to maintain the AZIP, or being unreachable and unable to respond to feedback or change requests. Disagreement with the current direction of an AZIP is not, by itself, a sufficient reason for a transfer; in such cases, contributors are encouraged to seek consensus or, if that is not possible, submit a competing AZIP.

Contributors who wish to assume ownership of an AZIP should submit a request addressed to both the current author and an AZIP editor. If the original author does not respond within a reasonable timeframe, the AZIP editor may make a unilateral decision regarding the transfer, with the understanding that such decisions can be revisited if circumstances change.

## AZIP Editors

AZIP editors are responsible for moving proposals from stage to stage, ensuring each AZIP meets the set criteria required to progress through the AZIP process. AZIP editors are currently designated by the Aztec Foundation and include members of the Aztec Foundation and Aztec Labs. 

The current AZIP editors are:

TBD

Many AZIPs are written and maintained by developers with write access to the Aztec codebase. The AZIP editors continuously monitor AZIP changes, and correct any structure, grammar, spelling, or markup mistakes.

### Responsibilities

AZIP editors are meant to be *administrators* vs. *decision-makers*. They are required to raise AZIPs up to Core Contributors regardless of their subjective views on the contents of those proposals. Their role should be to ensure that each AZIP is properly formatted, discussed, and meets the following criteria at each stage but not act as a subjective gatekeeper:

**`Idea -> Draft`**

When an author opens a PR with a new AZIP, an editor reads the AZIP to check if it meets the following requirements:

- All fields in AZIP template are completed (no TBDs)
- Title accurately describes content
- Full specification is drafted, including technical implementation
- Has been checked for language (spelling, grammar, sentence structure, etc.), markup (GitHub flavored Markdown), code style
- The proposal is technically sound and coherent, even when its likelihood of reaching RFD status is unknown

If the AZIP does not meet the requirements, the editor will comment on the PR requesting changes with specific instructions.

Once the AZIP meets the requirements, the editor will:

- Confirm the AZIP number (derived from the PR number)
- Update AZIP header to `Draft`
- Leave the PR open for peer review

**`Draft -> RFD (Ready for Discussion)`**

While a PR is open in `Draft` status, AZIP editors and proposal authors are responsible for socializing the AZIP with impacted stakeholders for peer review. All technical discussion and feedback happens in PR comments. Authors should update the AZIP in the PR based on feedback received.

Once an editor determines that sufficient peer review has been completed, they review the PR and associated discussion to verify:

- Peer review has been completed and documented in the PR, with provable engagement from impacted stakeholders
- Changes to the design and specification are technically sound and aligned with stakeholder feedback. If feedback is not incorporated, the proposal must include a clear justification.
- Specification is considered final, with only minor errata or non‑normative clarifications allowed thereafter.

The editor then merges the PR, which transitions the AZIP to `RFD` status and adds it to the repository as the canonical record.

Read more about the Core Contributors in the [AZUP Process](azup-process.md).

## History

This document was derived heavily from [Ethereums EIP-001](https://eips.ethereum.org/EIPS/eip-1), which in turn was derived from [Bitcoin's BIP-0001](https://github.com/bitcoin/bips) written by Amir Taaki which in turn was derived from [Python's PEP-0001](https://peps.python.org/). In many places text was simply copied and modified. Do not bother the authors of these ancestor works with technical questions specific to Aztec or the AZIP. Please direct all comments to the AZIP editors.

## Copyright

Copyright and related rights waived via CC0.

---

## Resources

AZIP Template
