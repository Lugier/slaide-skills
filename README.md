# Slaide agent skills

Agent skills for [Slaide](https://www.slaide.de), an AI presentation maker. They teach a coding or chat agent how to build, edit and export PowerPoint presentations, Word documents and Excel workbooks through the Slaide MCP server.

## Install

```bash
npx skills add Lugier/slaide-skills
```

Works with Claude Code, Cursor, Codex, OpenCode and the other agents supported by the [skills CLI](https://github.com/vercel-labs/skills).

## Skills

| Skill | What it does |
|---|---|
| [`ai-presentation-maker`](skills/ai-presentation-maker/SKILL.md) | Build, edit and export slide decks, documents and workbooks with Slaide: search, read, create, update, check jobs, export as PowerPoint, PDF, Word, PNG or Excel |

## Requirements

The skill uses the Slaide MCP server, `https://app.slaide.de/api/mcp` (OAuth 2.1, no API key). A free Slaide account is enough: [sign up](https://app.slaide.de/registrieren).

## Links

- Website: [slaide.de](https://www.slaide.de)
- Slaide for AI assistants: [slaide.de/ai-assistants](https://www.slaide.de/ai-assistants)
- MCP server repository: [Lugier/slaide-mcp](https://github.com/Lugier/slaide-mcp)
- Pricing: [slaide.de/pricing](https://www.slaide.de/pricing) · Privacy: [slaide.de/privacy](https://www.slaide.de/privacy) · Terms: [slaide.de/terms](https://www.slaide.de/terms)

## License

MIT, see `LICENSE`. Using the Slaide service is governed by the [Slaide terms](https://www.slaide.de/terms).
