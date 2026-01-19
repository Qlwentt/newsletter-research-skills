---
name: content-summarizer
description: Take structured source data and synthesize it into a concise, insightful summary. Use when you have research results that need to be distilled into clear takeaways.
---

# Content Summarizer Skill

Transform raw research into a clear, synthesized summary that captures what matters.

**Critical:** Always synthesize from the actual input provided. The examples in this file are for illustration only - never use them as actual data.

**Output behavior:** When this skill is part of a chain (e.g., leading to newsletter formatting), work silently - do not output the JSON to the user. Only output to the user when this skill is used standalone, or when the user explicitly asks to see the summary.

**Output behavior:**

- If the user asks for a complete workflow in one request (e.g., "research X and write a newsletter blurb"), work silently and pass data to the next step without showing intermediate output.
- If the user asks for just this step (e.g., "research X" or "summarize these sources"), show the output.

## When to Use

- You have output from the web-research skill (or similar structured source data)
- Multiple sources need to be distilled into a single narrative
- User needs to understand "what's the story here"

## Instructions

When given structured source data:

1. Identify the core narrative across sources
   - What's the main thing happening?
   - Why does it matter?
   - What are the implications?

2. Synthesize, don't just concatenate
   - Find the thread that connects the sources
   - Highlight agreements and tensions between sources
   - Draw out insights that aren't obvious from any single source

3. Keep it tight
   - Lead with the most important point
   - Cut anything that doesn't serve the core narrative
   - Aim for clarity over comprehensiveness

4. Return results in the structured format below

```json
{
  "topic": "string",
  "research_date": "YYYY-MM-DD",
  "sources": [
    {
      "title": "string",
      "url": "string",
      "publication_date": "YYYY-MM-DD or null",
      "key_points": ["string"]
    }
  ],
  "summary": "string"
}
```

**Or natural language:**

- Pasted article text or notes
- URLs to articles (Claude will read them)
- Bullet points or rough notes on a topic
- "Here's what I found about [topic]: ..."

Claude will normalize the input into structured data before processing.

## Output Format

```json
{
  "topic": "the topic summarized",
  "core_narrative": "1-2 sentences capturing the main story",
  "key_takeaways": [
    "First major takeaway",
    "Second major takeaway",
    "Third major takeaway (if warranted)"
  ],
  "implications": "1-2 sentences on why this matters or what might happen next",
  "source_count": 4,
  "confidence": "high | medium | low",
  "sources": [{ "title": "string", "url": "string" }]
}
```

## Confidence Levels

- **high**: 4+ sources agreeing, reputable publications, recent data
- **medium**: 2-3 sources, some disagreement, or older data
- **low**: Limited sources, significant conflicts, or uncertain reliability

## Edge Cases

- **Sources conflict significantly**: Set confidence to "low" and note the conflict in core_narrative
- **Sources are thin**: Set confidence appropriately and note limitations
- **Off-topic sources in input**: Ignore them and note that some sources were excluded

## Example

**Input**:

```json
{
  "topic": "recent developments in AI code assistants",
  "research_date": "2025-01-19",
  "sources": [
    {
      "title": "Cursor raises $100M",
      "url": "https://example.com/cursor-funding",
      "publication_date": "2025-01-15",
      "key_points": [
        "Series B at $1B valuation",
        "Expanding enterprise features"
      ]
    },
    {
      "title": "GitHub Copilot adds voice coding",
      "url": "https://example.com/copilot-voice",
      "publication_date": "2025-01-17",
      "key_points": ["New accessibility features", "Voice-to-code in beta"]
    },
    {
      "title": "Stack Overflow survey: 78% of devs now use AI assistants",
      "url": "https://stackoverflow.com/survey/2025",
      "publication_date": "2025-01-10",
      "key_points": [
        "Up from 52% last year",
        "Productivity gains reported at 30-50%"
      ]
    }
  ],
  "summary": "The AI code assistant market continues rapid growth, with major players securing significant funding and expanding enterprise capabilities."
}
```

**Output**:

```json
{
  "topic": "recent developments in AI code assistants",
  "core_narrative": "AI code assistants have hit mainstream adoption, with nearly 8 in 10 developers now using them daily. Major players are racing to expand capabilities and capture enterprise market share.",
  "key_takeaways": [
    "Developer adoption has jumped from 52% to 78% in one year",
    "Significant funding continues to flow into the space (Cursor's $1B valuation)",
    "Focus is shifting from individual productivity to enterprise collaboration and accessibility"
  ],
  "implications": "AI-assisted coding is becoming table stakes for developer productivity. Companies slow to adopt may face competitive disadvantages in hiring and shipping speed.",
  "source_count": 3,
  "confidence": "high",
  "sources": [
    {
      "title": "Stack Overflow Developer Survey 2025",
      "url": "https://stackoverflow.com/survey/2025"
    },
    {
      "title": "Cursor raises $100M",
      "url": "https://example.com/cursor-funding"
    },
    {
      "title": "GitHub Copilot adds voice coding",
      "url": "https://example.com/copilot-voice"
    }
  ]
}
```
