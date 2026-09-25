# Working with Modular Development and Unlocked Packages — Part 2
## Salesforce DX • Multiple Packages • Metadata Dependencies • `.forceignore` • Source Sync

> **Source:** Salesforce Developer Blog — *Working with Modular Development and Unlocked Packages: Part 2*  
> **Author:** Zayne Turner  
> **Original publication:** June 12, 2018
>
> **Context:** This is Part 2 of the modular-development/unlocked-packages series. Part 1 introduced packaging, org-based vs package-based development, dependency discovery, and extracting metadata. Part 2 goes deeper into **how to actually structure a Salesforce DX project for multiple packages/modules** and how to control metadata movement between a scratch org and local source.
>
> **Important:** The article is from 2018. Some `sfdx force:*` commands and exact CLI behavior are historical. The **architecture concepts remain useful**, while current CLI syntax should be checked against current Salesforce documentation before using commands in a modern project.

---

# 1. What This Part Is About

Part 2 focuses on:

- Creating a Salesforce DX project that supports multiple package directories
- Segmenting an application into packageable modules
- Organizing metadata using deployment dependencies
- Designing a base/object module
- Using scratch orgs to test module boundaries
- Understanding `actionOverrides`
- Separating object metadata from Lightning App Builder/FlexiPage dependencies
- Building and maintaining a `.forceignore`
- Understanding how `sfdx-project.json` and `.forceignore` influence source sync
- Using `force:source:pull`, `force:source:push`, and `force:source:status`
- Handling scratch-org source tracking problems
- Using aliases
- Deciding how granular modules should be
- Understanding when *not* to modularize
- Handling metadata/tooling limitations
- Moving from a rough base module toward more focused modules

The series itself progresses like this:

```text
Part 1
What is a package?
How do we start segmenting an org?
        |
        v
Part 2
How do we organize metadata into modules/packages?
How do we structure source control?
        |
        v
Part 3
What happens to App Builder workflows?
What happens when an unlocked package is installed in Production?
        |
        v
Part 4
Git branching + CI/CD + packaging
```

---

# 2. Part 2 Ka Core Idea

Part 2 ka main lesson:

> **Package boundary ko random metadata grouping ki tarah mat dekho. Deployment dependencies ko samajhkar logical layers/modules banao.**

Simple mental model:

```text
Business/Application
        |
        v
Metadata Dependencies
        |
        v
Layered Modules
        |
        v
Scratch Org Validation
        |
        v
Source Control
        |
        v
Packages
```

---

# 3. Packaging Construction Ko Deployment Se Start Karo

Author ne package construction ke baare mein sochte hue deployment se start kiya.

Reason:

> Packaging ka goal deployment ko easier, standardized aur repeatable banana hai.

Isliye question:

```text
My deployments fail kyun hote hain?
             |
             v
Deployment order ka kya pattern hai?
             |
             v
Kaunsi metadata dependency bottleneck create karti hai?
```

Author ko strong pattern mila:

> **Dependency management**

---

# 4. Deployment Failures → Dependency Management

Salesforce deployment mein common issue:

```text
Apex / Flow / LWC
       |
       v
Custom Object / Field required
       |
       v
Object/Field pehle deploy hona chahiye
```

Example:

```text
Custom Object
     |
     v
Custom Field
     |
     v
Apex Class
     |
     v
Flow
     |
     v
Lightning Component
```

Agar lower-level dependency available nahi hai:

```text
Deploy Apex
     |
     X
Field/Object missing
```

Therefore:

> **Deployment order itself can become the organizing principle for modules.**

---

# 5. Layered Package Architecture

Author ne dependency-management patterns ke basis par **layered packages** banaye.

Concept:

```text
             MOST DEPENDENT
                  ↑
                  |
          Application Features
                  |
          Space Management
                  |
             Base Styles
                  |
              Base Code
                  |
            Base Objects
                  |
                  ↓
             LEAST DEPENDENT
```

Dependency direction ko generally aise samjho:

```text
Higher Layer
     |
     | depends on
     v
Lower Layer
```

Example:

```text
es-space-mgmt
      |
      v
es-base-styles
      |
      v
es-base-code
      |
      v
es-base-objects
```

**Note:** Actual final module structure evolved during the author's work; modularization was iterative rather than predetermined.

---

# 6. Is This The Only Way?

**Absolutely not.**

Article explicitly says this is not the only way to segment an org.

But one thing is unavoidable:

> Whatever organizing principle you choose, you still need to deal with metadata dependencies that affect deployment.

Example:

```text
Code / Customization
        |
        v
Custom Object / Field
        |
        v
Must exist first
```

So:

```text
Organizing Principle
        +
Dependency Management
```

both matter.

---

# 7. Creating a Salesforce DX Project for Multiple Packages

Instead of modifying the old Salesforce DX project containing the unmanaged-package extract, the author created a **new empty repository/project**.

Historical command:

```bash
sfdx force:project:create -n easy-spaces -p es-base
```

Meaning conceptually:

```text
Project Name
    = easy-spaces

Initial Package Directory
    = es-base
```

---

# 8. Initial `sfdx-project.json`

The initial structure was:

```json
{
  "packageDirectories": [
    {
      "path": "es-base",
      "default": true
    }
  ],
  "namespace": "",
  "sfdcLoginUrl": "https://login.salesforce.com",
  "sourceApiVersion": "43.0"
}
```

### Important properties

#### `packageDirectories`

Defines directories that Salesforce CLI treats as package/source directories.

#### `path`

```json
"path": "es-base"
```

Points to the module directory.

#### `default`

```json
"default": true
```

Marks the default package directory.

This becomes particularly important during source synchronization.

#### `namespace`

The project had no namespace:

```json
"namespace": ""
```

#### `sfdcLoginUrl`

Historical login URL:

```json
"sfdcLoginUrl": "https://login.salesforce.com"
```

#### `sourceApiVersion`

Article used:

```json
"sourceApiVersion": "43.0"
```

This is **historical** and should not be copied blindly into a current project.

---

# 9. Scratch Org Definition

The author modified the default `project-scratch-def.json`.

Example from the article:

```json
{
  "orgName": "Easy Spaces",
  "edition": "Developer",
  "hasSampleData": "false",
  "features": "ServiceCloud;ServiceWave;SalesWave",
  "orgPreferences": {
    "enabled": [
      "S1DesktopEnabled",
      "IsLiveAgentEnabled"
    ],
    "disabled": [
      "S1EncryptedStoragePref2"
    ]
  }
}
```

---

# 10. `orgName`

Author deliberately gave scratch orgs meaningful names.

Instead of a generic/default organization name:

```text
zturner Company
```

use:

```text
Easy Spaces
```

Why?

As projects grow:

```bash
sfdx force:org:list
```

should produce understandable information.

Instead of:

```text
Unknown Org A
Unknown Org B
Unknown Org C
```

you want:

```text
Easy Spaces Dev
Customer Onboarding
Integration Test
```

Conceptually:

> **Naming is operational metadata.**

---

# 11. `features` and `orgPreferences`

The author enabled required features such as:

```text
ServiceCloud
ServiceWave
SalesWave
```

and configured org preferences.

The article also discusses enabling features needed for development, including Live Agent and Einstein Analytics in the broader explanation.

---

# 12. Why Configure Scratch Org Preferences?

The author wanted to avoid repeatedly changing Setup settings manually for every scratch org.

Example:

```text
Scratch Org Created
       |
       v
Open Setup
       |
       v
Change Cache Setting
       |
       v
Repeat for every org
```

Instead:

```text
Scratch Definition
       |
       v
Create Scratch Org
       |
       v
Required settings already configured
```

### Development lesson

> Automate environment setup wherever possible.

---

# 13. `.forceignore` — First Introduction

Before the initial project commit, the author added:

```text
.forceignore
```

Initially it was blank.

Purpose:

> **Control which metadata should be ignored when Salesforce CLI moves metadata between the org and local project.**

This becomes extremely important when one Salesforce DX project contains multiple modules.

---

# 14. Why `.forceignore` Matters More in Modular Projects

Suppose:

```text
Project
|
+-- es-base-objects
+-- es-base-code
+-- es-base-styles
+-- es-space-mgmt
```

A scratch org can contain all of these.

But while working on:

```text
es-base-objects
```

you may not want:

```text
Lightning Apps
FlexiPages
Content Assets
```

to suddenly appear inside that module.

Without control:

```text
Scratch Org
     |
     v
source:pull
     |
     +--> Objects
     +--> Apps
     +--> FlexiPages
     +--> Tabs
     +--> Other metadata
```

Now the module becomes polluted with unrelated metadata.

This is **cruft**.

---

# 15. Multiple Package Directories

As the author worked, the project evolved from one directory to multiple directories.

Later `sfdx-project.json`:

```json
{
  "packageDirectories": [
    {
      "path": "es-base-objects",
      "default": false
    },
    {
      "path": "es-base-styles",
      "default": false
    },
    {
      "path": "es-space-mgmt",
      "default": true
    }
  ],
  "namespace": "",
  "sfdcLoginUrl": "https://login.salesforce.com",
  "sourceApiVersion": "43.0"
}
```

---

# 16. Meaning of the Multiple Directories

Now Salesforce CLI knows about:

```text
es-base-objects
es-base-styles
es-space-mgmt
```

Each directory represents a logical module/package area.

Example:

```text
easy-spaces/
|
+-- es-base-objects/
|
+-- es-base-styles/
|
+-- es-space-mgmt/
|
+-- sfdx-project.json
|
+-- .forceignore
```

---

