# LLM Engineering Cheatsheet

A timeless guide to **thinking and building like a prompt engineer**. This cheatsheet focuses on core principles and patterns that apply across any model, provider, or tool — whether you're using OpenAI, Claude, Llama, or something that doesn't exist yet.

> This is not a cookbook or quickstart. It's a mindset guide — built for those who want to reason clearly and build reliably with LLMs.

---

## Core Philosophy

LLMs are **probabilistic next-token predictors**, not deterministic logic machines. Prompt engineering is about:

- Designing **clear, structured inputs**
- Working within **context and token limits**
- Thinking **iteratively**, not magically
- Debugging failures like a **system**, not like a mystery

Treat prompts as **interfaces**, not incantations.

---

## Prompting Patterns (Universal)

### Zero-Shot

Ask the model to do a task with no examples.

```txt
"Summarize the following article in 3 bullet points: ..."
```

### One-Shot / Few-Shot

Give one or more examples to improve reliability.

```txt
Review: "Great product, but shipping was late."
Response: "Thanks for your feedback! Sorry about the delay..."

Review: "Terrible quality."
Response: "We're sorry to hear that. Could you share more details so we can improve?"
```

### Role-Based Prompting

Set a role for the model to adopt.

```txt
System: You are a technical support agent who speaks clearly and concisely.
User: My internet keeps cutting out. What should I do?
```

### Constrained Output

Ask for output formats explicitly.

```txt
"List the steps as JSON: [step1, step2, step3]"
```

---

## Prompt Structure: The Anatomy

Always structure prompts with these components:

1. **Role** – Who is the model?
2. **Task** – What do you want?
3. **Input** – What info do they need?
4. **Constraints** – What form should the output take?
5. **Examples** _(optional)_ – Show what success looks like

### Example Prompt (all parts applied)

```txt
System: You are a helpful travel assistant that gives concise city guides.
User: I’m visiting Tokyo for 3 days. Suggest an itinerary with 3 activities per day.
Constraints:
- Format your response as bullet points grouped by day.
- Keep each activity description under 20 words.
Example:
Day 1:
- Visit Meiji Shrine in the morning
- Eat sushi at Tsukiji Market
- Explore Shibuya Crossing at night
```

---

## Context Management

- Be **aware of token limits** (e.g. 4k, 8k, 128k)
- Use **summarization** for long chat histories
- Drop irrelevant history when possible
- **Explicit > implicit** — don't assume the model remembers everything

---

## Evaluation Principles

LLM output is fuzzy. Define quality like this:

- Does it meet the **task objective**?
- Is the output **formatted correctly**?
- Would a human say it's **reasonable**?
- Can you detect regressions with **A/B comparisons**?

---

## Common Failure Modes

| Symptom         | Likely Cause                             |
| --------------- | ---------------------------------------- |
| Hallucination   | Vague or underspecified prompts          |
| Repetition      | Poor constraint or unclear output format |
| Refusal         | Misalignment between task and role       |
| Loss of context | Too much history or poor summarization   |

---

## Recommended Resources

- [OpenAI Best Practices](https://platform.openai.com/docs/guides/prompt-engineering)
- [LangChain Docs](https://python.langchain.com/docs/introduction)
- [Ollama for Local Models](https://ollama.com)

---

## Minimal Python Example

```python
import os
from openai import OpenAI

client = OpenAI(api_key=os.getenv("OPENAI_API_KEY"))

response = client.chat.completions.create(
  messages=[
    {
      "content": "You are a concise technical writer.",
      "role": "system"
    },
    {
      "content": "Explain what a vector database is in simple terms.",
      "role": "user"
    }
  ],
  model="gpt-4",
)

print(response.choices[0].message.content)
```

---

## Final Thought

This guide helps you stay grounded when everything else is changing. Focus on clarity. Prompt with intent. And always think like an engineer.
