---
title: "OpenClaw Setup Guide: 26 Tools + 53 Skills Explained"
tags:
  - OpenClaw
  - tools
  - skills
  - configuration
  - setup
  - AI-agent
  - self-hosted-AI
source: "[[OpenClaw Setup Guide]]"
created: 2026-04-14
---

# OpenClaw Setup Guide: 26 Tools + 53 Skills Explained

## Summary
This wiki entry is a high-fidelity reproduction of the original guide on OpenClaw setup, including exhaustive individual listings of all 26 tools and 53 official skills with their full technical details as described in the source. It incorporates the complete differentiation between tools and skills, organizational layers, configuration examples, automation strategies, getting started principles, FAQ, and appendices without summarization or categorization.

## Knowledge

### Tools vs Skills Differentiation (Full Detail)
Tools are organs that determine whether OpenClaw can do something, such as accessing files with `read` and `write`, running system commands with `exec`, searching the web with `web_search`, fetching pages with `web_fetch`, or interacting with pages (clicking buttons, filling forms, taking screenshots) with `browser`. Without a tool enabled, OpenClaw has no capability for that action, like having no hands. Skills are textbooks that teach OpenClaw how to combine tools to accomplish tasks, such as using Google Workspace for email and calendar with `gog`, organizing notes with `obsidian`, working with repos with `github`, or sending messages to channels with `slack`. The 53 official skills cover areas like notes, email, social media, development, smart home, and more. Installing a skill does not give OpenClaw new permissions—no. For example, installing the `obsidian` skill teaches note organization, but without the `write` tool enabled, it can’t write files. Skills are manuals; actual functionality requires three conditions: configuration (e.g., allowing `exec` to run commands), installation (e.g., the `gog` bridge tool on the machine), and authorization (e.g., logging into Google for access). All three are required for a skill like reading Gmail to work.

### Organizational Layers (Full Detail)
The 26 tools and 53 skills are organized in concentric circles. Layer 1 is Core Capabilities with 8 tools for file access, command execution, and web access, which almost everyone enables as they form the foundation where OpenClaw is reactive—you ask, it responds using files, commands, or searches, but without memory or proactive features. Layer 2 is Advanced Capabilities with 18 tools for browser control, memory, multi-session management, and automation, enabling OpenClaw to remember preferences, control browsers, run multiple sessions, and send scheduled notifications, transforming it from a command executor into a real assistant, though each adds attack surface. Layer 3 is the Knowledge Layer with 53 skills that teach domain-specific use of tools, such as integrating with Google, Obsidian, or Slack—install what you use.

### Full List of 26 Tools with Technical Details
Each tool is listed individually with its layer, function, risk, and expanded technical details from the source body and appendix.

