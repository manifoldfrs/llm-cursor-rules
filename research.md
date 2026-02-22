# Research report: `generate-claude.md`

## Objective

Understand how `generate-claude.md` currently works, what it is optimized for,
and what needs to change to align it with your planning-first workflow scaffold
(research -> plan -> annotate -> todo -> implement -> iterate).

Note: the repeat loop applies to the annotation cycle (1-6x) before the todo
list step, not to the full end-to-end pipeline.

## Method

- Performed a full deep read of `generate-claude.md` (748 lines).
- Compared it with `generate-agents.md` (266 lines) to identify intentional
  differences vs accidental drift.
- Cross-checked repository reality so recommendations fit this repo
  (`llm-cursor-rules/`) rather than an assumed monorepo app stack.

## Repository reality check

- This repository is documentation-first, not an app runtime monorepo.
- Current root contains guidance docs (`*.md`) and thematic folders
  (`marketing/`, `productivity/`, `sub-agents/`).
- There are no `apps/`, `packages/`, `services/`, or `.claude/` directories
  in the current tree.
- This matters because `generate-claude.md` currently assumes a Bun +
  TypeScript monorepo and generates many stack-specific examples.

## How `generate-claude.md` currently works

`generate-claude.md` is a meta-prompt template designed to make an agent output
a full CLAUDE.md hierarchy plus Claude-specific config artifacts.

### Functional flow (current)

1. **Phase 1: Repository analysis**
   - Requests architecture mapping, tooling, dangerous operations, and
     permission strategy.
2. **Phase 2: Root CLAUDE.md generation**
   - Requires a long root file (~200-400 lines) with 10 prescribed sections.
3. **Phase 3: Subdirectory CLAUDE.md generation**
   - Requires 100-200 line files for each major package/directory.
4. **Phase 4: Claude-specific config generation**
   - Adds `.claude/settings.json` hooks, slash command templates,
     MCP setup, and optional subagent recommendations.
5. **Output + quality gate**
   - Enforces output ordering and checklist validation.

### Key specificities in the current document

- Treats CLAUDE.md as immutable, high-priority system memory.
- Strongly emphasizes Claude-native capabilities (hooks, MCP, slash commands,
  subagents, context management).
- Uses many explicit code examples and canned command blocks.
- Imposes strict output formatting so generated files are copy/paste friendly.
- Promotes asking clarifying questions before generation.

## Strengths

- **Comprehensive coverage**: It addresses analysis, file generation,
  hooks, commands, MCP, and quality checks in one place.
- **Operational clarity**: The phased process and required sections reduce
  underspecified outputs.
- **Safety awareness**: Includes dangerous command blocking and secret-handling
  guidance.
- **Repeatability**: Output format and checklists make generation consistent.
- **Claude-aware design**: It is not generic AGENTS guidance; it intentionally
  leverages Claude Code primitives.

## Gaps and failure modes

### 1) Mismatch with your planning-first workflow

- Current file jumps from analysis directly into generation.
- It does not require a persistent `research.md` artifact before planning.
- It does not define the annotation cycle (inline note revisions 1-6 rounds).
- It does not enforce a hard gate: "no implementation before plan approval".
- It does not require a plan-embedded todo checklist prior to execution.

### 2) Over-assumed stack and structure

- Heavy Bun/TypeScript/Next.js/Express assumptions appear throughout.
- Preset directories (`apps/web`, `apps/api`, `packages/ui`, etc.) may not
  exist in many repos, including this one.
- Some commands are placeholders despite a quality rule saying commands should
  be copy/paste ready.

### 3) Artifact volume vs usability

- At 748 lines, the template is large and can be hard to maintain.
- It mixes normative rules, examples, and generation templates in one file.
- There is overlap with `generate-agents.md` in process scaffolding,
  increasing maintenance drift risk.

### 4) Implementation-stage control is underdefined

- The current prompt does not encode your preferred execution command style
  ("implement it all", task completion tracking, continuous typecheck).
