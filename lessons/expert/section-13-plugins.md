# Section 13: Plugins (Lessons 81-86)

## Overview
Learn how to extend Claude Code through the plugin system. Build, distribute, and manage plugins that package skills, subagents, hooks, and MCP servers into reusable, shareable units.

---

## Lesson 81: Plugin Architecture

### Objectives
- Understand the four core plugin components: skills, subagents, hooks, and MCP servers
- Learn the standard plugin directory structure
- Master the plugin.json manifest format
- Explain how plugins extend Claude Code capabilities

### Content

#### What Is a Plugin?

A plugin is a self-contained package that extends Claude Code with new capabilities. Rather than configuring skills, hooks, subagents, and MCP servers individually, a plugin bundles them together with metadata, documentation, and versioning so they can be installed, updated, and shared as a single unit.

#### The Four Plugin Components

| Component | Purpose | Example |
|-----------|---------|---------|
| **Skills** | Slash commands that perform specific tasks | `/deploy`, `/lint-fix`, `/db-migrate` |
| **Subagents** | Specialized agents for domain-specific work | Security auditor, API designer |
| **Hooks** | Event-driven automation triggered by Claude actions | Auto-format on file save, notify on error |
| **MCP Servers** | External tool integrations via Model Context Protocol | Database access, CI/CD control, monitoring |

#### Plugin Directory Structure

Every plugin follows a standard layout:

```
my-plugin/
├── skills/
│   ├── deploy.md
│   └── lint-fix.md
├── subagents/
│   └── security-auditor.md
├── hooks/
│   └── hooks.json
├── mcp/
│   └── mcp-config.json
├── README.md
└── plugin.json
```

Each directory is optional. A plugin may provide only skills, or only hooks, or any combination of the four components.

#### The plugin.json Manifest

The manifest file describes the plugin and declares its contents:

```json
{
  "name": "devops-toolkit",
  "version": "1.2.0",
  "description": "DevOps automation skills, hooks, and integrations for Claude Code",
  "author": "Jane Smith",
  "license": "MIT",
  "repository": "https://github.com/janesmith/devops-toolkit",
  "keywords": ["devops", "deploy", "ci-cd", "docker"],
  "claude_code_version": ">=1.0.0",
  "components": {
    "skills": [
      "skills/deploy.md",
      "skills/lint-fix.md"
    ],
    "subagents": [
      "subagents/security-auditor.md"
    ],
    "hooks": "hooks/hooks.json",
    "mcp": "mcp/mcp-config.json"
  },
  "config": {
    "deploy_target": {
      "type": "string",
      "description": "Default deployment target",
      "default": "staging"
    }
  },
  "dependencies": {
    "docker": ">=20.0.0",
    "node": ">=18.0.0"
  }
}
```

#### Key Manifest Fields

| Field | Required | Description |
|-------|----------|-------------|
| `name` | Yes | Unique plugin identifier (lowercase, hyphens) |
| `version` | Yes | Semver version string |
| `description` | Yes | One-line summary of the plugin |
| `components` | Yes | Map of component types to file paths |
| `author` | No | Plugin author name or organization |
| `claude_code_version` | No | Minimum compatible Claude Code version |
| `config` | No | User-configurable settings with defaults |
| `dependencies` | No | External tool requirements |

#### How Plugins Extend Claude Code

When a plugin is installed, Claude Code:

1. Copies skill files into the active skills directory
2. Registers subagent definitions for use by the agent system
3. Merges hook configurations into the project or user settings
4. Adds MCP server entries to the MCP configuration

All components become available immediately, as if they had been configured manually.

### Exercise

**Task**: Examine and sketch a plugin

1. Choose a workflow you repeat often (e.g., database migrations, test scaffolding)
2. Identify which components it would need (skills, hooks, subagents, MCP servers)
3. Write out the directory structure on paper or in a scratch file
4. Draft a `plugin.json` manifest with all required fields

