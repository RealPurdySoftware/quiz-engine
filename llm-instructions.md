# Quiz Format & LLM Generation Instructions

This file documents the JSON format used by [Quiz Engine](./README.md) and gives an LLM everything it needs to generate a compatible quiz file. If you're an LLM reading this: follow the instructions below exactly.

---

## Instructions for Humans

Paste this whole file into a conversation with an LLM along with your source material (notes, a textbook chapter, a study guide) and ask it to generate a quiz.

### Example prompt to paste into an LLM

```
Here is my study material: [paste or attach your notes/chapter/study guide]

Generate a multiple choice quiz from this material following the schema
and rules in the attached llm-instructions.md file. Output only the raw
JSON, no commentary or code fences.
```

---

## Instructions for the LLM

Generate a multiple choice quiz from the material the user provides. Output **a single JSON object** matching the schema below — no other text, no markdown code fences, no preamble or commentary. Just the raw JSON, ready to be saved directly as a `.json` file.

### Schema

```json
{
  "title": "string",
  "description": "string",
  "questions": [
    {
      "question": "string",
      "options": ["string", "string", "string", "string"],
      "correctIndex": 0,
      "explanation": "string"
    }
  ]
}
```

### Field requirements

| Field | Type | Required | Rules |
|---|---|---|---|
| `title` | string | yes | Short, descriptive title for the quiz (e.g. subject + scope, like "Cell Biology: Chapter 4"). |
| `description` | string | yes | One sentence summarizing what the quiz covers. Can be empty string `""` if not useful. |
| `questions` | array | yes | At least 1 question; cover the source material comprehensively rather than clustering on one part of it. |
| `questions[].question` | string | yes | Plain text only. No markdown, no HTML — it is rendered as-is. |
| `questions[].options` | array of strings | yes | 4 options is standard for exam-style multiple choice. Minimum 2, maximum 6. |
| `questions[].correctIndex` | integer | yes | **Zero-based** index into `options` for the correct answer. `0` = first option, `1` = second, etc. Must satisfy `0 <= correctIndex < options.length`. |
| `questions[].explanation` | string | yes | A real explanation of *why* the correct answer is correct — not just a restatement of the answer. This is shown to the learner after every question, right or wrong, so it carries most of the studying value. |

### Content rules

- **Plausible distractors.** Wrong options should be believable, not obviously wrong. Avoid filler options like "all of the above" or "none of the above."
- **One correct answer per question.** Don't write options where two could reasonably be defended as correct.
- **No copy-pasted text.** Write questions and explanations in your own words rather than lifting sentences verbatim from the source material.
- **Vary question style.** Mix direct recall ("What is X?") with applied/scenario questions ("A client presents with X — what's the correct response?") where the source material supports it. Scenario questions test understanding more rigorously than recall alone.
- **Double-check `correctIndex` against `explanation`.** Before finalizing output, verify that the index you've set actually points to the option your explanation describes as correct. This is the most common error in LLM-generated quizzes.

### Minimal valid example

```json
{
  "title": "Tiny Example Quiz",
  "description": "A two-question sanity check.",
  "questions": [
    {
      "question": "2 + 2 = ?",
      "options": ["3", "4", "5"],
      "correctIndex": 1,
      "explanation": "2 + 2 = 4."
    },
    {
      "question": "What is the chemical symbol for gold?",
      "options": ["Go", "Gd", "Au", "Ag"],
      "correctIndex": 2,
      "explanation": "Au comes from the Latin word for gold, 'aurum.' Ag is silver, and Go/Gd are not real element symbols."
    }
  ]
}
```

---

## Validation rules (what the app checks on load)

Quiz Engine validates files when they're loaded and shows an error instead of crashing if a file doesn't pass:

- `questions` must be a non-empty array
- Each question needs a `question` string
- Each question needs an `options` array with at least 2 entries
- Each question needs a numeric `correctIndex` that's a valid index into its own `options` array

The app **cannot** validate that an answer is factually correct — only that the file is structurally well-formed. A model can produce a valid file with a wrong answer key, so it's worth skimming a generated quiz before studying from it.

---

## Tips for better results

- **Chunk your source material.** Feed a chapter or section at a time rather than an entire book in one request — quality drops noticeably on very large single requests.
- **Ask for harder questions explicitly** if recall-level questions feel too easy: "write these as applied scenario questions, not direct definitions."
- **Regenerate selectively.** If a handful of questions feel off after reviewing, ask the LLM to regenerate just those rather than the whole file.
- **Keep explanations self-contained.** A good explanation should make sense even to someone who got the question wrong and has the source material closed — it's there to teach, not just confirm.
