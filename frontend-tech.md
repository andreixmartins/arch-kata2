# Frontend Tech

We decided to use Flutter instead of Native solutions or web.

## Why

1. Native
Flutter delivers near‑native performance with a single codebase, offering faster development and consistent UI.
Native apps provide stronger security, deeper OS integration, and the highest possible performance.

Overall: Flutter trades a bit of power for major productivity gains.

2. Web
Flutter offers better security, offline support, device attestation, and smoother realtime performance than mobile web.
Web is lighter, requires no installation, but lacks hardware protections and is more vulnerable to automation.

Overall: Flutter provides a more reliable, secure, and controlled environment for critical applications.

## Flutter
### Pros

- **One Codebase** – Faster development and simpler long‑term maintenance.
- **Near‑Native Performance** – Compiled to ARM, fast enough for required workloads.
- **Consistent UI** – Same UI across platforms (may be a con depending on product expectations).
- **Strong Security** – Access to Secure Enclave/Keystore via plugins (though still below native strength).
- **Realtime Ready** – Solid support for WebSockets, gRPC, and Streams.

### Cons

- **Lower Peak Performance** – Slightly behind true native in edge GPU/compute scenarios (not relevant here).
- **Flutter Engine Dependency** – Larger binaries and custom rendering layer (acceptable given our scope).
- **Native Integration Needs** – Some features (attestation, system APIs) may require platform‑specific code.
- **Smaller Ecosystem** – Fewer libraries and integrations compared to native Swift/Kotlin.
- **Framework Release Cycle** – Updates and fixes dependent on Flutter/Google timelines.
