# Product Requirements Document
## Feature: Create Content — AI Commerce Visibility (PP Portal)

**Feature ref:** VX-04  
**Status:** Prototype complete  
**Author:** Neha Narayan  
**Last updated:** 6 May 2026

---

## 1. Overview

The **Create Content** feature enables brand users to generate AI-optimised content (blog posts, listicles, comparison articles, etc.) that improves their visibility in AI search model responses (ChatGPT, Gemini, Perplexity, Claude, Google AI).

The workflow guides users through five steps: configure inputs → analyse high-priority prompts → review source citations → generate content → approve and publish. The output is saved to a shared Content Library.

---

## 2. User Stories

| ID | As a… | I want to… | So that… |
|---|---|---|---|
| US-01 | Brand manager | Configure content format, target market, persona, and LLMs | The generated content is tailored to my audience |
| US-02 | Brand manager | See which AI search prompts have the largest visibility gap | I can prioritise the prompts most worth targeting |
| US-03 | Brand manager | Select specific prompts to target | I can focus content on the highest-opportunity gaps |
| US-04 | Brand manager | See which sources are cited for those prompts across LLMs | I understand what content my competitors publish that I lack |
| US-05 | Brand manager | Select specific citations to include | I can control which competitive signals inform my content |
| US-06 | Brand manager | Generate a draft article from the selected citations | I get a ready-to-edit starting point grounded in citation data |
| US-07 | Brand manager | Refine the generated content | I can adjust tone, length, and structure without regenerating |
| US-08 | Brand manager | Approve content and save it to the library | My team can review and track all generated pieces |
| US-09 | Brand manager | Publish an approved article | I can record that a piece has gone live |
| US-10 | Brand manager | View all saved content in a library | I have a record of what has been created and its status |

---

## 3. Navigation & Page Structure

The feature lives within the PP Portal under **AI Commerce Visibility**. The persistent shell contains:

- **Topbar** (always visible): PP logomark, product name, AI-generated beta badge, live credit balance, user avatar
- **Nav tabs**: Insights | Create content (default active) | Content library

Switching tabs shows/hides the relevant section without a full page reload.

---

## 4. Feature Requirements

### 4.1 Create Content — Left Panel (Configuration)

**Content inputs card** — users configure the following before generating:

| Field | Type | Options |
|---|---|---|
| Content format | Single select | Blog post, Listicle, Comparison article, Social post, Email, Product description |
| Product category | Single select | Running Shoes, Coffee Pods, Sportswear, Outdoor Gear |
| Market | Single select | United States, Germany, France, United Kingdom, Netherlands |
| Persona | Single select | Marathon Trainer, Casual Runner, Performance Athlete, Beginner Runner |
| Target LLM | Multi-select toggle | ChatGPT (35%), Gemini (20%), Claude (20%), Perplexity (15%), Google AI (10%) |
| Language | Single select | English, German, French, Spanish, Dutch, Italian |
| Brand kit | Single select + upload | Existing brand kits OR upload new (PDF/ZIP/DOCX, max 20 MB) |
| Knowledge base | Single select + upload | Existing knowledge bases OR upload new (PDF/CSV/TXT, max 50 MB) |

**Target LLM behaviour:**
- Displayed as toggle pills, not a dropdown
- All 5 models active by default; "All models" pill syncs state
- Toggling "All models" activates or deactivates all 5 at once
- "All models" pill auto-activates when all 5 are individually on; deactivates when any is off
- Each LLM pill uses its brand colour when active

**Upload behaviour:**
- Selecting "+ Upload new" in Brand kit or Knowledge base reveals a dashed dropzone inline
- Dropzone supports drag-and-drop or click-to-browse
- File type and size constraints are validated; accepted files show a success confirmation state

**Action:**
- "Analyse prompts" primary CTA — triggers the prompt analysis step
- Credits cost and current balance displayed below the fields

---

**Content library mini card** — shows the 3 most recent approved/published items (title, format, category, status badge). "View all" navigates to the Content Library tab.

---

### 4.2 Create Content — Right Panel (5-Step Workflow)

A persistent step indicator bar shows: Configure → Select prompts → Citations → Generate → Approve. The current step is highlighted coral; completed steps show a green checkmark.

A loading bar below the indicator animates during content generation.

---

#### Step 1 — Configure

Right panel shows a placeholder instructing the user to fill in the left panel and click "Analyse prompts".

---

#### Step 2 — Select Prompts

Triggered when the user clicks "Analyse prompts".

Displays a ranked list of AI search prompts for the selected category, sorted by visibility gap (largest first).

**Each row shows:**
- Checkbox (multi-select)
- Prompt text
- Two mini horizontal bar charts: user's current visibility % vs competitive average % (to-scale, max ~70%)
- Gap badge (−Xpp) colour-coded by severity:
  - ≥30pp gap: red
  - 15–29pp gap: amber
  - <15pp gap: green

**Header controls:**
- "Select all / Deselect all" toggle

**Footer:**
- Count of selected prompts
- "View citations →" CTA (disabled until ≥1 prompt selected)

**Visibility data** comes from the prompt visibility dataset keyed by category. Each prompt record contains: prompt text, user visibility %, competitive average %.

---

#### Step 3 — Citations

Triggered when the user clicks "View citations".

**Opportunity banner** at the top: "Your brand appears in X of Y citations across N prompts. [Category-specific gap description.]"

