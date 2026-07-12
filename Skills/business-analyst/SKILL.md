---
name: business-analyst
description: Synthesize a market scan and trend/news summary into one prioritized market research report with concrete recommended opportunities. Use this as stage 3 (final) of the AI Market Research Agent pipeline, after market-research and trend-monitor have produced their output files, or standalone when the user asks to "turn this research into recommendations" or "what should we do given this market data".
metadata:
  author: robort.zo.computer
---

# Business Analyst

## Overview

Takes the `market-scan.md` (competitors + new tools) and `trends-news.md` (trends + news) produced for the same niche and run date, and synthesizes them into one report: a plain-language recap, and a short, prioritized list of recommended opportunities, each one traceable back to a specific finding in the two input files. See `references/output-template.md` for the exact report structure.

## Inputs

- `niche` (required): the industry or niche this run covers.
- `market_scan_file` (required): path to this run's `market-scan.md` from `market-research`.
- `trends_news_file` (required): path to this run's `trends-news.md` from `trend-monitor`.
- `run_date` (optional, default today, `YYYY-MM-DD`).

## Steps

1. Read both input files in full. If either is missing, stop and report which stage's output is missing rather than guessing its contents.
2. Write a short **recap**: 3-5 sentences on the overall state of the niche this run (what competitors are doing, what's trending, what's new).
3. Draft **recommended opportunities**: 3-6 concrete, actionable opportunities (e.g. a positioning angle, an underserved segment, a feature gap, a content/marketing angle). Every opportunity must cite the specific competitor move, trend, or news item it's derived from. Do not invent an opportunity that isn't traceable to something in the two input files.
4. Rank the opportunities by a simple heuristic: how many independent signals (competitor moves + trends + news items) support it. More corroborating signals = higher priority. State the ranking rationale briefly.
5. Fill in `references/output-template.md` and write the result to `Content/Market-Research/<niche-slug>/<run_date>.md`.
6. Never fabricate a recommendation, a competitor move, or a trend that isn't backed by something in `market_scan_file` or `trends_news_file`. If the inputs are thin, say the evidence is thin rather than padding the report.

## Output

`Content/Market-Research/<niche-slug>/<run_date>.md`: the final report the user reads.
