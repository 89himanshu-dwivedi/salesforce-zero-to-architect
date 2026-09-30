# Learn About Agentforce Builder --- Complete English Notes

## Purpose

These notes explain how to build and deploy a Salesforce Agentforce
service agent for Coral Cloud Resorts. They cover the learning
objectives, setup, agent and subagent creation, actions and flows,
reasoning instructions, testing, deployment, key terms, end-to-end
workflows, and a completion checklist.

The notes are organized as a practical walkthrough. Each major concept
includes a short explanation of why it matters.

------------------------------------------------------------------------


---

## 1. Learning Objectives

In this badge, you will learn how to:

-   Create a service agent in Agentforce Builder.
-   Create a custom subagent for the service agent.
-   Build custom actions that use Salesforce flows.
-   Update an existing deployment flow.
-   Add the service agent to an Experience Cloud site.

### Overall workflow

**Agentforce Builder → Service Agent → Custom Subagent → Actions and
Flows → Reasoning Instructions → Test → Activate → Update Deployment
Flow → Experience Cloud → Customer Chat**

------------------------------------------------------------------------


---

## 2. Coral Cloud Resorts and Agentforce Builder

Coral Cloud Resorts focuses on destination activities and customer
service. As the business grows, service agents need to answer questions,
recommend activities, check availability, and book experiences.

### The problem

-   Customer requests are increasing.
-   Human service agents have a growing workload.
-   Questions about activities, availability, and bookings need to be
    handled efficiently.

### The solution

Use Agentforce Builder to create an AI service agent that can:

-   Provide information about activities and experiences.
-   Retrieve availability and session information.
-   Validate customer details.
-   Create experience-session bookings.
-   Be made available through a customer-facing Experience Cloud site.

------------------------------------------------------------------------


---

## 3. What Is Agentforce Builder?

Agentforce Builder is used to create, configure, test, and activate
agents.

### 3.1 Natural-language configuration

You can describe in plain language what the agent should do. AI
assistance can help generate or refine:

-   Subagents.
-   Instructions.
-   Actions.
-   Guardrails and logic.

**Why it matters:** You do not need to configure every behavior
manually. Natural language makes it easier to describe the agent's
intended behavior.

### 3.2 Portability

Agent configuration can be developed using a text-first approach.
Instructions are visible in natural language in the canvas.

Benefits include:

-   Easier copying and pasting of text.
-   Sharing patterns across teams.
-   Reusing established patterns in another environment or team.
-   Keeping configuration from being scattered across unrelated
    settings.

**Why it matters:** Reusable, readable configuration makes agent
development and maintenance easier.

### 3.3 Flexibility

You can edit an agent in several ways:

-   **Canvas View:** Configure the agent using natural language.
-   **Agentforce Assistant:** Use plain-language requests to suggest
    changes.
-   **Script View:** Edit Agent Script directly.

Script View includes syntax highlighting, autocompletion, and
validation.

**Why it matters:** Canvas View is convenient for common changes, while
Script View gives developers more precise control.

### 3.4 Real-time visibility

The preview panel lets you inspect the agent while testing, including:

-   The agent's responses.
-   The plan it forms.
-   Actions it executes.
-   Interaction details.
-   Important events and metadata.
-   What happened at each step.

**Why it matters:** Inspect the agent's behavior before activating it
for customer use.

------------------------------------------------------------------------


---

## 4. Agent Script

**Agent Script** is the scripting language used to build and control
agents in Agentforce Builder. It combines natural-language flexibility
with programmatic expressions.

**Why it matters:** Natural language can describe high-level behavior,
while Agent Script can specify exact action references and logic.

------------------------------------------------------------------------


---

## 5. Create a Developer Edition Org or Playground

Use a custom Trailhead Playground or Developer Edition org that includes
Agentforce Studio and sample data.

### Steps

1.  Click **Create Playground**.
2.  Click **Yes, Create Playground**.
3.  Allow the new org to be attached to your Trailhead account.
4.  Note the org's expiration date.
5.  Click **Launch** to open the playground.

**Why it matters:** The playground provides the Salesforce environment
and sample data needed to complete the exercises.

------------------------------------------------------------------------


---

## 6. Enable Agentforce Studio

### Steps

1.  Open **Setup** using the Setup icon.
2.  In Setup Quick Find, search for **Salesforce Go**.
3.  Find or select **Agentforce Studio**.
4.  Click **Get Started**.
5.  Click **Turn On**.
6.  Click **Turn On** again in the confirmation window.