### Verification
- [ ] Can name all four plugin component types
- [ ] Directory structure follows the standard layout
- [ ] plugin.json includes name, version, description, and components
- [ ] Understand how each component integrates into Claude Code

---

## Lesson 82: Creating Plugins

### Objectives
- Create a plugin from scratch using the standard structure
- Build skills, hooks, and subagents for the plugin
- Add MCP server configuration when needed
- Test the plugin locally before distribution

### Content

#### Step 1: Design the Plugin Scope

Before writing any files, define what your plugin will do:

```markdown
Plugin: test-helper
Purpose: Streamline test creation and execution
Components needed:
  - Skill: /generate-test (creates test files from source)
  - Skill: /run-tests (executes tests with coverage)
  - Hook: auto-run tests when test files change
  - Subagent: test-reviewer (analyzes test quality)
```

Keep the scope focused. A plugin that does one thing well is more useful than one that does many things poorly.

#### Step 2: Create the Directory Structure

```bash
mkdir -p test-helper/{skills,subagents,hooks}
touch test-helper/plugin.json
touch test-helper/README.md
```

#### Step 3: Build Skills

Create `skills/generate-test.md`:

```markdown
# Generate Test

Create a comprehensive test file for a given source file.

## Usage
/generate-test <source-file>

## Instructions
1. Read the source file provided as an argument
2. Identify all exported functions, classes, and methods
3. Generate test cases covering:
   - Happy path for each function
   - Edge cases (null, undefined, empty values)
   - Error conditions
4. Use the project's existing test framework (detect from package.json)
5. Place the test file alongside the source with `.test` suffix
6. Run the tests to verify they pass

## Output
- Created test file path
- Number of test cases generated
- Test execution results
```

Create `skills/run-tests.md`:

```markdown
# Run Tests

Execute tests with coverage reporting and failure analysis.

## Usage
/run-tests [path] [--coverage] [--watch]

## Instructions
1. Detect the test framework from project configuration
2. Run tests for the specified path, or all tests if none given
3. If --coverage flag is present, include coverage reporting
4. On failure, analyze the failing tests and suggest fixes
5. Summarize results with pass/fail counts

## Output Format
- Total tests: X
- Passed: X
- Failed: X
- Coverage: X% (if requested)
- Failure analysis (if any failures)
```

#### Step 4: Build Hooks

Create `hooks/hooks.json`:

```json
{
  "hooks": {
    "PostToolUse": [
      {
        "matcher": "EditFile|CreateFile",
        "pattern": "\\.(test|spec)\\.(ts|js|tsx|jsx)$",
        "command": "npm test -- --bail --findRelatedTests $MATCHED_FILE",
        "description": "Auto-run related tests when test files are modified"
      }
    ]
  }
}
```

#### Step 5: Build Subagents

Create `subagents/test-reviewer.md`:

```markdown
# Test Reviewer

You are a test quality reviewer. When invoked, analyze test files for:

## Review Criteria
1. **Coverage completeness** - Are all code paths tested?
2. **Assertion quality** - Are assertions specific and meaningful?
3. **Test isolation** - Do tests depend on each other?
4. **Naming clarity** - Do test names describe the expected behavior?
5. **Edge cases** - Are boundary conditions covered?

## Output Format
Provide a quality score (1-10) and specific recommendations for improvement.
Prioritize recommendations by impact.
```

#### Step 6: Write the Manifest

Create `plugin.json`:

```json
{
  "name": "test-helper",
  "version": "1.0.0",
  "description": "Streamline test creation, execution, and quality review",
  "author": "Your Name",
  "license": "MIT",
  "keywords": ["testing", "test-generation", "coverage"],
  "claude_code_version": ">=1.0.0",
  "components": {
    "skills": [
      "skills/generate-test.md",
      "skills/run-tests.md"
    ],
    "subagents": [
      "subagents/test-reviewer.md"
    ],
    "hooks": "hooks/hooks.json"
  },
  "dependencies": {
    "node": ">=18.0.0"
  }
}
```