# 17. Default Package Directory

One directory is marked:

```json
"default": true
```

In this example:

```text
es-space-mgmt
```

is the default.

This becomes important when doing:

```bash
sfdx force:source:pull
```

because the CLI uses the default package directory as the first location for newly pulled metadata.

---

# 18. The Default Directory Is Contextual

A very important lesson:

> The default directory doesn't mean all project metadata permanently belongs there.

The author sometimes changed the `default` value depending on which module they were actively working on.

Example:

```text
Today:
default = es-base-objects

Tomorrow:
default = es-space-mgmt
```

This is a development workflow decision.

---

# 19. Making Sense of Package Modules

Before pulling apart metadata, the author did two things:

### Step 1

Deleted unneeded metadata that came from the unmanaged package extraction.

### Step 2

Looked for metadata that wasn't compatible with packaging.

The article also points to the Metadata Coverage Report as a way to understand metadata support.

---

# 20. Metadata Coverage

Before putting a metadata type into a package/module, ask:

```text
Is this metadata type supported?
Can it be deployed?
Can it be packaged?
What are its limitations?
```

This matters because not every Salesforce metadata type historically had the same deployment/package capabilities.

### Architect lesson

> **Metadata coverage is part of package design.**

Don't design a module boundary around metadata that cannot participate in the intended lifecycle.

---

# 21. Keep Source Even If It Isn't Part of the Initial Package

The author created a folder that was **not listed in `sfdx-project.json`**.

Purpose:

> Hold metadata that should remain in source control but was not related to the initial app modules.

This is a very useful distinction:

```text
Source Controlled
       !=
Package Directory
```

Something can be:

```text
Important source
```

without immediately being:

```text
Package module
```

---

# 22. Initial Package Sketch

The author organized modules based on dependency layers.

Conceptual diagram from the article:

```text
              Higher Dependency
                    ↑

      +---------------------------+
      | bots / analytics          |
      +---------------------------+
                    |
      +---------------------------+
      | space management          |
      +---------------------------+
                    |
      +---------------------------+
      | themes / styles           |
      +---------------------------+
                    |
      +---------------------------+
      | base                      |
      +---------------------------+

                    ↓
              Lower Dependency
```

The exact module names evolved later.

---

# 23. Start With One Module at a Time

The author did not attempt to perfect the whole architecture immediately.

Instead:

```text
Draft Base Module
       |
       v
Deploy
       |
       v
Observe
       |
       v
Modify
       |
       v
Deploy Again
       |
       v
Refine
```

This is an important development principle:

> **Modularization is iterative.**

---

# 24. Base Module — Object-Level Metadata

The first base module focused on object-level metadata.

It included things like:

- Custom Objects
- Page Layouts
- Custom List Views
- Relevant Fields
- Relevant standard fields
- Object-related metadata
- Tabs where appropriate

Why?

Because objects and fields were considered the first line of deployment dependencies.

---

# 25. Dependency Layer

Think:

```text
Object
  |
  +--> Field
  |
  +--> Layout
  |
  +--> List View
  |
  +--> Other metadata
```

Then higher layers:

```text
Object Layer
     |
     v
Code Layer
     |
     v
UI/Style Layer
     |
     v
Application Layer
```

The exact dependency direction varies by implementation, but the principle is to identify what must exist before something else can deploy.

---

# 26. Copy the Objects Folder

The author initially copied the entire:

```text
objects/
```

folder from the undifferentiated source project into:

```text
es-base/
```

Then also brought over:

```text
layouts/
tabs/
```

After that, unnecessary files were removed.

---

# 27. Why Delete Files After Copying?

Because metadata folders can contain multiple kinds of components.

Example:

```text
tabs/
```

might contain:

```text
Object tab
Lightning App tab
Other tab metadata
```

The author wanted the base module to contain only metadata relevant to the object layer.

---

# 28. How to Identify Metadata From XML

If the filename wasn't enough, the author looked at the XML.

Example:

Object-related tab:

```xml
<customObjects>
```

Lightning App page related metadata:

```xml
<flexiPage>
```

This is a useful practical technique:

> **When metadata filename semantics are unclear, inspect the metadata XML.**

---

# 29. First Scratch Org Test

After cleaning the module:

```text
Local Module
      |
      v
Scratch Org
      |
      v
Deploy / Push
      |
      v
Inspect Result
```

The author checked whether:

- Objects arrived
- Fields arrived
- Layouts arrived
- Tabs arrived

---

# 30. What Was Not Included?

More complex dependencies such as:

- Page Layout Assignments
- Field-Level Security (FLS)

were not part of the initial object-only module.

This is an important lesson:

> A technically related metadata component doesn't automatically belong in the same module.

---

# 31. Repeat the Cycle

The author repeatedly used:

```text
Add/Edit Metadata
       |
       v
Create/Use Scratch Org
       |
       v
Push Metadata
       |
       v
Inspect Result
       |
       v
Identify Missing/Unwanted Metadata
       |
       v
Refine Module
```

This is essentially an experimental architecture loop.

---

# 32. Why Scratch Orgs Are Powerful Here

Scratch orgs are useful because they allow you to test:

```text
"Can this module actually deploy by itself?"
```

without risking a production org.

A good module should be tested independently.

---

# 33. Pragmatics of Separation

Now comes one of the most important technical sections.

Separating objects into a truly independent module has side effects.

One major issue:

> **FlexiPage activation can create `actionOverrides` in object metadata.**

---

# 34. What Is `actionOverrides`?

When Lightning App Builder activates a record-based FlexiPage, Salesforce can add an `actionOverrides` entry to metadata.

Example:

```xml
<actionOverrides>
    <actionName>View</actionName>
    <comment>
        Action override created by Lightning App Builder during activation.
    </comment>
    <content>Contact_Record_Page</content>
    <formFactor>Large</formFactor>
    <skipRecordTypeSelect>false</skipRecordTypeSelect>
    <type>Flexipage</type>
    <pageOrSobjectType>Contact</pageOrSobjectType>
</actionOverrides>
```

---

# 35. Why This Breaks Object Modularity

Suppose:

```text
es-base-objects
```

contains:

```text
Contact
```

But Contact's metadata now contains:

```text
actionOverrides
      |
      v
Contact_Record_Page
```

Then:

```text
Contact Object
      |
      v
Contact_Record_Page FlexiPage
```

The object now depends on the FlexiPage.

Therefore:

```text
Object-only package
        X
```

is no longer truly independent.

---

# 36. Org-Wide Default FlexiPage

If a record-based FlexiPage is activated as the org-wide default, the `actionOverrides` tag is added into the object's:

```text
object-meta.xml
```

Therefore:

```text
Object
   |
   +--> actionOverride
           |
           +--> FlexiPage
```

---

# 37. App-Level FlexiPage

If the FlexiPage is activated at the application level, the corresponding reference can be added to the application's:

```text
app-meta.xml
```

Again, a dependency is created.

---

# 38. Deployment Consequence

If an object contains this `actionOverride`:

```text
Object
  |
  v
actionOverride
  |
  v
FlexiPage
```

you cannot treat the object as fully independent without handling that dependency.

The article's approach:

> Remove the `actionOverride` tags from the object metadata if the goal is to keep the object module independent.

---

# 39. Important: This Applies to Custom Action Overrides Too

The same pattern applies to other custom action overrides.

Example:

```xml
<actionOverrides>
    <actionName>NewContact</actionName>
    <content>newContactOverride</content>
    <formFactor>Large</formFactor>
    <skipRecordTypeSelect>false</skipRecordTypeSelect>
    <type>LightningComponent</type>
</actionOverrides>
```

This creates:

```text
Contact Object
      |
      v
NewContact Override
      |
      v
Lightning Component
```

So the object is now coupled to UI metadata.

---

# 40. Complete Contact XML Example From Article

The article gives a `CustomObject` example containing default action overrides plus a custom Lightning Component override:

```xml
<?xml version="1.0" encoding="UTF-8"?>
<CustomObject xmlns="http://soap.sforce.com/2006/04/metadata">
    <actionOverrides>
        <actionName>CancelEdit</actionName>
        <type>Default</type>
    </actionOverrides>

    <actionOverrides>
        <actionName>Clone</actionName>
        <type>Default</type>
    </actionOverrides>

    <actionOverrides>
        <actionName>Delete</actionName>
        <type>Default</type>
    </actionOverrides>

    <actionOverrides>
        <actionName>Edit</actionName>
        <type>Default</type>
    </actionOverrides>

    <actionOverrides>
        <actionName>Merge</actionName>
        <type>Default</type>
    </actionOverrides>

    <actionOverrides>
        <actionName>NewContact</actionName>
        <content>newContactOverride</content>
        <formFactor>Large</formFactor>
        <skipRecordTypeSelect>false</skipRecordTypeSelect>
        <type>LightningComponent</type>
    </actionOverrides>

    <actionOverrides>
        <actionName>View</actionName>
        <type>Default</type>
    </actionOverrides>

    <compactLayoutAssignment>SYSTEM</compactLayoutAssignment>
    <enableFeeds>true</enableFeeds>
    <enableHistory>false</enableHistory>

    <searchLayouts>
        <customTabListAdditionalFields>FULL_NAME</customTabListAdditionalFields>
        <customTabListAdditionalFields>ACCOUNT.NAME</customTabListAdditionalFields>
        <customTabListAdditionalFields>CONTACT.PHONE1</customTabListAdditionalFields>
        <lookupDialogsAdditionalFields>FULL_NAME</lookupDialogsAdditionalFields>
        <lookupDialogsAdditionalFields>ACCOUNT.NAME</lookupDialogsAdditionalFields>
        <lookupDialogsAdditionalFields>ACCOUNT.SITE</lookupDialogsAdditionalFields>
        <lookupPhoneDialogsAdditionalFields>FULL_NAME</lookupPhoneDialogsAdditionalFields>
        <lookupPhoneDialogsAdditionalFields>ACCOUNT.NAME</lookupPhoneDialogsAdditionalFields>
        <lookupPhoneDialogsAdditionalFields>CONTACT.PHONE1</lookupPhoneDialogsAdditionalFields>
        <searchResultsAdditionalFields>FULL_NAME</searchResultsAdditionalFields>
        <searchResultsAdditionalFields>ACCOUNT.NAME</searchResultsAdditionalFields>
        <searchResultsAdditionalFields>ACCOUNT.SITE</searchResultsAdditionalFields>
        <searchResultsAdditionalFields>CONTACT.PHONE1</searchResultsAdditionalFields>
        <searchResultsAdditionalFields>CONTACT.EMAIL</searchResultsAdditionalFields>
    </searchLayouts>

    <sharingModel>ControlledByParent</sharingModel>
</CustomObject>
```

