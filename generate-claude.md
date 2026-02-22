# Task: Analyze this codebase and generate a hierarchical CLAUDE.md system

## Critical context: Claude Code is different from generic agents

Claude Code has platform-specific behavior that should shape your output:

1. **Instruction hierarchy**: `CLAUDE.md` is treated as high-priority guidance.
2. **Hierarchical memory**: `CLAUDE.md` files are discovered across directories.
3. **Hooks**: lifecycle automation via `.claude/settings.json`.
4. **MCP support**: tool integration, including workspace-aware flows.
5. **Custom commands**: reusable workflows in `.claude/commands/`.
6. **Subagents**: isolated specialists for bounded tasks.
7. **Long sessions**: large context windows with compaction behavior.

Do not treat this as a one-shot text generation task.
Treat it as a gated software-delivery workflow.

## Core operating principle

Never generate implementation artifacts until the written plan has been reviewed
and explicitly approved.

For this task, implementation artifacts include:

- `CLAUDE.md` files,
- `.claude/settings.json`,
- `.claude/commands/*.md`,
- `.mcp.json` examples,
- subagent config suggestions.

## Workflow map (authoritative)

Use this exact flow:

`Research -> Plan -> Annotate (repeat 1-6x) -> Todo list -> Implement ->`
`Feedback & Iterate`

Important:

- `repeat 1-6x` applies to the annotate loop, not the whole pipeline.
- Only advance to todo list when `Satisfied? = Yes`.
- Only finish implementation when `Correct? = Yes` and `More tasks? = No`.

---

## Non-negotiable rules

1. **MUST** write deep findings to `research.md` before planning.
2. **MUST** write a detailed `plan.md` before implementation.
3. **MUST** include assumptions and unknowns in both files.
4. **MUST** keep the guard phrase `do not implement yet` during annotation.
5. **MUST NOT** generate final files before explicit approval.
6. **MUST** update `plan.md` todo items as implementation progresses.
7. **MUST** infer commands from the real repo, not from template assumptions.
8. **MUST NOT** invent stack-specific commands when evidence is missing.

---

## Your process

### Phase 1: deep research (output: `research.md`)

Read the repository deeply and produce a persistent research artifact.
Do not start planning until this file exists.

`research.md` must include:

1. Repository architecture and boundaries.
2. Existing conventions and invariants.
3. Tooling and validation commands that actually exist.
4. Critical touch points and dependency relationships.
5. Risks, unknowns, and assumptions (`verified` vs `unverified`).
6. Dangerous operations and protected files.

Research quality bar:

- No shallow summaries.
- Reference concrete files/paths/patterns.
- Surface ambiguities early.

Example prompt patterns:

```text
Read this area in depth. Understand how it works and write detailed
findings in research.md.
Do not plan or implement yet.
```

```text
Study this subsystem deeply, including edge cases and likely failure modes.
Write everything you learn in research.md with concrete file references.
Do not plan or implement yet.
```

Definition of done for Phase 1:

- `research.md` exists.
- No unresolved critical unknowns remain.
- Findings are concrete enough to support planning.

---

### Phase 2: planning (output: `plan.md`)

After research review, generate a detailed implementation plan.
This plan is the review surface and source of truth.

`plan.md` must include:

1. Objective and business outcome.
2. Scope in/out and protected interfaces.
3. File-by-file change plan.
4. Alternatives considered and trade-offs.
5. Data/model/migration impact (if any).
6. Verification strategy (tests, lint, typecheck, manual checks).
7. Rollback and risk mitigation strategy.
8. Assumptions and unknowns log.

Plan quality bar:

- Base recommendations on actual repository files.
- Include concrete snippets where useful.
- Avoid generic architecture advice disconnected from codebase reality.

Example prompt patterns:

```text
Using research.md and source files, write a detailed plan.md for this change.
Include touched files, trade-offs, and verification steps.
Do not implement yet.
```

```text
Write plan.md based on actual code in this repo.
Include assumptions and open questions explicitly.
Do not implement yet.
```

Definition of done for Phase 2:

