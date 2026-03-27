---
name: autoenableai-demo-walkthrough
description: "Hackathon Demo Script: Step-by-step walkthrough for presenting the AutoEnableAI pipeline live."
---

# AutoEnableAI — Hackathon Demo Walkthrough

## Pre-Demo Setup Checklist

- [ ] Open Snowsight and navigate to **Workspaces**
- [ ] Verify `.snowflake/cortex/skills/` folder with all 7 skill files is visible
- [ ] Verify `AGENTS.md` is at workspace root
- [ ] Open **AI & ML > Snowflake Intelligence** and verify `AUTOENABLEAI_AGENT` is listed
- [ ] Have the release note text ready to paste
- [ ] Open two browser tabs: Workspaces (Cortex Code) + Snowflake Intelligence (Agent)

---

## Act 1: The Problem (30 seconds)

**Narrate:**
> "Every time Snowflake releases a new feature, our Data Platform team spends 2-4 weeks
> manually researching it, writing compliance documents, scripting RBAC roles, building
> tutorials, and doing QA. That is 6 distinct phases of work — all manual, all slow."

---

## Act 2: The Solution — Live Demo (4-5 minutes)

### Step 1: Show the Architecture (15 seconds)

In Workspaces, show: `AGENTS.md` at root + `.snowflake/cortex/skills/` folder with 8 skill files.

**Say:** "We've encoded our entire team's tribal knowledge into six AI skills, plus a
master orchestrator. The AGENTS.md file acts as the routing map — CoCo reads it first
to know which skill to activate."

### Step 2: Trigger the Pipeline (paste the release note)

In the Cortex Code chat, type:

```
$autoenableai-orchestrator

Run the full enablement pipeline on this release note:

Mar 16, 2026: Snowflake Notebooks renamed to Legacy Snowflake Notebooks

The original Snowflake Notebooks have been renamed to Legacy Notebooks. This rename
reflects Snowflake's commitment to transitioning all users to Notebooks in Workspaces,
which is the next-generation notebook experience on Snowflake.

Migration to Notebooks in Workspaces: Snowflake will migrate all users from Legacy
Notebooks to Notebooks in Workspaces over the next few quarters. Before any mandatory
migration is enforced, a Behavior Change Request (BCR) will be issued.
```

### Step 3: Watch the Pipeline Execute

As Cortex Code generates each phase, narrate:

- **Phase 01 appears:** "The AI is now researching this service, cross-referencing official docs."
- **Phase 02 appears:** "It's drafting the compliance document our security team needs to sign off."
- **Phase 03 appears:** "Now it's generating the SQL scripts — notice our naming conventions are followed exactly."
- **Phase 04 appears:** "Here's the interactive notebook our Data Scientists will use to learn the service."
- **Phase 05 appears:** "Architectural best practices and anti-patterns — automatically generated."
- **Phase 06 appears:** "Finally, a strict QA audit catches any issues before human review."

### Step 4: Show the Production Architecture (30 seconds)

Switch to the Snowflake Intelligence tab. Show the `AUTOENABLEAI_AGENT`.

**Say:** "In production, this same pipeline runs as a native Cortex Agent. It uses custom
tool UDFs to log every phase to an audit trail, generates standardized RBAC names, and
can be triggered automatically by Snowflake Tasks watching for new releases."

Run this in the Agent chat:
```
Generate the RBAC names for Cortex Search in the DEV environment
```

Then:
```
What is the pipeline status for Notebooks in Workspaces?
```

### Step 5: Show the Audit Trail (15 seconds)

Run in a SQL worksheet:
```sql
SELECT * FROM AUTOENABLEAI.PIPELINE.ENABLEMENT_LOG ORDER BY CREATED_AT DESC;
```

**Say:** "Every phase is logged with full artifact content — complete audit trail for compliance."

---

## Act 3: Impact Summary (30 seconds)

| Metric | Before | After |
|--------|--------|-------|
| Time to Enable | 2-4 weeks | < 1 hour |
| Manual Scripts | 100% handwritten | 0% manual |
| Compliance Docs | Manual copy-paste | Auto-generated |
| Quality Assurance | Ad-hoc review | Automated 6-point audit |
| Audit Trail | None | Full artifact logging |

**Close with:** "AutoEnableAI transforms our team from manual document producers into
proactive architects of an intelligent, self-updating service catalog."

---

## Backup Prompts (if you need to demo individual phases)

### Phase 01 only:
```
$autoenableai-research Analyze this Snowflake release note: [paste text]
```

### Phase 03 only:
```
$autoenableai-dev-scripts Generate DEV scripts for Cortex Search
```

### Phase 06 only:
```
$autoenableai-review Review all the generated artifacts for compliance and quality
```

---

## Snowflake Objects Created

| Object | Type | Location |
|--------|------|----------|
| AUTOENABLEAI | Database | Account level |
| PIPELINE | Schema | AUTOENABLEAI.PIPELINE |
| ENABLEMENT_LOG | Table | AUTOENABLEAI.PIPELINE |
| RELEASE_NOTES_QUEUE | Table | AUTOENABLEAI.PIPELINE |
| LOG_AND_RETURN | Procedure | AUTOENABLEAI.PIPELINE |
| GENERATE_ROLE_NAMES | UDF | AUTOENABLEAI.PIPELINE |
| GET_PIPELINE_STATUS | UDF | AUTOENABLEAI.PIPELINE |
| PROCESS_RELEASE_NOTES | Procedure | AUTOENABLEAI.PIPELINE |
| AUTO_PROCESS_RELEASES | Task | AUTOENABLEAI.PIPELINE |
| AUTOENABLEAI_AGENT | Agent | AUTOENABLEAI.PIPELINE |
| ARTIFACTS | Stage | AUTOENABLEAI.PIPELINE |
