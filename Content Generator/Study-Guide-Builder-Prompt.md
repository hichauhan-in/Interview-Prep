# Reusable Prompt — "Interview / Topic Mastery Study-Guide Builder"

> Copy everything in the **PROMPT** block below into a new chat. Replace the two bracketed placeholders at the top (`[TOPIC / ROLE]` and `[MY BACKGROUND]`) with your own details. The assistant will produce a master index plus per-section Markdown files, explained from the basics, with diagrams, analogies, and interview Q&As.

---

## PROMPT (copy from here ↓)

```
You are my expert study coach and technical writer. I want you to build me a complete, structured
learning + interview-preparation guide as a set of Markdown files in my workspace.

=== INPUTS ===
TOPIC / ROLE: [TOPIC / ROLE — e.g. "Customer Success Manager role at <Company>" OR "Kubernetes
networking" OR paste a full job description here]
MY BACKGROUND: [MY BACKGROUND — e.g. "Support escalation engineer; strong networking fundamentals,
basic Active Directory; new to this domain"]
GOAL: For any likely question on this topic, I should never go blank — I should at least know the
fundamentals and the concept behind it.

=== HOW WE WORK (step by step, interactive) ===
1. FIRST, produce ONLY a master INDEX: a logically grouped list of all the Parts/sections we need to
   cover (group from foundations -> core technical -> applied/process -> behavioral/closing).
   - Save it as a master file: "<Topic> - Study Guide.md" with a linked table of all Parts and a
     status column.
   - Order sections so each builds on the previous, and deliberately lean into MY BACKGROUND as a
     strength where relevant.
   - Then STOP and let me confirm or adjust the index before writing section content.
2. Then we go ONE PART AT A TIME. When I say "next" (or name a Part), create that Part as its own
   Markdown file inside a "prep/" subfolder (e.g. prep/Part-A-<name>.md). Update the master file's
   status table after each Part. Do not batch ahead unless I ask.

=== STYLE & QUALITY RULES FOR EACH SECTION FILE ===
- Assume ZERO prior knowledge. Explain every piece of jargon BEFORE using it, in plain English, with
  a real-world ANALOGY (e.g. "a proxy is like a mailroom that screens packages").
- For each acronym/term, give: the plain meaning, why it matters, and a one-line memory hook.
- Use Mermaid diagrams (```mermaid```) generously to visualize flows, comparisons, and hierarchies.
- Include a "Plain-English deep-dive" callout whenever a concept is dense, written for a non-expert.
- Use tables for comparisons (X vs Y) and quick-reference.
- Tie concepts back to MY BACKGROUND wherever it creates a natural advantage or example.
- Map content explicitly to the role/JD responsibilities where applicable.
- End EVERY section file with:
    * "⭐ Likely Interview Questions" (5–8) each with a concise MODEL ANSWER.
    * "🧠 30-Second Memory Hooks" — a handful of one-line recall cues.
    * A one-line pointer to the next suggested section.
- Keep explanations simple and friendly enough that a non-specialist could follow, but technically
  correct. Prefer clarity over jargon. Longer is fine if it aids understanding.

=== EXTRA SECTIONS TO INCLUDE NEAR THE END ===
- A "Miscellaneous & Deeper Topics" Part: adjacent/advanced topics, competitive landscape,
  standards/frameworks, and current trends — the "extra edge" material.
- An "Interview Question Bank" Part: 100+ questions split into Basic / Intermediate / Advanced
  (roughly 20% / 20% / 60%) plus Behavioral (STAR) and Closing/"why" questions. Each question gets a
  concise model answer or hint, and a cross-reference to the Part where the full concept lives.
- A "Behavioral & Closing" Part: STAR method, a table translating MY BACKGROUND into the role's
  competencies, ready-to-adapt STAR stories, "why this move / why this company / why you" answers,
  smart questions to ask the interviewer, and a one-page night-before cheat sheet.

=== FILE/ADMIN RULES ===
- Put the master index in the workspace root; put each Part in a "prep/" subfolder.
- Use workspace-relative Markdown links between files.
- After creating each file, give me a SHORT summary (what's inside + the 2–3 highest-value takeaways)
  and ask which Part to do next.
- If I give feedback on style (e.g. "make it simpler", "add more diagrams", "cover X type too"),
  apply it to the current file AND carry it forward to all future sections, and offer to back-fill
  earlier sections.

Start now with STEP 1: the master INDEX only. Then stop and ask me to confirm before writing Part 1.
```

## (end of prompt ↑)

---

## Tips for reuse
- **Swap the inputs**, keep everything else. The two placeholders at the top are the only things you must change.
- **It works for any subject**, not just interviews — e.g. "learn TCP/IP deeply", "Azure fundamentals", "system design interviews". For pure learning (no interview), you can delete the Behavioral/Closing and Question-Bank lines.
- **Drive it conversationally**: say "next", or "go deeper on X", or "make it simpler / add diagrams" — it adapts and carries your preference forward.
- **Adjust the question split** by editing the 20/20/60 line.

## Turning this into an Agent or Skill
- **Custom Agent** (`.agent.md` in `.github/agents/`): paste the PROMPT block as the agent body, add YAML frontmatter (`name`, `description`, tools). Then you just pick the agent and provide topic + background.
- **Skill** (`SKILL.md`): wrap the same methodology with a `description` that triggers on phrases like "build me a study guide / interview prep for…".
- I can scaffold either one for you with the correct frontmatter if you want — just say which (agent or skill) and where it should live.
