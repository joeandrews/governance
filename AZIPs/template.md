# Aztec Improvement Proposal: [Title]

*Note: This template may change slightly to suit the purpose and scope of the proposal.*

## Preamble

- *Headers containing metadata about the AZIP, including the AZIP number, a short descriptive title (limited to a maximum of 80 characters), a description (limited to a maximum of 140 characters), and the author details. The title and description should not include the AZIP number. See below for details.*

### Header Format

| `azip` | `title` | `description` | `author` | `discussions-to` | `status` | `category` | `created` |
| --- | --- | --- | --- | --- | --- | --- | --- |
||  |  |  |  | |  |  |

|  |  |
| --- | --- |
| `azip` | AZIP number, assigned by an editor upon submission. Numbers are sequential. |
| `title` | The AZIP title is a few words, not a complete sentence. |
| `description` | Description is one full (short) sentence. |
| `author` | Comma-separated list of the authors. Example: `FirstName LastName (@GitHubUsername)`, `FirstName LastName <foo@bar.com>` |
| `discussions-to` | The URL pointing to the AZIP's GitHub Discussions thread. |
| `status` | `Draft`, `RFD`, `Accepted`, `Rejected`, or `Cancelled` |
| `category` | `Core`, `Economics`, `Standard`, or `Informational` |
| `created` | Date created on, in ISO 8601 (yyyy-mm-dd) format. |
| `requires` | Comma-separated list of AZIP numbers this proposal depends on (e.g. AZIP-3, AZIP-7). Optional. |

Headers that permit lists must separate elements with commas. Headers requiring dates will always do so in the format of ISO 8601 (yyyy-mm-dd).

### `author` header

The `author` header lists the names, email addresses or usernames of the authors/owners of the AZIP. Those who prefer anonymity may use a username only, or a first name and a username. The format of the `author` header value must be:

> Random J. User <address@dom.ain>
>

or

> Random J. User (@username)
>

or both, comma-separated:

> Random J. User (@username, address@dom.ain)
>

if the email address or GitHub username is included, and

> Random J. User
>

if neither is given.

At least one author must use a GitHub username, in order to get notified on change requests and have the capability to approve or reject them.

### `discussions-to` header

The `discussions-to` header will indicate the URL of the GitHub Discussions thread where the AZIP is being discussed.

## Abstract

- *Abstract is a multi-sentence (short paragraph) technical summary. This should be a very terse and human-readable version of the specification section. Someone should be able to read only the abstract to get the gist of what this specification does.*

## Motivation

- *A motivation section is required for all AZIPs. It should clearly explain why the existing protocol specification is inadequate to address the problem that the AZIP solves.*

## Specification (required for `Core` and `Standard` AZIPs):

- *The technical specification should describe the syntax and semantics of any new feature. The specification should be detailed enough to allow competing, interoperable implementations.*

## Rationale

- *The rationale fleshes out the specification by describing what motivated the design and why particular design decisions were made. It should describe alternate designs that were considered and related work, e.g. how the feature is supported in other languages. The rationale should discuss important objections or concerns raised during discussion around the AZIP.*

## Backwards Compatibility:

- *All AZIPs must include this section. Describe any backwards incompatibilities, their consequences, and how they will be addressed. If no incompatibilities exist, explicitly state that the proposal is fully backwards compatible.*

## Test Cases (if applicable):

- Tests should either be inlined in the AZIP as data (such as input/expected output pairs), or included in `../assets/azip-###/<filename>`.

## Reference Implementation (optional):

- An optional section that contains a reference/example implementation that people can use to assist in understanding or implementing this specification. This section may be omitted for all AZIPs.

## Economics Considerations (required for `Economics` AZIPs):

- *All proposals that change economic parameters or spend protocol funds should include a full economic analysis of the long-term effects on sequencing and proving the network for operators. Proposals that request funds from the treasury must clearly detail where the funds will go and how they will be spent. Any funds going to smart contracts must meet the Security Considerations below.*

## Security Considerations (required for `Core`, `Standard`, and `Economics` AZIPs):

- *All `Core`, `Standard`, and `Economics` AZIPs must contain a section that discusses the security implications/considerations relevant to the proposed change. Include information that might be important for security discussions, surfaces risks and can be used throughout the life-cycle of the proposal. E.g. include security-relevant design decisions, concerns, important discussions, implementation-specific guidance and pitfalls, an outline of threats and risks and how they are being addressed. AZIP submissions missing the "Security Considerations" section will be rejected. An AZIP cannot proceed to status `RFD` without a Security Considerations discussion deemed sufficient by the reviewers.*

## Copyright Waiver:

- All AZIPs must be in the public domain. This ensures that protocol specifications can be freely implemented, forked, and referenced by anyone without legal barriers — a requirement for credibly neutral, permissionless infrastructure. The copyright waiver MUST link to the license file and use the following wording: `Copyright and related rights waived via [CC0](/LICENSE).`

## Style Guide

### Titles

The `title` field in the preamble:

- Should not include the word "standard" or any variation thereof; and
- Should not include the AZIP's number.

### Descriptions

The `description` field in the preamble:

- Should not include the word "standard" or any variation thereof; and
- Should not include the AZIP's number.

### AZIP numbers

When referring to an AZIP by number, it should be written in the hyphenated form `AZIP-X` where `X` is the AZIP's assigned number.

### RFC 2119 and RFC 8174

AZIPs are encouraged to follow [RFC 2119](https://www.ietf.org/rfc/rfc2119.html) and [RFC 8174](https://www.ietf.org/rfc/rfc8174.html) for terminology and to insert the following at the beginning of the Specification section:

> The key words "MUST", "MUST NOT", "REQUIRED", "SHALL", "SHALL NOT", "SHOULD", "SHOULD NOT", "RECOMMENDED", "NOT RECOMMENDED", "MAY", and "OPTIONAL" in this document are to be interpreted as described in RFC 2119 and RFC 8174
>

## NOTE: Linking to External Resources

Links to external resources **SHOULD NOT** be included. External resources may disappear, move, or change unexpectedly. Exceptions are links to the [Aztec forum](https://forum.aztec.network/) and resources within the [governance repository](https://github.com/AztecProtocol/governance), which are permitted.

## NOTE: Linking to other AZIPs

References to other AZIPs should follow the format `AZIP-N` where `N` is the AZIP number you are referring to. Each AZIP that is referenced in an AZIP **MUST** be accompanied by a relative markdown link the first time it is referenced, and **MAY** be accompanied by a link on subsequent references. The link **MUST** always be done via relative paths so that the links work in the GitHub repository, forks of this repository, the main AZIPs site, mirrors of the main AZIP site, etc.

## NOTE: Auxiliary Files

Images, diagrams and auxiliary files should be included in a subdirectory of the `assets` folder for that AZIP as follows: `assets/azip-N` (where **N** is to be replaced with the AZIP number). When linking to an image in the AZIP, use relative links such as `../assets/azip-1/image.png`.
