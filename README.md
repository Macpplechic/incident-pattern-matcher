# incident-pattern-matcher

> A Claude Code skill for SREs and engineering teams to identify recurring failure patterns across post-mortems and incident logs.

[![Install Skill](https://img.shields.io/badge/Claude%20Skill-Install-orange)](https://github.com/posit-dev/incident-pattern-matcher)
[![Posit Plugin Marketplace](https://img.shields.io/badge/Posit-Plugin%20Marketplace-blue)](https://github.com/posit-dev/claude-plugins)

---

## What it does

The **Incident Pattern Matcher** skill lets you query across all your historical post-mortems and incident logs to surface recurring root causes, reliability trends, and engineering focus areas — without manually trawling through Confluence pages and Slack threads.

It connects live to:
- 📄 **Confluence** — post-mortem pages across PTD, HOS, and Platform spaces
- 💬 **Slack** — `#hosted-incidents` and per-incident channels (e.g. created by Datadog)
- 🎫 **Jira** — incident tickets for cross-referencing

---

## Trigger phrases

Use these in Claude to activate the skill:

```
Have we seen a failure like this before?
Generate a summary of our reliability trends this quarter.
Which service has the most recurring incidents?
Where should we focus engineering mop-up time?
What are the top root causes across all post-mortems?
```

---

## What Claude returns

Every analysis is structured as:

| Section | Description |
|---|---|
| **Pattern Summary** | Concise headline of the finding |
| **Recurring Patterns** | Grouped failure modes with frequency |
| **Root Cause Analysis** | Why these patterns keep recurring |
| **Recommended Focus Areas** | Where to invest engineering time |
| **Trend Outlook** | Improving, worsening, or new pattern? |

---

## Installation

### 1. Add to your Claude project

In your Claude project settings, add this skill from the [Posit Plugin Marketplace](https://github.com/posit-dev/claude-plugins).

### 2. Required MCP integrations

This skill requires the following MCP servers to be connected in your Claude Enterprise workspace:

| Integration | Required | Purpose |
|---|---|---|
| Atlassian | ✅ Yes | Search Confluence post-mortems + Jira tickets |
| Slack | ✅ Yes | Read `#hosted-incidents` and incident channels |

Ensure these are enabled under **Settings → Integrations** in Claude.

### 3. Claude Code (optional)

To use this skill via Claude Code, add the following to your `.claude/skills.json`:

```json
{
  "skills": [
    {
      "name": "incident-pattern-matcher",
      "source": "github:posit-dev/incident-pattern-matcher"
    }
  ]
}
```

---

## Skill rules

The core reasoning instructions are defined in [`skill.md`](./skill.md). Key behaviors:

- Always searches Confluence **and** Slack before synthesizing a response
- Groups incidents by failure signature, not just service name
- References real incident IDs and post-mortem titles in output
- Never hallucinates patterns — only reports what is found in sources
- Flags when a pattern appears in only one incident vs. multiple

---

## Example output

**Query:** *"Have we seen a failure like this before?"* (after describing a Connect Cloud OOM crash)

> **Pattern Summary:** Recurring OOM and capacity failures in Connect Cloud — 3 confirmed incidents since Sep 2025.
>
> **Recurring Patterns:**
> - Connect Cloud capacity shortage (Incidents 114, 117, 128) — autoscaler misbehavior + ephemeral storage scheduling
> - Ceph storage degradation (Incidents 99, 107) — burst credit exhaustion on aging OSD hosts
>
> **Root Cause Analysis:** Autoscaler configuration does not account for ephemeral storage constraints when scheduling workloads onto new node pools...
>
> **Recommended Focus Areas:** Autoscaler tuning + Ceph host refresh cycle
>
> **Trend Outlook:** ⚠️ Connect Cloud capacity incidents are increasing in frequency (Q4 2025 → Q1 2026)

---

## Related skills

- [doc-reviewer](https://github.com/posit-dev/doc-reviewer) — Documentation review skill
- [Posit Plugin Marketplace](https://github.com/posit-dev/claude-plugins)

---

## Contributing

Please open a PR or issue on this repository. Follow the conventions in the [Posit Plugin Marketplace contributing guide](https://github.com/posit-dev/claude-plugins/blob/main/CONTRIBUTING.md).
