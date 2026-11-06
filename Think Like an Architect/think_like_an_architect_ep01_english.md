# Think Like an Architect — Episode 1
## Grounding Agentforce in Dynamic External Documentation
### Salesforce Architecture | Complete English Notes

> **Source:** User-provided episode transcript.  
> **Preservation rule:** The complete supplied transcript is appended verbatim in Part 4. Speech-recognition mistakes and inconsistent product spellings are retained there rather than silently corrected.  
> **Editorial separation:** Parts 1–3 explain and extend the architecture; they are not verbatim statements by the speakers.

---

# Part 1 — Episode Overview

## 1. Objective

The business problem is to make thousands of externally managed product documents (PDF and HTML) available to Agentforce so that it can answer customer questions. The documents are updated regularly, so answers must reflect current content. A single conversation may require relevant information from different documents.

The architecture must cover the full lifecycle:
- Identify the external source.
- Ingest or reference documents in Data 360.
- Parse, chunk and index content.
- Retrieve relevant passages at runtime.
- Ground Prompt Templates and Agentforce with retrieved content.
- Cite the authoritative source.
- Synchronize updates and deletions.
- Enforce customer-specific visibility.

## 2. Speakers and Context

- Mary McCabe — Salesforce Architect Evangelism leader and Salesforce CTA.
- Scott Ratliff — Technical Architect Director with MuleSoft and Agentforce/Data 360 experience.
- Series objective: share concrete approaches for solving day-to-day Salesforce architecture problems.

---

# Part 2 — Three-Step Architectural Thinking Framework

## 3. WHAT — Understand the Business Requirement

An architect should understand the requirement before selecting a solution.

### 3.1 Requirement keyword analysis

| Business statement | Signal | Architectural implication |
|---|---|---|
| Thousands of product files | Volume | Scale, ingestion throughput, indexing time and cost |
| PDF and HTML | Format | Parsing, extraction, chunking, image/table handling |
| Managed outside Salesforce | External source | Connector, API, event notification, network/security |
| Agentforce answers customer questions | Agentforce | RAG, prompt, agent action |
| Files updated regularly | Freshness | Incremental updates, event triggers, refresh schedules |
| Always use the latest version | Current content | End-to-end freshness SLA and stale-index prevention |
| Agent can reference and read files | Searchable storage | Data 360 representation, search index and retriever |
| Different questions may use different files | Multi-document context | Retrieval across relevant documents in one conversation |

### 3.2 High-Level Requirements

**HLR-1 — External unstructured data access:** Agentforce must access externally managed unstructured PDF and HTML data.

**HLR-2 — Data freshness:** Source updates must be reflected in Agentforce answers within an agreed freshness window.

**HLR-3 — Cross-document retrieval:** Agentforce must retrieve relevant information from the appropriate documents across a single conversation.

### 3.3 Why paraphrase requirements?
The architect introduced terms such as “relevant” and “single conversation” to make the intended meaning explicit. This is not a silent change to the business requirement; it is a technique for validating shared understanding with stakeholders.

Questions to confirm:
- What does “immediately” mean: seconds, minutes or hours?
- Is every file customer-facing?
- Are PDF and HTML the only formats?
- Must tables, images, scanned PDFs and diagrams be interpreted?
- Is customer-specific document visibility required?
- Must citations navigate to the original source?

## 4. HOW — Design the Solution

1. Group related requirements.
2. Evaluate out-of-the-box capabilities first.
3. Test options against assumptions and constraints.
4. Record questions, assumptions, decisions and limitations.
5. Draw the architecture.
6. Preserve rejected alternatives and their reasons.

## 5. WHY — Justify the Decisions

The WHY step explains:
- Why the design was selected.
- Why alternatives were rejected.
- Which assumptions would cause a decision to be revisited.
- What future architects and maintainers need to know.

Capture these details in an **Architectural Decision Record (ADR)**.

---

# Part 3 — Detailed Architecture and Missing Technical Flow

## 6. Recommended Logical Architecture

The episode's design direction can be represented as:

```text
External Content Repository
(PDF / HTML / Product Files)
          |
          | 1. New / updated file notification
          v
   Source Connector / MuleSoft
          |
          | 2. Fetch file + metadata
          | 3. Validate / transform / classify
          v
       Data 360
   (Unstructured or structured ingestion)
          |
          | 4. Parse content
          | 5. Chunk content
          | 6. Build / update search index
          v
     Search Index
          |
          | 7. Retriever query
          v
      Retriever
          |
          | 8. Relevant chunks + metadata
          v
     Prompt Template
          |
          | 9. Grounded context
          v
      Agentforce
          |
          | 10. Reasoning / response
          v
       Customer
          |
          v
 Answer + Source Citation
```

**Important:** Source connector, MuleSoft, Data 360 ingestion mode, search index, retriever and prompt are separate architectural responsibilities. A particular implementation may automate or combine some steps.

## 7. Why Data 360 Was the Initial Choice

The architect assumed Data 360 because it provides a way to make unstructured data available for Agentforce grounding and retrieval. The discussion specifically references vector database/search capabilities.

