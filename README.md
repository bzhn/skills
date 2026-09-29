# bzhn/skills

Stack-agnostic skills for coding agents — Claude Code first, Cursor and others through the open [Agent Skills](https://agentskills.io) format. Each skill does one job, installs on its own and comes from real day-to-day development work.

## Install

### Claude Code

```
/plugin marketplace add bzhn/skills
/plugin install bzhn-skills@bzhn
```

Skills are then available as `/bzhn-skills:<skill-name>`.

### Other agents (Cursor, Codex, Gemini CLI, …)

```
npx skills add bzhn/skills
```

## Skills

| Skill | What it does |
|---|---|
| [`review-to-gitlab-format`](skills/review-to-gitlab-format/SKILL.md) | Converts a code review into paste-ready GitLab MR comments with file:line, severity, a plain-English TLDR and collapsible details. Saves them as an Obsidian-friendly note with an index table and "Posted" checkboxes. |
| [`teach-by-asking`](skills/teach-by-asking/README.md) | Teaches a topic by questioning you first and correcting your answers, with hints, confidence checks, a glossary and a recap to review later. Based on learning research ([why it works](skills/teach-by-asking/README.md#why-it-works)). |

## License

[MIT](LICENSE)
