# Claude Code Troubleshooting Guide

Common issues and solutions for Claude Code users.

---

## 🚨 Installation Issues

### Problem: "Command not found: claude"

**Symptoms**:
```bash
$ claude
-bash: claude: command not found
```

**Solutions**:

1. **Check Installation**:
```bash
# macOS/Linux
which claude

# Windows
where claude
```

2. **Reinstall**:
```bash
# macOS
brew uninstall claude
brew install anthropics/tap/claude

# Windows
winget uninstall Anthropic.Claude
winget install Anthropic.Claude
```

3. **Add to PATH** (if installed but not in PATH):
```bash
# Check installation location
# Add to ~/.bashrc or ~/.zshrc:
export PATH="$PATH:/path/to/claude"
```

4. **Restart Terminal**:
```bash
# Close and reopen terminal
# Or source your profile:
source ~/.bashrc  # or ~/.zshrc
```

---

### Problem: "Permission denied" on Installation

**Symptoms**:
```bash
Error: Permission denied when writing to /usr/local/
```

**Solutions**:

1. **Use sudo** (macOS/Linux):
```bash
sudo brew install anthropics/tap/claude
```

2. **Fix Homebrew permissions**:
```bash
sudo chown -R $(whoami) /usr/local
```

3. **Use user installation** (Windows):
```powershell
# Install for current user only
winget install Anthropic.Claude --scope user
```

---

## 🔐 Authentication Issues

### Problem: "Authentication failed"

**Symptoms**:
```bash
$ claude auth login
Error: Authentication failed
```

**Solutions**:

1. **Clear existing auth**:
```bash
claude auth logout
claude auth login
```

2. **Check account status**:
- Verify you have an active Claude subscription
- Check at https://claude.ai/account

3. **Network issues**:
```bash
# Test connection
curl https://api.anthropic.com

# Try with different network
# (e.g., disable VPN, try different WiFi)
```

4. **Clear auth cache**:
```bash
rm -rf ~/.claude/auth
claude auth login
```

---

### Problem: "Session expired"

**Symptoms**:
```
Error: Your session has expired. Please login again.
```

**Solution**:
```bash
claude auth login
```

---

## 📁 File Operation Issues

### Problem: Edit Tool Fails - "String not found"

**Symptoms**:
```
Error: old_string not found in file
```

**Causes & Solutions**:

1. **Whitespace Mismatch**:
```javascript
// File has:
function test() {
  return true
}

// You're looking for (wrong - extra space):
function test()  {
  return true
}
```

**Solution**: Copy exact string from Read output
```
"Read the file first, then edit it"
```

2. **File Changed Since Last Read**:
```
"Re-read the file to see current state, then edit"
```

3. **Not Unique**:
```
// Error: String appears multiple times
```

**Solution**: Include more context
```javascript
// Instead of:
Old: return true

// Use:
Old:
function isValid() {
  return true
}
```

4. **Use replace_all** for global changes:
```
"Rename 'oldName' to 'newName' using replace_all"
```

---

### Problem: Write Tool Overwrites Important File

**Symptoms**:
File content completely replaced unexpectedly

**Prevention**:
- ✅ Always review Write operations before approving
- ✅ Use Edit for existing files
- ✅ Use version control (git)

**Recovery**:
```bash
# If using git:
git checkout filename

# If not using git:
# Restore from backup or rewrite
```

---

### Problem: Can't Read Binary Files

**Symptoms**:
```
Error: Cannot read binary file
```

**Explanation**:
Claude can't read binary files (.exe, .zip, .pdf, etc.) as text

**Solutions**:
- For PDFs: Claude can process them visually
- For images: Claude can see them
- For other binaries: Extract/convert to text first

---

## 🔍 Search Issues

### Problem: Glob Finds No Files

**Symptoms**:
```
Glob pattern **/*.js returned 0 files
```

**Solutions**:

1. **Check pattern**:
```bash
# Test pattern manually
find . -name "*.js"
```

2. **Check current directory**:
```
"What directory are we in?"
ls  # via Bash tool
```

3. **Adjust pattern**:
```
# Too specific:
src/api/routes/**/*.js

# More general:
**/*.js

# Explicit:
src/**/*.{js,ts}
```

---

### Problem: Grep Finds Too Many Results

**Symptoms**:
Grep returns hundreds of matches

**Solutions**:

1. **Use more specific pattern**:
```
# Instead of:
"Search for 'user'"

# Use:
"Search for 'function.*user' in src/**/*.js"
```

2. **Use file type filter**:
```
"Search for 'authenticate' in JavaScript files only"
```

3. **Limit results**:
```
"Search for 'TODO' and show first 10 results"
```

---

## ⚙️ Permission Issues

### Problem: Permission Denied on File Operations

**Symptoms**:
```
Error: EACCES: permission denied
```

**Solutions**:

1. **Check file permissions**:
```bash
ls -la filename
```

