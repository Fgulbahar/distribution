# Distribution Workspace

This workspace contains tools, apps, and scripts for distribution and GTM work.

## Default Behavior

- Work from the current request; do not assume a recurring workflow.
- Inspect only the files, tools, and notes relevant to the task.
- Do not read the entire `notes/` vault by default.
- Do not create session logs, summaries, reports, or notes unless explicitly requested.
- Prefer an existing local app or script when it already handles the task.
- Before using a local tool, read only its relevant README or help output.
- External actions such as sending messages, publishing, or purchasing must be within the current request. Ask only if the scope or cost materially expands.
- Use the GitHub CLI (`gh`) for GitHub operations. Respect the active account and existing global Git and `gh` configuration; do not modify global configuration unless explicitly requested.

## Codex-Only Notes

- For commands that depend on host credentials, Keychain access, or global configuration, operate in the local host environment rather than the sandbox, requesting approval when required.

## Available Capabilities

- `notes/` — Obsidian vault. Search or read relevant notes when needed; write to it only when requested.
- Apify CLI (`apify`) — web scraping, data collection, and browser automation.
- Whisper — local audio and video transcription.
- FFmpeg (`ffmpeg`) — media inspection, conversion, and audio extraction.
- Tempo/x402/MPP — discover and purchase paid API services when the task calls for them, subject to configured spending limits.
- GitHub CLI (`gh`) — repositories, issues, pull requests, and other GitHub operations using the active global configuration.
- `apps/` — reusable GTM applications.
- `scripts/` — smaller reusable commands and automations.

## Local Tools

When a task might match an existing capability, check `apps/` and `scripts/` before building something new.

Each reusable tool should contain a short README explaining:

- What it does
- Expected input
- Exact command or entry point
- Output format and location
- Required credentials or paid services

When a new reusable tool is added, add one short line under **Available Capabilities**. Do not create routine notes about its use.
