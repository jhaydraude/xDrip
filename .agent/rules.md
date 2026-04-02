# Antigravity Agent Rules

## 1. Read Before You Code
At the start of every session, you MUST review the North Star Plan at `docs/implementation_plan.md` and the architecture overview at `docs/architecture/001-overview.md`. Ensure you understand the current state, active phase, and overall goals before proposing any changes. 

## 2. Phase Discipline
Adhere strictly to the phases outlined in the North Star Plan. *DO NOT* write code meant for future phases without the explicit permission of the human user. Maintain extreme focus on the active phase to prevent scope creep.

## 3. Architecture Consistency
- **Language**: Java natively. Do not introduce Kotlin unless the workspace is heavily updated, as xDrip+ is predominantly legacy Java. 
- **Async Patterns**: Use Android's `AlarmManager` for precise time-based scheduling instead of basic `Handlers` when dealing with Doze. 
- **IPC**: Use `GoogleApiClient` (or standard `MessageClient`/`NodeClient`) for all Phone-to-Watch communication.
- **Tools**: No generic `cat`, `grep`, or `sed` commands when dedicated tools (`read_file`, `grep_search`, `multi_replace_file_content`) exist.

## 4. When to Stop and Ask
You must halt autonomous execution and ask for human judgement when:
- **Core Modding**: You need to modify core Dexcom authentication sequences (`Ob1G5StateMachine.java`) or change fundamental data sync architectures.
- **Schema Changes**: You are altering database schemas (`ActiveModels` / SugarORM) which could induce data loss.
- **Failures**: Build environment issues persist (e.g., AGP Gradle Plugin crashes or JDK version mismatch errors during `./gradlew`).
Consult `docs/references.md` and `.agent/toolbox.md` prior to asking if unsure.
