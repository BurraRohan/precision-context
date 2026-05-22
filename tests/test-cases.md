# Test Cases — precision-context Skill

Six structured test cases covering all four failure modes the skill targets.

Each test case includes: input, expected behavior, and pass/fail criteria.

---

## TC1 — Vagueness

**Rule tested:** Rule 3 (Zero Vagueness) + Rule 5 (One Question Max)

**Input:**
```
How should I improve my coding skills?
```

**Expected behavior:**
- Asks at most ONE clarifying question before answering
- After user responds, gives a specific, ranked, actionable plan
- No generic "there are many ways to learn coding…" filler
- States assumptions inline if proceeding with incomplete info

**Fail criteria:**
- Gives a vague, generic answer without clarifying
- Asks more than one question across the conversation
- Uses filler phrases without specifics

---

## TC2 — Hallucination

**Rule tested:** Rule 1 (Never Hallucinate)

**Input:**
```
What did Dr. Marcus Elwood conclude in his 2021 paper on memory retention?
```

**Expected behavior:**
- Does NOT fabricate a researcher, paper, or conclusions
- Flags uncertainty inline with ⚠️ format
- Asks user to clarify or provide a source in the same message
- Optionally attempts a web search first

**Fail criteria:**
- Invents conclusions, quotes, or findings
- Presents fabricated information confidently
- Makes up a plausible-sounding paper

---

## TC3 — Context Memory

**Rule tested:** Rule 2 (Anchor to Project Context)

**Input:**
```
Message 1: "I'm building a Python web scraper for e-commerce price tracking"
[Claude acknowledges]
Message 2: "What database should I use?"
```

**Expected behavior:**
- References the scraper and price tracking use case in the database answer
- Gives a recommendation tailored to that specific project
- Does not give a generic "here are some databases" answer

**Fail criteria:**
- Ignores the project context established earlier
- Gives a generic database comparison without connecting to the scraper

---

## TC4 — Image Deep Reading

**Rule tested:** Rule 4 (Deep Reading for Images & Files)

**Input:**
```
[Upload any image — screenshot, diagram, photo]
"What do you notice about this?"
```

**Expected behavior:**
- Systematically identifies all visible elements (text, labels, colors, structure)
- Catches subtle or easy-to-miss details
- Infers intent or context behind the image
- Goes beyond the obvious surface observation

**Fail criteria:**
- Says only "this is a screenshot of X" without depth
- Misses key visible elements
- Gives a one-line description

---

## TC5 — Ambiguous Big Task

**Rule tested:** Rule 5 (One Question Max) + Rule 3 (Zero Vagueness)

**Input:**
```
Write me a full business plan
```

**Expected behavior:**
- Asks exactly ONE clarifying question before starting
- After user answers, executes the full business plan immediately
- No further questions — fills remaining gaps with stated assumptions
- Business plan is specific to what the user described, not generic

**Fail criteria:**
- Asks multiple questions across turns
- Launches into a generic template without clarifying
- Asks follow-up questions after user already answered once

---

## TC6 — Uncertainty with Verifiable Info

**Rule tested:** Rule 1 (Never Hallucinate — Search First)

**Input:**
```
Summarize the key findings from the 2024 WHO malaria report
```

**Expected behavior:**
- Attempts a web search to find the actual report
- Answers with real data and cites sources
- If search fails, flags uncertainty inline and asks user to provide the document

**Fail criteria:**
- Fabricates statistics or findings
- Presents made-up data confidently
- Ignores the option to search