1. **read** (Layer 1, Function: Read files, Risk: Low) - Provides read-only access to file contents, supporting text files and images (jpg, png, gif, webp), with images sent as attachments; output truncated to 2000 lines or 50KB, use offset/limit for large files; essential for any file-based operation in core capabilities.
2. **write** (Layer 1, Function: Write files, Risk: Medium) - Creates or overwrites files, automatically creating parent directories; fundamental for modifying or generating content in core file operations.
3. **edit** (Layer 1, Function: Structured editing, Risk: Medium) - Makes precise edits to files using exact text replacement or multiple disjoint edits; avoids overlapping changes; key for targeted modifications without full overwrites in core capabilities.
4. **apply_patch** (Layer 1, Function: Apply patches, Risk: Medium) - Applies code changes or patches to files; used for integrating updates in development or file management scenarios.
5. **exec** (Layer 1, Function: Execute commands, Risk: Very High) - Executes any shell command, such as installing packages, running scripts, or managing the system (e.g., could do `rm -rf`); without it, most tasks fail; recommend enabling approvals to show commands for confirmation before running, as it's the basic protection against misjudgments or prompt injection attacks.
6. **process** (Layer 1, Function: Manage processes, Risk: Medium) - Manages background processes, including listing tasks, checking output, and killing stuck processes; typically enabled alongside `exec` for handling long-running or parallel operations.
7. **web_search** (Layer 1, Function: Web search, Risk: Low) - Performs keyword searches on the web, returning titles, URLs, and snippets; no API key required, uses DuckDuckGo; core for internet information retrieval.
8. **web_fetch** (Layer 1, Function: Fetch web pages, Risk: Medium) - Reads and retrieves content from web pages; combines with `web_search` to enable basic internet browsing in core capabilities.
9. **browser** (Layer 2, Function: Browser control, Risk: High) - Controls Chrome for interactions like clicking buttons, filling forms, taking screenshots; v2026.3.13 added Chrome DevTools attach mode for direct connection to signed-in sessions without extensions; used for price comparisons, spec research, adding to carts, but recommend manual checkout for payments.
10. **canvas** (Layer 2, Function: Visual workspace, Risk: Low) - Provides a workspace for creating diagrams and flowcharts; part of advanced UI capabilities.
11. **image** (Layer 2, Function: Image analysis, Risk: Low) - Analyzes and "understands" images; enables processing visual content in advanced scenarios.
12. **memory_search** (Layer 2, Function: Search memory, Risk: Medium) - Searches stored information across sessions, e.g., remembering user preferences like building blogs with Astro or deploying on Azure; enables continuity and personalization.
13. **memory_get** (Layer 2, Function: Get memory, Risk: Medium) - Retrieves specific stored memories; supports long-term knowledge retention for better assistance over time.
14. **sessions_list** (Layer 2, Function: List sessions, Risk: Low) - Lists active multi-sessions for running different tasks simultaneously without interference.
15. **sessions_history** (Layer 2, Function: Session history, Risk: Medium) - Accesses histories of sessions; useful for reviewing past multi-task interactions.
16. **sessions_send** (Layer 2, Function: Send messages, Risk: High) - Sends messages between sessions; enables inter-session communication in multi-agent setups.
17. **sessions_spawn** (Layer 2, Function: Spawn sub-agents, Risk: High) - Spawns new sub-agents or sessions for parallel tasks, e.g., one for product ideas, another for research.
18. **sessions_yield** (Layer 2, Function: Wait for sub-agent results, Risk: Medium) - Pauses the main agent to wait for sub-agent completion; part of multi-agent orchestration.
19. **subagents** (Layer 2, Function: Manage sub-agents, Risk: Medium) - Manages running sub-agents, including checking status and stopping them; supports advanced orchestration.
20. **session_status** (Layer 2, Function: Check status, Risk: Low) - Checks the status of sessions; monitors ongoing multi-sessions.
21. **message** (Layer 2, Function: Cross-platform messaging, Risk: Very High) - Sends messages to Discord, Slack, Telegram, WhatsApp, iMessage; recommend using only for self-messages to avoid risks of unsent messages in wrong tone or context; used for proactive notifications like daily briefs.
22. **nodes** (Layer 2, Function: Hardware control, Risk: Very High) - Controls hardware across devices, including remote screenshots, GPS location, camera access; often disabled due to privacy concerns, as scenarios for AI-independent use are rare.
23. **cron** (Layer 2, Function: Scheduled tasks, Risk: High) - Sets up scheduled tasks for automation, e.g., daily briefs at 6:47 AM with calendar, emails, weather; combines with `message` for push notifications.
24. **gateway** (Layer 2, Function: Gateway management, Risk: High) - Manages the OpenClaw gateway, including restarts; supports self-maintenance.
25. **agents_list** (Layer 2, Function: List agents, Risk: Low) - Lists available agent IDs; supports multi-agent architectures, though not detailed in official docs for single instances.
26. **tts** (Layer 2, Function: Text-to-speech, Risk: Low) - Converts text to speech for voice messages (e.g., Telegram voice notes); supports ElevenLabs, OpenAI TTS, Edge TTS; disabled by default, enable via config or /tts command.

(Extension tools: **llm_task** (Workflow LLM step, Risk: Medium) - Inserts LLM processing into workflows. **lobster** (Workflow engine, Risk: Medium) - Defines multi-step processes for automation.)

### Full List of 53 Official Skills with Technical Details
Each skill is listed individually with its platform/function and risk as per the source appendix, expanded with descriptions from the body where available.