**Why it matters:** Agentforce Studio must be enabled before you can use
its tools to create and configure agents.

------------------------------------------------------------------------


---

## 7. Publish the Experience Cloud Site

The agent will be deployed on the Coral Cloud Experience Cloud site.

### Steps

1.  In Setup Quick Find, search for **All Sites**.
2.  Find the Coral Cloud site and click **Builder**.
3.  If a popup appears, click **OK**.
4.  Click **Publish** in the upper-right corner.
5.  Confirm by clicking **Publish**.
6.  Click **Got It**.
7.  Close the Experience Builder browser tab when finished.

**Why it matters:** The customer-facing site must be published so that
customers can access the experience and its chat interface.

------------------------------------------------------------------------


---

## 8. Create the Service Agent

### Steps

1.  Open the App Launcher.

2.  Search for and open **Agentforce Studio**.

3.  Click **New Agent**.

4.  For "What do you want your agent to do?", enter:

    ``` text
    You are a customer service representative, helping our guests make reservations, update bookings, and navigate all that Coral Cloud Resorts has to offer.
    ```

5.  Press Enter or Return.

6.  Set the agent name to:

    ``` text
    CC Service Agent
    ```

7.  Confirm that the Developer Name is populated automatically.

8.  Choose **Select User**.

9.  Search for and select:

    ``` text
    EinsteinServiceAgent User
    ```

10. Click **Let's Go**.

11. Click **Skip Ahead**.

**Important:** Use the exact agent name **CC Service Agent**.

**Why it matters:** The service agent is the main AI agent that handles
customer requests and routes them to the appropriate subagent or action.

------------------------------------------------------------------------


---

## 9. The Three Main Areas of Agentforce Builder

Agentforce Builder has three main areas:

1.  **Navigation Explorer**
2.  **Editor View**
3.  **Agentforce Assistant**

------------------------------------------------------------------------


---

## 10. Navigation Explorer

The Navigation Explorer organizes the agent's configuration.

Its main sections include:

-   Settings
-   Subagents
-   Variables
-   Connections
-   Data

### 10.1 Settings

Settings contain details such as the agent's:

-   Name.
-   Role.
-   Description.
-   Language.
-   Other configuration options.

**Why it matters:** These settings define the agent's identity and basic
configuration.

### 10.2 Subagents

Subagents are specialized workers that handle particular types of tasks.
The agent includes an **Agent Router** and its associated subagents.

#### Agent Router

The Agent Router uses the user's input and conversation history to
determine which subagent should handle the request.

**Example:** Customer request → Agent Router → Experience Management
subagent.

**Why it matters:** Specializing work helps route each request to the
appropriate part of the agent.

### 10.3 Variables

Variables store values that the agent can reference. They can support
logic, retain relevant values, and help the agent make decisions.

**Why it matters:** Agent logic often needs values to be stored and
reused during a conversation or process.

### 10.4 Connections

Connections link the agent to user-facing channels, such as:

-   Messaging.
-   Slack.
-   Voice.

**Why it matters:** Connections determine the channels through which
users can interact with the agent.

### 10.5 Data

Data includes knowledge sources that the agent can retrieve information
from. For example, a PDF containing FAQs may be uploaded as a knowledge
source through Data 360.

**Why it matters:** Connected knowledge sources help the agent provide
relevant answers based on available information.

------------------------------------------------------------------------


---

## 11. Editor View: Canvas View and Script View

Selecting an item in the Navigation Explorer opens it in the Editor
View.

### Canvas View

Canvas View is a natural-language editor. It provides:

-   Natural-language editing.
-   Shortcuts for common logic.
-   Resource pickers.
-   Access to subagents.
-   Access to actions.
-   Access to variables.

### Script View

Script View allows direct editing of Agent Script and includes:

-   Syntax highlighting.
-   Autocompletion.
-   Validation.

**Why it matters:** Canvas View supports accessible configuration, while
Script View supports precise editing and explicit references.

------------------------------------------------------------------------


---

## 12. Agentforce Assistant

Agentforce Assistant helps modify agent configuration using
plain-language requests.

Example requests:

``` text
Update the name of this agent to Coral Cloud Service Agent.
```

``` text
Create a new subagent named Case Management.
```

The assistant may ask clarifying questions. Proposed changes are applied
after the user confirms them.

