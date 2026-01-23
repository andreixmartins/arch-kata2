# Authentication Methods

## 1. OAuth2 / OpenID Connect (Primary)
OAuth2 with OpenID Connect (OIDC) is the primary authentication mechanism.

Flow

- Authorization Code Flow with PKCE (recommended for mobile & SPA)
- User authenticates via a trusted Identity Provider (IdP)
(e.g. Auth0, Cognito, Keycloak, Azure AD)


- Frontend receives:

  - Access Token (API authorization)

  - ID Token (user identity)

  - Optional Refresh Token

### Why OAuth2
- Industry standard
- Works across mobile and web
- Delegates password handling to IdP
- Supports MFA, SSO, and social login
- Easily extensible

### Token Storage
- Mobile
  - Tokens stored using platform-secure storage:
  - iOS: Keychain / Secure Enclave
  - Android: Keystore
  - Accessed via Flutter secure storage plugins

- Web
  - In-memory storage preferred
  - Refresh via silent re-auth when possible
  - Avoid localStorage/sessionStorage for long-lived tokens

#### Pros
- Strong security model
- Centralized identity management
- Supports enterprise features (RBAC, MFA, policies)

#### Limitations
- Requires external IdP availability
- Initial setup complexity
- Token lifecycle must be carefully handled (expiration, refresh, revocation)


## 2. Biometric Authentication (Secondary / Convenience Layer)
Biometric authentication is used as a local re-authentication mechanism, not as a primary identity proof.

Supported methods depend on device capabilities:
- Fingerprint
- Face ID
- Device PIN / Pattern (fallback)

### How It Works
- User authenticates once via OAuth2
- Tokens are stored securely
- On subsequent app launches:
  - Biometry is used to unlock access to stored tokens
  - No new identity is asserted to backend

### Supported Platforms
- iOS: Face ID / Touch ID
- Android: Fingerprint / Face (device-dependent)
- Web: Not supported (browser limitations)

#### Pros
- Improved UX (fast login)
- Strong hardware-backed security
- No credentials exposed to UI

#### Limitations
- Not portable across devices
- Cannot be used as sole authentication
- Relies on OS-level trust and user device integrity
- Web support is limited or non-existent

## 3. Session & Token Handling
- Short-lived Access Tokens
- Optional Refresh Tokens (mobile only, securely stored)
- Backend validates:
  - Signature
  - Expiration
  - Audience / scopes

#### Frontend responsibilities:
- Attach access token to API calls
- Refresh tokens when expired
- Trigger re-authentication when refresh fails