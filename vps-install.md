# Install on VPS

Deploy the Noelclaw MCP server on a Linux VPS so Hermes, OpenClaw, or any agent on the server can use Noel's tools.

---

## Requirements

- Node.js >= 18
- npm or pnpm
- Linux (Ubuntu 20.04+ recommended)

---

## Step 1 — Copy Files to VPS

**Option A — Upload folder directly (SCP):**
```bash
# From your local machine
scp -r C:/Users/sagir/Downloads/noelapp/mcp-server user@your-vps-ip:/srv/noelclaw-mcp
```

**Option B — Clone from GitHub** (if you push it):
```bash
git clone https://github.com/your-username/noelclaw-mcp /srv/noelclaw-mcp
```

---

## Step 2 — Install Dependencies and Build

```bash
cd /srv/noelclaw-mcp
npm install --production
npm run build
```

Verify it works (should start silently, waiting for stdin):
```bash
node dist/index.js
# Press Ctrl+C to exit
```

---

## Step 3 — Test Locally

Quick test with a raw MCP call:
```bash
echo '{"jsonrpc":"2.0","method":"tools/list","params":{},"id":1}' | node dist/index.js
```

Should return the list of 34 tools in JSON.

---

## Step 4 — Set Up as a Service (systemd)

If you want the MCP server available as a persistent process (for HTTP-mode clients):

Create `/etc/systemd/system/noelclaw-mcp.service`:
```ini
[Unit]
Description=Noelclaw MCP Server
After=network.target

[Service]
Type=simple
User=ubuntu
WorkingDirectory=/srv/noelclaw-mcp
ExecStart=/usr/bin/node dist/index.js
Restart=always
RestartSec=10
Environment=NOELCLAW_CONVEX_URL=https://valuable-fish-533.convex.site
StandardInput=null

[Install]
WantedBy=multi-user.target
```

Enable and start:
```bash
systemctl daemon-reload
systemctl enable noelclaw-mcp
systemctl start noelclaw-mcp
systemctl status noelclaw-mcp
```

> **Note:** For stdio-mode MCP clients (Hermes, Claude), you don't need systemd — the client launches the process on demand. systemd is only needed if you're exposing an HTTP endpoint.

---

## Step 5 — Point Hermes/OpenClaw to It

Once the files are on the VPS, add the absolute path to your Hermes config:

```yaml
# ~/.hermes/config.yaml
mcp_servers:
  noelclaw:
    command: node
    args:
      - /srv/noelclaw-mcp/dist/index.js
    env:
      NOELCLAW_CONVEX_URL: https://valuable-fish-533.convex.site
    timeout: 30
    connect_timeout: 10
```

Then in Hermes:
```
/reload-mcp
```

---

## Keeping It Updated

```bash
cd /srv/noelclaw-mcp

# Pull new changes (if using git)
git pull

# Rebuild
npm install
npm run build

# Restart systemd service if running
systemctl restart noelclaw-mcp
```

---

## Directory Structure on VPS

```
/srv/noelclaw-mcp/
├── dist/
│   └── index.js        ← compiled entry point
├── src/
│   └── index.ts        ← source
├── package.json
├── tsconfig.json
└── node_modules/
```

Only `dist/` and `node_modules/` are needed at runtime. The `src/` folder is optional on the VPS if you don't plan to rebuild there.

---

## Troubleshooting

**`node: not found`** — Install Node.js:
```bash
curl -fsSL https://deb.nodesource.com/setup_20.x | sudo -E bash -
sudo apt-get install -y nodejs
```

**`ENOENT dist/index.js`** — Run `npm run build` first.

**Hermes can't connect** — Check the absolute path is correct:
```bash
ls /srv/noelclaw-mcp/dist/index.js
```

**API errors from Convex** — The `NOELCLAW_CONVEX_URL` is the production URL, it works without any local setup. No Convex account needed to run the MCP server.
