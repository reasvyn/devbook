# Reading RFCs & Internet Standards

## Description

Requests for Comments (RFCs) define the protocols and standards that make the internet work — TCP, HTTP, DNS, TLS, and hundreds more. This document teaches developers how to navigate the RFC series, understand document maturity levels, interpret normative language (MUST, SHOULD, MAY), and apply this knowledge to build compliant, interoperable implementations.

## Prerequisites

- [How to Read a Research Paper](research-papers.md) — the three-pass method applies to RFC reading as well

## Table of Contents

- [What Are RFCs?](#what-are-rfcs)
- [The RFC Series Structure](#the-rfc-series-structure)
- [RFC Statuses and Maturity](#rfc-statuses-and-maturity)
- [RFC Structure and Sections](#rfc-structure-and-sections)
- [Navigating the RFC Index](#navigating-the-rfc-index)
- [Understanding Normative Language](#understanding-normative-language)
- [Notable RFCs Every Developer Should Know](#notable-rfcs-every-developer-should-know)
- [Errata and Updates](#errata-and-updates)
- [RFC vs. Implementation](#rfc-vs-implementation)
- [Reading RFCs Effectively](#reading-rfcs-effectively)
- [BCPs, STDs, and FYI Documents](#bcps-stds-and-fyi-documents)
- [RFC Etiquette and Contribution](#rfc-etiquette-and-contribution)
- [Glossary](#glossary)
- [Quick References](#quick-references)
- [Next Steps](#next-steps)

## Content / Material

### What Are RFCs?

RFC stands for Request for Comments, a historical name that persists even though modern RFCs are authoritative standards, not requests for discussion. The RFC series began in 1969 as a mechanism for documenting the protocols of the ARPANET. Today, the RFC Editor publishes documents that define internet standards, best practices, experimental protocols, and informational content.

**The organizations behind RFCs:**

| Organization | Role |
|---|---|
| IETF (Internet Engineering Task Force) | Develops and maintains internet standards |
| IRTF (Internet Research Task Force) | Explores long-term research topics |
| IAB (Internet Architecture Board) | Provides architectural oversight |
| RFC Editor | Publishes and maintains the RFC series |
| IANA (Internet Assigned Numbers Authority) | Manages protocol parameter registries |

**RFC categories:**

| Stream | Source | Examples |
|---|---|---|
| IETF | Working groups developing standards | TCP, HTTP, TLS |
| IRTF | Research groups | Delay-tolerant networking |
| Independent | Individual submissions | Some informational documents |
| IAB | Architectural guidance | Internet architecture principles |

**Why developers must read RFCs:**

| Reason | Explanation |
|---|---|
| Interoperability | RFCs define the protocol contract; implementations that deviate fail to interoperate |
| Correctness | Secondary sources (blog posts, tutorials) often omit edge cases and security considerations |
| Compliance | Some industries require standards-compliant implementations for certification |
| Security | Security considerations sections document known threats and mitigations |
| Performance | Understanding wire formats and protocol mechanics enables optimization |

RFCs are the definitive source of truth for internet protocols. Everything else is commentary.

### The RFC Series Structure

RFCs are numbered sequentially. RFC 1 (Host Software) was published in April 1969. As of 2025, there are over 9,500 RFCs. The sequential numbering means RFC numbers do not indicate topic or relationship.

**RFC numbering conventions:**

| Convention | Meaning | Example |
|---|---|---|
| RFC N | Original RFC | RFC 793 (TCP) |
| RFC N | Single RFC that obsoletes others | RFC 7230 obsoletes RFC 2616 |
| RFC N | RFC that updates another | RFC 7231 updates RFC 2616 |

**RFC relationships:**

```text
RFC 2616  (HTTP/1.1 original)
   |
   +-- RFC 7230 (obsoletes 2616, covers message syntax & routing)
   +-- RFC 7231 (obsoletes 2616, covers semantics & content)
   +-- RFC 7232 (obsoletes 2616, covers conditional requests)
   +-- RFC 7233 (obsoletes 2616, covers range requests)
   +-- RFC 7234 (obsoletes 2616, covers caching)
   +-- RFC 7235 (obsoletes 2616, covers authentication)
```

When a specification grows, it may be split into multiple RFCs. Always check whether the RFC you are reading has been obsoleted or updated.

**STD numbers:**

Some RFCs are designated as Internet Standards and assigned an STD (Standard) number. STD numbers are stable — they point to the current RFC defining that standard, even when the RFC is updated.

```
STD 7  -> RFC 793 (TCP) originally, now points to RFC 9293
STD 5  -> RFC 791 (IP) originally, now points to RFC 791 itself
```

An STD number is a stable reference to a standard, even as the underlying RFC changes. When you reference a protocol, reference its STD number if available.

### RFC Statuses and Maturity

Every RFC has a status that indicates its maturity and intended use. The status is listed at the top of the RFC.

**Full RFC status categories:**

| Status | Meaning | Stability | Example |
|---|---|---|---|
| Proposed Standard | Stable specification, well-reviewed, may change | Evolving | RFC 8446 (TLS 1.3) was Proposed Standard |
| Internet Standard | Proven in production, very stable | Stable | STD 7 / RFC 9293 (TCP) |
| Best Current Practice | Recommended practices, not a protocol spec | Evolving | RFC 2119 (Key words) |
| Experimental | Not proven, may be abandoned | Unstable | Various |
| Informational | Not a standard, just information | N/A | RFC 1149 (IP over Avian Carriers) |
| Historic | Superseded or obsolete | N/A | RFC 760 (IP, superseded by 791) |
| Unknown | Status not assigned | Varies | Some very early RFCs |

**The standards track maturity progression:**

```text
Proposed Standard
       |
       v
   (implementation and testing)
       |
       v
Draft Standard (currently deprecated path)
       |
       v
Internet Standard (only after significant production deployment)
```

In 2011, the IETF deprecated the Draft Standard level. Today, standards go directly from Proposed Standard to Internet Standard, but the criteria for Internet Standard are high — requiring multiple independent implementations and significant deployment experience.

**How to determine an RFC's status:**

1. Look at the top of the RFC document — the status line
2. Check the RFC Editor's status page for the current status of any RFC
3. For standards-track documents, check whether the RFC has been updated or obsoleted by newer RFCs

### RFC Structure and Sections

Modern RFCs follow a consistent structure, described in RFC 7322 (RFC Style Guide). Understanding the structure helps you find information quickly.

**Standard RFC structure:**

| Section | Content | Reading priority |
|---|---|---|
| Title and header | RFC number, title, status, date, author, category | Always read |
| Abstract (1) | Brief summary of the specification | Always read |
| Status of This Memo (2) | Legal status, copyright, and IETF rights | Skim |
| Copyright Notice (3) | Legal boilerplate | Skip |
| Table of Contents | Full navigation | Use for reference |
| 1. Introduction | Motivation, scope, and relationship to other RFCs | Always read |
| 1.1 Terminology | Definitions of key terms | Always read if present |
| 2. Protocol specification | The normative protocol definition | Read carefully |
| 2.x Subsections | Individual protocol elements | Reference |
| n. Security Considerations | Threats, attacks, and mitigations | Always read |
| n+1. IANA Considerations | Registry updates required | Skip unless implementing |
| n+2. References | Normative and informative references | Check for context |
| Appendix A | Examples, extended discussion | Read if needed |
| Appendix B | Backwards compatibility | Read if migrating from older versions |

**The most important sections for implementers:**

1. **Terminology** — defines key concepts and conventions used throughout the RFC
2. **Protocol specification** — the normative definition of the protocol behavior
3. **Security considerations** — documents known attacks and how the protocol addresses them

### Navigating the RFC Index

The RFC Index is a searchable database of all published RFCs. It is maintained by the RFC Editor.

**Using the RFC Editor search:**

The RFC Editor website provides search by:

- RFC number (e.g., "793" finds RFC 793)
- Title keywords (e.g., "HTTP" finds all RFCs with HTTP in the title)
- Author name
- Status (Proposed Standard, Internet Standard, etc.)
- Stream (IETF, IRTF, IAB, Independent)
- Date range

**Related RFCs:**

Each RFC lists references to related RFCs. The RFC Editor also provides:

- Updates / Updated by: which RFCs this one updates, and which update this one
- Obsoletes / Obsoleted by: which RFCs this one obsoletes, and which obsolete this one
- Also known as: STD/BCP numbers

**Example relationship graph for HTTP/1.1:**

```text
RFC 1945 (HTTP/1.0 - informational)
   |
   v
RFC 2068 (HTTP/1.1 - proposed standard, obsoleted)
   |
   v
RFC 2616 (HTTP/1.1 - draft standard, obsoleted)
   |
   v  (split into)
+-- RFC 7230 (message syntax and routing)
+-- RFC 7231 (semantics and content)
+-- RFC 7232 (conditional requests)
+-- RFC 7233 (range requests)
+-- RFC 7234 (caching)
+-- RFC 7235 (authentication)
   |
   v  (obsoleted by)
+-- RFC 9110 (HTTP semantics)
+-- RFC 9111 (HTTP caching)
+-- RFC 9112 (HTTP/1.1 message syntax)
+-- RFC 9113 (HTTP/2)
+-- RFC 9114 (HTTP/3)
```

Always check whether the RFC you found is the current version. A blog post from 2015 might reference RFC 2616, which has since been obsoleted.

### Understanding Normative Language

RFC 2119 (now BCP 14) defines key words used in RFCs to indicate requirement levels. These words have precise meanings that implementers must understand.

**The requirement levels:**

| Keyword | Meaning | Implementation consequence |
|---|---|---|
| MUST / REQUIRED / SHALL | Absolute requirement | The implementation fails if it does not do this |
| MUST NOT / SHALL NOT | Absolute prohibition | The implementation fails if it does this |
| SHOULD / RECOMMENDED | Strong recommendation | Deviate only with good reason and full understanding of consequences |
| SHOULD NOT / NOT RECOMMENDED | Strong discouragement | Do this only after careful consideration |
| MAY / OPTIONAL | Truly optional | The implementation may choose to include or omit |

**How to read normative language:**

```text
Example from RFC 7230 (HTTP/1.1):

"A recipient MUST parse the URI reference [...] If the URI reference
does not conform to the grammar, the recipient MUST reject the message."

Interpretation:
- Parsing the URI is mandatory (MUST)
- Rejecting malformed URIs is mandatory (MUST)
- No ambiguity, no exceptions
```

```text
Example of SHOULD:

"A user agent SHOULD include a Connection header field in any request
that uses a connection-specific option."

Interpretation:
- Strongly recommended to include the header
- An implementer MAY omit it, but must understand the consequences
- The consequences might include interoperability issues with some servers
```

**The difference between MUST and SHOULD:**

MUST creates a compliance requirement. If your implementation does not satisfy a MUST, it is not compliant with the standard.

SHOULD creates a strong recommendation. Deploying a variant that violates a SHOULD may work in practice but may cause interoperability issues in unexpected scenarios.

**Lowercase vs. uppercase:**

The normative meanings only apply when the key words appear in ALL CAPS. Lowercase "must", "should", and "may" carry ordinary English meanings and are not normative.

**BCP 14 and RFC 8174:**

RFC 8174 clarifies that RFC 2119 keywords in lowercase do not carry normative meaning. It also recommends that RFCs explicitly state when they use RFC 2119 terminology.

### Notable RFCs Every Developer Should Know

Every developer should be familiar with certain foundational RFCs — not necessarily the full text, but the purpose and scope.

**Transport layer:**

| RFC | Title | Status | Why it matters |
|---|---|---|---|
| RFC 9293 | Transmission Control Protocol (TCP) | Internet Standard | Defines TCP, the dominant transport protocol |
| RFC 768 | User Datagram Protocol (UDP) | Internet Standard | Defines UDP, the connectionless transport |
| RFC 9000 | QUIC: A UDP-Based Multiplexed and Secure Transport | Proposed Standard | Modern transport for HTTP/3 |

**Application layer:**

| RFC | Title | Status | Why it matters |
|---|---|---|---|
| RFC 9110 | HTTP Semantics | Proposed Standard | Core HTTP semantics — methods, status codes, headers |
| RFC 9112 | HTTP/1.1 | Proposed Standard | HTTP/1.1 message format |
| RFC 9113 | HTTP/2 | Proposed Standard | HTTP/2 framing, multiplexing, server push |
| RFC 9114 | HTTP/3 | Proposed Standard | HTTP/3 over QUIC |
| RFC 5321 | Simple Mail Transfer Protocol (SMTP) | Internet Standard | Email transmission |
| RFC 5322 | Internet Message Format | Proposed Standard | Email message format (headers, body) |

**Security:**

| RFC | Title | Status | Why it matters |
|---|---|---|---|
| RFC 8446 | The Transport Layer Security (TLS) Protocol v1.3 | Proposed Standard | Current TLS standard |
| RFC 5280 | Internet X.509 PKI Certificate and CRL Profile | Proposed Standard | Certificate infrastructure |
| RFC 7519 | JSON Web Token (JWT) | Proposed Standard | Token-based authentication |

**Infrastructure:**

| RFC | Title | Status | Why it matters |
|---|---|---|---|
| RFC 1034 | Domain Names — Concepts and Facilities | Internet Standard | DNS architecture |
| RFC 1035 | Domain Names — Implementation and Specification | Internet Standard | DNS wire format |
| RFC 791 | Internet Protocol (IP) | Internet Standard | IP packet format and addressing |
| RFC 826 | Ethernet Address Resolution Protocol (ARP) | Internet Standard | Address resolution |
| RFC 1519 | Classless Inter-Domain Routing (CIDR) | Proposed Standard | IP address allocation |

**Best practices:**

| RFC | Title | Status | Why it matters |
|---|---|---|---|
| RFC 2119 | Key Words for Use in RFCs | Best Current Practice | Normative language definitions |
| RFC 3339 | Date and Time on the Internet: Timestamps | Proposed Standard | ISO 8601 profile for internet timestamps |
| RFC 3986 | Uniform Resource Identifier (URI) | Internet Standard | URI syntax and semantics |

**Foundational:**

| RFC | Title | Status | Why it matters |
|---|---|---|---|
| RFC 1 | Host Software | Informational | The first RFC — historical |
| RFC 1149 | IP over Avian Carriers | Informational | Famous humorous RFC |

### Errata and Updates

RFCs are not perfect. The IETF maintains an errata system where errors, ambiguities, and clarifications are recorded.

**Types of errata:**

| Type | Meaning | Severity |
|---|---|---|
| Editorial | Spelling, grammar, formatting | Low |
| Technical | Incorrect protocol behavior described | High |
| Clarification | Ambiguous text that needs explanation | Medium |

**Checking errata:**

Every RFC has an errata page on the RFC Editor website. Before implementing, check the errata for your target RFC. A critical technical erratum can change the meaning of a MUST or SHOULD.

**How RFCs are updated:**

1. **Errata** — minor corrections, no new RFC
2. **Updates** — a new RFC that modifies specific sections of an existing RFC
3. **Obsoletes** — a new RFC that completely replaces an existing RFC

**Determining the current specification:**

For a given protocol, find the most recent RFC that obsoletes all previous versions. Check that the RFC is not itself obsoleted. Check the errata for any corrections.

### RFC vs. Implementation

An RFC specifies what a protocol should do. An implementation is what a program actually does. The two are not always the same.

**Common implementation deviations:**

| Deviation | Cause | Consequence |
|---|---|---|
| Missing feature | Implementation incomplete | Interoperability failure with parties that use the feature |
| Incorrect behavior | Misunderstanding the spec | Protocol violations, security issues |
| Extension | Implementation adds non-standard behavior | Works within the implementation's ecosystem, may not interoperate |
| Optimization | Implementation takes shortcuts that violate spec | Faster but potentially non-compliant |
| Strictness | Implementation rejects what the spec allows | Reduced interoperability |

**How implementations influence RFCs:**

The relationship is not one-way. Implementations also influence RFC evolution:

- Implementation experience reveals ambiguities in the specification
- Real-world deployment patterns inform protocol updates
- Security vulnerabilities discovered in implementations lead to revised security considerations

**The robustness principle (Postel's Law):**

"Be conservative in what you send, be liberal in what you accept."

This principle, stated in RFC 761 (TCP), guides implementers: generate compliant output, but tolerate non-compliant input when possible. However, this principle has been criticized in security contexts — liberal acceptance can mask protocol violations and reduce security. Modern best practice favors strict compliance on both sending and receiving for security-sensitive protocols.

### Reading RFCs Effectively

**The RFC reading strategy:**

Apply a modified three-pass method adapted for specifications:

Pass 1 — Context (5 minutes):

1. Read the abstract and status
2. Check whether the RFC is obsoleted or updated
3. Read the introduction for scope and motivation
4. Scan the table of contents

Pass 2 — Comprehension (30-60 minutes):

1. Read the terminology section carefully
2. Read the protocol specification section
3. Note every MUST, SHOULD, and MAY
4. Pay special attention to security considerations
5. Mark ambiguous or unclear sections

Pass 3 — Implementation (variable):

1. Re-read sections relevant to your implementation
2. Check errata for the RFC
3. Read referenced RFCs for context
4. Test edge cases against the specification
5. Verify your implementation handles every MUST and SHOULD

**Reading for different purposes:**

| Purpose | Sections to focus on | Sections to skip |
|---|---|---|
| Implementing a protocol | Protocol specification, security considerations, IANA considerations | Introduction (skim), related work |
| Understanding a protocol | Introduction, terminology, protocol specification (overview) | IANA considerations, full ABNF grammar |
| Debugging an implementation | Protocol specification (relevant section), security considerations | Full document |
| Evaluating security | Security considerations, protocol specification (attack surface) | IANA considerations |
| Writing about a protocol | Introduction, terminology, key concepts | Full protocol details |

**Dealing with RFC length:**

Some modern RFCs are very long. RFC 8446 (TLS 1.3) is about 160 pages. RFC 9110 (HTTP Semantics) is about 250 pages.

Strategy for long RFCs:

1. Read the abstract and introduction
2. Scan the table of contents
3. Read the sections relevant to your task
4. Use search to find specific terms
5. Do not attempt to read long RFCs cover-to-cover

### BCPs, STDs, and FYI Documents

The RFC series includes sub-series that organize related documents.

**BCP — Best Current Practice:**

BCP documents describe recommended practices for internet operation. They are not protocol specifications but guidance for implementers and operators.

| BCP | RFC(s) | Topic |
|---|---|---|
| BCP 14 | RFC 2119, RFC 8174 | Key words for requirement levels |
| BCP 38 | RFC 2827 | Network ingress filtering |
| BCP 56 | RFC 3434 | Operational security considerations |

BCP numbers are stable. BCP 14 always refers to the current set of RFCs defining key words, even if the underlying RFCs change.

**STD — Standard:**

STD documents are Internet Standards — protocols with significant deployment and multiple independent implementations.

| STD | RFC | Protocol |
|---|---|---|
| STD 5 | RFC 791 | Internet Protocol |
| STD 6 | RFC 792 | ICMP |
| STD 7 | RFC 9293 | TCP |
| STD 13 | RFC 1034, RFC 1035 | DNS |
| STD 68 | RFC 5321 | SMTP |

**FYI — For Your Information:**

The FYI sub-series (now discontinued) contained informational documents for the broader internet community.

### RFC Etiquette and Contribution

Developers can contribute to the RFC process by participating in IETF working groups, submitting Internet-Drafts, and providing implementation feedback.

**Internet-Drafts (I-Ds):**

Internet-Drafts are working documents of the IETF. They have a six-month lifetime and may become RFCs. Reading Internet-Drafts gives early access to evolving standards, but they should not be relied upon as stable references.

**Sending feedback:**

Every RFC includes the author's contact information. If you find an error or ambiguity, report it through the IETF's errata system.

**Participating in the IETF:**

1. Join a working group mailing list
2. Read the working group's charter and milestone
3. Read the working group's RFCs and Internet-Drafts
4. Contribute comments on the mailing list
5. Attend IETF meetings (remote participation is free)

## Glossary

| Term | Definition |
|---|---|
| ABNF | Augmented Backus-Naur Form — formal grammar used in RFCs to define protocol syntax |
| BCP | Best Current Practice — RFC sub-series for recommended practices |
| Errata | Corrections and clarifications to published RFCs |
| Historic | RFC status indicating the document is obsolete or superseded |
| IANA | Internet Assigned Numbers Authority — manages protocol registries |
| I-D | Internet-Draft — working document of the IETF, may become an RFC |
| IETF | Internet Engineering Task Force — develops internet standards |
| Implementation gap | Differences between what an RFC specifies and what an implementation actually does |
| Informational | RFC status for non-standard documentation |
| Internet Standard | RFC status for protocols with significant production deployment |
| Interoperability | Ability of different implementations to communicate correctly |
| Normative language | MUST, SHOULD, MAY, and related keywords defined in RFC 2119 |
| Obsoletes | When a newer RFC completely replaces an older one |
| Postel's Law | Be conservative in what you send, liberal in what you accept |
| Proposed Standard | RFC status for a stable, well-reviewed specification |
| RFC | Request for Comments — internet standard document |
| RFC Editor | Organization that publishes and maintains the RFC series |
| SHOULD | Strong recommendation; deviation requires good reason and understanding of consequences |
| STD | Standard — stable number pointing to the current RFC for an Internet Standard |
| Updates | When a newer RFC modifies specific parts of an older one |

## Quick References

- [RFC Editor](https://www.rfc-editor.org/) — search all RFCs, check status and errata
- [RFC 2119 / BCP 14](https://www.rfc-editor.org/rfc/rfc2119) — key words for requirement levels
- [RFC 7322](https://www.rfc-editor.org/rfc/rfc7322) — RFC style guide, explains document structure
- [IETF Working Groups](https://datatracker.ietf.org/wg/) — current standards development
- [IETF Datatracker](https://datatracker.ietf.org/) — track RFCs, drafts, and working group progress
- [Internet-Drafts](https://datatracker.ietf.org/drafts/) — search current Internet-Drafts
- [IETF Proceedings](https://www.ietf.org/how/meetings/) — meeting materials and proceedings
- [IP Stack RFCs: The Definitive Guide](https://www.iana.org/assignments/protocol-numbers) — IANA protocol number registry

## Next Steps

- [Reading Technical Specifications](specifications.md) — applying RFC reading skills to other types of specifications
- [Evaluating Sources & Claims](evaluating-sources.md) — assessing the credibility of technical content