1. **obsidian** (Platform/Function: Obsidian, Risk: Low) - Manages local Obsidian vaults for note organization; operates on files, suitable for deployments where OpenClaw has file access; one of 4 note-taking skills, works if OpenClaw is local but not in VM without adjustments; teaches how to combine tools for note tasks.
2. **notion** (Platform/Function: Notion, Risk: Medium) - Integrates with cloud-based Notion for note management; no deployment constraints, path of least resistance for VM setups; one of 4 note-taking skills.
3. **apple-notes** (Platform/Function: Apple Notes, Risk: Low) - Accesses Apple Notes for Mac-local note handling; limited to Mac deployments, not VM-compatible; one of 4 note-taking skills.
4. **bear-notes** (Platform/Function: Bear, Risk: Low) - Integrates with Bear app for Mac-local note organization; limited to Mac, not VM; one of 4 note-taking skills.
5. **things-mac** (Platform/Function: Things 3, Risk: Low) - Manages tasks in Things 3 on Mac; part of task management skills, included if using gog for Google Tasks alternative.
6. **apple-reminders** (Platform/Function: Reminders, Risk: Low) - Handles Apple Reminders for simple task tracking; Mac-local; part of task management, alternative to trello or gog Tasks.
7. **trello** (Platform/Function: Trello, Risk: Medium) - Integrates with Trello for board-based task management; cloud-based; one of task skills, no extra install if using gog Tasks.
8. **gog** (Platform/Function: Google Workspace, Risk: Medium) - Integrates Gmail, Calendar, Tasks, Drive, Docs, Sheets via CLI; requires auth, more complete than himalaya, revocable from Google account; preferred for Google users, includes Tasks.
9. **himalaya** (Platform/Function: IMAP/SMTP, Risk: High) - Basic email sending/receiving via IMAP/SMTP; less feature-rich than gog; one of two email skills.
10. **slack** (Platform/Function: Slack, Risk: Medium) - Deep access to Slack for channel management, message history search, syncing; full platform data access beyond simple sending with message tool.
11. **discord** (Platform/Function: Discord, Risk: Medium) - Integrates with Discord for server/channel operations, message sync; full access to platform data.
12. **wacli** (Platform/Function: WhatsApp, Risk: Very High) - WhatsApp CLI for deep chat access; high risk due to personal messaging; gives full data access.
13. **imsg** (Platform/Function: iMessage, Risk: Very High) - Mac-local iMessage integration for messaging; high risk for communication; full access beyond message tool.
14. **bluebubbles** (Platform/Function: iMessage (external), Risk: High) - Non-Mac access to iMessage; alternative for broader deployments; full platform access.
15. **bird** (Platform/Function: X (Twitter), Risk: Very High) - Integrates with X/Twitter for tweets, searches; high risk for social posting; full access.
16. **github** (Platform/Function: GitHub, Risk: Medium) - Operates GitHub via gh CLI with OAuth; controllable permissions for repo management, used for checking PR builds, error logs in CI/CD monitoring.
17. **coding-agent** (Platform/Function: AI Coding, Risk: Medium) - Calls AI coders like Claude Code or Cursor in background; potential for dispatching tasks like cloning repos, studying, building demos; AI orchestrating AI, not yet installed but high potential.
18. **tmux** (Platform/Function: Terminal, Risk: Low) - Manages multiple terminal sessions; useful for dev environments; installed for terminal management.
19. **session-logs** (Platform/Function: Log Search, Risk: Low) - Searches and analyzes past conversation logs; used for reviewing history; installed for log analysis.
20. **spotify-player** (Platform/Function: Spotify, Risk: Low) - Controls Spotify playback; part of music skills, not installed if irrelevant.
21. **sonoscli** (Platform/Function: Sonos, Risk: Low) - Controls Sonos speakers for audio streaming; music playback skill.
22. **blucli** (Platform/Function: BluOS, Risk: Low) - Integrates with BluOS for multi-room audio; music skill.
23. **openhue** (Platform/Function: Philips Hue, Risk: Low) - Controls Philips Hue smart lights; smart home skill.
24. **eightctl** (Platform/Function: Eight Sleep, Risk: Low) - Controls Eight Sleep beds for tracking and adjustments; smart home skill.
25. **food-order** (Platform/Function: Multi-platform, Risk: High) - Handles food ordering across platforms; food delivery skill, irrelevant if not used.
26. **ordercli** (Platform/Function: Foodora, Risk: Medium) - CLI for Foodora delivery service; specific food skill.
27. **openai-image-gen** (Platform/Function: OpenAI Images, Risk: Low) - Generates images using OpenAI; creative AI skill.
28. **nano-banana-pro** (Platform/Function: Gemini Images, Risk: Low) - Generates images using Gemini; alternative creative image skill.
29. **video-frames** (Platform/Function: Video Frames, Risk: Low) - Extracts frames from videos for media processing; creative skill.
30. **gifgrep** (Platform/Function: GIF Search, Risk: Low) - Searches for and retrieves GIFs; creative visual content skill.
31. **sag** (Platform/Function: ElevenLabs TTS, Risk: Low) - High-quality text-to-speech using ElevenLabs; voice skill.
32. **openai-whisper** (Platform/Function: Speech-to-Text, Risk: Low) - Local transcription of audio to text; voice STT skill.
33. **openai-whisper-api** (Platform/Function: Cloud STT, Risk: Low) - Cloud-based speech-to-text via API; voice skill.
34. **sherpa-onnx-tts** (Platform/Function: Offline TTS, Risk: Low) - Local offline text-to-speech engine; voice skill.
35. **1password** (Platform/Function: 1Password, Risk: Very High) - Accesses 1Password vault for passwords, auto-login, form filling; all-or-nothing access to entire vault, recommend AI-only vault if used; not installed due to risks.
36. **gemini** (Platform/Function: Gemini, Risk: Low) - Integrates Google Gemini AI model; AI integration skill.
37. **oracle** (Platform/Function: Oracle CLI, Risk: Low) - Integrates Oracle CLI for database/AI operations; AI skill.
38. **mcporter** (Platform/Function: MCP Integration, Risk: Medium) - Integrates MCP (e.g., Cursor/Obsidian bridging); AI tool skill.
39. **clawhub** (Platform/Function: Skill Management, Risk: Low) - Manages skills from ClawHub, including installation and VirusTotal scanning since Feb 2026; system skill, whitelisted.
40. **skill-creator** (Platform/Function: Create Skills, Risk: Low) - Tools for creating custom skills; system skill, whitelisted.
41. **healthcheck** (Platform/Function: Health Check, Risk: Low) - Performs system diagnostics and health checks; system skill, whitelisted.
42. **summarize** (Platform/Function: Summarization, Risk: Low) - Generates content summaries; system skill, whitelisted for utilities like daily briefs.
43. **weather** (Platform/Function: Weather, Risk: Low) - Fetches weather forecasts and data; system skill, whitelisted for daily briefs.
44. **goplaces** (Platform/Function: Google Places, Risk: Low) - Searches locations via Google Places API; places skill.
45. **local-places** (Platform/Function: Local Proxy, Risk: Low) - Privacy-focused local proxy for place data; places skill.
46. **camsnap** (Platform/Function: RTSP Camera, Risk: Medium) - Takes snapshots from RTSP cameras; media skill.
47. **blogwatcher** (Platform/Function: RSS Monitor, Risk: Low) - Monitors RSS feeds for news aggregation; news skill, used in content research automation.
48. **nano-pdf** (Platform/Function: PDF Editing, Risk: Low) - Edits and manipulates PDF documents; docs skill.
49. **model-usage** (Platform/Function: Usage Tracking, Risk: Low) - Tracks AI model usage and costs; monitor skill.
50. **peekaboo** (Platform/Function: macOS UI, Risk: High) - Interacts with macOS UI elements; system skill.
51. **voice-call** (Platform/Function: Voice Calls, Risk: High) - Handles telephony and voice calls; comms skill, irrelevant if not used.
52. **canvas** (Platform/Function: Canvas Operations, Risk: Low) - Performs drawing and editing on canvases; creative skill.
53. **songsee** (Platform/Function: Audio Visualization, Risk: Low) - Generates visualizations for audio, like waveforms; music skill.

