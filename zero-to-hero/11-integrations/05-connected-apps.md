[Home](../index.md) / [11 · Integrations](index.md) / **Connected Apps**

# Connected Apps

4 topics · Series 11: Integrations

**Topics on this page**

- [Policies](#policies)
- [Scopes](#scopes)
- [OAuth Settings](#oauth-settings)
- [Certificates](#certificates)

## Policies

*Controlling who can use a Connected App and how.*

### 🌱 Simple

*Beginner - plain language*

Connected App **policies** govern access: which users are permitted, session/IP restrictions, refresh-token expiry, and whether admins must pre-approve users.

### 📏 Limits

*Governor & platform limits*

- "Admin approved users are pre-authorised" requires explicit profile or permission set assignment.
- IP relaxation interacts with profile login IP ranges - both must allow the caller.
- Refresh token policy options include immediate and inactivity-based expiry.
- Policy changes take up to 10 minutes to propagate.

> **Want the deeper material for this topic?**
> Advanced depth, real-world scenarios, gotchas and interview questions are not published here.
> Connect: [LinkedIn](https://www.linkedin.com/in/himanshukumar-sf/) · [X](https://x.com/kum60094) · [GitHub](https://github.com/89himanshu-dwivedi) · [Email](mailto:himanshu.jee.1996@gmail.com)

## Scopes

*OAuth scopes limiting what an app's token can do.*

### 🌱 Simple

*Beginner - plain language*

Connected App **OAuth scopes** define what an access token is allowed to do — e.g., `api` (data access), `refresh_token`, `openid`, `web`, `full`. Grant only what's needed.

### 📏 Limits

*Governor & platform limits*

- Scopes cannot exceed what the running user's profile already permits.
- `refresh_token` requires `offline_access` to be requested.
- Adding a scope requires re-authorisation by existing users.
- Overly broad scopes are a standard security review finding.

> **Want the deeper material for this topic?**
> Advanced depth, real-world scenarios, gotchas and interview questions are not published here.
> Connect: [LinkedIn](https://www.linkedin.com/in/himanshukumar-sf/) · [X](https://x.com/kum60094) · [GitHub](https://github.com/89himanshu-dwivedi) · [Email](mailto:himanshu.jee.1996@gmail.com)

## OAuth Settings

*Configuring OAuth on the Connected App (callback, secret, flows).*

### 🌱 Simple

*Beginner - plain language*

Connected App **OAuth settings** enable OAuth and define the callback (redirect) URL, consumer key/secret, allowed scopes, and which flows are permitted.

### 📏 Limits

*Governor & platform limits*

- Consumer secret is visible only to users with the right permission - treat it as a credential.
- Callback URL matching is exact.
- Maximum 5 active access tokens per user per Connected App.
- Changes take up to 10 minutes to take effect.

> **Want the deeper material for this topic?**
> Advanced depth, real-world scenarios, gotchas and interview questions are not published here.
> Connect: [LinkedIn](https://www.linkedin.com/in/himanshukumar-sf/) · [X](https://x.com/kum60094) · [GitHub](https://github.com/89himanshu-dwivedi) · [Email](mailto:himanshu.jee.1996@gmail.com)

## Certificates

*Using certificates for app authentication and signing.*

### 🌱 Simple

*Beginner - plain language*

Connected Apps use **certificates** for stronger auth — e.g., the public certificate uploaded for JWT Bearer flow, so Salesforce verifies JWTs signed with the matching private key.

### 📏 Limits

*Governor & platform limits*

- Self-signed certificates have a maximum validity (typically 1-2 years) and must be rotated.
- Expiry breaks JWT flows, SSO and mTLS simultaneously and without warning.
- Maximum key sizes and algorithms are constrained by the platform.
- Certificates cannot be exported once created - plan backups of the CSR process, not the key.

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
