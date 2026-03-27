# AGENTS.md — AutoEnableAI Cortex Code Configuration

## Project Overview

AutoEnableAI is a 6-phase automated pipeline that transforms Snowflake release notes into complete enterprise enablement packages. All skills follow strict naming conventions and produce structured Markdown output.

## Default Behavior

**IMPORTANT**: When a user pastes a Snowflake release note, feature description, or asks to "enable" / "prepare" / "onboard" any Snowflake service, ALWAYS use the `autoenableai-orchestrator` skill. This is the default entry point. Do NOT ask which phase to run — run ALL 6 phases automatically.

## Global Rules

- All RBAC objects MUST follow naming: `{ENV}_{SERVICE}_ADMIN_AR`, `{ENV}_{SERVICE}_READ_AR`, `{ENV}_{SERVICE}_WH`, `{ENV}_{SERVICE}_DB`
- NEVER use ACCOUNTADMIN for day-to-day operations in generated scripts
- All SQL scripts MUST include `IF NOT EXISTS` clauses and `COMMENT` metadata
- All generated artifacts MUST use Markdown formatting with clear section headers
- Pipeline phases MUST execute in order: 01 → 02 → 03 → 04 → 05 → 06
- Each phase's output feeds as context into the next phase
- After all phases, log results using `CALL AUTOENABLEAI.PIPELINE.LOG_AND_RETURN()`
- After logging, export using `CALL AUTOENABLEAI.PIPELINE.EXPORT_ALL_PHASES_TO_DOCX()`

## Available Skills

### autoenableai-orchestrator (DEFAULT — use this for any enablement request)
Master pipeline controller that runs ALL 6 phases in a single response. Contains all phase instructions inline — no need to reference other skill files.
Trigger: Any release note pasted, "run the pipeline", "orchestrate", "enable", "enablement", "prepare", "onboard", or any new Snowflake service/feature discussion.
Location: `.snowflake/cortex/skills/00-master-orchestrator/SKILL.md`

### autoenableai-research
Phase 01 — Analyzes Snowflake release notes and extracts structured technical intelligence for enterprise onboarding.
Trigger: "research", "analyze", or "summarize" a new Snowflake service or feature.
Location: `.snowflake/cortex/skills/01-research/SKILL.md`

### autoenableai-sanction-doc
Phase 02 — Generates formal compliance and risk assessment documents with security, privacy, and approval workflows.
Trigger: "sanction", "approve", "assess risk", or "compliance document" for a Snowflake service.
Location: `.snowflake/cortex/skills/02-sanction-doc/SKILL.md`

### autoenableai-dev-scripts
Phase 03 — Produces production-ready SQL setup scripts with RBAC roles, grants, warehouses, and rollback scripts.
Trigger: "generate scripts", "enable the service", "set up roles", or "RBAC setup".
Location: `.snowflake/cortex/skills/03-dev-scripts/SKILL.md`

### autoenableai-notebooks
Phase 04 — Creates interactive learning notebooks for Data Scientists to get hands-on with newly sanctioned services.
Trigger: "create a notebook", "build a tutorial", or "scaffold a learning environment".
Location: `.snowflake/cortex/skills/04-notebooks/SKILL.md`

### autoenableai-design-pattern
Phase 05 — Produces architectural best-practice guides, anti-patterns, and cost optimization strategies.
Trigger: "best practices", "design patterns", "architecture guide", or "anti-patterns".
Location: `.snowflake/cortex/skills/05-design-pattern/SKILL.md`

### autoenableai-review
Phase 06 — Performs strict QA audit of all generated pipeline artifacts with SQL compile checks.
Trigger: "review", "audit", "validate", or "quality check" generated content.
Location: `.snowflake/cortex/skills/06-review/SKILL.md`

### autoenableai-demo-walkthrough
Hackathon demo script with exact prompts and narrative beats for the live presentation.
Trigger: "demo", "walkthrough", "presentation flow", or "how to present".
Location: `.snowflake/cortex/skills/demo-walkthrough/SKILL.md`

## Snowflake Objects

All pipeline infrastructure lives in `AUTOENABLEAI.PIPELINE`:
- **Agent**: `AUTOENABLEAI_AGENT` (Snowflake Intelligence)
- **Tables**: `ENABLEMENT_LOG` (audit trail), `RELEASE_NOTES_QUEUE` (auto-trigger queue)
- **Procedures**: `LOG_AND_RETURN` (audit logging), `PROCESS_RELEASE_NOTES` (auto-processing)
- **Functions**: `GENERATE_ROLE_NAMES` (RBAC naming), `GET_PIPELINE_STATUS` (status check)
- **Task**: `AUTO_PROCESS_RELEASES` (hourly cron trigger)
