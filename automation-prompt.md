# AI Market Research Agent: Automation Prompt

## Objective

Deliver ongoing market intelligence for a given industry or niche: monitor competitors, find trends, discover new tools, summarize industry news, and recommend opportunities, by running the three project-local skills in sequence: `market-research` -> `trend-monitor` -> `business-analyst`.

## Inputs

- `niche` (required): the industry or niche to research, e.g. "AI coding assistants for solo founders". If not given, read `config/research-topic.md`'s `## Industry / Niche` section.
- `topic_file` (optional, default `config/research-topic.md`): known competitors and focus areas to seed/scope the research.
- `run_date` (optional, default today, `YYYY-MM-DD`).
- `notify` (optional, default `none`): `none` | `email`. If `email`, email the finished report to the user via `send_email_to_user` after saving it. Any other delivery channel (Slack, SMS, etc.) requires the user to explicitly request it in the same run.

## Steps

1. Run the `market-research` skill (`Skills/market-research/SKILL.md`) for `niche`. It identifies/monitors competitors and discovers new tools using only `web_search`, `web_research`, `x_search`, and `read_webpage`/`view_webpage`. Never a paid competitive-intelligence API. Output: `Content/Market-Research/<niche-slug>/<run_date>/market-scan.md`.
2. Run the `trend-monitor` skill (`Skills/trend-monitor/SKILL.md`) for `niche`. It finds emerging trends and summarizes recent industry news using only `web_search`, `web_research`, and `x_search`. Never a paid trend-tracking API. Output: `Content/Market-Research/<niche-slug>/<run_date>/trends-news.md`.
3. Run the `business-analyst` skill (`Skills/business-analyst/SKILL.md`) on this run's `market-scan.md` and `trends-news.md`. It synthesizes both into one report with a recap and a prioritized list of recommended opportunities, each traceable to a specific finding. Output: `Content/Market-Research/<niche-slug>/<run_date>.md`.
4. Do not fabricate a competitor, trend, news item, or recommended opportunity at any stage. Only report what was actually observed or retrieved from a real source, with a source URL for every factual claim.
5. If `notify` is `email`, email the contents (or a clear summary + file path) of the report to the user via `send_email_to_user`. If `notify` is `none`, just report the file path. Do not send anything.

## Outputs

- `Content/Market-Research/<niche-slug>/<run_date>/market-scan.md`
- `Content/Market-Research/<niche-slug>/<run_date>/trends-news.md`
- `Content/Market-Research/<niche-slug>/<run_date>.md`: the report the user reads

Plus a short chat/report summary: niche researched, number of competitors monitored, number of trends identified, top recommended opportunity, and the report's file path.

## Error handling

- If no `niche` is given and `config/research-topic.md` has no real entry (only the placeholder example), stop and ask the user for a niche rather than guessing one.
- If a competitor or news source page fails to load, skip that source (not the whole run) and note it rather than fabricating content for it.
- If `business-analyst` can't find `market-scan.md` or `trends-news.md` for the run date, stop and report which stage's output is missing rather than guessing its contents.
- Never call a paid or external API at any stage, even if a source is hard to parse without one. Report the limitation instead.
