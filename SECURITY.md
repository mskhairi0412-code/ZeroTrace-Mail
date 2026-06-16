# Security Policy — ZeroTrace Mail

## Supported versions

| Version | Security fixes |
|---|---|
| 1.0.0 | ✅ Active |

---

## Reporting a vulnerability

**Do not open a public GitHub issue for security vulnerabilities.**

ZeroTrace Mail handles private communications for journalists, NGO workers, and privacy-sensitive users. A publicly disclosed vulnerability before a patch is available could put real people at risk.

### How to report

1. Use ZeroTrace Mail itself — send an encrypted message to the developer wallet address published at [zerotrace.app](https://zerotrace.app)
2. Or open a **private security advisory** on GitHub: Repository → Security → Advisories → New draft security advisory

### What to include

- Description of the vulnerability
- Steps to reproduce
- Affected versions
- Potential impact assessment
- Your suggested fix (optional but appreciated)

### Response timeline

| Stage | Target |
|---|---|
| Acknowledgement | Within 48 hours |
| Initial assessment | Within 7 days |
| Patch development | Within 30 days for critical issues |
| Coordinated disclosure | After patch is available |

We will credit researchers in the release notes unless you prefer to remain anonymous.

---

## Threat model

Understanding what ZeroTrace Mail protects against helps scope valid security reports.

### In scope

- Vulnerabilities that expose message plaintext to a third party
- Vulnerabilities that expose the wallet private key
- Vulnerabilities in the PIN/biometric unlock that allow bypass
- Vulnerabilities in encrypted local storage that allow extraction
- Vulnerabilities in the P2P delivery that allow message interception or injection
- Denial of service against the relay infrastructure
- Any issue that breaks the zero-knowledge property

### Out of scope

- IP address visibility to relay servers (known, documented limitation)
- Attacks requiring physical device access with root privileges (Android security model)
- Attacks requiring the device to already be unlocked
- Social engineering attacks against users
- Vulnerabilities in third-party dependencies that do not affect ZeroTrace Mail specifically
- Issues in PeerJS or TURN infrastructure not under our control

---

## Known limitations (not vulnerabilities)

These are documented design tradeoffs, not security issues:

- Relay servers observe IP addresses and connection timestamps
- PeerJS signaling server observes peer IDs and connection attempts
- No forward secrecy between message sessions (planned for v1.2.0)
- Message metadata (send time, approximate message count) may be inferable from traffic analysis

---

*ZeroTrace — Privacy by architecture, not by promise.*
