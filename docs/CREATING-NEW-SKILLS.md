# Creating New Skills

A guide for adding new skills to this library.

## What is a Skill?

A skill is a markdown file that teaches Claude how to perform a specific task. It includes:

- **Metadata** (name, description) so Claude knows when to use it
- **Instructions** for how to perform the task
- **Input/Output formats** so skills can chain together
- **Edge cases** for production reliability

## Quick Start

1. Create a new folder in `skills/`:

```
   mkdir skills/your-skill-name
```

2. Create `SKILL.md` inside it:

```
   touch skills/your-skill-name/SKILL.md
```

3. Use the template below.

## Skill Template

````markdown
---
name: your-skill-name
description: One sentence describing what this skill does and when to use it.
---

# Skill Name

Brief description of what this skill accomplishes.

## When to Use

- Situation where this skill applies
- Another situation
- Another situation

## Instructions

When asked to [perform this task]:

1. First step
2. Second step
3. Third step

## Input Format

This skill accepts either:

**Structured data:**
\```json
{
"field": "description"
}
\```

**Or natural language:**

- Example prompt a user might give
- Another example prompt
- What kinds of pasted content are acceptable

Claude will normalize natural language input into structured data before processing.

## Output Format

What this skill returns:

\```json
{
"field": "description"
}
\```

## Edge Cases

- **[Edge case 1]**: How to handle it
- **[Edge case 2]**: How to handle it
- **[Edge case 3]**: How to handle it

## Example

**Input (structured)**:
\```json
{
"example": "input"
}
\```

**Input (natural language)**:

> Example of how a user might ask for this in plain English

**Output**:
\```json
{
"example": "output"
}
\```
````

## Design Principles

### 1. Composability

Design inputs and outputs so skills can chain together.

- Output of skill A should be valid input for skill B
- Use consistent JSON structures
- Document your formats clearly

### 2. Dual Input Support

Skills should accept both structured data and natural language.

- Technical users and automations can pass JSON
- Non-technical users can describe what they want in plain English
- Claude normalizes both into the same internal format

### 3. Clear Boundaries

Each skill should do one thing well.

- ✅ "Research a topic and return sources"
- ❌ "Research a topic, summarize it, format it, and post to Slack"

### 4. Edge Case Handling

Production skills need to handle failure gracefully.

Always include:

- What to do when input is incomplete
- What to do when results are insufficient
- When to flag for human review

### 5. Useful Defaults

Skills should work well with minimal input.

- Provide sensible defaults
- Don't require optional parameters
- Make the common case easy

## Checklist Before Submitting

- [ ] Skill has clear name and description in frontmatter
- [ ] Instructions are specific and actionable
- [ ] Input format documents BOTH structured JSON and natural language options
- [ ] Output format is documented with JSON example
- [ ] Edge cases are defined
- [ ] At least one full example is included (ideally both input types)
- [ ] Skill does one thing well (not multiple unrelated tasks)
- [ ] Output format can be consumed by other skills if relevant

## Examples

See existing skills for reference:

- `skills/web-research/` - Gathers and structures source data
- `skills/content-summarizer/` - Synthesizes sources into takeaways
- `skills/newsletter-formatter/` - Formats for TLDR-style output