**Why it matters:** Conversational assistance can make agent
configuration easier, while confirmation keeps the user in control of
changes.

------------------------------------------------------------------------


---

## 13. Create the Custom Subagent: Experience Management

Coral Cloud Resorts needs a specialized subagent for resort activities,
called **Experience Management**.

Examples of experiences include scuba diving, kayaking, and hiking.

The subagent handles:

-   Questions about experiences.
-   Availability.
-   Reservations.
-   Session bookings.
-   Experience details.

### Steps

1.  In the Explorer, open **Subagents**.

2.  Click the plus icon and select **New Subagent**.

3.  Set the name to:

    ``` text
    Experience Management
    ```

4.  Set the description to:

    ``` text
    This subagent addresses customer inquiries and issues related to booking experiences at Coral Cloud Resorts, including making reservations, modifying session bookings, and answering queries about experience details.
    ```

5.  Click **Create and Open**.

6.  Confirm that Experience Management opens.

7.  Click **Save**.

**Why it matters:** A specialized subagent keeps experience-related
responsibilities together instead of placing all task logic in the main
service agent.

------------------------------------------------------------------------


---

## 14. What Are Actions?

Actions are tools that a subagent can use to perform work, such as
retrieving Salesforce data or carrying out an operation.

For example, when a customer asks about an experience, the agent may
need to retrieve its details.

**Why it matters:** Instructions tell an agent what it should do;
actions provide the mechanism to retrieve data or perform an operation.

------------------------------------------------------------------------


---

## 15. Create the Custom Action: Get Experience Details

### Steps

1.  Open the **Experience Management** subagent.

2.  Click **Add Action**.

3.  Select **+ Create a Custom Action**.

4.  Set the name to:

    ``` text
    Get Experience Details
    ```

5.  Set the description to:

    ``` text
    Provides details about an Experience__c that a user would like more information about.
    ```

6.  Click **Create and Open**.

7.  Set **Reference Action Type** to **Flow**.

8.  Set **Reference Action** to **Get Experience Details**.

9.  For the `experienceName` input, enable **Require Input to execute
    action**.

10. For the `experienceRecord` output, enable **Show in conversation**.

11. Leave the other options unchanged.

12. Click **Save**.

### Why the settings matter

-   **Require Input to execute action:** Ensures that the required input
    is supplied before the action runs.
-   **Show in conversation:** Makes the action's output available in the
    conversation.

The agent needs this action to retrieve experience details from
Salesforce.

------------------------------------------------------------------------


---

## 16. Validate Customer Details

Before running other actions, the agent must identify the customer using
the required information:

-   Email address.
-   Membership number.

### Create the Get Customer Details action

1.  In Experience Management, click the plus icon.

2.  Select **+ New Action**.

3.  Set the name to:

    ``` text
    Get Customer Details
    ```

4.  Set the description to:

    ``` text
    Validate the customer details by passing their email and memberNumber to see if there is a related contact.
    ```

5.  Click **Create and Open**.

6.  Set **Reference Action Type** to **Flow**.

7.  Set **Reference Action** to **Get Customer Details**.

8.  Configure the inputs and output:

    -   `email` → **Require Input to execute action**
    -   `memberNumber` → **Require Input to execute action**
    -   `contact` → **Show in conversation**

9.  Leave the other options unchanged.

10. Click **Save**.

**Why it matters:** Customer validation is a security and identification
step. The agent should identify the customer before running other
actions that depend on the customer's record.

------------------------------------------------------------------------


---

## 17. Add Existing Actions from the Asset Library

If actions already exist, reuse them from the Asset Library instead of
creating duplicates.

The two existing actions are:

-   **Get Sessions:** Retrieves the sessions associated with an
    experience.
-   **Create Experience Session Booking:** Creates a new booking record
    in Salesforce.

### Steps

1.  In Experience Management, click the plus icon.

2.  Select **Add from Asset Library**.

3.  Search for:

    ``` text
    session
    ```

4.  Select:

    -   **Create Experience Session Booking**
    -   **Get Sessions**

5.  Click **Add to Agent**.

6.  Confirm that Experience Management has four actions in total.

7.  Click **Save**.

### The four actions

1.  Get Experience Details
2.  Get Customer Details
3.  Create Experience Session Booking
4.  Get Sessions

**Why it matters:** Reusing existing assets avoids duplicated work and
makes use of actions that have already been created.

