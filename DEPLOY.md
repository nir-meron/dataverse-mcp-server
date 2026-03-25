# Deploy to Wix Workspace

## Step 1 — Build the server

```bash
cd wix-dataverse-mcp/mcp-server
npm install
npm run build
```

Verify `mcp-server/dist/index.js` exists.

## Step 2 — Install mcpb CLI

```bash
npm install -g @anthropic-ai/mcpb
```

## Step 3 — Package the extension

From the `wix-dataverse-mcp` root (NOT from mcp-server):

```bash
cd wix-dataverse-mcp
mcpb pack
```

This creates a `wix-dataverse.mcpb` file.

## Step 4 — Upload to Wix workspace (Admin only)

1. Open **Claude Desktop**
2. Click your initials → **Organization settings**
3. Go to **Connectors** → **Desktop** tab
4. If the Allowlist is off, toggle it **ON**
5. Click **"Add custom extension"**
6. Select the `wix-dataverse.mcpb` file
7. It appears under **Custom team extensions**
8. Click the **...** menu → **"Add to team"**

## Step 5 — Team members install

After the admin adds it, team members:

1. Open Claude Desktop
2. Go to the extensions/connectors page
3. Find **"Wix Dataverse"** under team extensions
4. Click **Install** (one-click)
5. On first use, Claude asks for their **Client ID** and **Client Secret**
6. Done — all 6 environments available immediately

## Updating the extension

When you release a new version:

1. Bump `version` in `manifest.json`
2. Rebuild: `cd mcp-server && npm run build && cd ..`
3. Repackage: `mcpb pack`
4. Admin uploads the new `.mcpb` → replaces the old version
