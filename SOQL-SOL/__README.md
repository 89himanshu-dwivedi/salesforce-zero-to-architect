# SOQL & SOSL: Zero to Advanced

Hi, I'm **Himanshu Kumar** — Technical Lead & Salesforce Solution Architect.

This repository is my structured learning path for **SOQL, SOSL, query performance, data access, and large-data-volume query design** in Salesforce.

The goal is not just to learn query syntax. The goal is to understand how to write queries correctly, securely, efficiently, and at scale.

---

## Start here — SOQL & SOSL

**6 sub-topic groups · 55 topics**

Open a group to read every topic in order.

The curriculum progresses from:

```text
SOQL Fundamentals
        ↓
Relationship Queries
        ↓
Aggregate Queries
        ↓
Advanced SOQL
        ↓
SOSL Fundamentals
        ↓
Query Performance
```

---

# 📚 Curriculum

## 01 — SOQL Fundamentals · 8 topics

The foundation of Salesforce Object Query Language.

- [SELECT FROM WHERE](01-soql-fundamentals.html#select-from-where)
- [ORDER BY LIMIT OFFSET](01-soql-fundamentals.html#order-by-limit-offset)
- [NULLS FIRST / LAST](01-soql-fundamentals.html#nulls-first-last)
- [= != < > <= >=](01-soql-fundamentals.html#item)
- [LIKE IN NOT IN](01-soql-fundamentals.html#like-in-not-in)
- [INCLUDES EXCLUDES](01-soql-fundamentals.html#includes-excludes)
- [AND OR NOT](01-soql-fundamentals.html#and-or-not)
- [Date Literals (TODAY, LAST_WEEK, etc.)](01-soql-fundamentals.html#date-literals-today-last-week-etc)

---

## 02 — Relationship Queries · 5 topics

Understanding how Salesforce relationships are queried.

- [Child to Parent Dot Notation](02-relationship-queries.html#child-to-parent-dot-notation)
- [Multi Level Parent Query](02-relationship-queries.html#multi-level-parent-query)
- [Parent to Child Sub Query](02-relationship-queries.html#parent-to-child-sub-query)
- [Nested Relationship Query](02-relationship-queries.html#nested-relationship-query)
- [Polymorphic WhoId WhatId](02-relationship-queries.html#polymorphic-whoid-whatid)

---

## 03 — Aggregate Queries · 9 topics

Working with grouped and summarized Salesforce data.

- [COUNT()](03-aggregate-queries.html#count)
- [COUNT_DISTINCT()](03-aggregate-queries.html#count-distinct)
- [SUM()](03-aggregate-queries.html#sum)
- [AVG()](03-aggregate-queries.html#avg)
- [MAX() MIN()](03-aggregate-queries.html#max-min)
- [GROUP BY](03-aggregate-queries.html#group-by)
- [HAVING](03-aggregate-queries.html#having)
- [ROLLUP](03-aggregate-queries.html#rollup)
- [CUBE](03-aggregate-queries.html#cube)

---

## 04 — Advanced SOQL · 17 topics

Query optimization, indexing, security, and dynamic query techniques.

- [Query Selectivity](04-advanced-soql.html#query-selectivity)
- [Query Cost](04-advanced-soql.html#query-cost)
- [Query Plan Tool](04-advanced-soql.html#query-plan-tool)
- [Explain Plan](04-advanced-soql.html#explain-plan)
- [Standard Index](04-advanced-soql.html#standard-index)
- [Custom Index](04-advanced-soql.html#custom-index)
- [External Id Index](04-advanced-soql.html#external-id-index)
- [Unique Index](04-advanced-soql.html#unique-index)
- [Compound Index](04-advanced-soql.html#compound-index)
- [Skinny Tables](04-advanced-soql.html#skinny-tables)
- [Divisions](04-advanced-soql.html#divisions)
- [WITH SECURITY_ENFORCED](04-advanced-soql.html#with-security-enforced)
- [USER_MODE](04-advanced-soql.html#user-mode)
- [stripInaccessible](04-advanced-soql.html#stripinaccessible)
- [Database.query()](04-advanced-soql.html#database-query)
- [Bind Variables](04-advanced-soql.html#bind-variables)
- [Dynamic Filters](04-advanced-soql.html#dynamic-filters)

---

## 05 — SOSL Fundamentals · 9 topics

Understanding Salesforce Object Search Language and multi-object search.

- [FIND RETURNING LIMIT](05-sosl-fundamentals.html#find-returning-limit)
- [ALL FIELDS](05-sosl-fundamentals.html#all-fields)
- [NAME FIELDS](05-sosl-fundamentals.html#name-fields)
- [EMAIL FIELDS](05-sosl-fundamentals.html#email-fields)
- [PHONE FIELDS](05-sosl-fundamentals.html#phone-fields)
- [Wildcards](05-sosl-fundamentals.html#wildcards)
- [Fuzzy Matching](05-sosl-fundamentals.html#fuzzy-matching)
- [Exact Search](05-sosl-fundamentals.html#exact-search)
- [Multi Object Search](05-sosl-fundamentals.html#multi-object-search)

---

## 06 — Query Performance · 7 topics

Designing efficient queries and working with large data volumes.

- [Governor Limits](06-query-performance.html#governor-limits)
- [SOQL For Loops](06-query-performance.html#soql-for-loops)
- [Bulk Queries](06-query-performance.html#bulk-queries)
- [Pagination](06-query-performance.html#pagination)
- [OFFSET Limitation](06-query-performance.html#offset-limitation)
- [Keyset Pagination](06-query-performance.html#keyset-pagination)
- [LDV Search Strategies](06-query-performance.html#ldv-search-strategies)

---

# 🧠 How This Curriculum Is Structured

Every topic is intended to go beyond query syntax.

The learning flow is:

```text
Concept
   ↓
Simple
   ↓
Advanced
   ↓
Super Advanced
   ↓
Practical
   ↓
Interview
   ↓
Errors & Gotchas
   ↓
Limits
   ↓
Best Option & Trade-offs
   ↓
Real World
```

The objective is to understand **why a query works, how Salesforce executes it, and how the design behaves as data volume grows**.

---

# 🔎 SOQL vs SOSL Thinking

A key goal of this repository is to understand the difference between the two query approaches.

```text
SOQL
 ↓
Structured query against Salesforce objects
 ↓
Known object / relationship
 ↓
Precise filtering and retrieval
```

versus:

```text
SOSL
 ↓
Search across Salesforce data
 ↓
Multiple objects
 ↓
Text-oriented search
```

The focus is not memorizing syntax.

It is learning **which query approach fits the requirement**.

---

# 🏗️ Relationship Query Thinking

Salesforce data is relational.

The curriculum builds from:

```text
Child
  ↓
Parent
  ↓
Parent of Parent
```

and:

```text
Parent
  ↓
Children
  ↓
Nested Relationship Data
```

The goal is to understand how relationship queries affect:

- Data retrieval
- Query structure
- Result handling
- Performance
- Maintainability

---

# 📊 Aggregate Query Thinking

Aggregate queries move beyond retrieving individual records.

```text
Records
   ↓
Aggregate Function
   ↓
GROUP BY
   ↓
HAVING
   ↓
Summary Result
```

The curriculum covers:

- COUNT()
- COUNT_DISTINCT()
- SUM()
- AVG()
- MAX()
- MIN()
- GROUP BY
- HAVING
- ROLLUP
- CUBE

The objective is to understand when the database should perform the aggregation instead of retrieving large record sets into Apex.

---

# ⚡ Advanced SOQL Thinking

Advanced SOQL is where query writing becomes architecture.

The curriculum covers:

```text
Query
 ↓
Selectivity
 ↓
Query Cost
 ↓
Query Plan
 ↓
Indexes
 ↓
Security
 ↓
Dynamic Query
 ↓
Performance
```

Important areas include:

- Query Selectivity
- Query Cost
- Query Plan Tool
- Explain Plan
- Standard Index
- Custom Index
- External Id Index
- Unique Index
- Compound Index
- Skinny Tables
- Divisions
- `WITH SECURITY_ENFORCED`
- `USER_MODE`
- `stripInaccessible`
- `Database.query()`
- Bind Variables
- Dynamic Filters

---

# 🔐 Secure Query Thinking

Query design is also a security concern.

The learning path includes:

```text
Query
  ↓
Data Access
  ↓
Object / Field Security
  ↓
User Context
  ↓
Secure Result
```

Security-related topics in this curriculum include:

- `WITH SECURITY_ENFORCED`
- `USER_MODE`
- `stripInaccessible`

The goal is to understand how data access should be considered while designing Apex queries.

---

# 🚀 Query Performance

Query performance becomes critical as data volume increases.

The progression is:

```text
Basic Query
    ↓
Selective Query
    ↓
Indexed Query
    ↓
Query Plan
    ↓
Efficient Retrieval
    ↓
Pagination
    ↓
Keyset Pagination
    ↓
Large Data Volume Strategy
```

The performance section covers:

- Governor Limits
- SOQL For Loops
- Bulk Queries
- Pagination
- OFFSET Limitation
- Keyset Pagination
- LDV Search Strategies

---

# 📦 Large Data Volume Thinking

A query that works with:

```text
10,000 records
```

may behave very differently with:

```text
10 million records
```

The architect mindset is therefore:

```text
Data Volume
     ↓
Access Pattern
     ↓
Selectivity
     ↓
Indexing
     ↓
Query Plan
     ↓
Pagination
     ↓
Performance
```

The objective is to design queries with production-scale data in mind.

---

# 🎤 Interview Preparation

Every major topic should eventually support questions such as:

### Basic

- What is SOQL?
- What is SOSL?
- What is the difference between SOQL and SOSL?
- What does `WHERE` do?
- What does `GROUP BY` do?
- What does `HAVING` do?

### Advanced

- What makes a query selective?
- What is the Query Plan Tool?
- What is an index?
- What is the difference between standard and custom indexes?
- What is keyset pagination?
- Why does OFFSET have limitations?
- When would you use SOSL instead of SOQL?

### Security

- What is `WITH SECURITY_ENFORCED`?
- What is `USER_MODE`?
- What is `stripInaccessible`?
- How should Apex query security be designed?

### Architect

- How would you design search for millions of Salesforce records?
- How would you investigate a slow SOQL query?
- How would you design pagination for a large dataset?
- How would you handle LDV?
- How would you decide between SOQL and SOSL?
- How would indexing influence your architecture?

---

# 🧩 Real-World Query Architecture

A production query should be thought of as part of a larger system:

```text
Business Requirement
        ↓
Access Pattern
        ↓
SOQL / SOSL
        ↓
Security
        ↓
Selectivity
        ↓
Index / Query Plan
        ↓
Governor Limits
        ↓
Pagination / Bulk Processing
        ↓
Application Logic
        ↓
User / Integration
```

This is the difference between:

```text
"I know SOQL"
```

and:

```text
"I can design Salesforce data access for production."
```

---

# 📈 Definition of Done

A topic is considered **COMPLETE** when we have:

- [ ] Concept
- [ ] Simple explanation
- [ ] Advanced explanation
- [ ] Super Advanced explanation
- [ ] Syntax / examples
- [ ] Practical use case
- [ ] Errors & Gotchas
- [ ] Limits / constraints
- [ ] Query performance considerations
- [ ] Security considerations
- [ ] Scalability considerations
- [ ] Trade-offs
- [ ] Real-world scenario
- [ ] Interview questions
- [ ] Architecture perspective

---

# 📊 Current Curriculum

```text
SOQL & SOSL
│
├── 01 SOQL Fundamentals ........  8 topics
├── 02 Relationship Queries .....  5 topics
├── 03 Aggregate Queries ........  9 topics
├── 04 Advanced SOQL ............ 17 topics
├── 05 SOSL Fundamentals ........  9 topics
└── 06 Query Performance ........  7 topics
                                  ─────────
                                   55 topics
```

---

# 🏁 Final Goal

The target is:

```text
SOQL Fundamentals
       +
Relationship Queries
       +
Aggregate Queries
       +
Advanced SOQL
       +
SOSL
       +
Query Performance
       ↓
🔥 STRONG SALESFORCE QUERY FOUNDATION
       ↓
Selective Queries
       ↓
Secure Queries
       ↓
Bulk & LDV Design
       ↓
Enterprise Data Access
       ↓
Salesforce Architecture
```

---

## Repository Navigation

[← Developer Foundations](../05-developer-foundations/index.html)

[Apex Fundamentals →](../07-apex-fundamentals/index.html)

---

## ⭐ Repository Principle

> **Don't just write queries that return data. Write queries that are secure, selective, scalable, governor-limit aware, and appropriate for the production data volume.**

This repository will continue to expand from SOQL/SOSL fundamentals into advanced Salesforce data-access and large-data-volume architecture patterns.
