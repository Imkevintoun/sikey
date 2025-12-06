# SIKEY – Sovereign Identity Key

**SIKEY** (Sovereign Identity Key) is an open-source, community-driven project to create a **self-owned, censorship-resistant digital identity** layer that does **not** depend on governments or Big Tech.

> Digital ID without surveillance. Identity without permission.  
> For humans, not systems.

---

## 🔹 What is SIKEY?

SIKEY is a protocol + reference implementation for:

- **Self-sovereign identity** – you control your keys, credentials, and revocation.
- **Privacy-preserving verification** – prove *facts* about yourself (age, licence, access, etc.) without exposing all your data, using **zero-knowledge proofs** and modern cryptography.
- **Interoperable identity** – portable across apps, platforms, and chains (where it makes sense), instead of being locked to a single government or platform.
- **Open, forkable, and community-governed** – anyone can inspect, contribute, fork, or build on top.

Right now, this repo is:

- A living **manifesto**,  
- An evolving **whitepaper & protocol spec**,  
- The home for **reference implementations** as they emerge.

---

## ⚡ Why SIKEY exists

Most “digital ID” systems today are:

- **Centralised** – controlled by governments or corporations.
- **Surveillance-friendly** – built on data collection and tracking.
- **Non-portable** – tied to one jurisdiction, one app, or one company.
- **Fragile** – a single hack or breach can expose millions of identities.

We believe:

1. **Identity should be self-sovereign**, not state-sovereign.  
2. **Privacy is the default**, not an add-on.  
3. **Open protocols > walled gardens.**  
4. **No single entity** (including us) should control the identity layer of the internet.

SIKEY is an attempt to create:

> *“The Bitcoin of digital identity”* – neutral, open, and not owned by anyone.

---

## 🧱 High-Level Architecture (v0 concept)

> This will evolve – see `/docs/architecture-overview.md` for updates.

Core components:

- **Sovereign Identity Keys (SIKs)**  
  - Your root **Sovereign Identity Key** pair.  
  - Used to derive per-context keys (per app / per verifier) to avoid correlation.

- **Decentralised Identifiers (DIDs)**  
  - SIKEY uses DIDs (or a DID-like model) for interoperable identifiers.
  - Support for different DID methods (on-chain and off-chain) is planned.

- **Verifiable Credentials & Attestations**  
  - Issuers (institutions *or* community verifiers) sign credentials.  
  - Users hold these locally in their wallet or secure storage.

- **Zero-Knowledge Proof Layer**  
  - Prove “I am over 18”, “I own this licence”, “I passed KYC”, etc.,
    without revealing the raw credential or sensitive data.

- **Governance & Token (SIK) – TBD**  
  - Potential token to coordinate incentives, not to gate access.  
  - Governance model designed to avoid capture by states or large actors.

---

## 🗺️ Project Status

Very early-stage and intentionally open.

- ✅ Manifesto stub – [`/docs/manifesto.md`](docs/manifesto.md)  
- ✅ Whitepaper stub – [`/docs/whitepaper.md`](docs/whitepaper.md)  
- 🧪 Design & research – threat models, architecture, and cryptographic choices  
- ⏳ Reference implementation – to be discussed and built *with* the community

If you care about **sovereignty, privacy, cryptography, Web3, or open governance**, you’re in the right place.

---

## 🤝 How to Get Involved

We’re especially looking for:

- Cryptographers & security engineers  
- Protocol / backend devs (Rust, Go, TypeScript, etc.)  
- Identity / SSI / DID experts  
- Product & UX designers who care about privacy  
- Legal / policy minds who understand digital ID risks  
- Builders who give a damn about freedom

### 1. Join the discussion

1. ⭐ Star this repo to follow progress.  
2. Check the [Issues](../../issues) for:
   - `discussion` – ideas, questions, design threads  
   - `good first issue` – things new contributors can pick up  
3. Open a new issue if:
   - You want to propose a change (use RFC format if it’s big).  
   - You want to start a design discussion.

### 2. Contribute

See [`CONTRIBUTING.md`](CONTRIBUTING.md) for:

- How we work  
- Branch / PR model  
- How to propose protocol changes  

---

## 🛡️ Security

This project touches **identity, cryptography, and privacy**.  
Please *do not* disclose potential vulnerabilities in public issues.

Instead, see [`SECURITY.md`](SECURITY.md) for how to report security issues responsibly.

---

## 🧭 Roadmap

High-level:

1. Document the vision clearly (manifesto + whitepaper v1).  
2. Agree the threat model & core principles.  
3. Design the minimal viable protocol for SIKEY v1.  
4. Build a reference implementation + example apps.  
5. Distribute governance and avoid capture.

More detail: [`ROADMAP.md`](ROADMAP.md).

---

## 📜 License

SIKEY is open-source under the **Apache 2.0 License** – see [`LICENSE`](LICENSE).

You are free to use, modify, fork, and build on this project, subject to the terms of the license.

---

## 🙏 A Note from the Initiator

> “I started SIKEY because I don’t want my identity – or my kids’ identity – to be owned by governments, banks, or Big Tech.  
> I don’t have all the answers and I can’t build this alone.  
> If this vision resonates with you, help shape it. Fork it. Challenge it. Improve it.  
> Let’s build an identity layer that belongs to people — not to systems.”

— Kevin (initiator of SIKEY)
