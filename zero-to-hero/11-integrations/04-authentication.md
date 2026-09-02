[Home](../index.md) / [11 · Integrations](index.md) / **Authentication**

# Authentication

12 topics · Series 11: Integrations

**Topics on this page**

- [OAuth Authorization Code Flow](#oauth-authorization-code-flow)
- [JWT Bearer Flow](#jwt-bearer-flow)
- [Client Credentials Flow](#client-credentials-flow)
- [Username Password Flow](#username-password-flow)
- [Web Server Flow](#web-server-flow)
- [Refresh Token Flow](#refresh-token-flow)
- [OpenID Connect](#openid-connect)
- [SAML](#saml)
- [SSO](#sso)
- [Identity Provider](#identity-provider)
- [Service Provider](#service-provider)
- [MFA](#mfa)

## OAuth Authorization Code Flow

*User-authorized OAuth flow with PKCE for apps.*

### 🌱 Simple

*Beginner - plain language*

The **Authorization Code flow** authenticates a user interactively: the app redirects to Salesforce login, the user consents, Salesforce returns an authorization *code*, and the app exchanges it for tokens.

### 📏 Limits

*Governor & platform limits*

- Access token lifetime equals the profile session timeout (default 2 hours).
- Maximum 5 active access tokens per user per Connected App.
- Callback URL must match exactly - protocol, host, port and path.
- Connected App changes take up to 10 minutes to propagate.

> **Want the deeper material for this topic?**
> Advanced and architect-level depth, real-world scenarios, gotchas and interview questions
> are not published here. Connect with me - **LinkedIn · X · GitHub** - details on the
> [home page](../index.md).

## JWT Bearer Flow

*Server-to-server OAuth using a signed JWT (no user login).*

### 🌱 Simple

*Beginner - plain language*

The **JWT Bearer flow** authenticates server-to-server without interactive login: the app signs a JWT with a certificate's private key and exchanges it for an access token.

### 📏 Limits

*Governor & platform limits*

- Requires a certificate uploaded to the Connected App and the user pre-authorised.
- Certificate expiry is a hard cutoff - track it as a calendar item.
- No refresh token is issued, so there is nothing to expire from inactivity.
- The user must be pre-authorised via profile or permission set.

> **Want the deeper material for this topic?**
> Advanced and architect-level depth, real-world scenarios, gotchas and interview questions
> are not published here. Connect with me - **LinkedIn · X · GitHub** - details on the
> [home page](../index.md).

## Client Credentials Flow

*Server-to-server OAuth tied to a single run-as user.*

### 🌱 Simple

*Beginner - plain language*

The **Client Credentials flow** authenticates server-to-server using just the Connected App's client id and secret — no user login — running as a configured integration user.

### 📏 Limits

*Governor & platform limits*

- Requires a "Run As" user configured on the Connected App.
- All access runs as that single user - no per-user identity.
- Access token lifetime follows that user's session timeout.
- Not suitable where the external system enforces per-user permissions.

> **Want the deeper material for this topic?**
> Advanced and architect-level depth, real-world scenarios, gotchas and interview questions
> are not published here. Connect with me - **LinkedIn · X · GitHub** - details on the
> [home page](../index.md).

## Username Password Flow

*Legacy OAuth flow exchanging credentials directly (avoid).*

### 🌱 Simple

*Beginner - plain language*

The **Username-Password flow** sends a username and password (plus security token) directly to get an access token. It's legacy, insecure, and being **deprecated** — avoid it.

### 📏 Limits

*Governor & platform limits*

- Blocked by MFA and being retired - treat any use as a finding.
- Requires a security token unless the IP is allow-listed.
- Password rotation silently breaks the integration.
- Credentials must be stored somewhere, which is the core weakness.

> **Want the deeper material for this topic?**
> Advanced and architect-level depth, real-world scenarios, gotchas and interview questions
> are not published here. Connect with me - **LinkedIn · X · GitHub** - details on the
> [home page](../index.md).

## Web Server Flow

*Authorization Code flow for confidential server-side apps.*

### 🌱 Simple

*Beginner - plain language*

The **Web Server flow** is the Authorization Code flow used by a confidential server-side app that can securely store a client secret — exchanging the code for tokens on the server.

### 📏 Limits

*Governor & platform limits*

- Requires a confidential client capable of holding a client secret.
- Exact callback URL match is enforced.
- Refresh token policy on the Connected App controls longevity.
- Not appropriate for SPAs or mobile - use Authorization Code with PKCE.

> **Want the deeper material for this topic?**
> Advanced and architect-level depth, real-world scenarios, gotchas and interview questions
> are not published here. Connect with me - **LinkedIn · X · GitHub** - details on the
> [home page](../index.md).

## Refresh Token Flow

*Obtaining new access tokens without re-login.*

### 🌱 Simple

*Beginner - plain language*

The **Refresh Token flow** exchanges a long-lived refresh token for a new access token when the current one expires — keeping the user logged in without re-authenticating.

### 📏 Limits

*Governor & platform limits*

- Policy options include immediate expiry and expiry after N days of inactivity.
- Low-volume integrations commonly fall below the inactivity threshold and fail.
- Revoking the token or blocking the app invalidates it immediately.
- Maximum 5 active access tokens per user per Connected App.

> **Want the deeper material for this topic?**
> Advanced and architect-level depth, real-world scenarios, gotchas and interview questions
> are not published here. Connect with me - **LinkedIn · X · GitHub** - details on the
> [home page](../index.md).

## OpenID Connect

*Identity layer on top of OAuth 2.0 for authentication.*

### 🌱 Simple

*Beginner - plain language*

**OpenID Connect (OIDC)** adds an authentication/identity layer on top of OAuth 2.0 — returning an **ID token** (JWT) that proves who the user is, not just authorization.

### 📏 Limits

*Governor & platform limits*

- Requires the `openid` scope on the Connected App.
- ID token validation (signature, issuer, audience, expiry) is the consumer's responsibility.
- Token lifetimes follow the session timeout settings.
- Not all identity providers issue refresh tokens by default.

> **Want the deeper material for this topic?**
> Advanced and architect-level depth, real-world scenarios, gotchas and interview questions
> are not published here. Connect with me - **LinkedIn · X · GitHub** - details on the
> [home page](../index.md).

## SAML

*XML-based assertion standard for enterprise SSO.*

### 🌱 Simple

*Beginner - plain language*

**SAML** (Security Assertion Markup Language) is an XML-based standard for SSO: an identity provider issues a signed **assertion** vouching for the user, which the service provider trusts.

### 📏 Limits

*Governor & platform limits*

- Assertions are time-bounded - clock skew between IdP and Salesforce causes failures.
- Signing certificates expire and must be rotated before the deadline.
- SAML is browser-based; it cannot be used for server-to-server API auth.
- Just-in-time provisioning consumes a licence per created user.

> **Want the deeper material for this topic?**
> Advanced and architect-level depth, real-world scenarios, gotchas and interview questions
> are not published here. Connect with me - **LinkedIn · X · GitHub** - details on the
> [home page](../index.md).

## SSO

*Single sign-on across multiple applications.*

### 🌱 Simple

*Beginner - plain language*

**Single Sign-On (SSO)** lets users authenticate once and access multiple applications without logging in again — via a shared identity provider using SAML or OIDC.

### 📏 Limits

*Governor & platform limits*

- Requires a verified My Domain.
- Certificate expiry breaks all logins simultaneously - a Sev1 waiting to happen.
- Keep at least one non-SSO admin account for break-glass access.
- SSO does not cover API access - that needs OAuth.

> **Want the deeper material for this topic?**
> Advanced and architect-level depth, real-world scenarios, gotchas and interview questions
> are not published here. Connect with me - **LinkedIn · X · GitHub** - details on the
> [home page](../index.md).

## Identity Provider

*The system that authenticates users and asserts identity.*

### 🌱 Simple

*Beginner - plain language*

An **Identity Provider (IdP)** authenticates users and issues assertions/tokens vouching for their identity to other applications (service providers). Salesforce can act as an IdP.

### 📏 Limits

*Governor & platform limits*

- Salesforce as IdP requires My Domain and a signing certificate.
- Certificates have a maximum validity and must be rotated.
- Connected App changes propagate in up to 10 minutes.
- Each service provider needs its own Connected App for independent revocation.

> **Want the deeper material for this topic?**
> Advanced and architect-level depth, real-world scenarios, gotchas and interview questions
> are not published here. Connect with me - **LinkedIn · X · GitHub** - details on the
> [home page](../index.md).

## Service Provider

*The application that consumes identity from an IdP.*

### 🌱 Simple

*Beginner - plain language*

A **Service Provider (SP)** is the application a user wants to access — it relies on an Identity Provider to authenticate the user and trusts the IdP's assertion/token. Salesforce often acts as the SP.

### 📏 Limits

*Governor & platform limits*

- Requires an Auth Provider or SAML SSO configuration in Salesforce.
- Callback URL is generated by Salesforce and cannot be freely chosen.
- JIT-provisioned users consume licences.
- Registration Handler runs in system context - all validation is your responsibility.

> **Want the deeper material for this topic?**
> Advanced and architect-level depth, real-world scenarios, gotchas and interview questions
> are not published here. Connect with me - **LinkedIn · X · GitHub** - details on the
> [home page](../index.md).

## MFA

*Multi-factor authentication strengthening login security.*

### 🌱 Simple

*Beginner - plain language*

**Multi-factor authentication (MFA)** requires a second factor (authenticator app, security key) beyond a password — Salesforce **requires** MFA for direct logins.

### 📏 Limits

*Governor & platform limits*

- MFA is contractually required for Salesforce access.
- It blocks the Username-Password OAuth flow entirely.
- API integrations must use JWT or Client Credentials to avoid interactive MFA.
- Exemptions exist for integration users but must be justified and audited.

> **Want the deeper material for this topic?**
> Advanced and architect-level depth, real-world scenarios, gotchas and interview questions
> are not published here. Connect with me - **LinkedIn · X · GitHub** - details on the
> [home page](../index.md).

---

## Connect

These pages carry the **definitions and limits** only. The advanced depth, real-world
scenarios, error playbooks, best-option reasoning and interview questions are kept aside.

If you would like them, or you want to talk about the topics on this page, connect with me
on **LinkedIn**, **X (Twitter)** or **GitHub** - all links are on the
[home page](../index.md).

*- Himanshu Kumar*
