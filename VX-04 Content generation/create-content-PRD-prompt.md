# Product Requirements Document
## Feature: Create Content — AI Commerce Visibility (PP Portal)

**Feature ref:** VX-04  
**Status:** Prototype complete  
**Author:** Neha Narayan  
**Last updated:** 8 May 2026 (rev 2)

---

## 1. Overview

The **Create Content** feature enables brand users to generate AI-optimised content (blog posts, listicles, comparison articles, etc.) that improves their visibility in AI search model responses (ChatGPT, Gemini, Perplexity, Claude, Google AI).

The workflow is a 6-step modal launched from the Content Library: configure context → select format → configure brand resources → analyse prompts → review citations → generate and approve content. Generated content is saved to the Content Library.

---

## 2. User Stories

| ID | As a… | I want to… | So that… |
|---|---|---|---|
| US-01 | Brand manager | Configure context (category, market, persona, LLM) and content format before generating | The generated content is tailored to my audience and channel |
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

- **Topbar** (always visible): PP logomark, product name "AI Commerce Visibility | Content", AI-generated beta badge, user avatar
- **Left nav** (always visible): 64px dark navy sidebar with icon tiles for each PP experience — AI Visibility (active, highlighted coral `#BD164B`), Decision Intel, Post-Purchase, Checkout, Returns, Logistics. Hovering a tile shows a full-name tooltip.
- **Nav tab** (below topbar): "Content library" — the only tab in this view

There is no "Create content" nav tab. The entry point to content generation is the **"+ Generate content"** primary button in the Content Library header.

---

## 4. Feature Requirements

### 4.1 Content Library (Main View)

The default view on page load. Displays all saved articles in a **table layout**.

**Header:**
- "Content library" title with article count
- **Search input** — filters the table live by title, format, or category; shows "No results found" empty state when no match
- "**+ Generate content**" primary CTA — opens the Generate modal

**Filter pills:** All | Draft | Approved | Published

**Table columns:** Title (with category/market tags below) | Format | Language | LLMs | Status | Date

**Row behaviour:**
- Clicking anywhere on a row opens the Article Detail view
- No separate "Open" button on rows
- Rows show pointer cursor on hover

---

### 4.2 Generate Content Modal (6-Step Workflow)

A centred modal (max-width 1040px, height 80vh) with a persistent step indicator bar.

**Step order:** Context → Format → Brand → Prompts → Citations → Review

Completed steps show a green checkmark. Current step is highlighted coral.

---

#### Step 1 — Context

| Field | Type | Options |
|---|---|---|
| Product category | Single-select native `<select>` | Running Shoes, Coffee Pods, Sportswear, Outdoor Gear |
| Market | Multi-search selector | United States, Germany, France, United Kingdom, Netherlands |
| Persona | Multi-search selector | Marathon Trainer, Casual Runner, Performance Athlete, Beginner Runner |
| Target LLM | Multi-search selector | ChatGPT (35%), Gemini (20%), Claude (20%), Perplexity (15%), Google AI (10%) |

**Product category** uses the same styled native `<select>` as the Format field — single selection only, no tag pills.

**Footer:** "Next →" (disabled until a product category is selected)

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

Displays a ranked list of AI search prompts for the selected category, sorted by visibility gap (largest first).

**Each row shows:**
- Checkbox (multi-select)
- Prompt text
- Two mini horizontal bar charts: user's current visibility % vs competitive average %
- Gap badge (−Xpp) colour-coded: ≥30pp red · 15–29pp amber · <15pp green

**Header controls:** Deselect all

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

Triggered by "Generate content". A loading animation ("Generating content…") runs for ~1.2 seconds, then the review view renders. **50 credits are deducted** and a toast confirms ("50 credits used").

**3-column layout:**

| Column | Width | Contents |
|---|---|---|
| Brief | 220px | AI-generated strategy brief explaining citation gaps and content approach |
| Content | flex (fills remaining space) | Edit/Preview toggle + editable textarea or rendered markdown + refine controls |
| Inputs | 196px | Read-only metadata summary of all configured inputs |

**Content column contains:**
- **Edit / Preview toggle** — switches between the editable textarea and a rendered markdown preview
- Editable textarea (min-height 300px) shown in Edit mode
- Rendered markdown view shown in Preview mode
- Quick-refine pills: More concise · More technical · Stronger CTA · Optimise for AI citations (each costs 10 credits; combined toast confirms: "Content refined · 10 credits used")
- Custom refine input + "Refine" button (costs 10 credits)

**Inputs column (read-only metadata):**
- Format, Language, Categories, Markets, Personas, LLMs, Brand kit, Knowledge base, Prompts (count), Citations (count)
- Displayed as labelled metadata rows — not interactive

