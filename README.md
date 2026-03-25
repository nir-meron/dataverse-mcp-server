# Wix Dataverse MCP Server

Internal MCP server for the Wix Dynamics 365 team. Gives Claude Code full access to all 6 Wix Dataverse environments with **68 tools** and **18 skills**.

**Zero configuration needed.** Tenant, Client ID, and all environments are pre-configured. Just authenticate with your Azure AD credentials on first use.

---

## Setup (one-time)

### 1. Clone and build

```bash
git clone https://github.com/wix-dynamics/dataverse-mcp-server
cd dataverse-mcp-server/mcp-server
npm install
npm run build
```

### 2. Register with Claude Code

```bash
claude mcp add wix-dataverse -t stdio -- node /full/path/to/dataverse-mcp-server/mcp-server/dist/index.js
```

### 3. Install skills

```bash
xcopy /E /I skills\* "%USERPROFILE%\.claude\skills\"
```

### 4. Authenticate

Open Claude Code and say:

> "Connect to Dataverse"

Claude will call `auth_status`, see you're not authenticated, and ask for your **Client ID** and **Client Secret**. Both are required.

Your credentials are held **in memory only** for the current session — never written to disk.

---

## Authentication

Each team member authenticates using their own **Azure AD app registration** credentials. This enables per-user permission control via Azure AD app permissions.

### Get your credentials

