<div align="center">

<img src="app/src/main/res/mipmap-xxxhdpi/ic_launcher.png" width="96" height="96" alt="ZeroTrace Mail logo" />

# ZeroTrace Mail

**Private encrypted email · Zero-knowledge · Server-blind · Wallet identity**

[![Android](https://img.shields.io/badge/Android-7.0%2B-green?logo=android)](https://developer.android.com)
[![API](https://img.shields.io/badge/API-24%2B-brightgreen)](https://developer.android.com/about/versions/nougat)
[![License](https://img.shields.io/badge/License-MIT-blue)](LICENSE)
[![Version](https://img.shields.io/badge/Version-1.0.0-orange)](https://github.com/zerotrace/zerotrace-mail/releases)

*"Privacy by architecture, not by promise."*

</div>

---

## What is ZeroTrace Mail?

ZeroTrace Mail is an Android email app that uses your **Ethereum wallet address** as your identity and **AES-256-GCM end-to-end encryption** for every message. No accounts. No phone numbers. No server ever reads your messages — not because we promise not to, but because we mathematically cannot.

Messages are delivered **peer-to-peer over WebRTC**. Servers relay encrypted packets they cannot open. Your private key never leaves your device.

---

## Screenshots

> *Add screenshots here after first build*

| Inbox | Read | Compose | Contacts | Keys |
|---|---|---|---|---|
| *inbox.png* | *read.png* | *compose.png* | *contacts.png* | *keys.png* |

---

## Features

- **Wallet identity** — your Ethereum address is your email address. Generated on-device. No registration.
- **AES-256-GCM encryption** — every message encrypted before leaving your device
- **ECDH-P256 key exchange** — session keys derived from wallet keypairs, never transmitted
- **P2P delivery** — direct device-to-device when both online, relay fallback when offline
- **Encrypted local storage** — Android Keystore backed EncryptedSharedPreferences
- **PIN + biometric unlock** — 6-digit PIN with fingerprint support
- **Address book** — save contacts by name, resolve to wallet on compose
- **Light and dark theme** — system-aware with manual toggle
- **Screenshot prevention** — blocks screen capture by default
- **Zero telemetry** — no analytics, no crash reporting, no usage data

---

## Architecture

```
┌─────────────────────────────────────────────────────────┐
│                    SENDER DEVICE                        │
│                                                         │
│  Compose → Encrypt (AES-256-GCM) → P2P or Relay        │
│               ↑                                         │
│         Session key                                     │
│     (ECDH-P256 local)                                   │
│     never transmitted                                   │
└─────────────────────────────────────────────────────────┘
                          │
                          │  [ciphertext only]
                          ▼
┌─────────────────────────────────────────────────────────┐
│                TURN / RELAY SERVER                      │
│                                                         │
│  Sees: encrypted bytes, IP, timestamp                   │
│  Cannot see: content, identity, keys                    │
└─────────────────────────────────────────────────────────┘
                          │
                          │  [ciphertext only]
                          ▼
┌─────────────────────────────────────────────────────────┐
│                   RECIPIENT DEVICE                      │
│                                                         │
│  Receive → Decrypt (local key) → Display               │
└─────────────────────────────────────────────────────────┘
```

### Security primitives

| Layer | Algorithm |
|---|---|
| Message encryption | AES-256-GCM |
| Key exchange | ECDH-P256 |
| PIN key derivation | PBKDF2-SHA256 · 100,000 iterations |
| Local storage | EncryptedSharedPreferences · AES256-GCM |
| Identity | Ethereum wallet keypair (secp256k1) |
| Transport | WebRTC · TURN relay fallback |

---

## Getting started

### Requirements

- Android Studio Hedgehog (2023.1.1) or later
- Android SDK 34
- Java 8+
- A physical or virtual Android device running API 24+

### Build from source

```bash
# Clone the repository
git clone https://github.com/zerotrace/zerotrace-mail.git
cd zerotrace-mail

# Open in Android Studio
# File → Open → select the zerotrace-mail folder

# Or build from command line
./gradlew assembleDebug

# Install on connected device
./gradlew installDebug
```

### First launch

1. Enter your display name — stored locally only, never transmitted
2. Your Ethereum wallet address is generated on-device — **write it down**
3. Set a 6-digit PIN to protect your wallet key
4. Start sending encrypted mail

---

## Project structure

```
app/src/main/
├── java/com/zerotrace/mail/
│   ├── ZeroTraceApp.java          # Application class — init on startup
│   ├── PinUnlockActivity.java     # Lock screen with PIN + biometric
│   ├── SetupActivity.java         # First-launch onboarding
│   ├── MainActivity.java          # Inbox
│   ├── ComposeActivity.java       # New message / reply
│   ├── ReadMailActivity.java      # Read email with quoted reply
│   ├── SentActivity.java          # Sent messages
│   ├── AddressBookActivity.java   # Contacts — alphabetical, search
│   ├── KeysActivity.java          # Identity card + crypto details
│   ├── SettingsActivity.java      # Settings — PIN, privacy, wipe
│   ├── MailMessage.java           # Message data model
│   ├── MailStore.java             # In-memory store + persistence
│   ├── MailAdapter.java           # RecyclerView adapter
│   ├── SecureStorage.java         # EncryptedSharedPreferences wrapper
│   ├── P2PMailService.java        # WebRTC P2P delivery engine
│   ├── AddressBook.java           # Contact storage and resolution
│   └── NotificationHelper.java   # Push notification helper
│
├── res/
│   ├── layout/                    # Activity layouts (XML)
│   ├── drawable/                  # Vector icons and backgrounds
│   ├── values/                    # Colors, strings, themes
│   └── mipmap-*/                  # Launcher icons (all densities)
│
└── AndroidManifest.xml
```

---

## Privacy model

### What ZeroTrace Mail guarantees

| Property | Status | How |
|---|---|---|
| Message content private | ✅ Guaranteed | AES-256-GCM — servers handle ciphertext only |
| Keys never leave device | ✅ Guaranteed | ECDH session keys derived locally |
| No account registration | ✅ Guaranteed | Wallet identity — no server sign-up |
| No message storage on server | ✅ Guaranteed | Relay holds ciphertext max until delivery |
| No telemetry or analytics | ✅ Guaranteed | Zero data collection code |

### Honest disclosures

| Property | Status | Detail |
|---|---|---|
| IP address hidden | ⚠️ Partial | TURN relay sees your IP — use a VPN for full IP privacy |
| Fully serverless | ⚠️ Partial | TURN + signaling servers used — but server-blind by design |
| Works when offline | ⚠️ Requires relay | Relay buffer must be reachable for async delivery |
| Push notifications | ❌ v1.0.0 | App must be open — planned for v1.1.0 |

### Legal resistance

A government subpoena of ZeroTrace relay infrastructure would produce:
- Encrypted ciphertext with no associated decryption key
- IP addresses and connection timestamps
- Nothing that identifies message content, sender, or recipient

This is a consequence of the cryptographic architecture, not a policy promise.

---

## Permissions

```xml
<uses-permission android:name="android.permission.INTERNET" />
<uses-permission android:name="android.permission.USE_BIOMETRIC" />
<uses-permission android:name="android.permission.USE_FINGERPRINT" />
<uses-permission android:name="android.permission.VIBRATE" />
```

**No permissions for:** contacts, camera, microphone, location, phone state, external storage, or any sensor. ZeroTrace Mail accesses only what is strictly necessary for encrypted messaging.

---

## Roadmap

- [ ] **v1.1.0** — Conversation threading · QR wallet sharing · Delivery ticks · Push notifications
- [ ] **v1.2.0** — Encrypted attachments · Nostr relay integration · Swipe gestures
- [ ] **v1.3.0** — Multi-wallet identity · Message expiry timer · Full-text search
- [ ] **v2.0.0** — ZeroTrace ecosystem integration (Messenger · Maps · Cam · Vault)

---

## Contributing

Contributions are welcome. Please read [CONTRIBUTING.md](CONTRIBUTING.md) before opening a pull request.

- Fork the repository
- Create a feature branch (`git checkout -b feature/your-feature`)
- Commit your changes (`git commit -m 'Add your feature'`)
- Push to the branch (`git push origin feature/your-feature`)
- Open a Pull Request

Security vulnerabilities should be reported privately — see [SECURITY.md](SECURITY.md).

---

## License

```
MIT License

Copyright (c) 2026 ZeroTrace

Permission is hereby granted, free of charge, to any person obtaining a copy
of this software and associated documentation files (the "Software"), to deal
in the Software without restriction, including without limitation the rights
to use, copy, modify, merge, publish, distribute, sublicense, and/or sell
copies of the Software, and to permit persons to whom the Software is
furnished to do so, subject to the following conditions:

The above copyright notice and this permission notice shall be included in all
copies or substantial portions of the Software.

THE SOFTWARE IS PROVIDED "AS IS", WITHOUT WARRANTY OF ANY KIND, EXPRESS OR
IMPLIED, INCLUDING BUT NOT LIMITED TO THE WARRANTIES OF MERCHANTABILITY,
FITNESS FOR A PARTICULAR PURPOSE AND NONINFRINGEMENT. IN NO EVENT SHALL THE
AUTHORS OR COPYRIGHT HOLDERS BE LIABLE FOR ANY CLAIM, DAMAGES OR OTHER
LIABILITY, WHETHER IN AN ACTION OF CONTRACT, TORT OR OTHERWISE, ARISING FROM,
OUT OF OR IN CONNECTION WITH THE SOFTWARE OR THE USE OR OTHER DEALINGS IN THE
SOFTWARE.
```

---

## Related projects

| App | Description |
|---|---|
| [ZeroTrace Messenger](https://github.com/zerotrace) | Encrypted P2P messaging |
| [ZeroTrace Maps](https://github.com/zerotrace) | Private location sharing |
| [ZeroTrace Cam](https://github.com/zerotrace) | Encrypted camera vault |
| [ZeroTrace Vault](https://github.com/zerotrace) | Encrypted file storage |

---

<div align="center">

**ZeroTrace Mail** · Privacy by architecture, not by promise

[Website](https://zerotrace.app) · [Issues](https://github.com/zerotrace/zerotrace-mail/issues) · [Releases](https://github.com/zerotrace/zerotrace-mail/releases)

</div>
