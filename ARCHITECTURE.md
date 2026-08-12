# PROX Ecosystem - Technical & Security Architecture

## 1. Overview & System Scope
PROX Ecosystem provides a distributed, privacy-first infrastructure designed to secure digital communications and empower small-to-medium digital operations. 

To maintain total transparency, the ecosystem differentiates between core specifications, live modules, and reference implementations:
* **ProtectPROX:** The core encryption protocol, key exchange standard, and security framework powering the ecosystem.
* **ChatPROX:** A live Reference Implementation / Proof-of-Concept (POC) application demonstrating client-side zero-knowledge message encryption.
* **PROX Sub-branches (BDPROX, BPPROX, PROXWallet):** Planned extensions currently in initial development and architectural design phases (see `ROADMAP.md`).

---

## 2. Cryptographic Architecture (ProtectPROX)

ProtectPROX operates strictly under a **Zero-Trust & Zero-Knowledge Protocol**. The underlying servers act purely as blind relays and never have access to unencrypted payloads or private keys.

### A. Key Exchange & Session Management
* **Asymmetric Handshake:** Elliptic-curve Diffie-Hellman (ECDH) over Curve25519 (`X25519`) is executed client-side to generate ephemeral shared secrets between endpoints.
* **Perfect Forward Secrecy (PFS):** Session keys are regenerated per communication stream. Compromising a single session key does not expose past or future messages.

### B. Payload Encryption
* **Symmetric Encryption:** Authenticated encryption via **AES-256-GCM** (Galois/Counter Mode).
* **Integrity & Authentication:** Every message payload includes an Authentication Tag (GMAC) ensuring protection against tampering, forgery, and replay attacks.

---

## 3. Infrastructure & Third-Party Services

### A. Stateless Virtual Relays (Hugging Face Integration)
To provide free, highly accessible infrastructure without sacrificing user security, ChatPROX utilizes virtual relay instances hosted on Hugging Face:
* **Zero Storage Policy:** Servers act strictly as ephemeral WebSockets / API relays. No plain text, message logs, or encryption keys are written to disk or database storage.
* **Client-Side Boundaries:** All cryptographic operations (Key Generation, Encryption, Decryption) occur exclusively inside the user's browser runtime before data leaves the device.

### B. Domain & Hosting Topology
* **Frontend Application:** Deployed on Netlify with automated build verification and strict HTTP Security Headers (HSTS, CSP, X-Frame-Options).
* **Domain Infrastructure:** Hosted on `prox.is-a.dev` in partnership and support of open-source, non-profit developer foundations.

---

## 4. Key Management & Distributed Node Topology
1. **Private Keys:** Stored locally on the user's device (Browser LocalStorage / IndexedDB with hardware-bound persistent tokens where applicable). Private keys never touch any server.
2. **Public Directory & Node Relay:** Infrastructure nodes managed by Qubadi Team facilitate peer discovery and payload routing without having visibility into payload contents.
