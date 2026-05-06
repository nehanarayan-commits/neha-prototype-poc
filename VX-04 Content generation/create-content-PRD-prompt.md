# Product Requirements Document
## Feature: Create Content — AI Commerce Visibility (PP Portal)

**Feature ref:** VX-04  
**Status:** Prototype complete  
**Author:** Neha Narayan  
**Last updated:** 6 May 2026

---

## 1. Overview

The **Create Content** feature enables brand users to generate AI-optimised content (blog posts, listicles, comparison articles, etc.) that improves their visibility in AI search model responses (ChatGPT, Gemini, Perplexity, Claude, Google AI).

The workflow is a 6-step modal launched from the Content Library: configure targeting → select format → configure brand resources → analyse prompts → review citations → generate and approve content. Generated content is saved to the Content Library.

---

## 2. User Stories

| ID | As a… | I want to… | So that… |
|---|---|---|---|
| US-01 | Brand manager | Configure targeting (category, market, persona, LLM) and content format before generating | The generated content is tailored to my audience and channel |
| US-02 | Brand manager | See which AI search prompts have the largest visibility gap | I can prioritise the prompts most worth targeting |
| US-03 | Brand manager | Select specific prompts to target | I can focus content on the highest-opportunity gaps |
| US-04 | Brand manager | See which sources are cited for those prompts across LLMs | I understand what content my competitors publish that I lack |
| US-05 | Brand manager | Select up to 5 citations to include | I can control which competitive signals inform my content |
| US-06 | Brand manager | Generate a draft article from the selected citations | I get a ready-to-edit starting point grounded in citation data |
| US-07 | Brand manager | Refine the generated content | I can adjust tone, length, and structure without regenerating |
| US-08 | Brand manager | Approve content and save it to the library | My team can review and track all generated pieces |
| US-09 | Brand manager | Publish an approved article | I can record that a piece has gone live |
| US-10 | Brand manager | View all saved content in a library table | I have a record of what has been created and its status |

---

## 3. Navigation & Page Structure

The feature lives within the PP Portal under **AI Commerce Visibility**. The persistent shell contains:

- **Topbar** (always visible): PP logomark, product name, AI-generated beta badge, user avatar
- **Nav tabs**: Insights | Content library (default active)

There is no "Create content" nav tab. The entry point to content generation is the **"+ Generate content"** primary button in the Content Library header.

---

## 4. Feature Requirements

### 4.1 Content Library (Main View)

The default view on page load. Displays all saved articles in a **table layout**.

**Header:**
- "Content library" title with article count
- "**+ Generate content**" primary CTA — opens the Generate modal

**Filter pills:** All | Approved | Published

**Table columns:** Title (with category/market tags below) | Format | Language | LLMs | Status | Date

**Row behaviour:**
- Clicking anywhere on a row opens the Article Detail view
- No separate "Open" button on rows
- Rows show pointer cursor on hover

---

### 4.2 Generate Content Modal (6-Step Workflow)

A centred modal (max-width 1040px, height 80vh) with a persistent step indicator bar.

**Step order:** Targeting → Format → Brand → Prompts → Citations → Review

Completed steps show a green checkmark. Current step is highlighted coral.

---

#### Step 1 — Targeting

Multi-search selector fields for:

| Field | Type | Options |
|---|---|---|
| Product category | Multi-select | Running Shoes, Coffee Pods, Sportswear, Outdoor Gear |
| Market | Multi-select | United States, Germany, France, United Kingdom, Netherlands |
| Persona | Multi-select | Marathon Trainer, Casual Runner, Performance Athlete, Beginner Runner |
| Target LLM | Multi-select | ChatGPT (35%), Gemini (20%), Claude (20%), Perplexity (15%), Google AI (10%) |

**Footer:** "Next →" (disabled until at least one product category is selected)

---

#### Step 2 — Format

Single-select native `<select>` fields for:

| Field | Options |
|---|---|
| Content format | Blog post, Listicle, Comparison article, Social post, Email, Product description |
| Language | English, German, French, Spanish, Dutch, Italian |

No pills shown after selection. Format and language are optional at this step.

**Footer:** "Next →" (always enabled)

---

#### Step 3 — Brand

Multi-search selector fields for:

| Field | Options |
|---|---|
| Brand kit | Nespresso Brand Kit v2.1, Zalora Brand Kit 2025, iHerb Brand Guidelines, Decathlon Style Guide |
| Knowledge base | Product catalog 2025, Marketing materials Q4, Custom URL set, PIM export — all SKUs |

No upload option. Selection only.

**Footer:** "Next →" (always enabled)

---

#### Step 4 — Prompts

Triggered when the user clicks "Next" from Brand. A brief loading animation ("Analysing prompts…") runs for ~0.8 seconds, then the prompt list renders.

Displays a ranked list of AI search prompts for the selected categories, sorted by visibility gap (largest first).

**Each row shows:**
- Checkbox (multi-select)
- Prompt text
- Two mini horizontal bar charts: user's current visibility % vs competitive average %
- Gap badge (−Xpp) colour-coded: ≥30pp red · 15–29pp amber · <15pp green

**Header controls:** Select all / Deselect all

**Footer:**
- Count of selected prompts
- "View citations →" CTA (disabled until ≥1 prompt selected)

---

#### Step 5 — Citations

Triggered when the user clicks "View citations".

