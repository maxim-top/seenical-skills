# Seenical Console Skill

This is a single-Skill repository for managing Seenical content workflows. Its
root `SKILL.md`, `agents/`, and `references/` use the standard Skill layout, so
compatible Agents can discover and read the Skill without understanding the
Seenical catalog format.

`.seenical/manifest.json` is Seenical-specific publication metadata. It maps
the root Skill to platform-owned Tool IDs and permission scopes. Tenant
installation, approval, Agent bindings, tokens, and Console configuration are
stored by Seenical and must never be committed here.

## Repository layout

```text
SKILL.md
agents/openai.yaml
references/tool-api.md
.seenical/manifest.json
.github/workflows/notify-connector.yml
```

To update the Skill:

1. Edit the root `SKILL.md` instructions or its text references.
2. Keep `.seenical/manifest.json` aligned with required Seenical Tool IDs and
   permission scopes. The single descriptor must use `"path": "."`.
3. Merge the reviewed change to `main`.

`SKILL.md` may link to text resources under `references/` and optional Agent UI
metadata at `agents/openai.yaml`. Connector validates and stores these files as
read-only Skill resources; it never executes repository content. Scripts,
executables, callback URLs, authentication handlers, tokens, passwords, and
tenant-specific configuration are not accepted.

This version defines the Tool contracts but does not ship a standalone CLI or
MCP server. A compatible runtime must expose the declared Seenical Tools and
provide authentication. Direct execution from Codex can be added later without
changing the Skill instructions or Tool IDs.

## Connector notification

Edit `CONNECTOR_NOTIFY_URL` in
`.github/workflows/notify-connector.yml` before publishing this template.
No GitHub secret is required. The workflow notifies Connector after Skill
content is pushed to `main`; Connector ignores repository data in the
notification and always pulls its server-configured public repository.

Use **Actions > Notify Seenical Connector > Run workflow** to retry a failed
notification or trigger the first import. A successful repository publication
only updates the shared catalog. Every App must still approve a Skill version
and bind it to an Agent separately.