For a base object-only module, the article says the custom `NewContact` override would need to be removed.

---

# 41. Standard Object — Minimal XML Example

If the base module does not need custom search layouts for a standard object, the article shows that the XML could be reduced to:

```xml
<?xml version="1.0" encoding="UTF-8"?>
<CustomObject xmlns="http://soap.sforce.com/2006/04/metadata"/>
```

The idea is not "always make XML this small."

The idea is:

> Keep the module's metadata focused on what the module actually owns.

---

# 42. Custom Object Base Metadata Example

The article also provides a custom object example called:

```text
My Custom Object
```

with standard/default action overrides and object settings.

Representative structure:

```xml
<CustomObject xmlns="http://soap.sforce.com/2006/04/metadata">

    <actionOverrides>
        <actionName>Accept</actionName>
        <type>Default</type>
    </actionOverrides>

    <actionOverrides>
        <actionName>CancelEdit</actionName>
        <type>Default</type>
    </actionOverrides>

    <actionOverrides>
        <actionName>Clone</actionName>
        <type>Default</type>
    </actionOverrides>

    <actionOverrides>
        <actionName>Delete</actionName>
        <type>Default</type>
    </actionOverrides>

    <actionOverrides>
        <actionName>Edit</actionName>
        <type>Default</type>
    </actionOverrides>

    <actionOverrides>
        <actionName>List</actionName>
        <type>Default</type>
    </actionOverrides>

    <actionOverrides>
        <actionName>New</actionName>
        <type>Default</type>
    </actionOverrides>

    <actionOverrides>
        <actionName>SaveEdit</actionName>
        <type>Default</type>
    </actionOverrides>

    <actionOverrides>
        <actionName>Tab</actionName>
        <type>Default</type>
    </actionOverrides>

    <actionOverrides>
        <actionName>View</actionName>
        <type>Default</type>
    </actionOverrides>

    <allowInChatterGroups>false</allowInChatterGroups>
    <compactLayoutAssignment>SYSTEM</compactLayoutAssignment>
    <deploymentStatus>Deployed</deploymentStatus>
    <enableActivities>false</enableActivities>
    <enableBulkApi>true</enableBulkApi>
    <enableChangeDataCapture>false</enableChangeDataCapture>
    <enableFeeds>false</enableFeeds>
    <enableHistory>false</enableHistory>
    <enableReports>true</enableReports>
    <enableSearch>true</enableSearch>
    <enableSharing>true</enableSharing>
    <enableStreamingApi>true</enableStreamingApi>

    <label>Show</label>

    <nameField>
        <label>My Custom Object Name</label>
        <type>Text</type>
    </nameField>

    <pluralLabel>My Custom Objects</pluralLabel>
    <searchLayouts/>
    <sharingModel>ControlledByParent</sharingModel>
    <visibility>Public</visibility>

</CustomObject>
```

---

# 43. What Goes Into Later Modules?

Once the base object module contains the foundational object metadata, later modules can contain the functionality that depends on it.

Example:

```text
Base Object Module
      |
      +--> Objects
      +--> Fields
      +--> Basic Layouts
      |
      v
Later Module
      |
      +--> Lightning Components
      +--> Visualforce
      +--> Apex
      +--> Overrides
```

The article notes that overrides may need to be manually activated/assigned after deployment.

---

# 44. Protect Your Base Metadata With `.forceignore`

Once you have carefully constructed:

```text
object-meta.xml
```

you don't want a later source pull to reintroduce metadata you deliberately removed.

Therefore:

```text
Carefully edited metadata
        |
        v
Update .forceignore
        |
        v
Prevent unwanted metadata from coming back
```

---

# 45. Understanding `.forceignore`

The `.forceignore` file is one of the most important operational pieces in this Part.

It:

- must live in the **root directory** of the project,
- applies to the project,
- controls what Salesforce CLI tries to fetch/deploy between org and local project,
- can become complex in a multi-module project.

---

# 46. Only One `.forceignore`

Because `.forceignore` must be at the project root:

```text
Project
|
+-- .forceignore   <-- ONE
|
+-- es-base-objects
+-- es-base-styles
+-- es-space-mgmt
```

You don't create:

```text
es-base-objects/.forceignore
es-base-styles/.forceignore
```

as independent project-level ignore files for the same Salesforce DX project.

---

# 47. Initial `.forceignore`

The article's early version:

```text
**profiles
package.xml

#es-base
es-base/main/default/applications
```

---

# 48. Meaning of the Initial Rules

### `**profiles`

Ignore profile metadata throughout the project.

### `package.xml`

Ignore `package.xml`.

### `es-base/main/default/applications`

For the `es-base` module, don't interact with application metadata.

This helped isolate the object/base module from application metadata.

---

# 49. Wildcards

The article explains that `.forceignore` supports wildcard patterns such as:

```text
**
```

The important point:

> Wildcards can apply across the project.

You cannot use them as though each module were a completely independent `.forceignore` scope.

---

# 50. Project-Wide vs Module-Specific Rules

A useful convention:

```text
# Project-wide rules
**profiles
**applications
**flexiPages

# Module-specific rules
#es-base-objects
es-base-objects/main/default/applications
```

This makes the file easier to understand.

---

# 51. Why `.forceignore` Can Become Long

When you split an org into many modules, you may need to tell Salesforce CLI:

```text
Module A:
ignore applications

Module B:
ignore applications + settings

Module C:
ignore objects + layouts + assets
```

Therefore:

```text
More modules
    |
    v
More source-boundary rules
    |
    v
Potentially longer .forceignore
```

The article says the repetition is worth it if it keeps modules clean and prevents accidental metadata movement/deletion.

---

# 52. Important Risk — Accidental Deletion

Suppose:

```text
Scratch Org
   |
   +--> Custom App
```

You pull it locally.

Then realize:

```text
This app doesn't belong in this module.
```

If you simply delete the local file and then push without understanding source tracking/ignore behavior, you may accidentally affect metadata in the development org.

Therefore:

> Understand `.forceignore` before deleting pulled metadata and pushing changes.

---

# 53. How to Discover What Belongs in `.forceignore`

The article recommends watching what appears after:

```bash
sfdx force:source:pull
```

Workflow:

```text
Make Change in Scratch Org
        |
        v
force:source:pull
        |
        v
Files appear locally
        |
        v
Inspect
        |
        +--> Wanted?
        |      |
        |      +--> Keep
        |
        +--> Unwanted?
               |
               v
        Add to .forceignore
               |
               v
        Delete local unwanted file
```

---

# 54. Image — Source Pull Workflow

The provided diagram shows:

```text
1. Make change in scratch org
             |
             v
        Scratch Org
             |
             |
2. sfdx force:source:pull
             |
             v
       Local Machine
             |
             v
3. Changes appear in folders
             |
             +--> 4. Note unwanted metadata names/paths
             |
             +--> 5. Update and save .forceignore
             |
             +--> 6. Delete unwanted local files
```

This is one of the most practical workflows in the article.

---

# 55. Example: Unwanted App Appears

Suppose you're working on:

```text
Data Model Module
```

You make a change in the scratch org.

After:

```bash
sfdx force:source:pull
```

you suddenly see:

```text
applications/
MySalesApp.app-meta.xml
```

But the application does not belong to your data model module.

Correct approach:

```text
1. Note path
2. Add path/pattern to .forceignore
3. Save .forceignore
4. Delete unwanted local file
5. Continue development
```

---

# 56. Project-Wide Convention for `.forceignore`

The author developed a convention:

```text
Top
 |
 +--> Project-wide exclusions
 |
 +--> Module A
 |
 +--> Module B
 |
 +--> Module C
```

Example:

```text
**profiles
**lightningExperienceThemes
**applications
**flexiPages
**tabs
**globalValueSets
package.xml

#es-base-objects
...

#es-base-styles
...

#es-space-mgmt
...
```

This makes the file maintainable.

---

# 57. Final `.forceignore` Example From the Article

```text
**profiles
**lightningExperienceThemes
**applications
**flexiPages
**tabs
**globalValueSets
package.xml

#es-base-objects
es-base-objects/main/default/applications

#es-base-styles
es-base-styles/main/default/applications
es-base-styles/main/default/settings

#es-space-mgmt
es-space-mgmt/main/default/contentassets
es-space-mgmt/main/default/settings
es-space-mgmt/main/default/objects
es-space-mgmt/main/default/layouts
es-space-mgmt/main/default/globalValueSets
```

