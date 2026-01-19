---
name: newsletter-formatter
description: Transform a content summary into a publication-ready newsletter blurb in TLDR style. Use when you have a synthesized summary that needs to be formatted for newsletter distribution.
---

# Newsletter Formatter Skill

Turn synthesized content into a punchy, scannable newsletter blurb that respects readers' time.

## When to Use

- You have output from content-summarizer skill (or equivalent summary data)
- Content needs to be formatted for email newsletter distribution
- Target audience is busy professionals who skim

## Instructions

When given summarized content:

1. Write in TLDR style
   - Lead with the news, not the background
   - Front-load the most important information
   - Use plain language (no jargon unless necessary)
   - Be direct and confident in tone

2. Structure for scannability
   - Entire headline is a clickable link to the primary source
   - Keep paragraphs to 2-3 sentences max
   - Bold key phrases sparingly for emphasis

3. Respect the reader's time
   - Total length: 80-150 words
   - Every sentence must earn its place
   - Cut throat on filler phrases

4. Headline is the hook
   - The entire headline links to the primary source
   - Make it specific and newsworthy
   - Numbers and specifics pop (e.g., "78% of Developers" not "Most Developers")

## Input Format

Expects output from content-summarizer skill or equivalent:

```json
{
  "topic": "string",
  "core_narrative": "string",
  "key_takeaways": ["string"],
  "implications": "string"
}
```

Also expects a `sources` array for linking:

```json
{
  "sources": [{ "title": "string", "url": "string" }]
}
```

## Output Format

```json
{
  "headline": "[Entire Headline Is the Link](https://primary-source-url)",
  "blurb": "The formatted newsletter blurb body. No link needed here since headline carries it.",
  "word_count": 112,
  "category_suggestion": "AI | Dev Tools | Funding | Product Launch | Research"
}
```

Note: The entire headline is hyperlinked to the primary source. This matches TLDR's actual format where readers click the headline to go straight to the source article.

## Style Guidelines

**Do:**

- Start with what happened, not why it matters
- Use numbers when available (they pop visually)
- Write like you're telling a smart friend
- Link the entire headline to the most important source

**Don't:**

- Start with "In a move that..." or "In an effort to..."
- Use phrases like "It's worth noting" or "Interestingly"
- Hedge excessively ("might potentially perhaps")
- Add additional links in the blurb body (headline link is sufficient)

## Edge Cases

- **Summary is thin**: Write shorter blurb (60-80 words) rather than padding
- **Multiple angles possible**: Pick the most newsworthy one, note alternatives in output
- **No good source link**: Flag `"needs_source_link": true` in output
- **Multiple equally important sources**: Link headline to primary source, mention secondary source by name in blurb without linking

## Example

**Input**:

```json
{
  "topic": "recent developments in AI code assistants",
  "core_narrative": "AI code assistants have hit mainstream adoption, with nearly 8 in 10 developers now using them daily.",
  "key_takeaways": [
    "Developer adoption jumped from 52% to 78% in one year",
    "Cursor raised at $1B valuation",
    "Focus shifting to enterprise collaboration"
  ],
  "implications": "AI-assisted coding is becoming table stakes for developer productivity.",
  "sources": [
    {
      "title": "Stack Overflow Developer Survey 2025",
      "url": "https://stackoverflow.com/survey/2025"
    }
  ]
}
```

**Output**:

```json
{
  "headline": "[78% of Developers Now Use AI Coding Assistants](https://stackoverflow.com/survey/2025)",
  "blurb": "Up from 52% just a year ago, according to Stack Overflow's latest survey. The surge in adoption is driving massive valuations—Cursor just raised at $1B—and shifting product focus from individual productivity to enterprise team features. AI-assisted coding is quickly becoming table stakes, not a competitive advantage.",
  "word_count": 52,
  "category_suggestion": "AI"
}
```
