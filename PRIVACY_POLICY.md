# Privacy Policy — ZeroTrace Mail

**Last updated:** June 2026
**App version:** 1.0.0
**Package:** `com.zerotrace.mail`
**Developer:** ZeroTrace
**Contact:** via ZeroTrace Mail wallet address (published on website)

---

## Summary

ZeroTrace Mail is designed so that we **cannot** collect your data — not because we choose not to, but because the architecture makes it impossible. This policy explains what that means in practice.

If you are looking for the short version:

> We collect nothing. We store nothing about you. Your messages, your identity, and your encryption keys never leave your device in readable form. We have no account system, no server-side user database, and no analytics. There is nothing to delete because there is nothing stored.

---

## 1. Information we do not collect

The following categories of data are **never collected, transmitted, or stored** by ZeroTrace Mail or any infrastructure we operate:

- Your name, email address, or phone number
- Your message content — subject lines, message bodies, or attachments
- Your Ethereum wallet address or private key
- Your contact list or address book
- Your location, IP address as an identifier, or device location
- Your device identifiers (IMEI, advertising ID, Android ID)
- Usage analytics, session data, or behavioural data
- Crash reports or diagnostic logs
- Any biometric data (fingerprint processing happens entirely on your device via the Android Biometric API)

---

## 2. Information that exists on your device only

The following information is created and stored exclusively on your device. It is encrypted using Android Keystore and never transmitted to any server:

**Wallet keypair** — your Ethereum wallet address and associated private key are generated on your device using a cryptographically secure random number generator. The private key is encrypted with a key derived from your PIN using PBKDF2-SHA256 with 100,000 iterations. It never leaves your device.

**Messages** — all incoming and sent messages are stored locally using `EncryptedSharedPreferences` backed by Android Keystore. On devices with a hardware security module, the encryption key is hardware-bound and cannot be extracted even by a root-level attacker.

**Contact list** — names and wallet addresses you save to your address book are stored locally in encrypted storage.

**Settings and preferences** — your display name, PIN (as a derived key, not plaintext), theme preference, and privacy settings are stored locally only.

---

## 3. What relay infrastructure sees

ZeroTrace Mail uses relay servers to route messages. These servers are **server-blind** — they handle encrypted data they cannot read. Here is an honest account of what they observe:

### TURN relay server
Used for WebRTC NAT traversal when direct peer-to-peer connection is not possible.

**Sees:**
- Your IP address at the time of connection
- The recipient device's IP address
- Connection timestamps
- Encrypted traffic volume (bytes transferred)

**Cannot see:**
- Message content
- Subject lines
- Sender or recipient identity
- Wallet addresses
- Any personally identifying information beyond IP address

IP addresses are transient connection data. They are not stored in association with message content or user accounts because no user accounts exist.

### PeerJS signaling server
Used to establish the initial WebRTC connection between two devices.

**Sees:**
- Your peer ID (a 32-character string derived from your wallet address — not your full wallet, not your name)
- The fact that two peer IDs attempted connection
- Connection timestamps
- Your IP address

**Cannot see:**
- Wallet addresses in full
- Message content
- Any personally identifying information

### ZeroTrace relay buffer
Used to hold encrypted messages when the recipient is offline.

**Stores:**
- Encrypted message ciphertext (AES-256-GCM — unreadable without the session key)
- Recipient peer ID (wallet-derived identifier, not a name or email)
- Message timestamp

**Does not store:**
- Decryption keys (these exist only on sender and recipient devices)
- Sender identity
- Message content in any readable form

Messages are deleted from the relay buffer immediately upon successful delivery to the recipient device.

---

## 4. Legal requests and law enforcement

ZeroTrace Mail is designed to be resistant to data requests — not through non-cooperation, but through architectural impossibility.

If a government authority issues a valid legal order to ZeroTrace or to relay infrastructure operators, the following is what could be produced:

- Encrypted ciphertext with no associated decryption key
- IP connection logs (if retained by relay operator — see relay operator policies)
- Peer ID strings (wallet-derived identifiers not linked to real-world identity)

**What cannot be produced under any legal order:**
- Message content (we do not have it)
- Sender or recipient names (we do not have them)
- Wallet private keys (they exist only on user devices)
- A user database (one does not exist)

