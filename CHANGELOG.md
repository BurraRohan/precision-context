# Changelog

All notable changes to the precision-context skill are documented here.

---

## [v2] — May 2026

### Fixed
- **Rule 5 — Hard question limit now enforced across entire conversation, not just per message**
  - v1 behavior: Claude would ask one question, receive an answer, then ask more questions in subsequent turns
  - v2 behavior: One question maximum across the entire conversation. After user answers, Claude executes immediately with no further questions
  - Affected test cases: TC1, TC5

- **Rules 2 and 5 conflict resolved**
  - v1 behavior: Rule 2 ("ask ONE question if context is missing") and Rule 5 ("ask one clarifying question") operated independently, allowing Claude to ask questions from two separate rules in the same conversation
  - v2 behavior: Rule 2 now explicitly defers to Rule 5's question budget — both rules share the same single-question allowance

- **Inline assumption-stating added (Rule 3)**
  - v1 behavior: When proceeding with incomplete information, Claude would just answer without flagging what it assumed
  - v2 behavior: Claude must state assumptions inline before proceeding: "(Assuming X — correct me if wrong)"
  - Affected test cases: TC1, TC5

- **Web search added to hallucination rule (Rule 1)**
  - v1 behavior: On uncertain claims, Claude would immediately flag uncertainty and ask user to clarify
  - v2 behavior: Claude first attempts web search to verify. If search confirms → answers with source. If search fails or unavailable → flags inline
  - Affected test cases: TC6 (was passing by luck in v1; now explicit in the rule)

- **Explicit ban on follow-up questions after user responds (Tone section)**
  - v1 behavior: No explicit instruction preventing Claude from asking further questions after receiving an answer
  - v2 behavior: Tone section now explicitly states "Never ask multiple questions in one message" and Rule 5 explicitly bans follow-ups after the user has answered

### No regressions
All test cases that passed in v1 (TC2, TC3, TC4, TC6) continue to pass in v2.

---

## [v1] — May 2026

### Initial release

Five rules covering the four core LLM failure modes:
- Rule 1: Never Hallucinate
- Rule 2: Anchor to Project Context
- Rule 3: Zero Vagueness Policy
- Rule 4: Deep Reading for Images & Files
- Rule 5: Clarify Before Big/Ambiguous Tasks

**Test results:** 4/6 full pass, 2/6 partial pass
**Known issues:** Multi-turn question chaining (TC1, TC5) — fixed in v2
