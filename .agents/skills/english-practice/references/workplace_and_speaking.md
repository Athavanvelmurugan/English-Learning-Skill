# Workplace English, Technical Scenarios & Speaking Framework

This reference document outlines real-world software engineering communication scenarios, role-play simulator configurations, email practice templates, and speaking evaluation frameworks for the English Practice Skill.

---

# 1. Workplace Communication Ladder

Practice scenarios progress through 10 stages:

```text
1. Basic Statement  ──► 2. Direct Question  ──► 3. Polite Request
                                                     │
6. Team Discussion  ◄── 5. Detailed Explanation ◄── 4. Clarification
         │
7. Professional Disagreement ──► 8. Negotiation ──► 9. Sprint Demo ──► 10. Leadership
```

---

# 2. Key Workplace Scenarios

## Scenario A — Daily Stand-up Update
- **Prompt:** State what you completed yesterday, what you are working on today, and any blockers.
- **Expected Key Patterns:**
  - `Yesterday, I completed [task / module].`
  - `Today, I am working on [feature / API integration].`
  - `Currently, I don't have any blockers.` / `I have a blocker regarding [issue].`

## Scenario B — Reporting a Bug to Team Lead
- **Prompt:** Report a critical backend 500 error found during testing.
- **Expected Key Structure:**
  - **Problem:** `I found a issue with...` → *an issue with the authentication API.*
  - **Impact:** `It affects the login flow for existing users.`
  - **Reproduction:** `It can be reproduced by sending an expired token.`
  - **Next Action:** `I am currently investigating the logs to trace the root cause.`

## Scenario C — Asking for Clarification
- **Key Phrasing Patterns:**
  - `Could you clarify the requirement regarding [feature]?`
  - `Just to confirm, should we [Option A] or [Option B]?`
  - `Do you mean that the API should return a 404 status code when user is missing?`

## Scenario D — Professional Disagreement
- **Key Phrasing Patterns:**
  - `I see your point regarding [Approach A]; however, [Approach B] might be more scalable because...`
  - `That makes sense, but could we consider [alternative]?`
  - `I understand the timeline constraint, but skipping unit tests could increase production risk.`

---

# 3. Role-Play Simulator Configurations

When running `conversation` or `interview` mode, the assistant adopts one of the following personas:

1. **Engineering Manager:** Focuses on timelines, blockers, risk mitigation, and team coordination.
2. **Senior Developer / Tech Lead:** Focuses on code quality, design patterns, testing, performance, and architecture.
3. **Client / Product Owner:** Focuses on business value, user experience, features, and delivery dates.
4. **Technical Interviewer:** Asks core engineering questions (*"Explain your project"*, *"Tell me about a difficult bug you solved"*).

> **Rule:** The assistant presents the role persona's dialogue first, waits for the learner's response, and ONLY THEN provides grammar, natural phrasing, and professional style corrections.

---

# 4. Speaking Practice Framework & Text Disclaimer

Fluency prompts:
- **Beginner (30s):** *Describe your daily routine at work.*
- **Intermediate (1-2 min):** *Explain a technical bug you fixed recently.*
- **Advanced (2-5 min):** *Defend choosing PostgreSQL over MongoDB for your application.*

Evaluation criteria for typed speaking responses:
1. **Grammatical Accuracy:** Tense consistency, subject-verb agreement, prepositions.
2. **Spoken Naturalness:** Use of natural connectors (`so`, `actually`, `basically`, `in terms of`).
3. **Conciseness & Structure:** Clear beginning, middle, and conclusion.

> **Mandatory Disclaimer:** Always include: *"I can evaluate your written phrasing, grammar, and spoken structure, but pronunciation and vocal cadence cannot be evaluated from text alone."*