### 7.1 What is unstructured data?
Data that does not naturally fit into a fixed relational row/column model:
- PDF manuals.
- HTML product documentation.
- Word documents.
- Images/scanned documents.
- Audio/video, subject to product and processing support.

“Unstructured” does not mean there is no structure at all. Documents may contain headings, paragraphs, tables and metadata; the challenge is processing them for retrieval.

### 7.2 Role of Data 360
- Ingest or reference external content.
- Process and represent the content.
- Create and maintain search indexes.
- Allow retrievers to return relevant content.
- Supply grounded context to Agentforce/Prompt Builder.

### 7.3 Vector search — simple explanation
Text can be represented numerically as embeddings. Semantic similarity between query and document-chunk embeddings can help retrieve relevant passages. Hybrid search can combine keyword and semantic relevance.

**Caution:** Verify current search modes, indexing behavior, supported formats, connector availability and entitlements against the current Data 360 release.

## 8. Option Analysis — Knowledge vs Data Library vs Direct Data 360 Ingestion

### 8.1 Option A — Salesforce Knowledge

Knowledge can be appropriate when the organization is prepared to manage the articles in Salesforce Knowledge and Knowledge is the operational content source of truth.

The episode challenged this option because:
1. The architect assumed the source files must remain in their existing repository/CMS.
2. A one-time migration into Knowledge did not fit that assumption.
3. MuleSoft-based transformation into Knowledge was considered, but preserving formatting and images can be complex.
4. Storing content in Knowledge does not automatically make it available to Agentforce; grounding/retrieval configuration is still required.
5. The episode discussed a 24-hour Data Library refresh and a 15-minute CRM connector refresh as concerns for the scenario.
6. Citations could point to the Knowledge copy while the authoritative source remained in the original CMS.

**Episode decision:** Knowledge was not selected as the preferred option.

### 8.2 Option B — Agentforce Data Library

The episode raised a concern about manually uploading and refreshing external unstructured files.

Important distinction:
- A Data Library is an out-of-the-box experience that can automate data-source, indexing and retriever setup for supported sources.
- The episode's concern was manual refresh of frequently changing files in the source repository.
- Do not generalize this to mean every Data Library integration is manual-only. Check current source and connector capabilities.

### 8.3 Option C — CRM Connector

The episode discussed a 15-minute refresh figure for the CRM connector route. Treat this as an episode-era assumption/observation, not a universal SLA for every connector or Data 360 ingestion mode.

### 8.4 Option D — MuleSoft + Data 360 ingestion

The architect considered MuleSoft the flexible route:
1. A file is created or updated in the source system.
2. MuleSoft captures the event.
3. MuleSoft retrieves the file and metadata.
4. Required preprocessing/transformation is performed.
5. Content/data is sent through a Data 360 ingestion interface.
6. Search indexing is updated.
7. The retriever can return the newly indexed content.

The episode mentioned an approximately three-minute refresh figure. This is not a guaranteed platform SLA. End-to-end freshness depends on event delivery, file transfer, ingestion, parsing, indexing and activation.

### 8.5 Decision comparison

| Option | Episode benefit | Episode concern |
|---|---|---|
| Knowledge | Familiar Salesforce content repository | Keep source external; migration/formatting complexity; refresh/citation concerns |
| Data Library | Out-of-the-box grounding experience | Manual upload/refresh not acceptable for this scenario |
| CRM Connector | Existing Salesforce CRM connectivity | Episode's 15-minute refresh assumption; source fit must be verified |
| MuleSoft + Data 360 ingestion | Flexibility, event-driven updates and preprocessing | More integration design and operational responsibility |

## 9. MuleSoft — From File Change to Data 360 Ingestion

### 9.1 How can MuleSoft detect a new file or new version?

Scott explained that the mechanism depends on the source repository.

Possible mechanisms:
1. **Webhook/event notification:** Repository emits a file-create/update event.
2. **Source API trigger:** API or connector detects new/updated files.
3. **Prebuilt connector/module:** Supported repositories such as Google Drive or SharePoint may have connector capabilities.
4. **Polling:** Query the source API at a fixed interval.
5. **Scheduled batch scan:** Use when real-time notification is unavailable.

### 9.2 End-to-end ingestion sequence

```text
1. File created/updated in repository
              |
2. Repository event / connector trigger
              |
3. MuleSoft receives file identity + metadata
              |
4. MuleSoft fetches latest file version
              |
5. Validate file type, size, status, checksum
              |
6. Extract metadata (source URL, title, customer, version)
              |
7. Optional preprocessing / transformation
              |
8. Call Data 360 ingestion interface
              |
9. Data 360 stores/represents content
              |
10. Parsing + chunk generation
              |
11. Search index updated
              |
12. Retriever can return new version
              |
13. Monitor and reconcile
```

### 9.3 Measuring freshness
“Immediately” is not measurable. Define a freshness SLA:
- Source update time.
- Event detection time.
- File retrieval completion.
- Ingestion acknowledgement.
- Parsing/chunking completion.
- Index availability.
- Retriever visibility.
- First answer using the new version.

