# Data Privacy — Privacy Ke 3 Pillars: Find, Manage, Control
## Section 4.4 | Easy Hinglish + 2026 Salesforce Updates

> **Standalone file:** Yeh document sirf Section 4.4 ke liye hai. Sections 4.1, 4.2 aur 4.3 is file mein merge nahi kiye gaye.
>
> **Preservation rule:** Original content pehle same sequence mein preserve kiya gaya hai. Explanation, 2026 update, correction aur recommended approach separate sections mein hain.
>
> **Scope note:** Original script mein Find, Manage aur Control ke high-level ideas diye gaye hain. Detailed DSAR, Salesforce implementation aur automation examples neeche additional learning material hain, original source ke missing text ka replacement nahi.

---

# 1. Original Source — Preserved

## 4.4 Privacy Ke 3 Pillars — Find, Manage, Control
**Poora framework sirf teen steps me**

Practical level par privacy ke teen components hain: (1) apna data dhoondo, (2) usko manage karna aasaan banao, (3) customer ko control do. Har pillar spreadsheet jitna simple ya API jitna advanced ho sakta hai.

### Teeno Pillars Ek Nazar Me

**Order important hai — pehla pillar foundation hai.**

**Privacy Program Ka Order**

**1 · Find**
Apna customer data dhoondo — kya, kahan, kiska

**2 · Manage**
Manage karna aasaan banao — DSAR process

**3 · Control**
Customer ko control do — consent aur preferences

**Source labels:** Normal step · Success path · Failure / error path · Watch-out

🔍

**Pillar 1 — Find**

Foundational piece. Jab tak pata nahi ki kya data hai, kuch nahi kar sakte. Kya PII hai, kahan hai, owner kaun hai, kaun manage karta hai, kis ke saath share hota hai, kis system me rehta hai.

🧰

**Pillar 2 — Manage**

Agar koi poochhe "mera data kahan-kahan use ho raha hai" — us sawaal ka jawaab kaise doge?

🎮

**Pillar 3 — Control**

Ab jab data aur process pata hai, tab user se poochh sakte ho — "kya aap chahte hain hum aapke data ke saath ye karein?"

### Spreadsheet Se API Tak — Har Level Valid Hai

**Full data governance program hona zaroori nahi.**

###### Rudimentary (Bilkul Valid)

- Ek spreadsheet / Word doc / Quip
- "Orders yahan, marketing list wahan, billing yahan, support yahan"
- Har system ka customer record aur uska manager listed
- Request aaye to ye doc dekh kar jawaab banao

###### Advanced

- Poora data governance program
- APIs ka set
- Button dabao → data automatically wapas aata hai
- Automated DSAR fulfilment

**Spreadsheet**
Simple, manual, par bilkul valid

**APIs**
Automated, scalable, mehnga

**Important Reassurance**

"Itna simple bhi chalega." Agar aapke paas ek **updated spreadsheet** hai jo batati hai — kaunse systems hain, unke managers kaun hain, unme kaunsa data hai, aur system se nikal kar data kahan jaata hai (partners) — to aap already pillar 1 aur 2 kar rahe ho

---

# 2. Easy Hinglish Explanation

## 2.1 Teen pillars ka simple meaning

| Pillar | Simple question | Kya karna hota hai? | Output |
|---|---|---|---|
| **Find** | Hamare paas customer ka data kahan-kahan hai? | Systems, objects, fields, files, copies, owners, partners aur data flows identify karo | Data inventory / data map |
| **Manage** | Customer request kare toh hum us data ko find, review aur respond kaise karenge? | DSAR workflow, ownership, verification, deadlines, exceptions, fulfilment aur evidence manage karo | Repeatable request process |
| **Control** | Customer ki consent/preference/rights ko data use mein kaise apply karenge? | Applicable consent, opt-out, objection, preference update aur downstream enforcement implement karo | Enforceable choices + audit trail |

## 2.2 Pillar 1 — Find: Foundation

Find ka matlab sirf Salesforce mein Contact record dhoondhna nahi. Organisation ke data ka landscape samajhna hai.

Inventory mein yeh cheezein include ho sakti hain:
- **What:** PII/personal data categories—name, email, phone, account details, support history, etc.
- **Where:** Salesforce objects, fields, files, emails, data warehouse, marketing platform, support tool, backups.
- **Whose:** Data kis individual/customer se related hai.
- **Who owns it:** Business owner, system owner, technical contact.
- **Who manages it:** Data steward ya team jo request fulfilment mein action leti hai.
- **Who receives it:** Vendors, partners, analytics tools, downstream applications.
- **Why it exists:** Purpose and relevant processing context.
- **How it moves:** Collection → Salesforce → integrations → partners → analytics/archives.

**Why first?** Agar data ka location ya destination unknown hai, toh access request incomplete ho sakti hai, deletion request ke copies miss ho sakte hain, aur data sharing risk evaluate karna mushkil hota hai.

## 2.3 Pillar 2 — Manage: Request ka jawaab dene ki capability

DSAR ka matlab aam taur par **Data Subject Access Request** hota hai, lekin privacy operations mein log kabhi-kabhi is term ko broader data-subject rights requests ke liye bhi use karte hain. Access ke alawa requests mein applicable law ke hisaab se correction, deletion/erasure, portability, objection ya other rights shamil ho sakte hain.

Manage pillar ke sawal:
- Request kaise receive hogi?
- Identity verify kaise karenge?
- Kaunsa data, kaunse systems aur kaunse time period mein search hoga?
- Kaun approve/review karega?
- Third-party information, legal holds ya exceptions kaise handle honge?
- Deadline/SLA kaun track karega?
- Response kaise deliver hoga?
- Fulfilment aur exceptions ka evidence kahan store hoga?

## 2.4 Pillar 3 — Control: Choice ko actual systems mein apply karna

Control ka matlab sirf preference screen dikhana nahi. Preference/consent update ke baad system ko appropriate behaviour karna chahiye.

Example:
1. Customer email marketing opt out karta hai.
2. Preference record update hota hai.
3. Downstream marketing platform ko update bheja jata hai.
4. Send-time logic current preference check karta hai.
5. Integration failure par alert/retry/reconciliation hoti hai.
6. Change aur result ka audit evidence rehta hai.

**Legal nuance:** Consent har processing ke liye required nahi hota. Control design mein applicable lawful basis, opt-out/objection rights, communication preferences aur legal exceptions ko distinguish karna zaroori hai.

## 2.5 Spreadsheet se API tak — maturity ka spectrum

Original script ka important reassurance: start karne ke liye massive governance platform zaroori nahi. Ek well-maintained spreadsheet bhi initial data map aur ownership register ho sakti hai. Lekin spreadsheet ko current, access-controlled, owned aur testable hona chahiye.

| Approach | Useful when | Strength | Risk / limitation |
|---|---|---|---|
| Spreadsheet / Word / Quip inventory | Small scope, early maturity, low request volume | Low cost, easy to start | Stale data, manual mistakes, access control and versioning risk |
| Structured catalog / governance tool | Multiple teams and systems | Ownership, lineage, search and change governance | Setup, licensing and maintenance effort |
| APIs + workflow automation | High volume, repeatable systems, reliable APIs | Speed, consistency and scale | Integration failures, permission issues, complex exception handling |
| Hybrid | Most growing organisations | Automation for known paths; manual review for exceptions | Requires clear ownership and monitoring |




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