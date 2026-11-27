# Section 2.4 — Org-Level 2FA Setup: From Permission Set to Authenticator App

## Show & Tell — Complete Click Path

### Prerequisite
Install Salesforce Authenticator on the phone beforehand. Enable push notifications during installation/setup.

## 1. Check Session Settings
**Setup → Session Settings**

Ensure **Two-Factor Authentication** is placed in the **High Assurance** column. If it is in the left/standard column, move it to High Assurance and Save.

## 2. Create a Test User
Test with a separate user rather than experimenting on your own administrator account.

## 3. Create a Permission Set
**Setup → Permission Sets → New**. Name the permission set and Save.

## 4. Enable the System Permission
Open the Permission Set and click **Edit**:
- Select `Two-Factor Authentication for User Interface Logins` (current Salesforce Help commonly labels this **Multi-Factor Authentication for User Interface Logins**).
- Save.

## 5. Assign Users
Permission Set detail page → **Manage Assignments → Add Assignments** → select the target users and assign.

## 6. Pair the Phone
Open Salesforce Authenticator. It displays a random **two-word phrase**. Enter that phrase in Salesforce's connection/setup screen to connect the app to the account. Pairing is generally a one-time step per connected device/account.

## 7. Test Login
Log in as the test user → receive a push notification → review and **Approve/Connect** → login completes.

## Flow

### One-Time Pairing / Registration Flow
Install Salesforce Authenticator → enable push notifications → start verifier setup in Salesforce → app displays a two-word phrase → enter phrase in Salesforce → phone/account is paired → verification method is registered.

### Daily Login Flow (Success)
Username + Password → MFA challenge → push to phone → user reviews request → Approve → login succeeds.

### Daily Login Flow (Failure / Error)
Login attempt → push is missing / phone unavailable / user denies / request times out → verification is incomplete → login is blocked or remains incomplete → use an available alternate verifier or recovery/admin assistance.

## One-Time Pairing vs. Daily Login

| Pairing / Setup (once per connection) | Daily Login |
|---|---|
| Install app and enable notifications | User attempts login |
| App provides two-word phrase | MFA push prompt |
| Enter phrase in Salesforce | User reviews request |
| Account/device is connected | Approve → login |

## Location-Based Shortcut / Automation
Salesforce Authenticator supports location-based trusted automation. After repeated trusted patterns, the app can automatically verify activity when the same user, action, browser/device, and trusted location match—so a manual phone approval may not be needed every time.

**Security note:** Automatic approval is a convenience and a security trade-off. Org policy, mobile OS permissions, location accuracy, trusted IP/network conditions, and device/browser continuity matter. Users should know how to review/remove trusted locations and recover from a lost phone.

## Real Gotcha — Presenter Experience
A user's profile language was Japanese. Changing the profile language to English did not change the language shown in the mobile app. The presenter couldn't resolve it; the user had to be deactivated and recreated. The Developer org had limited licenses, so the old user was deactivated to free the license.

**Learning:** This is an anecdotal experience, not a verified universal Salesforce bug or recommended fix. Before recreating a user, investigate app/device registration, connected verifiers, language/localization settings, browser/device behavior, and support options. Check license implications before deactivation.

## Alternative — Custom 2FA Without an Extra Device (Source Example)
One team found standard Salesforce device activation insufficient and RSA-token integration cumbersome. They built a custom verification flow within Salesforce.

### Custom Flow (as described)
1. Salesforce Flow generates a random code.
2. A Login Flow is attached to the login process.
3. The code is emailed to the user.
4. The user retrieves the code from email and completes verification.
5. The intended experience avoids a phone or hardware token.

**Security distinction/caution:** A custom email-code check can be an additional check in a custom Login Flow, but it must not automatically be treated as equivalent to Salesforce-supported MFA or phishing-resistant MFA. Validate current Salesforce MFA requirements, identity assurance, code expiry, one-time use, brute-force controls, email-account security, and privileged-user rules before production use.

## Technical Clarification
- The system permission is **Multi-Factor Authentication for User Interface Logins** (UI wording may vary; Salesforce Help currently uses this name).
- The permission set enforces MFA for assigned users' UI logins; it is distinct from device activation.
- Session security settings and MFA verification are related but distinct. Assigning MFA in the High Assurance session category is a session-security configuration, not a substitute for enabling/assigning MFA.
- MFA enforcement is evolving toward org-wide requirements for employee UI logins. Check current org-level **Identity Verification** settings and official rollout requirements rather than relying only on individual permission sets.

## Current Salesforce Comparison (2026)

| Training/source step or claim | Current Salesforce behavior / clarification |
|---|---|
| Permission Set → `Two-Factor Authentication for User Interface Logins` | Salesforce Help commonly labels this **Multi-Factor Authentication for User Interface Logins**. Permission Sets remain a supported method for selected users, especially when configuring before org-wide enforcement. |
| Use a separate test user | Still a sound admin practice. Test enrollment, push, fallback verifier, recovery, and access before broad rollout. |
| Pair using a two-word phrase | Salesforce Authenticator registration connects the app/account. A phrase can appear in connection/switch scenarios; if unexpected, troubleshoot existing or old account connections rather than assume this is always the universal pairing screen. |
| Push approval for each login | Push is a supported method, but the prompted verifier depends on org/user settings and method priority. Salesforce Authenticator also supports TOTP codes. |
| Location “remember” shortcut | Current automation can auto-verify under configured conditions. Troubleshooting guidance specifies matching user, action, browser/device, and location (within about 100 meters); mobile OS location permissions, background refresh/accuracy, and org settings matter. |
| Japanese-language Authenticator anecdote | Presenter-specific experience; not verified as a universal known bug or recommended fix. |
| Custom email-code Login Flow | Do not assume it satisfies Salesforce MFA. Evaluate current MFA and privileged-user phishing-resistant requirements separately; use Salesforce-supported methods for compliance. |
| Permission set as the long-term org-wide MFA strategy | Salesforce provides org-wide direct-UI MFA settings in Identity Verification. Enforcement can supersede per-user permission assignments for internal users. |

