# Contributing to ZeroTrace Mail

Thank you for considering contributing to ZeroTrace Mail. This is a privacy-critical application — contributions are welcome but held to a high standard because the people who use this app depend on it being correct.

---

## Before you start

Read the [Privacy Policy](PRIVACY_POLICY.md) and [Security Policy](SECURITY.md) to understand what ZeroTrace Mail is trying to achieve and what the threat model is. Contributions that weaken the zero-knowledge or server-blind properties will not be accepted regardless of other merit.

---

## Types of contributions

### Bug reports
Open a GitHub issue with:
- Android version and device model
- ZeroTrace Mail version
- Steps to reproduce
- Expected vs actual behaviour
- Relevant logcat output (redact any personal data)

**Security bugs** — see [SECURITY.md](SECURITY.md). Do not open public issues.

### Feature requests
Open a GitHub issue describing:
- The use case you are solving
- How it fits the privacy model
- Any privacy implications of the feature

### Code contributions
1. Fork the repository
2. Create a feature branch: `git checkout -b feature/your-feature-name`
3. Make your changes
4. Test on a physical device (emulators do not test biometric or hardware Keystore correctly)
5. Commit with a clear message: `git commit -m 'Add: description of change'`
6. Push: `git push origin feature/your-feature-name`
7. Open a Pull Request against `main`

---

## Code standards

### Privacy requirements — non-negotiable

Any contribution must maintain these properties:

- **No new server endpoints** that receive personally identifying information
- **No logging of message content** — logcat statements must never include message bodies, subjects, or wallet addresses in plaintext
- **No new permissions** without documented justification and privacy impact assessment
- **No analytics or tracking** of any kind
- **Encryption must be applied before network transmission** — never after

### Code style

- Java — follow existing conventions in the codebase
- No third-party HTTP clients (OkHttp, Retrofit, etc.) without prior discussion — network code is security-sensitive
- Avoid static fields that hold decrypted content — minimise the window during which plaintext exists in memory
- Add comments explaining *why* security-sensitive code works the way it does, not just what it does

### Testing

- Test on API 24 (Android 7.0) — the minimum supported version
- Test both light and dark themes
- Test both portrait and landscape
- Test the full send/receive flow if your change touches P2PMailService or ComposeActivity

---

## Pull request checklist

Before submitting a PR, confirm:

- [ ] Tested on a physical Android device
- [ ] Tested on API 24 (minimum supported)
- [ ] No new permissions added without justification
- [ ] No plaintext message content in any log statement
- [ ] No new server endpoints that receive personal data
- [ ] Privacy Policy updated if the PR changes data handling
- [ ] Release notes section added if the PR adds a user-visible feature

---

## What will not be accepted

- Code that sends any user data to external servers in identifiable form
- Code that adds advertising, analytics, or crash reporting
- Features that require account registration or phone number verification
- Weakening of encryption primitives or key derivation parameters
- Dependency on closed-source SDKs

---

*ZeroTrace — Privacy by architecture, not by promise.*
