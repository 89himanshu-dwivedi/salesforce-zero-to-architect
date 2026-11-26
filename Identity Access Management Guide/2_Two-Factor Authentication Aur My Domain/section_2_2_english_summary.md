# Section 2.2 — Common Types of 2FA: Hardware Tokens to Push Notifications

## Flow — How 2FA Login Works

**At-a-glance Flow:**

`Username + Password` → `Password Verified` → `Second-Factor Method Triggered` → `User Enters Code / Approves / Uses Device` → `Second Factor Validated` → **Login Success**

`Incorrect / Expired / Denied / Timeout` → **Login Not Completed**

**Basic Login Flow:**
1. The user enters a username and password.
2. The system verifies the first factor (password).
3. The system selects/triggers the registered second factor (token, OTP, authenticator app, push, or built-in authenticator).
4. The user provides or approves the second factor.
5. The system validates the second factor.
6. Login is allowed only if both factors are valid; an invalid, denied, expired, or timed-out verification does not complete login.

| Method | Second-Factor Flow |
|---|---|
| Hardware token | Password → token code/security-key challenge → verification → login |
| SMS/Voice OTP | Password → receive SMS/voice code → enter code → validate → login |
| TOTP app | Password → obtain current code from authenticator app → enter code → validate → login |
| Push notification | Password → push sent to phone → Approve/Deny → login only after approval; denial/timeout prevents login |
| Biometric / Passkey | Password or login prompt → built-in authenticator/passkey challenge → biometric/PIN verification on device → login after successful verification |

**Golden Rule:** A password alone is not enough—login completes only after the second factor is successfully verified.

## 1. Hardware Tokens
Physical authentication devices include:
- **Key fobs:** small keychain devices that display temporary codes at configured intervals.
- **USB tokens:** physical devices that store/use credentials; modern security keys can use cryptographic authentication.

**Why older tokens became less common:** hardware cost, distribution to each employee, lost/damaged-device replacement, inventory and lifecycle-management overhead.

## 2. SMS / Voice OTP
**OTP = One-Time Passcode.** After username/password verification, the service sends a code to the registered phone by SMS or voice. The user enters it; valid codes complete login, while invalid or expired codes are rejected.

Security caveat: SMS/voice OTP can be vulnerable to SIM swapping, phone-number takeover, interception, and phishing.

## 3. Software Tokens — TOTP
**TOTP = Time-Based One-Time Passcode.** An authenticator app generates a temporary code using a shared secret and current time. The user registers the app once; codes rotate after the time window. Examples include Google Authenticator and compatible apps. Continuous internet connectivity is not required to generate codes.

## 4. Push Notifications
A login attempt sends a notification to the registered phone. The user taps **Approve** or **Deny**; there is no code to type.

Salesforce Authenticator (iOS/Android) supports push notifications and TOTP codes.
- Approve: login proceeds after verification.
- Deny: login is blocked.
- Ignore/timeout: the request doesn't complete; login doesn't proceed.

Protect the phone with a device lock, PIN/biometrics, and never approve unexpected login prompts. Push verification relies on possession of the registered device.

## 5. Biometric / Built-In Authenticator
Fingerprint, face, iris, or a device PIN/password can verify identity through a device's built-in authenticator/passkey system (e.g., Face ID, Touch ID, Windows Hello). Salesforce's built-in-authenticator/passkey flow verifies through the device; biometric data is not normally sent to Salesforce as a fingerprint image.

## Additional Knowledge — Current Salesforce Check
| Additional / corrected point | Why it matters | Official reference |
|---|---|---|
| Salesforce direct-login SMS OTP is available as an MFA method for external users only (subject to configuration); it is not the standard employee MFA method. | Avoid assuming SMS OTP is available for all employee accounts. | https://help.salesforce.com/s/articleView?id=xcloud.mfa_config_available_verifiers.htm&type=5 |
| Supported Salesforce TOTP details commonly specify a 6-digit code and 30-second counter. | The source mentions 30 seconds / 2 minutes; current Salesforce TOTP requirements specify 30 seconds. | https://help.salesforce.com/s/articleView?id=sf.mfa_supported_verification_methods_totp.htm&type=5 |
| Salesforce distinguishes standard-strength methods (Salesforce Authenticator and third-party TOTP apps) from phishing-resistant methods (built-in passkeys and physical security keys). | MFA methods do not provide equal phishing resistance; privileged users may face stronger requirements. | https://help.salesforce.com/s/articleView?id=xcloud.mfa_supported_verification_methods.htm&type=5 |
| Salesforce Authenticator supports both push notifications and TOTP fallback codes. | Push is not the app's only capability; TOTP can help when connectivity is unavailable. | https://help.salesforce.com/s/articleView?id=sf.mfa_supported_verification_methods_sfa.htm&type=5 |
| Modern USB/FIDO2 security keys are different from older OTP key fobs. | Do not confuse rotating-code hardware tokens with passkey/public-key security keys. | https://help.salesforce.com/s/articleView?id=xcloud.mfa_supported_verification_methods.htm&type=5 |

## Quick Revision
- Hardware token = physical device.
- OTP = One-Time Passcode.
- TOTP = Time-Based One-Time Passcode, calculated from secret + time.
- Push = approve/deny notification, no code entry.
- Biometric/built-in authenticator = device-based identity verification.
- SMS OTP is convenient but exposed to phone-number attacks; Salesforce limits SMS MFA to external users.
- For privileged Salesforce users, prefer phishing-resistant passkeys or security keys.

## No-Skip Checklist
- [ ] Hardware token examples and management overhead
- [ ] OTP acronym and SMS/voice flow
- [ ] TOTP acronym, apps, secret/time mechanism, offline generation
- [ ] Push approve/deny/timeout and phone security
- [ ] Biometric/built-in authenticator concept
- [ ] Current Salesforce distinctions and official reference links
