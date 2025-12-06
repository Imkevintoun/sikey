# 0002: SIKEY Identity Model & Key Derivation (Draft v0.1)

**Category:** Standards Track – Core  
**Status:** Draft  
**Created:** 2025-12-07  
**Author:** Kevin (Initiator of SIKEY), Open-Source Contributors  
**Depends on:** 0001 – SIKEY Vision & Principles  
**Discussion:** https://github.com/Imkevintoun/sikey/issues  

---

## 1. Abstract

This specification defines the **core identity model** for SIKEY and the
**Sovereign Identity Key (SIK) hierarchy** used to derive context-specific keys.

The goals are to:

- Enable **self-sovereign, locally generated** identity roots.
- Provide a **deterministic key hierarchy** for multi-device use and recovery.
- Ensure **strong privacy and non-correlation** between different usage contexts.
- Remain **compatible with DID / VC ecosystems** without being dependent on them.
- Work with SIKEY’s broader **hybrid stack** (off-chain credentials, zk-proofs, optional on-chain anchors).

This spec is **normative** for any implementation claiming SIKEY compatibility.

---

## 2. Motivation

Specification 0001 established the **vision and principles** of SIKEY:
self-sovereignty, privacy by design, decentralisation, openness, and
minimalism.

To move from philosophy to a working protocol, SIKEY needs a **clear identity
model** that answers:

- What is a **Sovereign Identity Key** (SIK)?  
- How is it generated, stored, and recovered?  
- How do we derive distinct keys for different apps/verifiers without
  correlation?  
- How can SIKEY interoperate with existing identity standards (e.g. DIDs, VCs)
  while avoiding centralised registries or surveillance?  

This specification provides those answers.

---

## 3. Scope

In scope:

- SIK generation and entropy requirements.
- Hierarchical key derivation model (HD-style).
- Context-specific keys (per-app / per-verifier / per-session).
- SIKEY identifiers:
  - **SIDs** (SIKEY Identifiers) for internal use.
  - Optional mapping to **DIDs** for interoperability.
- Device keys and recovery model (high-level).
- Privacy and threat-mitigation guarantees at the key/ID layer.

Out of scope (defined in other specs):

- Credential / attestation formats (0003+).  
- zk-proof circuit definitions (0004+).  
- Concrete wallet UX, storage formats, or backup UIs.  
- Governance / token mechanics.

---

## 4. Identity Model Overview

At a high level, SIKEY defines:

1. A user-controlled **Sovereign Identity Root**, represented by one or more
   **Sovereign Identity Keys (SIKs)**.
2. A **deterministic key hierarchy** derived from a master secret, enabling:
   - Multiple devices
   - Context-specific keys
   - Recovery from backup seeds
3. A set of **non-global identifiers**:
   - **SIDs** – local / protocol-level identifiers derived from SIKs.
   - Optional **DIDs** – when interoperability with DID-based ecosystems is needed.
4. A strict **context isolation** policy:
   - Keys and identifiers used in one context MUST NOT be linkable to another
     without explicit user consent.

SIKEY uses a **hybrid model**: its own identity format and derivation rules, with
the ability to **export / mirror identities as DIDs** when required.

---

## 5. Sovereign Identity Root

### 5.1 Master Secret

Each subject (person, organisation, device, etc.) has a **master secret**:

- Derived from a **BIP-39 compatible mnemonic** or equivalent seed format.
- Length SHOULD be at least 128 bits of entropy; 256 bits is RECOMMENDED.
- Generated **locally**, using a secure randomness source.
- MUST NOT be transmitted to any other party or remote service.

Implementations MAY support other seed formats, but MUST provide a way to
export/import via a human-usable backup mechanism (e.g. mnemonic phrase).

### 5.2 Root Key Type

For the initial version of SIKEY:

- Implementations **MUST** support an Ed25519-based signing key as the
  **Root SIK**.
- Implementations **MAY** additionally support more advanced curves
  (e.g. BLS12-381) for aggregation and future multi-party schemes.

The **Root SIK** is conceptually at derivation path:

