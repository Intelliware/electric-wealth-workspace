---
name: detect-knowledge-base-drift
description: "Use when source repos may have changed without the KB being updated, when running a PR-gate freshness check, or for the scheduled (cron / weekly) drift sweep. Performs cheap-to-expensive layered detection (commit-stamp / structural diff / LLM contradiction / coverage sweep) and reports drift with per-component severity and recommended actions."
---

<!-- aide:generated-shim -->

# detect-knowledge-base-drift (discovery shim)

> This is a generated discovery shim. The canonical skill content lives at:
>
> **AIDE/.agents/skills/knowledge-base/detect-knowledge-base-drift/SKILL.md**
>
> Load that file for the full SKILL.md (Overview, When to use, Workflow, Output, etc.).
