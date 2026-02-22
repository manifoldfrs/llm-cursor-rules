# AGENTS.md - llm-cursor-rules

## Scope and Purpose
- This repository is documentation-heavy and contains AI assistant playbooks, writing guides, and project architecture notes.
- There is no runnable application code today, so agent work is mostly content maintenance and occasional addition of sample code snippets.
- Use this file as the primary instruction source for all AI-assisted edits in this repository.

## Rule Discovery (Cursor / Copilot)
- Checked `/Users/frshbb/github/autodidact/llm-cursor-rules/.cursor/rules` (not present).
- Checked `/Users/frshbb/github/autodidact/llm-cursor-rules/.cursorrules` (not present).
- Checked `/Users/frshbb/github/autodidact/llm-cursor-rules/.github/copilot-instructions.md` (not present).
- If any of these files appear later, treat them as higher-priority than this AGENTS file.

## Repository Layout (for navigation)
- `README.md`: main project description and quick notes.
- `generate-agents.md`: meta-instructions for building agenting guidance.
- `generate-claude.md`, `AIProductDev.md`, `CodeArchitect.md`: strategy and role prompts.
- `nextjs14-typescript-tailwind.md`, `nextjs14-clerk-convex-stripe.md`, `swift.md`, `optimization-principles.md`: reference playbooks.
- `Tailwind-v4.mdc`: formatter/config style note.
- `marketing/` and `productivity/`: content collections.
- `sub-agents/`: specialist persona prompts.

## Build / Lint / Test Commands
- There is currently no build system, package manager script set, or repository-wide test runner in this tree.
- Treat the repo as documentation-first; validation is done through static checks, not compilation.

### Baseline validation commands
- Check markdown linting (if `markdownlint-cli2` is installed):
  - `npx --yes markdownlint-cli2 "**/*.md"`
- Check a single markdown file (equivalent of a "single test"):
  - `npx --yes markdownlint-cli2 path/to/file.md`
- Check shell scripts syntax if any are added or edited:
  - `bash -n script.sh`
- Optional single-file shell syntax test:
  - `bash -n path/to/script.sh`
- Optional quick repo grep sanity for malformed Markdown tables (best-effort):
  - `rg -n "\t" *.md`

### Manual content checks (always run for substantial doc updates)
- Confirm heading hierarchy is consistent (`#`, `##`, `###`) and no skipped levels within a section.
- Confirm all commands in docs are valid for this repository’s context.
- Confirm links resolve (spot check):
  - `rg -n "\]\([^ )]+\)" *.md`
- Confirm no unresolved placeholders like `TODO`, `TBD`, `<TODO>` unless intentionally introduced.

### When testing is requested by user
- If the user asks for tests and expects a target stack not present, propose the closest equivalent command and ask only if blocked by missing tool availability.
- Do not invent package-specific commands unless they already exist in repo files.

## Editing Principles for this Codebase
- Make minimal, scoped edits. Preserve existing document voice and tone.
- Prefer additive updates over large rewrites.
- Avoid rearranging unrelated sections; keep historical context stable.
- Never remove existing guidance unless explicitly requested.
- Keep filenames descriptive and descriptive-kebab-case (`new-playbook.md`).
- For new top-level files, place them in the closest thematic area (docs root, `marketing/`, `productivity/`, `sub-agents/`).

## Markdown Style
- Use ATX headings (`#`, `##`, `###`) consistently.
- Use sentence case for heading titles unless style in surrounding file uses title case.
- Keep one blank line before and after heading blocks and lists.
- Use fenced code blocks with language tags when command/example language is clear.
- Keep line length roughly <= 100 chars for readability where practical.
- Use backticks for commands, file paths, env vars, and code identifiers.
- Prefer explicit links with context text, not bare URLs.
- Keep numbered lists sequential and stable after edits.
- For command examples include a `bash` info string.

## Markdown Content Conventions
- Avoid overloading documents with duplicated guidance that already exists elsewhere in this repo.
- If a document includes a workflow, include prerequisites and expected outcome.
- Mark uncertain recommendations as `Note:` or `Caveat:`.
- Keep tables small and aligned with column headings.
- Do not hide assumptions in unresolved placeholders.

