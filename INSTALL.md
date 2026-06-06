# Installation Guide

## Step 1 — Download Noel Crew

### Windows
Download the installer from GitHub Releases:
```
https://github.com/noelclaw/noel-crew/releases/latest
```
Run `NoelCrew-win-x64-setup.exe` → follow the installer.

### Mac
```
https://github.com/noelclaw/noel-crew/releases/latest
```
Download `NoelCrew-mac.dmg` → drag to Applications.

### Linux
```
https://github.com/noelclaw/noel-crew/releases/latest
```
Download `NoelCrew-linux.AppImage` → make executable:
```bash
chmod +x NoelCrew-linux.AppImage
./NoelCrew-linux.AppImage
```

---

## Step 2 — Launch Noel Crew

After installing, Noel Crew runs in your **system tray** (bottom right corner).

Click the tray icon → **Pet Manager** → pick your crew member → **Set Default**.

Your pet now lives in the corner of your screen! 🐾

---

## Step 3 — Connect to Your AI Agent

See the [Claude Code hooks docs](https://docs.anthropic.com/en/docs/claude-code/hooks) for the full connection guide.

**Quick setup for Claude Code:**

Create or edit `%APPDATA%\Claude\settings.json` (Windows) or `~/.claude/settings.json` (Mac/Linux):

```json
{
  "hooks": {
    "PreToolUse": [
      {
        "matcher": ".*",
        "hooks": [
          {
            "type": "command",
            "command": "node --input-type=module --eval \"import{createNoelCrewClient}from'file:///C:/Users/YOUR_USERNAME/noelcrew/packages/client/dist/index.js';createNoelCrewClient().react('working').catch(()=>{})\""
          }
        ]
      }
    ],
    "Stop": [
      {
        "hooks": [
          {
            "type": "command",
            "command": "node --input-type=module --eval \"import{createNoelCrewClient}from'file:///C:/Users/YOUR_USERNAME/noelcrew/packages/client/dist/index.js';createNoelCrewClient().react('celebrating').catch(()=>{})\""
          }
        ]
      }
    ]
  }
}
```

Replace `YOUR_USERNAME` with your Windows username.

Restart Claude Code → watch your pet react! 🎉
