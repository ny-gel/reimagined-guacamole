### Steering Codex with `agents.md`
- For instructions that should apply across every session, `AGENTS.md` is the better approach --> gives persistent instructions for the project
- Codex will read this file to understand how we want it to work in the codebase
  - Contains (1) how we want Codex to communicate, code style preferences, project-specific rules, commands Codex should know, including how to run tests
  - `AGENTS.md` can sit in a few places:
    - `~/.codex/AGENTS.md` for personal preferences that apply across all projects
    - `./AGENTS.md` for instructions scoped to a specific project
    - `/init` can help create a starter `./AGENTS.md` file directly inside the session.
   
### Slash commands
- `/diff` shows Git-style diff of what changed in the working directory
- `/clear` clears the current conversation context, so we can start fresh when a session is no longer useful
- `/model` changes which model Codex is using this session
- `/review` analyzes changes in working tree & provides feedback --> operates on Git-enabled workspace

### SDD with Codex
- The **interview technique**: "Ask me questions about the utility we are building. After I answer, turn my answers into a `spec.md` checklist."
  - ```I want to build a game. Before writing the spec, ask me a few questions about how I want it structured. Then create a SPEC.md with exactly four steps covering the MVP... Each step should have a single checkbox item.```

  - Can also be captured as a workflow in `AGENTS.md`, where `spec.md` tells Codex what we are building; and `AGENTS.md` tells Codex how to move through the work

  - ```py
    ###Next Task Workflow
    When I ask you to do the next task:
    1. Read `spec.md`.
    2. Find the first unchecked checklist item.
    3. Implement only that item.
    4. Update `spec.md` by marking that item complete.
    5. Summarize what changed.
    6. Wait for review before continuing.
    ```
- ### TDD with Codex
- This is a repeating cycle, since Codex is probabilistic - the same prompt can produce different implementations, and some will miss requirements or break existing behavior.
- Tests provide a predictable target: When Codex's output fails, it surfaces immediately rather than leaving it to manual code review.
- Do not implement the functions yet -- create tests first, run them, and confirm they fail (because implementation does not exist)
- "node --test textStats.test.js" with Node's built-in test runner 

### Context management with Codex
- For session-level context, we can manage what Codex carries forward:
  - Use `/clear` to stop carrying the old conversation forward
  - Use `/compact` to keep a shorter version of useful history
  - Use `/status` to inspect how much context the session is using
  - Use `@file` references to bring in only the files that matter
- Over the course of a project, an `AGENTS.md` file can collect several workflows and preferences.
  - `AGENTS.md` should contain durable, high-level guidance. More specialized details can live in focused documents.
  - Instead of placing every testing detail here, we can move that guidance into `docs/testing.md` and point Codex there when the task involves tests.