**Citation groups** — one accordion per selected prompt, all expanded by default:
- Group header: prompt text, "X/Y selected" count, total citation count, collapse chevron
- Collapsible body: list of citation items

**Each citation item shows:**
- Checkbox (pre-selected by default)
- Source domain with favicon-style letter avatar
- LLM badges indicating which models cited this source
- Excerpt snippet from the cited content
- "Your brand not cited" flag where applicable

**Selection behaviour:**
- All citations pre-selected when the view loads
- Clicking a row or its checkbox toggles it
- Group header count updates live
- Footer count updates live

**Footer:**
- "← Back to prompts" text button
- "X of Y citations selected" count
- "Select all / Deselect all" toggle
- "Generate from selected →" CTA (disabled at 0 selected)

---

#### Step 4 — Generate

Triggered by "Generate from selected" in the Citations step or "Regenerate" in the output view.

**Generation behaviour:**
1. A 1.8-second simulated loading animation plays on the loading bar
2. 2 credits are deducted
3. Content is assembled:
   - If citations are selected: prepend a "## Competitive citation analysis" section describing the citation gaps identified, then append the article body template
   - If no citations selected: use the article body template directly
4. Template is selected by format + category. If no exact match, use a generic fallback.
5. The textarea is rendered editable, the approval bar is shown

**Output area contains:**
- Meta tags row: format, category, market, persona, selected LLMs, language
- Copy button
- Regenerate button (2 credits)
- Editable content textarea
- Quick-refine pills: More concise | More technical | Stronger CTA | Optimise for AI citations | Beginner-friendly (each costs 1 credit; appends a transformation note to the content)
- Custom refine input + "Auto-refine (1C)" button (freetext instruction; costs 1 credit)
- Approval action bar (green): "Content is ready." + "Request changes" + "Approve & save"

**Request changes flow:**
- Hides the approval bar, shows an amber "Changes requested" bar with a "Re-submit" button
- Textarea remains editable
- Re-submit restores the approval bar

---

#### Step 5 — Approve

Triggered by "Approve & save".

- Saves the article to the Content Library with status: `approved`
- Hides the approval bar, shows "Approved and saved to library" green confirmation
- Textarea becomes read-only
- Refine controls are hidden
- Mini library on the left panel refreshes

**Article record fields:** title (extracted from first heading), format, category, market, persona, selected LLMs, language, preview (first 200 chars of body text), full body, date, status

---

### 4.3 Content Library Tab

**Header:**
- "Content library" title
- Article count: "N items · M approved · P published"
- Filter pills: All | Blog post | Listicle | Comparison article (more formats can be added)

**Grid:** 3-column responsive card grid.

**Each card shows:**
- Title
- Approved / Published status badge
- Format, category, LLM, language tags
- 3-line preview excerpt
- Saved date
- "View & publish →" (approved) or "View →" (published) footer hint

Clicking a card opens the article viewer.

---

### 4.4 Article Viewer

Right-anchored slide-in panel (760px wide, full-height overlay with translucent backdrop).

Opens/closes with a smooth slide animation. Clicking the backdrop or the ✕ button closes it.

**Header:** article title, meta tags (format, category, market, LLM, language), ✕ close button.

**Subheader — status and publish action:**
- Current status badge
- If `approved`: "Mark as published" button — updates status to `published` across the library grid and mini library card
- If `published`: "✓ Published" indicator

**Body:** full article rendered from markdown (h1, h2, bold, italic, bullet/numbered lists, tables).

**Footer:** "Saved [date] · [format] · [market]"

---

## 5. Status System

| Status | Meaning | Badge colour |
|---|---|---|
| Approved | Reviewed and saved; not yet live | Green: `#E1F5EE` / `#085041` / `#5DCAA5` |
| Published | Manually marked as live | Blue: `#EBF3FF` / `#0052CC` / `#B3D4FF` |

Status transitions: draft (in textarea) → approved (on "Approve & save") → published (on "Mark as published"). There is no automated publishing.

---

## 6. Credits System

| Action | Cost |
|---|---|
| Generate content | 2 credits |
| Regenerate | 2 credits |
| Quick-refine pill | 1 credit |
| Auto-refine (custom instruction) | 1 credit |

Credits are deducted immediately on action. The balance is displayed in three places: topbar pill, left panel balance display. Starting balance for the prototype: 124 credits.

---

## 7. LLM Brand Colours (Reference)

| Model | Background | Text | Border |
|---|---|---|---|
| ChatGPT | `#E8F5F0` | `#065F3F` | `#A0D8C0` |
| Gemini | `#E8F0FE` | `#1A4ED8` | `#93B4F5` |
| Claude | `#FDF0EA` | `#8B3A1A` | `#E8B090` |
| Perplexity | `#F0EFFE` | `#4B2DB3` | `#B5A3F0` |
| Google AI | `#FEF0F0` | `#B31A1A` | `#F0A3A3` |

These colours are used consistently in the LLM multi-select pills and the LLM badges shown on citations.

---

## 8. Out of Scope (for this release)

- Real AI content generation (backend LLM call)
- Real visibility data API
- Real citations retrieval
- User authentication / multi-user accounts
- Actual file processing for brand kit / knowledge base uploads
- Scheduling or automated publishing
- Editing published content
- Deletion of library items
