# Task: Analyze this repository and generate a hierarchical AGENTS.md system

## Purpose

Create a high-signal AGENTS.md hierarchy that helps coding agents work safely,
quickly, and with low token waste.

This is not a one-shot file generation task.
Use a gated workflow where understanding and planning happen before writing.

## Core principle

Never generate final AGENTS.md files until a written plan is reviewed and
explicitly approved.

## Workflow map (authoritative)

Use this exact flow:

`Research -> Plan -> Annotate (repeat 1-6x) -> Todo list -> Implement ->`
`Feedback & Iterate`

Important:

- `repeat 1-6x` applies to the annotate loop only.
- Advance to todo list only when `Satisfied? = Yes`.
- Finish only when `Correct? = Yes` and `More tasks? = No`.

---

## Context and hierarchy principles

1. **Root AGENTS.md is lightweight**
   - Universal rules, navigation, and links to deeper files.
2. **Nearest-wins hierarchy**
   - The closest AGENTS.md to edited files should carry most specificity.
3. **JIT indexing over content dumping**
   - Prefer paths, globs, and search commands over large pasted examples.
4. **Token efficiency**
   - Use concise, actionable instructions.
5. **Sub-folder detail**
   - Sub-folder AGENTS.md files hold local patterns and examples.

---

## Tooling / MCP

### Tavily MCP (mandatory for web research)

Use Tavily for web search and extraction:

- `tavily_tavily_search`
- `tavily_tavily_extract`
- `tavily_tavily_crawl`
- `tavily_tavily_map`

Do not use other web search/scraping tools unless explicitly instructed.

### Ref MCP (mandatory for documentation lookup)

Use Ref for documentation and API references:

- `Ref_ref_search_documentation`
- `Ref_ref_read_url`

Search with Ref first, then read the returned URL.

### RepoPrompt MCP (if available, strongly recommended)

When `RepoPrompt_*` tools are present, use them for deterministic context:

1. Bind workspace/tab first.
2. Build minimal selection (`slices` when possible).
3. Use context-aware planning/review loops.

Do not rely on ambiguous active-tab state during long runs.

---

## Non-negotiable rules

1. **MUST** write deep findings to `research.md` before planning.
2. **MUST** write a detailed `plan.md` before implementation.
3. **MUST** keep assumptions/unknowns explicit and status-labeled.
4. **MUST** keep using `do not implement yet` in annotation loops.
5. **MUST NOT** generate AGENTS.md files before explicit approval.
6. **MUST** infer commands from repository evidence.
7. **MUST NOT** invent stack-specific commands without evidence.
8. **MUST NOT** stage or commit files unless explicitly requested.

---

## Your process

### Phase 1: deep repository research (output: `research.md`)

Study the codebase deeply before planning any file output.

`research.md` must include:

1. Repository type (single project, monorepo, multi-package, etc.).
2. Primary stack and tooling actually present.
3. Candidate folders that need their own AGENTS.md files.
4. Build/lint/test commands that really exist.
5. Naming/style/convention patterns and anti-patterns.
6. Risks, constraints, protected files, and dangerous operations.
7. Assumptions and unknowns (`verified` vs `unverified`).

Quality bar:

- Deep, file-grounded findings.
- Concrete paths and examples.
- No generic stack assumptions.

Example prompt pattern:

```text
Read this repository in depth and write all findings in research.md.
Cover architecture, commands, conventions, risks, and unknowns.
Do not plan or implement yet.
```

Definition of done for Phase 1:

- `research.md` exists and is concrete.
- Critical unknowns are resolved or clearly escalated.

---

### Phase 2: planning (output: `plan.md`)

Create a file-by-file AGENTS hierarchy plan based on `research.md`.

`plan.md` must include:

1. Target AGENTS.md file tree (root + sub-folders).
2. For each file: purpose, audience, and level of detail.
3. Required sections per file.
4. Real command strategy by scope (root vs package-local).
5. Cross-link strategy between AGENTS files.
6. Trade-offs and deduplication approach.
7. Verification strategy and rollback plan.
8. Assumptions and unknowns log.

Example prompt pattern:

```text
Using research.md and repository files, write a detailed plan.md for the
AGENTS.md hierarchy. Include file-by-file scope and validation strategy.
Do not implement yet.
```

Definition of done for Phase 2:

- `plan.md` exists and is reviewable.
- Scope and quality checks are explicit.

---

### Phase 2b: annotation cycle (repeat 1-6x)

Run this loop until plan quality is accepted:

1. Draft/update `plan.md`.
2. User adds inline notes.
3. Address every note point-by-point.
4. Rewrite `plan.md`.
5. Decision: `Satisfied?`
   - No -> loop back.
   - Yes -> continue.

Hard guard:

- Include `do not implement yet` on every loop turn.

Example prompt pattern:

