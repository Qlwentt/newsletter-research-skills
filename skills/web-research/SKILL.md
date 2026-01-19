---
name: web-research
description: Search the web for recent information on a topic and return structured source data. Use when gathering sources for research, content creation, or competitive analysis.
---

# Web Research Skill

Research a topic by finding recent, relevant sources and extracting key information.

Note: **Important:** Always perform fresh web research. Never use example data from this file.

## When to Use

- User needs to gather sources on a topic
- Research for newsletter content, blog posts, or reports
- Competitive analysis or market research

## Instructions

When asked to research a topic:

1. Search for 4-6 recent, high-quality sources
2. Prioritize:
   - Reputable publications (major news outlets, industry blogs, official announcements)
   - Recency (prefer sources from the last 7 days when possible)
   - Relevance (directly addresses the topic, not tangential)
3. For each source, extract:
   - Title
   - URL
   - Publication date (if available)
   - 2-3 key points or takeaways
4. Return results in the structured format below

## Input Format

This skill accepts either:

**Structured request:**

```json
{
  "topic": "the topic to research",
  "time_frame": "optional - e.g., 'past 7 days'",
  "num_sources": "optional - defaults to 4-6"
}
```

**Or natural language:**

- "Research recent news about AI code assistants"
- "Find sources about the OpenAI drama from last week"
- "What's been happening with Tesla stock?"

Claude will interpret the request and proceed with research.

## Output Format

```json
{
  "topic": "the research topic as understood",
  "research_date": "YYYY-MM-DD",
  "sources": [
    {
      "title": "Article or page title",
      "url": "https://...",
      "publication_date": "YYYY-MM-DD or null if unknown",
      "key_points": ["First key takeaway", "Second key takeaway"]
    }
  ],
  "summary": "2-3 sentence synthesis of what the sources collectively say"
}
```

## Edge Cases

- **Not enough sources found**: If fewer than 3 quality sources exist, return what you found and set `"needs_review": true` with an explanation
- **Conflicting information**: Note conflicts in the summary field and include both perspectives
- **Ambiguous topic**: Ask for clarification before researching

## Example

**Input**: "Research recent developments in AI code assistants"

**Output**:

```json
{
  "topic": "recent developments in AI code assistants",
  "research_date": "2025-01-19",
  "sources": [
    {
      "title": "Cursor raises $100M to expand AI coding assistant",
      "url": "https://example.com/cursor-funding",
      "publication_date": "2025-01-15",
      "key_points": [
        "Cursor secured Series B funding at $1B valuation",
        "Plans to expand enterprise features and team collaboration"
      ]
    }
  ],
  "summary": "The AI code assistant market continues rapid growth, with major players securing significant funding and expanding enterprise capabilities."
}
```
