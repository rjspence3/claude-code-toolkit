---
name: site-audit
description: >
  Audit websites for SEO, accessibility, GEO (Generative Engine Optimization), and AI search readiness.
  Use this skill whenever the user asks to audit, review, grade, or assess a website's SEO, accessibility,
  structured data, AI visibility, or overall web presence. Also trigger when the user wants to run batch
  audits across multiple websites, generate a website scorecard or report card, check WCAG/ADA compliance
  signals, evaluate structured data or schema markup, check for llms.txt or AI crawler readiness, or
  produce a PDF findings report for a client. Trigger on phrases like "audit this site", "check their SEO",
  "how's their accessibility", "grade this website", "run an audit on these URLs", "website report card",
  "what's missing from this site", or "client-ready audit".
---

# Website Audit Skill

## Overview

This skill audits public-facing websites across four dimensions:
- **Accessibility** — ADA/WCAG compliance signals
- **SEO** — Search engine optimization fundamentals
- **GEO** — Generative Engine Optimization (AI search readiness)
- **Usability** — Mobile, navigation, contact info, online services

It produces a **client-ready PDF report** with a letter grade, scorecard, and prioritized fix recommendations.

## Modes

### Single Audit
User provides one URL. Skill fetches the page, runs all checks, generates a PDF report.

### Batch Audit
User provides a list of URLs (CSV, spreadsheet, or plain list). Skill audits each site and produces:
- One PDF per site
- A summary spreadsheet with all scores and grades

## Workflow

### Step 1: Gather Inputs

Determine from the user:
1. **URL(s)** to audit
2. **Organization type** for each (municipality, library, tourism, utility, restaurant, museum, housing, general). This shapes the recommendations and "why it matters" framing.
3. **Organization name** for each (used in the PDF title)

If batch mode, check if the user has a CSV/spreadsheet with columns for URL, name, and type. If not, accept a plain list and ask for types.

### Step 2: Fetch and Analyze

For each URL, use `web_fetch` to retrieve the HTML. Then analyze the raw HTML for the checks listed below.

**IMPORTANT**: Do NOT rely on what renders visually. Check the actual HTML source for each element.

### Step 3: Run the Checks

For each URL, evaluate these 17 checks. Score each as PASS, PARTIAL, or FAIL.

Read `~/.claude/skills/site-audit/references/checks.md` for the detailed criteria for each check.

### Step 4: Score and Grade

Read `~/.claude/skills/site-audit/references/scoring.md` for the scoring methodology and grade thresholds.

### Step 5: Generate PDF Report

Run the PDF generation script:

```bash
python3 ~/.claude/skills/site-audit/scripts/generate_report.py \
  --name "Organization Name" \
  --url "https://example.com" \
  --type "library" \
  --checks-json "/path/to/checks_result.json" \
  --output "/path/to/org_name_audit.pdf"
```

The checks JSON should be a file you create with this structure:
```json
{
  "checks": [
    {"name": "HTTPS", "status": "PASS", "notes": "Valid SSL certificate"},
    {"name": "Mobile Responsive", "status": "PARTIAL", "notes": "Viewport meta present but some elements overflow"},
    ...
  ],
  "scores": {
    "accessibility": 6,
    "usability": 7,
    "seo": 3,
    "geo": 2
  },
  "grade": "C+",
  "key_issues": "No structured data. No meta descriptions. No multilingual support.",
  "opportunities": "Add JSON-LD schema. Write meta descriptions. Multilingual critical for community demographics.",
  "context_note": "Optional note about timing hooks or specific context (e.g., FIFA World Cup, America250, grant compliance)"
}
```

### Step 6: Batch Summary (batch mode only)

If running multiple audits, after all PDFs are generated, create a summary spreadsheet using the xlsx skill with columns:
- Organization, Type, URL, Grade, Accessibility Score, Usability Score, SEO Score, GEO Score, Key Issues, Top Opportunity

### Step 7: Present Files

Use `present_files` to share:
- Individual PDF reports
- Summary spreadsheet (if batch)

## Org-Type Framing

The "Why This Matters" section in each report should be tailored to the org type:

| Type | Compliance Angle | Cost-of-Inaction Frame |
|------|-----------------|----------------------|
| municipality | ADA Title II, DOJ enforcement | $10K-$75K ADA settlement |
| library | ADA Title II, LSTA/LORI grants | $5K-$15K demand letter response |
| tourism | Visitor revenue, AI search visibility | Lost traffic during major events |
| utility | ADA Title II, EPA/SRF grants, PUC scrutiny | $5K-$15K legal + customer service costs |
| restaurant | Google/AI search visibility, foot traffic | Invisible to AI-powered discovery |
| museum | ADA compliance, grant funding, America250 | Missing once-in-a-generation visibility moment |
| housing | HUD Section 504, Fair Housing Act LEP requirements | Federal compliance finding |
| general | ADA compliance, search visibility | Competitive disadvantage |

## Additional References

- `~/.claude/skills/site-audit/references/checks.md` — Detailed pass/fail/partial criteria for all 17 checks
- `~/.claude/skills/site-audit/references/scoring.md` — Scoring methodology, weights, and grade thresholds
- `~/.claude/skills/site-audit/scripts/generate_report.py` — PDF report generator