**Footer:** "Save to library" CTA — saves with status `draft`

---

#### Save to Library

Triggered by "Save to library":
- Saves the article to the Content Library with status: `draft`
- Modal closes
- Library table refreshes
- Toast: "Content saved to library."

**Article record fields:** title (extracted from first heading), format, categories, markets, personas, LLMs, language, brief, full body, date, status, versions[]

---

### 4.3 Article Detail View

Opened by clicking any row in the Content Library table.

**Header:** Back link, article title, status badge, status action button(s), "Save" button (edit mode only), "Edit content" / "Cancel" button

**3-column layout:**

| Column | Width | Contents |
|---|---|---|
| Brief | 220px | Article strategy brief |
| Content | flex | Rendered markdown (read mode) or editable textarea + Edit/Preview toggle + refine controls (edit mode) |
| Right panel | 220px | Read-only inputs summary + Version history |

**Edit mode** (toggled by "Edit content"):
- Textarea replaces rendered content
- **Edit / Preview toggle** appears — switches between textarea and rendered markdown preview
- Refine pills and custom refine input appear — these update the textarea in place only, **no version is created**
- "Save" button in the header is the **only action that creates a new version**; saves edit and reverts status to Draft

**Version history** (in right panel):
- Lists all versions in reverse chronological order
- Each entry: version label (e.g. v1.2), date, note, **email of the user who made the change**, "Restore" button (non-current versions)
- Restoring creates a new version entry using the same versioning logic

---

## 5. Status System

| Status | Meaning | Badge colour |
|---|---|---|
| Draft | Saved to library; not yet approved | Grey: `#F4F5F7` / `#494949` |
| Approved | Reviewed and approved; not yet live | Green: `#E1F5EE` / `#085041` |
| Published | Manually marked as live | Blue: `#EBF3FF` / `#0052CC` |

**Status transitions:**

| Current status | Primary action | Secondary action |
|---|---|---|
| Draft | Approve (coral button) | — |
| Approved | Publish (outline button) | Revert to draft (outline button) |
| Published | — | Revert to draft (outline button) |

- Any edit to article content (save new version) **always reverts status to Draft**
- No automated publishing

---

## 6. Credits System

| Action | Cost |
|---|---|
| Generate content | 50 credits |
| Quick-refine pill | 10 credits |
| Custom refine | 10 credits |

Credits are deducted immediately on action. A **toast notification** is the only feedback:
- Generation: "50 credits used" (standard dark toast, bottom-right)
- Refine (modal or detail): "Content refined · 10 credits used" (green success toast, top-centre)
- Save version (detail): "New version saved." (green success toast, top-centre)

Credit balance and cost labels are **not** shown in the UI — no balance display in topbar, buttons, or pills.

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

### Native `<select>` for single-select
Used for Product category, Content format, and Language. Styled with CSS chevron. No pills shown after selection — avoids implying multi-select.

### Multi-search selector
Reusable component used across multi-select fields (markets, personas, LLMs, brand kit, knowledge base). Supports:
- Tag pills for selected values (removable with ×)
- Filterable dropdown on focus
- Single-select or multi-select mode
- `onChange` callback for live state sync

### Edit / Preview toggle
Used in both the modal Review step and the Article Detail edit mode. A two-button segmented control (Edit | Preview) sits in the content column header. Preview renders the current textarea content as markdown; switching back to Edit restores the textarea.

### Toast notifications
- **Dark navy toast** (bottom-right): errors, credit usage for generation, general confirmations
- **Green success toast** (top-centre): refine confirmations, save confirmations, status updates

Auto-dismiss after ~2.2 seconds.

### Version history model
Each article has a `versions[]` array: `[{v, date, note, email, body}]`. New versions are appended (never mutated). Each version records the **email** of the user who created it.

**Version label format:** `{major}.{minor}` — e.g. `1.1`, `1.2`, `2.1`

| Scenario | Behaviour |
|---|---|
| Save while article is Draft | Minor increments: 1.1 → 1.2 → 1.3 |
| Save after article was Approved or Published | Major increments, minor resets: 1.3 → 2.1 → 2.2 |
| Restore a version | Same major/minor logic as Save (based on current status at time of restore) |
| Refine (pill or custom) | Updates textarea in place only — **no version created** |

New articles start at `1.1`. The article object tracks `_majorV` and `_minorV` counters internally.

---

## 9. Out of Scope (for this release)

- Real AI content generation (backend LLM call)
- Real visibility data API
- Real citations retrieval
- User authentication / multi-user accounts
- File upload for brand kit / knowledge base
- Scheduling or automated publishing
- Deletion of library items
- Credit balance display
- Column sorting in the content library
- Diff view between versions
