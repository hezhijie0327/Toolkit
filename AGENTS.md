# AGENTS.md

The Toolkit is a collection of shell scripts for automating Git and GPG operations. See [README.md](README.md) for details.

## HANDOVER.md

When a multi-step task is interrupted, write progress to `HANDOVER.md` so the next session can pick up where you left off. Include: what was done, what remains, key decisions made, and the next concrete step. Delete the file when the task is complete.

## Behavioral Guidelines

**Tradeoff:** These guidelines bias toward caution over speed. For trivial tasks, use judgment.

### 1. Think Before Coding

**Don't assume. Don't hide confusion. Surface tradeoffs.**

Before implementing:
- State your assumptions explicitly. If uncertain, ask.
- If multiple interpretations exist, present them — don't pick silently.
- If a simpler approach exists, say so. Push back when warranted.
- If something is unclear, stop. Name what's confusing. Ask.

### 2. Simplicity First

**Minimum code that solves the problem. Nothing speculative.**

- No features beyond what was asked.
- No abstractions for single-use code.
- No "flexibility" or "configurability" that wasn't requested.
- No error handling for impossible scenarios.
- If you write 200 lines and it could be 50, rewrite it.

Ask yourself: "Would a senior engineer say this is overcomplicated?" If yes, simplify.

### 3. Surgical Changes

**Touch only what you must. Clean up only your own mess.**

When editing existing code:
- Don't "improve" adjacent code, comments, or formatting.
- Don't refactor things that aren't broken.
- Match existing style, even if you'd do it differently.
- If you notice unrelated dead code, mention it — don't delete it.

When your changes create orphans:
- Remove imports/variables/functions that YOUR changes made unused.
- Don't remove pre-existing dead code unless asked.

The test: Every changed line should trace directly to the user's request.

### 4. Goal-Driven Execution

**Define success criteria. Loop until verified.**

Transform tasks into verifiable goals:
- "Add validation" → "Write tests for invalid inputs, then make them pass"
- "Fix the bug" → "Write a test that reproduces it, then make it pass"
- "Refactor X" → "Ensure tests pass before and after"

For multi-step tasks, state a brief plan:
```
1. [Step] → verify: [check]
2. [Step] → verify: [check]
3. [Step] → verify: [check]
```

Strong success criteria let you loop independently. Weak criteria ("make it work") require constant clarification.

**These guidelines are working if:** fewer unnecessary changes in diffs, fewer rewrites due to overcomplication, and clarifying questions come before implementation rather than after mistakes.

**Additional rules:**
- Be concise in output but thorough in reasoning.
- Do not re-read files you have already read.
- User instructions always override this file.
- Don't wrestle with indentation in the Edit tool — focus on logic, let the project's formatter fix formatting. Use `sed` or `python3` freely when the Edit tool struggles with whitespace.

## Git commit convention

Every commit subject starts with a type prefix — `feat` / `fix` / `chore` / `docs` / `revert` / `refactor` / `perf` / `test` / `build` — optionally scoped: `fix(toolbar): ...`. Subject: imperative, lowercase start, no trailing period, aim ≤ 72 chars; detail goes in the body after a blank line (why, not what); breaking changes take `!` before the colon or a `BREAKING CHANGE:` body footer. Never rewrite pushed history.

- Commit incrementally — one logical change per commit, after lint and tests pass. Present changes for review before committing.
- PRs: link the issue, summarize the change and verification (tests/benchmarks), and include config/log/pprof evidence for bug fixes.

### Co-Author Format

- Always add co-author information. If closing an issue in commit text, verify against `main` first: `gh issue view <issue-id>`.
- Only ONE co-author line is allowed. If multiple agents contributed, aggregate into ONE entry.

Format: `Co-authored-by: <AgentName> <Email>`

Valid examples (choose EXACTLY ONE):

```
Co-authored-by: ZCode <noreply@z.ai>
```

## Documentation format

Family-wide Markdown rules, shared verbatim across all ZJ projects:

- **Language**: AGENTS.md, AUDIT.md, DESIGN.md and other agent-facing docs are English; README.md is Chinese (human-facing). Keep literal UI strings and code identifiers verbatim.
- **AGENTS.md skeleton** — generic sections first, then project; omit sections that don't apply, keep the order:
  `# AGENTS.md` → 1–2 sentence intro (what the project is + key doc links) → Behavioral Guidelines → Git commit convention → Documentation format → Project → Commands → Architecture → Conventions (Gotchas as a subsection) → project-specific sections → Key Docs.
