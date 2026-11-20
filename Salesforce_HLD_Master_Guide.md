# Salesforce HLD Interview Master Guide
## 60–70+ LPA Senior / Lead / Architect-Level Preparation

> **Core formula:**  
> **Salesforce HLD = Business Requirements + Scale + Salesforce Limits + Distributed Systems + Integration + Failure Handling + Security + Trade-offs**

---

# 1. How to Approach Any Salesforce HLD Question

Never start by drawing boxes immediately.

Use this order:

```text
1. Requirements
      ↓
2. Functional + Non-Functional Requirements
      ↓
3. Scale Estimation
      ↓
4. Identify System of Record
      ↓
5. High-Level Architecture
      ↓
6. Salesforce Components
      ↓
7. Integration Architecture
      ↓
8. Data Model
      ↓
9. Sync vs Async
      ↓
10. Security
      ↓
11. Failure Handling
      ↓
12. Limits & Scalability
      ↓
13. Observability
      ↓
14. Trade-offs
      ↓
15. Deep Dive
```

---

# 2. Ask Questions Before Designing

## What the interviewer expects

Before selecting Salesforce objects, Apex, Platform Events, or APIs, clarify the problem.

### Business questions

- What is the business use case?
- Who are the users?
- What is the expected user journey?
- What is the system of record?
- Which operations are business-critical?
- What data must be immediately consistent?

### Scale questions

- Number of users?
- Daily transactions?
- Peak transactions per second?
- Number of records?
- Data growth per year?
- Read-heavy or write-heavy?
- Peak traffic pattern?

### Integration questions

- Which external systems exist?
- ERP?
- Payment?
- Data warehouse?
- Marketing platform?
- Identity provider?
- Is integration synchronous or asynchronous?
- What are the external system SLAs?

### Reliability questions

- What happens if Salesforce is unavailable?
- What happens if an external system is unavailable?
- Can requests be retried?
- Can duplicate requests occur?
- Is data loss acceptable?
- Is eventual consistency acceptable?

### Salesforce-specific questions

- Standard objects or custom objects?
- Large Data Volume?
- Sharing/security requirements?
- API volume?
- Governor-limit constraints?
- Platform Events / CDC required?
- Batch processing required?
- Experience Cloud / external users?
- Data retention requirements?

### Senior-level signal

Don't ask questions just to fill time.

Ask questions that **change your architecture**.

---

# 🚀 Connect, Share & Follow for Complete Learning Materials (for Full material connect with me as below)

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