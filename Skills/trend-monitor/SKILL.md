---
name: trend-monitor
description: Find emerging trends and summarize recent industry news for a given industry or niche. Use this as stage 2 of the AI Market Research Agent pipeline, or standalone when the user asks to "find trends in [industry]", "what's happening in [niche]", "summarize industry news for [industry]", or "what's trending in [niche] right now".
metadata:
  author: robort.zo.computer
---

# Trend Monitor

## Overview

Given an industry or niche, this skill scans recent news and social discourse to surface emerging trends and produces a plain-language news summary. It relies only on `web_search` (with `topic="news"`), `web_research`, and `x_search`. No paid trend-tracking or media-monitoring API is used.

## Inputs

- `niche` (required): the industry or niche, e.g. "AI coding assistants for solo founders".
- `topic_file` (optional, default `config/research-topic.md`): read the `## Focus Areas` section if present, to narrow which sub-topics to prioritize.
- `run_date` (optional, default today, `YYYY-MM-DD`).
- `time_range` (optional, default `week`): how far back to look for news/trend signals.

## Steps

1. **Scan recent news.** Run `web_search(topic="news", time_range=time_range)` with 2-3 differently-worded queries about `<niche>` to get broad coverage. Follow up with `web_research` for anything that looks significant but thin on detail.
2. **Scan social discourse.** Run `x_search` for `<niche>` and, if focus areas are given, for each focus area, to capture trends/sentiment that haven't hit mainstream news coverage yet.
3. **Identify trends.** Group what you found into 3-6 named trends (e.g. "shift toward usage-based pricing", "rising interest in on-device inference"). A trend must be backed by at least two independent sources found in steps 1-2. A single article is news, not yet a trend.
4. **Write the output file** at `Content/Market-Research/<niche-slug>/<run_date>/trends-news.md` using this structure:

```markdown
# Trends & News: <niche> (<run_date>)

## Emerging Trends
### <Trend name>
<1-2 sentence description>
- Source: <URL>
- Source: <URL>

## News Summary
- <Plain-language recap of a notable news item>, Source: <URL>

## Notes
<Anything ambiguous, single-sourced, or worth flagging to the next stage.>
```

5. Never label something a "trend" without at least two sources. Single-sourced items go under News Summary or Notes instead.

## Output

`Content/Market-Research/<niche-slug>/<run_date>/trends-news.md`: consumed by `business-analyst` in stage 3.