- Feedback loops during implementation are not modeled as first-class workflow
  steps.

## Coverage mapping against your scaffold

### Research

- **Current status**: Partial.
- Has analysis phase, but no hard requirement to persist deep findings to
  `research.md` as a review surface.

### Plan

- **Current status**: Partial.
- Produces CLAUDE artifacts directly; does not create a dedicated `plan.md`
  with reviewable implementation choices and trade-offs.

### Annotate

- **Current status**: Missing.
- No explicit loop for user inline comments + agent revision rounds.

### Todo list

- **Current status**: Missing.
- No required granular checklist appended to plan prior to coding.

### Implement

- **Current status**: Weakly modeled.
- Focuses on generating docs/config, not a gated "execute approved plan"
  phase with strict implementation constraints.

### Feedback and iterate

- **Current status**: Missing.
- No short correction loop protocol for live implementation adjustments.

## Structural insights: why this matters

Your scaffold solves the most expensive AI failure mode: correct-looking code
that is system-wrong because assumptions were wrong early.

`generate-claude.md` currently optimizes for completeness of generated policy,
but not for correctness of project understanding before execution. It treats
planning as content emission, not as a collaboratively reviewed spec.

The missing piece is shared mutable artifacts (`research.md`, `plan.md`) that
survive context compaction and support precise, local annotations.

## Recommended update direction for `generate-claude.md`

### 1) Reframe around explicit artifact gates

Add hard phase contracts:

1. **Research phase (mandatory output: `research.md`)**
2. **Planning phase (mandatory output: `plan.md`)**
3. **Annotation cycles (update `plan.md`, "do not implement yet")**
4. **Todo expansion (append granular checklist into `plan.md`)**
5. **Implementation (only after explicit approval)**
6. **Feedback/iterate loop (short corrective prompts, update checklist)**

### 2) Add non-negotiable guardrails

- "Never implement before plan approval."
- "Every research request writes to `research.md`, not chat-only summaries."
- "After each annotation pass, rewrite `plan.md` in place and wait."
- "Before coding, ensure todo list exists and is complete enough to execute."

### 3) Reduce baked-in stack assumptions

- Move Bun/Next/Express examples into optional examples sections.
- Require commands to be inferred from actual repo files.
- If a command is unknown, require the model to state that explicitly and
  propose the closest repo-valid check.

### 4) Model implementation as mechanical execution

Embed your preferred execution prompt pattern directly:

- implement all tasks in plan
- mark each completed task in `plan.md`
- avoid unnecessary comments/JSDoc
- avoid `any` and `unknown` unless justified
- run typecheck/lint/tests continuously where available

### 5) Add concise iterative feedback protocol

- Encourage terse corrections during implementation.
- Instruct the agent to map each correction back to the relevant plan task.
- If direction is wrong, support explicit revert-and-rescope workflow.

## Proposed shape of the revised file

A cleaner v2 structure could be:

1. **Intent and operating principle** (plan before code)
2. **Phase 1: Deep research -> `research.md`**
3. **Phase 2: Plan -> `plan.md`**
4. **Phase 2b: Annotation loop (1-6 rounds, no implementation)**
5. **Phase 2c: Todo expansion in `plan.md`**
6. **Phase 3: Implementation execution contract**
7. **Phase 4: Feedback and iterative corrections**
8. **Optional Claude-specific add-ons** (hooks/MCP/slash commands/subagents)
9. **Output and quality checklist**

This keeps Claude-specific power features available, but subordinate to the
research/plan/approval pipeline.

## Detailed findings summary

- `generate-claude.md` is powerful but currently "generator-first," not
  "understanding-first."
- Your scaffold is compatible with the file's goals, but requires a process
  inversion: research and planning artifacts become mandatory gates.
- The biggest practical improvement is not more examples; it is enforcing
  persistent artifacts + annotation loops before any code execution.
- Once that gate exists, implementation quality and token efficiency should
  improve materially because execution becomes constrained and mechanical.

## Recommended next step