------------------------------------------------------------------------


---

## 18. Reasoning Instructions

Adding actions is not enough. The agent also needs instructions
explaining:

-   Which action to run.
-   The order in which actions should run.
-   What information to collect from the customer.
-   How to use the results returned by actions.

These are configured as **Subagent Reasoning Instructions**.

**Why it matters:** Reasoning instructions guide the agent's use of its
tools and help it follow the required workflow.

------------------------------------------------------------------------


---

## 19. Configure Experience Management Reasoning Instructions

Replace the current instructions with the following:

``` text
1. If a customer would like more information on Activities or Experiences, you should run the appropriate action and then summarize the results with improved readability. Always ensure you know the customer before running this action.

2. If the customer is not known, you must always ask for their email address and membership number to get their Contact record by running {!@actions.Get_Customer_Details} before running any other actions.

3. If asked to get sessions for an experience, use {!@actions.Get_Sessions}. Ask for the date of the sessions if it has not been provided. Use the ID of the Experience__c from {!@actions.Get_Experience_Details}. Do not use the experience name; an ID is required.
```

------------------------------------------------------------------------


---

## 20. Use the Resource Picker to Reference an Action

In the first instruction, replace the generic phrase:

``` text
appropriate action
```

with the actual action reference.

### Steps

1.  Place the cursor where the phrase **appropriate action** appears.
2.  Type `@` manually. Typing `@` opens the Resource Picker; pasting it
    does not trigger the picker.
3.  Select **Actions**.
4.  Select **Get Experience Details**.

**Why it matters:** Replacing generic wording with an exact action
reference tells the agent which action it should execute.

------------------------------------------------------------------------


---

## 21. Refine Instructions with AI Assistance

Agentforce Builder can help refine instructions.

You can:

-   Hover over an instruction and select the sparkle icon.
-   Use the `/` command to request suggested changes.

Suggested changes may show:

-   Removed content highlighted in red.
-   Added content highlighted in green.

You can accept or decline the proposed changes.

**Why it matters:** AI-assisted refinement can make instructions
clearer, but the user remains responsible for approving the final
wording.

------------------------------------------------------------------------


---

## 22. Switch to Script View

Use Script View to add a more specific booking instruction.

### Steps

1.  Open the **Experience Management** subagent.
2.  Click **Script View** in the upper-right corner. The toggle may
    appear as `</>`.
3.  In Script View, place the cursor at the end of the instructions.
4.  Press Enter to create a blank line.
5.  If you need to find the relevant instruction, use:
    -   Windows: `Ctrl + F`
    -   Mac: `Command + F`

Search for:

``` text
if asked to get
```

------------------------------------------------------------------------


---

## 23. Add the Booking Instruction

Add the following instruction:

``` text
4. If asked to book, use the appropriate action. The Contact__c is the contact ID from {!@actions.Get_Customer_Details}. The Session__c is the ID of the session from the action {!@actions.Get_Sessions}. If multiple sessions are present, ask the customer to select one of the sessions and use that Session as the ID for Session__c. Prompt for the Number of Guests and use that for Number_of_Guests__c.
```

In the first sentence, replace:

``` text
appropriate action
```

with:

``` text
{!@actions.Create_Experience_Session_Booking}
```

### Final booking logic

When creating a booking:

-   **Contact ID:** Obtain it from `Get_Customer_Details`.
-   **Session ID:** Obtain it from `Get_Sessions`.
-   **Multiple sessions:** Ask the customer to select the intended
    session.
-   **Number of guests:** Ask the customer and use the provided count.
-   **Booking action:** Run `Create_Experience_Session_Booking`.

**Why it matters:** The booking must be associated with the correct
customer and session, and it must store the requested guest count.

------------------------------------------------------------------------


---

## 24. Save, Commit, and Activate

1.  Click **Save**.
2.  Return to Canvas View.
3.  Click **Commit**.
4.  Confirm by clicking **Commit** in the confirmation window.
5.  Click **Activate**.
6.  Confirm activation when prompted.

### What these steps mean

-   **Save:** Saves the current edits.
-   **Commit:** Commits the agent's configuration changes.
-   **Activate:** Enables the agent in its active state.

### Troubleshooting

If a commit error occurs:

1.  Open Agentforce Assistant.

2.  Enter:

    ``` text
    scan and fix
    ```

3.  Follow the prompts.

