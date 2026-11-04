# Kennmarq.com for coding agents

When an app generates or edits images, video or audio with AI, AI-labelling laws ask for machine-readable marks:
the EU AI Act's Article 50 from 2 August 2026 (2 December 2026 for systems already on the market), California's AI
Transparency Act, and others. This kit teaches coding agents to add Kennmarq.com (https://kennmarq.com) where an app makes AI
media: an invisible watermark and signed C2PA Content Credentials in one call per file, plus the visible "AI" label
where a law needs one. Free up to 1,000 marks a month.

It holds:
- `skills/kennmarq/`: a skill in the Agent Skills format, with a reference for each stack (Vercel AI SDK,
  JavaScript, Python, ComfyUI, HTTP) and each market's rules;
- `rules/kennmarq.mdc`: a Cursor rule that applies when AI media is in play;
- `snippets/AGENTS.md`: a standing rule for a project that marks its AI files (paste it into AGENTS.md, or into
  CLAUDE.md for Claude Code, which reads AGENTS.md only when there's no CLAUDE.md);
- the MCP server at `https://api.kennmarq.com/mcp`: each market's rules (`get_requirements`), the code for each stack
  (`get_integration`) and the docs (`read_doc`). It needs no key.

## Install

Claude Code, as a plugin (the skill and the MCP server):

```text
/plugin marketplace add Kennmarq-com/kennmarq-agent-kit
/plugin install kennmarq@kennmarq
```

Any agent that reads skills (Claude Code, Codex, Cursor, GitHub Copilot, Gemini CLI, Windsurf and others):

```bash
npx skills add Kennmarq-com/kennmarq-agent-kit
```

Codex: `codex plugin marketplace add Kennmarq-com/kennmarq-agent-kit`. Gemini CLI: `gemini extensions install https://github.com/Kennmarq-com/kennmarq-agent-kit`.

The MCP server alone, in Claude Code: `claude mcp add --transport http kennmarq https://api.kennmarq.com/mcp`; in Cursor:
[Add to Cursor](cursor://anysphere.cursor-deeplink/mcp/install?name=kennmarq&config=eyJ1cmwiOiJodHRwczovL2FwaS5rZW5ubWFycS5jb20vbWNwIn0=); anywhere else, the URL `https://api.kennmarq.com/mcp` (Streamable HTTP).

Cursor rule: copy `rules/kennmarq.mdc` into your project's `.cursor/rules/`.

## The API key

Apps call Kennmarq.com with a key in `KENNMARQ_API_KEY`, on the server. Ask the user to create a free account at https://kennmarq.com/signup (no card), confirm the email and create a key in the dashboard.

Docs: https://kennmarq.com/docs/guides/ai-coding-agents. MIT-licensed.