## Verification Matrix

| Test | Expected result |
|---|---|
| Permission Set assigned to test user | MFA permission is effective for the intended user |
| Authenticator connected | User can complete the verifier challenge |
| Push approved | Legitimate login completes |
| Push denied | Unrecognized login is denied/incomplete |
| Push unavailable | Configured alternate verifier/recovery path is usable |
| Trusted-location automation | Only eligible matching trusted conditions auto-verify |
| New phone / deactivation recovery | Admin recovery and verifier removal/re-registration process works |
| Org-wide MFA policy | Test user is covered by actual current org settings |

## Gotchas
- Do not test first on the administrator's own account.
- Push notifications must be enabled; phone settings must allow delivery.
- Keep a recovery/alternate verification method available per policy.
- Do not confuse device activation with MFA permission enforcement.
- Trusted-location automation reduces friction but needs careful configuration.
- Deactivation/recreation is not a first-line fix for app language or registration issues.
- Custom email OTP is not automatically compliant MFA.

## Recommended Architect Approach
1. Validate current Identity Verification/org MFA settings and privileged-user requirements.
2. Configure a targeted Permission Set only where appropriate; confirm its effective permission on a test user.
3. Test enrollment, approval, denial, no-push/fallback, lost phone, and new-device scenarios.
4. Pilot with a small group before wider rollout.
5. Treat trusted-location automation as an explicit security/convenience decision.
6. Avoid custom email-code authentication for production MFA unless security-reviewed and confirmed to meet Salesforce requirements.

## Interview Q&A

**Q1. How do you enable MFA for a selected group of Salesforce users?**  
Create a Permission Set, enable Multi-Factor Authentication for User Interface Logins, and assign it to target users. Verify org-wide MFA settings because enforcement can apply independently of the individual permission.

**Q2. Why use a test user?**  
To validate enrollment, login behavior, fallback, recovery, and permission effects without risking administrator access.

**Q3. What is the two-word phrase for?**  
It is part of connecting/registering the Salesforce Authenticator account/device. If it appears unexpectedly, investigate existing connections or account context.

**Q4. What does trusted-location automation do?**  
It can automatically verify eligible repeated activity when configured trusted conditions match; it is not a blanket MFA bypass for all logins.

**Q5. Is a custom email OTP Login Flow equivalent to Salesforce MFA?**  
Not automatically. Confirm current MFA policy and phishing-resistant requirements. Email OTP alone should not be assumed to satisfy them.

## Board-Level Answer
We enable Salesforce MFA using a targeted Permission Set when appropriate, pilot the enrollment and push-verification experience with a test user, validate recovery and failure paths, and confirm alignment with current org-wide MFA and privileged-user requirements. Trusted-location automation is used only after assessing its security trade-off. Custom email-code flows are not treated as a substitute for Salesforce-supported MFA without formal security validation.

## Quick Revision
- Permission Set → System Permissions → MFA for UI logins → assign users.
- Pair Authenticator once; later logins typically use push or another registered method.
- Approve = legitimate login proceeds; deny/fail = login doesn't complete.
- Location automation can auto-verify eligible trusted activity when enabled.
- Custom email OTP is a custom implementation example—not automatically equivalent to Salesforce-compliant MFA.

## No-Skip Checklist
- [ ] App install and push-notification prerequisite
- [ ] Session Settings / High Assurance placement
- [ ] Test user
- [ ] Permission Set creation
- [ ] MFA UI-login system permission
- [ ] Assignment to target users
- [ ] Authenticator registration/pairing
- [ ] Test login and push approval
- [ ] Pairing vs daily login distinction
- [ ] Success, failure, and recovery flow
- [ ] Trusted-location automation and security caveat
- [ ] Japanese-language anecdote preserved with caveat
- [ ] Custom email-code Login Flow example and security distinction
- [ ] Current Salesforce comparison (2026)
- [ ] Verification matrix
- [ ] Gotchas and architect recommendation
- [ ] Interview Q&A and Board-Level Answer

## Official Salesforce References
- [Meet MFA Requirement by Permission Set](https://help.salesforce.com/s/articleView?id=000392254&language=en_US&type=1)
- [Cannot Enable MFA on Standard Profiles — use Permission Set or Custom Profile](https://help.salesforce.com/s/articleView?id=000395447&language=en_US&type=1)
- [Enable MFA for Direct Login](https://help.salesforce.com/s/articleView?id=xcloud.security_mfa_org_wide_setting.htm&language=en_US&type=5)
- [Salesforce Authenticator for MFA](https://help.salesforce.com/s/articleView?id=sf.mfa_supported_verification_methods_sfa.htm&language=en_US&type=5)
- [Salesforce Authenticator Automation](https://help.salesforce.com/s/articleView?id=000389323&language=en_US&type=1)
- [Authenticator Troubleshooting](https://help.salesforce.com/s/articleView?id=000380378&language=en_US&type=1)
- [Use Salesforce Authenticator for MFA](https://help.salesforce.com/s/articleView?id=salesforce_authenticator_2fa.htm&language=en_US&type=0)