Use this research as the basis for a direct rewrite of `generate-claude.md`
that preserves its Claude-specific sections but reorganizes the workflow around
the six-phase scaffold and explicit "do not implement yet" checkpoints.

## Addendum: additional gaps from the source workflow doc

Yes, there are a few high-value elements from your source workflow that are not
yet explicitly captured and should be added to this research (and later to
`generate-claude.md`).

### 1) Explicit acceptance criteria for each artifact

Current research says "produce `research.md` and `plan.md`," but does not define
what makes each artifact acceptable.

Recommended additions:

- `research.md` must include architecture map, invariants, dependencies,
  constraints, and open questions.
- `plan.md` must include file-level changes, trade-offs, migration impact,
  rollback strategy, and verification steps.
- Implementation cannot begin until these acceptance criteria are satisfied.

### 2) Assumptions and unknowns log

Your workflow emphasizes catching wrong assumptions early. Add a required
"assumptions/unknowns" section in both `research.md` and `plan.md`.

Recommended additions:

- List each assumption explicitly.
- Mark each as verified/unverified.
- Require resolution of critical unknowns before implementation.

### 3) Stronger annotation loop protocol

We captured "annotation cycles," but not the concrete operational contract.

Recommended additions:

- Add loop limit guidance (e.g., 1-6 cycles).
- Require agent to address every inline note point-by-point.
- Keep the hard guard phrase: "do not implement yet" on every loop turn.
- Encode the decision flow explicitly: if not satisfied, loop back to review;
  if satisfied, proceed to requesting todo list.

### 4) Standard prompt templates per phase

The source doc relies on stable, reusable prompts. Current research summarizes
this, but does not preserve concrete templates.

Recommended additions:

- Include canonical prompts for research, planning, annotation update,
  todo expansion, and implementation.
- Keep implementation prompt as a reusable "execution contract" block.

### 5) Role shift during implementation

Your workflow has a key transition: architect in planning, supervisor during
implementation.

Recommended additions:

- State that post-approval feedback should be terse and directive.
- Preserve plan as source of truth; corrections should map back to plan tasks.
- Include the two-gate loop from the diagrams:
  1) `Correct?` -> if no, issue terse correction and re-implement;
  2) if yes, evaluate `More tasks?` -> continue or end.

### 6) Reference-first correction strategy

The source doc stresses "make it look like existing X" and screenshot-driven
frontend corrections.

Recommended additions:

- Require pointing to existing in-repo reference files for UI/UX parity.
- Encourage screenshot-based feedback for visual iteration tasks.

### 7) Revert-and-rescope protocol

We mention this briefly, but it should be formalized.

Recommended additions:

- If implementation drifts, prefer revert + narrow rescope over patching a bad
  direction incrementally.
- Require a mini-plan refresh after rescope.

### 8) Session continuity strategy

The source doc explicitly values single long sessions and artifact persistence
through compaction.

Recommended additions:

- Prefer one continuous session for research -> plan -> implement when feasible.
- Re-anchor context by re-reading `research.md` and `plan.md` after compaction.

### 9) Scope control decisions as a first-class step

Your "accept/modify/skip/override" pattern is central but not explicit yet.

Recommended additions:

- Add an explicit decision pass before implementation:
  accept, modify, remove, or override each proposed plan item.
- Treat scope trimming (removing nice-to-haves) as expected behavior.
- Keep the fan-out/fan-in structure explicit: all decisions converge into a
  refined implementation scope before execution.

### 11) Diagram-derived control points to codify

The screenshots add concrete flow-control semantics that should be mirrored in
`generate-claude.md` language:

- **Master sequence**: `Research -> Plan -> Annotate -> Todo List -> Implement
  -> Feedback & Iterate`.
- **Repeat marker placement**: `repeat 1-6x` belongs to the annotate loop.
- **Annotation exit condition**: only `Satisfied? = Yes` advances to todo list.
- **Implementation exit condition**: only `Correct? = Yes` and
  `More tasks? = No` yields `Done`.
