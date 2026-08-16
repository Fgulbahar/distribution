# PostHog

Access is via the `posthog-cli` binary, not an MCP connector. Works identically for any agent.

## Projects

Two projects, both with **write access**. Pick deliberately.

| Project | How to reach it | Use for |
|---|---|---|
| **Marketing** (`422846`) | default — no flags | traffic, sources, UTMs, campaigns, landing pages |
| **Application** | `--dotenv-file .env.app` | signups, activation, retention, feature flags, errors |

```bash
posthog-cli api call query-trends '<json>'                          # Marketing
posthog-cli --dotenv-file .env.app api call query-retention '<json>' # Application
```

Marketing is the default because `posthog-cli login` saved a Marketing-scoped token to `~/.posthog/credentials.json`. That token **cannot** reach the Application project — only `.env.app` can.

## Using it

Run `posthog-cli api --agent-help` first; it is the canonical guide and is meant to be followed. The loop is:

```bash
posthog-cli api search <regex>     # find a tool
posthog-cli api info <tool>        # load its schema once, then reuse
posthog-cli api call <tool> '<json>'
```

Do not guess a schema, and do not re-run `info` before every call.

## Safety

- `--dry-run` validates input against the schema without executing. Use it before any mutation.
- Destructive tools refuse to run without `--confirm`. Add it only after verifying the exact target IDs.
- Never print or commit the contents of `.env.app`.

## Gotchas

- **A wrong `POSTHOG_CLI_PROJECT_ID` fails silently** — it 404s, then falls back to Marketing. If a result says `Marketing` when you targeted the app, the config didn't take; it is not a working query.
- Use `--dotenv-file`, never `--env-file`. The npm package wraps the Rust binary in node, and node intercepts its own `--env-file` flag.
- The two projects do not share person data. Cross-project questions need one query per project, joined on UTMs.
