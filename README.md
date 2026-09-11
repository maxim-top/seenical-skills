# Seenical Console Skill

This is a single-Skill repository for managing Seenical content workflows. Its
root `SKILL.md`, `agents/`, and `references/` use the standard Skill layout, so
compatible Agents can discover and read the Skill without understanding the
Seenical catalog format. The root `SKILL.md` explicitly routes Codex and other
Skill readers to the relevant `references/api/*.json` business contract through
standard progressive disclosure.

`.seenical/manifest.json` is Seenical-specific publication metadata.
`.seenical/runtime.json` selects the host-provided Butler API runtime and points
to `references/api/*.json`, which maps Tool IDs to relative, existing Butler
APIs. Domains, tokens, headers, tenant data, and Console configuration must
never be committed here.

## Repository layout

```text
SKILL.md
agents/openai.yaml
references/agents.md
references/plugins.md
references/knowledge-bases.md
references/content.md
references/sites.md
references/api/agents.json
references/api/plugins.json
references/api/knowledge-bases.json
references/api/content.json
references/api/sites.json
.seenical/manifest.json
.seenical/runtime.json
.github/workflows/notify.yml
```

To update the Skill:

1. Edit the root `SKILL.md` instructions or its text references.
2. Keep the manifest Tool list aligned with `references/api/*.json`. The single
   descriptor must use `"path": "."`.
3. Merge the reviewed change to `main`.

`SKILL.md` may link to text resources under `references/` and optional Agent UI
metadata at `agents/openai.yaml`. Connector validates and stores these files as
read-only Skill resources; it never executes repository content. Scripts,
executables, authentication handlers, tokens, passwords, and tenant-specific
configuration are not accepted. Public callback URLs are data passed only to
the reviewed Butler endpoint and are subject to runtime validation.

The filename `agents/openai.yaml` is the standard Skill UI metadata location
used by OpenAI clients such as Codex. It contains only display and invocation
metadata, does not restrict the Skill to one model provider, and is not read by
the Seenical Butler API runtime.

This version lets Codex discover and understand the same Tool contracts, but it
does not ship a standalone CLI or MCP server. Seenical Console supplies its
authenticated HTTP client to `butler_api/v1`. Direct execution from Codex still
requires a host-provided Butler runtime or a later MCP/OAuth adapter; API
knowledge alone does not grant authentication or execution capability.

## Update notification

Configure the repository Actions variable `NOTIFY_URL` under
**Settings > Secrets and variables > Actions > Variables** when update
notifications are needed. No GitHub secret is required. If the variable is not
configured, the notification step is skipped and the workflow succeeds. If it
is configured, the workflow sends repository and commit metadata after Skill
content is pushed to `main`; the receiver decides how to process the update.

Use **Actions > Notify Skill Update > Run workflow** to retry a failed
notification or trigger the first import. A successful repository publication
updates the shared global version. The Skill becomes usable for an App after
that App binds its Seenical IM user; there is no per-Agent approval or binding.