---

# 58. Why Some Metadata Appears at Both Levels

The author sometimes had:

```text
**applications
```

at project level and:

```text
es-base-styles/main/default/applications
```

at module level.

This was part of experimentation.

The author would sometimes:

```text
comment wildcard
        |
        v
test pull
        |
        v
observe result
        |
        v
restore/change rule
```

This allowed focused source pulls.

---

# 59. Working With One Metadata Type at a Time

Suppose the developer is currently focusing on:

```text
Aura folder
```

They may not want:

```text
Applications
FlexiPages
Tabs
```

moving between org and local project.

So temporarily ignoring those types allows focused work.

Example:

```text
Focus = Aura

Ignore:
Applications
FlexiPages
Tabs
```

Later:

```text
Focus = FlexiPages

Remove/comment relevant wildcard
```

---

# 60. `.forceignore` Is a Boundary Control Mechanism

Think of it like:

```text
Scratch Org
     |
     | source sync
     v
+----------------------+
| .forceignore         |
|                      |
| Allowed / Ignored    |
+----------------------+
     |
     v
Local Modules
```

It helps enforce the intended module boundary.

---

# 61. Source Synchronization

Now the article moves to an important question:

> How does Salesforce know where pulled metadata should go?

Answer:

You need to understand the relationship between:

```text
sfdx-project.json
```

and:

```text
force:source:* commands
```

---

# 62. `packageDirectories` + `default`

Suppose:

```json
"packageDirectories": [
  {
    "path": "es-base-objects",
    "default": false
  },
  {
    "path": "es-base-styles",
    "default": false
  },
  {
    "path": "es-space-mgmt",
    "default": true
  }
]
```

When you run:

```bash
sfdx force:source:pull
```

new metadata is first added to:

```text
es-space-mgmt
```

because it is the default directory.

---

# 63. Existing Metadata Behaves Differently

Once metadata has already been placed into a particular package directory, Salesforce CLI respects that organization.

Important concept:

```text
Default directory
      |
      v
Where NEW pulled metadata goes
```

It does not mean:

```text
All existing metadata moves to default directory
```

---

# 64. Default Directory Does Not Reorganize Existing Metadata

Suppose:

```text
Object A -> es-base-objects
```

Then you change:

```text
default = es-space-mgmt
```

Object A doesn't automatically move to:

```text
es-space-mgmt
```

The CLI respects the existing location.

---

# 65. What Can Block Push/Pull?

The article highlights two important things:

### 1. `.forceignore`

Can prevent metadata from moving between org and local source.

### 2. Removing a folder from `sfdx-project.json`

If the package directory is no longer declared, the CLI behavior changes and source synchronization can be affected.

---

# 66. Source Sync Mental Model

```text
sfdx-project.json
        |
        +--> Which package directories exist?
        |
        +--> Which one is default?
        |
        v
Source Sync

.forceignore
        |
        +--> What metadata should be ignored?
        |
        v
Source Sync
```

Both need to be understood together.

---

# 67. Image — Final Modular Structure

The first provided architecture diagram shows the final style of module separation.

Conceptually:

```text
                    More Dependencies
                           ↑
                           |
     +----------+----------+----------+----------+
     |          |                     |          |
   es-images   data               es-space-mgmt bots  resv-mgmt
     |          |                     |
     |          |                     |
     +----------+---------------------+
                |
          es-base-styles
                |
          es-base-code
                |
          es-base-objects
                |
                ↓
             Less Dependency
```

Dashed boxes in the image represent:

> **Metadata not fully modularized**

---

# 68. Image — Dependency Layer Interpretation

The image shows:

### `es-base-objects`

Contains:

- Objects
- Fields
- Page Layouts
- Object Tabs
- Custom Metadata
- Related Permission Set

### `es-base-code`

Contains:

- Apex services
- Shared service Lightning Components

### `es-base-styles`

Contains:

- Design tokens
- Shared UI components
- Related Lightning Events
- Custom Page Templates
- Shared Content Assets

### `es-space-mgmt`

Contains:

- Space Management App
- Tabs
- App Pages
- Code
- Related Permission Set

### `bots`

Contains:

- Einstein Bot metadata

### `resv-mgmt`

Contains:

- Einstein Analytics metadata

### `data`

Contains:

- Sample data

### `es-images`

Contains:

- Non-deployable theme images

---

# 69. Metadata Not Fully Modularized

The dashed areas in the image indicate metadata that wasn't completely modularized.

This is important.

Real-world modularization is not always:

```text
100% perfect
```

Instead:

```text
Highly modular
     +
Known exceptions
     +
Documented limitations
```

is often more realistic.

---

# 70. More Granular Modules Are Better — But With Context

The author eventually decided that:

```text
es-base
```

was too broad.

Instead:

```text
es-base
```

became:

```text
es-base-objects
es-base-code
es-base-styles
```

Why?

Because shared Apex classes and Lightning components were also common services and didn't necessarily belong inside an object-only base module.

---

# 71. Granularity Evolution

Initial:

```text
es-base
   |
   +--> Objects
   +--> Code
   +--> Styles
```

Later:

```text
es-base-objects
es-base-code
es-base-styles
```

This improved isolation.

---

# 72. But Don't Modularize Without a Reason

This is one of the strongest lessons from Part 2.

Do NOT think:

```text
More packages = Better architecture
```

Instead ask:

> Why should this metadata be separate?

---

# 73. Decision Question

For every metadata component:

```text
Can this go into a base module?
          |
          v
      Why / Why not?
          |
          v
Is dependency deeply intertwined?
          |
       +--+--+
       |     |
      YES    NO
       |     |
       v     v
Keep with   Consider
dependent   refactoring
module      into shared module
```

---

# 74. Deeply Intertwined Dependency

Suppose:

```text
Space Management Feature
       |
       +--> Custom Object
       +--> Apex
       +--> LWC
```

If the Apex/LWC has no meaningful existence without the Space Management feature:

```text
Keep Together
```

because the dependency is deeply intertwined.

---

# 75. Reusable Functionality

But suppose:

```text
Apex Service
```

is useful for:

```text
Space Management
+
Future App
+
Another Module
```

Then consider:

```text
es-base-code
```

as a shared module.

This is where modularization can reveal reusable architecture.

---

# 76. Refactoring Opportunity

Modularization is not just packaging work.

It can reveal:

```text
Hidden Shared Capability
```

Example:

```text
Original App
 |
 +--> UI Code
 +--> Shared Service
 +--> Feature-specific Code
```

After analysis:

```text
Shared Service
       |
       v
es-base-code
```

This improves reuse.

---

# 77. UI Layer Discovery

The author discovered that some metadata initially considered part of the application was actually a broader UI layer.

That included:

- Lightning bundles
- UI-focused code
- Shared styles

So:

```text
es-styles
```

could evolve into:

```text
es-base-styles
```

and become reusable by multiple application modules.

---

# 78. No Objectively Right Modularization

Very important:

> There may be no objectively "right" modularization method.

Because every org has different:

- Teams
- Skills
- Ownership
- Deployment practices
- Business capabilities
- Legacy constraints
- Metadata dependencies
- Release processes

---

# 79. Example — Permission Set Split

The author chose to split a Permission Set into two to align it with module boundaries.

Benefit:

```text
Object-level permissions
       |
       v
Reusable later
```

Cost:

```text
More Permission Sets
       |
       v
More developer workflow complexity
```

This was an explicit trade-off.

---

# 80. Alternative Decision

The team could instead keep:

```text
One Complex Permission Set
```

inside the related application module.

Then another app could have:

```text
Its Own Permission Set
```

This would make the developer workflow simpler.

---

# 81. Architecture = Trade-Off

The important lesson:

```text
More modularity
       |
       +--> More isolation
       +--> More reuse
       +--> More independent deployment
       |
       +--> More complexity
       +--> More coordination
       +--> More configuration
```

Therefore:

> Choose the level of modularity that makes sense for your team.

---

# 82. Be Ready for Workarounds

The article warns that while building modules you will discover:

- Metadata API coverage limitations
- Tooling oddities
- Deployment limitations
- Source synchronization issues
- Metadata types that behave differently

Therefore:

```text
Architecture
    |
    v
Tooling Reality
    |
    v
Workaround
```

must be part of the workflow.

---

# 83. Start Small Because of These Limitations

If you start with:

```text
Entire Enterprise Org
```

and hit a tooling limitation, debugging becomes difficult.

If you start with:

```text
Small Module
```

you can isolate the problem.

Therefore:

> **Start small → learn the tooling → establish conventions → scale.**

---

# 84. Source Sync — `force:source:status`

The article strongly recommends using:

```bash
sfdx force:source:status
```

often.

Especially after:

- changing things directly in the development environment,
- enabling a feature in Setup,
- modifying configuration,
- changing metadata manually.

---

# 85. Why Use `force:source:status` Frequently?

It helps you understand:

```text
What changed?
        |
        v
What metadata is now different?
        |
        v
What might be pulled?
        |
        v
Do I need to modify .forceignore?
```

It can also expose metadata types you didn't realize were involved.

---

# 86. Important Limitation of `source:status`

Do NOT treat:

```bash
sfdx force:source:status
```

as a perfect preview of:

```bash
sfdx force:source:push
```

or:

```bash
sfdx force:source:pull
```

The article explicitly warns:

> `source:status` does not act as a preview/sanity check for `.forceignore` behavior.

You still need to watch actual source movement.

---

# 87. `.sfdx` Directory

The article tells developers to understand the:

```text
.sfdx
```

