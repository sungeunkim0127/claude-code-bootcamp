# Frequently Asked Questions (FAQ)

## Getting Started

**Q: Do I need programming experience?**
A: Basic programming knowledge helps, but the bootcamp starts from the beginning. If you can use a terminal and edit code, you're ready.

**Q: How long does it take to complete?**
A: Depends on your path:
- Quick Start: 2 weeks part-time (30-40 hours)
- Professional: 4 weeks intensive (80-100 hours)
- Expert: 6-8 weeks intensive (160-200 hours)

**Q: Can I learn part-time?**
A: Absolutely! Adjust the schedule to your pace. Consistency matters more than speed.

**Q: Which path should I choose?**
A:
- New to Claude Code? Start with Quick Start or Professional
- Experienced developer? Professional or Expert
- Team lead? Professional path, then focus on Enterprise sections
- DevOps? Follow the specialized DevOps path

---

## Technical Questions

**Q: What's the difference between Read, Write, and Edit?**
A:
- **Read**: View file contents
- **Write**: Create new files or completely replace existing
- **Edit**: Make precise changes to existing files
Always use Edit for modifications, Write only for new files.

**Q: When should I use plan mode?**
A: Use plan mode for:
- Complex refactoring
- Large features
- Architectural changes
- When you want to review approach before execution

**Q: What are skills vs hooks vs subagents?**
A:
- **Skills**: Manually invoked workflows (`/skill-name`)
- **Hooks**: Automatic actions on events (auto-format after write)
- **Subagents**: Full agents with custom behavior (specialized AI assistants)

**Q: How do I know which tool Claude is using?**
A: Claude shows tool calls in conversation. You'll see `[Tool: Read - file.js]` etc.

---

## Learning & Progress

**Q: Can I skip lessons?**
A: You can, but each lesson builds on previous ones. Skipping may create gaps in understanding.

**Q: How do I know I'm ready to move to the next level?**
A: Complete the verification checklist at the end of each section. If you can do everything confidently, move forward.

**Q: What if I get stuck on an exercise?**
A:
1. Re-read the lesson objectives
2. Check TROUBLESHOOTING.md
3. Try a simpler version first
4. Ask Claude for help
5. Move on and come back later

**Q: Should I do all 20 projects?**
A: Do at least one per level. More practice = better mastery.

---

## Best Practices

**Q: Should I use auto-accept mode?**
A: Not while learning! Use normal mode to see what Claude does. After you're comfortable, auto-accept is fine for trusted operations.

**Q: How often should I use Claude Code?**
A: Daily if possible. Regular practice builds muscle memory.

**Q: Should I create CLAUDE.md for every project?**
A: Yes! It saves time and improves Claude's effectiveness.

**Q: When should I create custom skills?**
A: When you do the same task 3+ times. If it's repetitive, make it a skill.

---

## Troubleshooting

**Q: Edit tool keeps failing with "string not found"**
A: Most common cause is whitespace mismatch. Copy the exact string from Read output, including all spaces and tabs.

**Q: Claude seems to forget context**
A:
- Use `/compact` to compress conversation
- Reference earlier decisions explicitly
- Use CLAUDE.md for persistent context
- Start new session for new topics

**Q: How do I undo Claude's changes?**
A: If using git: `git checkout filename`
If not using git: You can't undo easily - always use git!

**Q: Claude is slow**
A:
- Use Haiku model for simple tasks
- `/compact` to reduce context
- Break large requests into smaller ones
- Request parallel operations

---

## Advanced Topics

**Q: How do I build a custom MCP server?**
A: See Lesson 59 and Project 15. Requires TypeScript/JavaScript knowledge and understanding of the MCP protocol.

**Q: Can I use Claude Code in CI/CD?**
A: Yes! See Lesson 89 and Project 18 for GitHub Actions integration.

**Q: How do I share skills with my team?**
A:
1. Package skills directory
2. Share via git repository
3. Team copies to `~/.claude/skills/` or project `.claude/skills/`
4. Or create enterprise plugin

**Q: What's the difference between user and project skills?**
A:
- **User** (`~/.claude/skills/`): Available in all projects
- **Project** (`.claude/skills/`): Only in this project, shared via git

---

## Enterprise & Teams

**Q: How do we deploy Claude Code company-wide?**
A: See Section 16 (Lessons 97-100) and Project 20 for complete enterprise deployment guide.

**Q: Can we enforce coding standards?**
A: Yes! Use hooks to auto-format, lint, and enforce rules. See Section 8 (Lessons 43-50).

**Q: How do we measure ROI?**
A: Track:
- Velocity (features/sprint)
- Quality (bugs, code review time)
- Time savings (hours/week)
- Team satisfaction
See CASE-STUDIES.md for examples.

**Q: What about security and compliance?**
A: See Lessons 98-99 for security best practices and compliance guidelines.

---

## Community & Support

**Q: Where can I get help?**
A:
1. TROUBLESHOOTING.md (30+ solutions)
2. Official documentation
3. Community forums
4. GitHub issues

**Q: Can I contribute to this bootcamp?**
A: Yes! Improvements, additional lessons, and examples are welcome.

**Q: How do I share my success story?**
A: Document your experience and share with the community. See CASE-STUDIES.md for format.

---

## Certification

**Q: Are there official certifications?**
A: This bootcamp provides a certification framework (see ASSESSMENT-TESTS.md), but it's self-administered.

**Q: What do I get when certified?**
A: Knowledge and skills! Add it to your resume/portfolio. The real value is productivity gains.

---

**Still have questions? Check TROUBLESHOOTING.md or ask in the community!**
