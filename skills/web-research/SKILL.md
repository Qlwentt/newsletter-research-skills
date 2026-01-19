---
name: web-research
description: Search the web for recent information on a topic and return structured source data. Use when gathering sources for research, content creation, or competitive analysis.
---

# Web Research Skill

Research a topic by finding recent, relevant sources and extracting key information.

**Critical:** Always perform fresh web research. The examples in this file are for illustration only - never use them as actual data, and do not let them influence your search queries. Base your search solely on the user's request. Do not add years or dates to search queries unless the user specifies a time period.

**Output behavior:**

- If the user asks for a complete workflow in one request (e.g., "research X and write a newsletter blurb"), work silently and pass data to the next step without showing intermediate output.
- If the user asks for just this step (e.g., "research X" or "summarize these sources"), show the output.

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
3. Search tips:
   - Do NOT include years in your search query unless the user specifically asks for a time period
   - Let the search engine determine recency naturally
   - Use the topic keywords only
4. For each source, extract:
   - Title
   - URL
   - Publication date (if available)
   - 2-3 key points or takeaways
5. Return results in the structured format below

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

**Note:** This is a fictional example for illustration only.

**Input**: "Research recent news about the fictional Acme Corporation"

**Output**:

```json
{
  "topic": "recent news about the fictional Acme Corporation",
  "research_date": "YYYY-MM-DD",
  "sources": [
    {
      "title": "Acme Corp Announces New Widget",
      "url": "https://example.com/acme-widget",
      "publication_date": "YYYY-MM-DD",
      "key_points": ["Example key point one", "Example key point two"]
    }
  ],
  "summary": "Example summary of what sources collectively say."
}
```
