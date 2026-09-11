---
name: seenical-console
description: Manage Seenical Agents, content generation plans, sites, previews, and publishing when a user asks to inspect or change Seenical Console through conversation.
---

# Seenical Console

Use the Seenical tools exposed by the current runtime. Read
[references/tool-api.md](references/tool-api.md) before selecting a tool or
constructing arguments.

## Workflow

1. Use a read tool to resolve the intended target before changing it.
2. For a change, send only fields the user requested and preserve omitted
   values.
3. Show the runtime-provided preview or field-level difference before asking
   for approval.
4. Do not treat creation as immediate generation. Run content only when the
   user explicitly requests it.
5. Never resume a paused plan merely because its prompt, language, or other
   configuration changed.
6. Treat generation, preview creation, publication, and preview discard as
   separate operations with their declared confirmation level.
7. If current state changed before execution, read it again and propose a new
   change; never silently overwrite it.

## Authorization boundaries

The runtime supplies authentication and enforces App, IM user, Agent,
conversation, original-message, and client capability authorization. Do not request, display, persist, or pass
access tokens, Git credentials, model keys, passwords, or billing credentials
as tool arguments.

Do not use this Skill to change members, roles, account security, billing, or
credentials. If an expected Seenical tool is unavailable, explain that the
current runtime has not exposed or authorized it; do not substitute an
unapproved HTTP endpoint.