- **Scope governance**: proposal triage (`accept/modify/skip/override`) must
  happen before implementation and resolve into one refined scope.

### 10) Closure criteria and done definition per phase

The workflow implies strict closure but does not yet have a formal checklist in
this research document.

Recommended additions:

- Research done: no unresolved critical assumptions.
- Plan done: annotated notes resolved and approved.
- Todo done: granular checklist present.
- Implementation done: tasks completed, checks passing, feedback addressed.

## Addendum: RepoPrompt MCP deep research

## Scope of this addendum

Research the `RepoPrompt_*` MCP toolset in detail and capture practical
guidance for adding a dedicated RepoPrompt section to `generate-claude.md`.

## What RepoPrompt is (in practice)

RepoPrompt is a workspace-aware MCP layer for codebase operations and
LLM-assisted planning/edit/review workflows. It combines:

- workspace/window/tab state management,
- selection and token-budget controls,
- file tree/search/read/edit primitives,
- read-only git analysis + diff snapshot publishing,
- integrated chat modes (`chat`, `plan`, `edit`, `review`), and
- an autonomous context discovery agent (`context_builder`).

It behaves like a structured IDE-memory system more than a single-file tool.

## Capability map by tool family

### 1) Workspace and tab control

Primary tools:

- `RepoPrompt_list_windows`
- `RepoPrompt_select_window`
- `RepoPrompt_manage_workspaces`

High-value behavior:

- Multi-workspace and multi-root support.
- Ability to bind this MCP session to a specific compose tab.
- Safe background execution via tab binding without changing active UI tab.

Key operational detail:

- Deterministic runs should always bind to an explicit tab to avoid context
  drift from user tab switches.

Observed in this session:

- Workspace `dotfiles` had multiple roots including this repository.
- `list_tabs` exposes `[active]` vs `[bound]`, which is crucial for stable
  tool behavior.

### 2) Selection and context budget control

Primary tools:

- `RepoPrompt_manage_selection`
- `RepoPrompt_workspace_context`
- `RepoPrompt_prompt`
- `RepoPrompt_get_code_structure`

Selection modes:

- `full`: full file contents.
- `slices`: selected line ranges.
- `codemap_only`: symbol/signature maps where available.

High-value behavior:

- Direct token accounting for selected files and prompts.
- Slice-based context curation is effective for large files.
- Prompt text in tab contributes to token usage and can be cleared.

Observed in this session:

- `generate-claude.md` full selection consumed ~5,552 tokens.
- Slice selection of two ranges reduced that to ~2,233 tokens.
- `RepoPrompt_prompt` with `op: clear` removed tab prompt overhead.
- Codemap coverage is parser-dependent; markdown had no codemap.

### 3) File discovery and reads

Primary tools:

- `RepoPrompt_get_file_tree`
- `RepoPrompt_file_search`
- `RepoPrompt_read_file`

High-value behavior:

- Unified path + content search with filters and regex controls.
- Tree views for roots, selected files, folders-only, and depth limits.
- Path-scoped searching across selected roots.

Observed in this session:

- Path-mode search for `*.md` returned expected repository markdown files.
- Content search returned matched lines with file-level grouping.
- `read_file` line-window reads were reliable and concise.

### 4) File edits

Primary tools:

- `RepoPrompt_apply_edits`
- `RepoPrompt_file_actions`

High-value behavior:

- Supports full rewrite, single replacement, and multiple literal edits.
- File actions support create/move/delete with safety constraints
  (notably absolute path requirement for delete).

Note:

- In this run, file editing for repository docs was done with non-RepoPrompt
  tools, but RepoPrompt editing primitives are suitable for deterministic
  doc and code updates when tab context is already established.

### 5) Git intelligence and snapshot artifacts

Primary tool:

- `RepoPrompt_git` (`status`, `diff`, `log`, `show`, `blame`)

High-value behavior:

- Read-only git operations with compare presets (`uncommitted`, `staged`,
  `main`, `back:N`, etc.).
