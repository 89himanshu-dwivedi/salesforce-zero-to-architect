# Section 4.9 — Salesforce Feature: Consent Ka Tiered Model
## Individual se Purpose Tak 4 Levels | Easy Hinglish Notes | Standalone File

> **Golden Rule:** Original source ko pehle preserve kiya gaya hai. Explanation, official Salesforce documentation-based updates, corrections, examples, gotchas aur interview Q&A alag sections mein hain. Yeh file sirf Section 4.9 ke liye hai.

---

# 1. Original Source — Preserved

## 4.9Salesforce Feature — Consent Ka Tiered ModelIndividual se purpose tak 4 levels+

Consent ko manage karne ke liye Salesforce ne ek tiered model banaya — broadest se lekar sabse granular tak. Har level ek alag object hai, aur upar se ek legal basis ka concept bhi hai.

### Ye Objects Kyu Bane — Bespoke Solutions Ki Problem

**Log ek jaise hi bespoke solutions bana rahe the.**

**Feature Kaise Bana**

**Customers Ko Consent Manage Karna ThaPar platform me kuch tha nahi**

**Sab Ne Bespoke Solutions BanayeApne-apne custom objects**

**Research HuiLog kaise manage kar rahe hain**

**Pata Chala — Sab Ek Jaise Hi Bana Rahe TheSimilar patterns**

**Standard Objects Introduce KiyeAb platform me built-in**

**Normal stepSuccess pathFailure / error pathWatch-out**

**Optional Hain**

Ye objects system me maujood hain — agar aapke privacy program me fit hote hain to use karo, warna zaroori nahi.

### Consent Ke 4 Levels — Broad Se Granular Tak

**Har level neeche wale se zyada specific hai.**

**Level 1 — Sabse Broad**

**Individual**Bahut broad privacy protections — jaise "mujhe solicit mat karo", "mujhe track mat karo"

**Level 2**

**Channel Consent**Channel ke level par — "email theek hai, par mujhe call mat karo"

**Level 3**

**Contact Point Consent**Specific email / phone par — "personal email par bhejo, work email par nahi"

**Level 4 — Sabse Granular**

**Purpose Consent**Purpose ke hisaab se — "email theek hai par sirf new products ke liye", ya "gmail par sirf webinars ke liye"

| **Level**         | **Example**                                                            | **Kya Control Karta Hai**                                             |
| :---------------- | :--------------------------------------------------------------------- | :-------------------------------------------------------------------- |
| 1 · Individual    | "Mujhe solicit mat karo", "Mujhe track mat karo"                       | Sabse broad, sab par lagoo                                            |
| 2 · Channel       | "Email OK, phone call bilkul nahi"                                     | Communication channel                                                 |
| 3 · Contact Point | "Personal email par bhejo, work email par nahi"                        | Specific email address / phone number                                 |
| 4 · Purpose       | "Email OK, par sirf new products ke liye" ya "gmail par sirf webinars" | Data use purpose — aur ye directly contact point se bhi jud sakta hai |

**Level 3 + 4 Ka Combination**

Aap dono ko mila sakte ho: "Mera **personal gmail** use karo, par sirf **webinars** ke purpose ke liye." Yaani purpose ko contact point se seedha address kar sakte ho.

### Legal Basis Aur Party Consent (CCPA Ke Liye)

**Sab kuch consent par nahi chalta.**

**Kya Aapko Consent Chahiye Bhi Hai?**

**Customer Ko Kuch Bhejna HaiEmail / communication / processing**

**Legal Basis Check KaroData Use Legal Basis object**

**Kaunsa Legal Basis Hai?**

**Success**

**ContractJaise billing bhejna**

**Consent Ki Zarurat NahiData subject opt out nahi kar sakta**

**Bhej DoLegally valid**

**Failure**

**Sirf Consent Par BasedJaise marketing email**

**Consent Check KaroTiered objects me**

**Opt Out Hai To Mat BhejoWarna violation**

**Normal stepSuccess pathFailure / error pathWatch-out**

