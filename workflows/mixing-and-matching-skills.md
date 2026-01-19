# Mixing and Matching Skills

You don't have to use all three skills together. Mix and match based on what you need.

## Available Skills

| Skill                    | What it does                        | Input   | Output                     |
| ------------------------ | ----------------------------------- | ------- | -------------------------- |
| **web-research**         | Finds recent sources on a topic     | A topic | Structured list of sources |
| **content-summarizer**   | Distills sources into key takeaways | Sources | Summary with insights      |
| **newsletter-formatter** | Formats for TLDR-style newsletter   | Summary | Ready-to-publish blurb     |

## Two Ways to Use Skills

### Option A: Natural Language (Recommended for most users)

Just tell Claude what you want in plain English. Claude figures out which skills to use.

### Option B: Structured Data

Pass JSON between skills for precise control. Useful when integrating with other tools or automations.

Both approaches work with any combination below.

## Common Combinations

### Full Newsletter Blurb

**Skills:** web-research → content-summarizer → newsletter-formatter

**When to use:** You have a topic and want a publish-ready blurb.

**Natural language:**

> Research [TOPIC] and write a TLDR-style newsletter blurb with a linked headline.

**Structured:**

```json
{
  "topic": "recent developments in AI code assistants",
  "time_frame": "past 7 days"
}
```

Claude will run all three skills and return the formatted blurb.

---

### Research Brief (No Formatting)

**Skills:** web-research → content-summarizer

**When to use:** You want to understand a topic but don't need newsletter formatting.

**Natural language:**

> Research [TOPIC] and summarize the key takeaways.

**Structured:**

```json
{
  "topic": "remote work trends in tech",
  "num_sources": 5
}
```

Then ask Claude to summarize without formatting.

---

### Format Existing Research

**Skills:** content-summarizer → newsletter-formatter

**When to use:** You already have notes or sources and just need them formatted.

**Natural language:**

> Here are my notes on [TOPIC]: [paste notes]. Summarize and format as a TLDR-style newsletter blurb.

**Structured:**

```json
{
  "topic": "your topic",
  "sources": [
    {
      "title": "Article title",
      "url": "https://example.com/article",
      "key_points": ["point 1", "point 2"]
    }
  ]
}
```

---

### Just Research

**Skills:** web-research only

**When to use:** You just want sources, you'll handle the rest yourself.

**Natural language:**

> Find 5 recent sources about [TOPIC]. Give me the key points from each.

**Structured:**

```json
{
  "topic": "AI regulation in the EU",
  "num_sources": 5,
  "time_frame": "past 30 days"
}
```

---

### Just Summarize

**Skills:** content-summarizer only

**When to use:** You have content that needs to be distilled.

**Natural language:**

> Here's an article: [paste or link]. What are the key takeaways?

**Structured:**

```json
{
  "topic": "the article topic",
  "sources": [
    {
      "title": "Article title",
      "url": "https://example.com",
      "key_points": ["main point 1", "main point 2", "main point 3"]
    }
  ]
}
```

---

### Just Format

**Skills:** newsletter-formatter only

**When to use:** You have a summary and just need TLDR-style output.

**Natural language:**

> Format this as a TLDR newsletter blurb with a linked headline: [paste summary]

**Structured:**

```json
{
  "topic": "your topic",
  "core_narrative": "The main story in 1-2 sentences.",
  "key_takeaways": ["takeaway 1", "takeaway 2"],
  "implications": "Why this matters.",
  "sources": [{ "title": "Primary source", "url": "https://example.com" }]
}
```

## Create Your Own Combination

You can ask Claude to do any combination by describing what you want in plain language.

**Examples:**

> Research [TOPIC] but don't format it yet - I want to review the sources first.

> Take these three articles and give me a newsletter blurb: [paste links]

> I have research on [TOPIC] from last week. Just format it for the newsletter.

## Tips

- **Start with what you have.** Already have sources? Skip the research step.
- **Ask for checkpoints.** Say "show me the sources before summarizing" if you want control.
- **Iterate.** Ask Claude to adjust the output: "make it shorter" or "focus on the funding angle."
- **Be specific.** "AI news" is vague. "OpenAI's latest model release" gets better results.
- **Use structured data for automation.** If you're building workflows in n8n or other tools, JSON inputs give you consistent results.
