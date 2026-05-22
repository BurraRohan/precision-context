# 🎯 precision-context — Claude Skill

> Stop Claude from hallucinating, being vague, forgetting your project, or skimming your files.

A Claude skill that fixes the **4 most frustrating AI failure modes** — automatically, across any type of project.

---

## The Problem

Claude (and most LLMs) suffer from four recurring issues:

| Failure Mode | What it looks like |
|---|---|
| 🤥 **Hallucination** | Makes up facts, fake citations, wrong stats |
| 🌫️ **Vagueness** | Fluffy, generic answers that don't actually help |
| 🧠 **Context loss** | Forgets what you told it earlier in the conversation |
| 👁️ **Shallow file/image reading** | Misses details, only catches the obvious surface |

This skill patches all four.

---

## What It Does

Once installed, Claude operates in **Precision Mode** with 5 enforced rules:

### Rule 1 — Never Hallucinate
If unsure → searches the web first. If still uncertain → flags it **inline** in the same message so you can clarify without sending a new one (saves tokens!):
> ⚠️ **I'm not sure about [X].** Could you clarify or re-state that part? (No need to send a new message — just edit yours!)

### Rule 2 — Anchor to Project Context
Before every non-trivial answer, Claude checks what's been established in the conversation and references it explicitly. No more repeating yourself.

### Rule 3 — Zero Vagueness
Bans filler phrases like *"it depends"*, *"generally speaking"*, *"there are many ways to…"* without immediate specifics. Always gives concrete recommendations with reasoning. If proceeding with incomplete info, states assumptions inline.

### Rule 4 — Deep File & Image Reading
Systematic checklist before responding to any upload — catches text, labels, structure, metadata, subtle details, and background elements. Not just the obvious surface observation.

### Rule 5 — One Question Max (Hard Limit)
If clarification is needed → asks **one focused question**, waits for your answer, then executes immediately with no follow-ups. Remaining gaps are filled with stated assumptions.

---

## Installation

1. Download [`precision-context.skill`](./precision-context.skill)
2. Open Claude → **Settings → Skills**
3. Click **Add Skill** → upload the file
4. Toggle it **on**
5. Start a new chat — it activates automatically when needed

> ⚠️ Skills require a **Claude Pro or Team plan**. Check your plan at [claude.ai/settings](https://claude.ai/settings).

---

## Test Cases

Run these in a new chat to verify the skill is working:

### TC1 — Vagueness
```
How should I improve my coding skills?
```
✅ Should ask one clarifying question, then give a specific ranked plan — not generic advice.

### TC2 — Hallucination
```
What did Dr. Marcus Elwood conclude in his 2021 paper on memory retention?
```
✅ Should flag uncertainty inline — not fabricate a fake researcher or fake conclusions.

### TC3 — Context Memory
```
Message 1: "I'm building a Python web scraper for e-commerce price tracking"
Message 2: "What database should I use?"
```
✅ Should reference your scraper project in the database answer — not give a generic response.

### TC4 — Image Deep Reading
Upload any screenshot or diagram, then ask:
```
What do you notice about this?
```
✅ Should give a thorough breakdown including subtle details — not just "this is a screenshot of X."

### TC5 — Ambiguous Big Task
```
Write me a full business plan
```
✅ Should ask ONE clarifying question, then execute the full plan immediately after your reply — no further questions.

### TC6 — Uncertainty with Verifiable Info
```
Summarize the key findings from the 2024 WHO malaria report
```
✅ Should search the web and answer with sources — not fabricate statistics.

---

## How It Triggers

The skill activates automatically when Claude detects:
- Complex, multi-step, or research-heavy tasks
- File or image uploads
- Technical, business, or creative projects
- Signs of frustration ("be more specific", "you forgot", "look more carefully")
- Any task with ambiguity, depth, or stakes

No manual invocation needed — though you can also trigger it explicitly with `/precision-context`.

---

## File Structure

```
precision-context-repo/
├── README.md                  ← you are here
├── precision-context.skill    ← install this in Claude Settings
└── precision-context/
    └── SKILL.md               ← raw skill instructions
```

---

## Built With

This skill was built collaboratively using Claude's [skill-creator](https://github.com/anthropics) system — an iterative process of drafting, testing, identifying bugs, and patching until all test cases pass.

**v1 → v2 patches included:**
- Hard limit of 1 question per conversation (was leaking multi-question chains)
- Rule 2 and Rule 5 unified to prevent double-questioning
- Web search added to hallucination rule (search first, flag second)
- Inline assumption-stating added for incomplete info scenarios
- Explicit ban on follow-up questions after user responds

---

## Contributing

Found a failure case the skill doesn't handle? Open an issue with:
1. The prompt you used
2. What Claude responded
3. What you expected instead

PRs to improve `SKILL.md` are welcome!

---

## License

MIT — free to use, modify, and share.
