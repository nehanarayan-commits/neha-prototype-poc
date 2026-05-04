---
name: pp-portal-prototype
description: >
  Use this skill whenever someone asks to prototype, mock up, or design a screen
  for the Parcel Perform portal. Triggers include: "build a prototype for [feature]",
  "mock up [page/feature]", "create a PP portal page for...", "design [feature] screen",
  or any request to visualise a PP product feature. Always load this skill before
  writing any HTML. Do NOT skip this skill for any PP portal UI request — even quick
  mockups must use the correct shell, tokens, and experience color theming.
---

# PP Portal Prototype Skill

Generates fully interactive, brand-compliant PP portal prototype HTML files
delivered as downloadable artifacts via Claude Code.

---

## Output Rules

- Output is always a **single self-contained `.html` file**
- Delivered to `/mnt/user-data/outputs/[feature-name].html`
- Full JS interactivity: nav flyouts, tabs, slide-in panels, chart interactions
- Realistic PP-adjacent proxy data — never lorem ipsum
- Min-width 1440px, desktop-first

---

## Step 1 — Identify the Experience

Infer the product area from the request:

| If request mentions… | Experience | Nav tile to activate |
|---|---|---|
| AI Visibility, GEO, AI shopping, brand mentions | AI Commerce Visibility | AI Visibility tile |
| Decision Intel, cohort, routing rules, carrier selection, EDD | AI Decision Intelligence | AI Decision Intel tile |
| Post-purchase, tracking page, notifications, WISMO | Post-Purchase | Post-Purchase tile |
| Checkout, EDD widget, delivery promise | Checkout | Checkout tile |
| Returns, return flow, RMA | Returns | Returns tile |
| Logistics, carrier, booking, cost audit | Logistics | Logistics tile |

**Set the active nav tile** by adding `pp-nav-tile--active` to the correct tile. Only one tile should have this class at a time.

---

## Step 2 — Apply Experience Color Theming

Look up the experience color from the token table below. Apply:

| Token role | CSS variable | Where used |
|---|---|---|
| Page body background | `--exp-100` | `pp-body` background (replaces `--surface-03`) |
| Accent / active states | `--exp-400` | Active tabs, active borders, highlights |
| Dark accent | `--exp-500` | Submenu headers, strong emphasis |
| Submenu background | `--exp-100` | `.pp-nav-submenu` background |

### Experience Color Token Map

```css
/* AI Decision Intelligence */
--exp-100: #EAEDF9;
--exp-400: #062ABC;
--exp-500: #0C1A66;

/* AI Commerce Visibility */
--exp-100: #FFEDF3;
--exp-400: #BD164B;
--exp-500: #540B0A;

/* Post-Purchase */
--exp-100: #FFEFE5;
--exp-400: #FE5903;
--exp-500: #C53F1F;

/* Checkout */
--exp-100: #E5F1FF;
--exp-400: #1777FF;
--exp-500: #004B99;

/* Returns */
--exp-100: #EDE5FF;
--exp-400: #6C36FF;
--exp-500: #2F318E;

/* Logistics */
--exp-100: #E9FAFC;
--exp-400: #00C9D5;
--exp-500: #009AB5;
```

---

## Step 3 — Design Tokens (Always Declare)

```css
:root {
  /* Core brand */
  --pp-navy:        #001B3A;
  --pp-orange:      #EA580C;

  /* Surface */
  --surface-00:     #FFFFFF;
  --surface-100:    #F8F8FC;
  --surface-200:    #EFF2F6;
  --surface-300:    #EAEDF3;
  --surface-400:    #D0D7E4;

  /* Text (black with opacity) */
  --text-high:      rgba(0,0,0,0.87);
  --text-medium:    rgba(0,0,0,0.60);
  --text-disabled:  rgba(0,0,0,0.38);
  --text-divider:   rgba(0,0,0,0.20);
  --text-overlay:   rgba(0,0,0,0.10);

  /* Elevation */
  --elev-01: 0px 0px 1px 0px rgba(40,41,61,0.08), 0px 0.5px 2px 0px rgba(96,97,112,0.16);
  --elev-03: 0px 0px 2px 0px rgba(40,41,61,0.04), 0px 4px 8px 0px rgba(96,97,112,0.16);

  /* Neutral */
  --neutral-slateblue:  #5D89A3;
  --neutral-stoneblue:  #1A2752;
  --neutral-grey-100:   #CCCCCC;
  --neutral-grey-500:   #8A8A8A;
  --neutral-grey-700:   #494949;

  /* Status */
  --status-red-100:    #FAE9E9;
  --status-red-400:    #D73027;
  --status-green-100:  #EDF6EF;
  --status-green-400:  #1B7837;
  --status-blue-100:   #EDF5FD;
  --status-blue-400:   #5E90BF;
  --status-blue-500:   #0073BA;
  --status-yellow-400: #E89041;

  /* Links */
  --link-default: #1577FF;
  --link-light:   #7CB5FF;

  /* Parcel delivery status */
  --pds-delivered: #0F5A99;
  --pds-active:    #14A7DF;
  --pds-issue:     #B20000;
  --pds-pending:   #898989;
  --pds-inactive:  #F2C446;
  --pds-expired:   #ADADAD;
  --pds-archived:  #D3D3D3;

  /* Experience colors (override per experience — see Step 2) */
  --exp-100: #EAEDF9;
  --exp-400: #062ABC;
  --exp-500: #0C1A66;
}
```

