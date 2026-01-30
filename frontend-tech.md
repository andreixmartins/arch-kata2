# Frontend Architecture & Authentication

This document outlines the strategic decision to utilize **Flutter** for the frontend and the multi layer **Authentication Strategy** required to secure the application.

---

## 1. Frontend: Flutter
We have selected **Flutter** as the primary framework, positioning it as the optimal middle ground between high performance native development and the flexibility of the web.

### 1.1 Strategic Rationale
| Platform | Comparison vs. Flutter |
| :--- | :--- |
| **Native** | Flutter delivers near native performance with a single codebase. While Native offers deeper OS integration, Flutter significantly increases development velocity. |
| **Web** | Flutter provides superior security, offline support, and hardware backed device attestation compared to standard mobile web, creating a more controlled environment. |

### 1.2 Pros & Cons
#### **Pros**
* **One Codebase:** Streamlines maintenance and ensures feature parity across iOS and Android.
* **Near Native Performance:** Compiled to ARM/Machine code for high-speed execution.
* **Consistent UI:** Pixel-perfect rendering across all devices via the Skia/Impeller engines.
* **Security Access:** Direct integration with Secure Enclave (iOS) and Keystore (Android).
* **Realtime Ready:** Native-level support for WebSockets, gRPC, and Streams.

#### **Cons**
* **Engine Overhead:** Larger binary sizes compared to pure native apps.
* **Native Integration:** Advanced features may require platform specific "Method Channels" (Swift/Kotlin).
* **Ecosystem:** Some niche libraries may be less mature than their native counterparts.

---

## 2. Authentication Strategy
The application employs a layered authentication model, combining industry- tandard protocols with hardware-level local security.

### 2.1 Primary Method: OAuth2 / OpenID Connect (OIDC)
OAuth2 is the foundational identity layer, delegating credential management to a trusted Identity Provider (IdP) such as Auth0, Keycloak, or Azure AD.

* **Flow:** Authorization Code Flow with **PKCE** (Proof Key for Code Exchange).
* **Token Output:**
    * **Access Token:** Short lived; used for API authorization.
    * **ID Token:** Contains user profile information.
    * **Refresh Token:** Used to obtain new access tokens without user intervention.



### 2.2 Secondary Method: Biometric Authentication
Biometrics serve as a **convenience and local re-authentication layer**; they do not replace the primary identity asserted by OAuth2.

* **Logic:** Once the user logs in via OAuth2, biometrics "unlock" the secure storage where tokens are kept.
* **Supported Modalities:** Face ID, Touch ID, Fingerprint, or Device PIN/Pattern.
* **Implementation:** Utilizes the device’s hardware-backed security modules.

### 2.3 Token Storage & Security
Security implementation differs by platform:

* **Mobile (Flutter):** Tokens are stored in **Keychain (iOS)** and **Keystore (Android)**. This ensures tokens are not accessible to other apps.
* **Web:** In memory storage is preferred to mitigate XSS risks. LocalStorage is avoided for long lived sensitive tokens.

---

## 3. Session & Token Management
To maintain a secure yet seamless user experience, the system follows these lifecycle rules:

1.  **Backend Validation:** Every request must validate the token’s signature, expiration, and scopes.
2.  **Short lived Access:** Access tokens expire quickly to minimize the window of risk if intercepted.
3.  **Silent Refresh:** The Flutter app uses Refresh Tokens to renew sessions automatically before the Access Token expires.
4.  **Graceful Failure:** If a refresh fails, the app must clear local state and redirect to the primary login screen.

> **Important:** Biometric failure must never lock a user out of their account; it should simply force a fallback to the primary OAuth2 login flow.
