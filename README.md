# POP-STAR-Innovators
Enhancing air travel, satellite services, and the industry is crucial. Embracing innovation, including AI and other technologies, is encouraged. The goal is to make participation open to anyone interested in making a lasting impact on global connectivity, safety, and enjoyment.


# About
We are a five-member team participating in NASA's Space Apps Challenge 2023.
- Ashlyn Hall
- Chandan Tamariya
- Aubrey Cloud
- Pintilei Bianca-Emilia
- Alec Halici

The NASA Space Apps Challenge is an annual international hackathon-style event organized by NASA (the National Aeronautics and Space Administration) in collaboration with various partners. It brings together individuals from all over the world who share an interest in space, science, and technology to collaborate on innovative solutions for real-world challenges in space exploration and Earth science. This event serves as a competition, a valuable learning experience, and an opportunity for space enthusiasts and innovators to contribute to NASA's missions and scientific endeavors. We are thrilled and honored to participate in this global competition!

Our team has focused on the project STAR: Revolutionizing Technical Standards with AI, aiming to "create a copilot" for mission designers, enabling them to embark on missions with greater confidence, knowing they have met all necessary requirements. We have developed an AI application that assists mission designers in crafting mission requirements for space missions, consisting of both back-end and front-end components. Our ultimate objective is to enhance the capabilities of AI and provide valuable guidance to mission designers.

This website serves as a software setup guide, and we're excited to make a significant impact in this endeavor! Let's rock it!

Update 2026: 
# POP-STAR-Innovators — Policy Navigator

A tool that helps people work with dense IT/security policy documents — like
NIST SP 800-53 controls or NIST Cybersecurity Framework subcategories — by
automatically finding the control references in a document, highlighting
them, and explaining what each one means in plain language through a
built-in chatbot.

## About

We are a five-member team originally formed for NASA's Space Apps Challenge
2023:
- Ashlyn Hall
- Chandan Tamariya
- Aubrey Cloud
- Pintilei Bianca-Emilia
- Alec Halici

The project started as **STAR: Revolutionizing Technical Standards with AI**,
a copilot for mission designers navigating technical requirements. It has
since grown to also cover NIST/IT policy documents and program/technical
jargon: a person can paste in a policy, compliance, or program document, and
the tool finds every control reference (`AC-1`, `CM-2(3)`, `PR.AC-1`, etc.)
*and* recognized terms of art (`CoA`, `multi-fidelity models`, `surrogate
models`, `AF Modeling & Simulation`, etc.), highlights them inline, and lets
the person click a highlight — or just ask the chatbot directly — to get a
plain-English explanation.

## What's in this repo

- **`index.html`** — the whole web app: paste a document, click **Scan
  document** to highlight every recognized control reference, then click a
  highlight (or type a question like *"What is IA-2?"*) to get an
  explanation from the built-in policy assistant. No build step or server
  needed — open the file in any browser, or serve it with GitHub Pages.
- **`policy_tracker.py`** — a companion script for teams that keep a
  spreadsheet-based compliance tracker. It scans a document for control IDs
  and marks the matching rows in an `.xlsx` tracker as `Updated/Compliant`.
  Run it with:
  ```
  python policy_tracker.py documentation.md NIST_Tracker.xlsx
  ```

## Hierarchy

NIST content is layered, and the app shows that. The sidebar is a collapsible
tree: **800-53** is family > control (AC > AC-2), the **CSF** is function >
category > subcategory (Protect > PR.AC > PR.AC-1), and the program terms are
grouped by topic (Modeling & Simulation > Multi-Fidelity Models > Surrogate
Models). Click any name for an explanation and where it fits. After you scan a
document, every group shows how many of its entries the document references
(e.g. `1/5` in the AC family). You can also ask the bot, e.g. *"What's in the
AC family?"* or *"What's under multi-fidelity models?"*.

Families and CSF categories are built automatically from the IDs in
`POLICY_KB` (names live in `FAMILY_NAMES` / `CSF_CATEGORIES`). The term groupings
are hand-set in `buildHierarchy()`.

## Matcher robustness

The matcher handles messy real-world text, not just clean "AC-1"-style input:
dash look-alikes from Word/PDF exports, "AC 1" / "AC - 1" spacing, zero-padded
numbers ("AC-01"), enhancements that fall back to their base control
("CM-2(3)" → CM-2), plural/hyphenated term variants ("design spaces",
"course-of-action"), and a prefix whitelist so lookalikes like `SHA-256` or
`ISO-27001` are never mistaken for a control. See `experiments/` for the
6,000-document Monte Carlo test that found these gaps and the before/after
numbers after fixing them (55.8% → 95.4% detection, 0 false positives).

## Extending the knowledge base

`index.html` has two knowledge bases in its `<script>`:
- **`POLICY_KB`** — NIST 800-53 / CSF control IDs (e.g. `AC-1`), matched by pattern.
- **`TERM_KB`** — program/technical terms and acronyms that don't follow a
  control-ID shape (e.g. `CoA`, `surrogate models`, `AF Modeling &
  Simulation`). Each entry lists every phrase variant that should trigger it
  (`match: [...]`) plus a `label` and `explanation`.

`policy_tracker.py`'s `NIST_EXPLANATIONS` dict covers the same NIST controls
for the spreadsheet-tracking workflow. Add a new entry to whichever file
matches what you're extending — a control ID goes in `POLICY_KB`, a jargon
phrase goes in `TERM_KB`.

## Running it

Just open `index.html` in a browser — everything runs client-side with no
dependencies. To publish it as a live site, enable GitHub Pages for this
repo and point it at `index.html`.
