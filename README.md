# MCP Registry (GitHub Copilot)

This repository hosts the **organization‑approved Model Context Protocol (MCP) registry** used by GitHub Copilot.

The registry defines **which MCP servers GitHub Copilot is allowed to use** within the organization, ensuring that Copilot's access to internal and third‑party systems is **explicitly approved, centrally controlled, and auditable**.

---

## Why this repository exists

GitHub Copilot supports MCP servers that allow AI tooling to access external systems such as documentation platforms, APIs, and internal services.

While powerful, unrestricted MCP usage introduces **data‑leakage and compliance risks**.  
This repository exists to:

- ✅ Enforce a **central allow‑list** of MCP servers
- ✅ Prevent users from configuring arbitrary or untrusted MCP servers
- ✅ Ensure Copilot can only use security‑reviewed and approved integrations
- ✅ Provide a clear, version‑controlled audit trail of allowed integrations

This registry is referenced from **GitHub Organization → Copilot settings**, where MCP access is restricted to the servers defined here.

---

## How it works

The registry exposes a single endpoint that GitHub Copilot queries to enforce the allow-list:

| Endpoint | Source file |
|---|---|
| `GET /v0.1/servers` | `v0.1/servers-list.json` |

URL rewriting (configured in `staticwebapp.config.json` or `_redirects`) maps the API path to the static JSON file.

---

## Hosting and deployment

The registry must be served over HTTPS with CORS headers. Two hosting configurations are included:

- **Azure Static Web Apps** — uses `staticwebapp.config.json` (recommended for Microsoft/Azure environments)
- **Netlify** — uses `_redirects`

Configure the GitHub Copilot organization policy to point to the base URL of the deployed site (e.g. `https://your-registry.azurestaticapps.net`). Do **not** include the `/v0.1/servers` path suffix — Copilot appends this automatically.

---

## Approved MCP servers

The current registry intentionally contains a **minimal set** of MCP servers.

### atlassian-confluence

- **Title:** Atlassian Confluence MCP
- **URL:** `https://mcp.atlassian.com`
- **Transport:** Streamable HTTP
- **Auth:** OAuth 2.0 (Bearer token — users authenticate via Atlassian OAuth)
- **Purpose:** Read and write access to Confluence spaces, pages, and content

---

## File structure

```
v0.1/
  servers-list.json          ← GET /v0.1/servers
staticwebapp.config.json     ← Azure SWA routing + CORS headers
_redirects                   ← Netlify routing rules
```

---

## Security model

This repository **does not grant access** to any system.

It only defines **which MCP servers Copilot is allowed to use**.

Security boundaries are enforced by:
- GitHub Copilot organization policy
- MCP protocol enforcement in Copilot clients
- OAuth and permission models of downstream systems (e.g. Confluence)

Making this repository public is intentional and safe:
- The contents are non‑sensitive
- Transparency improves security reviews and audits
- No credentials or internal data are exposed

---

## Governance and change management

Changes to the registry must follow the organization's security review process.

Recommended practices:
- Pull requests required
- Review by Security / Platform / Developer Enablement
- Explicit approval for any new MCP server
- Clear commit history explaining *why* a server was added or removed

---

## Adding a new MCP server

1. Add the server entry to `v0.1/servers-list.json` under the `servers` array and increment `metadata.count`
2. Open a pull request including:
   - Purpose of the MCP server
   - Data accessed
   - Authentication method
   - Security and compliance review reference
5. Obtain required approvals and merge

---

## What is intentionally *not* allowed

- User‑defined MCP servers
- Local `stdio` MCP servers
- Arbitrary third‑party MCP servers
- Filesystem or unrestricted data access MCP servers

These are blocked by design to protect company data.

---

## Questions or access requests

For questions, or to request a new MCP server to be added, contact:

- **Platform Engineering**
- **Security**
- **Developer Experience**

---

## References

- [Configure MCP server access — GitHub Docs](https://docs.github.com/en/copilot/how-tos/administer-copilot/manage-mcp-usage/configure-mcp-server-access)
- [Configure MCP registry — GitHub Docs](https://docs.github.com/en/copilot/how-tos/administer-copilot/manage-mcp-usage/configure-mcp-registry)
- [MCP Registry specification — modelcontextprotocol.io](https://registry.modelcontextprotocol.io/docs)
