---
name: content-summarizer
description: Take structured source data and synthesize it into a concise, insightful summary. Use when you have research results that need to be distilled into clear takeaways.
---

# Content Summarizer Skill

Transform raw research into a clear, synthesized summary that captures what matters.

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

## Input Format

Expects output from web-research skill or equivalent:

```json
{
  "topic": "string",
  "sources": [
    {
      "title": "string",
      "url": "string",
      "key_points": ["string"]
    }
  ]
}
```

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
  "confidence": "high | medium | low"
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
  "sources": [
    {
      "title": "Cursor raises $100M",
      "key_points": [
        "Series B at $1B valuation",
        "Expanding enterprise features"
      ]
    },
    {
      "title": "GitHub Copilot adds voice coding",
      "key_points": ["New accessibility features", "Voice-to-code in beta"]
    },
    {
      "title": "Stack Overflow survey: 78% of devs now use AI assistants",
      "key_points": [
        "Up from 52% last year",
        "Productivity gains reported at 30-50%"
      ]
    }
  ]
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
  "confidence": "high"
}
```
