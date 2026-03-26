---
description: "Audit websites for SEO, accessibility, GEO (Generative Engine Optimization), and AI search readiness. Produces a scored report with prioritized fix recommendations."
---

# Website Audit

Audit public-facing websites across four dimensions:
- **Accessibility** — ADA/WCAG compliance signals
- **SEO** — Search engine optimization fundamentals
- **GEO** — Generative Engine Optimization (AI search readiness)
- **Usability** — Mobile, navigation, contact info, online services

Produces a scored report with a letter grade and prioritized fix recommendations.

## Step 1: Gather Inputs

Determine from the user (or from $ARGUMENTS):
1. **URL(s)** to audit
2. **Organization type** (municipality, library, tourism, utility, restaurant, museum, housing, general). This shapes recommendations.
3. **Organization name** (used in report title)

If the user provides multiple URLs, run each audit sequentially and produce a summary at the end.

## Step 2: Fetch and Analyze

For each URL, fetch the HTML. Analyze the raw HTML source — do NOT rely on what renders visually.

## Step 3: Run the Checks

Evaluate these 17 checks. Score each as **PASS**, **PARTIAL**, or **FAIL**.

### Accessibility (5 checks)
1. **HTTPS** — Valid SSL certificate
2. **Alt text** — All `<img>` tags have meaningful alt attributes (not empty, not "image")
3. **Heading hierarchy** — Single `<h1>`, logical h2→h3→h4 nesting, no skipped levels
4. **Form labels** — All `<input>` elements have associated `<label>` or `aria-label`
5. **Color contrast** — Check for low-contrast text patterns (light gray on white, etc.)

### Usability (4 checks)
6. **Mobile responsive** — Has `<meta name="viewport">` with `width=device-width`
7. **Navigation** — Clear nav structure, skip-to-content link present
8. **Contact info** — Phone, email, or address findable within 2 clicks from homepage
9. **Page speed signals** — No render-blocking scripts in `<head>`, images have dimensions

### SEO (4 checks)
10. **Title tag** — Present, unique, under 60 characters
11. **Meta description** — Present, under 160 characters, descriptive
12. **Canonical URL** — `<link rel="canonical">` present
13. **Structured data** — JSON-LD schema markup present (Organization, LocalBusiness, etc.)

### GEO (4 checks)
14. **llms.txt** — Check if `/llms.txt` exists (AI crawler guidance)
15. **Semantic HTML** — Uses `<article>`, `<section>`, `<main>`, `<aside>` appropriately
16. **FAQ/QA schema** — FAQPage or QAPage structured data present
17. **Multilingual** — `hreflang` tags or language switcher present (PARTIAL if single-language with lang attribute)

## Step 4: Score and Grade

**Scoring:** PASS = 2 points, PARTIAL = 1 point, FAIL = 0 points per check.

**Category scores** (percentage of max points in that category):
- Accessibility: /10
- Usability: /8
- SEO: /8
- GEO: /8

**Overall score:** Sum of all points / 34 max

**Grade thresholds:**

| Score | Grade |
|-------|-------|
| 90%+  | A     |
| 80-89%| B     |
| 70-79%| C     |
| 60-69%| D     |
| <60%  | F     |

Use +/- modifiers (e.g., B+ for 87%, C- for 70%).

## Step 5: Generate Report

Output a structured report with:

1. **Header** — Organization name, URL, date, overall grade
2. **Scorecard** — Category scores in a table
3. **Detailed findings** — Each check with PASS/PARTIAL/FAIL and specific notes
4. **Key issues** — Top 3-5 problems, ordered by impact
5. **Opportunities** — Quick wins that would improve the grade
6. **Why this matters** — Tailored to org type (see framing table below)

### Org-Type Framing

| Type | Compliance Angle | Cost-of-Inaction Frame |
|------|-----------------|----------------------|
| municipality | ADA Title II, DOJ enforcement | $10K-$75K ADA settlement |
| library | ADA Title II, LSTA/LORI grants | $5K-$15K demand letter response |
| tourism | Visitor revenue, AI search visibility | Lost traffic during major events |
| utility | ADA Title II, EPA/SRF grants, PUC scrutiny | $5K-$15K legal + customer service costs |
| restaurant | Google/AI search visibility, foot traffic | Invisible to AI-powered discovery |
| museum | ADA compliance, grant funding | Missing visibility opportunities |
| housing | HUD Section 504, Fair Housing Act | Federal compliance finding |
| general | ADA compliance, search visibility | Competitive disadvantage |

## Step 6: Batch Summary (multiple URLs only)

If auditing multiple sites, produce a comparison table:
- Organization, URL, Grade, Accessibility, Usability, SEO, GEO, Top Issue
