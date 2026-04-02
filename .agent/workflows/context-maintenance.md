---
description: Context Maintenance Workflow
---

# Context Maintenance Workflow (SOP)

This workflow must be executed at the start of every session or when a phase completes.

## Steps
1. **Read Rules**: Read the file at `.agent/rules.md` to refresh memory on constraints, behavior, and stack guidelines.
2. **Check North Star Plan**: View `docs/implementation_plan.md` to identify the current project version and the presently Active Phase. Ensure your current goals strictly align.
3. **Review ADRs**: Scan the `docs/architecture/` folder for any newly added Architecture Decision Records (ADRs) to understand recent technical shifts.
4. **Log State**: Output a brief "Context Sync Complete" message to the user summarizing the current phase and verifying that all rules have been loaded into memory.
