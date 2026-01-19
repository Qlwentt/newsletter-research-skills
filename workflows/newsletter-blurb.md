# Newsletter Blurb Workflow

Generate a publication-ready newsletter blurb from a topic in one conversation.

## Overview

This workflow chains three skills:

1. **web-research** → gathers sources on your topic
2. **content-summarizer** → synthesizes sources into key takeaways
3. **newsletter-formatter** → outputs a TLDR-style blurb

You don't need to call each skill separately. Just describe what you want and Claude will use them automatically.

## How to Use

### Option 1: One-shot prompt

Ask Claude:

> Research recent news about [YOUR TOPIC] and write a TLDR-style newsletter blurb with a linked headline.

Claude will research, synthesize, and format in one pass.

### Option 2: Step-by-step (more control)

**Step 1 - Research:**

> Research recent news about [YOUR TOPIC]. Return structured source data.

Review the sources. If you want different angles, ask Claude to research more.

**Step 2 - Summarize:**

> Summarize these sources. What's the core narrative?

Review the summary. Adjust if needed.

**Step 3 - Format:**

> Format this as a TLDR-style newsletter blurb with the headline linked to the primary source.

## Example

**Prompt:**

> Research the latest developments in AI code assistants and write a TLDR-style newsletter blurb.

**Output:**

**[78% of Developers Now Use AI Coding Assistants](https://stackoverflow.com/survey/2025)**

Up from 52% just a year ago, according to Stack Overflow's latest survey. The surge in adoption is driving massive valuations—Cursor just raised at $1B—and shifting product focus from individual productivity to enterprise team features. AI-assisted coding is quickly becoming table stakes, not a competitive advantage.

## Tips

- **Be specific with your topic.** "AI code assistants" gets better results than "AI stuff."
- **Specify a time frame if needed.** "News from the past week" helps focus the research.
- **Ask for revisions.** "Make the headline punchier" or "Focus more on the funding angle" work naturally.

## Troubleshooting

| Problem               | Solution                                             |
| --------------------- | ---------------------------------------------------- |
| Sources are outdated  | Ask: "Find more recent sources from the past 7 days" |
| Blurb is too long     | Ask: "Tighten this to under 80 words"                |
| Wrong angle           | Ask: "Rewrite focusing on [specific aspect]"         |
| Headline isn't catchy | Ask: "Give me 3 alternative headlines"               |
