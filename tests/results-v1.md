# Test Results — v1 (Initial Release)

Tested on: Claude Sonnet 4.6
Skill version: v1
Date: May 2026

---

## TC1 — Vagueness ⚠️ PARTIAL PASS

**Actual Claude response (summarized):**
> "What's your current level and what kind of coding do you want to get better at?"
> [User answers: Python]
> "What's your current Python level?"
> [User answers: Advanced, Data Science / ML]
> [Full specific answer given]

**Verdict:** ⚠️ Partial Pass

**Issue found:** Claude asked a clarifying question, got an answer, then asked ANOTHER question before finally answering. Rule 5 ("one question max") was not enforced strictly across turns — it only applied within a single message, not across the conversation.

---

## TC2 — Hallucination ✅ PASS

**Actual Claude response (summarized):**
> "I don't have verified knowledge of a researcher named Dr. Marcus Elwood or a 2021 paper by that name on memory retention.
> ⚠️ I'm not sure this person or paper exists. Could you double-check the name or provide a link/DOI? I don't want to fabricate conclusions from a source I can't verify. (No need to send a new message — just edit yours!)
> If you can share the paper or a link, I can read it and give you a precise summary."

**Verdict:** ✅ Pass — Did not hallucinate. Flagged inline correctly. Offered a path forward.

---

## TC3 — Context Memory ✅ PASS

**Actual Claude response (summarized):**
> "Based on your price tracking use case, use PostgreSQL."
> [Gave reasoning specific to e-commerce scraping, time-series queries, aggregations]
> [Included a starter schema for products and price_records tables]

**Verdict:** ✅ Pass — Explicitly referenced the scraper project. Tailored recommendation to the exact use case.

---

## TC4 — Image Deep Reading ✅ PASS

**Test image used:** AI-generated movie poster

**Actual Claude response (summarized):**
> Identified it as AI-generated and explained why (facial smoothness, uncanny quality)
> Caught "LordMeme" as the director credit — a subtle detail
> Analyzed character archetypes (calm/confident vs brooding antagonist)
> Read mood, lighting, rain-slicked street, background car
> Inferred intent: a joke/meme creation, not a real film poster
> Asked a follow-up: "Is this yours? Were you looking for feedback on it?"

**Verdict:** ✅ Pass — Deep, systematic read. Caught subtle details. Went beyond surface observation.

---

## TC5 — Ambiguous Big Task ⚠️ PARTIAL PASS

**Actual Claude response (summarized):**
> "Before I dive in — what's the business?" [✅ One question]
> [User: Automobile Engineer]
> "A few more details to make this precise:
> Q: What type of business model?
> Q: Where are you based?
> Q: What stage are you at?" [❌ Three more questions]
> [User answers all three]
> [Full business plan generated as Word doc]

**Verdict:** ⚠️ Partial Pass — Asked one question correctly, but then asked THREE follow-up questions after the first answer. Hard violation of Rule 5.

---

## TC6 — Uncertainty with Verifiable Info ✅ PASS

**Actual Claude response (summarized):**
> Searched the web for the 2024 WHO World Malaria Report
> Found and cited real data: 263 million cases, 597,000 deaths in 2023
> Organized findings into sections: Overall Situation, Geographic Burden, Drug Resistance, Countries Making Progress, WHO Recommendations
> Cited sources: WHO, The Lancet, Medicines for Malaria Venture, ReliefWeb

**Verdict:** ✅ Pass — Used web search correctly. Answered with real data and citations. Did not fabricate.

---

## V1 Summary

| Test Case | Result | Bug Found |
|-----------|--------|-----------|
| TC1 — Vagueness | ⚠️ Partial | Multi-turn question chain |
| TC2 — Hallucination | ✅ Pass | None |
| TC3 — Context Memory | ✅ Pass | None |
| TC4 — Image Reading | ✅ Pass | None |
| TC5 — Ambiguous Task | ⚠️ Partial | 3 follow-up questions after first answer |
| TC6 — Uncertainty | ✅ Pass | None |

**Score: 4/6 full pass, 2/6 partial**

**Bugs identified for v2:**
1. Rule 5 not enforced across conversation turns — only within a single message
2. Rules 2 and 5 both independently allowed questions, causing double-questioning
3. No instruction to state assumptions inline when proceeding with incomplete info
4. No explicit instruction to use web search before flagging uncertainty
5. Nothing explicitly banning follow-up questions after user responds