#### Step 7: Test Locally

Install the plugin in your own project to verify it works:

```bash
# Copy plugin to your Claude Code plugins directory
cp -r test-helper/ ~/.claude/plugins/test-helper/

# Or symlink for active development
ln -s $(pwd)/test-helper ~/.claude/plugins/test-helper

# Start Claude and test each skill
claude
> /generate-test src/utils.ts
> /run-tests --coverage
```

Verify each component:
- Skills appear and function correctly
- Hooks trigger on the expected events
- Subagents produce useful output

### Exercise

**Task**: Build a complete plugin

1. Choose a workflow from your daily development routine
2. Create the full directory structure with at least two skills
3. Add at least one hook or subagent
4. Write the plugin.json manifest
5. Install locally and test all components

### Verification
- [ ] Plugin directory follows standard structure
- [ ] At least two skills created and functional
- [ ] plugin.json is valid and complete
- [ ] Plugin installs and works locally
- [ ] All components tested individually

---

## Lesson 83: Plugin Distribution

### Objectives
- Package a plugin for distribution
- Publish to a registry or GitHub
- Write comprehensive documentation for users
- Establish a versioning strategy

### Content

#### Packaging Your Plugin

Before distribution, validate and clean the plugin:

```bash
# Validate plugin.json
cat test-helper/plugin.json | python -m json.tool

# Ensure no unnecessary files are included
# Create a .pluginignore file (similar to .gitignore)
cat > test-helper/.pluginignore << 'EOF'
node_modules/
.DS_Store
*.log
.env
test-output/
EOF
```

#### Publishing to GitHub

GitHub is the simplest distribution channel:

```bash
cd test-helper

# Initialize the repository
git init
git add .
git commit -m "Initial release v1.0.0"

# Create a tagged release
git tag v1.0.0
git remote add origin https://github.com/yourname/claude-plugin-test-helper.git
git push origin main --tags
```

Use GitHub Releases to provide release notes and changelogs:

```bash
gh release create v1.0.0 \
  --title "test-helper v1.0.0" \
  --notes "Initial release with generate-test and run-tests skills"
```

#### Writing a Comprehensive README

Your README is the primary documentation for users. Include these sections:

```markdown
# test-helper

Streamline test creation, execution, and quality review in Claude Code.

## Installation

    claude plugin install github:yourname/claude-plugin-test-helper

## Features

- **/generate-test** - Auto-generate test files from source code
- **/run-tests** - Execute tests with coverage and failure analysis
- **Auto-run hook** - Tests execute automatically when test files change
- **Test reviewer** - AI-powered test quality analysis

## Requirements

- Node.js >= 18.0.0
- A supported test framework (Jest, Vitest, Mocha)

## Configuration

Add to your project's `.claude/settings.json`:

    {
      "plugins": {
        "test-helper": {
          "framework": "jest",
          "coverageThreshold": 80
        }
      }
    }

## Usage Examples

### Generate tests for a file
    /generate-test src/utils/parser.ts

### Run all tests with coverage
    /run-tests --coverage

### Run tests for a specific directory
    /run-tests src/api/

## Changelog

### 1.0.0
- Initial release
- Skills: generate-test, run-tests
- Hook: auto-run on test file changes
- Subagent: test-reviewer
```

#### Installation Instructions

Provide multiple installation methods:

```bash
# From GitHub
claude plugin install github:yourname/claude-plugin-test-helper

# From a local directory
claude plugin install ./path/to/test-helper

# From a specific version
claude plugin install github:yourname/claude-plugin-test-helper@v1.2.0
```

#### Versioning Strategy

Follow Semantic Versioning (semver):

