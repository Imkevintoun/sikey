0001: SIKEY Vision & Principles (Draft v0.1)

Category: Informational
Status: Draft
Created: 2025-12-07
Author: Kevin (Initiator of SIKEY), Open-Source Contributors
Discussion: https://github.com/Imkevintoun/sikey/issues

1. Abstract

This document describes the foundational vision, mission, and design principles of the SIKEY – Sovereign Identity Key protocol.
It establishes the philosophical and technical baseline for all subsequent SIKEY specifications.

It is non-normative but highly influential: all future technical decisions, specifications, and implementations should align with the vision and principles defined here.

2. Motivation

Digital identity today is dominated by:

Governments

Corporations

Centralised intermediaries

Surveillance-based business models

These systems are fragile, non-portable, and often hostile to privacy, autonomy, and human freedom.

There is no global, open, decentralised alternative for identity that:

Is self-sovereign

Enables privacy-preserving verification

Is resistant to capture by governments or corporations

Is interoperable across digital and physical systems

Is simple enough for mass adoption

SIKEY exists to fill this gap.

The mission of SIKEY is to build:

A sovereign identity layer for humanity — open, decentralised, privacy-first, and impossible to monopolise.

3. SIKEY Vision Statement

SIKEY envisions a world where:

Identity belongs to people, not systems.

Verification does not require exposure.

Privacy is not a privilege — it is the default.

No institution, corporation, or government can unilaterally control identity.

Anyone, anywhere, can prove who they are without giving away everything they are.

SIKEY aims to become:

The Bitcoin of digital identity

A neutral public good

A cryptographic foundation for trust

An interoperable identity rail for open systems

A tool of sovereignty for individuals and communities

4. Core Principles (Non-Negotiables)

These serve as the “constitution” of SIKEY.
Changing them requires broad community consensus and governance process.

4.1 Self-Sovereignty

Users must have:

Full control over their identity keys (SIKs)

The ability to generate, rotate, and revoke keys

No reliance on a single authority (government, corporation, foundation)

No implementation should force users into custodial or dependency models.

4.2 Privacy by Design

SIKEY must:

Minimise data sharing at every layer

Support zero-knowledge proofs as a first-class capability

Prevent correlating identity usage across contexts

Avoid centralised registries, ledgers, or tracking systems

The protocol should empower selective disclosure — proving only the necessary fact, nothing more.

4.3 Decentralisation and Anti-Capture

The SIKEY ecosystem must resist:

State capture

Corporate monopolisation

Foundation dominance

Undue influence from special interests

Decentralisation applies to:

Governance

Issuers

Verifiers

Storage

Key management

Implementations

No single governing body should be able to unilaterally define or change the protocol.

4.4 Openness and Interoperability

SIKEY is an open protocol, not a platform.

Anyone can implement it

Anyone can fork it

Anyone can build on top of it

It should interoperate with existing standards where appropriate (DIDs, VCs, ZK standards, etc.)

SIKEY should integrate naturally with the broader decentralised ecosystem.

4.5 Minimalism and Practicality

The protocol should:

Be simple enough to understand

Be implementable with minimal resources

Avoid unnecessary cryptographic complexity

Prioritise practical use cases

Over-engineering is a threat to adoption.

4.6 Security as a First Principle

Because SIKEY touches identity, privacy, and potentially life-altering trust relationships:

Security considerations must inform every decision

Cryptography should be modern, peer-reviewed, and standardised

Key compromise and metadata leakage must be mitigated

Implementations should be auditable and transparent

Security cannot be bolted on later.

5. Architectural Foundations

This section outlines the conceptual model that future specs will detail.

5.1 Sovereign Identity Keys (SIKs)

The foundational element of SIKEY is the Sovereign Identity Key, a user-controlled root key pair.

Properties:

Generated locally

Never shared

Used to derive context-specific keys

Portable across devices and backup systems

Upgradable and revocable

Future specs will define:

Key derivation hierarchy

Recovery mechanisms

Device syncing approaches

5.2 Decentralised Identifiers (DIDs) or DID-like Models

SIKEY may use:

DID Core

A custom DID method

An entirely new lightweight identifier model

Requirements:

Non-correlation

Privacy-preserving resolution

No global registry required

Support for local-only identifiers

5.3 Attestations & Verifiable Credentials

SIKEY will leverage:

VC-style credentials

Attestations signed by issuers

Community, institutional, or peer issuers

ZK-enabled proofs of claims

Credentials must be:

Locally held

User-controlled

Never broadcast to a central authority

5.4 Zero-Knowledge Proof Layer

SIKEY must support ZK systems enabling proofs for:

Age

Citizenship

Licence ownership

Membership

Human verification

Reputation

KYC/AML compliance without exposing personal data

The ZK subsystem must be:

Modular

Efficient

Cryptographically sound

5.5 Governance & Token Model (Optional, TBD)

A token may be introduced only if:

It improves decentralisation

It coordinates incentives

It does not create a rent-seeking layer

It does not become a gatekeeper

Any governance mechanism must be:

Transparent

Community-driven

Resistant to capture

Optional for protocol usage

6. Non-Goals

SIKEY is not:

A government-backed digital ID

A surveillance tool

A centralised identity database

A replacement for passports or driver’s licenses (though it can verify them)

A commercial identity marketplace

A blockchain-only system

A company or corporation

SIKEY is a public good protocol.

7. Guiding Ethical Commitments

SIKEY commits to:

Protecting human dignity

Protecting vulnerable populations

Avoiding harm through misuse

Ensuring transparency

Encouraging community review and criticism

Building tools that empower, not control

The protocol exists to serve humanity — not institutions.

8. Design Decision Alignment Test

Every technical proposal should answer:

Does this increase or decrease self-sovereignty?

Does this enhance or weaken privacy?

Does this centralise power or decentralise it?

Does this align with minimalism and practicality?

Does this create dependencies on governments or corporations?

Is this resilient against surveillance and coercion?

If a proposal violates these principles — it should not be part of SIKEY.

9. Future Work

Future specifications will define:

Key management

Credential formats

Attestation workflows

ZK proof circuits

DID model

Revocation systems

Governance structures

Client libraries

Reference wallet implementation

Integration patterns

Security audits

10. References

W3C Decentralized Identifiers (DIDs)

W3C Verifiable Credentials (VCs)

ZK-SNARKs, STARKs, Bulletproofs, Halo2

Bitcoin Whitepaper

Zcash Protocol

Privacy by Design (Cavoukian)

Solid Principles for Decentralized Identity

11. License

This document is licensed under the Apache 2.0 License.