4.  Click **Accept All** if appropriate.

5.  Try committing again.

If activation fails with **We can't activate your agent**, review the
Salesforce help article titled **Agentforce: Unable to Activate
Agentforce Agent**.

------------------------------------------------------------------------


---

## 25. Preview and Test the Agent

Preview lets you test the agent while building it. You can inspect the
plan, executed actions, interaction details, and the conversation.

### Steps

1.  Click **Preview**.

2.  Refresh if necessary.

3.  Enter this test prompt:

    ``` text
    Can you let me know more about the full moon beach party experience?
    ```

4.  If the agent asks for an email and membership number, enter:

    ``` text
    I am sofiarodriguez@example.com and my membership number is 10008155.
    ```

5.  Then ask the agent to book a session.

------------------------------------------------------------------------


---

## 26. Live Test Mode vs. Simulate

### Live Test Mode

The agent can access and modify actual org data.

**Important:** Real data changes may occur.

### Simulate

Tests the agent in a simulated context.

**Why it matters:** Choose the testing mode based on the purpose of the
test. Live Test Mode can demonstrate behavior against org data, while
Simulate can reduce the risk of affecting live data.

------------------------------------------------------------------------


---

## 27. Publish the Embedded Service Deployment

The web deployment must be published so that it uses the latest agent
configuration.

### Steps

1.  Open **Setup**.
2.  In Quick Find, search for **Embedded Service Deployments**.
3.  Open **ESA Web Deployment**.
4.  Click **Publish**.

Publishing may take up to 10 minutes. You can continue with the next
setup step while it completes.

**Why it matters:** Publishing makes the latest configuration available
through the web deployment.

------------------------------------------------------------------------


---

## 28. Update the Route to ESA Flow

Update the existing flow so incoming work is routed to the new service
agent.

### Steps

1.  In Setup Quick Find, search for **Flows**.

2.  Open the **Route to ESA** flow.

3.  Select the **Route to ESA** component.

4.  Update the input values:

    **Route To**

    ``` text
    Agentforce Service Agent
    ```

    **Agentforce Service Agent**

    ``` text
    CC Service Agent
    ```

5.  Click **Save As New Version**.

6.  Leave the other settings unchanged.

7.  Click **Save**.

8.  Click **Activate**.

9.  Use the back arrow to return to Setup.

**Why it matters:** The flow must route incoming work to the newly
created **CC Service Agent**.

### Troubleshooting

If **CC Service Agent** does not appear as an option:

-   Open CC Service Agent in Agentforce Builder.
-   Confirm that the agent is activated.

------------------------------------------------------------------------


---

## 29. Add Agentforce to the Coral Cloud Site

Add the embedded messaging component to the Experience Cloud site.

### Steps

1.  Open **Setup**.

2.  In Quick Find, search for **All Sites**.

3.  Click **Builder** next to Coral Cloud.

4.  Open the **Components** panel.

5.  Search for:

    ``` text
    Embedded Messaging
    ```

6.  Drag the component into the **Book an Experience of a Lifetime**
    section.

7.  Leave the default settings unchanged.

8.  Click **Publish**.

9.  Confirm by clicking **Publish**.

10. Click **Got It**.

**Why it matters:** Embedded Messaging provides the customer-facing chat
interface through which customers can interact with the agent.

------------------------------------------------------------------------


---

## 30. Test the Agent as a Customer

This is the final customer-side test.

### Steps

1.  Open the **Experience Builder** menu.

2.  Select **View coral-cloud**.

3.  Allow a few minutes for the published site to become available if
    necessary.

4.  Click the messaging icon in the lower-right corner.

5.  Wait for the agent's greeting.

6.  Enter this test prompt:

    ``` text
    Can you let me know about the Underground Cave Exploration?
    ```

7.  If asked, provide:

    ``` text
    sofiarodriguez@example.com and my membership number is 10008155.
    ```

8.  Answer the agent's follow-up questions and try booking a session.

### Optional verification

Find the **Underground Cave Exploration** session record in Salesforce
and check the selected date or booking record.

The provided exercise indicates that the agent can create or update
records based on the information supplied by the customer.

### If the agent does not respond

Try republishing the Experience Cloud site. The initial publication of
the agent and site connection may take a few minutes to become
available.

------------------------------------------------------------------------


---

