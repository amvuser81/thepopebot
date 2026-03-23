# config/ — Configuration Files

## Directory Structure

```
config/
├── agent-chat/
│   └── SYSTEM.md              # Agent chat system prompt (supports {{skills}}, {{datetime}})
├── code-chat/
│   └── SYSTEM.md              # Code workspace system prompt
├── agent-job/
│   ├── SOUL.md                # Agent personality/identity (used by Docker agent)
│   ├── AGENT_JOB.md           # Agent runtime environment docs (used by Docker agent)
│   └── SUMMARY.md             # Prompt for summarizing completed jobs
├── cluster/
│   ├── SYSTEM.md              # Cluster worker system prompt
│   └── ROLE.md                # Per-role prompt template for cluster workers
├── HEARTBEAT.md               # Self-monitoring behavior (cron task prompt)
├── CRONS.json                 # Scheduled job definitions
└── TRIGGERS.json              # Webhook trigger definitions
```

## Markdown File Includes

Markdown files in `config/` support includes and built-in variables, powered by `lib/utils/render-md.js`.

- **File includes**: `{{ filepath.md }}` — resolves relative to project root, recursive with circular detection. Missing files are left as-is.
- **`{{datetime}}`** — Current ISO timestamp.
- **`{{skills}}`** — Dynamic bullet list of active skill descriptions from `skills/active/*/SKILL.md` frontmatter. Never hardcode skill names — this is resolved at runtime.

## Next.js Config Wrapper (index.js)

`withThepopebot()` wraps user's `next.config.mjs`. Adds `transpilePackages` and `serverExternalPackages` for the npm package's dependencies that need special bundling.

## Instrumentation (instrumentation.js)

Server startup hook loaded by Next.js. Sequence: loads `.env`, initializes database, starts cron scheduler, starts cluster runtime. Skipped during `next build` (checks `NEXT_PHASE`).
