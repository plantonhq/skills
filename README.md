# Planton Skills

[![skills.sh](https://skills.sh/b/plantonhq/skills)](https://skills.sh/plantonhq/skills)

Agent skills that let your coding agent work with [Planton](https://planton.ai): compose cloud infrastructure, validate it, deploy it, and run services -- from Cursor, Claude Code, Codex, Gemini CLI, and any other host that reads the [Agent Skills](https://agentskills.io) format.

Ask your agent for "a Postgres for this service in dev" and it writes a grounded manifest under `infrastructure/` in your repository, validates it with the `planton` CLI, tells you what it will cost, and asks before applying.

## What is here

| Skill | What it carries |
| --- | --- |
| `skills/planton` | The working craft: Infra Charts and manifest sets, the compile loop, wiring resources by reference, deployed projects, service registration, push-to-deploy, CI/CD, and the boundaries that never bend (no mutation without consent, never outside your repository). |
| `skills/multi-cloud-catalog` | The component reference pack, shipped inside the skill: one page per cloud component across every supported provider (spec fields, validation rules, outputs, wiring), the catalog-wide reference graph, and verified fact sheets for cost, security posture, and runner permissions. Facts are read from these files at answer time, never recalled from memory. |

## Install

**One command, any agent** (the [skills CLI](https://github.com/vercel-labs/skills) detects your agents and offers project or global scope):

```bash
npx skills add plantonhq/skills
```

Target one agent explicitly, or install for every agent on the machine:

```bash
npx skills add plantonhq/skills --agent cursor
npx skills add plantonhq/skills --agent claude-code --global
npx skills add plantonhq/skills --agent '*' -y
```

**Claude Code marketplace:**

```
/plugin marketplace add plantonhq/skills
/plugin install planton@planton
```

**Manual** (any agent): clone this repository and copy or symlink `skills/planton` and `skills/multi-cloud-catalog` into your agent's skills directory -- `~/.cursor/skills/`, `~/.claude/skills/`, `~/.codex/skills/`, or `.agents/skills/` inside a project. `SKILL.md` must sit directly inside each skill folder.

## Pair it with the CLI

The skill does its best work with the `planton` CLI on your PATH: validation and schema lookups run fully offline, and one `planton login` unlocks the compile loop, lookups, and deploys against your organization.

```bash
brew install plantonhq/tap/planton   # macOS; other platforms: https://planton.ai/docs/cli
planton login
```

## Optional: the platform's tools over MCP

Without the CLI, or in addition to it, your agent can reach the platform's own operations (build, apply, deploy, read your organization's estate) through the hosted Planton MCP server. Create an API key in the Planton console, export it as `PLANTON_API_KEY`, then:

- **Claude Code:** `/plugin install planton-platform-tools@planton`
- **Cursor** (`~/.cursor/mcp.json` or `.cursor/mcp.json`):

  ```json
  {
    "mcpServers": {
      "planton": {
        "type": "http",
        "url": "https://mcp.planton.ai/",
        "headers": { "Authorization": "Bearer ${env:PLANTON_API_KEY}" }
      }
    }
  }
  ```

The full guide, including the first journey end to end, is at [planton.ai/docs/coding-agents](https://planton.ai/docs/coding-agents).

## Versions

Every commit on `main` is a Planton release, tagged with the release's tag (for example `v0.5.41`). The content under `skills/` is byte-identical to the checksummed skill archives that release published to Planton's downloads CDN, and it is what Planton Desktop and the hosted assistant run at that version. `npx skills update` brings the latest release; pin a release with the tag.

## Where the content comes from

`skills/` is release output. The skills are authored in [plantonhq/planton](https://github.com/plantonhq/planton) under `skills/`, linted against the Agent Skills specification on every pull request, and copied here by that repository's release lane. Pull requests that change files under `skills/` in this repository are refused with a pointer to the source; improvements are welcome there.

## License

Apache-2.0, the same as the source repository.