Example: `Source update → searchable in agent = p95 under 5 minutes` is an illustrative SLA, not a Salesforce guarantee.

## 10. DLO vs UDLO vs UDMO

The transcript's speech recognition may mix DLO, UDL and UDMO terms. Their conceptual roles differ.

### 10.1 DLO — Data Lake Object
- Representation of ingested structured/tabular data in Data 360.
- Data is represented as records and fields.
- Suitable for structured data workflows, subject to the configured model.

### 10.2 UDLO — Unstructured Data Lake Object
- Represents/references unstructured files stored in external object storage.
- The file content can remain in blob storage while Data 360 manages references/metadata and processing pipelines.
- Salesforce documentation includes external storage and file-notification pipeline examples.

### 10.3 UDMO — Unstructured Data Model Object
- Model representation used for processed unstructured content.
- UDMO and related chunk objects can appear in Agentforce Data Library/Intelligent Context workflows.
- Exact generated object names and relationships can vary by configuration.

### 10.4 Quick comparison

| Term | Meaning |
|---|---|
| DLO | Data-lake representation of structured records |
| UDLO | Reference/representation for external unstructured files |
| UDMO | Model representation of processed unstructured content |
| Search Index | Search-optimized indexed representation |
| Retriever | Component that returns relevant records/chunks for a query |

## 11. Structured vs Unstructured Ingestion — MuleSoft Decision

Scott explained that direct unstructured ingestion may provide limited filtering depending on the metadata supplied by the source.

### 11.1 Direct unstructured route
- Ingest/reference the original document as unstructured content.
- Search the document chunks.
- Filtering depends on connector and source metadata capabilities.

### 11.2 Preprocessing + structured ingestion
MuleSoft can:
1. Identify the file.
2. Read directory/source/customer context.
3. Parse or extract content.
4. Enrich metadata.
5. Store required fields in a structured representation.
6. Send it through a structured ingestion route into Data 360.
7. Use dynamic filters in a custom retriever.

Possible metadata:
- `CustomerId`
- `ProductFamily`
- `Region`
- `DocumentType`
- `EffectiveFrom`
- `EffectiveTo`
- `Version`
- `Confidentiality`
- `SourceUri`

**Trade-off:** Better filtering and control, but more custom processing, schema design, maintenance and testing.

## 12. Requirement 3 — Retriever + Prompt Template + Agentforce

### 12.1 Runtime flow

```text
Customer asks a question
          |
          v
Agentforce receives user message
          |
          v
Agent reasoning selects relevant topic/action
          |
          v
Prompt Template invoked
          |
          +----------------------+
          |                      |
          v                      v
     User Query             Retriever
                                 |
                                 v
                         Search Index query
                                 |
                                 v
                     Relevant document chunks
                                 |
                                 v
                   Chunks + metadata/citations
          |                      |
          +----------+-----------+
                     |
                     v
             Prompt is grounded
                     |
                     v
             LLM generates answer
                     |
                     v
          Agentforce returns response
                     |
                     v
              Customer sees answer
```

### 12.2 What does a retriever do?
At runtime, a retriever queries a search index and returns relevant content chunks/fields. Instead of blindly placing an entire document into the prompt, it supplies relevant passages.

### 12.3 What does a Prompt Template do?
A Prompt Template:
- Provides instructions to the model.
- Receives the user's question.
- Receives retriever output/context.
- Defines behavior when information is missing.
- Defines response format and citation expectations.

### 12.4 Role of the Agentforce reasoning engine
As described in the episode:
- Agent topics, instructions and actions are available.
- The reasoning engine determines which action to use.
- The prompt/retriever action supplies grounded context.
- Agentforce returns the response to the user.

### 12.5 Recommended grounding instructions
- Treat retrieved context as primary evidence.
- If the context does not contain the answer, say so.
- Do not invent unsupported facts.
- Include citations/source references.
- Do not silently merge conflicting documents.
- Do not use content outside the user's access scope.
- Consider document effective dates and versions.

## 13. Citations — Link Back to the Source of Truth

The architect raised a concern: if the original source of truth remains in a CMS but a copy is migrated to Knowledge, citations may point to the Knowledge copy rather than the authoritative source.

Preferred design:
- Citations should point to the authoritative source/document/version where possible.
- Preserve stable source URI, title, file ID/version and page/section metadata with retrieved chunks.
- Ensure the user is authorized to open the cited source.
- Prevent stale citations after source deletion or replacement.

**Important:** Configuring a citation field in a retriever and authorizing the user to open the source are separate concerns.

## 14. Visibility, Customer-Specific Data and Dynamic Filters

### 14.1 Problem
Customer A may have specific product standards while Customer B has different procedures. Customer A's answer must not use Customer B's confidential documents.

### 14.2 Filtering design
- Store customer/product/region/access classification in trusted document metadata.
- Derive runtime user/customer context from authenticated identity.
- Bind retriever filters to trusted context.
- Enforce access at the retrieval boundary, not only in the UI after retrieval.
- Run negative tests: Customer A must never retrieve Customer B content.

