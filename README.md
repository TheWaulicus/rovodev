# Rovo Dev Configuration

This repository contains configuration files for Rovo Dev, an AI-powered development assistant.

## Files

- **AGENTS.md** - Agent behavior instructions and coding standards
- **config.yml** - Main configuration including bash command permissions
- **mcp.json** - Model Context Protocol (MCP) server configurations

## Configuration Summary

### Bash Command Permissions

The config.yml has been optimized from **700+ individual command entries** to **~60 pattern-based rules**:

- **Default permission**: `ask` (secure by default)
- **Untrusted patterns** (20 rules): Commands that can modify/delete files or perform risky operations
- **Trusted patterns** (42 rules): Safe read-only commands and git operations

### Key Security Features

**Untrusted (require permission):**
- File deletion/modification commands
- Git force operations (`push --force`, `reset --hard`, `clean -f`)
- Package manager installs (`npm install`, `yarn add`)
- Arbitrary Python code execution (`python3 -c`)
- Output redirection to files (prevents data exfiltration)

**Trusted (auto-allowed):**
- Read-only commands (`cat`, `grep`, `ls`, `find`, `head`, `tail`)
- Safe git operations (`add`, `commit`, `checkout`, `pull`, `fetch`)
- Command chaining with safe tools
- Temporary file cleanup (`rm tmp_rovodev_*`)

### Most Common Workspaces

Based on session history analysis:
1. `/Users/sherman/Bitbucket/mdm-web-tooling/unified_jamf_manager` (7 sessions)
2. `/Users/sherman/Bitbucket/macos-mdm/Packages/CA Renewal Remediation` (5 sessions)
3. `/Users/sherman/Github` (4 sessions)
4. `/Users/sherman/Bitbucket/macos-mdm` (3 sessions)
5. `/Users/sherman/Bitbucket/cpe-tools` (3 sessions)

## Agent Instructions (AGENTS.md)

### Role
- Senior Client Platform Engineer specializing in Python and Bash
- Direct, terse, and casual communication style
- Provides actual code/solutions immediately

### Coding Standards
- **Naming conventions:**
  - `camelCase` for variables, functions, methods
  - `PascalCase` for class names
  - `snake_case` for files/directories
  - `UPPER_CASE` for environment variables
- **Python**: PEP 8, type hints, pytest for testing
- **Bash**: POSIX-compliant, shellcheck validation, error handling with trap

### Documentation
- Create all documentation in Confluence (not .md files in repos)
- Personal space: https://hello.atlassian.net/wiki/spaces/~70354112/overview

### MCP Servers
- Always use all available MCP servers when possible
- Context7 auto-triggered for code generation, setup/config, library docs
- Leverage Atlassian, Scout, Git, Fetch tools

## MCP Server Configuration (mcp.json)

Active MCP servers:
- **atlassian** - Jira and Confluence integration
- **scout** - Code intelligence and search
- **context7** - Library documentation and code generation
- **git** - Git repository operations
- **fetch** - Web content fetching
- **sequential-thinking** - Complex problem solving

## Usage

1. Copy configuration files to `~/.rovodev/`:
   ```bash
   cp config.yml ~/.rovodev/
   cp mcp.json ~/.rovodev/
   cp AGENTS.md ~/.rovodev/
   ```

2. Restart Rovo Dev to apply changes

3. Configuration is loaded automatically on startup

## Backup & Version Control

Repository: https://github.com/TheWaulicus/rovodev

Changes are tracked and pushed to GitHub for backup and version history.

## Session Management

Sessions are stored in `~/.rovodev/sessions/` with:
- `metadata.json` - Session title and workspace
- `session_context.json` - Full conversation history
- `memory` - Agent instructions and context

## Customization

### Adding New Bash Commands

Edit `config.yml` under `toolPermissions.bash.commands`:

```yaml
- command: ^your_command_pattern.*
  permission: allow  # or ask
```

Patterns are evaluated in order:
1. UNTRUSTED patterns (ask) - highest precedence
2. TRUSTED patterns (allow) - lower precedence
3. Default permission (ask) - fallback

### Project-Specific Instructions

Create `AGENTS.md` or `AGENTS.local.md` in project directories for workspace-specific guidelines.

## Security Notes

- Default permission is `ask` for safety
- Output redirection requires permission (prevents data exfiltration)
- Destructive git operations require confirmation
- Package installations require explicit approval
- Temporary files use `tmp_rovodev_` prefix for auto-cleanup

## Maintenance

Keep configuration backed up to GitHub:
```bash
cd ~/Github/rovodev
git add -A
git commit -m "Update configuration"
git push
```

---

*Last updated: 2024-10-30*
