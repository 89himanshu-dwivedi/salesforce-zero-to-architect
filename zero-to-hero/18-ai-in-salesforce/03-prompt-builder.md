[Home](../index.md) / [18 · AI in Salesforce](index.md) / **Prompt Builder**

# Prompt Builder

4 topics · Series 18: AI in Salesforce

**Topics on this page**

- [Prompt Templates](#prompt-templates)
- [Grounding](#grounding)
- [Dynamic Prompting](#dynamic-prompting)
- [Prompt Testing](#prompt-testing)

## Prompt Templates

*Reusable, grounded prompts surfaced across Salesforce.*

### 🌱 Simple

*Beginner - plain language*

**Prompt Templates** in Prompt Builder are **reusable prompts** with merge fields, grounded in Salesforce data, that generate content (emails, summaries, field values) — surfaced on records, fields, and flows without code.

### 📏 Limits

*Governor & platform limits*

- Types: Field Generation/Record Summary/Email/Flex; merge-field grounding.
- Surfaced on pages/fields/flows/agents; Trust Layer; model choice.

> **Want the deeper material for this topic?**
> Advanced and architect-level depth, real-world scenarios, gotchas and interview questions
> are not published here. Connect with me - **LinkedIn · X · GitHub** - details on the
> [home page](../index.md).

## Grounding

*Inject real data so the LLM answers accurately.*

### 🌱 Simple

*Beginner - plain language*

**Grounding** means feeding the LLM **real, relevant data** (record fields, related records, retrieved knowledge) so its output is **accurate and specific** rather than generic or hallucinated.

### 📏 Limits

*Governor & platform limits*

- Sources: merge fields, Flow/Apex, RAG retrieval; runtime + current.
- Masked via Trust Layer; token-budget aware; quality-dependent.

> **Want the deeper material for this topic?**
> Advanced and architect-level depth, real-world scenarios, gotchas and interview questions
> are not published here. Connect with me - **LinkedIn · X · GitHub** - details on the
> [home page](../index.md).

## Dynamic Prompting

*Adapt prompt content to context at runtime.*

### 🌱 Simple

*Beginner - plain language*

**Dynamic prompting** builds the prompt **conditionally at runtime** — including different data or instructions based on the record, user, or situation — so one template adapts to many contexts.

### 📏 Limits

*Governor & platform limits*

- Flow/Apex + retrieval assemble conditional prompts at runtime.
- Context-aware grounding/instructions; one template vs many; test each path.

> **Want the deeper material for this topic?**
> Advanced and architect-level depth, real-world scenarios, gotchas and interview questions
> are not published here. Connect with me - **LinkedIn · X · GitHub** - details on the
> [home page](../index.md).

## Prompt Testing

*Validate prompt quality, safety, and consistency.*

### 🌱 Simple

*Beginner - plain language*

**Prompt testing** validates that a prompt produces **accurate, safe, consistent** output before deployment — previewing against real records and checking edge cases, tone, and hallucination.

### 📏 Limits

*Governor & platform limits*

- Preview real + edge records; check accuracy/tone/toxicity/consistency.
- Compare models; verify Trust Layer; regression-test on change.

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
