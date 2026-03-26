# Extract

The `/browser/extract` endpoint uses your real Chrome browser to pull structured data from any page — optionally powered by an LLM for intelligent schema-based extraction.

Think of it as Firecrawl's `/scrape`, but running through your actual browser with your real cookies, sessions, and fingerprint.

---

## What It Does

- Navigates to a URL (or uses an existing tab)
- Extracts page content in multiple formats: markdown, JSON, HTML, links, screenshot
- If a `prompt` or `schema` is provided and an LLM is configured, returns structured JSON
- Without an LLM, returns clean markdown + links by default
- Closes the tab automatically after extraction (unless `keepTab: true`)

---

## Quick Start

```bash
# Basic extraction — returns markdown + links
curl -X POST http://localhost:7201/browser/extract \
  -H "Content-Type: application/json" \
  -d '{"url": "https://example.com"}'
```

```bash
# Structured extraction with LLM
curl -X POST http://localhost:7201/browser/extract \
  -H "Content-Type: application/json" \
  -d '{
    "url": "https://shop.example.com/products",
    "prompt": "Extract all product names and prices",
    "formats": ["json", "markdown"]
  }'
```

---

## API Reference

### `POST /browser/extract`

**Request body:**

| Field | Type | Default | Description |
|-------|------|---------|-------------|
| `url` | string | — | URL to navigate to. Opens a new tab. Required if no `tabId`. |
| `tabId` | string | — | Use an existing open tab instead of navigating. |
| `prompt` | string | — | Natural language instruction for LLM extraction. |
| `schema` | object | — | JSON Schema for structured output. Used with LLM extraction. |
| `formats` | string[] | `["markdown","links"]` | What to return. Options: `markdown`, `json`, `html`, `links`, `screenshot` |
| `waitForSelector` | string | — | CSS selector to wait for before extracting (max 30s). |
| `waitMs` | number | `0` | Additional wait in milliseconds after page load. |
| `keepTab` | boolean | `false` | Don't close the tab after extraction. |

**Example request:**

```json
{
  "url": "https://news.ycombinator.com",
  "prompt": "Extract the top 5 stories with their titles, points, and comment counts",
  "schema": {
    "type": "object",
    "properties": {
      "stories": {
        "type": "array",
        "items": {
          "type": "object",
          "properties": {
            "title": { "type": "string" },
            "points": { "type": "number" },
            "comments": { "type": "number" }
          }
        }
      }
    }
  },
  "formats": ["json", "markdown"]
}
```

**Example response:**

```json
{
  "ok": true,
  "data": {
    "url": "https://news.ycombinator.com",
    "title": "Hacker News",
    "markdown": "# Hacker News\n\n1. [Some Story](https://...) — 342 points...",
    "json": {
      "stories": [
        { "title": "Some Interesting Story", "points": 342, "comments": 87 },
        { "title": "Another Story", "points": 198, "comments": 45 }
      ]
    },
    "links": [
      { "text": "Some Interesting Story", "href": "https://..." }
    ],
    "metadata": {
      "statusCode": 200,
      "language": "en",
      "description": "Hacker News"
    }
  }
}
```

---

## Formats

### `markdown`
Clean markdown representation of the page content. Scripts, styles, nav, and footer elements are stripped. Great for feeding into an LLM context window.

### `json`
Structured JSON output. **Requires `prompt` or `schema` and a configured LLM.** The page text is sent to your LLM with the extraction instructions, and the result is returned as parsed JSON.

### `html`
Cleaned HTML of the page body (sanitized, no scripts/styles).

### `links`
All links found on the page as an array of `{ text, href }` objects.

### `screenshot`
Base64-encoded PNG screenshot of the page. `data:image/png;base64,...`

---

## Using with LLM (Structured Extraction)

For schema-based JSON extraction, configure your LLM first:

```bash
curl -X POST http://localhost:7201/agent/config \
  -H "Content-Type: application/json" \
  -d '{
    "provider": "openai",
    "apiKey": "sk-...",
    "model": "gpt-4o-mini"
  }'
```

Then use `formats: ["json"]` with a `prompt` or `schema`:

```bash
curl -X POST http://localhost:7201/browser/extract \
  -H "Content-Type: application/json" \
  -d '{
    "url": "https://example.com/pricing",
    "prompt": "Extract all pricing tiers with their names, prices, and included features",
    "formats": ["json"]
  }'
```

---

## Using without LLM (Basic Extraction)

No LLM needed for basic page content. Just omit `prompt`/`schema` and use `markdown`, `html`, `links`, or `screenshot` formats:

```bash
curl -X POST http://localhost:7201/browser/extract \
  -H "Content-Type: application/json" \
  -d '{
    "url": "https://example.com",
    "formats": ["markdown", "links", "screenshot"]
  }'
```

---

## Waiting for Dynamic Content

For JavaScript-rendered pages, use `waitForSelector` or `waitMs`:

```bash
curl -X POST http://localhost:7201/browser/extract \
  -H "Content-Type: application/json" \
  -d '{
    "url": "https://spa-app.example.com/dashboard",
    "waitForSelector": "[data-loaded='true']",
    "waitMs": 500,
    "formats": ["markdown"]
  }'
```

---

## MCP Tool Usage

If you're using SurfAgent's MCP server with Claude Code or Cursor, the `extract` tool is available directly:

```
Extract the product listings from https://shop.example.com using the extract tool with a schema for name and price fields.
```

Claude will call the `extract` MCP tool automatically with appropriate parameters.

---

## Limits

| Parameter | Max |
|-----------|-----|
| Prompt length | 4096 chars |
| Schema size | 10 KB |
| Page content sent to LLM | 8000 chars |
| LLM call timeout | 30s |

---

## Error Cases

| Error | Cause | Fix |
|-------|-------|-----|
| `LLM not configured` | `json` format used without configuring LLM | Call `/agent/config` first |
| `Tab not found` | Invalid `tabId` | Check `/browser/tabs` for active tabs |
| `Navigation timeout` | Page took too long to load | Add `waitMs` or `waitForSelector` |
| `Schema validation failed` | LLM returned non-conforming JSON | Simplify schema or add more specific prompt |
