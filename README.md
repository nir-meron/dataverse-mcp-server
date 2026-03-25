# Wix Dataverse MCP Server

Internal MCP server for the Wix Dynamics 365 team. Gives Claude Code full access to all 6 Wix Dataverse environments with 70 tools and 18 skills.

**Zero configuration needed.** Tenant, Client ID, and all environments are pre-configured. Just authenticate with your client secret on first use.

## Setup (one-time)

### 1. Clone and build

```bash
git clone <repo-url>
cd wix-dataverse-mcp/mcp-server
npm install
npm run build
```

### 2. Register with Claude Code

```bash
claude mcp add wix-dataverse -t stdio -- node /full/path/to/wix-dataverse-mcp/mcp-server/dist/index.js
```

That's it. No secrets in config files, no env vars.

### 3. Authenticate

Open Claude Code and say:

> "Connect to Dataverse"

Claude will call `auth_status`, see you're not authenticated, and ask for your client secret. Paste it, and you're in.

Your secret is held in memory only for the current session — it's never written to disk.

## Get your client secret

1. Go to [Azure Portal → App Registrations](https://portal.azure.com/#view/Microsoft_AAD_RegisteredApps/ApplicationsListBlade)
2. Find the shared Dataverse app
3. Go to **Certificates & secrets** → **New client secret**
4. Copy the secret value

## Environments

All 6 environments are built in:

| Name | URL |
|------|-----|
| DEV | wixdyndev.crm4.dynamics.com |
| POC | wixdynpoc.crm4.dynamics.com |
| INT | wixdynint.crm4.dynamics.com |
| Test | wixdyntest.crm4.dynamics.com |
| UAT | wixdynuat.crm4.dynamics.com |
| Prod | wixdynprod.crm4.dynamics.com |

Just say "switch to DEV" or "list entities in Prod".

## Alternative: pre-set secret via env var

If you prefer not to authenticate each session, set the env var when registering:

```bash
claude mcp add wix-dataverse -t stdio -e D365_CLIENT_SECRET=YOUR_SECRET -- node /path/to/mcp-server/dist/index.js
```

## Skills

Copy to your Claude skills folder for best-practice guides:

```bash
xcopy /E /I skills\* "%USERPROFILE%\.claude\skills\"
```