**Important Concept**

**Consent sirf ek legal basis hai** — aur bhi hain. Kuch data use purposes aise hote hain jinse data subject **opt out kar hi nahi sakta** (jaise contract ke tehat billing). Isliye **Data Use Legal Basis** object alag se hai.

#### Party Consent — CCPA specific

🏆

**Top-Level Consent Item**

"Aap mera data third parties ke saath share kar sakte ho", "Aap mera data third parties ko bech sakte ho" — ye CCPA ke core threads hain.

⏰

**Time-Bound Permission**

"Main ye permission ek nirdharit samay ke liye de raha hoon."

🎯

**Purpose-Limited**

"Main ye permission sirf kuch specific data use purposes ke liye de raha hoon."

🇩🇪

**Double Opt-In Support**

"Maine iske liye**dusri baar**opt in kiya hai" — Germany jaise double opt-in requirement wale markets ke liye.

---

# 2. Easy Hinglish Explanation

## 2.1 Tiered consent model ka simple meaning

Salesforce Consent Data Model ek individual ki privacy aur communication preferences ko multiple levels par store aur evaluate karne ke liye standard data model hai. Broad/global preference se shuru karke channel, specific contact point aur data-use purpose tak granularity badhayi ja sakti hai.

Yeh chaar levels ek conceptual model hain; har level ke liye alag Salesforce consent object/record type use hota hai. Exact object mapping aur relationships ko org ke current Salesforce model ke against verify karein.

## 2.2 Four levels ko example se samjho

### Level 1 — Individual / Global consent
Broad preference, jaise customer ki overall privacy preference, tracking ya solicitation-related choices. Iska exact behavior fields and configuration par depend karta hai. “Individual” object ko sirf ek generic global opt-out checkbox na samjhein; ismein multiple privacy preference fields ho sakte hain.

### Level 2 — Channel consent
Customer email allow karta hai lekin phone calls ya SMS nahi. Salesforce standard model mein is level ko commonly **Contact Point Type Consent** se represent kiya jata hai. Current Salesforce documentation mein channel/engagement-channel terminology product ke hisaab se vary kar sakti hai.

### Level 3 — Contact Point consent
Customer ke kisi specific email address ya phone number par preference. Example: `personal@gmail.com` allowed hai, `work@company.com` par marketing nahi. Standard object: **Contact Point Consent**.

### Level 4 — Data Use Purpose
Preference specific purpose ke liye: marketing, billing, surveys, product announcements, webinars, etc. Standard model mein **Data Use Purpose** purpose ko define karta hai, aur consent records is purpose se associate ho sakte hain. “Purpose Consent” ko ek alag universal object name samajhne ke bajay actual Salesforce object relationships check karein.

## 2.3 Levels ko combine kaise karte hain?

Example: “Mujhe `personal@gmail.com` par webinars ke invites bhej sakte ho, lekin product promotions nahi.”

Evaluation ke liye system ko identify karna hoga:
1. Correct individual kaun hai?
2. Relevant channel email hai?
3. Kaunsa contact point use ho raha hai?
4. Purpose `Webinar Invitations` hai ya `Product Marketing`?
5. Kya applicable consent/preference active hai?
6. Kya consent effective dates, withdrawal, brand or other rules apply karte hain?

Agar multiple records conflicting hon, system ko documented precedence/cascade rules ke mutabik resolve karna chahiye. Sirf latest ya most permissive record ko automatically choose karna safe assumption nahi.

## 2.4 Bespoke solutions se standard model tak

Customers pehle custom objects/checkboxes se consent store karte the. Isse inconsistent field names, inconsistent opt-out logic, duplicate data aur difficult reporting ho sakti thi. Standard data model common concepts ko represent karne mein help karta hai.

Lekin standard objects ka available hona iska matlab nahi ki existing custom implementation automatically replace ho jaye. Migration se pehle mapping, duplicate resolution, data quality, historical evidence, integration changes and sending-system enforcement plan karna chahiye.


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