## Import Style (applies to added code snippets)
- Group imports by origin:
  1) runtime/built-in, 2) third-party, 3) internal/project aliases, 4) relative.
- Use one import group per section separated by a blank line.
- Prefer named imports for explicitness where supported by the language.
- Avoid wildcard imports in newly written snippets.
- Keep import aliases meaningful and non-generic.

## Formatting Conventions (applies to added snippets)
- Use two-space indentation for TypeScript/JavaScript examples, unless surrounding file uses another convention.
- Use two-space indentation for YAML/JSON-like data blocks and 2-space nested list indentation in Markdown.
- Prefer semicolons in JS/TS examples to match modern lint defaults.
- Use trailing commas in multiline object/array literals.
- Keep function parameters each on own line when signatures get long.

## Naming Conventions (for added code)
- Variables and functions: `camelCase`.
- Types, classes, interfaces: `PascalCase`.
- File-level constants: `UPPER_SNAKE_CASE`.
- Command or helper script names: `kebab-case.sh`.
- Markdown filenames: `kebab-case.md`.
- Prefer full words over abbreviations in public names.

## Type Discipline (future-facing guidance)
- Prefer explicit return types for exported/public functions in TS/TSX and Python type hints for new snippets.
- Avoid `any`/`unknown` when a concrete shape is known.
- Prefer immutable data shape descriptions and avoid mutation-heavy examples unless justified.
- For Lua snippets, use local variable declarations and avoid implicit globals.
- When a type is optional, model it explicitly (`?`, `Optional`, union with `None`/`nil`) rather than relying on comments.

## Error Handling Expectations
- Prefer explicit error paths over silent failures.
- In TypeScript/JS examples:
  - Catch exceptions at boundary layers (IO, network, JSON parse).
  - Return/throw domain-specific messages (`context` + `cause` if available).
- In shell examples:
  - Use `set -euo pipefail` for non-interactive scripts.
  - Quote variable expansions and guard empty variables.
  - Fail fast on required dependency checks (`command -v` pattern).
- In markdown docs, call out preconditions before command blocks.

## Security and Privacy Rules
- Do not add credentials, secrets, or personal data.
- Never include raw API keys, tokens, or paths that reveal private environment details.
- Use placeholders for environment variables: `{{API_KEY}}` only in examples.

## Command Placement and Snippet Hygiene
- Keep command snippets minimal and runnable.
- Include required context flags (`--help`, paths, env vars) when demonstrating non-trivial commands.
- Avoid stale commands that depend on private/local state without notes.
- Prefer explicit path-based examples over relative assumptions.

## Validation Checklist Before Finishing Changes
- Confirm the target file exists and scope is respected.
- Run a targeted markdown check on at least the edited files.
- Run a targeted command for any edited shell script (syntax check).
- Verify links and references still make sense after edits.
- Summarize changed files and rationale in response.

## Single-File Workflow
- Preferred when changes are narrow:
  1) Edit one file.
  2) Run a targeted lint/check for that file.
  3) Validate the affected section only.
- Use this as the default when request scope is small and changes are textual.

## Multi-File Workflow
- For broad updates, group changes by theme (e.g., one playbook plus references).
- Run bulk checks only on files actually modified.
- Keep edits in the same conceptual file order to reduce merge noise.

## Git and Review Behavior
- Do not stage/commit unless explicitly requested by the user.
- Do not revert unrelated local edits.
- If an AGENTS file already existed, do not delete or replace without coordination; merge and keep legacy points.

## Quick References for Agentic Work
- `build`: no project-wide build pipeline.
- `lint`: `npx --yes markdownlint-cli2 "**/*.md"`.
- `test`: no dedicated test suite; use per-file markdown lint as single-test equivalent.
- `single test`: `npx --yes markdownlint-cli2 path/to/file.md`.

## Change Quality Expectations
- Keep edits deterministic and diff-friendly.
- Use plain ASCII in tracked files unless existing content already contains non-ASCII characters.
- Avoid over-formatting whole files when making small content updates.
- Prefer semantic clarity over stylistic overengineering.

## End-of-Change Reporting Requirements
- Report command(s) run and results.
- List all files modified with one-line references.
- Note any missing tool/dependency if validation command could not be executed.
- Provide follow-up suggestions only when natural and actionable.