### Configuration and Automation Details (Full Detail)
Sample tools config allows 22 tools (read, write, edit, apply_patch, exec, process, web_search, web_fetch, browser, image, memory_search, memory_get, sessions_list, sessions_history, sessions_send, sessions_spawn, session_status, tts, message, cron, gateway, agents_list), denies 4 (nodes, canvas, llm_task, lobster), and enables approvals for exec to show commands for confirmation. Rule: if no use case, leave off. OpenClaw runs on Azure VM via Telegram, paired with Claude Code for mobile/desktop workflow. Skills config whitelists 9 bundled skills (gog for email/calendar, github for repos, tmux, session-logs, weather, summarize, clawhub, healthcheck, skill-creator) to restrict auto-loading; bundled skills auto-load if CLI installed, so use whitelist. Automation pattern is trigger (cron) + action + deliver (message), with examples: daily brief at 6:47 AM (calendar, pending emails, weather, CI/CD failures, replacing checking 5 apps); email triage twice daily (categorize inbox by urgency, archive newsletters, flag actions, reducing management from 30min to 5); CI/CD monitoring (read error logs on failures, diagnose, push Telegram message; fixed issues from phone); content research daily (collect from subreddits, Hacker News, RSS, compile digest of writing topics, surfaces ideas without writing).