- **README.md skeleton**: `# <project>` → badge block → one-line positioning + stack + key links → 特性 → 快速开始 → 配置 / 用法 → 文档 → License (omit as needed).
- **README badges** (shields.io, after the title, in order): Version (only where a real version exists, linked to the repo's GitHub releases), License (Apache 2.0 with Commons Clause — every project), primary-language badge (Go / TypeScript / Bash / Python / Docker), Biome for TypeScript projects; pure data/config repos get License only. Version links use the real GitHub repo name.
- **Formatting**: ATX headings (`#`), never skip a level; code fences always carry a language tag (`bash`, `go`, `json`, `text` for trees); tables for structured reference; no decorative emoji in headings or bullets; bold for key terms only.

## Project

The Toolkit is a collection of shell scripts for automating Git and GPG operations. It's a minimalistic Bash-based toolkit with no external dependencies beyond standard Unix tools.

## Commands

### Running the Scripts
```bash
# Git operations
bash ./Git.sh -u "username" -r "repo" -b "branch" -d "1" -m "clone"
bash ./Git.sh -u "username" -r "repo" -e "email@example.com" -f "." -i "commit message" -m "push"

# GPG operations
bash ./GPG.sh -m "decrypt" -p "password" -f "file"
bash ./GPG.sh -m "encrypt" -r "recipient" -f "file"
bash ./GPG.sh -m "export" -t "private" -p "password" -k "keyfile"
bash ./GPG.sh -m "import" -t "private" -p "password" -k "keyfile"
bash ./GPG.sh -m "sign" -e "asc" -p "password" -f "file"
bash ./GPG.sh -m "verify" -f "file" -s "signature"
```

### Testing Commands
- No automated test framework is currently in place
- Manual testing by running scripts with various parameters is expected
- Verify functionality with both valid and invalid inputs

### Linting Commands
- No formal linting configuration exists
- Recommended to use shellcheck for script validation:
```bash
shellcheck Git.sh
shellcheck GPG.sh
```

### Code Formatting
- No formal formatting configuration exists
- Recommended to use shfmt for consistent formatting:
```bash
shfmt -w Git.sh
shfmt -w GPG.sh
```

## Conventions

### Script Structure
1. **Shebang**: Always start with `#!/bin/bash`
2. **Usage Examples**: Include clear usage examples at the top of each script
3. **Section Comments**: Use `##` for major sections (Parameter, Function, Process)
4. **Function Comments**: Use `#` for single-line comments describing functions

### Parameter Parsing
- Use `getopts` for parameter parsing
- Use descriptive variable names in UPPERCASE
- Each parameter should have a single letter option

### Variable Naming
- **Global Variables**: UPPERCASE with underscores (e.g., `GIT_MODE`, `REPO_NAME`)
- **Function Names**: PascalCase with descriptive names (e.g., `CheckConfigurationValidity`)
- **Local Variables**: Lowercase with underscores when needed

### Error Handling
- Always validate required parameters before processing
- Use consistent error message format: `"An error occurred during processing. [Error description], please check it and try again."`
- Exit with code 1 on errors
- Check command exit codes with `"$?" -eq "0"`

### Function Organization
- **Validation Functions**: Check parameter validity and requirements
- **Main Functions**: Implement core functionality
- Function names should be descriptive and verb-based

### Code Patterns
1. **Parameter Validation**: Use the `CheckConfigurationValidity` pattern
2. **Requirement Checking**: Use the `CheckRequirement` pattern for external tools
3. **Success/Failure Messages**: Use consistent messaging:
   - Success: `"Congratulations! [action] has [past tense verb] successfully."`
   - Failure: `"Oops! [action] has not [past tense verb] successfully."`

### String Quoting
- Always quote variables: `"${VARIABLE}"`
- Use double quotes for strings that may contain spaces
- Use single quotes for literal strings

### Conditional Statements
- Use `[ ]` for test conditions
- Use `&&` and `||` for simple conditions
- Use `if/elif/else` for complex logic

### File Operations
- Always check file existence before operations: `[ ! -f "${FILE}" ]`
- Use appropriate file permissions
- Handle file operations in atomic fashion where possible

### Git Operations
- Use standard git commands with explicit options
- Configure user.name and user.email before commits
- Use --force flag carefully in push operations

### GPG Operations
- Use --pinentry-mode "loopback" for automated operations
- Use --armor for ASCII-armored output
- Use --batch for non-interactive imports

## Security Considerations

- Never log or echo passwords in production
- Validate all user inputs, especially file paths
- Use appropriate GPG trust models
- Handle temporary files securely

## License

- Project uses Apache License 2.0 with Commons Clause v1.0
- Cannot "Sell" the Software as defined by the Commons Clause
- Include license header in new files

## Development Workflow

1. Create/edit script files
2. Test with valid parameters
3. Test with invalid parameters (error handling)
4. Run shellcheck for script validation
5. Format with shfmt if available
6. Commit with descriptive messages