| Change Type | Version Bump | Example |
|-------------|-------------|---------|
| Bug fix, minor tweak | Patch: x.x.X | 1.0.0 -> 1.0.1 |
| New skill or feature | Minor: x.X.0 | 1.0.1 -> 1.1.0 |
| Breaking change to existing skills | Major: X.0.0 | 1.1.0 -> 2.0.0 |

Maintain a CHANGELOG.md following the Keep a Changelog format:

```markdown
## [1.1.0] - 2025-06-15
### Added
- New /test-coverage skill for detailed coverage reports
### Changed
- generate-test now detects framework automatically
### Fixed
- Hook pattern now matches .mjs and .cjs files
```

### Exercise

**Task**: Package and publish a plugin

1. Validate your plugin.json and all component files
2. Write a comprehensive README with installation and usage instructions
3. Initialize a git repository and create a tagged release
4. Create a GitHub release with release notes
5. Test the installation process from GitHub

### Verification
- [ ] plugin.json passes validation
- [ ] README covers installation, features, configuration, and usage
- [ ] Git repository has a tagged release
- [ ] Plugin installs successfully from the published source
- [ ] CHANGELOG.md documents the initial release

---

## Lesson 84: Plugin Marketplaces

### Objectives
- Discover plugins through community channels
- Install plugins from a marketplace or registry
- Evaluate plugin quality through ratings and reviews
- Contribute to the plugin community

### Content

#### Discovering Plugins

Find plugins through multiple channels:

```bash
# Search for plugins by keyword
claude plugin search "docker"

# Browse popular plugins
claude plugin list --sort=popular

# List plugins by category
claude plugin list --category=devops
```

Community resources for finding plugins:
- Official Claude Code plugin registry
- GitHub topic: `claude-code-plugin`
- Community forums and Discord channels
- Blog posts and curated lists

#### Installing from a Marketplace

```bash
# Install by plugin name (from registry)
claude plugin install devops-toolkit

# Install a specific version
claude plugin install devops-toolkit@2.1.0

# Install from GitHub directly
claude plugin install github:author/plugin-name

# View plugin details before installing
claude plugin info devops-toolkit
```

#### Evaluating Plugin Quality

Before installing a plugin, check these quality signals:

| Signal | What to Look For |
|--------|-----------------|
| **Version** | >= 1.0.0 indicates stability |
| **Last Updated** | Active maintenance (updated within 6 months) |
| **Documentation** | Complete README with examples |
| **License** | Open source license present |
| **Dependencies** | Minimal external requirements |
| **Stars/Downloads** | Community adoption |
| **Issues** | Responsive maintainer, low open bug count |

#### Rating and Reviewing Plugins

After using a plugin, contribute back to the community:

```bash
# Rate a plugin (1-5 stars)
claude plugin rate devops-toolkit 5

# Write a review
claude plugin review devops-toolkit --body "Excellent deploy skill.
Saved hours of manual work. The auto-rollback hook is especially useful."
```

Write helpful reviews that mention:
- Which features you used and how
- Any setup difficulties encountered
- How the plugin improved your workflow
- Suggestions for improvement

#### Community Contribution Guidelines

Contributing to the plugin ecosystem:

1. **Report bugs** - File issues with reproduction steps
2. **Submit improvements** - Open PRs with tests
3. **Share configurations** - Post useful config snippets
4. **Write tutorials** - Help others adopt plugins
5. **Maintain quality** - Follow standards in your own plugins

### Exercise

**Task**: Explore and install community plugins

1. Search for plugins relevant to your tech stack
2. Evaluate at least three plugins using the quality signals table
3. Install one plugin and test its features
4. Write a review for the plugin you tested
5. Identify a gap in the marketplace and note a plugin idea

### Verification
- [ ] Searched the marketplace and found relevant plugins
- [ ] Evaluated plugins using quality criteria
- [ ] Successfully installed and tested a plugin
- [ ] Wrote a constructive review
- [ ] Identified a potential new plugin to build

---

## Lesson 85: Enterprise Plugin Management