## 31. Important Terms: Quick Reference

  -----------------------------------------------------------------------
  Term                    Meaning                 Why it matters
  ----------------------- ----------------------- -----------------------
  **Agent**               An AI-powered service   Helps automate or
                          interface that          assist with
                          understands requests    customer-service tasks.
                          and can execute
                          suitable actions.

  **Service Agent**       An agent focused on     Handles customer
                          customer service.       questions, bookings,
                                                  and service tasks.

  **Subagent**            A specialized worker    Separates complex work
                          within the main agent.  into focused
                                                  responsibilities.

  **Agent Router**        Uses user input and     Routes requests to the
                          conversation history to relevant specialist.
                          choose a subagent.

  **Action**              A tool an agent can use Enables data retrieval
                          to perform work.        and operations.

  **Custom Action**       An action created for a Supports specific
                          particular requirement, business operations.
                          such as Get Experience
                          Details.

  **Flow**                Salesforce automation   Carries out the
                          that can perform a      operation referenced by
                          process or backend      an action.
                          operation.

  **Asset Library**       A source of reusable    Helps avoid duplicating
                          assets and actions.     existing work.

  **Reasoning             Instructions that guide Makes the workflow more
  Instructions**          a subagent's use of     explicit and
                          actions and results.    consistent.

  **Variables**           Values that the agent   Supports logic and
                          can store and           decisions.
                          reference.

  **Connections**         Configuration that      Enables interaction
                          connects the agent to   through channels such
                          user-facing channels.   as Messaging, Slack, or
                                                  Voice.

  **Data**                Knowledge sources       Helps the agent answer
                          available for           using relevant
                          retrieval.              available information.

  **Canvas View**         A natural-language      Makes common
                          agent editor.           configuration changes
                                                  accessible.

  **Script View**         An environment for      Enables precise
                          direct Agent Script     scripting and
                          editing.                references.

  **Agentforce            An AI helper that       Supports conversational
  Assistant**             suggests                editing.
                          agent-configuration
                          changes.

  **Resource Picker**     A picker opened by      Helps insert exact
                          typing `@` to select    references into
                          resources such as       instructions.
                          actions.

  **Contact ID**          The identifier of a     Associates a booking
                          customer's Contact      with the correct
                          record.                 customer.

  **Experience ID**       The identifier of an    Uniquely identifies the
                          `Experience__c` record. experience used to
                                                  retrieve sessions.

  **Session ID**          The identifier of a     Associates a booking
                          particular experience   with the correct
                          session.                session.

  **Number of Guests**    The number of guests    Stores the requested
                          included in a booking.  guest count.

  **Require Input to      A setting that requires Helps prevent execution
  Execute Action**        an input before an      without required
                          action can run.         information.

  **Show in               A setting that makes an Lets the agent use or
  Conversation**          action output available display the action
                          in the conversation.    result.

  **Commit**              Commits the agent's     Prepares the
                          configuration changes.  configuration for
                                                  activation.

  **Activate**            Enables the agent in    Makes the agent
                          its active state.       available for the
                                                  relevant testing and
                                                  deployment flow.

  **Preview**             An interface for        Helps verify responses
                          testing the agent       and action execution
                          during development.     before customer use.

  **Live Test Mode**      A testing mode that can Tests behavior against
                          access or modify actual org data, but can cause
                          org data.               real changes.

  **Simulate**            A simulated testing     Helps test without the
                          context.                same impact on live
                                                  data.

  **Embedded Service      A web deployment        Helps expose the agent
  Deployment**            configuration for       through the web
                          making a service        experience.
                          experience available.

  **Route to ESA Flow**   A flow that routes      Sends work to the
                          incoming work to an     intended agent.
                          Agentforce service
                          agent.

  **Experience Cloud**    Salesforce              Hosts the Coral Cloud
                          functionality for       customer experience.
                          customer-facing sites
                          and digital
                          experiences.

  **Embedded Messaging**  The customer-facing     Lets customers start a
                          chat interface added to conversation with the
                          the site.               agent.
  -----------------------------------------------------------------------

------------------------------------------------------------------------


---

## 32. End-to-End Architecture

``` text
Customer
   |
   v
Coral Cloud Experience Cloud Site
   |
   v
Embedded Messaging
   |
   v
Agentforce Service Agent
(CC Service Agent)
   |
   v
Agent Router
   |
   v
Experience Management Subagent
   |
   +-------------------------+-------------------------+
   |                         |                         |
   v                         v                         v
Get Customer Details   Get Experience Details     Get Sessions
   |                         |                         |
   +-------------------------+-------------------------+
                             |
                             v
              Create Experience Session Booking
                             |
                             v
                    Salesforce Record
```

