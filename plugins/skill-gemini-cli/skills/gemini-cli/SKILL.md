---
name: gemini-cli
description: Call Gemini CLI as your AI colleague for real-time web search, 2M-token codebase analysis, multi-modal processing, concurrent sub-agents, and cross-validation when you hit your own limits.
---

# Gemini CLI

Gemini CLI runs headless as an AI colleague. For rare flags, run `gemini --help` at runtime.

## Capability Triggers

- **Web search** — native `google_web_search`, anything beyond your knowledge cutoff
- **2M token context** — large file / full codebase analysis exceeding your context window
- **Multi-modal** — image (PNG/JPG/GIF/WEBP/SVG) and audio (MP3/WAV/FLAC) as positional args
- **Sub-agents** — `codebase_investigator` (architecture/deps), `generalist_agent` (parallel/bulk)
- **Cross-validation** — second opinion when stuck after 2+ attempts

## Standard Call

```bash
PROMPT='your prompt here'
gemini -p "$PROMPT" --model flash --approval-mode auto_edit -o text
```

| Element | Why |
|---|---|
| `-p` | **REQUIRED.** Headless mode — omitting causes interactive hang. |
| `--approval-mode` | **REQUIRED.** `default` (prompt for approval), `auto_edit` (auto-approve edits, no shell), `yolo`/`-y` (all tools, review after), `plan` (read-only). |
| `--model` | `pro` (complex), `flash` (default), `flash-lite` (simple). |
| Quoted `$PROMPT` | Never interpolate raw user text. Always use a shell variable. |
| `timeout: 300000` | 5-min timeout on Bash tool calls. |

## Web Search

```bash
gemini -p "Use google_web_search to find: <query>. Summarize with source URLs." --model flash --approval-mode auto_edit -o text
```

## Codebase Analysis

```bash
gemini -p "Use codebase_investigator to: <task>. Summarize findings." --model pro --approval-mode auto_edit -o text
```

## Multi-modal

```bash
gemini -p "Describe this" ./img.png --approval-mode auto_edit -o text
```

File paths as positional args after `-p`. Supports image and audio formats.

## Rules

1. **Context slicing**: pass file paths, let Gemini read them itself
2. **Session isolation**: fresh session each call, no `-r` unless continuing same task

## Troubleshooting

- **Hangs** → missing `-p` or `--approval-mode`. **429** → downgrade `pro`→`flash`→`flash-lite`.
- **Auth** → check `GEMINI_API_KEY` or `! gemini` to re-auth.