**Opportunity banner** at top: "Your brand appears in X of Y citations across N prompts."

**Citation groups** — one accordion per selected prompt, all expanded by default:
- Group header: prompt text, "X/Y selected" count, collapse chevron
- Each citation item: checkbox, source domain with letter avatar, LLM badges, excerpt snippet, "Your brand not cited" flag where applicable

**Selection behaviour:**
- First 5 citations pre-selected when the view loads
- **Maximum 5 citations** can be selected at any time — attempting to select a 6th shows a toast
- "Deselect all" button clears selection

**Footer:**
- "← Back" button
- "X / 5 citations selected" count
- "Deselect all" button
- "Generate content →" CTA (disabled at 0 selected)

---

#### Step 6 — Review

Triggered by "Generate content". A loading animation ("Generating content…") runs for ~1.2 seconds, then the review view renders. **2 credits are deducted** and a toast confirms ("2 credits used").

**3-column layout:**

| Column | Width | Contents |
|---|---|---|
| Brief | 220px | AI-generated strategy brief explaining citation gaps and content approach |
| Content | flex (fills remaining space) | Editable textarea with generated article + refine controls |
| Inputs | 196px | Read-only metadata summary of all configured inputs |

**Content column contains:**
- Editable textarea (min-height 300px)
- Quick-refine pills: More concise · More technical · Stronger CTA · Optimise for AI citations (each costs 1 credit; toast confirms usage)
- Custom refine input + "Refine" button (costs 1 credit)

**Inputs column (read-only metadata):**
- Format, Language, Categories, Markets, Personas, LLMs, Brand kit, Knowledge base, Prompts (count), Citations (count)
- Displayed as labelled metadata rows — not interactive

**Footer:** "Approve & save" CTA

---

#### Approve & Save

Triggered by "Approve & save":
- Saves the article to the Content Library with status: `approved`
- Modal closes
- Library table refreshes
- Toast: "Content saved to library."

**Article record fields:** title (extracted from first heading), format, categories, markets, personas, LLMs, language, brief, full body, date, status, versions[]

---

### 4.3 Article Detail View

Opened by clicking any row in the Content Library table.

**Header:** Back link, article title, status badge, "Mark as published" or "✓ Published" indicator, "Edit content" button

**3-column layout (matches Review page):**

| Column | Width | Contents |
|---|---|---|
| Brief | 220px | Article strategy brief |
| Content | flex | Rendered article (markdown) or editable textarea in edit mode + refine controls |
| Right panel | 220px | Inputs summary (read-only metadata) + Version history |

**Edit mode** (toggled by "Edit content"):
- Textarea replaces rendered content
- Refine pills and custom refine input appear
- "Save as new version" CTA saves edit as a new version entry

**Version history** (in right panel):
- Lists all versions in reverse chronological order
- Each entry: version number, date, note, "Restore" button (non-current versions)
- Restoring creates a new version entry

---

## 5. Status System

| Status | Meaning | Badge colour |
|---|---|---|
| Approved | Reviewed and saved; not yet live | Green: `#E1F5EE` / `#085041` |
| Published | Manually marked as live | Blue: `#EBF3FF` / `#0052CC` |

Status transitions: generated (in textarea) → approved (on "Approve & save") → published (on "Mark as published"). No automated publishing.

---

## 6. Credits System

| Action | Cost |
|---|---|
| Generate content | 2 credits |
| Quick-refine pill | 1 credit |
| Custom refine | 1 credit |

Credits are deducted immediately on action. A **toast notification** is the only feedback ("X credit(s) used"). Credit balance and cost labels are **not** shown in the UI — no balance display in topbar, buttons, or pills.

---

## 7. LLM Brand Colours (Reference)

| Model | Background | Text | Border |
|---|---|---|---|
| ChatGPT | `#E8F5F0` | `#065F3F` | `#A0D8C0` |
| Gemini | `#E8F0FE` | `#1A4ED8` | `#93B4F5` |
| Claude | `#FDF0EA` | `#8B3A1A` | `#E8B090` |
| Perplexity | `#F0EFFE` | `#4B2DB3` | `#B5A3F0` |
| Google AI | `#FEF0F0` | `#B31A1A` | `#F0A3A3` |

Used in LLM multi-select pills and LLM badges shown on citations.

---

## 8. Key Component Patterns

### Multi-search selector
Reusable component used across all multi-select fields (categories, markets, personas, LLMs, brand kit, knowledge base). Supports:
- Tag pills for selected values (removable with ×)
- Filterable dropdown on focus
- Single-select or multi-select mode
- `onChange` callback for live state sync

### Native `<select>` for single-select
Used for Content format and Language. Styled with CSS chevron. No pills shown after selection — avoids implying multi-select.

### Toast notifications
Used for: credit usage, save confirmation, validation errors, insufficient credits. Auto-dismiss after ~2.2 seconds.

### Version history model
Each article has a `versions[]` array: `[{v, date, note, body}]`. New versions are appended (never mutated). Restoring a version creates a new version entry.

---

## 9. Out of Scope (for this release)

- Real AI content generation (backend LLM call)
- Real visibility data API
- Real citations retrieval
- User authentication / multi-user accounts
- File upload for brand kit / knowledge base
- Scheduling or automated publishing
- Editing published content
- Deletion of library items
- Credit balance display
