[Home](../index.md) / [13 · Security Deep Dive](index.md) / **Authentication**

# Authentication

8 topics · Series 13: Security Deep Dive

**Topics on this page**

- [Username Password](#username-password)
- [MFA](#mfa)
- [Login Flows](#login-flows)
- [SSO](#sso)
- [OAuth](#oauth)
- [OpenID Connect](#openid-connect)
- [SAML](#saml)
- [Certificate Based Auth](#certificate-based-auth)

## Username Password

*Basic credential authentication and its hardening.*

### 🌱 Simple

*Beginner - plain language*

**Username/password** is the most basic authentication: a user proves identity with a known username and a secret password. In Salesforce it's the default login, hardened by policies and (now mandatory) MFA.

### 📏 Limits

*Governor & platform limits*

- Blocked by MFA and being retired for OAuth use.
- Requires a security token unless the login IP is allow-listed.
- Password policies (expiry, complexity, lockout) are set per profile.
- Credentials must be stored somewhere - the core weakness.

> **Want the deeper material for this topic?**
> Advanced and architect-level depth, real-world scenarios, gotchas and interview questions
> are not published here. Connect with me - **LinkedIn · X · GitHub** - details on the
> [home page](../index.md).

## MFA

*Multi-factor authentication — a required second factor.*

### 🌱 Simple

*Beginner - plain language*

**MFA** (Multi-Factor Authentication) requires a second proof of identity beyond the password — something you *have* (authenticator app, security key) or *are* (biometric). Salesforce mandates it for all logins.

### 📏 Limits

*Governor & platform limits*

- Contractually required for Salesforce access.
- Blocks the Username-Password OAuth flow entirely.
- Integration users need JWT or Client Credentials to avoid interactive MFA.
- Exemptions must be justified and are auditable.

> **Want the deeper material for this topic?**
> Advanced and architect-level depth, real-world scenarios, gotchas and interview questions
> are not published here. Connect with me - **LinkedIn · X · GitHub** - details on the
> [home page](../index.md).

## Login Flows

*Custom screens/logic injected into the login process.*

### 🌱 Simple

*Beginner - plain language*

**Login Flows** let you insert custom screens or logic into the authentication process after credentials are verified but before the user lands in Salesforce — for extra verification, acceptance, or data capture.

### 📏 Limits

*Governor & platform limits*

- Run after authentication but before the user reaches the app.
- A broken login flow can lock out every user - always test with a spare admin.
- Assigned per profile; only one active flow per profile.
- Cannot be used to block API logins.

> **Want the deeper material for this topic?**
> Advanced and architect-level depth, real-world scenarios, gotchas and interview questions
> are not published here. Connect with me - **LinkedIn · X · GitHub** - details on the
> [home page](../index.md).

## SSO

*Single Sign-On — one identity across systems.*

### 🌱 Simple

*Beginner - plain language*

**SSO** (Single Sign-On) lets users authenticate once with a central identity provider and access Salesforce (and other apps) without separate logins — using SAML or OpenID Connect.

### 📏 Limits

*Governor & platform limits*

- Requires a verified My Domain.
- Certificate expiry breaks all logins simultaneously.
- Keep a non-SSO break-glass admin account.
- SSO does not cover API access - that needs OAuth.

> **Want the deeper material for this topic?**
> Advanced and architect-level depth, real-world scenarios, gotchas and interview questions
> are not published here. Connect with me - **LinkedIn · X · GitHub** - details on the
> [home page](../index.md).

## OAuth

*Delegated authorization framework for API access.*

### 🌱 Simple

*Beginner - plain language*

**OAuth 2.0** is the framework that lets an app obtain limited access to Salesforce on a user's (or system's) behalf via **tokens**, without sharing the password. It's authorization, not authentication.

### 📏 Limits

*Governor & platform limits*

- Access token lifetime equals the profile session timeout.
- Maximum 5 active access tokens per user per Connected App.
- Connected App changes take up to 10 minutes to propagate.
- Scopes cannot exceed the user's underlying permissions.

> **Want the deeper material for this topic?**
> Advanced and architect-level depth, real-world scenarios, gotchas and interview questions
> are not published here. Connect with me - **LinkedIn · X · GitHub** - details on the
> [home page](../index.md).

## OpenID Connect

*Identity layer on top of OAuth 2.0.*

### 🌱 Simple

*Beginner - plain language*

**OpenID Connect (OIDC)** adds an **authentication** layer to OAuth 2.0 — issuing an **ID token** (a signed JWT) that proves *who* the user is, not just what an app can access.

### 📏 Limits

*Governor & platform limits*

- Requires the `openid` scope.
- ID token validation is the consumer's responsibility.
- Clock skew causes intermittent validation failures.
- Not all providers issue refresh tokens by default.

> **Want the deeper material for this topic?**
> Advanced and architect-level depth, real-world scenarios, gotchas and interview questions
> are not published here. Connect with me - **LinkedIn · X · GitHub** - details on the
> [home page](../index.md).

## SAML

*XML-based federation standard for enterprise SSO.*

### 🌱 Simple

*Beginner - plain language*

**SAML** (Security Assertion Markup Language) is an XML-based standard for SSO: an IdP issues a signed **assertion** vouching for the user's identity, which the Service Provider (Salesforce) trusts.

### 📏 Limits

*Governor & platform limits*

- Assertions are time-bounded; clock skew breaks them.
- Signing certificates expire and must be rotated ahead of time.
- Browser-based only - cannot authenticate server-to-server API calls.
- JIT provisioning consumes a licence per created user.

> **Want the deeper material for this topic?**
> Advanced and architect-level depth, real-world scenarios, gotchas and interview questions
> are not published here. Connect with me - **LinkedIn · X · GitHub** - details on the
> [home page](../index.md).

## Certificate Based Auth

*Authenticating with X.509 certificates instead of secrets.*

### 🌱 Simple

*Beginner - plain language*

**Certificate-based authentication** proves identity with an X.509 certificate (and its private key) instead of a password or shared secret — used for mTLS and signing JWTs for server-to-server auth.

### 📏 Limits

*Governor & platform limits*

- Certificates have a maximum validity and cannot be exported after creation.
- Expiry causes total, silent integration failure.
- mTLS is not supported by every authentication protocol.
- Rotation requires coordination with the counterparty.

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
