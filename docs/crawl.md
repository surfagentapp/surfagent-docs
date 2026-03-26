# Crawl & Map

SurfAgent ships two endpoints for site-wide data collection: `/browser/crawl` for full content extraction across pages, and `/browser/map` for fast URL discovery.

Both use your real Chrome browser — which means they work on sites that block headless scrapers.

---

## Overview

| Endpoint | Purpose | Returns |
|----------|---------|---------|
| `POST /browser/crawl` | BFS crawl a domain, extract content from each page | Array of pages with content |
| `POST /browser/map` | Quickly discover all URLs on a domain | Array of URLs only |

---

## `/browser/crawl`

Crawls a domain starting from a seed URL using breadth-first search. Visits each page, extracts content, collects links, and repeats — respecting depth and page count limits.

### Quick Start

```bash
# Crawl a docs site, get markdown from each page
curl -X POST http://localhost:7201/browser/crawl \
  -H "Content-Type: application/json" \
  -d '{
    "url": "https://docs.example.com",
    "maxPages": 10,
    "formats": ["markdown"]
  }'
```

### Request

| Field | Type | Default | Description |
|-------|------|---------|-------------|
| `url` | string | **required** | Starting URL for the crawl |
| `maxPages` | number | `10` | Max pages to visit (hard limit: 50) |
| `maxDepth` | number | `3` | Max link depth from start URL (hard limit: 10) |
| `formats` | string[] | `["markdown","links"]` | What to extract per page. Same options as `/extract`. |
| `includePatterns` | string[] | — | Glob patterns — only crawl URLs matching these |
| `excludePatterns` | string[] | — | Glob patterns — skip URLs matching these |
| `prompt` | string | — | LLM extraction prompt (applied to each page) |
| `schema` | object | — | JSON Schema for structured extraction per page |
| `waitMs` | number | `1000` | Wait between page loads (ms). Be respectful. |
| `concurrency` | number | `1` | Parallel pages (currently capped at 1) |

**Example request:**

```json
{
  "url": "https://docs.surfagent.app",
  "maxPages": 20,
  "maxDepth": 3,
  "formats": ["markdown", "links"],
  "includePatterns": ["https://docs.surfagent.app/docs/*"],
  "excludePatterns": ["*/changelog*", "*/api-reference*"],
  "waitMs": 800
}
```

### Response

```json
{
  "ok": true,
  "data": {
    "pages": [
      {
        "url": "https://docs.surfagent.app",
        "depth": 0,
        "title": "SurfAgent Documentation",
        "markdown": "# SurfAgent Docs\n\nWelcome to...",
        "links": [
          { "text": "Getting Started", "href": "https://docs.surfagent.app/docs/getting-started" }
        ]
      },
      {
        "url": "https://docs.surfagent.app/docs/getting-started",
        "depth": 1,
        "title": "Getting Started",
        "markdown": "# Getting Started\n\n..."
      }
    ],
    "totalPages": 12,
    "crawlDuration": 18400
  }
}
```

---

## `/browser/map`

Fast URL discovery without full content extraction. Crawls the site collecting links but skips heavy content processing — much faster than `/crawl` when you just need to know what pages exist.

### Quick Start

```bash
curl -X POST http://localhost:7201/browser/map \
  -H "Content-Type: application/json" \
  -d '{
    "url": "https://example.com",
    "maxPages": 50
  }'
```

### Request

| Field | Type | Default | Description |
|-------|------|---------|-------------|
| `url` | string | **required** | Starting URL |
| `maxPages` | number | `100` | Max pages to visit |
| `maxDepth` | number | `5` | Max link depth |

### Response

```json
{
  "ok": true,
  "data": {
    "urls": [
      "https://example.com",
      "https://example.com/about",
      "https://example.com/blog",
      "https://example.com/blog/post-1",
      "https://example.com/docs/getting-started"
    ],
    "totalUrls": 47
  }
}
```

---

## Include/Exclude Patterns

Use glob-style patterns to control which URLs get crawled.

**Include only docs pages:**
```json
{
  "includePatterns": ["https://example.com/docs/*"]
}
```

**Exclude login, admin, and asset pages:**
```json
{
  "excludePatterns": [
    "*/login*",
    "*/admin*",
    "*/dashboard*",
    "*.pdf",
    "*.png",
    "*.jpg"
  ]
}
```

**Combined (crawl only blog, skip tag pages):**
```json
{
  "includePatterns": ["https://blog.example.com/*"],
  "excludePatterns": ["*/tag/*", "*/author/*", "*/page/*"]
}
```

---

## Depth and Page Limits

| Setting | Default | Hard Max | Notes |
|---------|---------|----------|-------|
| `maxPages` | 10 | 50 | Total pages visited across the crawl |
| `maxDepth` | 3 | 10 | Link hops from the seed URL |

The crawl stops when **either** limit is hit.

A depth-0 page is the seed URL. Links found there are depth-1, and so on.

---

## Extracting Structured Data During Crawl

Combine with LLM extraction to get structured data from every page:

```bash
curl -X POST http://localhost:7201/browser/crawl \
  -H "Content-Type: application/json" \
  -d '{
    "url": "https://jobs.example.com",
    "maxPages": 30,
    "formats": ["json"],
    "prompt": "Extract the job title, location, department, and salary range if listed",
    "schema": {
      "type": "object",
      "properties": {
        "title": { "type": "string" },
        "location": { "type": "string" },
        "department": { "type": "string" },
        "salary": { "type": "string" }
      }
    }
  }'
```

> Requires LLM configured via `POST /agent/config`. See [extract docs](./extract.md) for setup.

---

## MCP Tool Usage

With SurfAgent's MCP server, both `crawl` and `map` tools are available to Claude Code and Cursor.

**Crawl example:**
```
Crawl https://docs.example.com/api and extract all endpoint names and their descriptions. Use the crawl tool with maxPages 15.
```

**Map example:**
```
Use the map tool to discover all URLs on https://competitor.com — I want to see their site structure.
```

Claude will use the appropriate MCP tool with the right parameters.

---

## Behavior Notes

- **Same-origin only by default.** The crawler won't follow links off the starting domain.
- **Visited URL deduplication.** Each URL is visited at most once per crawl.
- **Tab discipline.** A single tab is reused and navigated between pages. Opened and closed cleanly.
- **Politeness.** Default `waitMs: 1000` adds a 1s gap between page loads. Reduce carefully.
- **Real Chrome advantage.** Sites with Cloudflare, bot detection, or JavaScript gating that block headless tools work fine through SurfAgent's real Chrome.

---

## Error Cases

| Error | Cause |
|-------|-------|
| `URL must be http/https` | File:// or other protocols rejected |
| `maxPages exceeds limit` | Requested over 50 pages |
| `LLM not configured` | Used `json` format without `/agent/config` |
| `No pages crawled` | Start URL unreachable or all links excluded by patterns |