### 14.3 Options discussed in the episode

**Option A — Multiple connectors/indexes:** Use separate connector configurations for directories or groups. This can provide separation but may become manual and operationally complex.

**Option B — MuleSoft preprocessing:** Identify metadata from the file source/directory/content, enrich the document and use structured ingestion with dynamic custom-retriever filters.

**Option C — File naming conventions:** Infer customer or document type from naming patterns. This can simplify development but creates dependency on naming discipline and introduces error risk.

### 14.4 Additional best practice
Do not use a filename as the authorization source for sensitive content. Make access decisions using trusted identity and authoritative metadata/ACLs.

## 15. Multiple Sources — Ensemble Retriever

Scenario: The agent needs answers from both Knowledge and unstructured documents.

Scott discussed:
- Calling multiple individual retrievers from a prompt.
- Using an Ensemble Retriever to query multiple search indexes through one retriever interface.

### 15.1 Multiple individual retrievers
Advantages:
- Independent control for each source.
- Source-specific filters and fields.
- Different retrieval strategies.

Trade-offs:
- Multiple calls.
- More prompt orchestration.
- Result merging, ranking and duplicate handling.

### 15.2 Ensemble Retriever
The episode described these benefits:
- One retriever interface can query multiple search indexes.
- Multiple searches can occur behind the scenes.
- The prompt/agent receives a unified retrieval experience.

Validate current platform support, ranking behavior, filter propagation and source-level permissions before selecting it.

## 16. Intelligent Context — What, Why and When

### 16.1 Episode discussion
Intelligent Context was discussed as a way to improve unstructured-content processing and chunking.

Scott's points:
- It may be useful for PDFs/images, subject to format and size limits.
- It can help improve parsing and retrieval quality.
- It may provide more configuration control than default unstructured processing.
- It is not suitable for every file type or use case.

### 16.2 When to consider it
- PDFs contain tables, images, diagrams or complex layouts.
- Default parsing creates incoherent chunks.
- Retrieval answers miss important context.
- Content-specific parsing/chunking tuning is required.

### 16.3 When not to assume it is suitable
- File formats are unsupported.
- Files exceed configured limits.
- Content is mostly video or complex media unsupported by the selected path.
- Cost/processing constraints outweigh the retrieval benefit.

### 16.4 Evaluation loop
1. Select representative documents.
2. Configure parsing and preprocessing.
3. Generate chunks.
4. Inspect chunk boundaries and extracted text.
5. Run representative user questions.
6. Inspect retrieved passages and citations.
7. Measure answer accuracy and retrieval recall.
8. Tune and repeat.
9. Publish/activate the approved configuration.

## 17. Chunking Strategy — Detailed Explanation

### 17.1 What is a chunk?
A chunk is a smaller segment created by dividing a large document so the retrieval engine can find relevant passages.

### 17.2 Episode's default-chunk discussion
Scott mentioned approximately 512-token chunks and paragraph separation in the context of the default behavior he was describing. This is an episode-specific description, not a universal or guaranteed current setting.

### 17.3 Chunking trade-offs

| Strategy | Benefit | Risk |
|---|---|---|
| Small chunks | Precise retrieval | Missing context |
| Large chunks | More surrounding context | Irrelevant text/noisy prompt |
| Paragraph-based | Natural boundaries | Paragraphs may be too long/short |
| Heading/section-aware | Preserves document structure | Depends on good parsing |
| Overlapping chunks | Preserves boundary context | Duplicate retrieval/token cost |
| Semantic chunking | Topic-coherent segments | More processing/configuration |

### 17.4 How to test chunk quality
- Is the heading retained with the chunk?
- Are table headers and values kept together?
- Are image captions/context preserved?
- Are definitions and exceptions retrievable together?
- Is each chunk traceable to a source page/section?
- Can the required answer be assembled from one or more chunks?

### 17.5 Recommended tuning approach
Start with out-of-the-box defaults. Measure retrieval quality on representative questions. Introduce custom preprocessing/chunking only when it produces measurable improvement.

## 18. File Deletion and Synchronization — Critical Missing Flow

The Q&A addressed deletion. The speaker noted that connector behavior varies by source and suggested that a weekly full comparison/reconciliation may be useful where real-time deletion detection is unavailable.

### 18.1 Delete-event flow

```text
Source file deleted
       |
       v
Repository emits delete event
       |
       v
MuleSoft / connector receives file ID
       |
       v
Locate corresponding Data 360 record/index entry
       |
       v
Delete, deactivate or mark unavailable
       |
       v
Refresh affected search index
       |
       v
Retriever no longer returns deleted content
       |
       v
Citation/link is invalidated or marked unavailable
       |
       v
Audit + reconciliation
```

### 18.2 If delete events are unavailable
- Run a scheduled inventory scan.
- Compare source file IDs/versions with the ingested inventory.
- Detect missing source files.
- Apply the deletion/deactivation policy.
- Record an audit trail.
- Test that deleted content is no longer retrievable.

