# Homebrew tap for the AgentDB CLI

AgentDB is a persistent, typed database for AI agents. This CLI logs you in and connects your AI tools to it.

## Install and log in

```bash
brew install gleert/agentdb/agentdb
```

```bash
agentdb login
```

`login` opens your browser. Approve with your API key, or create a workspace there. The key is stored in your macOS Keychain: never in chats, command lines or AI tool config files.

Upgrade later with `brew upgrade agentdb`.

## Connect your AI tool

One command per tool. It registers `agentdb mcp proxy`, which uses your saved login, so **no API key is written to the tool's settings**.

| Tool | Command | Status |
|---|---|---|
| Claude Code | `agentdb mcp install claude` | Tested |
| OpenAI Codex (CLI and IDE extension) | `agentdb mcp install codex` | Tested (config read by Codex) |
| Cursor | `agentdb mcp install cursor` | Experimental |
| VS Code (Copilot agent mode) | `agentdb mcp install vscode` | Experimental |
| Claude Desktop | `agentdb mcp install claude-desktop` | Experimental |
| Windsurf | `agentdb mcp install windsurf` | Experimental |
| Gemini CLI | `agentdb mcp install gemini` | Config verified (`gemini mcp list`: Connected); use a trusted folder |

Then **start a new chat** in the tool and ask: *"Using agentdb, list my objects."*

- An existing `agentdb` server entry (for example an older setup with the URL and key) is only replaced with `--force`. Your other servers and settings are kept, and the previous file is saved as `*.agentdb-backup`.
- Check what's connected: `agentdb mcp list`. Remove: `agentdb mcp remove <tool>`.
- Restart Cursor or VS Code after installing, if the tool doesn't show up.

### Other tools (manual, not yet tested)

Any tool that can start a local (stdio) MCP server can use `agentdb mcp proxy`. Run `which agentdb` for the full path and add a server named `agentdb` with that command and the arguments `mcp proxy`.

**Web apps** (claude.ai, ChatGPT) can't start local programs, so they connect to `https://alpha.agentdb.link/mcp` directly. See the setup page shown after you create your workspace.

## Workspaces

```bash
agentdb workspace list
agentdb workspace create "My Application"
agentdb workspace use my-application        # add --project to keep it per project (.agentdb.json)
agentdb workspace current
```

Agents can do the same over MCP (`workspace_list`, `workspace_create`, `workspace_select`).

## Teach your agent

```bash
agentdb skill install claude
```

This installs the AgentDB skill for Claude Code. For other agents: `agentdb skill print`, or point them to https://alpha.agentdb.link/skill.md.

## Everyday commands

```bash
agentdb status
agentdb schema inspect
agentdb records list <object>
agentdb files upload <path>
```

Output is JSON. Scripts can use `AGENTDB_TOKEN` and `AGENTDB_API_URL` instead of a login.

## Notes

- `agentdata` is the old name and still works for now.
- If new AgentDB tools are missing in your AI tool after an upgrade, start a new chat or reconnect the server.
- Downloads come from `https://alpha.agentdb.link/downloads/cli/` and are checked against the SHA-256 sums in `Formula/agentdb.rb`.
- AgentDB is in alpha: store only test or non-sensitive data. More: https://agentdb.link
