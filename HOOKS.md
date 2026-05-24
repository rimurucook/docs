# Connecting Noel Crew to AI Agents

Noel Crew connects to any AI agent via a simple local IPC pipe.
No cloud, no API keys — everything runs locally.

---

## Available Reactions

```
idle        thinking    working     editing
running     testing     waiting     waving
success     error       celebrating
```

---

## Claude Code

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

Replace `PATH_TO_NOELCREW` with your install path, e.g.:
- Windows: `C:/Users/sagir/noelcrew`
- Mac/Linux: `/home/user/noelcrew`

---

## Hermes

Add to your Hermes config or hooks file:

```bash
# On agent start
node --input-type=module --eval "import{createNoelCrewClient}from'file:///PATH_TO_NOELCREW/packages/client/dist/index.js';createNoelCrewClient().react('thinking').catch(()=>{})"

# On file edit
node --input-type=module --eval "import{createNoelCrewClient}from'file:///PATH_TO_NOELCREW/packages/client/dist/index.js';createNoelCrewClient().react('editing').catch(()=>{})"

# On task complete
node --input-type=module --eval "import{createNoelCrewClient}from'file:///PATH_TO_NOELCREW/packages/client/dist/index.js';createNoelCrewClient().react('celebrating').catch(()=>{})"
```

---

## OpenClaw

Same as Hermes — add shell hooks to your OpenClaw workflow triggers:

```bash
# Trigger any reaction manually
node --input-type=module --eval "import{createNoelCrewClient}from'file:///PATH_TO_NOELCREW/packages/client/dist/index.js';createNoelCrewClient().react('working').catch(()=>{})"
```

---

## Any MCP-Compatible Agent

Add the Noel Crew MCP server to your agent:

```bash
claude mcp add noel-crew node "PATH_TO_NOELCREW/packages/mcp/dist/index.js"
```

Your agent can then call these MCP tools directly:
- `noelcrew_react` — trigger a reaction
- `noelcrew_say` — show a speech bubble
- `noelcrew_status` — check if pet is running

---

## Manual Trigger (Testing)

```bash
node --input-type=module --eval "import{createNoelCrewClient}from'file:///PATH_TO_NOELCREW/packages/client/dist/index.js';createNoelCrewClient().react('celebrating').then(r=>console.log(r)).catch(e=>console.error(e))"
```