### 18.3 Version update vs delete-and-recreate
- Preserve stable file identity and version tracking.
- Fetch the latest version on update.
- Validate how old chunks are replaced/deactivated.
- Prevent duplicate versions from appearing in search results.
- Preserve effective dates and superseded status.

## 19. Intelligent Context Limits and File Constraints

The episode mentioned, in the context of a sample setup:
- Up to five files.
- 10 MB per file.
- Representative variations of files for testing Intelligent Context.

**Critical note:** These are episode-specific setup observations. Do not treat them as universal current platform limits. Verify current Data 360/Intelligent Context documentation, edition, file type, UI workflow and release-specific limits.

General checklist:
- Maximum file size.
- Maximum files per configuration/test.
- Supported formats.
- PDF page count.
- Image/table parsing capability.
- Index size and record/row limits.
- Ingestion throughput.
- Credits/consumption.
- Search query limits and concurrency.

## 20. Intelligent Context vs Direct Unstructured Processing vs Custom Preprocessing

| Approach | Use when | Trade-off |
|---|---|---|
| Default unstructured processing | Standard documents and basic retrieval | Less parsing/chunking control |
| Intelligent Context | Complex PDFs/images and tuned extraction/chunking | Format/size/processing constraints |
| MuleSoft preprocessing + structured ingestion | Strong metadata/filtering and custom extraction needed | More custom code, schema and operations |
| Multiple indexes/retrievers | Different content domains or access scopes | More configuration and orchestration |

## 21. Employee Agent vs Service Agent

Scott's answer was that agent type depends on the use case and audience.

- **Employee Agent:** Internal employee workflows; employee identity/context and internal permissions matter.
- **Service Agent:** Customer-facing service scenarios; service identity/configuration and customer access boundaries matter.

Decision questions:
- Is the user an employee or a customer?
- Which channel will host the agent?
- Under which identity will data be accessed?
- How will user-specific authorization be enforced?
- How will customer-specific files be isolated?

The episode discussed a Service Agent as suitable for customer questions arriving from a website.

## 22. AWS S3 and Other External Repositories

The episode's Q&A noted:
- The unstructured-data route can also be considered for documents stored in AWS S3.
- Available connector metadata influences filtering and preprocessing.
- Metadata depends on the source.

Design checklist:
- S3 bucket/object key.
- Version ID/ETag.
- Last-modified timestamp.
- Tags/metadata.
- Event notifications.
- Access role and bucket policy.
- Delete marker/versioning behavior.
- Region/network/security.
- Stable source URI and citation.

---

# Part 4 — Architecture Decision Register

## 23. Decisions, Assumptions and Open Questions

| ID | Type | Item | Why it matters |
|---|---|---|---|
| D-01 | Decision | Data 360 as central grounding/data platform | Agentforce needs searchable unstructured context |
| A-01 | Assumption | Source files remain in the existing repository/CMS | Rules out one-time migration as the default |
| D-02 | Decision | Do not migrate all files into Knowledge as a one-off | Preserves source ownership |
| A-02 | Assumption | Knowledge/Data Library refresh paths discussed are too slow/manual for this SLA | Determines ingestion path |
| D-03 | Decision | Prefer direct Data 360 ingestion path | Better freshness/flexibility |
| Q-01 | Question | What is the source system and its event/connector capability? | Determines CRM connector vs MuleSoft |
| D-04 | Conditional decision | CRM connector or MuleSoft + ingestion API | Depends on source capability and freshness |
| A-03 | Assumption | 24-hour and 15-minute refresh are not acceptable | Business SLA must confirm |
| A-04 | Assumption | Approximately 3-minute ingestion path is close enough | Must be load-tested and measured |
| D-05 | Decision | Retriever + Prompt Template grounds Agentforce | Connects indexed content to runtime answer |
| Q-02 | Question | Are there customer/file-level visibility restrictions? | Determines custom retriever/filter design |
| D-06 | Conditional decision | Custom retriever(s) if restrictive visibility is required | Enforces scoped retrieval |
| D-07 | Option | Ensemble Retriever for multiple indexes | Simplifies unified retrieval interface |
| Q-03 | Question | How are file deletions detected and propagated? | Prevents stale/unauthorized content |

## 24. Questions for Business and Source-System Owners

1. What is the exact repository/CMS?
2. How many files, average size and maximum size?
3. What is the update frequency and peak update volume?
4. What does “immediately” mean in seconds/minutes?
5. Are PDFs text-searchable or scanned images?
6. Are tables, diagrams and images business-critical?
7. Are files customer-, region- or product-specific?
8. What is the source-of-truth URL and citation requirement?
9. How are new files, updates, renames and deletions detected?
10. Is source version history available?
11. What are the access-control/ACL rules?
12. What is expected concurrency?
13. What answer accuracy and citation coverage are required?
14. What is the retention/deletion policy?
15. What are Data 360 credit and operational cost constraints?

---

# Part 5 — Production-Grade Design Additions