------------------------------------------------------------------------


---

## 33. Complete Action Flows

### Scenario A: A customer asks about an experience

``` text
Customer asks for experience information
                  |
                  v
        Check customer identity
                  |
                  v
     Ask for email and membership number
                  |
                  v
         Get Customer Details
                  |
                  v
       Identify the Contact record
                  |
                  v
        Get Experience Details
                  |
                  v
      Retrieve experience information
                  |
                  v
       Summarize the results for the customer
```

### Scenario B: A customer asks for sessions

``` text
Customer asks for available sessions
                  |
                  v
        Check customer identity
                  |
                  v
        Get Experience Details
                  |
                  v
       Obtain the Experience__c ID
                  |
                  v
          Ask for a date if missing
                  |
                  v
             Get Sessions
                  |
                  v
           Return session results
```

**Important:** `Get Sessions` requires the \*\*Experience\_\_c ID\*\*,
not the experience name.

### Scenario C: A customer books a session

``` text
Customer wants to book a session
                  |
                  v
         Get Customer Details
                  |
                  v
             Obtain Contact ID
                  |
                  v
              Get Sessions
                  |
                  v
              Obtain Session ID
                  |
                  v
      If there are multiple sessions,
          ask the customer to choose
                  |
                  v
        Ask for the number of guests
                  |
                  v
   Create Experience Session Booking
                  |
                  v
           Booking record created
```

------------------------------------------------------------------------


---

## 34. Important IDs and Data Mapping

  Information         Source                   Used for
  ------------------- ------------------------ ---------------------------------
  Customer email      Customer                 Input to Get Customer Details
  Membership number   Customer                 Input to Get Customer Details
  Contact ID          Get Customer Details     `Contact__c`
  Experience name     Customer request         Input to Get Experience Details
  Experience ID       Get Experience Details   Input to Get Sessions
  Session ID          Get Sessions             `Session__c`
  Number of guests    Customer                 `Number_of_Guests__c`

------------------------------------------------------------------------


---

## 35. Badge Completion Checklist

Use this checklist while completing the Trailhead exercises.

### Environment and setup

-   [ ] Create a custom Playground.
-   [ ] Launch the Playground.
-   [ ] Enable Agentforce Studio.
-   [ ] Publish the Coral Cloud Experience Cloud site.
-   [ ] Open Agentforce Studio.

### Agent and subagent

-   [ ] Create `CC Service Agent`.
-   [ ] Assign `EinsteinServiceAgent User`.
-   [ ] Create the `Experience Management` subagent.

### Actions and reasoning

-   [ ] Add `Get Experience Details`.
-   [ ] Configure its Flow reference.
-   [ ] Require `experienceName`.
-   [ ] Show `experienceRecord` in the conversation.
-   [ ] Add `Get Customer Details`.
-   [ ] Require `email`.
-   [ ] Require `memberNumber`.
-   [ ] Show the contact output.
-   [ ] Add `Create Experience Session Booking`.
-   [ ] Add `Get Sessions`.
-   [ ] Add reasoning instructions.
-   [ ] Replace the generic action wording with the
    `Get Experience Details` reference.
-   [ ] Switch to Script View.
-   [ ] Add the booking instruction.
-   [ ] Reference `Create Experience Session Booking`.

### Save and test

-   [ ] Save.
-   [ ] Commit.
-   [ ] Activate.
-   [ ] Preview the agent.
-   [ ] Test experience information retrieval.
-   [ ] Test customer validation.
-   [ ] Test session retrieval.
-   [ ] Test booking.

### Deployment

-   [ ] Republish the ESA Web Deployment.
-   [ ] Update the `Route to ESA` flow.
-   [ ] Set **Route To** to `Agentforce Service Agent`.
-   [ ] Set **Agentforce Service Agent** to `CC Service Agent`.
-   [ ] Save as a new flow version.
-   [ ] Activate the flow.
-   [ ] Add Embedded Messaging to the Coral Cloud site.
-   [ ] Publish the site.
-   [ ] Open the Coral Cloud site.
-   [ ] Open Messaging.
-   [ ] Test the customer conversation.
-   [ ] Optionally verify the booking or session record.

