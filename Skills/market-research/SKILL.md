---
name: market-research
description: Identify and monitor competitors in a given industry or niche, and surface new tools/products entering that space. Use this as stage 1 of the AI Market Research Agent pipeline, or standalone when the user asks to "monitor competitors", "find competitors in [industry]", "what tools are new in [niche]", or "who's doing what in [industry]".
metadata:
  author: robort.zo.computer
---

# Market Research

## Overview

Given an industry or niche, this skill identifies (or updates) the set of relevant competitors/players, checks each for recent moves (launches, pricing changes, positioning shifts), and separately scans for new tools/products entering the space. It relies only on `web_search`, `web_research` (category `company`), `x_search`, and `read_webpage`/`view_webpage`. No paid competitive-intelligence or scraping API is used.

## Inputs

- `niche` (required): the industry or niche, e.g. "AI coding assistants for solo founders".
- `topic_file` (optional, default `config/research-topic.md`): read the `## Known Competitors` and `## Focus Areas` sections if present, to seed and scope the search instead of starting cold.
- `run_date` (optional, default today, `YYYY-MM-DD`).

## Steps

1. **Identify competitors.** If `config/research-topic.md` lists known competitors, start from that list. Otherwise (or in addition), run `web_research(category="company")` and `web_search` queries like `"<niche> companies"`, `"<niche> startups"`, `"top <niche> tools"` to identify 5-10 relevant players. Do not fabricate a competitor you have no source for.
2. **Check each competitor for recent moves.** For each identified competitor, use `read_webpage`/`view_webpage` on their homepage and pricing page (if discoverable) plus `web_search`/`x_search` for recent news/announcements about them. Note anything that looks like a launch, pricing change, or repositioning, with a source for each claim.
3. **Discover new tools.** Independent of the competitor list, run `web_search`/`web_research`/`x_search` for queries like `"new <niche> tool"`, `"<niche> launch"`, `"Product Hunt <niche>"` to surface tools/products that may not yet be established competitors.
4. **Write the output file** at `Content/Market-Research/<niche-slug>/<run_date>/market-scan.md` using this structure:

```markdown
# Market Scan: <niche> (<run_date>)

## Competitors Monitored
- **<Competitor>**: <homepage URL>
  - Recent activity: <what changed, or "no notable change found">
  - Source: <URL>

## New Tools / Products Discovered
- **<Tool name>**: <one-line description>, Source: <URL>

## Notes
<Anything ambiguous, unconfirmed, or worth flagging to the next stage.>
```

5. Every factual claim (a competitor exists, a launch happened, a tool is new) must carry a source URL. If no source can be found, say so explicitly rather than omitting the caveat or guessing.

## Output

`Content/Market-Research/<niche-slug>/<run_date>/market-scan.md`: consumed by `business-analyst` in stage 3.