### Objectives
- Create and manage an approved plugin list for your organization
- Set up enterprise plugin distribution channels
- Implement a security review process for plugins
- Establish plugin governance policies

### Content

#### Approved Plugin Lists

Enterprises need control over which plugins developers can use. Define an allow-list in your organization-level settings:

```json
{
  "plugins": {
    "policy": "allowlist",
    "approved": [
      "devops-toolkit@>=2.0.0",
      "test-helper@^1.2.0",
      "security-scanner@latest",
      "internal:code-standards"
    ],
    "blocked": [
      "untrusted-plugin",
      "deprecated-tool"
    ],
    "requireApproval": true,
    "approvalContact": "platform-team@company.com"
  }
}
```

Distribute this configuration through your organization's settings management:

```bash
# Push approved list to all developers
# via organization-level .claude/settings.json in shared config repo
```

#### Enterprise Plugin Distribution

Host internal plugins through private channels:

```bash
# Install from private GitHub org
claude plugin install github:mycompany/internal-standards --token $GH_TOKEN

# Install from internal registry
claude plugin install registry:internal.company.com/code-standards

# Install from shared network path
claude plugin install file:///shared/plugins/code-standards
```

Set up an internal plugin registry:

```json
{
  "pluginRegistries": [
    {
      "name": "internal",
      "url": "https://plugins.internal.company.com",
      "auth": "bearer",
      "priority": 1
    },
    {
      "name": "public",
      "url": "https://plugins.claude.ai",
      "priority": 2
    }
  ]
}
```

#### Security Review Process

Establish a formal review process before approving plugins:

```
Step 1: Submission
  Developer submits plugin for review via internal tool or PR

Step 2: Automated Scan
  - Static analysis of all plugin files
  - Check for hardcoded secrets or credentials
  - Verify no network calls in hooks (unless expected)
  - Validate plugin.json schema

Step 3: Manual Review
  - Code review by security team
  - Verify MCP server permissions are minimal
  - Check hook commands for injection risks
  - Assess data exposure through subagents

Step 4: Testing
  - Install in sandboxed environment
  - Run all skills and verify behavior
  - Monitor network and filesystem access
  - Validate against test projects

Step 5: Approval
  - Sign off by security team
  - Add to approved list with version pin
  - Document any usage restrictions
```

Security review checklist:

| Check | Risk | What to Look For |
|-------|------|-----------------|
| Hook commands | High | Shell injection, data exfiltration |
| MCP server access | High | Excessive permissions, external calls |
| Skill instructions | Medium | Prompt injection, unsafe operations |
| Dependencies | Medium | Known vulnerabilities, unmaintained |
| Data handling | Medium | Logging sensitive info, temp files |
| Network access | High | Unexpected outbound connections |

#### Plugin Governance Policies

Document and enforce organizational plugin policies:

```markdown
# Plugin Governance Policy

## Installation
- Only approved plugins may be installed on company machines
- Developers may request new plugins through the platform team
- All plugins must pass security review before approval

## Updates
- Plugin updates reviewed within 5 business days of release
- Critical security patches fast-tracked within 24 hours
- Major version upgrades require re-review

## Internal Plugins
- Must follow company coding standards
- Require documentation and tests
- Owned by a specific team with on-call support
- Reviewed quarterly for continued relevance

## Incident Response
- Plugin-related incidents escalated to platform team
- Compromised plugins removed from approved list immediately
- Post-incident review within 48 hours
```

### Exercise

**Task**: Design an enterprise plugin management strategy

1. Draft an approved plugin list for your team or organization
2. Define a security review checklist with at least 8 items
3. Write a plugin governance policy covering installation, updates, and incidents
4. Set up an internal plugin distribution mechanism (private repo or registry)
5. Document the process for requesting a new plugin approval

### Verification
- [ ] Approved plugin list created with version constraints
- [ ] Security review checklist is thorough and actionable
- [ ] Governance policy covers the full plugin lifecycle
- [ ] Internal distribution channel configured
- [ ] Approval request process documented

