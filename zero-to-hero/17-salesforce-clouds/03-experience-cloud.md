[Home](../index.md) / [17 · Salesforce Clouds](index.md) / **Experience Cloud**

# Experience Cloud

7 topics · Series 17: Salesforce Clouds

**Topics on this page**

- [Sites](#sites)
- [Communities](#communities)
- [Templates](#templates)
- [Guest User Security](#guest-user-security)
- [Sharing Sets](#sharing-sets)
- [Audience Targeting](#audience-targeting)
- [CMS](#cms)

## Sites

*Public/authenticated web experiences on the platform.*

### 🌱 Simple

*Beginner - plain language*

**Experience Cloud Sites** are branded web experiences (portals, help centers, partner sites) built on Salesforce data — for customers, partners, or the public — using LWR/Aura templates or custom code.

### 📏 Limits

*Governor & platform limits*

- LWR (fast) vs Aura (richer OOTB) vs custom; per audience/license.
- Custom domain/SEO/CDN; least-privilege security mandatory.

> **Want the deeper material for this topic?**
> Advanced depth, real-world scenarios, gotchas and interview questions are not published here.
> Connect: [LinkedIn](https://www.linkedin.com/in/himanshukumar-sf/) · [X](https://x.com/kum60094) · [GitHub](https://github.com/89himanshu-dwivedi) · [Email](mailto:himanshu.jee.1996@gmail.com)

## Communities

*Collaborative experiences for customers and partners.*

### 🌱 Simple

*Beginner - plain language*

**Communities** (now Experience Cloud sites) are online spaces where customers, partners, and employees **collaborate** — ask questions, access records, get support, and engage with your brand and each other.

### 📏 Limits

*Governor & platform limits*

- Templates (Customer Service/Partner Central); license tiers.
- Sharing sets/rules + FLS; high-volume users outside role hierarchy.

> **Want the deeper material for this topic?**
> Advanced depth, real-world scenarios, gotchas and interview questions are not published here.
> Connect: [LinkedIn](https://www.linkedin.com/in/himanshukumar-sf/) · [X](https://x.com/kum60094) · [GitHub](https://github.com/89himanshu-dwivedi) · [Email](mailto:himanshu.jee.1996@gmail.com)

## Templates

*Pre-built experience frameworks in Experience Builder.*

### 🌱 Simple

*Beginner - plain language*

**Templates** are pre-built starting points in **Experience Builder** — like Customer Service, Partner Central, Help Center, and Build Your Own (LWR) — providing pages, components, and theming to launch a site fast.

### 📏 Limits

*Governor & platform limits*

- Aura (rich OOTB, heavier) vs LWR (fast, fewer components).
- Theming/audiences; validate components; migration = rebuild.

> **Want the deeper material for this topic?**
> Advanced depth, real-world scenarios, gotchas and interview questions are not published here.
> Connect: [LinkedIn](https://www.linkedin.com/in/himanshukumar-sf/) · [X](https://x.com/kum60094) · [GitHub](https://github.com/89himanshu-dwivedi) · [Email](mailto:himanshu.jee.1996@gmail.com)

## Guest User Security

*Lock down the unauthenticated public profile.*

### 🌱 Simple

*Beginner - plain language*

**Guest User Security** governs what **unauthenticated visitors** can access on a public site. The guest user runs as a special profile and must be tightly restricted to avoid exposing data.

### 📏 Limits

*Governor & platform limits*

- No role; read-only OWD; explicit guest sharing rules.
- Minimal profile + FLS; with-sharing Apex; abuse controls.

> **Want the deeper material for this topic?**
> Advanced depth, real-world scenarios, gotchas and interview questions are not published here.
> Connect: [LinkedIn](https://www.linkedin.com/in/himanshukumar-sf/) · [X](https://x.com/kum60094) · [GitHub](https://github.com/89himanshu-dwivedi) · [Email](mailto:himanshu.jee.1996@gmail.com)

## Sharing Sets

*Grant external users access via account/contact match.*

### 🌱 Simple

*Beginner - plain language*

**Sharing Sets** grant high-volume community users access to records based on their **account or contact relationship** — e.g., let a customer see cases where they are the Contact — without using the role hierarchy.

### 📏 Limits

*Governor & platform limits*

- For high-volume users (outside role hierarchy); account/contact match.
- Read or R/W; share groups for internal; LDV-friendly.

> **Want the deeper material for this topic?**
> Advanced depth, real-world scenarios, gotchas and interview questions are not published here.
> Connect: [LinkedIn](https://www.linkedin.com/in/himanshukumar-sf/) · [X](https://x.com/kum60094) · [GitHub](https://github.com/89himanshu-dwivedi) · [Email](mailto:himanshu.jee.1996@gmail.com)

## Audience Targeting

*Personalize pages/components by audience criteria.*

### 🌱 Simple

*Beginner - plain language*

**Audience Targeting** personalizes an Experience Cloud site by showing different **pages, components, or branding** to different visitors based on criteria like profile, location, record fields, or user attributes.

### 📏 Limits

*Governor & platform limits*

- Criteria (profile/location/record/custom); page/component/branding targets.
- Personalization NOT security; enforce sharing/FLS separately.

> **Want the deeper material for this topic?**
> Advanced depth, real-world scenarios, gotchas and interview questions are not published here.
> Connect: [LinkedIn](https://www.linkedin.com/in/himanshukumar-sf/) · [X](https://x.com/kum60094) · [GitHub](https://github.com/89himanshu-dwivedi) · [Email](mailto:himanshu.jee.1996@gmail.com)

## CMS

*Manage and deliver content across channels.*

### 🌱 Simple

*Beginner - plain language*

**Salesforce CMS** is a hybrid content management system to **create, manage, and deliver content** (images, news, banners, documents) to Experience Cloud sites and other channels — reusable across experiences.

### 📏 Limits

*Governor & platform limits*

- Workspaces → channels; standard + custom content types.
- Coupled (Builder) + headless (Delivery/GraphQL API); collections/targeting.

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