------------------------------------------------------------------------


---

## 36. Final Summary: What You Learned

### 1. Agentforce Builder

Agentforce Builder provides a place to create, configure, customize,
test, and activate a service agent.

### 2. Agent and subagent

The main service agent handles customer interaction, while specialized
subagents handle focused work.

In this badge:

**CC Service Agent → Experience Management**

### 3. Actions

Actions are the tools the agent uses. In this example:

-   Get Experience Details
-   Get Customer Details
-   Get Sessions
-   Create Experience Session Booking

### 4. Flows

Custom actions can reference Salesforce flows. A flow performs the
underlying operation or automation.

### 5. Reasoning instructions

The agent needs more than a list of available tools. Its instructions
should explain how to:

-   Identify the customer.
-   Collect required information.
-   Run the correct action.
-   Use the correct IDs.
-   Ask the customer to choose when multiple sessions are available.
-   Collect the guest count.
-   Execute the booking action.

### 6. IDs matter

The booking must use the correct record identifiers:

``` text
Contact__c = Contact ID
Session__c = Session ID
Number_of_Guests__c = Customer-provided guest count
```

In particular, \*\*Get Sessions uses the Experience\_\_c ID, not the
experience name\*\*.

### 7. Preview and testing

Before relying on the agent, inspect its conversation, plan, action
execution, and interaction details in Preview.

### 8. Live Test Mode vs. Simulate

-   **Live Test Mode:** Can access or modify actual org data.
-   **Simulate:** Uses a simulated testing context.

Choose the mode with awareness of whether real data may be affected.

### 9. Deployment

Creating an agent is not the final step. The deployment process
includes:

``` text
Create and configure agent
          |
          v
        Activate
          |
          v
Publish Embedded Service Deployment
          |
          v
Update Route to ESA Flow
          |
          v
       Activate Flow
          |
          v
Configure Experience Cloud
          |
          v
Add Embedded Messaging
          |
          v
      Publish Site
          |
          v
  Customer uses the agent
```

------------------------------------------------------------------------


---

## 37. One-Minute Revision

Remember this sequence:

``` text
Agentforce Builder
        |
        v
Create Service Agent: CC Service Agent
        |
        v
Create Experience Management Subagent
        |
        v
Add Actions:
- Get Experience Details
- Get Customer Details
- Get Sessions
- Create Experience Session Booking
        |
        v
Add Reasoning Instructions
        |
        v
Use the correct record IDs
        |
        v
Save → Commit → Activate
        |
        v
Preview and test
        |
        v
Publish Embedded Service Deployment
        |
        v
Update Route to ESA Flow
        |
        v
Route work to CC Service Agent
        |
        v
Add Embedded Messaging
        |
        v
Publish Experience Cloud Site
        |
        v
Customer interacts with the agent
        |
        v
Experience information and bookings are handled
```

------------------------------------------------------------------------


---

## 38. Key Takeaways

-   **Agentforce Builder** is used to create, configure, test, and
    activate an agent.
-   **Subagent** means a specialized responsibility within the agent.
-   **Action** is a tool that performs work.
-   **Flow** is Salesforce automation used by an action to carry out an
    operation.
-   **Reasoning Instructions** tell the agent which actions to use and
    how to use them.
-   **Preview** helps test the agent during development.
-   **Activate** enables the agent in its active state.
-   **Embedded Messaging** provides the customer-facing chat interface.
-   **Experience Cloud** hosts the customer-facing site.
-   **Route to ESA Flow** routes incoming work to the intended
    Agentforce service agent.

------------------------------------------------------------------------


---

## 39. Final Mental Model

``` text
CUSTOMER
   |
   | “Tell me about an experience”
   v
SERVICE AGENT
   |
   v
AGENT ROUTER
   |
   v
EXPERIENCE MANAGEMENT SUBAGENT
   |
   v
Check customer identity
   |
   v
Get Customer Details
   |
   v
Get Experience Details
   |
   v
Get Sessions
   |
   v
Customer selects a session
   |
   v
Customer provides guest count
   |
   v
Create Experience Session Booking
   |
   v
SALESFORCE RECORD
```

**Core idea:** Agentforce Builder is not just for creating a chatbot
that gives conversational answers. It can be used to build a service
workflow that combines a main agent, specialized subagents, actions,
Salesforce flows, reasoning instructions, customer validation, testing,
deployment, and Experience Cloud messaging.