## 25. Security and Trust Boundaries

- Authenticate to the source repository and MuleSoft using approved credentials.
- Use least-privilege access to source files and Data 360.
- Preserve source ACLs/customer entitlements as trusted metadata.
- Derive runtime user/customer context from authenticated identity.
- Apply retrieval filters before content reaches the LLM.
- Do not rely on prompt instructions alone for document-level authorization.
- Ensure citations do not expose documents the user cannot open.
- Protect against prompt injection in retrieved documents: treat document text as untrusted content, not system instructions.
- Audit ingestion, updates, deletes and retrieval access.

## 26. Freshness, Consistency and Version Management

Define an end-to-end freshness objective:
- Source update timestamp.
- Event detection timestamp.
- Ingestion start/completion.
- Parsing completion.
- Search index availability.
- Retriever visibility.
- Agent response using the new version.

Recommended controls:
- Store file ID, source version, checksum/ETag and last-modified timestamp.
- Make ingestion idempotent.
- Prevent an older version from overwriting a newer version.
- Deactivate superseded chunks.
- Reconcile source inventory with Data 360 inventory.
- Alert when the freshness SLA is breached.

## 27. Observability and Operational Metrics

Measure:
- New/updated/deleted file count.
- Event-to-ingestion latency.
- Ingestion failures and retries.
- Parsing failures.
- Chunk count and quality.
- Indexing latency.
- Search latency.
- Retriever no-result rate.
- Citation validity.
- Stale-version retrieval rate.
- Cross-customer retrieval leakage (must be zero).
- Data 360 credit/consumption.

## 28. RAG Quality Testing

Build a test set containing:
- Simple direct questions.
- Questions requiring two or more documents.
- Questions whose answers are absent.
- Conflicting old/new document versions.
- Customer-specific restricted questions.
- Table/image-heavy PDFs.
- Similar product names.
- Prompt-injection attempts embedded in documents.

Evaluate:
- Retrieval precision/recall.
- Answer correctness.
- Citation correctness.
- Groundedness.
- Abstention when evidence is missing.
- Latency and cost.
- Authorization isolation.

## 29. Failure Scenarios and Recovery

| Failure | Recommended handling |
|---|---|
| Source API unavailable | Retry with backoff; preserve event/checkpoint |
| File download incomplete | Verify checksum/size; retry |
| Unsupported file type | Quarantine and alert |
| Parsing failure | Persist error; retry if transient; remediate if permanent |
| Indexing delayed | Track pending state; alert on SLA breach |
| Duplicate update event | Idempotency key/version check |
| Out-of-order versions | Reject older version |
| Delete event missed | Scheduled source-vs-index reconciliation |
| Retriever returns stale content | Deactivate old chunks; reindex and regression-test |
| Citation URL broken | Refresh source metadata or mark unavailable |
| Unauthorized document retrieved | Stop response, investigate filters/ACLs, audit and remediate |

## 30. Recommended Implementation Phases

1. **Discovery:** Source repository, volume, formats, update/delete APIs, security and SLA.
2. **Proof of concept:** Representative PDFs/HTML, ingestion, parsing, chunking, index and retriever.
3. **Freshness test:** New/update/delete event to searchable content.
4. **Security test:** Customer/region/user isolation and citation access.
5. **RAG quality test:** Multi-document questions, no-answer cases, citations and chunk tuning.
6. **Scale test:** File volume, concurrent queries, ingestion throughput and cost.
7. **Production rollout:** Monitoring, alerts, runbooks, backfill and reconciliation.

---

# Part 6 — Interview Preparation and Revision

## 31. Important Interview Questions

**Q1. Why use Data 360 for external product documentation?**  
To make unstructured content available for processing, indexing and retrieval so Agentforce can use relevant document context to ground its answers.

**Q2. Why not migrate all documents into Salesforce Knowledge?**  
In this case the source had to remain in its existing repository. The episode also raised formatting/image migration complexity, refresh cadence and citation-to-source concerns.

**Q3. What is the difference between a Data Library and a custom ingestion pipeline?**  
A Data Library automates common grounding setup for supported sources. A custom pipeline provides more control over source events, file retrieval, preprocessing, metadata enrichment, ingestion and lifecycle management, at higher implementation/operational cost.

**Q4. What does MuleSoft do in this architecture?**  
It detects or receives source changes, retrieves files, transforms/enriches metadata and invokes the Data 360 ingestion path.

**Q5. What is RAG?**  
Retrieval-Augmented Generation. The system retrieves relevant external knowledge at runtime and supplies it as context to the LLM before it generates an answer.

**Q6. What is a retriever?**  
A runtime component that queries a configured search index and returns relevant chunks/fields for grounding.

**Q7. What is a Prompt Template?**  
A reusable prompt configuration that combines instructions, user inputs and retrieved context to guide the LLM response.

**Q8. What is chunking and why does it matter?**  
Chunking divides long documents into retrieval-sized segments. Poor boundaries can separate facts from headings/tables or omit context; good chunking improves retrieval relevance.