2. **Fix permissions**:
```bash
chmod 644 filename  # For files
chmod 755 dirname   # For directories
```

3. **Check ownership**:
```bash
sudo chown $(whoami) filename
```

4. **Run with appropriate permissions**:
```bash
# Only if necessary and safe:
sudo claude
```

---

### Problem: Can't Approve/Deny Permissions

**Symptoms**:
Permission prompt doesn't respond

**Solutions**:

1. **Check terminal**:
- Ensure terminal window is focused
- Try pressing keys: A, D, E

2. **Restart Claude**:
```bash
Ctrl+C
claude --resume
```

3. **Use auto-accept** (temporarily):
```bash
claude --auto-accept
# ⚠️ Be careful!
```

---

## 🌐 Network Issues

### Problem: "Connection timeout" or "Network error"

**Symptoms**:
```
Error: Request timeout
Error: Network request failed
```

**Solutions**:

1. **Check internet connection**:
```bash
ping 8.8.8.8
curl https://api.anthropic.com
```

2. **Check firewall/proxy**:
- Disable VPN temporarily
- Check corporate firewall settings
- Configure proxy if needed

3. **Increase timeout**:
```bash
claude --timeout 60000  # 60 seconds
```

4. **Retry**:
```
"Let's try that again"
```

---

## 🧠 Context & Token Issues

### Problem: "Context length exceeded"

**Symptoms**:
```
Error: Maximum context length exceeded
```

**Solutions**:

1. **Compact context**:
```
/compact
```

2. **Start new session**:
```
/exit
claude --session "new-session-name"
```

3. **Read less at once**:
```
# Instead of:
"Read all files in src/"

# Use:
"Read just the key files: app.js, config.js, routes.js"
```

4. **Use line ranges**:
```
"Read app.js lines 1-100"
```

---

### Problem: Claude "Forgets" Earlier Context

**Symptoms**:
Claude asks about things you already discussed

**Solutions**:

1. **Reference earlier context**:
```
"As we discussed earlier, the auth system uses JWT..."
```

2. **Summarize key points**:
```
"To summarize what we've done:
1. Created user model
2. Added authentication
3. Now we're working on authorization"
```

3. **Resume session** instead of starting new:
```bash
claude --resume
```

4. **Use CLAUDE.md** for persistent context:
```markdown
# Project Context

Current work: Adding payment integration
Decisions made:
- Using Stripe API
- Webhook-based notifications
```

---

## 🐛 Git Integration Issues

### Problem: Git Commands Fail

**Symptoms**:
```
Error: git: command not found
Error: fatal: not a git repository
```

**Solutions**:

1. **Install git**:
```bash
# macOS
brew install git

# Ubuntu/Debian
sudo apt-get install git

# Windows
winget install Git.Git
```

2. **Initialize repository**:
```bash
git init
```

3. **Check git config**:
```bash
git config --global user.name "Your Name"
git config --global user.email "your@email.com"
```

---

### Problem: Can't Create Commits

**Symptoms**:
```
Error: No user.name configured
Error: No user.email configured
```

**Solution**:
```bash
git config user.name "Your Name"
git config user.email "your@email.com"
```

---

### Problem: Pre-commit Hooks Block Commits

**Symptoms**:
```
husky > pre-commit hook failed
lint-staged failed
```

**Solutions**:

1. **Fix linting issues**:
```bash
npm run lint:fix
```

2. **Fix formatting**:
```bash
npm run format
```

3. **Bypass hooks** (if absolutely necessary):
```bash
git commit --no-verify
# ⚠️ Use sparingly!
```

---

## 🧪 Testing Issues

### Problem: Tests Fail After Claude's Changes

**Symptoms**:
Tests were passing, now failing after edits

**Solutions**:

1. **Review changes**:
```
"Show me what you changed"
git diff
```

2. **Run specific test**:
```bash
npm test -- specific-test-file.test.js
```

3. **Revert and re-approach**:
```bash
git checkout filename
```
```
"Let's try a different approach. The tests are failing because..."
```

4. **Update tests** (if behavior intentionally changed):
```
"Update the tests to match the new behavior"
```

---

## ⚡ Performance Issues

### Problem: Claude is Slow

**Symptoms**:
Long wait times for responses

**Solutions**:

1. **Use Haiku for simple tasks**:
```
/model haiku
```

2. **Reduce context**:
```
/compact
```

3. **Break into smaller requests**:
```
# Instead of:
"Analyze, refactor, test, and document this entire codebase"

# Use:
"First, analyze the architecture"
[wait]
"Now, let's refactor the user service"
[wait]
"Add tests for the user service"
```

4. **Use parallel operations**:
```
"Read these 5 files in parallel"
```

---

### Problem: Bash Commands Timeout

**Symptoms**:
```
Error: Command timed out after 120000ms
```

**Solutions**:

1. **Run in background**:
```
"Run the build in background"
# Or use Ctrl+B
```