- `plan.md` exists and is reviewable.
- Scope and trade-offs are explicit.
- Verification and rollback paths are explicit.

---

### Phase 2b: annotation cycle (repeat 1-6x)

Run this loop until plan quality is accepted.

Flow:

1. Agent writes/updates `plan.md`.
2. User reviews in editor and adds inline notes.
3. Agent addresses each note point-by-point.
4. Agent rewrites `plan.md` accordingly.
5. Decision gate: `Satisfied?`
   - No -> loop back.
   - Yes -> continue to todo list.

Hard guard:

- Every loop instruction includes: `do not implement yet`.

Example update prompt:

```text
I added notes in plan.md. Address every note and update the document.
Do not implement yet.
```

---

### Phase 2c: todo expansion (inside `plan.md`)

Before implementation, add a granular checklist with phases and tasks.

Requirements:

- Tasks are actionable and ordered.
- Include validation checkpoints.
- Include status markers (pending/in_progress/completed).

Example prompt:

```text
Add a detailed todo list to plan.md with all phases and individual tasks.
Do not implement yet.
```

Definition of done for Phase 2c:

- Todo list is complete enough for uninterrupted execution.

---

### No-implement gate (required stop)

After todo expansion, stop and ask for explicit approval.

Required behavior:

- Do not generate target files yet.
- Ask: `Approve implementation? (yes/no)`
- Only proceed on explicit approval.

---

### Phase 3: implementation execution

Once approved, execute the plan mechanically.

Standard execution contract:

```text
Implement it all.
When you complete a task or phase, mark it completed in plan.md.
Do not stop until all tasks and phases are completed.
Do not add unnecessary comments or JSDoc.
Do not use any or unknown types without explicit justification.
Continuously run available checks (typecheck/lint/tests) to catch issues early.
```

Implementation rules:

- Plan is authoritative; do not freelance new scope.
- If commands are unavailable, report closest repo-valid check.
- Keep diffs scoped and deterministic.

Definition of done for Phase 3:

- All plan tasks marked complete.
- Relevant checks pass (or blockers are explicitly documented).

---

### Phase 4: feedback and iterate

After implementation begins, feedback should be short and directive.

Loop behavior:

1. `I review / test`
2. Decision gate: `Correct?`
   - No -> provide terse correction -> implement updates -> re-check
   - Yes -> continue
3. Decision gate: `More tasks?`
   - Yes -> continue implementation
   - No -> done

Guidance:

- Prefer terse corrections over long restatements.
- Point to existing references for UI/UX parity.
- Use screenshot feedback for visual alignment when relevant.

Revert and rescope protocol:

- If direction is wrong, prefer revert + narrow rescope.
- Refresh `plan.md` mini-plan before re-implementation.

---

## Scope governance pass (before implementation)

Before approval, triage each proposed change item:

- Accept as-is
- Modify approach
- Skip/remove
- Override technical choice

All outcomes must converge into one refined implementation scope in `plan.md`.

---

## RepoPrompt MCP workflow (when available)

If `RepoPrompt_*` tools are available, use them deliberately.

### A) Deterministic context binding

1. `RepoPrompt_list_windows`
2. `RepoPrompt_select_window`
3. `RepoPrompt_manage_workspaces` (`list_tabs` then `select_tab`)

Why:

- Prevent context drift from active-tab changes.
- Keep long workflows reproducible.

### B) Discovery and selection hygiene

Use:

- `RepoPrompt_get_file_tree`
- `RepoPrompt_file_search`
- `RepoPrompt_read_file`
- `RepoPrompt_manage_selection`
- `RepoPrompt_workspace_context`

Rules:

- Prefer `slices` over full-file selection for large files.
- Keep prompt/selection lean to reduce token waste.
- In multi-root workspaces, scope paths explicitly.

### C) Planning and review flow with RepoPrompt chat

Use:

- `RepoPrompt_chat_send` mode `plan` for plan refinement.
- `RepoPrompt_git` for status/diff/log.
- `RepoPrompt_chat_send` mode `review` after git prep.

Important nuance:

- Review mode may require prepared git diff context.
- Publish artifacts first for reliability when needed:
  `RepoPrompt_git` with `artifacts: true`.

### D) Context builder for broad tasks

Use `RepoPrompt_context_builder` when task scope is uncertain or broad.
It can auto-select relevant files and produce structured prompt context.

---

## Phase 5 deliverable target for this task

For this specific generator task (hierarchical Claude system), implementation
should produce these artifacts after approval:

1. Root `CLAUDE.md`.
2. Subdirectory `CLAUDE.md` files as needed.
3. `.claude/settings.json` hooks configuration.
4. `.claude/commands/*.md` command set.
5. MCP setup recommendations (and optional `.mcp.json` example).
6. Optional subagent recommendations.

---

## Root `CLAUDE.md` requirements

Generate a comprehensive root file with these sections:

1. Project identity and architecture snapshot.
2. Universal MUST/SHOULD/MUST NOT rules.
3. Core commands inferred from repo reality.
4. Project structure map with links to sub-`CLAUDE.md` files.
5. JIT quick-find commands based on real file patterns.
6. Security and secrets policy.
7. Git workflow.
8. Testing strategy.
9. Tool permissions.
10. Specialized-context navigation.

Constraint:

- Do not claim commands or frameworks that are not evidenced in-repo.

---

## Subdirectory `CLAUDE.md` requirements

For each major package/folder, include:

1. Package identity and parent-context link.
2. Setup/run commands for that package.
3. Architecture and code organization patterns.
4. Key files and touch points.
5. JIT search hints for that package.
6. Common gotchas.
7. Package-specific pre-PR validation command.

Keep examples concrete and path-based.

---

## Claude-specific config requirements

### Hooks (`.claude/settings.json`)

Define conservative automation first:

- Pre-tool safety checks (dangerous commands, protected files).
- Post-tool formatting/testing hooks with scoped matchers.

Do not make hooks so broad they slow every operation.

### Custom commands (`.claude/commands/`)

Start with common workflows only (3-5 commands), for example:

- review
- fix-issue
- migrate-db

Each command should include validation steps and expected outputs.

### MCP recommendations

Recommend servers that match actual project needs.
Avoid suggesting integrations with no clear use case.

---

## Output format (two-pass)

### Pass 1 (research and planning only)

Return in this order:

1. Analysis summary.
2. `research.md`.
3. `plan.md`.
4. Updated `plan.md` after annotation loops.
5. Todo-augmented `plan.md`.

Then stop at the no-implement gate and ask for approval.

### Pass 2 (implementation artifacts after approval)

Return in this order:

1. Root `CLAUDE.md`.
2. `.claude/settings.json`.
3. `.claude/commands/*.md` files.
4. Subdirectory `CLAUDE.md` files.
5. MCP setup guide.
6. Optional subagent configs.

Format each file as:

```text
---
File: `path/to/file`
Purpose: short purpose line
---
[full content]
```

---

## Quality checklist

Before finalizing, verify:

- [ ] Research quality is deep, path-grounded, and assumption-aware.
- [ ] Plan quality includes trade-offs, risk, rollback, verification.
- [ ] Annotation loop ran until `Satisfied? = Yes`.
- [ ] Todo list exists before implementation.
- [ ] No implementation started before explicit approval.
- [ ] Implementation tracked task completion in `plan.md`.
- [ ] Feedback loop used `Correct?` and `More tasks?` gates.
- [ ] Root and sub-`CLAUDE.md` files avoid unsupported stack assumptions.
- [ ] Commands are repo-valid and copy-paste ready.
- [ ] Hooks are specific and not overly broad.
- [ ] Security and dangerous-operation rules are explicit.
- [ ] No duplication between hierarchy levels.

---

## Session strategy

Prefer one continuous session across research, planning, and implementation.
When context compacts, re-anchor on persistent artifacts:

- `research.md`
- `plan.md`

These files are the durable memory layer.

---

## Start here

1. Perform deep repository research.
2. Write `research.md`.
3. Write `plan.md`.
4. Run annotation loops until approved.
5. Add todo list.
6. Stop and request explicit implementation approval.

Do not skip gates.
