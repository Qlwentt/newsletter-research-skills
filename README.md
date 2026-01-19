# Newsletter Research Skills

A composable Claude Skills library for newsletter content research and production.

## What This Is

Three modular skills that chain together to turn a topic into a publish-ready newsletter blurb:

1. **web-research** → Finds recent sources on a topic
2. **content-summarizer** → Synthesizes sources into key takeaways
3. **newsletter-formatter** → Outputs a TLDR-style blurb with linked headline

Use them together or mix and match based on what you need.

## Quick Start

### If you just want a newsletter blurb:

Tell Claude:

> Research recent developments in AI code assistants and write a TLDR-style newsletter blurb with a linked headline.

Claude will research, synthesize, and format automatically.

### If you want more control:

> Research AI code assistants but don't format yet - show me the sources first.

Review the sources, then:

> Now summarize and format as a newsletter blurb.

## Skills

| Skill                                                        | What it does                        | Input                  | Output                 |
| ------------------------------------------------------------ | ----------------------------------- | ---------------------- | ---------------------- |
| [web-research](skills/web-research/SKILL.md)                 | Finds recent sources on a topic     | Topic (text or JSON)   | Structured source data |
| [content-summarizer](skills/content-summarizer/SKILL.md)     | Distills sources into key takeaways | Sources (text or JSON) | Summary with insights  |
| [newsletter-formatter](skills/newsletter-formatter/SKILL.md) | Formats for TLDR-style newsletter   | Summary (text or JSON) | Ready-to-publish blurb |

Each skill accepts both natural language and structured JSON input. See individual skill files for details.

## Documentation

- [Newsletter Blurb Workflow](workflows/newsletter-blurb.md) - Step-by-step guide for the full workflow
- [Mixing and Matching Skills](workflows/mixing-and-matching-skills.md) - How to combine skills for different use cases
- [Creating New Skills](docs/CREATING-NEW-SKILLS.md) - Template and guidelines for adding skills

## Example Output

**Input:** "Research recent developments in AI code assistants"

**Output:**

[78% of Developers Now Use AI Coding Assistants](https://stackoverflow.com/survey/2025)

Up from 52% just a year ago, according to Stack Overflow's latest survey. The surge in adoption is driving massive valuations—Cursor just raised at $1B—and shifting product focus from individual productivity to enterprise team features. AI-assisted coding is quickly becoming table stakes, not a competitive advantage.

## Design Principles

**Composable**: Each skill does one thing well. Chain them together for complex workflows.

**Flexible input**: Works with natural language ("research X for me") or structured JSON for automation.

**Production-ready**: Skills handle edge cases, flag issues for human review, and output consistent structured data.

**Non-technical friendly**: End users can create workflows by describing what they want in plain English.

## Project Structure

```
newsletter-research-skills/
├── README.md
├── skills/
│   ├── web-research/
│   │   └── SKILL.md
│   ├── content-summarizer/
│   │   └── SKILL.md
│   └── newsletter-formatter/
│       └── SKILL.md
├── workflows/
│   ├── NEWSLETTER-BLURB.md
│   ├── MIXING-AND-MATCHING-SKILLS.md
│   └── n8n/
│       └── README.md
├── app/
│   └── README.md
└── docs/
    └── CREATING-NEW-SKILLS.md

## TLDR-specific Considerations

This library is one way to implement composable skill architecture for content workflows. An implementation that matched TLDRs specific format would likely differ in a few ways:

**Single-source summarization:** Each TLDR blurb links to one article, so all facts should trace to that source. The current multi-source approach is better suited for research briefs. This version would summarize individual articles rather than synthesizing across sources.

**Curated selection workflow:** Rather than outputting one blurb, a different workflow might:
1. Research and surface the top 10 most impactful stories on a topic
2. Generate a blurb for each story individually
3. Let editors pick which ones make it into the newsletter

This gives editors control while automating the research and drafting work.

**Tighter recency filters:** TLDR is a daily newsletter covering yesterday's news. TLDR-specific skills would filter more aggressively for recency (past 24-48 hours).
```
