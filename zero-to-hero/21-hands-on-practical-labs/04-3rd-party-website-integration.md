[Home](../index.md) / [21 · Hands-On Practical Labs](index.md) / **3rd-Party Website Integration**

# 3rd-Party Website Integration

6 topics · Series 21: Hands-On Practical Labs

**Topics on this page**

- [LWC Call Public API (No Auth)](#lwc-call-public-api-no-auth)
- [LWC Call API With Named Credential](#lwc-call-api-with-named-credential)
- [Embed 3rd-Party Widget](#embed-3rd-party-widget)
- [Load 3rd-Party JS Library](#load-3rd-party-js-library)
- [Salesforce to External REST](#salesforce-to-external-rest)
- [External Site Reads SF Data](#external-site-reads-sf-data)

## LWC Call Public API (No Auth)

*Browser-side fetch to a public 3rd-party API — 'without connection'.*

### 🌱 Simple

*Beginner - plain language*

"**Without connection**" means the LWC talks **directly from the browser** to a public 3rd-party website API (no login/secret, no Salesforce server in the middle). Good for *public, free, no-key* data — weather, currency, public info. You just need a **CSP Trusted Site** so the browser is allowed.

### 📏 Limits

*Governor & platform limits*

- CORS must be allowed by the third party; Salesforce cannot override it.
- CSP Trusted Site required with the correct directive (`connect-src`).
- No Apex governor limits apply - but also no Platform Cache, no retry, no logging.
- Blocked entirely in some Experience Cloud security levels.

> **Want the deeper material for this topic?**
> Advanced depth, real-world scenarios, gotchas and interview questions are not published here.
> Connect: [LinkedIn](https://www.linkedin.com/in/himanshukumar-sf/) · [X](https://x.com/kum60094) · [GitHub](https://github.com/89himanshu-dwivedi) · [Email](mailto:himanshu.jee.1996@gmail.com)

## LWC Call API With Named Credential

*Authenticated 3rd-party call routed through Apex — 'with connection'.*

### 🌱 Simple

*Beginner - plain language*

"**With connection**" means the call to the 3rd-party website needs **authentication** (an API key, OAuth token). You must **never** expose that in the browser, so the LWC calls **Apex**, and Apex calls the 3rd-party site via a **Named Credential** (which holds the secret). This also bypasses CORS.

### 📏 Limits

*Governor & platform limits*

- **100 callouts** and **120s** total callout time per transaction.
- Callout timeout max 120s; set it explicitly - the 10s default is often too short.
- Request/response body max **6 MB** (12 MB async).
- Platform Cache is capped by your org's purchased cache size and entries can be evicted early.
- No callouts after uncommitted DML in the same transaction.

> **Want the deeper material for this topic?**
> Advanced depth, real-world scenarios, gotchas and interview questions are not published here.
> Connect: [LinkedIn](https://www.linkedin.com/in/himanshukumar-sf/) · [X](https://x.com/kum60094) · [GitHub](https://github.com/89himanshu-dwivedi) · [Email](mailto:himanshu.jee.1996@gmail.com)

## Embed 3rd-Party Widget

*Show an external website/widget inside Salesforce (iframe).*

### 🌱 Simple

*Beginner - plain language*

Sometimes you don't call an API — you want to **display an entire 3rd-party web page or widget** inside Salesforce: a chatbot, a payment page, a dashboard, a map. The way to embed another website's UI is an **iframe**, allowed via **CSP Trusted Sites (`frame-src`)**.

### 📏 Limits

*Governor & platform limits*

- Requires CSP Trusted Site with `frame-src`; Experience Cloud may block it at stricter security levels.
- Vendors that send `X-Frame-Options: DENY` cannot be iframed at all.
- Iframe content is outside Lightning Web Security - it is a separate origin with its own rules.
- Mobile app rendering of iframes is inconsistent; test on device.

> **Want the deeper material for this topic?**
> Advanced depth, real-world scenarios, gotchas and interview questions are not published here.
> Connect: [LinkedIn](https://www.linkedin.com/in/himanshukumar-sf/) · [X](https://x.com/kum60094) · [GitHub](https://github.com/89himanshu-dwivedi) · [Email](mailto:himanshu.jee.1996@gmail.com)

## Load 3rd-Party JS Library

*Use an external JS/CSS library (charts, PDF, maps) inside LWC.*

### 🌱 Simple

*Beginner - plain language*

To use a 3rd-party JavaScript library (Chart.js, D3, jsPDF, a maps SDK) inside an LWC, you **upload it as a Static Resource** and load it with **`loadScript`/`loadStyle`** from `lightning/platformResourceLoader`. You don't `<script>`-tag external CDNs directly.

### 📏 Limits

*Governor & platform limits*

- Static resource max **5 MB** each; **250 MB** total per org.
- Some libraries are incompatible with LWS/Locker - test, do not assume.
- `loadScript` resolves once per component instance; caching is handled by the browser.
- No inline scripts and no `eval` anywhere in Lightning.

> **Want the deeper material for this topic?**
> Advanced depth, real-world scenarios, gotchas and interview questions are not published here.
> Connect: [LinkedIn](https://www.linkedin.com/in/himanshukumar-sf/) · [X](https://x.com/kum60094) · [GitHub](https://github.com/89himanshu-dwivedi) · [Email](mailto:himanshu.jee.1996@gmail.com)

## Salesforce to External REST

*Outbound: Salesforce pushes data INTO another system.*

### 🌱 Simple

*Beginner - plain language*

This is the **outbound** direction: an event happens in Salesforce (record created/updated) and Salesforce **sends data to an external system's REST API** — e.g., create an invoice in an ERP when an Opportunity closes. Apex makes an HTTP callout (usually async).

### 📏 Limits

*Governor & platform limits*

- **100 callouts** per transaction; **120s** cumulative callout time.
- Single callout timeout max **120s** (default 10s).
- Body limit **6 MB** sync / **12 MB** async.
- No callouts after uncommitted DML.
- Concurrent long-running requests (>5s) capped at **10** per org - slow callouts can lock out users.

> **Want the deeper material for this topic?**
> Advanced depth, real-world scenarios, gotchas and interview questions are not published here.
> Connect: [LinkedIn](https://www.linkedin.com/in/himanshukumar-sf/) · [X](https://x.com/kum60094) · [GitHub](https://github.com/89himanshu-dwivedi) · [Email](mailto:himanshu.jee.1996@gmail.com)

## External Site Reads SF Data

*Inbound: a 3rd-party website pulls/pushes Salesforce data.*

### 🌱 Simple

*Beginner - plain language*

This is the **inbound** direction: an external website/app wants to **read or write Salesforce data**. It must **authenticate** (OAuth via a Connected App → bearer token) and then call Salesforce's **REST API** (standard objects) or your **custom Apex REST endpoint**.

### 📏 Limits

*Governor & platform limits*

- Daily API request limit is org-wide (edition + licence based) and shared by every integration.
- Query API returns 2,000 records per page by default; follow `nextRecordsUrl`.
- Composite API: max **25** subrequests; Composite Graph: 500 records total.
- Concurrent long-running (>20s) API requests capped at **25** per org.
- Access token lifetime tied to the profile's session timeout.

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