- `m / 44' / 7777' / 0' / 0 / 0`  (example; see section 6.3)

This path is **not** used directly in everyday operations. It is the ultimate
root from which further keys are derived.

---

## 6. Hierarchical Key Derivation

SIKEY follows an **HD-wallet style** derivation model adapted for identity
contexts, not just accounts.

### 6.1 Goals

- Deterministic: same seed → same keys.
- Hierarchical: clear separation between layers (root, device, app, verifier,
  session).
- Non-correlatable: each context derives unique keys.
- Extensible: new contexts and subtrees can be added without breaking
  compatibility.

### 6.2 General Derivation Model

SIKEY uses a BIP32-like derivation:

- `ChildKey = HKDF(parent_key, context_info)`  
- With hardened and non-hardened levels as appropriate.

Implementations MAY reuse existing HD libraries, but MUST respect the namespace
and path semantics defined here.

### 6.3 Suggested Derivation Paths

This specification defines a **canonical path scheme**:

```text
m / purpose' / coin_type' / identity_index' / device_index / context_branch / context_index

Where:

purpose' = 4444' for SIKEY (avoids collision with BIP-44 wallets).

coin_type' = reserved for future chain-specific anchors; set to 0' for
purely off-chain use.

identity_index' = index of identities under a single seed (most users will use 0').

device_index = per-device key separation.

context_branch = numeric code for context type (app / verifier / session).

context_index = index within that context type.

Example mapping:

Level	Description
purpose'	4444' = SIKEY
identity_index'	Which identity under the same seed (0' default)
device_index	0 = primary device, 1 = phone, 2 = tablet…
context_branch	0 = wallet/management, 1 = app, 2 = verifier, 3 = session
context_index	index of particular app/verifier/session

Implementations MAY use additional sub-levels but MUST NOT repurpose existing
levels in incompatible ways.

7. Context-Specific Keys
7.1 Device Keys

Each device gets its own device master key:

m / 4444' / 0' / identity_index' / device_index


Used as the parent for all keys on that device.

Compromise of one device SHOULD NOT compromise other devices, assuming seeds
are not reused across devices.

7.2 Application Keys

For each application (e.g. “SIKEY Wallet”, “Chat App X”, “DAO Y”), derive:

m / 4444' / 0' / identity_index' / device_index / 1 / app_index


context_branch = 1 for apps.

app_index is a deterministic index computed from the app’s AppID.

AppID SHOULD be defined as a stable identifier, e.g.:

Hash of the app’s domain name, package name, or public key:

app_index = Truncate32( SHA256("sikey-app-id:" || canonical_app_identifier) )


Implementations MUST ensure:

Each app gets a unique keypair per device.

Apps cannot enumerate keys for other apps.

7.3 Verifier Keys

For each verifier (entity that checks proofs/credentials), derive:

m / 4444' / 0' / identity_index' / device_index / 2 / verifier_index


context_branch = 2 for verifiers.

verifier_index computed similarly from a canonical verifier identifier
(e.g. domain name, DID, or public key).

This ensures:

The user presents verifier-specific identifiers and proofs.

Verifiers cannot trivially correlate identities across other verifiers.

7.4 Session Keys

For high-privacy or short-lived interactions:

m / 4444' / 0' / identity_index' / device_index / 3 / session_index


context_branch = 3 for sessions.

session_index MAY be sequential or derived from random nonces.

Session keys are:

Short-lived.

Useful for one-off proofs, ephemeral chats, or temporary pseudonyms.

8. SIKEY Identifiers (SIDs) and DIDs
8.1 SIDs – SIKEY Identifiers

A SID is the primary protocol-level identifier used within SIKEY, derived
from a context-specific public key.

For a given context keypair (sk, pk):

SID = Base58Check( prefix || Hash(pk) )


Where:

prefix distinguishes SID types, e.g. 0x53 for SIKEY.

Hash SHOULD be a modern hash function (e.g. SHA-256 or BLAKE2b-256).

Properties:

Non-global: SIDs are context-specific; users have many.

Self-contained: derived from the user’s keys, no central registry.

Opaque: reveals nothing about subject identity.

8.2 DIDs – Optional Interoperability

To interoperate with DID-based ecosystems, SIKEY supports exporting certain
keys as DIDs. For example:

did:sikey:<encoded_pk_or_hash>

Implementations:

MAY implement a did:sikey DID method.

MAY map SIDs to other DID methods (e.g. did:key) where appropriate.

Important constraints:

Use of DIDs MUST NOT require registering in any global, centralised
registry.

DID documents SHOULD be resolvable via:

Local data

Off-chain distributed storage

Optional on-chain anchors (per SIKEY’s hybrid stack), but not a single
centralised state database.

9. Privacy & Threat Mitigation

The identity and key model MUST implement the following protections.

9.1 No Global Identifier

There is no single, global identifier for a subject at the protocol
level.

The Root SIK and master seed MUST never appear directly in network traffic.

9.2 Context Isolation

Keys for apps, verifiers, and sessions are derived via separate branches.

Reuse of the same key across independent contexts is NOT ALLOWED.

Implementations SHOULD prevent accidental reuse by design.

9.3 Metadata Minimisation

SIDs and DIDs SHOULD NOT embed human-readable or sensitive information.

Identifiers SHOULD be opaque hashes or encodings of public keys.

9.4 Compromise Containment

Compromise of a single device key:

SHOULD NOT compromise other devices.

Compromise of an app-specific key:

SHOULD NOT automatically compromise other apps or verifiers.

Recovery mechanisms SHOULD allow revocation/rotation of specific branches
without discarding the entire identity tree.

10. Device Sync & Recovery (High-Level)

The full UX and storage model are out of scope, but any implementation MUST:

Allow users to back up the master seed in a human-usable format.

Warn users that loss of seed = loss of control (unless advanced recovery is
implemented).

Avoid syncing the raw master seed via unencrypted channels.

Implementations MAY:

Use encrypted cloud backups (with clear warnings).

Support multi-party recovery (e.g. Shamir shares, social recovery).

Use hardware security modules (HSMs, secure enclaves, hardware wallets).

Advanced recovery patterns SHOULD be defined in a separate specification.

11. Interactions with Higher-Level Specs

Credentials & Attestations (0003) will reference:

SIDs or DIDs as subject identifiers.

Specific context keys for signing and verification.

ZK Proof Layer (0004) will use:

Public keys derived via this spec as identifiers in circuits.

Non-correlatable identifiers per verifier.

Hybrid Stack / On-Chain Anchors (future specs) will:

Use SIK-derived keys for registering optional anchors.

Ensure minimal leakage when anchoring on public ledgers.

12. Implementation Considerations

Existing HD wallet libraries may be reused if:

Paths follow SIKEY semantics.

Key derivation is compatible with Ed25519 (or chosen curve).

Care must be taken to:

Use hardened vs non-hardened derivation appropriately.

Prevent derivation path collisions with other systems using the same seed.

Implementations SHOULD provide:

A “view” showing which contexts exist (apps, verifiers).

A way to rotate or revoke keys for a specific context.

13. Open Questions (For Community Discussion)

This draft intentionally leaves some topics open for discussion:

Should SIKEY fix the curve to Ed25519 only in v1, or allow multiple
curves from the start?

What is the best canonical format for AppID and VerifierID?

Should SIKEY reserve a full BIP44 coin type, or keep using 0' with a
unique purpose?

How should multi-party identities (organisations, DAOs) derive shared SIKs?

How aggressively should DID interoperability be pushed in v1?

These questions SHOULD be tracked as GitHub issues and resolved before this
spec is marked Final.

14. References

0001: SIKEY Vision & Principles

BIP-32: Hierarchical Deterministic Wallets

BIP-39: Mnemonic code for generating deterministic keys

W3C Decentralized Identifiers (DIDs)

W3C Verifiable Credentials

Ed25519 Public-Key Signature System

15. License

This document is licensed under the Apache 2.0 License, the same as the
SIKEY repository.