folder.

It is located inside the main project directory.

Example:

```text
project/
|
+-- .sfdx/
|    |
|    +-- orgs/
|
+-- force-app/
+-- sfdx-project.json
+-- .forceignore
```

---

# 88. `.sfdx/orgs`

Inside:

```text
.sfdx/orgs
```

you can find configuration information associated with scratch orgs used by the project.

Names may look like generated scratch-org usernames, for example:

```text
test-xmgpv1tmshae@example.com
```

---

# 89. Be Careful With `.sfdx`

The article warns:

> Be careful modifying files inside `.sfdx`.

These files are intrinsic to Salesforce CLI behavior on the local machine.

The important operational point is:

```text
Know what the folder does
```

rather than casually editing files inside it.

---

# 90. Scratch Org Tracking Problems

The article describes cases where:

```text
source:push
```

or:

```text
source:status
```

may temporarily stop correctly tracking metadata.

This can happen after an error during a push.

Symptoms may include:

- Metadata appears skipped
- Source status looks incorrect
- Changes don't seem to be tracked correctly

---

# 91. Recovery — Delete Scratch Org Configuration

According to the article's historical workflow:

```text
Identify scratch org
       |
       v
Delete its org configuration file
       |
       v
Retry source commands
```

To identify orgs:

```bash
sfdx force:org:list
```

The article also notes that all org configuration files can be deleted if necessary; configuration will be recreated when the relevant scratch org is used again.

---

# 92. Moving Metadata to a New Folder Can Also Break Tracking

Example:

```text
Before:
es-base/objects/Account

After:
es-base-objects/objects/Account
```

You move the file locally.

Then:

```bash
sfdx force:source:push
```

may report errors referencing the old path.

Historical recovery:

```text
Delete scratch-org configuration
       |
       v
Retry
```

---

# 93. When in Doubt — Create a New Scratch Org

This is one of the best practical lessons.

Scratch orgs are:

- disposable,
- relatively quick to provision,
- designed for experimentation.

So:

```text
Something feels broken
       |
       v
New Scratch Org
       |
       v
Start from clean source
```

Sometimes this is faster and safer than fighting a corrupted/unclear local tracking state.

---

# 94. Scratch Org as an Architecture Experiment

Use scratch orgs to answer:

```text
Can this module deploy independently?
Can this dependency be removed?
Does this metadata come back during pull?
Does this .forceignore rule work?
Does this package boundary make sense?
```

This is more powerful than treating scratch orgs only as developer sandboxes.

---

# 95. Delete Old Scratch Orgs

The article recommends deleting old scratch orgs frequently.

Benefits:

- Avoid consuming scratch-org limits unnecessarily
- Keep environment list clean
- Reduce confusion
- Encourage source to remain the source of truth

---

# 96. Source as Source of Truth

A powerful principle:

```text
Source
  |
  v
Scratch Org
```

rather than:

```text
Scratch Org
  |
  v
Permanent Source of Truth
```

Scratch orgs should be disposable.

---

# 97. Org Aliases

The article recommends setting aliases for orgs.

Conceptually:

```text
scratch-org-1
     |
     v
space-dev
```

Instead of remembering generated usernames.

---

# 98. Why Aliases Matter

You may want to run commands against:

- current scratch org
- old scratch org
- sandbox
- another environment

Aliases make commands easier and reduce mistakes.

Example concept:

```bash
-u space-dev
-u integration-test
-u qa
```

rather than long generated usernames.

---

# 99. Aliases Also Help With Tracking

Meaningful aliases help answer:

```text
Which org was this experiment performed in?
Which scratch org is this?
Which environment should I open?
```

This becomes especially valuable when multiple modules and scratch orgs exist.

---

# 100. Final Module Structure

The author's final module organization evolved into:

```text
Less Dependencies
       ↓

es-base-objects
       |
es-base-code
       |
es-base-styles
       |
       +-----------------------+
       |                       |
es-space-management       Other Feature Modules
       |
       +--> bots
       +--> resv-mgmt
       +--> data
       +--> es-images

       ↑
More Dependencies
```

The exact visual ordering should be interpreted as a dependency-oriented architecture rather than a universal Salesforce package template.

---

# 101. Final Modules — Image Interpretation

The provided final architecture diagram shows:

### `es-base-objects`

```text
Objects
Fields
Page Layouts
Object Tabs
Custom Metadata
Related Permission Set
```

### `es-base-code`

```text
Apex Services
Shared Service Lightning Components
```

### `es-base-styles`

```text
Design Tokens
Shared UI Components
Related Lightning Events
Custom Page Templates
Shared Content Assets
```

### `es-space-mgmt`

```text
Space Management App
Tabs
App Pages
Code
Related Permission Set
```

### `bots`

```text
Einstein Bot Metadata
```

### `resv-mgmt`

```text
Einstein Analytics Metadata
```

### `data`

```text
Sample Data
```

### `es-images`

```text
Non-deployable Theme Images
```

---

# 102. More Granular, Focused Modules

The author says:

> More granular, focused modules are better.

But interpret this correctly:

```text
Granular
      ≠
Tiny for no reason
```

Good granularity means:

```text
Clear responsibility
Clear dependencies
Clear ownership
Useful lifecycle
```

---

# 103. `es-base` → `es-base-objects` + `es-base-code` + `es-base-styles`

Original:

```text
es-base
```

contained too many categories.

Final:

```text
es-base-objects
es-base-code
es-base-styles
```

Why?

Because:

```text
Objects
```

are foundational metadata.

```text
Shared Code
```

can be reusable services.

```text
Shared Styles
```

can be reusable UI infrastructure.

These have different dependency and reuse characteristics.

---

# 104. Don't Modularize Without a Reason

For every split:

```text
Why split?
```

If answer is only:

```text
Because smaller packages are always better
```

that's not enough.

Better:

```text
Why split?
 |
 +--> Independent deployment
 +--> Reuse
 +--> Ownership
 +--> Reduced coupling
 +--> Different lifecycle
 +--> Better dependency boundary
```

---

# 105. Deep Dependency Test

Suppose metadata A depends on metadata B.

Ask:

> "What functionality would A have without B?"

If answer:

```text
Almost none
```

then A may belong with B in the same module.

If answer:

```text
A still has useful independent functionality
```

then A may be a candidate for separation.

This is a powerful modularization heuristic.

---

# 106. Refactoring During Modularization

Modularization can expose opportunities to refactor.

Example:

```text
Application
 |
 +--> Shared Service
 |
 +--> Feature Service
 |
 +--> Feature UI
```

If Shared Service is useful elsewhere:

```text
Application
    |
    +--> Feature-specific module
    |
    +--> Shared Service module
```

This is how modularization can improve architecture rather than merely reorganize folders.

---

# 107. Permission Set Trade-Off — Detailed

Suppose one application has:

```text
Permission Set A
```

and it contains:

```text
Object permissions
Feature permissions
UI permissions
```

To align with modules, you could split:

```text
Object Permission Set
+
Feature Permission Set
```

### Benefit

Reusable object-level permission model.

### Cost

More artifacts:

```text
More setup
More assignments
More developer complexity
```

No universal answer.

---

# 108. Team Working Style Matters

A technically elegant architecture can still be operationally bad if the team finds it too difficult to work with.

Consider:

```text
Architecture Quality
       =
Technical Design
+
Developer Experience
+
Operational Simplicity
+
Release Manageability
```

---

# 109. Metadata Coverage Constraints

Some metadata types may:

- not deploy,
- not package,
- require special handling,
- behave differently between environments,
- need manual activation.

Therefore:

```text
Desired Module
      |
      v
Metadata Coverage Check
      |
      +--> Supported?
      |       |
      |       +--> Include
      |
      +--> Unsupported?
              |
              v
        Alternative workflow
```

---

# 110. Practical Modularization Workflow

Use this workflow for a real Salesforce project:

```text
1. Identify business capability
          |
          v
2. Identify metadata involved
          |
          v
3. Map dependencies
          |
          v
4. Identify foundational layer
          |
          v
5. Create package directory
          |
          v
6. Move/copy candidate metadata
          |
          v
7. Clean unrelated metadata
          |
          v
8. Configure .forceignore
          |
          v
9. Push to scratch org
          |
          v
10. Inspect results
          |
          v
11. Run source:status
          |
          v
12. Refine
          |
          v
13. Test clean scratch org
          |
          v
14. Commit to Git
```

---

# 111. Recommended Development Loop

```text
                    +----------------+
                    | Scratch Org    |
                    +----------------+
                           |
                    Make a change
                           |
                           v
                source:pull / status
                           |
                           v
                 Local module update
                           |
                           v
                  Inspect unwanted
                     metadata
                           |
                           v
                  Update .forceignore
                           |
                           v
                    Clean source
                           |
                           v
                      source:push
                           |
                           v
                   Validate module
                           |
                           +-----> Repeat
```

---

# 112. Architect Perspective — Dependency Layers

A useful architecture model:

```text
L5 — Application
     |
     v
L4 — Feature / Domain
     |
     v
L3 — Shared UI / Styles
     |
     v
L2 — Shared Services / Code
     |
     v
L1 — Data Model / Objects
```

But don't treat this as a mandatory Salesforce template.

The actual layers depend on:

- business capabilities,
- metadata relationships,
- team structure,
- release requirements.

---

# 113. Architect Perspective — Dependency Direction

Try to make dependencies intentional.

Example:

```text
Application
     |
     v
Feature
     |
     v
Shared Services
     |
     v
Data Model
```

Avoid uncontrolled:

```text
A --> B
B --> C
C --> D
D --> A
```

Circular dependency creates deployment and maintenance pain.

---

# 114. Architect Perspective — Cohesion

High cohesion means:

> Components in one module strongly belong together.

Example:

```text
Customer Onboarding
|
+-- Onboarding Flow
+-- Onboarding Apex
+-- Onboarding LWC
+-- Onboarding Permission
```

This is generally more meaningful than:

```text
All Apex
All Flows
All LWC
```

as separate packages solely by metadata type.

---

# 115. Architect Perspective — Coupling

Good module:

```text
High Cohesion
+
Manageable Coupling
```

Bad module:

```text
Low Cohesion
+
High Coupling
```

---

# 116. Architect Perspective — Deployment Order

A useful dependency graph:

```text
Objects
   |
   v
Shared Code
   |
   v
Shared UI
   |
   v
Application
   |
   v
Advanced Features
```

Then deployment can follow:

```text
Base
  ↓
Shared
  ↓
Feature
  ↓
Application
```

---

# 117. Architect Perspective — Package Boundary vs Deployment Boundary

A package boundary can be:

```text
Code organization
+
Ownership boundary
+
Deployment boundary
+
Versioning boundary
```

But it doesn't automatically provide all four.

You must intentionally design them.

---

# 118. Source Control Strategy

Once modules are defined:

```text
Git Repository
|
+-- es-base-objects
+-- es-base-code
+-- es-base-styles
+-- es-space-mgmt
+-- bots
+-- resv-mgmt
```

Then use:

```text
Feature Branch
      |
      v
Pull Request
      |
      v
Validation
      |
      v
Merge
      |
      v
Release
```

---

# 119. Modern CLI Note

The article uses historical commands such as:

```bash
sfdx force:source:pull
sfdx force:source:push
sfdx force:source:status
sfdx force:org:list
```

Modern Salesforce CLI has moved much of the command surface toward:

```bash
sf ...
```

Therefore, for a new 2026 project:

> Learn the **workflow and concepts** from the article, but verify the current CLI command equivalents before implementing.

---

# 120. `.forceignore` Design Best Practices

A maintainable `.forceignore` should be:

```text
Predictable
Readable
Documented
Grouped
Reviewed
```

Recommended organization:

```text
# ==========================================
# Global exclusions
# ==========================================

...

# ==========================================
# es-base-objects
# ==========================================

...

# ==========================================
# es-base-code
# ==========================================

...

# ==========================================
# es-base-styles
# ==========================================

...

# ==========================================
# es-space-mgmt
# ==========================================

...
```

---

# 121. `.forceignore` Review Checklist

Before committing:

```text
[ ] Is the rule global or module-specific?
[ ] Is the wildcard too broad?
[ ] Could it hide required metadata?
[ ] Could it cause accidental deletion?
[ ] Is the path correct?
[ ] Is the rule documented?
[ ] Does source:pull behave as expected?
[ ] Does source:push behave as expected?
```

---

# 122. Source Sync Troubleshooting Checklist

If source tracking looks wrong:

```text
1. Run org list
2. Run source status
3. Inspect .forceignore
4. Check packageDirectories
5. Check default directory
6. Check whether metadata was moved
7. Check scratch-org config
8. Retry
9. If necessary, create a fresh scratch org
```

---

# 123. Troubleshooting Matrix

| Problem | Possible area |
|---|---|
| Unwanted file appears after pull | `.forceignore` |
| New metadata lands in wrong module | `default: true` |
| Existing metadata doesn't move | Existing package directory assignment |
| Push references old path | Scratch-org source tracking |
| Metadata isn't tracked | Scratch-org configuration / source tracking |
| Module deploys only with another module | Hidden dependency |
| FlexiPage breaks object isolation | `actionOverrides` |
| Permission model doesn't align | Permission Set boundaries |
| Metadata cannot deploy | Metadata coverage/tooling limitation |

---

# 124. Error / Gotcha — Default Directory

Wrong assumption:

> "Changing `default: true` will move all metadata there."

No.

It mainly influences where **newly pulled metadata** is placed.

Existing metadata retains its package-directory organization.

---

# 125. Error / Gotcha — `.forceignore` Is Not a Preview Engine

Wrong assumption:

```text
source:status
=
exact preview of push/pull behavior
```

Not guaranteed.

You need to test actual:

```text
push
pull
```

behavior.

---

# 126. Error / Gotcha — Delete Local File Carelessly

Wrong approach:

```text
Unwanted file
   |
   v
Delete
   |
   v
Push
```

Better:

```text
Unwanted file
   |
   v
Understand source tracking
   |
   v
Add appropriate .forceignore rule
   |
   v
Delete local file
   |
   v
Validate
```

---

# 127. Error / Gotcha — One `.forceignore` Per Module

Wrong:

```text
module-a/.forceignore
module-b/.forceignore
```

for project-level source sync.

The article's model uses:

```text
project-root/.forceignore
```

and organizes rules within that single file.

---

# 128. Error / Gotcha — Metadata Dependency Hidden in XML

You may think:

```text
Object
```

is independent.

But XML can reveal:

```text
actionOverride -> FlexiPage
```

Therefore:

> Always inspect actual metadata representation when dependency behavior is unclear.

---

# 129. Error / Gotcha — Modularization Without Business Reason

Wrong:

```text
Every component = separate package
```

Better:

```text
Business capability
+
Deployment dependency
+
Ownership
+
Reuse
+
Lifecycle
```

---

# 130. Error / Gotcha — Treating Scratch Org as Permanent

Scratch orgs are disposable.

If the environment becomes confusing:

```text
Create fresh scratch org
```

rather than spending excessive time repairing a disposable environment.

---

# 131. Real-World Example — Large Salesforce Org

Suppose:

```text
Enterprise Org
|
+-- Sales
+-- Service
+-- Customer Portal
+-- Reservations
+-- Analytics
+-- Bots
+-- Shared UI
+-- Integrations
```

A possible dependency-aware architecture:

```text
es-base-objects
      |
      v
es-base-code
      |
      v
es-base-styles
      |
      +----------------+
      |                |
      v                v
Sales / Service   Reservation
      |
      +--> Analytics
      +--> Bots
```

Again, exact boundaries must come from real dependencies.

---

# 132. Real-World Example — Shared Apex

Suppose three applications use:

```text
CustomerService.cls
```

If it is tightly coupled to one application:

```text
Keep with application
```

If it is genuinely reusable:

```text
es-base-code
```

Then:

```text
App A ----\
App B -----+--> es-base-code
App C ----/
```

---

# 133. Real-World Example — Shared UI

Suppose multiple apps use:

```text
SharedHeader
SharedModal
DesignTokens
CustomTheme
```

Instead of:

```text
App A owns everything
```

you may identify:

```text
es-base-styles
```

as a shared UI layer.

---

# 134. Real-World Example — FlexiPage

Suppose:

```text
Account
```

has:

```text
Account_Record_Page
```

activated as the default.

Then:

```text
Account object
    |
    v
actionOverrides
    |
    v
Account_Record_Page
```

If you want Account in a foundational object package:

```text
Remove/separate the override
```

and activate the UI override later as part of the application/UI module.

---

# 135. Real-World Example — Permission Sets

Suppose:

```text
Base Objects
```

need reusable CRUD/FLS permissions.

You might separate:

```text
Object Permission Set
```

from:

```text
Feature Permission Set
```

But consider the operational cost of more permission sets.

---

# 136. Advanced Concept — Module Independence

A module is more independent when:

```text
Few external dependencies
+
Clear contract
+
Clear ownership
+
Independent testing
+
Independent deployment
```

A module is less independent when:

```text
Many cross-references
+
Circular dependencies
+
Shared mutable configuration
+
Shared permission artifacts
+
Hidden UI activation dependencies
```

---

# 137. Advanced Concept — Dependency Depth

Think about dependency depth:

```text
Application
   |
Feature
   |
UI
   |
Code
   |
Object
```

If a top module depends on many layers, it has a larger dependency footprint.

This affects:

- deployment order,
- testing,
- release management,
- troubleshooting.

---

# 138. Advanced Concept — Dependency Fan-Out

One foundational component may be referenced by many modules:

```text
es-base-code
   |
   +--> Sales
   +--> Service
   +--> Portal
   +--> Reservation
   +--> Analytics
```

High fan-out means:

> A change to the shared component may have a large blast radius.

Therefore, foundational modules require stronger:

- backward compatibility,
- testing,
- governance,
- code review.

---

# 139. Advanced Concept — Shared Modules Become Architecture Contracts

If many modules depend on:

```text
es-base-code
```

then that module becomes a shared contract.

Changes should be treated carefully:

```text
Shared Module
     |
     +--> Consumer A
     +--> Consumer B
     +--> Consumer C
```

A breaking change can affect all consumers.

---

# 140. Advanced Concept — Package Versioning

Once modules become packageable:

```text
es-base-code v1
       |
       v
Consumers
```

Then:

```text
es-base-code v2
```

must be evaluated against consumers.

Package versioning therefore becomes part of dependency management.

---

# 141. Advanced Concept — Deployment Order as a Graph

Instead of thinking only in folders:

```text
A
B
C
```

think:

```text
A --> B
A --> C
B --> D
C --> D
```

This is a directed dependency graph.

A valid deployment order follows dependency direction.

Example:

```text
D
↓
B / C
↓
A
```

depending on the direction used in your graph notation.

The key is consistency: define whether an arrow means "depends on" or "is depended upon."

---

# 142. Advanced Concept — Dependency Graph + CI/CD