```text
I added notes in plan.md. Address all notes and update the plan.
Do not implement yet.
```

---

### Phase 2c: todo expansion (inside `plan.md`)

Before writing AGENTS files, add a granular checklist:

- file creation/update tasks,
- cross-linking tasks,
- validation tasks,
- final quality checks.

Example prompt pattern:

```text
Add a detailed todo list to plan.md with all phases and tasks.
Do not implement yet.
```

Definition of done for Phase 2c:

- Todo list is explicit and execution-ready.

---

### No-implement gate (required stop)

After todo expansion, stop and ask:

`Approve implementation? (yes/no)`

Do not generate final AGENTS.md artifacts before explicit approval.

---

### Phase 3: implementation (generate AGENTS files)

After approval, execute the plan and mark todo items complete.

Execution contract:

```text
Implement all planned AGENTS.md updates.
Mark each completed task in plan.md.
Do not stop until all tasks are complete.
Keep output concise and avoid filler text.
Run available checks after edits.
```

Implementation rules:

- Root AGENTS.md should stay lightweight (target: 100-200 lines).
- Sub-folder AGENTS.md files should be more specific.
- Keep instructions path-anchored and practical.
- Do not add fictional commands or paths.

---

### Phase 4: feedback and iterate

Use terse, directive corrections during execution.

Loop:

1. Review generated files.
2. Decision gate: `Correct?`
   - No -> terse correction -> apply updates.
   - Yes -> continue.
3. Decision gate: `More tasks?`
   - Yes -> continue.
   - No -> done.

If direction drifts, prefer revert + narrow rescope + mini-plan refresh.

---

## Scope governance pass (pre-implementation)

Before approval, triage each planned change item:

- Accept as-is
- Modify approach
- Skip/remove
- Override technical choice

All decisions must converge into one refined implementation scope in `plan.md`.

---

## AGENTS.md content requirements

### Root `AGENTS.md` (lightweight)

Target length: ~100-200 lines.

Required sections:

1. Project snapshot.
2. Root setup/validation commands.
3. Universal conventions.
4. Security and secrets handling.
5. JIT index (directory map + quick find commands).
6. Definition of done / pre-PR baseline.

Requirements:

- Link to sub-folder AGENTS.md files.
- Keep root guidance universal and concise.
- Avoid duplicating deep local rules from sub-files.

### Sub-folder `AGENTS.md` files (detailed)

For each major package/folder include:

1. Package identity and purpose.
2. Setup/run/validate commands.
3. Patterns and conventions (most important section).
4. Key touch points and reference files.
5. JIT search hints.
6. Common gotchas.
7. Pre-PR checks for that package.

Requirements:

- Use real paths and real examples.
- Include at least one concrete DO and one concrete DON'T where possible.
- Keep package-specific details local to that file.

### Special considerations (add only when applicable)

- Design system packages.
- Database/data-layer services.
- API/backend services.
- Testing-focused packages.

Only include sections supported by repository evidence.

---

## RepoPrompt workflow (if available)

Use this order:

1. Bind to window/tab.
2. Discover structure (`get_file_tree`, `file_search`, `read_file`).
3. Build scoped selection (`manage_selection`, prefer `slices`).
4. Use `workspace_context` to verify token budget.
5. Use planning/review loops with explicit git context.

Guardrails:

- In multi-root workspaces, always scope paths explicitly.
- Clear stale prompt context between unrelated tasks.

---

## Output format (two-pass)

### Pass 1: research and planning only

Return in this order:

1. Analysis summary.
2. `research.md`.
3. `plan.md`.
4. Updated `plan.md` after annotation loops.
5. Todo-augmented `plan.md`.

Then stop at the no-implement gate and ask for approval.

### Pass 2: implementation artifacts (after approval)

Return in this order:

1. Root `AGENTS.md`.
2. Each sub-folder `AGENTS.md` (one at a time, with file path).

Format each file as:

```text
---
File: `path/to/AGENTS.md`
Purpose: short purpose line
---
[full content]
```

---

## Constraints and quality checks

Before finalizing, verify:

- [ ] Root AGENTS.md is concise and under 200 lines.
- [ ] Root links to all sub-AGENTS.md files.
- [ ] Sub-files contain concrete examples with real paths.
- [ ] Commands are copy-paste ready and repo-valid.
- [ ] No unnecessary duplication across hierarchy levels.
- [ ] JIT hints use actual repository patterns.
- [ ] Pre-PR checks are explicit and runnable.
- [ ] Dangerous operations require explicit permission.
- [ ] No staging or committing without explicit user request.
- [ ] Annotation and approval gates were followed.

---

## Start here

1. Perform deep repository research.
2. Write `research.md`.
3. Write `plan.md`.
4. Run annotation loop until satisfied.
5. Add todo list.
6. Stop and request explicit approval.
7. Only then generate AGENTS.md artifacts.

Do not skip gates.