- Diff detail levels from summary to full patches.
- Optional artifact publishing (`artifacts: true`) writes snapshot files
  under RepoPrompt git-data with MAP and patch files.

Observed in this session:

- `status` correctly reported modified/untracked files.
- `diff` with artifacts produced snapshot IDs and artifact paths
  (e.g. `_git_data/.../MAP.txt`, `all.patch`).
- `show`, `log`, and line-range `blame` behaved as expected.

### 6) LLM chat modes and discovery agent

Primary tools:

- `RepoPrompt_list_models`
- `RepoPrompt_chat_send`
- `RepoPrompt_chats`
- `RepoPrompt_context_builder`

Chat modes:

- `chat`: general discussion.
- `plan`: planning output.
- `edit`: code-change generation.
- `review`: diff-focused analysis.

High-value behavior:

- Model presets are discoverable at runtime.
- Chat sessions are addressable and resumable via `chat_id`.
- `context_builder` can auto-discover relevant files, build optimized
  selection, and rewrite prompt context with relationships/ambiguities.

Observed in this session:

- Available model preset surfaced as `GPT-5.2 High` for all four modes.
- `context_builder` created a dedicated tab, selected 3 relevant files, and
  emitted a structured prompt plus answer.
- `chat_send` in `plan` mode produced actionable restructuring guidance.

## Important behavioral nuances

### Review mode and git context

Empirical behavior indicates `chat_send` `review` mode does not always include
diff context automatically. Two distinct outcomes were observed:

- Without prepared snapshot context, review mode requested pasted `git diff`.
- After generating a git snapshot with `RepoPrompt_git` and using
  `git_scope: selected`, review mode successfully returned file-specific risks.

Practical implication:

- For reliable review runs, publish diff artifacts first and align selection
  with changed files before invoking review mode.

### Prompt and token hygiene

- Tab prompts can silently consume token budget.
- Clearing stale prompts before large context-building tasks improves
  efficiency.
- Slice selection is one of the highest ROI practices for token control.

### Multi-root caveat

- When workspace has multiple roots, path targeting must be explicit to avoid
  accidental cross-repo context in searches or selections.

## Recommended RepoPrompt section for `generate-claude.md`

`generate-claude.md` should gain a dedicated "RepoPrompt workflow" section with
clear, ordered guidance:

1. Bind window and tab (`list_windows` -> `select_window` -> `list_tabs` ->
   `select_tab`).
2. Establish minimal context (`get_file_tree`, `file_search`, `read_file`).
3. Build scoped selection (`manage_selection` with `slices` where possible).
4. Use `context_builder` for broad discovery tasks.
5. Use `chat_send` `plan` for planning; keep implementation blocked until plan
   approval.
6. Use `RepoPrompt_git diff` with `artifacts: true` before `chat_send` review.
7. Clear/reduce prompt + selection between unrelated tasks.

## Suggested guardrails to encode

- Always bind to a specific tab for deterministic context.
- Do not rely on review mode diff context without artifact prep.
- Prefer `slices` over `full` for large files unless full-file semantics are
  required.
- In multi-root workspaces, always pass explicit root/path filters.
- Keep discovery/planning and implementation phases separated by explicit
  approval gates.

## RepoPrompt-specific contribution to your workflow scaffold

RepoPrompt maps cleanly to your six-phase approach:

- **Research**: `context_builder` + `file_search` + `read_file` +
  `workspace_context`.
- **Plan**: `chat_send` `plan` with curated selection.
- **Annotate**: edit `plan.md` externally, then re-run `chat_send` with updated
  context.
- **Todo list**: maintain checklist in `plan.md`; optionally track with
  structured selection slices.
- **Implement**: `chat_send` `edit` / direct edits with strict scope.
- **Feedback/iterate**: `RepoPrompt_git diff` + `chat_send` `review` loops.

This makes RepoPrompt a strong fit for your artifact-first, annotation-heavy,
single-session workflow.