CI/CD can validate:

```text
Changed Module
      |
      v
Dependency Graph
      |
      v
Affected Modules
      |
      v
Targeted Validation
```

This can reduce unnecessary testing while still protecting dependent modules.

---

# 143. Advanced Concept — Source of Truth

The overall goal is:

```text
Git / Source
      |
      v
Controlled Module
      |
      v
Scratch Org
      |
      v
Validation
```

Avoid:

```text
Scratch Org
      |
      +--> random manual changes
      |
      v
Unknown state
```

---

# 144. Advanced Concept — Environment Reproducibility

A clean modular project should make it possible to reproduce an environment:

```text
Source
+
Scratch Definition
+
Dependencies
+
Package Metadata
+
Configuration
```

→

```text
New Scratch Org
```

This is one of the strongest benefits of source-driven development.

---

# 145. Advanced Concept — Disposable Environment

If your source is good:

```text
Delete Scratch Org
       |
       v
Create New Scratch Org
       |
       v
Deploy/Push Source
       |
       v
Reproduce Environment
```

This gives confidence that source really represents the application.

---

# 146. Advanced Concept — Configuration vs Code

Not everything should be treated exactly like code.

Separate:

```text
Source
Metadata
Configuration
Data
Environment-specific values
Secrets
```

Especially:

```text
Sample data
```

and:

```text
Non-deployable images
```

may need different handling.

The final diagram explicitly shows:

```text
data
es-images
```

as special areas.

---

# 147. Advanced Concept — Non-Deployable Assets

`es-images` in the final architecture is marked as:

```text
non-deployable theme images
```

This is a good reminder:

> Not every project artifact belongs in the same deployment mechanism.

You may still keep such assets in source control, but deployment/package treatment can differ.

---

# 148. Advanced Concept — Sample Data

The diagram has:

```text
data
(sample data)
```

Sample data is not the same as metadata.

Think:

```text
Metadata
   |
   v
Defines structure/behavior

Data
   |
   v
Represents records
```

A modular deployment strategy needs a separate data strategy.

---

# 149. Advanced Concept — Deployment vs Activation

Some metadata can deploy successfully but still require:

```text
Activation
Assignment
Publishing
Configuration
```

Example from the article:

```text
Lightning action override
```

may be deployed separately and then manually activated/assigned.

So:

```text
Deployment Success
      !=
Feature Fully Activated
```

---

# 150. Advanced Concept — Module Ownership

A useful production model:

```text
es-base-objects
    |
    +--> Platform Team

es-base-code
    |
    +--> Shared Services Team

es-space-mgmt
    |
    +--> Space Management Team
```

Ownership makes:

- support,
- reviews,
- releases,
- change decisions

clearer.

---

# 151. Advanced Concept — Module Size

Don't optimize for:

```text
Smallest possible module
```

Optimize for:

```text
Small enough to understand
+
Large enough to be cohesive
```

This is the sweet spot.

---

# 152. Advanced Concept — Change Frequency

Another useful criterion:

```text
Module A
changes weekly

Module B
changes quarterly
```

If they are tightly bundled:

```text
A + B
```

you may release B unnecessarily often.

A modular boundary can reduce this coupling if the dependency structure permits it.

---

# 153. Advanced Concept — Team Boundary

If:

```text
Team A
```

and:

```text
Team B
```

work independently, module boundaries may help.

But don't split solely based on team ownership if the technical dependency is extremely tight.

Use:

```text
Technical Boundary
+
Business Boundary
+
Team Boundary
```

together.

---

# 154. Advanced Concept — Blast Radius

A foundational module:

```text
es-base-objects
```

can have high downstream impact.

A change to:

```text
Customer__c
```

may affect:

```text
Apex
Flows
LWC
Reports
Integrations
Permissions
```

Therefore foundational modules need stronger regression testing.

---

# 155. Development Checklist — New Module

Before creating a module:

```text
[ ] Business capability identified
[ ] Purpose documented
[ ] Dependencies mapped
[ ] Consumers identified
[ ] Ownership identified
[ ] Metadata coverage checked
[ ] Deployment order understood
[ ] Data dependency understood
[ ] Security dependency understood
[ ] Integration dependency understood
[ ] Testing strategy defined
```

---

# 156. Development Checklist — After Creating Module

```text
[ ] Deploy to clean scratch org
[ ] Check all expected metadata
[ ] Check for unwanted metadata
[ ] Run source:status
[ ] Test source:pull
[ ] Test source:push
[ ] Review .forceignore
[ ] Check actionOverrides
[ ] Check Permission Sets
[ ] Check FlexiPages
[ ] Check tabs
[ ] Check layouts
[ ] Commit source
```

---

# 157. Interview Q&A

## Q1. Why did the author use deployment dependencies to organize packages?

**Answer:**

Because deployment failures often come from metadata dependencies and incorrect deployment order. Building layers around those dependencies can make deployments more predictable and repeatable.

---

## Q2. Is dependency-based modularization the only correct approach?

**Answer:**

No. The article explicitly says it is not the only way. But regardless of organizing principle, metadata dependencies affecting deployment must be addressed.

---

## Q3. Why have multiple `packageDirectories`?

**Answer:**

To represent multiple logical modules/package areas in one Salesforce DX project and allow the CLI to work with them as part of the project.

---

## Q4. What does `default: true` mean?

**Answer:**

It identifies the default package directory. In the article's workflow, newly pulled metadata is first placed there.

---

## Q5. Does changing the default directory move existing metadata?

**Answer:**

No. Existing metadata already organized in another package directory remains there. The default primarily influences where new pulled metadata goes.

---

## Q6. Why is `.forceignore` important?

**Answer:**

Because it controls which metadata Salesforce CLI interacts with during source synchronization, which is essential when multiple modules share one project.

---

## Q7. Where does `.forceignore` live?

**Answer:**

At the project root, with one project-level `.forceignore`.

---

## Q8. Why did `actionOverrides` matter?

**Answer:**

Because activating FlexiPages or custom actions can add references into object metadata. Those references create dependencies that can prevent an object from being deployed independently.

---

## Q9. How do you keep an object module independent from a FlexiPage?

**Answer:**

In the article's approach, remove/separate the relevant `actionOverrides` from the object metadata and deploy the dependent UI/override metadata in a later module.

---

## Q10. Why use scratch orgs during modularization?

**Answer:**

They provide disposable environments where you can test whether a proposed module can deploy independently and identify hidden dependencies without risking a production environment.

---

## Q11. What if source tracking becomes inconsistent?

**Answer:**

Check source status, project configuration, `.forceignore`, and scratch-org configuration. The article describes deleting the relevant scratch-org configuration and retrying; if the environment remains confusing, create a fresh scratch org.

---

## Q12. Why use aliases?

**Answer:**

To make it easier and safer to identify and target specific scratch orgs and other environments without relying on generated usernames.

---

## Q13. Should every module be as small as possible?

**Answer:**

No. The module should be cohesive and useful. Excessive fragmentation increases complexity.

---

## Q14. When should shared code become a base module?

**Answer:**

When the code has meaningful independent functionality and is genuinely reusable by multiple parts of the application/org.

---

## Q15. Is there one objectively correct modularization strategy?

**Answer:**

No. The article emphasizes team context, deployment needs, dependencies, trade-offs, and tooling limitations.

---

# 158. Errors & Gotchas — Quick List

```text
1. Assuming dependency-based layering is the only architecture.
2. Treating default package directory as a permanent destination for all metadata.
3. Assuming source:status perfectly predicts pull/push behavior.
4. Creating multiple .forceignore files per module.
5. Ignoring actionOverrides.
6. Treating FlexiPages as independent when object XML references them.
7. Deleting local metadata without understanding source tracking.
8. Keeping a broken scratch org alive too long.
9. Forgetting to clean up old scratch orgs.
10. Not using aliases.
11. Creating modules without a reason.
12. Over-fragmenting Permission Sets.
13. Ignoring metadata coverage.
14. Assuming deployment means activation.
15. Treating data as metadata.
16. Treating non-deployable assets as normal deployable metadata.
17. Ignoring shared-code blast radius.
18. Forgetting external/runtime dependencies.
```

---

# 159. Limits / Constraints

Modular Salesforce DX projects can face:

- Metadata API coverage limitations
- CLI/source tracking oddities
- FlexiPage/object coupling
- Permission Set complexity
- Deployment order requirements
- Source synchronization complexity
- Environment-specific settings
- Non-deployable assets
- Data migration requirements
- Shared-module blast radius
- Legacy metadata
- Tooling limitations

---

# 160. Best-Practice Decision Framework

When deciding whether metadata belongs in a module, ask:

```text
1. What functionality does it provide?
2. What does it depend on?
3. What depends on it?
4. Can it work independently?
5. Is the dependency deeply intertwined?
6. Is it reusable?
7. Does it have a different lifecycle?
8. Does another team own it?
9. Can it be tested independently?
10. Can it be deployed independently?
11. Does packaging support it?
12. Will separation increase operational complexity?
```

---

# 161. A Better Modern Workflow

For a current Salesforce project, a practical workflow can be:

```text
Business Capability
       |
       v
Metadata Inventory
       |
       v
Dependency Graph
       |
       v
Candidate Modules
       |
       v
sfdx-project.json / modern project config
       |
       v
Source Control
       |
       v
Scratch Org
       |
       v
Source Sync Validation
       |
       v
CI Validation
       |
       v
Package / Deployment Strategy
```

Exact CLI commands should be verified for the Salesforce CLI version being used.

---

