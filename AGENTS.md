# AGENTS.md

This file contains guidelines and commands for agentic coding agents working on the Toolkit repository.

## Project Overview

The Toolkit is a collection of shell scripts for automating Git and GPG operations. It's a minimalistic Bash-based toolkit with no external dependencies beyond standard Unix tools.

## Build/Test/Lint Commands

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

## Code Style Guidelines

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