# OpenCode Configs

My personal configuration files for [OpenCode](https://opencode.ai) and [Oh-My-OpenCode](https://github.com/code-yeongyu/oh-my-opencode).

## Files

| File | Description |
|------|-------------|
| `opencode.json` | Main OpenCode config - providers, MCPs, plugins, keybinds |
| `oh-my-opencode.json` | Oh-My-OpenCode agent model assignments |

## Setup

1. Copy configs to `~/.config/opencode/`:
   ```bash
   cp opencode.json oh-my-opencode.json ~/.config/opencode/
   ```

2. Set environment variables (or replace placeholders in `opencode.json`):
   ```bash
   export MORPH_API_KEY="your-morph-api-key"
   export EXA_API_KEY="your-exa-api-key"
   ```

3. Start the API proxy (see below)

---

## Authentication Options

### Option 1: Simple Setup (No Proxy)

Use [opencode-google-antigravity-auth](https://github.com/shekohex/opencode-google-antigravity-auth) - a plugin that handles authentication directly without needing a local proxy.

1. Add to your `opencode.json` plugins:
   ```json
   {
     "plugin": [
       "opencode-google-antigravity-auth"
     ]
   }
   ```

2. Authenticate:
   ```bash
   opencode auth login
   ```

That's it. No proxy required.

---

### Option 2: Local Proxy Setup (Advanced)

For more control, you can run a local API proxy on `localhost:8317` to route requests through Google's Antigravity/Gemini infrastructure. This enables access to Claude models via Gemini's API.

#### macOS: VibeProxy

[VibeProxy](https://github.com/automazeio/vibeproxy/) is a macOS wrapper for CLIProxyAPI.

```bash
# Install via Homebrew (check repo for latest instructions)
brew install --cask vibeproxy

# Or download from releases
# https://github.com/automazeio/vibeproxy/releases
```

Once running, it exposes:
- `http://localhost:8317/v1` → Anthropic-compatible API
- `http://localhost:8317/v1beta` → Gemini-compatible API

#### Windows: CLIProxyAPI

[CLIProxyAPI](https://github.com/router-for-me/CLIProxyAPI/) is the underlying proxy that VibeProxy wraps.

```powershell
# Download from releases
# https://github.com/router-for-me/CLIProxyAPI/releases

# Run the executable
.\CLIProxyAPI.exe
```

Exposes the same endpoints on `localhost:8317`.

---

## Monitoring Antigravity Quota

### Cross-Platform: AntigravityQuota

[AntigravityQuota](https://github.com/Henrik-3/AntigravityQuota) works on both macOS and Windows.

```bash
# Check repo for installation instructions
# https://github.com/Henrik-3/AntigravityQuota
```

### macOS Only: CodexBar

[CodexBar](https://github.com/steipete/CodexBar/) is a macOS menu bar app for monitoring quota.

```bash
# Install via Homebrew
brew install --cask codexbar

# Or download from releases
# https://github.com/steipete/CodexBar/releases
```

---

## Provider Overview

| Provider | Description | Auth |
|----------|-------------|------|
| `google` | Direct Google AI (Gemini models) | Google Antigravity Auth plugin |
| `openai` | OpenAI Codex API | Codex Auth plugin |
| `vibeproxy-anthropic` | Claude via local proxy | Dummy key (proxy handles auth) |
| `vibeproxy-gemini` | Gemini via local proxy | Dummy key (proxy handles auth) |
| `fetchs` | Free tier access (zAI) | Public key |
| `openrouter-shortcut` | OpenRouter models | Requires `OPENROUTER_API_KEY` |

## MCP Servers

| MCP | Description | Status |
|-----|-------------|--------|
| `morph-mcp` | Morph LLM editing tools | Enabled |
| `context7` | Context7 documentation lookup | Enabled |
| `exa` | Exa web search | Enabled |
| `browsermcp` | Browser automation | Enabled |
| `wizlight` | Smart home lights | Disabled |
| `chrome-devtools` | Chrome DevTools | Disabled |

### Bring Your Own Keys (BYOK)

**Both Morph and Exa require your own API keys.** Replace the placeholders in `opencode.json`:

```bash
# In opencode.json, replace:
# ${MORPH_API_KEY} → your Morph API key
# ${EXA_API_KEY} → your Exa API key
```

#### Morph MCP

[Morph](https://morphllm.com) provides fast codebase reading and editing tools:

- **`warpgrep_codebase_search`** - Sends a swarm of subagents that read your codebase in parallel, extremely fast for large repos
- **`edit_file`** - Fast file editing without reading entire files into context

This MCP helps avoid repeated file reads and writes, which can save money/usage if you're paying for API calls. However, **the MCP itself is still somewhat expensive** - factor this into your costs.

Get your key at: https://morphllm.com

#### Exa MCP

[Exa](https://exa.ai) is an excellent search tool. Oh-My-OpenCode includes a built-in Exa integration, but I use my own MCP with a **paid API key** to avoid hitting rate limits on the free tier.

If you don't need heavy search usage, you can disable this MCP and rely on the built-in Oh-My-OpenCode Exa tool instead.

Get your key at: https://exa.ai

## Plugins

- `oh-my-opencode@2.5.4` - Enhanced agent system
- `opencode-google-antigravity-auth` - Google AI authentication
- `opencode-openai-codex-auth@4.2.0` - OpenAI Codex authentication
- `@franlol/opencode-md-table-formatter@0.0.3` - Markdown table formatting

---

## License

Personal configs - use at your own risk.