# 162. Super-Advanced — Architecture Is an Iterative Discovery Process

Part 2 demonstrates that package architecture was not designed perfectly upfront.

Instead:

```text
Initial Assumption
       |
       v
Implementation
       |
       v
Deployment
       |
       v
Dependency Discovered
       |
       v
Refactor Module
       |
       v
New Insight
       |
       v
Better Module
```

This is normal.

---

# 163. Super-Advanced — Dependency Discovery Can Reveal Architecture

At first:

```text
es-base
```

Then dependency analysis revealed:

```text
Objects
Code
Styles
```

Then:

```text
es-base-objects
es-base-code
es-base-styles
```

Therefore:

> Dependency analysis can reveal the actual architecture of an application that was previously hidden inside a monolithic org.

---

# 164. Super-Advanced — Metadata as Architecture Evidence

Don't only ask:

```text
What should the architecture be?
```

Also inspect:

```text
What does the metadata tell us the architecture already is?
```

For example:

```text
Object XML
   |
   +--> actionOverride
   |
   v
Hidden UI dependency
```

The metadata itself exposes coupling.

---

# 165. Super-Advanced — Source Control as an Architecture Tool

Git isn't just for version history.

With modular source:

```text
Git
 |
 +--> Shows boundaries
 +--> Shows ownership
 +--> Shows change frequency
 +--> Shows dependency impact
 +--> Enables code review
```

Therefore source control can help improve architecture.

---

# 166. Super-Advanced — The Cost of a Module Boundary

Every module boundary has a cost:

```text
Boundary
 |
 +--> More isolation
 +--> More reuse
 +--> Independent release
 |
 +--> More dependency management
 +--> More configuration
 +--> More testing
 +--> More operational complexity
```

Therefore:

> **A module boundary should pay for itself.**

---

# 167. Super-Advanced — Dependency Inversion

If a feature depends on shared functionality, prefer a stable contract rather than tightly coupling internal implementation details where practical.

Concept:

```text
Feature
   |
   v
Stable Service Contract
   |
   v
Shared Implementation
```

This can reduce coupling.

The article itself is focused on metadata/package modularization rather than prescribing a full dependency-inversion framework, so this is an architectural extension rather than a direct article claim.

---

# 168. Super-Advanced — Shared Module Governance

For:

```text
es-base-code
```

used by many modules:

```text
Consumer A
Consumer B
Consumer C
      |
      v
es-base-code
```

consider:

- compatibility,
- regression testing,
- release notes,
- ownership,
- versioning,
- change approval.

The more consumers a module has, the more carefully its contract should be managed.

---

# 169. Super-Advanced — Dependency Heatmap

A useful extension for architecture reviews:

```text
Module          Consumers       Dependency Depth
------------------------------------------------
base-objects       8                 High
base-code          6                 Medium
base-styles        5                 Medium
space-management   2                 Low
bots               1                 Low
```

This helps identify:

- foundational modules,
- high blast-radius modules,
- candidates for stronger governance.

---

# 170. Super-Advanced — Module Health

A module can be evaluated by:

```text
Cohesion
Coupling
Dependency Count
Consumer Count
Change Frequency
Deployment Frequency
Test Coverage
Ownership Clarity
```

This isn't a Salesforce official scoring model; it's a practical architecture-review framework.

---

# 171. Part 2 — One-Page Revision

```text
PART 2
 |
 +--> Start with deployment dependencies
 |
 +--> Build layered modules
 |
 +--> Create multiple packageDirectories
 |
 +--> Configure scratch org
 |
 +--> Use meaningful org names
 |
 +--> Use .forceignore
 |
 +--> Start with base objects
 |
 +--> Test module in scratch org
 |
 +--> Watch actionOverrides
 |
 +--> Separate FlexiPage dependencies
 |
 +--> Use source:pull/status/push carefully
 |
 +--> Understand default package directory
 |
 +--> Understand .sfdx/orgs
 |
 +--> Recover from source tracking problems
 |
 +--> Create fresh scratch orgs when useful
 |
 +--> Use aliases
 |
 +--> Make modules more granular when justified
 |
 +--> Don't modularize without a reason
 |
 +--> Accept team-specific trade-offs
 |
 +--> Work around metadata/tooling limitations
```

---

# 172. Part 2 — 30-Second Interview Answer

If interviewer asks:

> **"How would you structure a Salesforce DX project for modular development?"**

You can answer:

> "I would first understand the deployment dependency graph rather than splitting metadata randomly. I would create a Salesforce DX project with multiple package directories, start with foundational metadata such as objects and fields, and then build higher layers such as shared code, shared UI, and application-specific functionality. I would validate each module in a clean scratch org and use `.forceignore` to prevent unrelated metadata from contaminating module boundaries. I would pay special attention to hidden dependencies such as FlexiPage-generated `actionOverrides`, Permission Sets, and metadata coverage limitations. Finally, I would keep the modules cohesive, avoid unnecessary fragmentation, and let the team's deployment, ownership, and release model drive the final boundaries."

---

# 173. Final Summary

Part 2 ka main lesson:

> **Modularization is not folder splitting. It is dependency-aware architecture.**

Start with:

```text
Deployment Problems
       |
       v
Dependency Patterns
       |
       v
Layered Modules
```

Then:

```text
Salesforce DX Project
       |
       +--> packageDirectories
       |
       +--> .forceignore
       |
       +--> Scratch Org
       |
       +--> Source Tracking
```

Then continuously:

```text
Deploy
  ↓
Observe
  ↓
Discover Dependency
  ↓
Refactor
  ↓
Deploy Again
```

And remember:

```text
More modules
     ≠
Better architecture
```

The right question is:

> **"Does this boundary create useful independence without introducing unnecessary complexity?"**

---

# 174. Key Takeaways

1. **Start package design from deployment dependencies.**
2. **Layered packages are one approach, not the only approach.**
3. **Every package strategy must handle metadata dependencies.**
4. **Use multiple `packageDirectories` for multiple logical modules.**
5. **`default: true` matters for where new pulled metadata is placed.**
6. **Changing the default does not reorganize existing metadata.**
7. **Use meaningful scratch-org names.**
8. **Automate scratch-org configuration.**
9. **Use a single root `.forceignore`.**
10. **`.forceignore` is critical for multi-module source synchronization.**
11. **Start with foundational object metadata.**
12. **Validate modules in scratch orgs.**
13. **Inspect XML when metadata dependencies are unclear.**
14. **FlexiPage activation can create `actionOverrides`.**
15. **`actionOverrides` can couple objects to UI metadata.**
16. **Separate/remove overrides if you need truly independent object modules.**
17. **Use `source:status` frequently, but don't treat it as a perfect push/pull preview.**
18. **Understand `.sfdx/orgs` and scratch-org source tracking.**
19. **When tracking becomes confusing, a fresh scratch org is often useful.**
20. **Use aliases for environments.**
21. **Delete old scratch orgs to stay organized and conserve limits.**
22. **More granular modules can improve isolation.**
23. **Don't modularize without a reason.**
24. **Shared code and UI can become reusable base modules.**
25. **There is no universally correct modularization strategy.**
26. **Permission Set splitting has both benefits and costs.**
27. **Metadata coverage/tooling limitations must be considered.**
28. **Deployment success doesn't always mean activation is complete.**
29. **Source-controlled metadata, data, and non-deployable assets need different strategies.**
30. **A package boundary should pay for its complexity.**

---

# 175. LinkedIn Post — Hinglish

## Salesforce Modular Development: Package Banana Goal Nahi, Dependency Samajhna Goal Hai 🚀

Salesforce DX ke modular development ko deeply study karte hue ek important lesson:

**Packages ko metadata folders ki tarah mat design karo.**

Pehle deployment failures samjho.

```text
Deployment Failure
       ↓
Dependency
       ↓
Dependency Layer
       ↓
Module
       ↓
Package
```

Part 2 mein mujhe sabse interesting concept laga:

### `actionOverrides`

Aap soch sakte ho:

```text
Object = Base Module
```

But Lightning App Builder ke through activate ki hui FlexiPage object metadata mein `actionOverrides` add kar sakti hai.

Then:

```text
Object
   ↓
actionOverride
   ↓
FlexiPage
```

Suddenly object independent nahi raha.

This is why Salesforce architecture mein:

**Metadata XML ko samajhna bhi important hai.**

Another important learning:

### `.forceignore` is not just a cleanup file.

Multi-module Salesforce DX project mein `.forceignore` actually source boundaries control karne mein help karta hai.

```text
Scratch Org
     ↓
source:pull
     ↓
.forceignore
     ↓
Correct Module
```

Aur sabse important:

> **More packages ≠ better architecture.**

Good modularization means:

- high cohesion
- manageable coupling
- clear dependencies
- clear ownership
- independent testing where practical
- meaningful deployment boundaries

Finally:

```text
Business Capability
        ↓
Dependency Graph
        ↓
Module
        ↓
Source Control
        ↓
Package
        ↓
CI/CD
```

That's the mindset I would carry into Salesforce Architect-level design.

#Salesforce #SalesforceDX #UnlockedPackages #SalesforceArchitect #DevOps #Git #CI_CD #ModularArchitecture #Metadata #SalesforceDevelopment

---

# 176. Source Note

This document is based on the provided **Part 2 source material and diagrams**, preserving its terminology, examples, workflow, XML, and historical Salesforce DX context.

Where additional development/architect guidance is included, it is explicitly framed as an architectural extension rather than presented as a direct claim from the 2018 article.

For current implementation, verify historical `sfdx force:*` command syntax against the current Salesforce CLI.

