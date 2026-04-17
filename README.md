# Compounding Knowledge with Obsidian as a Second Brain and MCP server

## Overview
Obsidian serves as our shared "second brain" – a digital vault for capturing, organizing, and linking knowledge. By integrating it with my (Ada's) capabilities, we can compound your insights over time: turning raw chats, ideas, and data into interconnected notes that grow more valuable. This creates exponential returns on your thinking, like interest on knowledge.

The same vault is also wired for **Cursor**: an MCP server can expose the vault to the editor's AI as tools (search, read, create, or patch notes), so coding sessions and the wiki stay in sync.

## Cursor MCP (this vault as a server)

**Idea:** Cursor runs an MCP client; a small **Obsidian MCP server** process talks to Obsidian over HTTP using the community plugin **Local REST API** (`obsidian-local-rest-api`), which is enabled for this vault under `.obsidian/community-plugins.json`.

**What you need running**

1. **Obsidian** with this folder opened as the vault (`rivers_edge/` at the repo root).
2. **Local REST API** enabled in Obsidian -> Settings -> Community plugins, with HTTPS (and optionally HTTP) listening on the ports shown in the plugin's settings (defaults are often **27124** secure / **27123** insecure; confirm in your UI).
3. **Cursor** with your chosen Obsidian MCP package configured under **Settings -> MCP** (or your user `mcp.json`), pointing at that base URL and sending the **API key** from the same plugin settings.

**Security**

- Treat the plugin **API key like a password**. Prefer HTTPS to localhost. Do not paste keys into this README or commit them; keep them only in Cursor's MCP config (or a secrets manager) and rotate if leaked.

**How this differs from OpenClaw file tools**

- **MCP + Local REST API:** best when you want Cursor's built-in agent to call Obsidian-shaped tools while Obsidian is running.
- **OpenClaw `read` / `write` / skills:** operate on markdown files on disk directly; Obsidian does not need to be open, but you lose live plugin/API behavior.

## Key Principles
- **Capture Everything**: Summarize chats, lessons, or research into MD files. No idea is too small – link it to build context.
- **Atomic Notes**: Keep notes focused (one idea per note) for easy linking/reuse.
- **Linking & Graph View**: Use [[Wiki-style links]] to connect concepts. Obsidian's graph visualizes how knowledge compounds.
- **Daily/Weekly Reviews**: Pull from my MEMORY.md/dailies to distill into Obsidian for long-term access.

## Workflow
1. **From Chat to Note**: When you say "summarize this to Obsidian," I'll create/edit a file (e.g., in Thinking/) with key takeaways.
2. **Compounding Mechanism**: 
   - Reference past notes in responses for context.
   - Suggest links: "This ties to [[2026-04-06 Power Outage Lessons]]."
   - Periodic reviews: During heartbeats, scan recent dailies and propose Obsidian updates.
3. **Folders Structure**:
   - **Personal**: Life stuff, goals, reflections.
   - **Thinking**: Ideas, brainstorms like this note.
4. **Agent Integration**: I can read/write via tools – e.g., append to this file with new insights. In Cursor, the MCP layer can do the same while Obsidian is the live front-end to the vault.

## Benefits
- **Your Brain + Mine**: You generate ideas; I organize and recall them instantly.
- **Compounding Effect**: Notes link and build – a simple chat today becomes part of a knowledge web tomorrow.
- **Accessibility**: Searchable, offline, and synced (if set up).

Let's start small: What chat or idea to capture first?

Last updated: 2026-04-17
