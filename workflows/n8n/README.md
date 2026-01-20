# n8n Newsletter Research Workflow

An automated workflow that chains the newsletter research skills together, triggered by a webhook.

## What It Does

1. **Webhook** - Receives a topic via POST request
2. **Web Search** - Searches for recent sources using Tavily API
3. **Build Research Request** - Transforms search results into a properly formatted Claude API request (necessary because n8n's JSON mode can't handle dynamic data with nested quotes)
4. **Research** - Structures search results into clean JSON
5. **Build Synthesize Request** - Transforms research output into the next Claude API request
6. **Synthesize** - Distills sources into key takeaways
7. **Build Format Request** - Transforms summary into the final Claude API request
8. **Format** - Outputs a TLDR-style newsletter blurb

The Code nodes (Build [X] Request) handle data transformation between steps. They're necessary because embedding JSON output from one API call into the next request body requires proper escaping that n8n's expression syntax doesn't handle well.

## Prerequisites

- [n8n](https://n8n.io/) (self-hosted or cloud)
- [Tavily API key](https://tavily.com/) (free tier available)
- [Anthropic API key](https://console.anthropic.com/)

## Setup

1. **Import the workflow**
   - Open n8n
   - Go to Workflows → Create Workflow → ... (top right) → Import from file
   - Select `newsletter-automation.json`

2. **Configure Tavily API key**
   - Open the "Web Search" node
   - Replace `[YOUR-TAVILY-API-KEY-HERE]` with your actual Tavily API key

3. **Configure Anthropic credentials**
   - Go to Credentials → Add Credential → Header Auth
   - Name: `ANTHROPIC API KEY`
   - Header Name: `x-api-key`
   - Header Value: Your Anthropic API key
   - Link this credential to the Research, Synthesize, and Format nodes

4. **Activate the workflow**
   - Toggle the workflow to Active

## Usage

Send a POST request to the webhook URL:

```bash
curl -X POST https://your-n8n-instance/webhook/[webhook-path] \
  -H "Content-Type: application/json" \
  -d '{"topic": "AI code assistants"}'
```

The workflow returns a JSON response with:

- `headline` - Linked headline for the newsletter
- `blurb` - TLDR-style summary (80-150 words)
- `word_count` - Word count of the blurb
- `category_suggestion` - Suggested category

## Production Considerations

### API Key Security

The Tavily API key is stored directly in the workflow JSON because n8n environment variables (`$env`) are not accessible during manual UI testing—only in production webhook executions. For production deployments:

- **n8n Cloud**: Use n8n's built-in credential system to store the Tavily key securely
- **Self-hosted**: Use environment variables with `$env.TAVILY_API_KEY` (requires `N8N_BLOCK_ENV_ACCESS_IN_NODE=false`)
- **Docker**: Pass secrets via Docker secrets or environment variables in your compose file

### Local vs Production

This workflow was developed and tested on a local n8n instance. For production use:

- Deploy n8n to a server or use n8n Cloud
- Set up a persistent database (default SQLite is not recommended for production)
- Configure proper webhook URLs with HTTPS
- Set up error handling and notifications for failed executions
- Consider rate limits on both Tavily and Anthropic APIs

## Customization

- **Change search depth**: In the Web Search node, change `search_depth` from `basic` to `advanced` for more thorough results
- **Adjust source count**: Change `max_results` in the Web Search node. This affects how many sources Claude has to synthesize from (default: 5). The final blurb links to one primary source but draws insights from all sources.
- **Modify output style**: Edit the system prompts in the Code nodes
- **Get fresh sources**: Tavily returns the same top results for identical queries. To get different sources, try:
  - Adding a date filter: `"days": 7` in the Web Search body to limit to past week
  - Using `"exclude_domains": ["example.com"]` to skip previously used sources
  - Varying the query wording

## Workflow Diagram

```
Webhook → Web Search → Build Research Request → Research → Build Synthesize Request → Synthesize → Build Format Request → Format
```