This is not a policy position — it is a cryptographic fact. The data does not exist in a form that could be meaningfully handed over.

---

## 5. Children's privacy

ZeroTrace Mail does not knowingly collect any information from children under the age of 13 (or 16 in the European Union). The app has no registration system and collects no personal information from any user regardless of age. If you are a parent and believe your child has used this application, there is no information to request removal of — none is collected.

---

## 6. Third-party services

ZeroTrace Mail uses the following third-party open-source components. None of these collect personal data:

| Component | Purpose | Privacy policy |
|---|---|---|
| PeerJS | WebRTC signaling abstraction | Open source — self-hostable |
| Android Keystore | Hardware key storage | Android platform — Google |
| AndroidX Biometric | Fingerprint authentication | Android platform — Google |
| AndroidX Security | EncryptedSharedPreferences | Android platform — Google |

The Google Android platform components (Keystore, Biometric, Security) operate entirely on your device and do not transmit data to Google as part of their normal operation within ZeroTrace Mail.

---

## 7. Data retention

**On your device:** Data is retained until you delete individual messages, clear all messages via Settings, or wipe the app entirely. Uninstalling the app removes all local data.

**On relay servers:** Encrypted message ciphertext is retained only until delivery to the recipient device, after which it is deleted. If delivery cannot be confirmed, ciphertext is retained for a maximum period defined by relay operator policy before being purged.

**Connection logs:** IP connection logs on relay servers are subject to the relay operator's retention policy. ZeroTrace-operated relay infrastructure retains connection logs for no longer than 24 hours.

---

## 8. Your rights

Because ZeroTrace Mail collects no personal data linked to your identity, traditional data subject rights (access, rectification, erasure, portability) apply in a limited way:

**Right to access:** There is no user account or profile to access. Your data exists only on your device under your control.

**Right to erasure:** You can delete all local data at any time via Settings → Wipe wallet and reset app. This permanently removes all messages, contacts, and identity data from your device. Relay-held ciphertext is automatically purged on delivery or expiry.

**Right to portability:** Your messages are stored in encrypted form on your device. ZeroTrace Mail does not currently provide an export function — this is planned for a future release.

**Right to object:** There is no data processing to object to. No profiling, no analytics, no marketing.

---

## 9. International users

ZeroTrace Mail is available globally. Relay infrastructure may be operated in various jurisdictions. Because relay servers hold only encrypted ciphertext without decryption keys, cross-border data transfer concerns are limited — the data that crosses borders cannot be read in any jurisdiction.

Users in the European Union: the processing described in this policy involves no personal data as defined under GDPR Article 4(1) beyond transient IP address connection data. IP addresses visible to relay infrastructure are not stored in association with user identities.

---

## 10. Security

ZeroTrace Mail implements the following security measures:

- **AES-256-GCM** message encryption — current gold standard for symmetric encryption
- **ECDH-P256** key exchange — ephemeral session keys per message session
- **PBKDF2-SHA256** PIN key derivation — 100,000 iterations, resistant to brute force
- **Android Keystore** — hardware-backed key storage on supported devices
- **Certificate pinning** — not applicable (no ZeroTrace server endpoints require authenticated sessions)
- **Screenshot prevention** — enabled by default, blocks screen capture

No security system is perfect. We recommend users in high-risk environments combine ZeroTrace Mail with a VPN or Tor to protect IP metadata, which is the primary remaining attack surface.

---

## 11. Changes to this policy

If this privacy policy changes materially, the updated policy will be published in the app repository and noted in the release notes. Because we collect no contact information, we cannot notify users directly — please check the repository for updates.

Changes will never retroactively expand data collection beyond what is described here.

---

## 12. Contact

ZeroTrace Mail does not maintain a traditional support email address (doing so would require us to collect your email). For questions about this policy:

- Open an issue on the [GitHub repository](https://github.com/zerotrace/zerotrace-mail/issues)
- Use the ZeroTrace Mail app itself to contact the developer wallet address (published on the website)

---

*This privacy policy was written to be read, not to obscure. If something is unclear, open an issue and we will improve it.*

*ZeroTrace Mail — Privacy by architecture, not by promise.*