---

## Step 4 — Font Setup

Always include in `<head>`:

```html
<link rel="preconnect" href="https://fonts.googleapis.com">
<link href="https://fonts.googleapis.com/css2?family=Nunito+Sans:ital,opsz,wght@0,6..12,400;0,6..12,600;0,6..12,700;0,6..12,800;1,6..12,400&display=swap" rel="stylesheet">
```

Font rules:
- **Font family:** `'Nunito Sans', sans-serif` — everywhere
- **Weights:** 400 (regular), 600 (semibold), 700 (bold), 800 (extrabold)
- **Radius:** 4px cards/inputs/buttons · 1000px pills · 8–14px round elements
- **Motion:** submenus 0.18s ease · slide-in 0.3s cubic-bezier(0.4,0,0.2,1) · hover 0.12–0.15s

---

## Step 5 — Shell Structure

Use the shell from the reference below **verbatim** for all fixed zones.
Only customise the content within `.pp-body` between the page header and footer.

### Protected zones (never modify):
- `.pp-nav` and all `.pp-nav-tile` elements (except which tile has `--active`)
- `.pp-nav-submenu` flyouts
- `.pp-agent-sidebar`
- `.pp-footer`
- `.pp-page-header` structure

### Active nav tile colour — ALWAYS use `var(--exp-400)`:
```css
/* ⚠️ NEVER hardcode a colour here — this must always use the experience token */
.pp-nav-tile--active { background: var(--exp-400); border-color: var(--exp-400); }
```
This ensures the active tile reflects the correct experience colour (orange for Post-Purchase, teal for Logistics, blue for AI Decision Intel, etc.). Using `#009AB5` or any hardcoded hex here is a bug.

### Body zone rules:
```css
.pp-body {
  flex: 1; overflow-y: auto; overflow-x: hidden;
  background: var(--exp-100);   /* ← experience color, not surface-03 */
  padding: 20px;
  display: flex; flex-direction: column;
  gap: 16px;
}
```

### Content cards:
```css
.pp-card {
  background: #fff;
  border-radius: 4px;
  padding: 16px;
  box-shadow: var(--elev-01);
}
```

---

## Step 6 — Core Components Quick Reference

| Component | Class | Notes |
|---|---|---|
| Surface card | `.pp-card` | White, 4px radius, elev-01, 16px padding |
| Primary button | `.pp-btn-primary` | **Always** PP orange `#EA580C` — never experience color. White text, 4px radius, 36px height |
| Accent/CTA button | `.pp-btn-accent` | **Always** PP orange `#EA580C` — same as primary. Experience color (`--exp-400`) is reserved for active states, tab underlines, and highlights only — never buttons. |
| Secondary button | `.pp-btn-secondary` | Border `--surface-400`, text `--text-high` |
| Status badge (pill) | `.pp-badge .pp-badge--[variant]` | 1000px radius, white text |
| Delta chip | `.pp-delta .pp-delta--[pos/neg]` | Green ↑ `#EDF6EF/#1B7837` · Red ↓ `#FAE9E9/#8F0000` |
| Table wrapper | `.pp-table-wrapper > .pp-table` | `border-collapse: collapse` |
| Table header `.pp-th` | 52px height, 700 13px, bottom border |
| Table row `.pp-tr` | 48px height, hover `rgba(0,0,0,0.03)` |
| Table cell `.pp-td` | 13px regular, max-width 160px, ellipsis |
| Tabs | `.pp-tabs > .pp-tab` | Active: `--exp-400` underline + text |
| Search | `.pp-search-bar > .pp-search-field` | 420px wide, 36px height |
| Slide-in panel | `.pp-slidein` | Right drawer, 760px wide, `position: fixed; right: 80px` — always anchored left of the agent sidebar. NEVER use `right: 0` or `width > 760px` as this covers Ava and Help. Animate with `transform: translateX(calc(100% + 80px))` → `translateX(0)` |

### Badge variants:
```css
.pp-badge--assigned   { background: #5D89A3; }
.pp-badge--shipped    { background: #1A2752; }
.pp-badge--booked     { background: #0073BA; }
.pp-badge--failed     { background: #D73027; }
.pp-badge--unassigned { background: #949DAD; }
```

