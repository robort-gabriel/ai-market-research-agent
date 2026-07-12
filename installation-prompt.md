# AI Market Research Agent — Installation Prompt

Paste everything below the line into a **new Zo chat** and send it. It's self-contained — the AI reading it has no memory of how this project was built, so it spells out every step. Swap the repo URL first if you're installing from a fork.

---

Fetch and install the "AI Market Research Agent" Zo automation from this public GitHub repo:

`https://github.com/robort-gabriel/ai-market-research-agent`

Do the following, in order. Steps marked **(confirm)** must not proceed until I explicitly approve — don't treat silence or a prior approval as blanket permission.

### 1. Fetch the repo

- Target folder: `/home/workspace/Zo-Automations/ai-market-research-agent/`.
- **(confirm)** If that folder already exists, tell me what's in it and ask whether to overwrite before touching anything.
- Clone or download the repo (it's public, no auth needed) into that folder, preserving its structure exactly:
  ```
  README.md
  installation-prompt.md
  persona.md
  automation-prompt.md
  starter-prompts.md
  config/research-topic.md
  Skills/market-research/SKILL.md
  Skills/trend-monitor/SKILL.md
  Skills/business-analyst/SKILL.md
  Skills/business-analyst/references/output-template.md
  ```

### 2. Verify the skills

- Confirm each of `Skills/market-research/SKILL.md`, `Skills/trend-monitor/SKILL.md`, `Skills/business-analyst/SKILL.md` exists and has valid frontmatter (`name` matching its folder, non-empty `description`).
- These skills are project-local by design. Do not copy them into the global `Skills/` folder or any other project — this automation only ever reads/writes inside `/home/workspace/Zo-Automations/ai-market-research-agent/` and its output folder `/home/workspace/Content/Market-Research/`.

### 3. Set up the research topic

- **(confirm)** Ask me for the industry or niche I want researched, plus (optional) any known competitors and focus areas.
- Write them into `config/research-topic.md`, replacing the placeholder example, using the format already documented in that file.

### 4. Create a dedicated persona

- **(confirm)** the name and scope with me before creating anything.
- Read `persona.md` in the repo and use its content verbatim: the `Name` field as the persona name, the `Prompt` field as the persona's system prompt. Do not paraphrase or shorten it.
- Create the persona (via the persona-creation tool) with that exact name and prompt text.
- After creating it, ask me whether to switch to it now (set it active) or leave it available to select later.

### 5. Offer to set up recurring automation

- **(confirm)** Ask me for: run frequency (e.g. weekly), and whether I want the report emailed to me (`notify=email`) or just saved to the workspace (`notify=none`, the default).
- Do not create the scheduled agent until I explicitly confirm frequency and notify preference.
- Explain plainly what the automation will do (run the 3-skill pipeline, write files under `Content/Market-Research/`, and optionally email me the report), how often it will run, and that each run is a full Zo session — without inventing a specific cost figure.
- If I confirm, create a scheduled agent using the instructions in `automation-prompt.md` (filled in with my values) as its prompt, on the frequency I gave.
- If I decline or want to think about it, skip this step — ad hoc usage still works without it.

### 6. Report back

- Confirm: the install path, whether the folder was overwritten or created fresh, the niche and any competitors/focus areas added to `config/research-topic.md`, which persona was created (and whether it's active), and whether a recurring automation was set up (with its schedule and notify setting) or left for later.
- Give me one ready-to-run example prompt from `starter-prompts.md` so I can try the pipeline ad hoc right away.

Throughout all of this, do not read, write, or modify any files outside `/home/workspace/Zo-Automations/ai-market-research-agent/` and `/home/workspace/Content/Market-Research/`, and do not call any paid or external API — this automation is built to run entirely on free, built-in Zo tools (`web_search`, `web_research`, `x_search`, `read_webpage`/`view_webpage`) with no sign-up or API key required.