**Q9. How do you support multiple documents in one conversation?**  
Use an index/retriever that searches the relevant corpus and supplies retrieved chunks to the prompt. Multiple indexes can be combined through multiple retrievers or an ensemble capability where supported.

**Q10. How do you enforce customer-specific document visibility?**  
Use trusted identity and document metadata/ACLs to filter retrieval before content reaches the LLM. Test cross-customer isolation.

**Q11. How do you handle document deletion?**  
Capture delete events where available; otherwise reconcile source inventory with indexed inventory. Delete/deactivate corresponding content and verify it is no longer retrievable.

**Q12. What is Intelligent Context?**  
In the episode, it is discussed as a way to configure and improve processing/parsing/chunking of unstructured content, especially complex PDFs/images, subject to supported formats and limits.

**Q13. What is an ensemble retriever?**  
A retrieval abstraction that can query multiple search indexes and present results through one retriever interface, where supported.

**Q14. Why are source citations important?**  
They help users verify answers and trace information to the authoritative document/version. Citation correctness and user access permissions must both be addressed.

**Q15. Why is “immediate refresh” not a sufficient requirement?**  
It is not measurable. Define a freshness SLA and measure source-update-to-searchable-content latency.

## 32. Common Interview and Architecture Traps

1. Uploading a file does not automatically make it searchable by an agent.
2. Ingestion, parsing, chunking, indexing and retrieval are separate stages.
3. A retriever is not the same as a search index.
4. A prompt template is not the source repository.
5. Agentforce does not automatically know every external document; grounding must be configured.
6. A connector refresh schedule is not the same as end-to-end answer freshness.
7. The episode's “3 minutes”, “15 minutes” and “24 hours” are scenario-era figures, not universal guarantees.
8. Citations should point to the authoritative source where possible.
9. Prompt instructions alone cannot enforce document-level security.
10. Deletion must be designed; connectors do not necessarily propagate deletes identically.
11. More chunks do not automatically mean better answers.
12. Large chunks can reduce precision; small chunks can lose context.
13. Customer-specific retrieval needs trusted filters and negative security tests.
14. Multiple retrievers can increase prompt orchestration complexity.
15. Intelligent Context suitability depends on actual formats, size and use case.

## 33. Quick Summary

- Use WHAT → HOW → WHY to structure architecture decisions.
- Convert the business narrative into explicit requirements: external PDF/HTML content, freshness and cross-document answers.
- Data 360 is the episode's selected grounding/data-platform direction.
- Knowledge was challenged because the source must remain external and migration, refresh and citation concerns exist.
- The episode's manual Data Library concern must not be generalized to all current connectors.
- MuleSoft can capture file changes, fetch files, preprocess/enrich metadata and ingest into Data 360.
- DLO, UDLO and UDMO represent different data concepts; do not use them interchangeably.
- A Search Index stores a searchable representation; a Retriever returns relevant chunks.
- A Prompt Template combines user question, instructions and retriever output.
- Agentforce uses the grounded prompt/action to answer.
- Visibility restrictions require trusted metadata/identity filters before LLM context is created.
- An Ensemble Retriever can simplify access to multiple indexes where supported.
- Evaluate Intelligent Context and chunking using representative documents and questions.
- Both file updates and deletions must be synchronized for trustworthy dynamic content.
- Define freshness, quality, security and operational SLAs.

## 34. No-Skip Checklist

- [ ] Episode objective and speakers
- [ ] WHAT/HOW/WHY approach
- [ ] Thousands of PDF/HTML product files
- [ ] External source repository
- [ ] Agentforce customer Q&A
- [ ] Regular updates and latest-version requirement
- [ ] Multiple documents in one conversation
- [ ] HLR formulation and paraphrasing
- [ ] Data 360 as initial architecture choice
- [ ] Vector/search capability
- [ ] Knowledge option and migration/formatting concerns
- [ ] Knowledge does not automatically ground the agent
- [ ] Data Library and manual-upload concern
- [ ] 24-hour / 15-minute / approximately 3-minute episode figures with caveat
- [ ] CRM connector vs MuleSoft decision
- [ ] Source system discovery
- [ ] Event-triggered file ingestion
- [ ] Google Drive and SharePoint connector discussion
- [ ] DLO / UDLO / UDMO terminology
- [ ] Prompt Template + Retriever + Agentforce flow
- [ ] Visibility constraints and custom retriever
- [ ] Intelligent Context
- [ ] Employee Agent vs Service Agent
- [ ] Multiple retrievers and Ensemble Retriever
- [ ] Customer-specific standards/procedures
- [ ] Metadata filtering and preprocessing
- [ ] File naming convention trade-off
- [ ] Chunking and default chunk discussion
- [ ] Preprocessing and multiple prompts
- [ ] File deletion and weekly reconciliation
- [ ] AWS S3 source and metadata
- [ ] File size/count and platform-limit discussion
- [ ] Lucid diagramming and ADR resources
- [ ] Security, freshness, observability and RAG testing additions

## 35. Board-Level Answer (60–90 seconds)