---

## Lesson 86: Plugin Best Practices

### Objectives
- Apply proven design patterns when building plugins
- Implement versioning and backward compatibility strategies
- Develop a testing strategy for plugin components
- Follow documentation standards
- Avoid common pitfalls

### Content

#### Plugin Design Patterns

**Single Responsibility**: Each plugin should serve one clear purpose.

```
Good:  "docker-deploy" - manages Docker deployments
Bad:   "everything-tool" - deploys, tests, lints, and manages databases
```

**Progressive Disclosure**: Start with simple defaults, allow advanced configuration.

```json
{
  "config": {
    "target": {
      "type": "string",
      "default": "staging",
      "description": "Deployment target (staging, production)"
    },
    "advanced": {
      "timeout": {
        "type": "number",
        "default": 300,
        "description": "Deployment timeout in seconds"
      },
      "rollbackOnFailure": {
        "type": "boolean",
        "default": true,
        "description": "Auto-rollback on deployment failure"
      }
    }
  }
}
```

**Graceful Degradation**: Handle missing dependencies without crashing.

```markdown
## Skill: /deploy

### Instructions
1. Check if Docker is installed by running `docker --version`
2. If Docker is not available:
   - Inform the user that Docker is required
   - Provide installation instructions for their platform
   - Exit gracefully without errors
3. If Docker is available, proceed with deployment...
```

**Composability**: Design plugins that work well alongside other plugins.

```markdown
## Skill: /generate-api-types

### Instructions
- Output TypeScript types to a standard location (src/types/generated/)
- Use standard naming conventions so other plugins can discover the output
- Do not modify files owned by other plugins or tools
```

#### Versioning and Backward Compatibility

Follow these rules when releasing updates:

**Patch Release (1.0.x)** - No user action required:
```
- Fix typo in skill instructions
- Correct hook regex pattern
- Update documentation
```

**Minor Release (1.x.0)** - New features, fully backward compatible:
```
- Add new skill to the plugin
- Add optional configuration fields with defaults
- Support additional file patterns in hooks
```

**Major Release (x.0.0)** - Breaking changes, migration required:
```
- Rename or remove existing skills
- Change required configuration fields
- Alter hook behavior that users depend on
```

Provide migration guides for major releases:

```markdown
# Migrating from v1.x to v2.0

## Breaking Changes

### Skill renamed: /deploy -> /deploy-app
Update any scripts or documentation referencing the old name.

### Configuration change: `target` moved under `deploy` key
Before:
    { "target": "staging" }
After:
    { "deploy": { "target": "staging" } }
```

#### Testing Strategies

**Test each component type independently:**

```bash
# Test skills: invoke them manually and verify output
claude
> /generate-test src/utils.ts
# Verify: test file created, tests pass

# Test hooks: trigger the matching event and check behavior
# Modify a test file and confirm the hook runs

# Test subagents: invoke with sample input
# Verify output quality and format

# Test MCP servers: check connection and tool availability
claude
> Use the db-query tool to run SELECT 1
```

**Create a test project** for your plugin:

```
test-helper-tests/
├── fixtures/
│   ├── sample-source.ts
│   └── expected-test-output.ts
├── test-skills.sh
├── test-hooks.sh
└── README.md
```

`test-skills.sh`:
```bash
#!/bin/bash
set -e

echo "Testing /generate-test skill..."
cd fixtures
claude --print "/generate-test sample-source.ts"

if [ -f "sample-source.test.ts" ]; then
    echo "PASS: Test file generated"
else
    echo "FAIL: Test file not found"
    exit 1
fi

echo "Running generated tests..."
npx jest sample-source.test.ts
echo "PASS: Generated tests execute successfully"
```

**Regression testing**: Before each release, run through all skills and verify they still work as expected. Keep a checklist of manual verification steps.

#### Documentation Standards

Every plugin should include:

