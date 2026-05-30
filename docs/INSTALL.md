# Installation Guide

## Claude Code

### Quick Install

```bash
# From your project root
mkdir -p .claude/skills/test
curl -o .claude/skills/test/SKILL.md https://raw.githubusercontent.com/Evan1108-Coder/code-testing-skill-claude-codex/main/skill/SKILL.md
```

### Manual Install

1. Download `skill/SKILL.md` from this repository
2. Place it at `.claude/skills/test/SKILL.md` in your project
3. The skill is now available — invoke with `/test`

### Verify Installation

Run `/test` in Claude Code. You should see it activate and ask what you'd like to test.

## Codex (OpenAI)

### Option 1: Custom Instructions

1. Copy the full contents of `skill/SKILL.md` (everything after the frontmatter `---` block)
2. Paste it into your Codex agent's custom instructions or system prompt

### Option 2: Reference File

1. Add `skill/SKILL.md` to your project repository
2. Reference it in your Codex configuration as an instruction file

## Configuration

No configuration needed. The skill automatically:

- Detects your test framework from project files
- Matches your existing test patterns and conventions
- Uses the correct test runner commands

## Updating

Pull the latest version:

```bash
curl -o .claude/skills/test/SKILL.md https://raw.githubusercontent.com/Evan1108-Coder/code-testing-skill-claude-codex/main/skill/SKILL.md
```

## Troubleshooting

**Skill not appearing in Claude Code?**
- Ensure the file is at exactly `.claude/skills/test/SKILL.md`
- Check that the frontmatter (the `---` block at the top) is intact

**Tests not running?**
- Make sure your test framework is installed (`npm install`, `pip install pytest`, etc.)
- Check that the test runner command works manually first
