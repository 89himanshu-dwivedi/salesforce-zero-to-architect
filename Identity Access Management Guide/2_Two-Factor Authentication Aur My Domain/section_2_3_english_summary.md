# Section 2.3 — Salesforce's Two 2FA Modes: Service-Based vs Policy-Based

> **Exam Alert:** The difference between Service-Based and Policy-Based verification is important.

## 1. Service-Based vs Policy-Based

| Point | Service-Based | Policy-Based |
|---|---|---|
| When triggered | When Salesforce detects a new/unrecognized browser or device (usually first use) | At login or when accessing a protected action/resource, according to admin policy |
| Second factor | Click a link in the verification email (source example) | Authenticator app (Salesforce/Google), TOTP, or configured verification method |
| Who enables it | Salesforce's risk-based device-activation behavior | Admin configuration through permissions/policies |
| Result | Browser/device becomes activated; verification is usually not repeated on a recognized device | Verification is required according to the policy's scope |
| Purpose | Verify an unfamiliar device | Enforce an admin-defined security requirement |

## 2. Service-Based — First-Time / New Device Flow

### Flow — Normal / Success

```text
User logs in from a new/unrecognized browser
        ↓
Username + Password
        ↓
Salesforce detects an unfamiliar device/browser
        ↓
Verification email link is sent
        ↓
User clicks the link (identity verification)
        ↓
Device/browser is activated
        ↓
Future logins from this recognized device usually don't repeat this verification
```

### Failure / Error Path


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