---
name: study-guide-builder
description: '**WORKFLOW SKILL** — Build a complete, structured learning or interview-preparation study guide from a topic or pasted job description. USE WHEN the user asks to "make a study guide", "prep me for an interview / role", "help me learn <topic> from basics", "create interview prep notes", or "build an index + notes/md files for <subject>". Produces a master index plus per-section Markdown files explained from zero knowledge, with Mermaid diagrams, plain-English analogies, interview Q&As, memory hooks, a 100+ question bank, and behavioral/closing prep. DO NOT USE FOR: answering a single factual question, or writing production code.'
---

# Study Guide Builder

A repeatable workflow for turning any topic or job description into a complete, beginner-friendly study + interview-prep guide spread across linked Markdown files.

## When to use
- "Prepare me for the <role> interview" / a pasted job description.
- "Help me learn <topic> from the basics" / "make notes/md files for <subject>".
- Any request for structured, multi-section study material with diagrams and practice questions.

## Workflow (follow in order)

### Step 0 — Gather inputs
Confirm (ask concisely with the askQuestions tool if missing):
1. **Topic / Role** — subject, or pasted job description.
2. **My background** — current knowledge, so explanations are pitched right and strengths are leveraged.
3. **Goal** — default: "never go blank; know fundamentals + the concept behind each answer".
4. **Mode** — *interview prep* (default; include question bank + behavioral) or *pure learning* (omit interview-only sections).

### Step 1 — Master index first, then STOP
- Build a grouped index of Parts, ordered **foundations → core technical → applied/process → behavioral/closing**, each building on the last, leaning into the user's background, and (for a role) mapped to the JD's responsibilities.
- Save as `<Topic> - Study Guide.md` in the workspace root, with the grouped index plus a **linked Parts table with a Status column**.
- **Stop and ask the user to confirm/adjust the index** before writing any section content.

### Step 2 — One Part at a time
- On "next" (or a named Part), create that Part as `prep/Part-<X>-<name>.md`.
- Update the master file's status table after each Part.
- Give a short summary (contents + 2–3 top takeaways) and ask what's next. Don't batch ahead.

## Rules for every section file
See [references/section-template.md](references/section-template.md) for the full structure to follow. Key rules:
- **Assume zero prior knowledge.** Explain every term before use, in plain English, with a real-world **analogy**.
- Use **Mermaid diagrams** liberally (flowcharts, sequence diagrams, comparisons).
- Add **🔍 Plain-English deep-dive** callouts for dense concepts.
- Use **tables** for X-vs-Y comparisons and quick reference.
- **Tie back to the user's background** for natural examples/advantages.
- End every file with **⭐ Likely Interview Questions** (5–8 with model answers), **🧠 30-Second Memory Hooks**, and a pointer to the next section.

## Required near-the-end Parts (interview-prep mode)
- **Miscellaneous & Deeper Topics** — adjacent/advanced material, competitive landscape, standards, current trends ("extra edge").
- **Interview Question Bank** — 100+ questions, ~20% Basic / 20% Intermediate / 60% Advanced, plus Behavioral (STAR) and Closing/"why"; each with a concise answer/hint and a cross-reference; include a self-quiz tracker.
- **Behavioral & Closing** — STAR method; background→competency translation table; ready-to-adapt STAR stories; "why this move / company / you" answers; questions to ask the interviewer; one-page night-before cheat sheet.

## Admin rules
- Master index in root; Parts in `prep/`. Use workspace-relative Markdown links (never backticked).
- Only create files the user has advanced to; don't pre-generate everything.
- If the user gives style feedback, apply it to the current file, **carry it forward** to all future Parts, and offer to back-fill earlier ones.

## Honesty
If asked "am I ready now?", be candid: reading builds knowledge, but readiness also needs answering aloud, writing real STAR stories, and mock practice. Offer a mock interview or to co-write STAR stories.
