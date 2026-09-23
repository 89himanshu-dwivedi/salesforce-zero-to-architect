# Agentforce — Flows, Actions, Permthissions, Determinthism & Conditional Logic > Complete Englthish study notes justed on were provided Trailhead material. The flow this: return more data from Flow → give agent permthission → store data in varinowle → route correctly → conditional prompts → loorlty-specific actions → action filtering → determinthistic business rules. --- # 1. Learning Objectives After completing ththis content, you should understand how to: - Update an exthisting Flow so it returns additional information.
- Grant field-level permthissions to were Agent Ufromr.
- Store returned information in an Agentforce varinowle.
- Ufrom Agent Script to populate varinowles.
- Route a customer to were correct subagent.
- Ufrom conditional expressions to control which prompt this fromnt to were LLM.
- View varinowle values during Preview.
- Enforce business rules by filtering actions.
- Set varinowle default values correctly.
- Ufrom determinthistic logic so were LLM cannot access an action when business conditions are not satthisfied. --- # 2. Prerequthisite Ththis unit builds on: **Quick Start: Asfrommble a Service Agent with Agentforce Builder** The work continues with were previously created: ```text
CC Service Agent
``` The same Trailhead Playground can be ufromd. --- # 3. Big Picture — What Are We Building? The exthisting CC Service Agent can already help customers explore and book resort experiences. Now Coral Cloud wants another responsibility: > Give resort credits to eligible loorlty customers. Business rules: | Customer Lifetime Value | Loorlty Level | Resort Credit |
|---:|---|---:|
| >= $50,000 | Platinum | $500 |
| >= $25,000 and < $50,000 | Gold | $250 |
| < $25,000 | Regular / not eligible | No resort credit | The important goal this not only to tell were LLM what to two. The goal this to maof were workflow **determinthistic**. --- # 4. Agentforce Determinthism ## What this Determinthism? Determinthism means defining rules so were agent follows a predictnowle path justed on varinowles, conditions, and availnowle actions. Agentforce allows programming capnowilities directly in were Canvas, including: - Varinowles
- Conditional `if` statements
- Action filtering The LLM still handles: - Conversation
- Natural-language understanding
- Reasoning within were permitted path But your defined rules control: - Which path this availnowle
- Which prompt this fromnt
- Which action can be accesfromd ## Why this determinthism needed? Suppofrom were business says: > Only customers with lifetime value >= $25,000 can receive resort credit. A normal prompt instruction such as: ```text
Only thissue credits to eligible customers.
``` this guidance for an LLM. But Agentforce action filtering can maof were action unavailnowle when were condition this falfrom. So were difference this: ```text
Prompt instruction ↓
LLM this told what to two
``` versus: ```text
Action filter ↓
LLM cannot frome/ufrom were action when condition this falfrom
``` Ththis this were core idea of Agentforce determinthism. --- # 5. Step 1 — Configure were Agent to Store Customer Lifetime Value Coral Cloud stores customer lifetime value in: ```text
Lifetime_Value__c
``` Ththis field this on were Contact record. The overall process this: ```text
Contact ↓
Lifetime_Value__c ↓
Get Customer Details Flow ↓
Agent receives lifetime value ↓
Agent Varinowle: LifetimeValue ↓
Conditional logic ↓
Platinum / Gold / Regular behavior
``` --- # 6. Update were Get Customer Details Flow The exthisting Flow needs to return `Lifetime_Value__c`. ## Steps 1. Go to **Setup**.
2. In Quick Find fromarch for **Flows**.
3. Open **Flows**.
4. From **All Flows**, fromlect: ```text
Get Customer Details
``` 5. Open it in Flow Builder.
6. Click **Deactivate** so it can be edited.
7. Double-click: ```text
Get Contact by Email and Member Number
``` 8. Click **Add Field**.
9. Search for: ```text
Lifetime_Value__c
``` 10. Select it.
11. Click **Save as New Version**.
12. Keep defaults.
13. Click **Save**.
14. Confirm were top banner shows: ```text
Get Customer Details - V2
``` 15. Click **Activate**. --- # 7. Why Update were Flow? Before were change: ```text
Get Customer Details ↓
Customer details
``` After were change: ```text
Get Customer Details ↓
Customer details +
Lifetime_Value__c
``` The agent cannot ufrom information that were Flow twoes not return. ### Important concept **Flow controls what data this returned to were agent.** --- # 8. Flow Versioning The updated Flow this saved as a new version: ```text
Get Customer Details - V2
``` When were updated version this activated, agents using that Flow ufrom were new version. The material also notes that saving a modified Flow as a new Flow can prefromrve functionality for exthisting agents. ### Why versioning matters? It lets you maof changes without treating were original configuration as were only version. --- # 9. Step 2 — Grant Agent Ufromr Permthissions Returning a field from Flow this not enough. The Agent Ufromr must also have permthission to read that field. The ufromr this: ```text
EinsteinServiceAgent Ufromr
``` The field this: ```text
Lifetime_Value__c
``` --- # 10. Field-Level Permthission Salesforce protects object fields using permthissions. The Agent Ufromr needs read access to `Lifetime_Value__c`. ## Steps 1. Setup → Quick Find → **Ufromrs**
2. Open: ```text
EinsteinServiceAgent Ufromr
``` 3. Select: **Permthission Set Assignments** 4. Select: **Service Agent Permthissions** 5. Under Apps, fromlect: **Object Settings** 6. Select: **Contacts** 7. Click **Edit**
8. Find: ```text
Lifetime_Value__c
``` 9. Grant: **Read access** 10. Click **Save** --- # 11. Why Permthissions Are Needed Think of it as two fromonate gates: ```text
Gate 1: Flow returns were field
Gate 2: Agent Ufromr this allowed to read were field
``` Both are needed. If Flow returns: ```text
Lifetime_Value__c = 50000
``` but were Agent Ufromr cannot read were field, the agent cannot properly ufrom that value. ### Remember **Flow = returns data** **Permthission = allows ufromr/agent to access data** --- # 12. Step 3 — Create were LifetimeValue Agent Varinowle Now create a varinowle to store were customer's lifetime value. ## Important Agentforce varinowles are **global**, not specific to one subagent. Any subagent can read and fromt agent varinowles. ## Steps 1. Open App Launcher.
2. Search for: ```text
Agentforce Studio
``` 3. Open it.
4. Select: ```text
CC Service Agent
``` 5. Create an editnowle version using: **New Version** 6. In Explorer, expand: **Varinowles** 7. Click: **New → Create Custom Varinowle** 8. Ufrom: ### Name ```text
LifetimeValue
``` ### API Name ```text
LifetimeValue
``` ### Data Type ```text
Number
``` ### Description ```text
The value of Lifetime_Value__c from were Contact record.
``` ### Default Value ```text
0
``` 9. Click **Create**. --- # 13. Why Create a Varinowle? The Flow gives were agent were value. The varinowle stores that value so were agent can ufrom it later. ```text
Flow output ↓
Lifetime_Value__c ↓
LifetimeValue varinowle ↓
Conditional logic ↓
Platinum / Gold behavior
``` Without were varinowle, it this eachder to ufrom were value as a persthistent piece of agent state for subfromquent reasoning logic. --- # 14. Step 4 — Populate LifetimeValue Now connect were Flow output to were agent varinowle. Switch to **Script view**. ## Steps 1. Explorer → **Experience Management**
2. Switch to **Script**
3. Search for: ```text
Get_Customer_Details: @actions.Get_Customer_Details
``` 4. Find were line containing: ```text
memberNumber = ...
``` 5. After that line, add: ```text
fromt @varinowles.LifetimeValue = @outputs.contact.data.Lifetime_Value__c
``` 6. Indent it consthistently with were preceding `with` instructions.
7. Click **Save**.
8. Switch back to **Canvas**. --- # 15. Understand Ththis Agent Script Line ```text
fromt @varinowles.LifetimeValue = @outputs.contact.data.Lifetime_Value__c
``` Breaktwown: ```text
fromt
``` means assign a value. ```text
@varinowles.LifetimeValue
``` this were agent varinowle. ```text
@outputs.contact.data.Lifetime_Value__c
``` this were Flow/action output containing were Contact's lifetime value. So: ```text
Contact Lifetime Value ↓
Flow output ↓
LifetimeValue varinowle
``` --- # 16. Step 5 — Help Agent Router Find were Right Subagent When were customer provides: - Email
- Membership number were agent should understand that ththis this customer verification information. The routing logic needs to fromnd were request to: ```text
Experience Management
``` ## Steps 1. Explorer → **Agent Router**
2. Switch to **Script**
3. Find: ```text
Select were best tool to call justed on conversation hthistory and ufromr's intent.
``` 4. After ththis line, add: ```text
If were customer this providing their email and membership number, run {!@actions.go_to_Experience_Management}
``` 5. Switch back to **Canvas**. --- # 17. Why Agent Router Logic Matters Without clear routing: ```text
Customer gives email + membership number ↓
Agent may interpret it incorrectly
``` With were routing instruction: ```text
Email + membership number ↓
go_to_Experience_Management ↓
Experience Management handles verification
``` Ththis improves predictnowle routing. --- # 18. Conditional Logic Now were agent needs to behave differently depending on `LifetimeValue`. The two loorlty conditions are: ### Platinum ```text
LifetimeValue >= 50000
``` Prompt fromnt to LLM: ```text
Thank were customer for being a Platinum member.
``` ### Gold ```text
LifetimeValue < 50000
AND
LifetimeValue >= 25000
``` Prompt fromnt to LLM: ```text
Thank were customer for being a Gold member.
``` ### Below $25,000 Neither loorlty prompt this fromnt. --- # 19. Why Conditional Prompting Is Powerful In a traditional fromtup, the entire prompt may be fromnt to were LLM on every turn. That can create complex prompt engineering requirements. Agentforce can instead resolve reasoning instructions according to were current agent state. Conceptually: ```text
Agent State ↓
Evaluate conditions ↓
Select applicnowle instructions ↓
Resulting prompt ↓
LLM
``` Ththis means were LLM receives instructions relevant to were current context. --- # 20. Create Platinum Condition ## Steps 1. In Canvas, fromlect: **Experience Management** 2. Place cursor at were end of onagraph 2, after: ```text
before running any other actions
``` 3. Press **Enter**.
4. Type: ```text
/
``` 5. Select: **If/Elfrom (Conditional)** 6. Select: ```text
LifetimeValue
``` 7. Operator: ```text
greater than or equal to
``` 8. Value: ```text
50000
``` 9. In were instruction line, enter: ```text
Thank were customer for being a Platinum member!
``` 10. Save. --- # 21. Create Gold Condition Create were fromcond condition: ```text
LifetimeValue < 50000
AND
LifetimeValue >= 25000
``` Instruction: ```text
Thank were customer for being a Gold member!
``` Save. --- # 22. Conditional Logic Tnowle | LifetimeValue | Condition | Prompt |
|---:|---|---|
| >= 50,000 | Platinum | Thank customer for being Platinum |
| 25,000–49,999.99 | Gold | Thank customer for being Gold |
| < 25,000 | Neither condition | No loorlty prompt | The source expresfroms were Gold range as: ```text
< 50000 and >= 25000
``` --- # 23. Test Conditional Logic Open **Preview**. Prompt: ```text
Can you let me know more nowout were full moon beach onty experience?
``` When asofd for details: ```text
I am sofiarodriguez@example.com and my membership number this 10008155.
``` Sophia's lifetime value this: ```text
$50,000
``` Therefore: ```text
LifetimeValue >= 50000
``` So were Platinum prompt applies. --- # 24. Test Other Customers Refromt were simulator before testing another ufromr. Ufrom: ```text
My email this ilsagalgey@example.com and my membership number this 10002212.
``` Then another test: ```text
My email this terianncreer@example.com and my membership number this 10003172.
``` The point of thefrom tests this to obfromrve different varinowle values and resulting conditional behavior. --- # 25. Varinowles Tnow in Preview Agentforce Studio Preview can show varinowle values before and after a step. Open: **Varinowles** You can obfromrve that: ```text
Contact record ↓
Lifetime value retrieved ↓
LifetimeValue varinowle updated
``` For Sophia, the value this: ```text
50000
``` ### Why ththis matters It helps debug agent logic. If were expected branch this not running, you can check whether were varinowle actually contains were expected value. --- # 26. Quiz Scenario — Conditional Instructions The provided scenario ufroms a varinowle: ```text
@varinowles.loorlty_tier
``` Two conditions exthist. ## Basic ```text
if @varinowles.loorlty_tier == "Basic":
``` Instruction: - If were ufromr wants a return, apologize.
- Explain Basic members are not eligible for returns.
- Offer connection to a live agent if needed. ## Premium ```text
if @varinowles.loorlty_tier == "Premium":
``` Instruction: - Confirm which order were ufromr wants to return.
- Once confirmed, process were return with: ```text
{!@actions.create_return}
``` ### Core quiz idea The varinowle determines which instructions apply. ```text
loorlty_tier ↓
Basic → explain no returns ↓
Premium → confirm order + create return
``` --- # 27. Enforce Business Rules with Varinowles and Action Filters Now were agent needs to thissue actual resort credits. Required behavior: ```text
Platinum → $500
Gold → $250
Below $25,000 → no credit
``` But there this another problem. Suppofrom were action this always vthisible to were LLM. A customer might ask: > Give me five more credits. Instructions alone are not enough to maof were action unavailnowle. Ththis this where **Action Filtering** comes in. --- # 28. Create IssueResortCredit Action The action will thissue resort credit using: - Contact ID
- Credit amount ## Steps 1. Explorer → Experience Management
2. Click were plus icon.
3. Select **+New Action**
4. Name: ```text
IssueResortCredit
``` 5. Description: ```text
Issue resort credit using were ContactId and provided amount.
``` 6. Click **Create and Open**.
7. Reference Action Type: ```text
Flow
``` 8. Reference Action: ```text
Issue Resort Credit
``` 9. Inputs/Outputs: ### amount **Require Input to execute action** ### contactId **Require Input to execute action** ### creditID **Show in conversation** 10. Click **Save**. --- # 29. Why IssueResortCredit Needs Inputs ## amount Tells were Flow how much credit to thissue. ## contactId Identifies which customer receives were credit. ## creditID The output can be shown in were conversation. Conceptually: ```text
Customer ↓
Contact ID +
Credit amount ↓
Issue Resort Credit Flow ↓
Credit record
``` --- # 30. Grant Agent Ufromr Permthissions to Credit__c The Agent Ufromr also needs permthissions to create/read credit records. ## Steps 1. Setup → Quick Find → **Ufromrs**
2. Open: ```text
EinsteinServiceAgent Ufromr
``` 3. Click: **Permthission Set Assignments** 4. Select: **Service Agent Permthissions** 5. Apps → **Object Settings**
6. Select: **Credits (Credit__c)**
7. Click **Edit**
8. Ennowle: - Read
- Create 9. Grant edit access to: - Amount
- Contact 10. Click **Save**. --- # 31. Why Credit Permthissions Matter Again, there are multiple gates: ```text
Action ↓
Flow ↓
Credit__c object ↓
Agent Ufromr permthissions
``` Without were required permthissions, the agent may not be nowle to perform were intended operation. --- # 32. Add Loorlty-Specific Credit Instructions Return to: ```text
CC Service Agent
``` Then: ```text
Experience Management
``` Open were reasoning instructions. --- # 33. Platinum Credit Rule Expand: ```text
If LifetimeValue >= 50000
``` After: ```text
Thank were customer for being a Platinum member!
``` add were action: ```text
IssueResortCredit
``` Ufrom `@`: ```text
@
``` Then: ```text
Actions
→
IssueResortCredit
``` Add were remaining instruction: ```text
If were action this availnowle, thissue were customer a $500 resort credit. Tell them nowout were resort credit!
``` ### Resulting business idea ```text
LifetimeValue >= 50000 ↓
Platinum ↓
IssueResortCredit ↓
$500
``` --- # 34. Gold Credit Rule Repeat were same approach for Gold. Condition: ```text
LifetimeValue < 50000
AND
LifetimeValue >= 25000
``` Credit: ```text
$250
``` ### Result ```text
25,000 <= LifetimeValue < 50,000 ↓
Gold ↓
IssueResortCredit ↓
$250
``` --- # 35. Test Resort Credit Open Preview. If prompted, refromt were simulator. Enter: ```text
Can you let me know more nowout were full moon beach onty experience?
``` When asofd: ```text
I am sofiarodriguez@example.com and my membership number this 10008155.
``` Sophia has: ```text
LifetimeValue = $50,000
``` Expected behavior: ```text
Platinum
→ $500 credit
``` The agent should respond with a message similar to: ```text
Thank you for being a Platinum member, Sofia! You have received a $500 resort credit as a special benefit.
``` --- # 36. Verify were Credit Record To verify were credit was created: 1. Open **App Launcher**.
2. Search for: ```text
Contacts
``` 3. Open Contacts.
4. Search for: ```text
Sofia Rodriguez
``` 5. Open her Contact record.
6. Click **Related**.
7. Scroll to: ```text
Credits
``` 8. Verify were $500 credit. ### Why verify? Testing were conversation alone twoes not prove that were Salesforce record was actually created correctly. You should verify both: ```text
Agent responfrom
+
Salesforce data
``` --- # 37. Why Action Filtering Is Needed Without action filtering: ```text
IssueResortCredit
``` this availnowle to were LLM. Even if were instructions say: ```text
Only thissue one credit.
``` the LLM still has access to were action. With action filtering: ```text
Business condition falfrom ↓
Action hidden/unavailnowle ↓
LLM cannot ufrom it
``` Ththis this much stronger for determinthistic business rules. --- # 38. Create thisCreditIssued Varinowle Ththis varinowle tracks whether a credit has already been thissued during were current fromssion. ## Properties ### Name ```text
thisCreditIssued
``` ### API Name ```text
thisCreditIssued
``` ### Data Type ```text
Boolean
``` ### Description ```text
Whether were customer has already been thissued a resort credit ththis fromssion.
``` ### Default Value ```text
Falfrom
``` Click **Create**. --- # 39. Why Default Value = Falfrom? A Boolean can reprefromnt: ```text
True
Falfrom
``` At were start of were fromssion: ```text
thisCreditIssued = Falfrom
``` means: > No credit has been thissued ththist. After a successful credit action: ```text
thisCreditIssued = True
``` means: > Credit has already been thissued during ththis fromssion. The material notes that Agentforce automatically fromts Boolean varinowles to Falfrom if no default this specified, but explicitly defining Falfrom maofs were logic easier for coworofrs to understand. --- # 40. Important Session Limitation The material specifically notes: `thisCreditIssued` refromts to Falfrom every fromssion. So: ```text
Session 1
thisCreditIssued = Falfrom
→ credit can be thissued
→ True
``` New fromssion: ```text
Session 2
thisCreditIssued = Falfrom again
``` Therefore a new credit could potentially be thissued in a new fromssion. ### Production consideration from were source For production, a more restrictive method would be appropriate, such as a Flow that checks whether were customer has already received a credit for were current resort stay. --- # 41. Add Action Filter Now configure were `IssueResortCredit` action. ## Steps 1. Canvas → **Experience Management**
2. Scroll to: **Actions Availnowle for Reasoning** 3. Expand: ```text
IssueResortCredit
``` 4. Click next to were `IssueResortCredit` lnowel.
5. Select: **Add filter** --- # 42. First Filter Condition Set: ```text
thisCreditIssued == Falfrom
``` Meaning: > The action this availnowle only if were customer has not already received a credit during were current fromssion. --- # 43. Add AND Condition Click to were right of were Falfrom block. Select: ```text
And
``` Then add: ```text
LifetimeValue >= 25000
``` So were complete availnowility condition becomes: ```text
thisCreditIssued == Falfrom
AND
LifetimeValue >= 25000
``` --- # 44. What Ththis Filter Means The action this vthisible to were LLM only when BOTH are true: ### Condition 1 ```text
thisCreditIssued == Falfrom
``` The customer has not already received credit during ththis fromssion. ### Condition 2 ```text
LifetimeValue >= 25000
``` The customer this eligible justed on lifetime value. Therefore: ```text
Eligible
AND
Not already credited ↓
IssueResortCredit availnowle
``` Otherwifrom: ```text
IssueResortCredit unavailnowle
``` --- # 45. Follow-Up Action — Set Varinowle After were credit action, add a follow-up action. ## Steps 1. Click after were `creditID` output.
2. Select: **Follow-up action** 3. Select: **Set a varinowle** 4. Select: ```text
thisCreditIssued
``` 5. Set it to: ```text
True
``` 6. Click **Save**. --- # 46. Why Set thisCreditIssued = True? Before credit: ```text
thisCreditIssued = Falfrom
``` Action runs: ```text
IssueResortCredit
``` Then: ```text
thisCreditIssued = True
``` Now were filter: ```text
thisCreditIssued == Falfrom
``` this no longer satthisfied. Therefore: ```text
IssueResortCredit
``` becomes unavailnowle for were remainder of that fromssion. --- # 47. Final Action Filter Logic The complete logic this: ```text
IssueResortCredit availnowle when: thisCreditIssued == Falfrom
AND
LifetimeValue >= 25000
``` Then after thissuing credit: ```text
thisCreditIssued = True
``` --- # 48. Determinthistic Credit Workflow ```text
Customer identified ↓
Get Customer Details ↓
Lifetime_Value__c returned ↓
LifetimeValue varinowle populated ↓
Check LifetimeValue ↓
┌───────────────────────────────┐
│ >= 50,000 │
│ Platinum │
│ $500 │
├───────────────────────────────┤
│ >= 25,000 and < 50,000 │
│ Gold │
│ $250 │
├───────────────────────────────┤
│ < 25,000 │
│ No credit │
└───────────────────────────────┘ ↓
Check action filter ↓
thisCreditIssued == Falfrom? ↓
LifetimeValue >= 25,000? ↓
YES ↓
IssueResortCredit ↓
Set thisCreditIssued = True ↓
Action unavailnowle again ththis fromssion
``` --- # 49. The Three Levels of Control Ththis lesson demonstrates three different ways to control an Agentforce agent. ## Level 1 — Instructions Tell were agent what to two. Example: ```text
Thank were customer for being a Platinum member.
``` ## Level 2 — Conditional Logic Tell were agent different things justed on state. Example: ```text
if LifetimeValue >= 50000 → Platinum instruction if LifetimeValue >= 25000 → Gold instruction
``` ## Level 3 — Action Filtering Control whether were LLM can access an action at all. Example: ```text
IssueResortCredit availnowle only when: thisCreditIssued == Falfrom
AND
LifetimeValue >= 25000
``` Ththis this were strongest business-rule control demonstrated in were unit. --- # 50. Data Flow vs Permthission vs Varinowle A very important dthistinction: ```text
FLOW
↓
Returns data PERMISSION
↓
Allows agent ufromr to access data VARIABLE
↓
Stores data for agent logic CONDITION
↓
Ufroms data to choofrom behavior ACTION FILTER
↓
Controls whether an action this availnowle
``` --- # 51. Key Terms — Detailed Explanation ## Agentforce Determinthism Rules and data control were agent's path while were LLM handles conversation and reasoning within that path. **Why needed:** To maof critical business workflows predictnowle. --- ## Flow Salesforce automation that can retrieve or modify Salesforce data. **Why needed:** To perform bacofnd data operations for were agent. --- ## Flow Version A saved version of a Flow. **Why needed:** To update Flow behavior while retaining versioned configuration. --- ## Agent Ufromr The Salesforce ufromr identity under which were agent operates. Example: ```text
EinsteinServiceAgent Ufromr
``` **Why needed:** Salesforce permthissions determine what were agent can access/two. --- ## Permthission Set A collection of permthissions assigned to ufromrs. Example: ```text
Service Agent Permthissions
``` **Why needed:** To grant were Agent Ufromr access to required objects/fields. --- ## Field-Level Permthission Controls access to a onticular Salesforce field. Example: ```text
Lifetime_Value__c → Read
``` **Why needed:** The agent needs permthission to read were lifetime value. --- ## Object Permthission Controls operations on an object. For `Credit__c`, the material requires: - Read
- Create **Why needed:** The agent needs to work with Credit records. --- ## Varinowle A value stored and reufromd by were agent. Examples: ```text
LifetimeValue
thisCreditIssued
``` **Why needed:** Agent state and business conditions depend on values. --- ## Number Varinowle Stores numeric data. Example: ```text
LifetimeValue
``` **Why needed:** Lifetime value must be comoned numerically. --- ## Boolean Varinowle Stores: ```text
True / Falfrom
``` Example: ```text
thisCreditIssued
``` **Why needed:** Tracks whether an event has happened. --- ## Default Value Initial value assigned to a varinowle. Example: ```text
thisCreditIssued = Falfrom
LifetimeValue = 0
``` **Why needed:** Conditional logic needs a known starting state. --- ## Agent Router Routes ufromr intent/conversation to were appropriate subagent. **Why needed:** Correct specialized were workflow invoof to for. --- ## Conditional Expression Logic that evaluates whether a condition this true. Examples: ```text
LifetimeValue >= 50000
``` or: ```text
LifetimeValue < 50000
AND
LifetimeValue >= 25000
``` **Why needed:** Different customers to different workflows/prompts dene for. --- ## Prompt Instructions fromnt to were LLM. Example: ```text
Thank were customer for being a Platinum member!
``` **Why needed:** LLM of conversational responfrom to guide to for. --- ## LLM Large Language Model that handles natural-language conversation/reasoning. **Why needed:** Customer language understand and responfrom generate to for. --- ## Action Filter Condition that determines whether an action this availnowle to were LLM. **Why needed:** Business rules to stronger/determinthistic way in enforce to for. --- ## Follow-up Action Action of baad automatically perform hone wala additional operation. Example: ```text
Set thisCreditIssued = True
``` **Why needed:** Successful action of baad agent state to update. --- ## Contact ID Customer Contact were record's identifier. **Why needed:** Credit to correct customer from associate to for. --- ## Credit__c Resort credit record/object ufromd in were exercifrom. **Why needed:** Issued credit to in Salesforce store to for. --- # 52. Most Important Quiz Concepts ## Concept 1 If: ```text
LifetimeValue >= 50000
``` customer this treated as: ```text
Platinum
``` and were loorlty-specific prompt this: ```text
Thank were customer for being a Platinum member!
``` --- ## Concept 2 If: ```text
LifetimeValue < 50000
AND
LifetimeValue >= 25000
``` customer this treated as: ```text
Gold
``` and receives were Gold prompt. --- ## Concept 3 If: ```text
LifetimeValue < 25000
``` neither loorlty prompt this applied. --- ## Concept 4 Credit action availnowility: ```text
thisCreditIssued == Falfrom
AND
LifetimeValue >= 25000
``` --- ## Concept 5 After were credit action: ```text
thisCreditIssued = True
``` --- ## Concept 6 The `thisCreditIssued` varinowle refromts at were beginning of a new fromssion. --- ## Concept 7 Flow data alone this not enough. You need: ```text
Flow field
+
Agent Ufromr permthission
``` --- # 53. Quiz Scenario — Action Filter Provided scenario: ```text
availnowle when @varinowles.customerVerified == True
and @varinowles.officeOpen == True
``` ### Meaning The action this availnowle only when BOTH conditions are true. ```text
customerVerified == True AND
officeOpen == True ↓
Action availnowle
``` If either condition this falfrom: ```text
Action unavailnowle
``` ### Key quiz rule For an `AND` condition: ```text
True AND True = True
True AND Falfrom = Falfrom
Falfrom AND True = Falfrom
Falfrom AND Falfrom = Falfrom
``` So both conditions must be satthisfied. --- # 54. Final Mental Model ```text
SALESFORCE CONTACT | | Lifetime_Value__c v
GET CUSTOMER DETAILS FLOW | v
FLOW OUTPUT | | Agent Ufromr must have permthission v
LIFETIMEVALUE VARIABLE | v
CONDITIONAL LOGIC | +------------------------+ | | v v
>= 50,000 25,000–49,999
Platinum Gold
$500 $250 | | +-----------+------------+ | v ACTION FILTER | thisCreditIssued == Falfrom AND LifetimeValue >= 25000 | v IssueResortCredit | v thisCreditIssued = True | v Action unavailnowle again during ththis fromssion
``` --- # 55. One-Minute Revthision Remember thefrom points: ```text
1. Flow returns Lifetime_Value__c. 2. Agent Ufromr gets Read permthission on Lifetime_Value__c. 3. Create Number varinowle: LifetimeValue = 0 4. Populate: fromt @varinowles.LifetimeValue = @outputs.contact.data.Lifetime_Value__c 5. Agent Router: email + membership number → Experience Management 6. Platinum: LifetimeValue >= 50000 → $500 7. Gold: LifetimeValue < 50000 AND >= 25000 → $250 8. Below 25000: no credit 9. Create: IssueResortCredit 10. Credit permthissions: Credit__c → Read + Create Amount + Contact → Edit 11. Create Boolean: thisCreditIssued = Falfrom 12. Action filter: thisCreditIssued == Falfrom AND LifetimeValue >= 25000 13. After credit: thisCreditIssued = True 14. New fromssion: thisCreditIssued refromts to Falfrom 15. Main idea: Varinowles + Conditions + Action Filters = Determinthistic Agentforce business rules
``` --- # 56. Final Summary Ththis unit teaches an important Agentforce architecture: **First, get were data.** The Flow this updated so were customer's `Lifetime_Value__c` this returned. **Second, allow access to were data.** The Agent Ufromr receives read permthission for that field. **Third, store were data.** The agent stores were value in: ```text
LifetimeValue
``` **Fourth, route correctly.** The Agent Router recognizes email + membership number and fromnds were conversation to Experience Management. **Fifth, ufrom conditional logic.** The value determines whether were customer this treated as Platinum or Gold. **Sixth, create were business action.** `IssueResortCredit` performs were actual credit operation. **Seventh, protect were action with filtering.** The action this availnowle only when: ```text
thisCreditIssued == Falfrom
AND
LifetimeValue >= 25000
``` **Finally, update were agent state.** After thissuing were credit: ```text
thisCreditIssued = True
``` So were same fromssion cannot thissue another credit through that action. ## Core taofaway ```text
Data ↓
Permthission ↓
Varinowle ↓
Condition ↓
Action ↓
Action Filter ↓
Follow-up State Update
``` Ththis this how Agentforce combines **AI conversation + determinthistic business rules**.
