# Noel Crew — Desktop Pet Companion

Noel Crew is a tray-first desktop companion that lives on your screen and reacts in real-time as your AI agent works. Each agent action — editing files, running tests, waiting, finishing a task — triggers a matching animation on the pet.

Local-first. No cloud. No telemetry.

---

## How It Works

Your AI agent connects to Noel Crew via MCP or local IPC. When it calls a reaction tool, the desktop pet plays the matching animation instantly.

| Agent activity | Pet animation |
|----------------|---------------|
| Thinking, starting a task | `working` |
| Editing files | `editing` |
| Running terminal commands | `running` |
| Running tests | `testing` |
| Waiting for input | `waiting` |
| Task finished | `celebrating` |
| Error or failure | `error` |
| Signal fired | `celebrating` |
| Research in progress | `working` |

---

## Get Started

The fastest way to install Noel Crew is via MCP skill — no download needed.

Skip to the "Install as MCP Skill" section below.

> Desktop app installer coming soon at github.com/noelclaw/noel-crew/releases

---

## Install as MCP Skill

The `@noelclawai/crew` MCP server connects your AI client to the desktop pet. No build step — runs via `npx`.

### Claude Code

```bash
claude mcp add noel-crew -- npx @noelclawai/crew
```

Verify:
```bash
claude mcp list
# noel-crew   npx @noelclawai/crew
```

### Hermes

```bash
hermes mcp add noel-crew -- npx @noelclawai/crew
```

Reload without restarting:
```
/reload-mcp
```

### OpenClaw

```bash
openclaw mcp add noel-crew -- npx @noelclawai/crew
```

### Cursor / Windsurf / Any MCP Client

Edit your MCP config (`~/.cursor/mcp.json`, `~/.windsurf/mcp.json`, or equivalent):

```json
{
  "mcpServers": {
    "noel-crew": {
      "type": "stdio",
      "command": "npx",
      "args": ["-y", "@noelclawai/crew"]
    }
  }
}
```

Restart your client. Tools appear automatically.

---

## Auto-Reactions for Claude Code

Wire Noel Crew into Claude Code hooks so the pet reacts automatically on every tool call — no manual triggers needed.

Add to `%APPDATA%\Claude\settings.json` (Windows) or `~/.claude/settings.json` (Mac/Linux):

```json
{
  "hooks": {
    "PreToolUse": [
      {
        "matcher": ".*",
        "hooks": [{"type": "command", "command": "node --input-type=module --eval \"import{createNoelCrewClient}from'file:///PATH_TO_NOELCREW/packages/client/dist/index.js';createNoelCrewClient().react('working').catch(()=>{})\""}]
      }
    ],
    "PostToolUse": [
      {
        "matcher": "Edit|Write|MultiEdit",
        "hooks": [{"type": "command", "command": "node --input-type=module --eval \"import{createNoelCrewClient}from'file:///PATH_TO_NOELCREW/packages/client/dist/index.js';createNoelCrewClient().react('editing').catch(()=>{})\""}]
      },
      {
        "matcher": "Bash",
        "hooks": [{"type": "command", "command": "node --input-type=module --eval \"import{createNoelCrewClient}from'file:///PATH_TO_NOELCREW/packages/client/dist/index.js';createNoelCrewClient().react('running').catch(()=>{})\""}]
      }
    ],
    "Stop": [
      {
        "hooks": [{"type": "command", "command": "node --input-type=module --eval \"import{createNoelCrewClient}from'file:///PATH_TO_NOELCREW/packages/client/dist/index.js';createNoelCrewClient().react('celebrating').catch(()=>{})\""}]
      }
    ]
  }
}
```

Replace `PATH_TO_NOELCREW`:
- Windows: `C:/Users/YOUR_USERNAME/noelcrew`
- Mac/Linux: `/home/user/noelcrew`

---

## Available Reactions

Call any reaction directly via MCP or the local client:

| Reaction | When to use |
|----------|-------------|
| `idle` | Default — no active task |
| `working` | General agent activity, thinking |
| `editing` | Writing or modifying files |
| `running` | Executing terminal commands or scripts |
| `testing` | Running tests or validation |
| `waiting` | Awaiting user input or approval |
| `celebrating` | Task complete, signal fired |
| `error` | Failure, exception, or timeout |
| `waving` | Greeting or first contact |
| `thinking` | Long reasoning or planning |

---

## MCP Tools

| Tool | Description |
|------|-------------|
| `noelcrew_status` | Check whether Noel Crew is running and which pet is targeted |
| `noelcrew_react` | Set a reaction on the desktop pet (`working`, `editing`, `error`, etc.) |
| `noelcrew_say` | Show a short message bubble on the pet — safe text only, no code or URLs |
| `noel_signal_fired` | Signal fired → celebrating animation |
| `noel_whale_alert` | Whale alert detected → waiting animation |
| `noel_research_start` | Research beginning → working animation |
| `noel_research_complete` | Research finished → celebrating animation |
| `noel_swap_executing` | Swap in progress → running animation |
| `noel_error` | Error condition → error animation |

---

## Add a Custom Pet

Noel Crew supports custom character packs loaded from a local directory.

1. Open the tray icon → **Pet Manager**
2. Switch to the **Codex** tab
3. Click **Import** next to a character

Codex pets live in `~/.codex/pets/<id>/` and consist of two files:

```
~/.codex/pets/my-pet/
  spritesheet.webp   9-row × 8-col animation sheet (192×208 px per frame)
  pet.json           { "id", "displayName", "description", "spritesheetPath" }
```

Custom pets use the same 9-row animation layout as built-in characters. Each row maps to a reaction state (idle, walking, waving, celebrating, thinking, working, running, error, success).

---

## Troubleshooting

| Problem | Fix |
|---------|-----|
| Pet doesn't react | Make sure Noel Crew is running (tray icon visible) and MCP is connected |
| `npx: command not found` | Install Node.js 18+ from nodejs.org |
| `spawn npx ENOENT` | Use full path: find it with `where npx` (Windows) or `which npx` (Mac/Linux) |
| Hooks not firing in Claude Code | Confirm hooks are in `~/.claude/settings.json`, not `%APPDATA%\Claude\settings.json` |
| Wrong pet showing | Open Pet Manager → set the active pet |