1. Go to [Azure Portal → App Registrations](https://portal.azure.com/#view/Microsoft_AAD_RegisteredApps/ApplicationsListBlade)
2. Find or create your app registration for Dataverse access
3. Copy the **Application (client) ID** from the Overview page
4. Go to **Certificates & secrets** → **New client secret** → copy the secret **Value**

### Authenticate in Claude

```
"Connect to Dataverse with client ID <your-id> and secret <your-secret>"
```

Or just say `"Connect to Dataverse"` and Claude will ask for both values interactively.

### Alternative: pre-set via env vars

```bash
claude mcp add wix-dataverse -t stdio \
  -e D365_CLIENT_ID=your-client-id \
  -e D365_CLIENT_SECRET=your-secret \
  -- node /path/to/mcp-server/dist/index.js
```

---

## Environments

All 6 Wix environments are built in — no setup needed:

| Name | URL |
|------|-----|
| DEV  | wixdyndev.crm4.dynamics.com |
| POC  | wixdynpoc.crm4.dynamics.com |
| INT  | wixdynint.crm4.dynamics.com |
| Test | wixdyntest.crm4.dynamics.com |
| UAT  | wixdynuat.crm4.dynamics.com |
| Prod | wixdynprod.crm4.dynamics.com |

Just say `"switch to DEV"` or `"list entities in Prod"`.

---

## Tools (68)

### Authentication & Environment

| Tool | Description |
|------|-------------|
| `authenticate` | Authenticate with your Azure AD Client ID + Client Secret |
| `auth_status` | Check if authenticated and ready |
| `list_environments` | List all pre-configured environments |
| `select_environment` | Set the active environment for the session |
| `add_environment` | Add a custom environment URL at runtime |

### Metadata

| Tool | Description |
|------|-------------|
| `list_entities` | List all entities (LogicalName, DisplayName, EntitySetName, IsCustomEntity) |
| `get_entity_details` | Full entity metadata — display names, ownership, capabilities |
| `get_entity_attributes` | All fields with types, labels, constraints, and option sets |
| `get_entity_relationships` | 1:N, N:1, and N:N relationships for an entity |
| `get_entity_keys` | Alternate keys defined on an entity |
| `get_picklist_options` | Option set values for a specific field |
| `get_global_option_sets` | List all global option sets in the environment |
| `get_global_option_set_details` | Full option values for a named global option set |

### Records (CRUD)

| Tool | Description |
|------|-------------|
| `query_records` | OData query with filtering, ordering, expansion, and pagination |
| `get_record` | Fetch a single record by ID with selected fields |
| `create_record` | Create a new record in any entity |
| `update_record` | Update fields on an existing record |
| `delete_record` | Delete a record by ID |
| `execute_fetchxml` | Run a raw FetchXML query |
| `execute_action` | Call any Dataverse action or custom API by name |
| `associate_records` | Associate or disassociate records via relationships |
| `execute_batch` | Execute multiple operations in a single OData batch request |

### Users & Security

| Tool | Description |
|------|-------------|
| `get_current_user` | Get the currently authenticated user's profile |
| `find_users` | Search users by name or email |
| `get_user_roles` | List all security roles assigned to a user |
| `get_user_teams` | List teams a user belongs to |
| `get_user_queues` | List queues a user is a member of |
| `list_security_roles` | List all security roles in the environment |
| `get_role_privileges` | Get all privileges granted by a security role |
| `compare_role_privileges` | Diff the privileges of two security roles |
| `assign_security_role` | Assign a security role to a user |
| `remove_security_role` | Remove a security role from a user |

### Plugins

| Tool | Description |
|------|-------------|
| `list_plugin_assemblies` | List all registered plugin assemblies |
| `get_plugin_assembly_details` | Full details for an assembly including version and isolation mode |
| `list_plugin_types` | List plugin types (classes) within an assembly |
| `list_sdk_messages` | List SDK messages (entity events like Create, Update, etc.) |
| `list_sdk_message_filters` | List available message filters for a given SDK message |
| `list_processing_steps` | List all plugin processing steps, optionally filtered |
| `get_processing_step_details` | Full details for a processing step |
| `register_processing_step` | Register a new plugin step on an SDK message |
| `update_processing_step` | Update configuration of an existing step |
| `toggle_processing_step` | Enable or disable a plugin step |
| `delete_processing_step` | Remove a plugin step registration |
| `list_step_images` | List pre/post images for a processing step |
| `register_step_image` | Register a new entity image on a step |
| `update_step_image` | Update an existing entity image |
| `delete_step_image` | Remove an entity image from a step |
| `get_plugin_traces` | Retrieve plugin trace logs (errors, exceptions, custom traces) |
| `get_org_settings` | Get organization-level settings including trace log configuration |
| `set_plugin_trace_setting` | Enable or disable plugin trace logging |

### Custom Actions & APIs

| Tool | Description |
|------|-------------|
| `find_custom_actions` | Search for custom actions by name or entity |
| `get_custom_action_metadata` | Get input/output parameters and execution details for a custom action |
| `find_custom_apis` | Search for custom APIs by name or binding |
| `get_custom_api_metadata` | Get full metadata for a custom API including request/response parameters |

### Solutions & ALM

| Tool | Description |
|------|-------------|
| `list_solutions` | List all solutions installed in the environment |
| `get_solution_details` | Full solution info — publisher, version, managed/unmanaged |
| `get_solution_components` | List all components in a solution by type |
| `get_solution_dependencies` | Show dependency tree for a solution |
| `export_solution` | Export a solution as a zip file (managed or unmanaged) |
| `import_solution` | Import a solution zip into the active environment |
| `publish_customizations` | Publish all pending customizations |

### Web Resources

| Tool | Description |
|------|-------------|
| `list_web_resources` | List web resources with optional type/name filter |
| `get_web_resource` | Get a web resource including decoded content |
| `update_web_resource` | Update an existing web resource content |
| `create_web_resource` | Create a new web resource (JS, HTML, CSS, etc.) |

### Environment Variables

| Tool | Description |
|------|-------------|
| `list_environment_variables` | List all environment variable definitions and their current values |
| `get_environment_variable` | Get the value of a specific environment variable |
| `set_environment_variable` | Set or update an environment variable value |

### Audit & Compliance

| Tool | Description |
|------|-------------|
| `get_audit_history` | Retrieve audit log entries for a record — who changed what and when |
| `get_entity_audit_status` | Check if auditing is enabled on an entity |
| `get_field_audit_status` | Check if auditing is enabled on a specific field |
| `get_org_audit_status` | Check organization-level audit settings |

### Webhooks & Service Endpoints

| Tool | Description |
|------|-------------|
| `list_service_endpoints` | List all registered service endpoints and webhooks |
| `get_service_endpoint_details` | Full details for a service endpoint |
| `register_webhook` | Register a new HTTP webhook endpoint |
| `register_service_bus_endpoint` | Register an Azure Service Bus queue, topic, or Event Hub |
| `update_service_endpoint` | Update a service endpoint configuration |
| `delete_service_endpoint` | Remove a service endpoint registration |

### Managed Identity

| Tool | Description |
|------|-------------|
| `list_managed_identities` | List Dataverse managed identity records |
| `create_managed_identity` | Create a new managed identity record linked to a UAMI |
| `associate_assembly_managed_identity` | Link a managed identity to a plugin assembly |
| `compute_federated_credential_subject` | Compute the federated credential subject for CI/CD signing |

---

## Skills (18)

Skills are Claude Code prompt templates that guide Claude to use the right tools in the right order. Copy them to your Claude skills folder and invoke them by name.

| Skill | What it does |
|-------|-------------|
| `explore-metadata` | Deep-dive into an entity's fields, types, option sets, relationships, and keys |
| `query-records` | Translate natural language questions into OData queries with metadata-aware field selection |
| `create-record` | Create records with metadata validation — resolves option sets and lookup bindings |
| `summarize-entity` | Produce a business-readable summary of a record or set of records |
| `debug-plugin` | Investigate plugin errors via trace logs, inspect custom actions and custom APIs |
| `manage-plugins` | Browse assemblies, register/update/delete steps and images, enable/disable steps |
| `manage-webresources` | View, create, edit, and publish web resources (JS, HTML, CSS) |
| `manage-webhooks` | Register webhooks and Azure Service Bus / Event Hub integrations |
| `inspect-solutions` | Explore installed solutions, their components, publishers, and dependency trees |
| `alm-operations` | Export/import solutions, publish customizations, manage environment variables |
| `security-audit` | Investigate users, roles, teams, and privileges — compare roles side by side |
| `audit-history` | Query audit logs — who changed what, when, and what the old/new values were |
| `managed-identity` | Set up managed identities for plugin assemblies, compute federated credentials |
| `d365-research` | Research Dynamics 365 topics against Microsoft Learn documentation |
| `pac-auth` | Manage Power Platform CLI auth profiles for Dataverse environments |
| `pac-plugin` | Scaffold, build, sign, and deploy plugin projects using PAC CLI + .NET |
| `pac-pcf` | Scaffold, build, and deploy PCF (PowerApps Component Framework) controls |
| `pac-solutions` | Pack, unpack, clone, and source-control solutions using PAC CLI |

### Install skills

```bash
xcopy /E /I skills\* "%USERPROFILE%\.claude\skills\"
```

---

## Quick start examples

```
"Connect to Dataverse"                          → authenticates with your credentials
"Switch to DEV"                                 → sets active environment to DEV
"Show me all fields on the incident entity"     → explore-metadata skill
"Who changed record <id> on wix_ticket?"        → audit-history skill
"List all plugins registered on Create of wix_ticket" → manage-plugins skill
"Export the BackendProcessSolution to UAT"      → alm-operations skill
"Show me all web resources with 'ticket' in the name" → manage-webresources skill
"Does user john@wix.com have the System Admin role?" → security-audit skill
```