| Document | Purpose | Location |
|----------|---------|----------|
| README.md | Installation, features, usage examples | Root directory |
| CHANGELOG.md | Version history and migration notes | Root directory |
| Skill headers | Usage and behavior for each skill | Inside each skill .md |
| Config docs | All configuration options with defaults | README or separate doc |
| Troubleshooting | Common issues and solutions | README section |

Skill documentation template:

```markdown
# Skill Name

Brief one-line description.

## Usage
/skill-name <required-arg> [optional-arg] [--flag]

## Arguments
- `required-arg` - Description of what this argument does
- `optional-arg` - (Optional) Description with default value noted

## Flags
- `--flag` - What this flag controls

## Examples

### Basic usage
    /skill-name src/index.ts

### With options
    /skill-name src/index.ts --coverage

## Instructions
[Detailed instructions for Claude on how to execute the skill]
```

#### Common Pitfalls and How to Avoid Them

| Pitfall | Problem | Solution |
|---------|---------|----------|
| Overly broad scope | Plugin tries to do everything | Limit to one domain; split into multiple plugins |
| Hardcoded paths | Breaks on different OS or project layouts | Use relative paths and detect project structure |
| Missing error handling | Skills fail silently on bad input | Include explicit error cases in skill instructions |
| No version pinning | Users get breaking changes unexpectedly | Pin dependencies and declare claude_code_version |
| Excessive permissions | MCP servers request more access than needed | Follow principle of least privilege |
| Poor naming | Skills clash with built-in commands | Use unique, descriptive names; prefix with plugin name if needed |
| No testing | Bugs ship to users | Test locally and with a test project before release |
| Stale documentation | README does not match current behavior | Update docs as part of every release |

### Exercise

**Task**: Audit and improve an existing plugin

1. Select a plugin you have built or installed
2. Evaluate it against each best practice category in this lesson
3. Identify at least three areas for improvement
4. Apply the improvements (better docs, tests, error handling, etc.)
5. Write a CHANGELOG entry for the improvements
6. Bump the version appropriately following semver

### Verification
- [ ] Plugin follows single responsibility principle
- [ ] Versioning uses semver correctly
- [ ] At least one testing approach is implemented
- [ ] Documentation covers all skills, configuration, and troubleshooting
- [ ] Common pitfalls checked and addressed

---

## Section 13 Summary

### Key Takeaways

| Lesson | Core Concept | Key Skill |
|--------|-------------|-----------|
| 81 | Plugin Architecture | Understand the four components and plugin.json manifest |
| 82 | Creating Plugins | Build skills, hooks, subagents, and MCP servers in a plugin |
| 83 | Plugin Distribution | Package, version, and publish plugins to GitHub or a registry |
| 84 | Plugin Marketplaces | Discover, evaluate, install, and review community plugins |
| 85 | Enterprise Management | Govern plugins with approved lists, security reviews, and policies |
| 86 | Best Practices | Apply design patterns, testing, versioning, and documentation standards |

### Essential Commands

```bash
# Plugin lifecycle
claude plugin install <source>        # Install a plugin
claude plugin list                     # List installed plugins
claude plugin info <name>              # View plugin details
claude plugin update <name>            # Update a plugin
claude plugin remove <name>            # Uninstall a plugin

# Discovery
claude plugin search <keyword>         # Search for plugins
claude plugin list --sort=popular      # Browse popular plugins
```

### Plugin Quality Checklist

- [ ] plugin.json is valid with all required fields
- [ ] README covers installation, usage, and configuration
- [ ] CHANGELOG tracks version history
- [ ] All skills have clear instructions and examples
- [ ] Hooks use precise matcher patterns
- [ ] Dependencies are declared and version-pinned
- [ ] Security review completed (for enterprise use)
- [ ] Tested locally on a real project

### Next Steps
Proceed to Section 14: Advanced Workflows to master parallel development, CI/CD integration, and multi-repository operations.
