[Home](../index.md) / [20 · Real-Time Scenarios (CTA)](index.md) / **Identity Scenarios**

# Identity Scenarios

4 topics · Series 20: Real-Time Scenarios (CTA)

**Topics on this page**

- [SSO](#sso)
- [Multiple Identity Providers](#multiple-identity-providers)
- [B2B Authentication](#b2b-authentication)
- [B2C Authentication](#b2c-authentication)

## SSO

*Single Sign-On architecture across the enterprise.*

### 🌱 Simple

*Beginner - plain language*

**SSO (Single Sign-On)** lets users authenticate once with a central identity provider and access Salesforce (and other apps) without logging in again — improving security and user experience.

### 📏 Limits

*Governor & platform limits*

- SAML/OIDC; Salesforce as SP to corporate IdP; My Domain prerequisite.
- JIT provisioning; Federation ID; enforce MFA/session; break-glass admin.

> **Want the deeper material for this topic?**
> Advanced depth, real-world scenarios, gotchas and interview questions are not published here.
> Connect: [LinkedIn](https://www.linkedin.com/in/himanshukumar-sf/) · [X](https://x.com/kum60094) · [GitHub](https://github.com/89himanshu-dwivedi) · [Email](mailto:himanshu.jee.1996@gmail.com)

## Multiple Identity Providers

*Support several IdPs for different user populations.*

### 🌱 Simple

*Beginner - plain language*

**Multiple identity providers** means supporting **several IdPs** in one org — e.g., employees via Azure AD, partners via their own IdPs, customers via social login — routing each to the right authentication source.

### 📏 Limits

*Governor & platform limits*

- Multiple SAML/OIDC configs + Auth Providers; login discovery/flows routing.
- Registration handlers/Federation ID linking; per-audience isolation + governance.

> **Want the deeper material for this topic?**
> Advanced depth, real-world scenarios, gotchas and interview questions are not published here.
> Connect: [LinkedIn](https://www.linkedin.com/in/himanshukumar-sf/) · [X](https://x.com/kum60094) · [GitHub](https://github.com/89himanshu-dwivedi) · [Email](mailto:himanshu.jee.1996@gmail.com)

## B2B Authentication

*Authenticate business partners and their employees.*

### 🌱 Simple

*Beginner - plain language*

**B2B authentication** secures access for **business partners and their employees** — typically via SSO to the partner's own IdP, partner community licenses, and account-based identity and provisioning.

### 📏 Limits

*Governor & platform limits*

- Federation to each partner IdP (SAML/OIDC); account-based external users.
- JIT/SCIM + delegated admin; partner licenses/sharing sets; lifecycle deprovision.

> **Want the deeper material for this topic?**
> Advanced depth, real-world scenarios, gotchas and interview questions are not published here.
> Connect: [LinkedIn](https://www.linkedin.com/in/himanshukumar-sf/) · [X](https://x.com/kum60094) · [GitHub](https://github.com/89himanshu-dwivedi) · [Email](mailto:himanshu.jee.1996@gmail.com)

## B2C Authentication

*Authenticate large volumes of consumers securely.*

### 🌱 Simple

*Beginner - plain language*

**B2C authentication** secures access for **large volumes of individual consumers** — via self-registration, social login, passwordless options, and scalable identity, often using Salesforce as a customer identity platform.

### 📏 Limits

*Governor & platform limits*

- External Identity at scale; self-registration; social + passwordless login.
- Account linking/dedupe; risk-based MFA + bot protection; consent/privacy.

> **Want the deeper material for this topic?**
> Advanced depth, real-world scenarios, gotchas and interview questions are not published here.
> Connect: [LinkedIn](https://www.linkedin.com/in/himanshukumar-sf/) · [X](https://x.com/kum60094) · [GitHub](https://github.com/89himanshu-dwivedi) · [Email](mailto:himanshu.jee.1996@gmail.com)

---

## Connect

These pages carry the **definitions and limits** only. The advanced depth, real-world
scenarios, error playbooks, best-option reasoning and interview questions are kept aside.

If you would like them, or you want to talk about anything on this page:

- **LinkedIn** - [in/himanshukumar-sf](https://www.linkedin.com/in/himanshukumar-sf/)
- **X** - [@kum60094](https://x.com/kum60094)
- **GitHub** - [89himanshu-dwivedi](https://github.com/89himanshu-dwivedi)
- **Email** - [himanshu.jee.1996@gmail.com](mailto:himanshu.jee.1996@gmail.com)

*- Himanshu Kumar*
