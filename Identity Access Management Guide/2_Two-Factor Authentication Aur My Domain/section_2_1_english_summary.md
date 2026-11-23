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

## 9. Additional Knowledge (Separate — Focused Only on This Topic)

| Additional Point | Why it matters to 2FA | Official Salesforce Reference |
|---|---|---|
| MFA is not merely optional for employee UI access | Salesforce requires MFA for internal logins to active production and sandbox orgs, including direct and SSO login. | [Understand MFA Requirements](https://help.salesforce.com/s/articleView?id=xcloud.mfa_make_the_case.htm&language=en_US&type=5) |
| Privileged users need phishing-resistant MFA | Standard OTP methods may not satisfy the stronger requirement for privileged users. | [Prepare for MFA Enforcement for All Employee Users](https://help.salesforce.com/s/articleView?id=005321561&language=en_US&type=1) |
| Users register their own verification method | Salesforce admins cannot register a user's MFA verifier on their behalf; each user completes registration. | [Register MFA Verification Methods](https://help.salesforce.com/s/articleView?id=xcloud.security_register_verification_methods.htm&language=en_US&type=5) |
| Salesforce differentiates verification-method strength | Passkeys/security keys are phishing-resistant; Salesforce Authenticator and TOTP apps are standard MFA methods in the documented tiering. | [Verification Method Selection](https://help.salesforce.com/s/articleView?id=sf.security_optimize_verification_method_registration.htm&language=en_US&type=5) |

## 10. Technical Clarifications

- **2FA vs. MFA:** 2FA means exactly two authentication factors; MFA is the broader umbrella for two or more factors. Salesforce policy and Help content primarily use “MFA.”
- **Factor vs. method:** A factor is a category of evidence (know/have/are). A method is a concrete implementation, such as a passkey, authenticator app, or security key.
- **Security is layered:** MFA reduces the risk of password-only compromise, but it is not a guarantee against every attack. Phishing-resistant methods provide stronger protection against credential phishing.
- **Alerts:** The source says the user may receive an alert after a blocked attempt. Alerting depends on the service and its configuration; it is not a universal consequence of 2FA.

## 11. Interview Q&A

**Q1. What is Two-Factor Authentication?**  
It verifies a user's identity using two distinct factor categories, typically a password plus a second factor such as a registered authenticator, passkey, or security key.

**Q2. What are the three factor categories?**  
Something you know, something you have, and something you are.

**Q3. If an attacker steals the password, why does 2FA help?**  
The stolen password alone is insufficient; the attacker also needs to satisfy the second factor.

**Q4. Is 2FA the same as MFA?**  
2FA uses exactly two factors. MFA is the general term for authentication requiring multiple factors.

**Q5. What does Salesforce require in 2026?**  
Salesforce MFA requirements apply to internal users accessing active production and sandbox UI logins. Privileged users have a stronger phishing-resistant MFA requirement. Exact rollout timing and applicable requirements should be checked against Salesforce’s current official guidance.

## 12. Board-Level Answer

“Passwords alone are no longer a sufficient control for protecting user accounts. Two-factor authentication adds a second, independent verification factor, reducing the likelihood that a stolen password results in account takeover. For Salesforce, we should implement MFA for employee access and adopt phishing-resistant methods—especially for privileged users—while preparing user enrollment and support processes.”

## 13. Quick Revision

- 2FA = two distinct factors; MFA is the broader term.
- KNOW = secret/password; HAVE = device/token; ARE = biometric.
- A compromised password should not be enough to pass a properly configured 2FA login.
- Salesforce uses MFA terminology and requires MFA for employee UI access to active production/sandbox orgs.
- Privileged users require phishing-resistant MFA.
- Not every MFA method has equal phishing resistance.

## 14. No-Skip Checklist

- [ ] 2FA/MFA meaning and “Your other password” analogy
- [ ] Know / Have / Are categories and examples
- [ ] Two-factor login flow
- [ ] Password-compromise scenario
- [ ] CEO/CFO four-week hacking-attempt story from source
- [ ] Public Service Announcement for banking/payment services
- [ ] Service examples from source
- [ ] Salesforce 2026 MFA requirement and phishing-resistant MFA for privileged users
- [ ] Additional knowledge kept separate from main source topic
- [ ] Interview Q&A and Board-Level Answer reviewed

## 15. Official References

- [Salesforce — Understand MFA Requirements](https://help.salesforce.com/s/articleView?id=xcloud.mfa_make_the_case.htm&language=en_US&type=5)
- [Salesforce — Learn What MFA Is and Why It's So Important](https://help.salesforce.com/s/articleView?id=sf.mfa_learn_about.htm&language=en_US&type=5)
- [Salesforce — Prepare for MFA Enforcement for All Employee Users](https://help.salesforce.com/s/articleView?id=005321561&language=en_US&type=1)
- [Salesforce — Help Users Register MFA Verification Methods](https://help.salesforce.com/s/articleView?id=xcloud.security_register_verification_methods.htm&language=en_US&type=5)
- [Salesforce — Verification Method Selection During MFA Registration](https://help.salesforce.com/s/articleView?id=sf.security_optimize_verification_method_registration.htm&language=en_US&type=5)
