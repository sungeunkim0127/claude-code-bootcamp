# Section 16: Enterprise & Security (Lessons 97-100)

## Overview
Deploy Claude Code across organizations with proper governance, security, and compliance.

---

## Lesson 97: Enterprise Configuration

### Objectives
- Set up enterprise managed settings
- Deploy centralized configurations
- Control permissions organization-wide
- Monitor usage and costs

### Enterprise Settings Hierarchy

```
Managed (Enterprise) ← Highest priority
    ↓
Project Settings
    ↓
User Settings
    ↓
Defaults ← Lowest priority
```

### Managed Configuration

Enterprise admins create managed settings:

```json
// managed-settings.json (deployed centrally)
{
  "enterprise": {
    "name": "Acme Corp",
    "version": "1.0.0"
  },
  "permissions": {
    "enforce": {
      "deny": [
        "Bash(rm -rf *)",
        "Bash(curl * | bash)",
        "Write:.env*"
      ],
      "requireApproval": [
        "Bash(npm publish*)",
        "Bash(git push --force*)"
      ]
    }
  },
  "models": {
    "default": "sonnet",
    "allowed": ["haiku", "sonnet"],
    "restricted": ["opus"]  // Requires special approval
  },
  "plugins": {
    "required": ["@acme/claude-plugin"],
    "allowed": ["@approved/*"],
    "blocked": ["*"]
  }
}
```

### Deployment Options

**1. MDM (Mobile Device Management):**
```
Deploy to ~/.claude/managed-settings.json
```

**2. Environment Variable:**
```bash
CLAUDE_MANAGED_SETTINGS_URL=https://config.company.com/claude
```

**3. Package Manager:**
```bash
npm install @company/claude-config -g
```

### Usage Monitoring

```json
{
  "telemetry": {
    "enabled": true,
    "endpoint": "https://analytics.company.com/claude",
    "includeUserActivity": false,  // Privacy
    "includeCosts": true,
    "includeErrors": true
  }
}
```

### Exercise 97: Enterprise Setup Plan

**Task**: Design enterprise deployment

1. List configuration requirements
2. Design permission policies
3. Plan deployment method
4. Create monitoring strategy
5. Document rollout plan

**Verification**:
- [ ] Requirements documented
- [ ] Policies defined
- [ ] Deployment planned
- [ ] Monitoring designed

---

## Lesson 98: Security Best Practices

### Objectives
- Audit tool permissions comprehensively
- Protect sensitive data
- Implement security hooks
- Secure credentials and secrets

### Permission Audit

Regular audit checklist:
```
□ Review allowed Bash commands
□ Check file path restrictions
□ Verify MCP server access
□ Audit plugin permissions
□ Review hook commands
```

### Dangerous Patterns to Block

```json
{
  "permissions": {
    "deny": [
      "Bash(rm -rf /)",
      "Bash(curl * | sh)",
      "Bash(eval *)",
      "Bash(chmod 777 *)",
      "Write:*.pem",
      "Write:*.key",
      "Read:.env.production",
      "Bash(*password*)",
      "Bash(*secret*)"
    ]
  }
}
```

### Sensitive Data Protection

**Never commit secrets:**
```
.gitignore:
  .env*
  *.key
  *.pem
  credentials.json
  secrets/
```

**Security hook:**
```json
{
  "hooks": {
    "PreToolUse": [
      {
        "matcher": "Bash:git (add|commit)",
        "command": "! git diff --cached | grep -E '(password|secret|api.?key|token)\\s*[:=]'"
      }
    ]
  }
}
```

### Credential Management

**Best practices:**
1. Use environment variables
2. Never hardcode credentials
3. Use secret managers (Vault, AWS Secrets)
4. Rotate credentials regularly

**Detection hook:**
```json
{
  "hooks": {
    "PreToolUse": [
      {
        "matcher": "Write:.*\\.(js|ts|py)$",
        "command": "! echo '$CONTENT' | grep -E 'sk-[a-zA-Z0-9]{48}'"
      }
    ]
  }
}
```

### Exercise 98: Security Audit

**Task**: Perform security audit

1. Audit current permissions
2. Identify risky configurations
3. Implement security hooks
4. Test protective measures
5. Document security policies

**Verification**:
- [ ] Permissions audited
- [ ] Risks identified
- [ ] Protections implemented
- [ ] Policies documented

---

## Lesson 99: Compliance & Privacy

### Objectives
- Understand data handling policies
- Configure privacy settings
- Meet regulatory requirements
- Implement audit trails

### Data Handling

**What Claude sees:**
- Code in files you share
- Conversation history
- Tool outputs

