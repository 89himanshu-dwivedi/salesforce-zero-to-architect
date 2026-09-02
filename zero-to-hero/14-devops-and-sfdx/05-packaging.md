[Home](../index.md) / [14 · DevOps & SFDX](index.md) / **Packaging**

# Packaging

4 topics · Series 14: DevOps & SFDX

**Topics on this page**

- [Unlocked Packages](#unlocked-packages)
- [Managed Packages](#managed-packages)
- [2nd Gen Packaging](#2nd-gen-packaging)
- [Package Dependencies](#package-dependencies)

## Unlocked Packages

*Source-controlled, versioned packages for internal orgs.*

### 🌱 Simple

*Beginner - plain language*

**Unlocked packages** are versioned, source-driven packages for organizing your own org's metadata into modular, deployable units — ideal for internal/enterprise development.

### 📏 Limits

*Governor & platform limits*

- Removing a component from a released package requires a new major version.
- Version creation is asynchronous and rate-limited.
- Ancestry must be declared for upgradable versions.
- Not all metadata types are packageable.

> **Want the deeper material for this topic?**
> Advanced depth, real-world scenarios, gotchas and interview questions are not published here.
> Connect: [LinkedIn](https://www.linkedin.com/in/himanshukumar-sf/) · [X](https://x.com/kum60094) · [GitHub](https://github.com/89himanshu-dwivedi) · [Email](mailto:himanshu.jee.1996@gmail.com)

## Managed Packages

*Locked, IP-protected packages for AppExchange ISVs.*

### 🌱 Simple

*Beginner - plain language*

**Managed packages** are versioned packages with **locked, IP-protected** code — used by ISVs to distribute and sell apps on AppExchange, with upgradeable versions.

### 📏 Limits

*Governor & platform limits*

- `global` members can never be removed - a permanent API commitment.
- Security review is required and takes weeks.
- Namespace is permanent and cannot be changed.
- Subscriber orgs cannot modify packaged code.

> **Want the deeper material for this topic?**
> Advanced depth, real-world scenarios, gotchas and interview questions are not published here.
> Connect: [LinkedIn](https://www.linkedin.com/in/himanshukumar-sf/) · [X](https://x.com/kum60094) · [GitHub](https://github.com/89himanshu-dwivedi) · [Email](mailto:himanshu.jee.1996@gmail.com)

## 2nd Gen Packaging

*Modern source-driven packaging (2GP).*

### 🌱 Simple

*Beginner - plain language*

**Second-Generation Packaging (2GP)** is the modern, **source-driven** way to build packages (unlocked and managed) directly from a Git repo via the CLI — without relying on a legacy packaging org.

### 📏 Limits

*Governor & platform limits*

- Requires a Dev Hub and a namespace for managed 2GP.
- Version numbers and ancestry rules are strict and cannot be rewritten.
- Package creation jobs are asynchronous and can fail on metadata not supported for packaging.

> **Want the deeper material for this topic?**
> Advanced depth, real-world scenarios, gotchas and interview questions are not published here.
> Connect: [LinkedIn](https://www.linkedin.com/in/himanshukumar-sf/) · [X](https://x.com/kum60094) · [GitHub](https://github.com/89himanshu-dwivedi) · [Email](mailto:himanshu.jee.1996@gmail.com)

## Package Dependencies

*Declaring and resolving inter-package requirements.*

### 🌱 Simple

*Beginner - plain language*

**Package dependencies** declare that one package requires another (or a specific version) — so installs happen in the right order with compatible versions.

### 📏 Limits

*Governor & platform limits*

- Dependencies must be installed in order; circular dependencies are not supported.
- Version ranges are not supported - you pin an exact version.
- Upgrading a dependency may force upgrading dependents.

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
