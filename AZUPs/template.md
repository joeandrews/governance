# AZUP-X: [Title]

*Note: This template may change slightly to suit the purpose and scope of the proposal.*

## Preamble

- *Headers containing metadata about the AZUP, including the AZUP number, a short descriptive title (limited to a maximum of 44 characters), a description (limited to a maximum of 140 characters), and the author details. Irrespective of the category, the title and description should not include the AZUP number. See below for details.*

### Header Format

|  |  |
| --- | --- |
| `azup` | AZUP number (this is determined by the AZUP author when it is published). |
| `title` | The AZUP title is a few words, not a complete sentence. |
| `description` | Description is one full (short) sentence. |
| `author` | Comma separated list of the author's. Example: `FirstName LastName (@GitHubUsername)`, `FirstName LastName <foo@bar.com>` |
| `azips-included` | URLs pointing to included AZIPs in GitHub |
| `discussions-to` | The URL pointing to the official discussion thread in the [Aztec forum](https://discourse.aztec.network/). |
| `status` | `Scheduled` `Deployed` |
| `created` | Date created on, in ISO 8601 (yyyy-mm-dd) format. |

Headers that permit lists must separate elements with commas. Headers requiring dates will always do so in the format of ISO 8601 (yyyy-mm-dd).

### `author` header

The `author` header lists the names, email addresses or usernames of the authors/owners of the AZUP. Those who prefer anonymity may use a username only, or a first name and a username. The format of the `author` header value must be:

> Random J. User <address@dom.ain>
>

or

> Random J. User (@username)
>

if the email address or GitHub username is included, and

> Random J. User
>

if the email address is not given.

It is not possible to use both an email and a GitHub username at the same time. If important to include both, one could include their name twice, once with the GitHub username, and once with the email.

At least one author must use a GitHub username, in order to get notified on change requests and have the capability to approve or reject them.

### `discussions-to` header

While an AZUP is being discussed, the `discussions-to` header will indicate the URL where the AZUP is being discussed (in the [Aztec forum](https://discourse.aztec.network/)).

## Abstract

- *Abstract is a multi-sentence (short paragraph) technical summary. This should be a very terse and human-readable summary of what this upgrade does. Someone should be able to read only the abstract to get the gist of the proposal.*

## Motivation

- *Motivation provides high-level context and rationale for the proposal, explaining why this upgrade is needed now and why it should move through governance at this moment.*

## Specification

### 1. Payload / Action Details

| Item | Value |
| --- | --- |
| **Payload / Contract / Proposal Address** | `[0x... or identifier]` |
| **Repository** | `[Org/repo-name or docs link]` |
| **Contract / Module** | `[path/to/ContractOrModule.sol]` |
| **Explorer** | `[Verified Contract / Explorer link]` |

### 2. Sequencer Configuration (for signalling)

- *Add environment variable for sequencers to set the payload address for signalling. Include a short explanation.*

## Impact Evaluation

- *Summarize the expected impact on relevant stakeholders and outline all expected actions.*

## Security & Audits

- *Discuss security implications, potential risks, and how they are being addressed. Include links to the relevant code repository, key contracts or modules, and any verified deployments on explorers so reviewers can inspect the logic themselves.*

## Implementation Timeline

| Phase | Start Date | Estimated Duration | Target / Notes |
| --- | --- | --- | --- |
| Community Discussion | [FILL IN] | 3 days | — |
| Signaling  | [FILL IN] | -  | [Quorum or condition] |
| Proposal Created | [FILL IN] | [Timing relative to signaling] | — |
| Proposal Delay | [FILL IN] | 3 days |  |
| Voting Period | [FILL IN] | 7 days | — |
| Execution Delay | [FILL IN] | 7 days | — |
| **Earliest Possible Execution** | [FILL IN] | — | **[Date and time in UTC, if known]** |

*Note: The actual execution date depends on when signaling quorum is reached (if relevant), the proposal passes, and execution occurs.*

## Open Questions and Feedback

- *Any Open Questions or Requests for Feedback for the Aztec community*

## NOTE: Linking to External Resources

Links to external resources **SHOULD NOT** be included. External resources may disappear, move, or change unexpectedly. The exception is links to the [Aztec forum](https://forum.aztec.network/), which are permitted.

## NOTE: Linking to other AZIPs and AZUPs

References to AZIPs should follow the format `AZIP-N` and references to AZUPs should follow the format `AZUP-N`, where `N` is the number you are referring to. Each reference **MUST** be accompanied by a relative markdown link the first time it is referenced, and **MAY** be accompanied by a link on subsequent references. The link **MUST** always be done via relative paths so that the links work in the GitHub repository, forks of this repository, and mirrors.

## NOTE: Auxiliary Files

Images, diagrams and auxiliary files should be included in a subdirectory of the `assets` folder for that AZUP as follows: `assets/azup-N` (where **N** is to be replaced with the AZUP number). When linking to an image in the AZUP, use relative links such as `../assets/azup-1/image.png`.
