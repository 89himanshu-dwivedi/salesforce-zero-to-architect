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
