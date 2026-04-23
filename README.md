# MCP Registry (GitHub Copilot)

This repository hosts the **organization‑approved Model Context Protocol (MCP) registry** used by GitHub Copilot.

The registry defines **which MCP servers GitHub Copilot is allowed to use** within the organization, ensuring that Copilot’s access to internal and third‑party systems is **explicitly approved, centrally controlled, and auditable**.

---

## Why this repository exists

GitHub Copilot supports MCP servers that allow AI tooling to access external systems such as documentation platforms, APIs, and internal services.

While powerful, unrestricted MCP usage introduces **data‑leakage and compliance risks**.  
This repository exists to:

- ✅ Enforce a **central allow‑list** of MCP servers
- ✅ Prevent users from configuring arbitrary or untrusted MCP servers
- ✅ Ensure Copilot can only security‑reviewed and approved
- ✅ Provide a clear, version‑controlled audit trail of allowed integrations

This registry is referenced from **GitHub Organization → Copilot settings**, where MCP access is restricted to the servers defined here.

---

## How it works

- `registry.json` contains the list of **approved MCP servers**
- GitHub Copilot is configured to:
  - Use this registry URL
  - Reject any MCP servers not listed here
- All Copilot clients (CLI, VS Code, Coding Agent, etc.) inherit this policy automatically

No secrets, credentials, or access tokens are stored in this repository.

---

## Approved MCP servers

The current registry intentionally contains a **minimal set** of MCP servers.

Typical approved servers include:
- Official vendor‑managed MCP servers (e.g. Atlassian Confluence MCP)
- Internally hosted MCP servers that have passed security review

Each entry specifies:
- Server identity and description
- Connection type (HTTP)
- Authentication method (OAuth where possible)

Actual authorization and access control are enforced **by the target system itself** (for example, Confluence permissions).

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

Changes to `registry.json` must follow the organization’s security review process.

Recommended practices:
- Pull requests required
- Review by Security / Platform / Developer Enablement
- Explicit approval for any new MCP server
- Clear commit history explaining *why* a server was added or removed

---

## Adding or updating an MCP server

1. Open a pull request modifying `registry.json`
2. Include:
   - Purpose of the MCP server
   - Data accessed
   - Authentication method
   - Security and compliance review reference
3. Obtain required approvals
4. Merge to make the server available to Copilot users

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

- GitHub Copilot MCP documentation  
- Model Context Protocol (MCP) specification  
- Internal security and data‑handling policies