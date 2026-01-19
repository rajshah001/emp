# PRD: Engineering-as-Marketing Tool Recommendation Engine (POC)

## 1. Product Overview

### Product Name (Working)

**EaaM Tool Advisor (POC)**

### Purpose

Help founders and product teams identify **high-leverage free tools** they can build (Engineering as Marketing) to drive **organic, high-intent traffic**, based solely on their **website URL**.

This POC focuses **only on recommendations**, not tool generation or SEO execution.

---

## 2. Problem Statement

Founders know that "free tools" can drive organic traffic, but they struggle with:

* Knowing **which tools to build**
* Ensuring tools are **relevant to their product**
* Avoiding low-intent or vanity traffic
* Bridging tools to their paid product

Existing SEO and marketing tools provide **data**, not **decisions**.

---

## 3. Goals & Non-Goals

### Goals (POC)

* Accept a website URL as input
* Analyze website positioning and product intent
* Recommend **3–5 free tool ideas**
* Clearly explain *why* each tool fits
* Demonstrate higher usefulness than generic SEO audits

### Non-Goals (Explicitly Out of Scope)

* Building the tools themselves
* Keyword volume accuracy (Ahrefs-level)
* User authentication
* Payments
* Backend persistence
* Crawling entire websites

---

## 4. Target User (ICP for POC)

**Primary ICP**

* B2B SaaS founders
* DevTools / Infra / Cloud / Security products
* Technical or semi-technical teams
* Interested in organic growth, not ads

---

## 5. High-Level User Flow

```
User enters website URL
        ↓
Frontend fetches website content
        ↓
Structured data extraction
        ↓
Rule-based classification
        ↓
LLM-assisted reasoning & expansion
        ↓
Ranked tool recommendations
```

All steps are executed **client-side (ReactJS)** in the POC.

---

## 6. System Architecture (POC)

### Frontend-Only Architecture

* ReactJS application
* Direct API calls to:
  * Website fetch (via CORS-safe proxy if needed)
  * GLM / Claude API
* No backend required for POC

### Why frontend-only is acceptable

* Input is public website data
* No sensitive user data
* Faster iteration
* Lower complexity

---

## 7. Functional Requirements

### 7.1 Website Data Extraction

**Input**

* Website URL

**Extract (minimum)**

* Meta title
* Meta description
* H1 text
* Hero section text
* Feature section text (best effort)
* Pricing page text (if accessible)

**Output (Structured JSON)**

```json
{
  "meta_title": "",
  "meta_description": "",
  "headings": [],
  "hero_text": "",
  "feature_text": "",
  "pricing_present": true
}
```

---

### 7.2 Product & ICP Classification (LLM-assisted)

**Objective**

Convert raw website text into a structured **Product Profile**.

**LLM Role**

* Classification only
* No recommendations at this stage

**Output Schema**

```json
{
  "product_category": "",
  "target_users": [],
  "core_problems": [],
  "inputs": [],
  "outcomes": [],
  "buying_intent": "low | medium | high"
}
```

---

### 7.3 Tool Archetype Library (Rule-Based)

Maintain a **static internal dataset** of tool archetypes.

**Example Archetypes**

* Diagnostic / Grader
* Gap Analyzer
* Estimator / Calculator
* Validator / Checker

**Example Archetype Schema**

```json
{
  "archetype": "Estimator",
  "intent_type": "estimate",
  "required_signals": ["cost", "usage", "pricing"],
  "works_for_categories": ["cloud", "infra"],
  "bridge_pattern": "Estimation → Optimization"
}
```

---

### 7.4 Tool Matching Logic (Deterministic)

**Matching Rules**

* Product inputs overlap with archetype requirements
* Product category matches archetype category
* Buying intent is medium or high

**Output**

List of **eligible archetypes** for the website.

---

### 7.5 Tool Idea Generation (LLM-assisted)

Now LLM is used to **instantiate tool ideas**, not decide strategy.

**Prompt Inputs**

* Product Profile (structured)
* Matched Tool Archetypes
* Strict output schema

**Output Schema**

```json
{
  "tool_name": "",
  "tool_type": "",
  "free_value": "",
  "gap": "",
  "cta": "",
  "reasoning": ""
}
```

---

### 7.6 Opportunity Scoring & Ranking

Simple heuristic scoring.

**POC Scoring Factors**

* Product relevance
* Intent strength
* Archetype fit
* Build simplicity

**Output**

Top **3–5 ranked tools**.

---

## 8. LLM Prompt Design (Critical)

### Prompt Principles

* Provide **structured inputs**
* Enforce **JSON-only output**
* Use LLM for *reasoning*, not guessing
* Never ask: "What should they build?" without constraints

### Prompt Sections

1. Website Summary
2. Product Profile
3. Allowed Tool Archetypes
4. Output Schema

---

## 9. User Interface Requirements

### Input Screen

* Single input: Website URL
* "Analyze Website" CTA

### Output Screen

For each recommended tool:

* Tool name
* Tool type (archetype)
* Why this tool fits their product
* What problem it exposes
* How it leads to their paid product

**Important UX Copy**

> "These are recommendations based on your website positioning and common user intent patterns."

---

## 10. Success Metrics (POC)

### Qualitative

* "This makes sense for my product"
* "These are better ideas than SEO tools give me"
* "I would actually build one of these"

### Quantitative (Optional)

* Time spent reviewing recommendations
* Clicks on "Why this tool?"

---

## 11. Key Design Constraints

* No crawling entire sites
* No hard dependency on Ahrefs
* No auto-publishing
* No authentication
* No backend persistence

---

## 12. Future Extensions (Explicitly Not Now)

* Ahrefs / GSC integration
* Tool generation
* Embeddable widgets
* Backend recommendation engine
* Tool ROI tracking

---

## 13. Core Product Philosophy (for Claude)

> This product does **not invent marketing ideas**.
> It **maps proven engineering-led tool patterns to a specific website using structured logic and constrained LLM reasoning**.

---

## 14. One-Line Summary (for Claude context)

> "Build a frontend-only POC that takes a website URL, extracts positioning signals, classifies the product, matches it against known engineering-as-marketing tool archetypes, and outputs ranked tool recommendations with clear reasoning."
