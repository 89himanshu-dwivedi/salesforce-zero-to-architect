# Session 2 — Section 2.1: Two-Factor Authentication (2FA)

## 1. Learning Context

This first technical session introduces the basic idea behind Two-Factor Authentication (2FA): why a password alone is not enough and how a second verification factor strengthens identity confirmation.

## 2. Core Concept — “Your Other Password”

**Two-Factor Authentication (2FA)** asks for an additional credential or verification factor beyond the normal password. This creates an extra security layer. **MFA (Multi-Factor Authentication)** is the broader term; 2FA specifically uses two factors.

If a password is stolen, leaked, or phished, the password alone should not be sufficient to access an account—the attacker must also satisfy the second factor.

## 3. Three Authentication Factor Categories

| Category | Meaning | Examples |
|---|---|---|
| Something you KNOW | Something only you know | Password, secret questions (source examples: family-related question, childhood pet, favourite movie) |
| Something you HAVE | Something physically/device-possession based | Smartphone, hardware token, older key fob |
| Something you ARE | Biometric identity/pattern | Fingerprint, iris scan, face recognition |

**2FA principle:** verify identity using two different factor categories. Two passwords from the same “know” category do not, by themselves, constitute two-factor authentication.

## 4. 2FA Login Flow — Mental Model

**Visual Flow:**

`Username + Password` → `First Factor Verified` → `Second Factor Requested` → `User Verifies (Have/Are)` → `Second Factor Validated` → **Login Success**

If second-factor verification fails, is unavailable, or times out: **Login Blocked / Not Completed**.

1. The user enters a username/password (first factor: something they know).
2. The system requests a second factor/verification method (something they have/are, depending on the supported method).
3. If the second factor succeeds, identity is confirmed and login proceeds.
4. If the second factor is unavailable or incorrect, authentication fails or is blocked.

## 5. What If the Password Is Stolen?

| Situation | 2FA enabled | 2FA not enabled |
|---|---|---|
| Attacker has the password | Access can be blocked without the second factor; the user may receive an alert (alert behavior is service-specific). | The password may be enough to log in, increasing account-compromise risk. |

**Security lesson:** 2FA reduces account-takeover risk, but phishing-resistant methods and account-recovery protections also matter.

## 6. Real Story / Source Scenario

The source describes a company where the CEO and CFO accounts faced continuous hacking attempts for four weeks. Because 2FA was enabled, the attacker could not get in—the attacker did not have the second factor/phone.

## 7. Public Service Announcement

For services that handle your money—such as PayPal and banking services—enable 2FA/MFA when available. Even if a password is compromised, additional verification can help prevent unauthorized access.

The source lists services offering 2FA: Google, Facebook, Twitter/X, Microsoft, Dropbox, Evernote, LastPass, Okta, Ping Identity, Amazon, Heroku, and Slack. Availability and methods may vary by service and account.

## 8. Salesforce Current Status — 2026 (Officially Checked)

| Point | Current Salesforce guidance | Practical meaning |
|---|---|---|
| MFA requirement | Salesforce requires MFA for internal users accessing active production and sandbox org UIs, including direct login and SSO; enforcement is rolling out in 2026. | Plan MFA enrollment and verify your org’s enforcement schedule. |
| Privileged users | Users with privileged access (e.g., System Administrator profile or specified powerful permissions) must use phishing-resistant MFA. | Admins should prioritize passkeys/built-in authenticators or security keys. |
| Verification methods | Salesforce distinguishes phishing-resistant methods (such as passkeys/built-in authenticators and security keys) from standard MFA (such as Salesforce Authenticator or TOTP apps), depending on user requirements. | Do not assume every second factor has the same strength. |
| MFA vs. 2FA terminology | Salesforce documentation commonly uses **MFA**. 2FA is two-factor authentication; MFA is the broader term. | Use Salesforce’s MFA terminology when discussing Salesforce policy. |

# 🚀 Connect, Share & Follow for Complete Learning Materials

**Want to explore the complete material, detailed notes, practical examples, and additional learning resources?**

Let's connect and grow together! 🤝

I regularly share valuable learning materials, technical insights, practical knowledge, and resources designed to help you strengthen your skills and expand your understanding.

### 📚 Get Access to Complete Materials
- 📖 **Detailed Notes:** Access comprehensive notes covering important concepts and topics.
- 💡 **Practical Examples:** Learn through real-world scenarios, use cases, and hands-on examples.
- 🛠️ **Technical Resources:** Explore useful tools, guides, and additional learning resources.
- 🚀 **Continuous Learning:** Stay updated with new content, insights, and knowledge.

### 🤝 Let's Build a Learning Community

If you find this content useful, take a moment to:

- 🔗 **Connect with me** for access to complete learning materials and additional resources.
- 📤 **Share** this content with friends, colleagues, and anyone who might benefit from it.
- ❤️ **Follow me** to stay connected and receive updates on upcoming content and learning resources.

**Your support helps us share knowledge, learn together, and grow as a community.**

---

### 🌟 Learn More. Share More. Grow Together.

*Connect | Share | Follow | Keep Learning*

---