**What Claude doesn't see:**
- Files you don't reference
- Other users' sessions
- System outside permissions

### Privacy Configuration

```json
{
  "privacy": {
    "telemetryLevel": "minimal",  // minimal, standard, full
    "excludePaths": [
      "**/secrets/**",
      "**/.env*",
      "**/credentials/**"
    ],
    "redactPatterns": [
      "\\b[A-Za-z0-9._%+-]+@[A-Za-z0-9.-]+\\.[A-Z|a-z]{2,}\\b",  // emails
      "\\b\\d{3}-\\d{2}-\\d{4}\\b"  // SSN pattern
    ]
  }
}
```

### Compliance Requirements

**SOC 2:**
- Audit logging enabled
- Access controls documented
- Change tracking active

**GDPR:**
- Data minimization
- Consent documentation
- Deletion capabilities

**HIPAA:**
- PHI protection
- Access logging
- Encryption in transit

### Audit Trail

```json
{
  "audit": {
    "enabled": true,
    "logPath": "/var/log/claude-audit/",
    "retention": "90d",
    "include": [
      "fileAccess",
      "bashCommands",
      "permissions"
    ]
  }
}
```

### Exercise 99: Compliance Implementation

**Task**: Implement compliance controls

1. Identify applicable regulations
2. Configure privacy settings
3. Enable audit logging
4. Test data handling
5. Document compliance

**Verification**:
- [ ] Regulations identified
- [ ] Privacy configured
- [ ] Auditing enabled
- [ ] Compliance documented

---

## Lesson 100: Cloud & Remote Execution

### Objectives
- Use Claude Code on web
- Offload heavy tasks to cloud
- Sync sessions between local and remote
- Choose optimal execution location

### Claude Code Web

Access via browser:
- https://claude.ai/code (example)
- Runs in cloud environment
- No local installation needed

### When to Use Cloud

**Cloud better for:**
- Heavy computation
- Large codebase analysis
- Team collaboration
- Consistent environment

**Local better for:**
- Sensitive code
- Fast iteration
- Offline work
- Custom tools

### Session Sync

```bash
# Push local session to cloud
claude push --session "feature-work"

# Pull cloud session locally
claude pull --session "team-review"

# Sync automatically
claude --sync
```

### Hybrid Workflow

```
Local Development:
  - Write code
  - Quick tests
  - Sensitive work

Cloud Processing:
  - Full test suites
  - Large analysis
  - Team reviews
  - CI/CD integration
```

### Remote Execution

```json
{
  "remote": {
    "enabled": true,
    "defaultExecutor": "cloud",
    "fallback": "local",
    "heavyTaskThreshold": "10m"  // Tasks over 10 min go to cloud
  }
}
```

### Exercise 100: Hybrid Setup

**Task**: Configure hybrid workflow

1. Set up cloud access
2. Configure session sync
3. Define local vs cloud policies
4. Test hybrid workflow
5. Optimize based on patterns

**Verification**:
- [ ] Cloud access configured
- [ ] Sync working
- [ ] Policies defined
- [ ] Workflow optimized

---

## Section 16 Summary

### Key Takeaways

1. **Enterprise settings** override individual configurations
2. **Security** requires proactive measures and auditing
3. **Compliance** needs documentation and controls
4. **Hybrid** approaches maximize flexibility

### Enterprise Checklist

- [ ] Managed settings deployed
- [ ] Permission policies defined
- [ ] Security hooks active
- [ ] Audit logging enabled
- [ ] Compliance documented
- [ ] Training materials ready
- [ ] Support process established

### Security Layers

```
1. Enterprise managed settings
2. Permission deny lists
3. Security hooks
4. Audit logging
5. Credential protection
6. Access monitoring
```

---

## Expert Level Complete!

Congratulations! You've completed the entire Claude Code Bootcamp.

### You Can Now:

**Beginner Skills:**
- Use all core file tools
- Work with Git effectively
- Configure Claude for projects

**Intermediate Skills:**
- Create custom skills
- Build automation hooks
- Use subagents for complex tasks

**Advanced Skills:**
- Connect MCP servers
- Build custom subagents
- Integrate with IDEs

**Expert Skills:**
- Deploy enterprise-wide
- Ensure security and compliance
- Optimize performance
- Build advanced workflows

### Next Steps

1. **Practice**: Apply skills to real projects
2. **Share**: Help team members learn
3. **Contribute**: Create skills and plugins
4. **Optimize**: Continuously improve workflow
5. **Stay Current**: Follow Claude Code updates

### Resources

- Official Documentation
- Community Forums
- Plugin Marketplace
- Update Announcements

**Welcome to Claude Code mastery!**
