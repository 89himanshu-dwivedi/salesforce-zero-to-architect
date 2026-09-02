[Home](../index.md) / [10 · LWC Advanced](index.md) / **File Handling**

# File Handling

4 topics · Series 10: LWC Advanced

**Topics on this page**

- [File Upload](#file-upload)
- [ContentVersion](#contentversion)
- [ContentDocument](#contentdocument)
- [ContentDocumentLink](#contentdocumentlink)

## File Upload

*Uploading files in LWC via lightning-file-upload.*

### 🌱 Simple

*Beginner - plain language*

**lightning-file-upload** gives a drag-and-drop/picker control to upload files to a record. It creates `ContentVersion` records and links them, returning uploaded file info via the `uploadfinished` event.

### 📏 Limits

*Governor & platform limits*

- `lightning-file-upload` maximum 2 GB per file but 10 files per upload.
- Not supported for guest users in all Experience Cloud configurations.
- Base64 upload through Apex is bounded by the 6 MB heap and callout limits.
- File storage allocation is separate from data storage.

> **Want the deeper material for this topic?**
> Advanced and architect-level depth, real-world scenarios, gotchas and interview questions
> are not published here. Connect with me - **LinkedIn · X · GitHub** - details on the
> [home page](../index.md).

## ContentVersion

*The object that stores actual file content and versions.*

### 🌱 Simple

*Beginner - plain language*

**ContentVersion** stores a file's actual binary content (in `VersionData`) plus metadata. Each upload or new version creates a ContentVersion row; the first one generates a parent `ContentDocument`.

### 📏 Limits

*Governor & platform limits*

- Every version consumes file storage - version history is a hidden cost.
- Inserting via Apex with base64 is bounded by heap (6 MB sync).
- Maximum file size 2 GB via the UI, lower via API paths.

> **Want the deeper material for this topic?**
> Advanced and architect-level depth, real-world scenarios, gotchas and interview questions
> are not published here. Connect with me - **LinkedIn · X · GitHub** - details on the
> [home page](../index.md).

## ContentDocument

*The logical file record pointing to its latest version.*

### 🌱 Simple

*Beginner - plain language*

**ContentDocument** represents the logical file in Salesforce Files — one per file, pointing to the latest `ContentVersion`. You delete the document to remove the file (and all versions).

### 📏 Limits

*Governor & platform limits*

- Deleting the parent record does not always delete the ContentDocument - orphans accumulate.
- Sharing is controlled by ContentDocumentLink, not by the parent record alone.
- Counts toward file storage until purged from the Recycle Bin.

> **Want the deeper material for this topic?**
> Advanced and architect-level depth, real-world scenarios, gotchas and interview questions
> are not published here. Connect with me - **LinkedIn · X · GitHub** - details on the
> [home page](../index.md).

## ContentDocumentLink

*Junction sharing a file (ContentDocument) to records/users.*

### 🌱 Simple

*Beginner - plain language*

**ContentDocumentLink** links a `ContentDocument` to a record or user (`LinkedEntityId`), with a `ShareType` and `Visibility`. It's how a file becomes visible on a record's Files related list.

### 📏 Limits

*Governor & platform limits*

- Cannot be queried without a filter on `ContentDocumentId` or `LinkedEntityId`.
- `ShareType` and `Visibility` must be set correctly or guest users see nothing.
- Inserting links for setup and non-setup entities in one transaction can trigger Mixed DML.

> **Want the deeper material for this topic?**
> Advanced and architect-level depth, real-world scenarios, gotchas and interview questions
> are not published here. Connect with me - **LinkedIn · X · GitHub** - details on the
> [home page](../index.md).

---

## Connect

These pages carry the **definitions and limits** only. The advanced depth, real-world
scenarios, error playbooks, best-option reasoning and interview questions are kept aside.

If you would like them, or you want to talk about the topics on this page, connect with me
on **LinkedIn**, **X (Twitter)** or **GitHub** - all links are on the
[home page](../index.md).

*- Himanshu Kumar*
