# Salesforce AI — Forward Deployed Engineer Roadmap

**Part 3 of 3 — AI Forward Deployed Engineer path.** Deep dive into Salesforce's AI stack — Agentforce, Data 360, Einstein Trust Layer, Agent Script, MCP on Salesforce — plus the Salesforce-specific FDE skill set.

> Companion files:
> - **`ai-ml-dl-rag-roadmap.md`** — Software Engineering Foundation → AI/ML Fundamentals → AI Engineering (LLMs, RAG, agents) → Production AI Engineering → GenAIOps + AI Security.
> - **`fde-roadmap.md`** — Enterprise Integration → AI System Design → the Forward Deployed Layer → the AI Engineer (Forward Deployment) role.

---

## 📌 Levels in This File

9. [☁️ Salesforce AI — In Depth](#9-salesforce-ai--in-depth)
10. [☁️ Salesforce AI — Tools & Platform In Depth](#10-salesforce-ai--tools--platform-in-depth)

---

## 9. Salesforce AI — In Depth

<details>
<summary><b>Salesforce AI Platform</b></summary>

Agentforce 360 Platform · Einstein / Salesforce AI Capabilities · AI Embedded in CRM Workflows · Data 360 as AI Data Foundation · Agentforce Trust Layer · AI Models and Model Management · Models API · External Model Connectivity / BYOLLM · LLM Open Connector
</details>

<details>
<summary><b>Agentforce Fundamentals</b></summary>

Agentforce Architecture · Agent Types and Use Cases · Subagents · Instructions and Boundaries · Topics / Job-to-be-Done Design · Agent Actions · Reasoning and Orchestration · Human Handoff · Autonomy Levels · Agent Lifecycle
</details>

<details>
<summary><b>Agent Builder</b></summary>

Create an Agent · Configure Subagents · Natural-Language Instructions · Action Libraries · Variables and Context · Agent Configuration · Channels · Permissions · Testing Configuration · Deployment Configuration
</details>

<details>
<summary><b>Agentforce Actions</b></summary>

Flow Actions · Invocable Apex Actions · REST Apex Actions · Prompt Template Actions · MuleSoft API Actions · Standard Salesforce Actions · Custom Actions · Action Input/Output Schemas · Error Handling · Idempotent Actions
</details>

<details>
<summary><b>Prompt Builder</b></summary>

Prompt Templates · Field Generation Prompts · Record Summary Prompts · Flex Prompts · Grounding Prompts with CRM Data · Reusable Prompt Design · Prompt Versioning · Prompt Testing · Prompt Security · Prompt Output Validation
</details>

<details>
<summary><b>Salesforce AI Models</b></summary>

Salesforce-Managed Models · Model Configuration · Model Selection by Use Case · External Model Providers · OpenAI / Azure / Vertex / Bedrock Connectivity · BYOLLM · LLM Open Connector · Model API Integration · Model Latency and Cost Trade-offs · Fallback Model Strategy
</details>

<details>
<summary><b>Data 360 for AI</b></summary>

Data 360 Architecture · Data Ingestion · Data Streams · Data Model Objects · Identity Resolution · Calculated Insights · Unified Customer Profiles · Structured and Unstructured Data · Metadata and Semantic Context · Data Governance · Data Freshness · Data Permissions
</details>

<details>
<summary><b>Agentforce Grounding & RAG</b></summary>

Grounding Concepts · Agentforce Data Library · Knowledge Articles · Salesforce Fields · File Uploads · Web Sources · Retrievers · Custom Retrievers · Chunking and Indexing Concepts · Retrieval Relevance · Citation / Source Grounding · Permission-Aware Retrieval
</details>

<details>
<summary><b>Einstein Trust Layer</b></summary>

Trust Layer Architecture · Data Masking · Zero-Retention Concepts · Secure Data Handling · Prompt Injection Considerations · Toxicity and Safety Controls · Grounding · Auditability · Data Access Controls · Responsible AI
</details>

<details>
<summary><b>Salesforce Data + Metadata Context</b></summary>

Salesforce Object Model · Standard Objects · Custom Objects · Relationships · Fields and Metadata · Record Context · Permission Sets · Sharing Rules · Field-Level Security · Business Metadata for AI
</details>

<details>
<summary><b>Agent Script</b></summary>

Agent Script Fundamentals · Agent Instructions · Variables · Conditional Logic · Transitions · Actions · Subagents · State and Control Flow · Deterministic Guardrails · Advanced Agent Orchestration
</details>

<details>
<summary><b>Agentforce APIs & SDKs</b></summary>

Agentforce REST APIs · Agent Invocation · Session Handling · Authentication · API Request/Response Design · Streaming / Async Interaction · Error Handling · External Application Integration · SDK Concepts · API Security
</details>

<details>
<summary><b>Agentforce DX</b></summary>

Developer Workflow · Metadata Deployment · Source Control · CLI Tooling · Agent Configuration as Metadata · Scratch / Sandbox Workflows · CI/CD · Environment Promotion · Automated Validation · Production Deployment
</details>

<details>
<summary><b>Apex for AI</b></summary>

Apex Fundamentals · Invocable Apex · REST Apex · Apex Callouts · Governor Limits · Bulkification · Async Apex · Exception Handling · Secure Apex · Testing Apex Actions
</details>

<details>
<summary><b>Flow for AI</b></summary>

Autolaunched Flows · Screen Flows · Flow Actions · Variables and Collections · Decision Logic · Invocable Actions · Error Paths · Flow Testing · Flow Limits · Flow + Agentforce Orchestration
</details>

<details>
<summary><b>MuleSoft + Agentforce</b></summary>

MuleSoft APIs · API-Led Connectivity · Connectors · Agent Fabric · Third-Party System Actions · ERP Integration · SaaS Integration · API Governance · Agent-to-System Security · Cross-Enterprise Orchestration
</details>

<details>
<summary><b>Agent-to-Agent Architecture</b></summary>

A2A Concepts · Agent Discovery · Agent Communication · Delegation · Agent Identity · Agent Permissions · Multi-Agent Orchestration · Agent Interoperability · Failure Handling · Governed Agent Networks
</details>

<details>
<summary><b>Agentforce Testing & Evaluation</b></summary>

Testing Strategy · Testing Center · Test Cases · Expected Outcomes · Golden Test Sets · Regression Testing · Action Accuracy · Grounding Accuracy · Safety Testing · Production Feedback Loops
</details>

<details>
<summary><b>Agent Analytics & Monitoring</b></summary>

Agent Analytics · Session Tracing · Conversation Traces · Action Traces · Latency Monitoring · Error Monitoring · Token / Usage Monitoring · Quality Monitoring · Einstein Audit Data · Feedback Analysis
</details>

<details>
<summary><b>Salesforce AI Security</b></summary>

Least Privilege · Permission Sets · Sharing and Visibility · Field-Level Security · Data Access Boundaries · Prompt Injection Defense · Unsafe Tool-Use Defense · Sensitive Data Handling · Audit Logs · Security Review
</details>

<details>
<summary><b>Salesforce AI Governance</b></summary>

AI Use-Case Governance · Risk Assessment · Human Oversight · Data Governance · Model Governance · Prompt Governance · Access Reviews · Retention · Compliance Evidence · Responsible AI
</details>

<details>
<summary><b>CRM AI Use Cases</b></summary>

Sales AI · Service AI · Marketing AI · Commerce AI · Slack AI · Employee Agents · Customer Service Agents · Sales Development Agents · Case Summarization · Email and Content Generation
</details>

<details>
<summary><b>Salesforce AI Solution Architecture</b></summary>

Business Problem to AI Use Case · Data Readiness Assessment · Agent Boundary Design · Grounding Architecture · Action Architecture · Integration Architecture · Security Architecture · Evaluation Architecture · Deployment Architecture · Business KPI Architecture
</details>

<details>
<summary><b>Salesforce AI FDE Skills</b></summary>

Customer Discovery for Agentforce · AI Use-Case Qualification · Org Assessment · Data Readiness Workshops · Agent Design Workshops · POC Planning · Production Readiness Reviews · Security Stakeholder Alignment · Executive Demos · ROI and Adoption Measurement
</details>

---

## 10. Salesforce AI — Tools & Platform In Depth

<details>
<summary><b>Salesforce Platform & Developer Tools</b></summary>

Salesforce Org · Objects & Fields · Metadata · SOQL · SOSL · Apex · Flow · Lightning Web Components · Salesforce CLI · Salesforce DX · Developer Console
</details>

<details>
<summary><b>Agentforce</b></summary>

Agentforce 360 · Agentforce Builder · Agents · Subagents · Topics · Instructions · Actions · Reasoning · Agent Lifecycle · Agent Deployment
</details>

<details>
<summary><b>Agentforce Builder</b></summary>

Agent Configuration · Subagent Configuration · Natural-Language Instructions · Action Libraries · Variables · Context · Channels · Permissions · Testing · Deployment
</details>

<details>
<summary><b>Agentforce Actions</b></summary>

Apex Actions · Invocable Apex · Apex REST · MuleSoft Actions · Action Inputs · Action Outputs · Idempotency
</details>

<details>
<summary><b>Prompt Builder</b></summary>

Field Generation · Record Summaries · Prompt Variables · CRM Grounding · Reusable Prompts · Versioning · Testing · Output Validation
</details>

<details>
<summary><b>Salesforce AI Models</b></summary>

Model Selection · Models API · External Models · OpenAI · Azure OpenAI · Google Vertex AI · Amazon Bedrock · Model Routing · Fallback Models
</details>

<details>
<summary><b>Data 360</b></summary>

Data 360 Architecture · Data Streams · Data Ingestion · Data Model Objects · Identity Resolution · Unified Customer Profile · Calculated Insights · Data Graphs · Structured Data · Unstructured Data · Data Freshness · Data Governance
</details>

<details>
<summary><b>Agentforce Data & RAG</b></summary>

Data Libraries · Salesforce Knowledge · Files · Documents · Web Sources · Retrievers · Custom Retrievers · Chunking · Indexing · Embeddings · Semantic Search · Hybrid Retrieval · Grounding · Citations · Permission-Aware Retrieval
</details>

<details>
<summary><b>Einstein Trust Layer</b></summary>

Dynamic Grounding · Zero Data Retention · Toxicity Detection · Prompt Injection Controls · Privacy Controls
</details>

<details>
<summary><b>MCP — Model Context Protocol</b></summary>

MCP Fundamentals · MCP Client · MCP Server · MCP Tools · MCP Resources · MCP Prompts · MCP Tool Actions · MCP Security · MCP Testing · MCP Observability
</details>

<details>
<summary><b>Salesforce MCP & Agentforce</b></summary>

Agentforce MCP Client · Agentforce Registry · API Catalog · Third-Party MCP Servers · Salesforce-Hosted MCP Servers · MuleSoft MCP · Agentforce Gateway · MCP Policies · Tool Allowlisting · MCP Response Schemas · External AI → Salesforce Agent · Salesforce Agent → External Tool
</details>

<details>
<summary><b>Salesforce Hosted MCP Servers</b></summary>

Custom MCP Server · Apex → MCP Tool · Flow → MCP Tool · Apex REST → MCP Tool · AuraEnabled → MCP Tool · Named Query → MCP Tool · Prompt Builder → MCP · Agentforce Agent → MCP Tool · API Catalog → MCP Tool
</details>

<details>
<summary><b>Agent Script</b></summary>

Instructions · Conditions · Deterministic Logic · Agentic Reasoning · State Management · Guardrails
</details>

<details>
<summary><b>Agent-to-Agent / A2A</b></summary>

A2A Fundamentals · Agent Discovery · Agent Delegation · Agent Communication · Multi-Agent Orchestration · Agent Identity · Agent Permissions · Agent Governance · Failure Handling
</details>

<details>
<summary><b>Apex for AI</b></summary>

InvocableMethod · Apex Actions · Apex REST · HTTP Callouts · Named Credentials · External Credentials · Apex Testing
</details>

<details>
<summary><b>Flow for AI</b></summary>

Record-Triggered Flow · Autolaunched Flow · Screen Flow · Loops · Agent + Flow Orchestration
</details>

<details>
<summary><b>MuleSoft + Agentforce</b></summary>

Anypoint Platform · API Catalog · Mule Flows · Agentforce Integration · MCP · External APIs
</details>

<details>
<summary><b>Salesforce APIs</b></summary>

REST API · SOAP API · Bulk API · Composite API · GraphQL · Metadata API · Tooling API · Connect REST API · OAuth · Named Credentials · External Credentials · API Versioning
</details>

<details>
<summary><b>Salesforce Security</b></summary>

Profiles · Permission Sets · Permission Set Groups · Roles · Sharing Rules · Organization-Wide Defaults · Field-Level Security · Object Permissions · Record-Level Security · Connected Apps · OAuth · Named Credentials · External Credentials · Audit Logs
</details>

<details>
<summary><b>Agentforce Testing & Evaluation</b></summary>

Golden Datasets · Action Testing · Grounding Testing · Agent Trajectory · Quality Metrics · Production Feedback
</details>

<details>
<summary><b>Agentforce Observability</b></summary>

Agent Tracing · Session Tracing · Action Tracing · Agent Analytics · Error Monitoring · Latency Monitoring · Token Usage · Cost Monitoring · Quality Monitoring · User Feedback
</details>

<details>
<summary><b>Salesforce AI Deployment</b></summary>

Sandbox · Developer Org · Scratch Org · Metadata Deployment · Salesforce CLI · Git · CI/CD · Change Sets · Environment Promotion · Production Release · Rollback
</details>

<details>
<summary><b>Salesforce AI Architecture</b></summary>

Business Problem → AI Use Case · Data Readiness · Agent Boundaries · Grounding Architecture · Action Architecture · Integration Architecture · Security Architecture · Evaluation Architecture · Deployment Architecture · KPI Architecture
</details>

<details>
<summary><b>Salesforce AI FDE Skills</b></summary>

Customer Discovery · Salesforce Org Assessment · Data Readiness Workshop · Agent Design Workshop · Architecture Workshop · Integration Design · Security Review · POC · Pilot · Production Readiness · Deployment · User Training · Adoption · KPI Measurement · ROI
</details>

---