2. **Use streaming commands**:
```bash
# Instead of waiting for completion:
npm test

# Stream output:
npm test 2>&1 | tee test-output.log
```

3. **Increase timeout**:
```
"Run this command with 10-minute timeout"
```

---

## 🔧 IDE Integration Issues

### Problem: VS Code Extension Not Working

**Symptoms**:
Claude panel doesn't appear or is unresponsive

**Solutions**:

1. **Reload VS Code**:
```
Cmd/Ctrl + Shift + P → "Reload Window"
```

2. **Reinstall extension**:
- Uninstall Claude extension
- Restart VS Code
- Reinstall extension

3. **Check authentication**:
```bash
# In terminal:
claude auth whoami
```

4. **Check logs**:
```
View → Output → Claude Code
```

---

### Problem: @-mentions Don't Work

**Symptoms**:
@-mentions don't trigger autocomplete

**Solutions**:

1. **Update extension**:
Check for extension updates

2. **Check file is saved**:
@-mentions work better with saved files

3. **Use full path**:
```
@src/api/users.js
```

---

## 🎨 Skill & Hook Issues

### Problem: Custom Skill Not Found

**Symptoms**:
```
Error: Skill 'my-skill' not found
```

**Solutions**:

1. **Check skill location**:
```bash
ls ~/.claude/skills/my-skill/SKILL.md
```

2. **Check skill frontmatter**:
```yaml
---
name: my-skill  # Must match
commands:
  - my-command
---
```

3. **Restart Claude**:
```bash
/exit
claude
```

---

### Problem: Hook Not Executing

**Symptoms**:
Hook should run but doesn't

**Solutions**:

1. **Check hook configuration**:
```bash
cat .claude/settings.json
```

2. **Verify match pattern**:
```json
{
  "match": {
    "tool": "Write",
    "file_path": "**/*.js"  // Make sure pattern is correct
  }
}
```

3. **Test command manually**:
```bash
# Try running the hook command manually
prettier --write test-file.js
```

4. **Check for errors**:
```bash
# Enable verbose mode
claude --verbose
```

---

## 💾 Data & Settings Issues

### Problem: Settings Not Persisting

**Symptoms**:
Settings reset after closing Claude

**Solutions**:

1. **Check settings file**:
```bash
cat ~/.claude/settings.json
```

2. **Fix JSON syntax**:
```bash
# Validate JSON
jq . ~/.claude/settings.json
```

3. **Use correct scope**:
- User settings: `~/.claude/settings.json`
- Project settings: `.claude/settings.json`
- Local settings: `.claude/local.settings.json`

---

### Problem: Lost Session History

**Symptoms**:
Previous sessions not in /history

**Solutions**:

1. **Check session storage**:
```bash
ls ~/.claude/sessions/
```

2. **Session may have been unnamed**:
```
/history  # Shows all sessions, including unnamed
```

3. **Search by date**:
```
"Show me sessions from last week"
```

---

## 🆘 When Nothing Works

### Nuclear Options

1. **Clear all cache**:
```bash
rm -rf ~/.claude/cache/
```

2. **Reset settings**:
```bash
mv ~/.claude/settings.json ~/.claude/settings.json.backup
# Restart Claude - creates new default settings
```

3. **Reinstall Claude**:
```bash
# macOS
brew uninstall claude
brew install anthropics/tap/claude

# Windows
winget uninstall Anthropic.Claude
winget install Anthropic.Claude

# Authenticate again
claude auth login
```

4. **Fresh start**:
```bash
# Backup first!
mv ~/.claude ~/.claude.backup
claude auth login
```

---

## 📞 Getting More Help

### Official Channels
- **Documentation**: https://code.claude.com/docs
- **GitHub Issues**: Report bugs
- **Community**: Discord/Forums

### Debug Information to Provide

When asking for help, include:

```bash
# Version
claude --version

# OS
uname -a  # macOS/Linux
systeminfo  # Windows

# Last error
# Copy the full error message

# Steps to reproduce
1. Did X
2. Then Y
3. Got error Z

# Expected vs actual behavior
# What you expected to happen
# What actually happened
```

---

## 💡 Prevention Tips

### Best Practices to Avoid Issues

1. **Always use git**:
```bash
git init
# Make commits frequently
```

2. **Review before approving**:
- Read diffs carefully
- Don't auto-approve everything

3. **Start simple**:
- Test with small examples first
- Build complexity gradually

4. **Keep Claude updated**:
```bash
brew upgrade claude  # macOS
winget upgrade Anthropic.Claude  # Windows
```

5. **Backup important work**:
- Use version control
- Keep backups of critical files

6. **Test in safe environment first**:
- Try new features on test projects
- Use branches for experiments

---

**Most issues have simple solutions. Don't hesitate to ask for help!**

*Remember: When in doubt, read the error message carefully - it usually tells you what's wrong!*
