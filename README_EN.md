[中文](./README.md) | **English**

# Gemini CLI Skill for Claude Code

## Purpose

This plugin makes Google Gemini CLI a first-class colleague for Claude Code. Instead of being a tool called on demand, Gemini becomes an always-available AI wingman that Claude proactively leverages to compensate for its own limitations — real-time web search, 2M-token codebase analysis, audio processing, concurrent sub-agents, and cross-validation of reasoning.

## Prerequisites
- **Platform:** Unix-like shell required (macOS, Linux, or Windows with Git Bash/WSL). Native PowerShell/CMD is not supported.
- `gemini` CLI installed and available in `PATH` (run `gemini --version` to verify).
- Gemini credentials configured (API key via `GEMINI_API_KEY` env var, or OAuth via `gemini auth login`).

## Installation

### Option 1: Plugin Marketplace (Recommended)

```
/plugin marketplace add qichiyuhub/skill-gemini-cli
/plugin install skill-gemini-cli@skill-gemini-cli
```

### Option 3: Standalone Skill

```bash
git clone --depth 1 https://github.com/qichiyuhub/skill-gemini-cli.git /tmp/skills-temp
cp -r /tmp/skills-temp/plugins/skill-gemini-cli/skills/gemini-cli ~/.claude/skills/gemini-cli
rm -rf /tmp/skills-temp
```

Then reference the skill path in your project `CLAUDE.md` or enable it via Claude Code skill configuration.

## What Gemini Brings to the Table

| Claude's Limitation | Gemini's Strength |
|---|---|
| Knowledge cutoff | `google_web_search` — native Google Search for real-time info |
| Context window limit | 2M token context — analyze entire codebases at once |
| Single-threaded | `generalist` / `codebase_investigator` concurrent sub-agents |
| No audio processing | Multi-modal: MP3, WAV, FLAC analysis |
| Image analysis limits | Multi-modal: PNG, JPG, GIF, WEBP, SVG, BMP second opinion |
| Reasoning uncertainty | Cross-validation as a second AI opinion |
| No native search engine | Fact verification, latest docs lookup |
| Stuck in logic loop | Fresh-eye review after 2+ failed attempts |

## How It Works

Claude will **proactively** call Gemini when it recognizes a task that matches Gemini's strengths. No need to say "use Gemini" — Claude decides autonomously based on the situation.

### Example Workflows

**Real-time search:**
```
User: What are the breaking changes in React 19.1?
Claude: (recognizes post-cutoff question, calls Gemini with google_web_search)
```

**Large codebase analysis:**
```
User: Map out all the dependency chains in this monorepo.
Claude: (delegates to Gemini's codebase_investigator with 2M token context)
```

**Cross-validation:**
```
User: Is this concurrency logic correct?
Claude: (analyzes first, then asks Gemini to independently verify)
```

**Audio analysis:**
```
User: Transcribe and summarize this meeting recording.
Claude: (delegates audio file to Gemini's multi-modal processing)
```

## Delegation Modes

| Mode | Use Case |
|---|---|
| `auto_edit` (default) | Analysis, search, code review, read + edit tasks |
| `yolo` / `-y` | Full autonomy when user authorizes, or task requires file modifications |
| `plan` | Read-only planning and architecture proposals |

## Skill Reference

Full execution rules, error handling, and collaboration protocol: [`plugins/skill-gemini-cli/skills/gemini-cli/SKILL.md`](plugins/skill-gemini-cli/skills/gemini-cli/SKILL.md)

## Troubleshooting

| Problem | Solution |
|---|---|
| `gemini: command not found` | Install Gemini CLI and ensure it is in your `PATH`. |
| Auth error on first call | Run `! gemini` interactively in Claude Code to authenticate, or set `GEMINI_API_KEY` env var. |
| Rate limit (429) errors | The skill auto-downgrades model tier (`pro` -> `flash` -> `flash-lite`). If persistent, check your API quota. |
| Command hangs | Ensure `-p` and `--approval-mode` flags are present. Kill and retry. |
| Slow first invocation | Normal — Gemini CLI initializes extensions on first run. Subsequent calls are fast. |

## License

MIT — see [LICENSE](LICENSE).
