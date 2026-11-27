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