“We need Agentforce to answer customer questions using thousands of externally managed PDF and HTML product documents, while reflecting source updates quickly and retrieving relevant content across multiple documents in one conversation. I would first confirm the source repository, document formats, volume, update/delete mechanisms, customer-level access rules and a measurable freshness SLA.

The target pattern is to make the external content available in Data 360, process and index it, and expose the indexed content through a retriever. MuleSoft is a flexible integration option where the source needs event detection, file retrieval, preprocessing or metadata enrichment. At runtime, the user's question is passed to a Prompt Template, the retriever returns relevant chunks and source metadata, and Agentforce generates a grounded answer with citations.

If customer-specific visibility is required, I would enforce trusted metadata/identity filters at retrieval time, not rely on prompt instructions alone. I would also design update and delete propagation, version control, idempotency, reconciliation, monitoring and RAG quality tests. I would validate connector capabilities and current refresh behavior in a proof of concept before committing to an SLA.”

---

# Part 7 — 2026 Salesforce Documentation Checks (Separate from Episode)

> Product names and UI labels evolve. These are current documentation references checked for this note. The episode's numeric refresh/size statements remain attributed to the recording and are not treated as current universal limits.

## 36. Current Documentation Findings

1. Salesforce documents that Data 360 can ingest and index unstructured data and make it available for Agentforce, RAG, automation and analytics. Built-in connector examples include Amazon S3, Azure, Google Drive and Microsoft SharePoint.  
   Source: [Data 360 Features Overview](https://developer.salesforce.com/docs/data/data-cloud-dev/guide/dc-features-overview.html).

2. Salesforce CRM connector documentation distinguishes batch and streaming modes. Batch checks at scheduled intervals (the current Help page describes 10–15 minutes); streaming processes with minimal latency. Initial ingestion still starts in batch mode, and eligible streams can move to streaming.  
   Sources: [CRM Connector Streaming](https://help.salesforce.com/s/articleView?id=data.c360_a_crm_connector_streaming.htm&language=en_US&type=5), [CRM Connection Considerations](https://help.salesforce.com/s/articleView?id=data.c360_a_managing_crm_connections.htm&language=en_US&type=5).

3. Salesforce documentation describes Agentforce Data Libraries as automating multiple setup steps across Data 360, including data streams/mapping, search index and retriever creation for supported sources. Therefore, do not generalize the episode's manual-upload concern to every current Data Library integration.  
   Sources: [What Are Data Libraries?](https://help.salesforce.com/s/articleView?id=data_library_concept.htm&language=en_US&type=5), [Ground Your Agent with Data Libraries](https://developer.salesforce.com/docs/ai/agentforce/guide/agent-dx-adl.html).

4. Salesforce's current developer guidance describes creating a search index and retriever for ingested website content, including tuning parsing/chunking for the content format and returning citation-ready fields.  
   Source: [Create Search Index and Retriever](https://developer.salesforce.com/docs/ai/ground-agentforce-on-website/guide/aes-create-search-index-and-retriever.html).

5. Salesforce's Intelligent Context guide describes configuring parsing/preprocessing, generating chunks, publishing the configuration and creating a retriever. It illustrates a workflow using a limited set of sample files; do not treat that sample setup as the universal production file limit.  
   Source: [Configure Data 360 and Intelligent Context](https://developer.salesforce.com/docs/ai/power-agentforce-visual-data/guide/aes-configure-data-360.html).

6. Salesforce documents a UDLO setup for external unstructured files, including a file-notification pipeline to keep the UDLO current. The documented example is Google Cloud Storage; use the relevant source-specific guide for other repositories.  
   Source: [Set Up Unstructured Data Lake Object](https://developer.salesforce.com/docs/data/data-cloud-int/guide/c360-a-gcs-udlo.html).

7. Salesforce documents Prompt Template grounding as combining the user question and retriever output, with instructions to keep answers tied to indexed content.  
   Source: [Build and Test a Prompt in Prompt Builder](https://developer.salesforce.com/docs/ai/ground-agentforce-on-website/guide/aes-create-prompt.html).

8. Salesforce Help states that Agentforce Data Libraries require Data 360 setup and consume Data 360 credits. Check current editions, permissions and entitlements for the target org.  
   Source: [Use the Answer Questions with Knowledge Action](https://help.salesforce.com/s/articleView?id=sf.agent_setup_data_sources.htm&language=en_US&type=5).

## 37. Episode Statements vs Current Validation

| Episode statement | How to treat it |
|---|---|
| Knowledge/Data Library refresh 24 hours | Scenario-era figure; validate current source and ingestion mode |
| CRM connector refresh 15 minutes | Not universal; current CRM connector docs distinguish batch and streaming |
| Ingestion API around 3 minutes | Episode estimate, not guaranteed end-to-end SLA |
| Intelligent Context sample max five files / 10 MB | Recording-specific setup observation; verify current feature limits |
| Default chunk size around 512 | Episode-specific description; inspect actual current configuration |
| Manual Data Library upload | Not universally true for all current Data Library source integrations |

---