### Rule badges:
```css
.pp-rule-badge--cost     { background: #EBF3FF; color: #0052CC; border: 1px solid #B3D4FF; }
.pp-rule-badge--balanced { background: #E8F5EB; color: #1B7837; border: 1px solid #B8DFC0; }
.pp-rule-badge--manual   { background: #FFF8E1; color: #856300; border: 1px solid #FFD966; }
.pp-rule-badge--fallback { background: #F4F5F7; color: #494949; border: 1px solid #DFE1E6; }
```

---

## Step 7 — Charts

Use **Chart.js** (loaded from CDN) for all data visualisations.

```html
<script src="https://cdnjs.cloudflare.com/ajax/libs/Chart.js/4.4.1/chart.umd.min.js"></script>
```

Chart styling rules:
- Use `--exp-400` as the primary chart color
- Use `--exp-100` as fill/background tint
- Always include a legend and axis labels
- Use realistic PP proxy data (see Step 8)
- Wrap each chart in a `.pp-card` with a title and optional subtitle

---

## Step 8 — Proxy Data

Always use realistic PP-adjacent data. Never lorem ipsum or "Sample Data".

| Data type | Examples |
|---|---|
| Merchants | Nespresso, Zalora, iHerb, Decathlon, ASOS, Shopee, Lazada |
| Carriers | DHL, J&T Express, Aramex, GLS, DPD, Hermes, UPS, FedEx |
| Regions / tradelanes | DE→NL, SG→AU, MY→ID, AE→SA, GB→DE |
| EDD delta | 2.1d, 1.8d, 0.9d, 3.4d, 0.4d |
| Dates | 12 Jun 2025, 08:43 — ISO-adjacent |
| Costs | €0.43, €1.12, $2.80 (match merchant region) |
| Tracking IDs | PP-SG-20251206-001, DHL-4920384756 |
| Users | User A · Operations Lead, User B · Finance Analyst, user.a@..., user.b@... — **always generic; never copy real names or emails from uploaded screenshots** |

### Page header user example:
```html
<div class="pp-avatar">JD</div>
<div class="pp-user-info">
  <span class="pp-user-name">Jane Doe</span>
  <span class="pp-user-org">[Merchant] · [Role]</span>
</div>
```
| Metrics | On-time rate, EDD accuracy %, cost per shipment, carrier SLA breach rate |

---

### Slide-in CSS (copy verbatim — NEVER override right/width):

```css
/* Slide-in must always sit left of the 80px agent sidebar */
.pp-slidein-backdrop {
  position: fixed; inset: 0; background: rgba(0,0,0,0.4);
  z-index: 100; opacity: 0; pointer-events: none;
  transition: opacity 0.3s;
}
.pp-slidein-backdrop.open { opacity: 1; pointer-events: all; }

.pp-slidein {
  position: fixed; top: 0; right: 80px; height: 100vh; width: 760px;
  background: #fff; z-index: 101;
  box-shadow: -4px 0 24px rgba(0,0,0,0.15);
  transform: translateX(calc(100% + 80px));
  transition: transform 0.3s cubic-bezier(0.4,0,0.2,1);
  display: flex; flex-direction: column; overflow: hidden;
}
.pp-slidein.open { transform: translateX(0); }
```

⚠️ `right: 80px` and `transform: translateX(calc(100% + 80px))` are a matched pair. Never use `right: 0` — this buries the Ava agent sidebar.

---

## Step 9 — JS Interactions (Copy Verbatim)

```javascript
// Nav submenu
function toggleSubMenu(e) {
  e.stopPropagation();
  const menu = document.getElementById('logisticsSubMenu');
  const isOpen = menu.classList.contains('open');
  document.querySelectorAll('.pp-nav-submenu').forEach(m => m.classList.remove('open'));
  if (!isOpen) menu.classList.add('open');
}
function toggleSettingsMenu(e) {
  e.stopPropagation();
  const menu = document.getElementById('settingsSubMenu');
  const isOpen = menu.classList.contains('open');
  document.querySelectorAll('.pp-nav-submenu').forEach(m => m.classList.remove('open'));
  if (!isOpen) menu.classList.add('open');
}
document.addEventListener('click', () => {
  document.querySelectorAll('.pp-nav-submenu').forEach(m => m.classList.remove('open'));
});
```

---

## Step 10 — Z-index Ladder

| Layer | z-index |
|---|---|
| Shell | 1 |
| Agent sidebar | 5 |
| Left nav | 6 |
| Submenu flyouts | 7 |
| Slide-in backdrop | 100 |
| Slide-in panel | 101 |
| Tooltip / dropdown | 200 |

---

## Deliver

Save final file to `/mnt/user-data/outputs/[feature-name].html` and present with `present_files`.
