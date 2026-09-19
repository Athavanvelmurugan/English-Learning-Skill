# MCQ Engine & Targeted Exercise Framework

This specification defines how the English Practice Skill must generate **unique, adaptive, non-repetitive questions** and targeted exercises.

The primary goal is:

> **Do not repeatedly ask the same question, sentence pattern, or answer pattern. Test the same concept through different contexts, structures, and cognitive patterns.**

---

# 1. Core Question Generation Rules

## 1.1 Default Quiz

Generate **10 questions per session**.

Difficulty distribution:

* Questions 1–3 → Basic
* Questions 4–7 → Intermediate
* Questions 8–10 → Advanced

However, difficulty must be adapted based on the learner's previous performance.

---

# 2. Anti-Repetition Engine

Before generating every question, check the learner's previous questions and mistakes.

Track:

* Exact question
* Sentence structure
* Grammar topic
* Sub-topic
* Correct answer
* Incorrect answers
* Question type
* Vocabulary used
* Context
* Difficulty
* Previous attempt result

## Never repeat

Do NOT generate:

1. The exact same sentence.
2. The same sentence with only words replaced.
3. The same question structure with reordered options.
4. The same blank position with identical grammar logic.
5. The same correct answer pattern repeatedly.
6. The same example used during previous explanations.
7. A question the learner has already answered correctly multiple times unless it is being used as spaced revision.

### Example

Previous:

> She ___ to the office every day.
> A. go
> B. goes ✅
> C. going
> D. gone

Do NOT generate:

> He ___ to school every day.
> A. go
> B. goes ✅
> C. going
> D. gone

This is effectively the same question.

Instead, test the same concept differently:

> Which sentence is grammatically correct?

A. She go to office every day.
B. She going to office every day.
C. She goes to office every day. ✅
D. She gone to office every day.

---

# 3. Question Diversity Engine

For every grammar topic, rotate different question formats.

Use:

### Type A — Fill in the Blank

> She ___ working on the project.

### Type B — Error Identification

> Identify the incorrect part.

> She don't likes coffee.

### Type C — Correct Sentence

> Choose the grammatically correct sentence.

### Type D — Sentence Completion

> If I had more time, I ___ the project.

### Type E — Contextual MCQ

Provide a short real-world situation and ask the learner to choose the correct sentence.

### Type F — Meaning-Based Grammar

Ask which sentence correctly expresses a particular meaning.

### Type G — Transformation

Convert:

> "I completed the task."

into the appropriate passive/negative/question form.

### Type H — Workplace English

Use realistic professional situations:

* meetings
* emails
* stand-ups
* Jira
* Git
* coding
* testing
* interviews
* presentations
* discussions with managers

### Type I — Dialogue

> Manager: "Can you finish this today?"
> Developer: "Yes, I ___ it before 6 PM."

### Type J — Spot the Difference

Give two similar sentences and ask which one is correct and why.

---

# 4. Topic Diversity

Do not generate all 10 questions from one grammar pattern.

Example:

Bad:

1. am/is/are
2. am/is/are
3. am/is/are
4. am/is/are
5. am/is/are
6. am/is/are
7. am/is/are

Good:

1. Subject-verb agreement
2. Articles
3. Prepositions
4. Present tense
5. Past tense
6. Future tense
7. Pronouns
8. Conditionals
9. Passive voice
10. Relative clauses

The session should contain a **balanced topic mix** unless the learner is currently in a targeted remediation session.

---

# 5. Concept vs Question Separation

The engine must distinguish between:

**Concept**

> Present Simple — third-person singular

and

**Question**

> She works remotely.

Knowing the concept does NOT mean the exact question should be repeated.

A learner may be tested on the same concept using:

* positive sentence
* negative sentence
* question
* workplace sentence
* dialogue
* error correction
* sentence transformation
* contextual situation

---

# 6. Question Fingerprint

Every generated question should internally receive a fingerprint based on:

```text
topic
subtopic
question_type
grammar_pattern
sentence_structure
context
target_word
correct_answer
```

Example:

```text
present_simple
third_person
error_correction
workplace
subject_verb_agreement
```

If the fingerprint is too similar to a recently used question, generate another question.

---

# 7. Similarity Check

Before presenting a question, perform a mental/semantic duplication check.

Reject the question if it is:

* Exact duplicate
* Near duplicate
* Same sentence template
* Same grammar logic with superficial word substitution
* Same context + same answer logic
* Same question type repeatedly

Target:

> **Semantic diversity, not just textual diversity.**

Changing:

> She works in Chennai.

to:

> He works in Bangalore.

does NOT count as a new question if the grammar test is identical.

---

# 8. Answer Option Diversity

Do not repeatedly use the same correct option.

Bad:

```text
Q1 → B
Q2 → B
Q3 → B
Q4 → B
```

Rotate answer positions naturally:

```text
Q1 → B
Q2 → D
Q3 → A
Q4 → C
```

Do NOT make the pattern predictable.

Options must also be plausible.

Avoid obviously wrong distractors.

---

# 9. Distractor Generation

Wrong answers should represent **realistic learner mistakes**.

Example:

Target:

> She ___ completed the task.

Options:

A. have
B. has ✅
C. having
D. is

The distractors should correspond to common errors:

* subject mismatch
* incorrect tense
* incorrect auxiliary
* incorrect verb form

Do not create nonsense options merely to make the answer obvious.

---

# 10. Difficulty Engine

## Level 0 — Recognition

Learner identifies basic grammar.

Example:

> I ___ a developer.

A. am ✅
B. is
C. are
D. be

---

## Level 1 — Basic Application

Apply a known rule.

> She ___ to work every day.

---

## Level 2 — Contextual Application

Apply grammar in a realistic situation.

> I usually ___ the API before deploying it.

---

## Level 3 — Error Detection

Identify and correct realistic mistakes.

> He don't understand the requirement.

---

## Level 4 — Sentence Construction

Learner creates a sentence using the target grammar.

---

## Level 5 — Advanced Application

Combine multiple grammar concepts.

Example:

> If the team had reviewed the code earlier, the issue ___ before deployment.

---

# 11. Adaptive Difficulty

Calculate:

```text
Accuracy = Correct Answers / Total Answers × 100
```

### 90–100%

Increase difficulty.

* Introduce advanced grammar.
* Reduce obvious distractors.
* Increase contextual questions.
* Introduce mixed grammar.
* Increase sentence complexity.

### 75–89%

Maintain difficulty.

* Introduce new question formats.
* Increase contextual application.
* Mix related concepts.

### 50–74%

Focus on the weak concept.

* Reduce complexity.
* Explain the rule.
* Give targeted examples.
* Provide 3 new questions using different structures.

### Below 50%

Step back.

* Identify prerequisite knowledge.
* Teach the prerequisite.
* Use simple Tamil + English explanation when useful.
* Start from Level 0/1.
* Gradually rebuild difficulty.

---

# 12. Weak Topic Detection

Do not calculate only overall accuracy.

Calculate:

```text
Topic Accuracy
Subtopic Accuracy
Question-Type Accuracy
Recent Accuracy
Repeated-Mistake Count
```

Example:

```text
Overall: 82%

Articles: 95%
Prepositions: 90%
Past Tense: 80%
Present Perfect: 55%
Conditionals: 60%
```

The next session should focus more on:

> Present Perfect + Conditionals

rather than repeating topics the learner already understands.

---

# 13. Repeated Mistake Engine

Track mistakes in:

```text
english-practice/mistakes.md
```

Record:

```text
Date
Topic
Subtopic
Question
Learner Answer
Correct Answer
Mistake Type
Explanation
Strike Count
```

---

# 14. Three-Strike Protocol

Trigger targeted remediation when the same conceptual mistake occurs repeatedly.

### Strike 1

Normal correction.

Explain:

* What was wrong
* Correct rule
* One example

### Strike 2

Trigger:

```text
⚠️ Repeated mistake detected
Category: Present Perfect
Pattern: have/has + past participle
Strike: 2/3
```

Provide:

* concise rule
* 3 contrastive examples
* 3 new questions

### Strike 3

Start a dedicated remediation session.

Structure:

```text
1. Simple explanation
2. Rule
3. Common mistake
4. Contrast examples
5. 3 easy questions
6. 3 medium questions
7. 1 sentence construction
8. Final verification question
```

Do NOT repeat the previous questions.

---

# 15. Spaced Revision

A question answered correctly should not disappear forever.

Instead, revisit the **concept**, not the same question.

Example:

Session 1:

> She ___ working here since 2022.

Session 2:

Use a different structure:

> I have ___ this project for three years.

Session 3:

Use contextual conversation:

> "How long have you worked here?"

This tests the same concept without repeating the same question.

---

# 16. Question Rotation Matrix

For each topic, rotate:

```text
Grammar Concept
      ↓
Question Type
      ↓
Context
      ↓
Sentence Structure
      ↓
Vocabulary
      ↓
Difficulty
```

Example:

| Session | Concept         | Format                | Context    |
| ------- | --------------- | --------------------- | ---------- |
| 1       | Present Perfect | Fill blank            | Daily life |
| 2       | Present Perfect | Error correction      | Workplace  |
| 3       | Present Perfect | Dialogue              | Office     |
| 4       | Present Perfect | Transformation        | Technical  |
| 5       | Present Perfect | Sentence construction | Interview  |

This prevents repetitive practice.

---

# 17. Context Rotation

Rotate contexts to improve practical English.

Use:

* Daily life
* Office
* Software development
* Java, Python
* Meetings
* Interviews
* Emails
* Presentations
* Travel
* Shopping
* Friends
* Family
* Customer support
* Technical discussions
* ZOHO

Do not repeatedly use the same context unless the learner is specifically practicing that domain.

---

# 18. Sentence Construction

After MCQs, periodically require active production.

Examples:

### Guided

> Make a sentence using:
> `have been + verb-ing`

### Workplace

> Write one sentence explaining what you are currently working on.

### Transformation

> Change this sentence into a question.

### Free Production

> Write one sentence using "although".

Evaluate:

```text
Grammar
Word choice
Sentence structure
Article usage
Prepositions
Verb tense
Naturalness
Professional tone
```

---

# 19. Error Correction Framework

Provide realistic mistakes.

Example:

```text
Correct the following:

1. ❌ I am working here since two years.
2. ❌ She don't understand the requirement.
3. ❌ We discussed about the issue yesterday.
```

After the learner answers, provide:

### Tier 1 — Correction

```text
❌ I am working here since two years.

✅ I have been working here for two years.
```

### Tier 2 — Explanation

Explain:

* What was wrong
* Why it was wrong
* The rule
* One new example

Do not reuse the exact original sentence as the only teaching example.

---

# 20. Session-Level Deduplication

Within a single 10-question quiz:

* No duplicate questions.
* No duplicate sentence templates.
* No same correct answer more than 3 times consecutively.
* No same question type more than 3 times consecutively.
* Avoid testing the exact same grammar rule in consecutive questions unless it is a targeted drill.
* At least 5 different sentence structures should appear in a normal quiz.

---

# 21. Historical Deduplication

Across previous sessions:

### Recently asked

Avoid repeating for the next few sessions.

### Correct repeatedly

Reduce frequency.

### Incorrect repeatedly

Increase frequency, but change:

* sentence
* context
* question type
* vocabulary
* difficulty

### Example

Do NOT:

> She has ___ the work.

again and again.

Instead:

> Which sentence correctly describes an action that started in the past and continues now?

or:

> "How long ___ you ___ working here?"

or:

> Correct the sentence:

> I am working here since 2023.

All target the same underlying concept through different cognitive routes.

---

# 22. Question Quality Gate

Before presenting every question, verify:

```text
[ ] Exactly one correct answer
[ ] 3 or 4 options
[ ] Grammar is correct
[ ] Distractors are plausible
[ ] Question is understandable
[ ] Appropriate difficulty
[ ] Not previously asked
[ ] Not semantically duplicated
[ ] Not just a word substitution
[ ] Correct answer position is not predictable
[ ] Context is different when possible
[ ] Question tests the intended concept
```

If any major condition fails:

> Regenerate the question.

---

# 23. Learning Priority Algorithm

Prioritize concepts using:

```text
Priority =
Mistake Frequency
+ Recent Failure Rate
+ Difficulty
+ Time Since Last Revision
```

Concepts with repeated mistakes receive higher priority.

Concepts mastered repeatedly receive lower frequency and move into spaced revision.

---

# 24. Normal Quiz vs Targeted Drill

## Normal Quiz

Goal:

> Measure broad English ability.

Use:

* multiple topics
* different question types
* mixed contexts
* adaptive difficulty
* strong anti-repetition

## Targeted Drill

Goal:

> Fix one specific weakness.

Use:

* one grammar concept
* multiple sentence structures
* multiple contexts
* progressive difficulty
* immediate feedback

Do NOT mix unrelated grammar topics into a targeted drill.

---

# 25. Final Principle

The system must follow this rule:

> **Repeat the concept when necessary, but never lazily repeat the question.**

The learner should feel:

> "I am practicing the same skill in a new situation."

Not:

> "I am answering the same question again."

The objective is **mastery through variation**, not memorization through repetition.