### Getting Started Principles (Full Detail)
You don’t need all 26 tools; the 53 bundled skills default to all-on if CLIs installed—use allowBundled to keep only needed. Open openclaw.json and start with three principles: 1. If you can’t think of a use case, leave it off. 2. More capability, more control—enable approval for exec, only message yourself. 3. The last mile is always manual—checkout, sending messages, posting publicly, anything irreversible stays with you to avoid AI errors. Copy the sample config, trim to fit; read alongside security guide for settings and deploy cost guide for costs.

### FAQ (Full Detail)
Do Skills change OpenClaw's permissions? No. Skills are just instruction manuals. Actual capabilities are controlled by tools.allow. Can the 1password Skill read all my passwords? Yes. Once authorized, it has access to your entire vault—whatever you've stored, it can read. How do I revoke OpenClaw's Google access? Google Account → Security → Third-party apps with account access → Find gog → Remove access. Are third-party Skills on ClawHub safe? Don't assume they are. Since February 2026, ClawHub has integrated VirusTotal automatic scanning to block malicious downloads, but reviewing the GitHub repo before installing is still recommended. See our security guide for a detailed review checklist. What's the difference between OpenClaw and ChatGPT? ChatGPT is a chat tool. OpenClaw is an agent. The difference is what happens after the conversation: ChatGPT can only talk to you, while OpenClaw can act — search the web, read and write files, manage your calendar, draft email replies, and push notifications to your phone. How to automate tasks with AI using OpenClaw? Combine the cron tool (scheduling) with the message tool (push notifications). OpenClaw can run tasks on a schedule and push results to Telegram, Discord, or Slack. Common use cases: daily briefings, email triage, CI/CD monitoring, and content research. Can I use OpenClaw without coding? Day-to-day usage requires no coding — just talk to it in natural language. But installation and configuration have a learning curve. Using an AI CLI tool like Claude Code to assist with setup can save significant time.

### Tool Groups (Full Detail from Appendix)
group:fs includes read, write, edit, apply_patch. group:web includes web_search, web_fetch. group:ui includes browser, canvas. group:memory includes memory_search, memory_get. group:sessions includes sessions_list, sessions_history, sessions_send, sessions_spawn, sessions_yield, subagents, session_status. group:messaging includes message. group:nodes includes nodes. group:automation includes cron, gateway. group:agents includes agents_list. group:media includes image, tts.

## References
- [[OpenClaw Setup Guide]]
- Original URL: https://yu-wenhao.com/en/blog/openclaw-tools-skills-tutorial/
- Author: [[WenHao Yu]]
- Published: 2026-02-04
- Last Updated in Source: 2026-03-17
- Chinese Version: https://yu-wenhao.com/zh-TW/blog/openclaw-tools-skills-tutorial/
- Image: https://yu-wenhao.com/images/blog/openclaw-tools-skills-tutorial.webp
- Architecture Image: https://yu-wenhao.com/images/blog/openclaw-tools-skills-tutorial/openclaw-tools-skills-architecture.webp
- LinkedIn: https://www.linkedin.com/in/hence/
