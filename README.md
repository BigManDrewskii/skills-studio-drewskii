# Claude Skills Studio

A collection of custom skills for Claude Code, extending its capabilities with specialized workflows and best practices.

## What Are Claude Skills?

Claude Skills are reusable instruction sets that give Claude specialized knowledge and workflows for specific tasks. Skills act like plugins that can be invoked via slash commands (e.g., `/systematic-debugging`, `/code-reviewer`).

This repository contains my personal collection of skills for claude code.

## Installation

To use these skills in your Claude Code setup:

1. Clone this repository:
```bash
git clone https://github.com/BigManDrewskii/skills-studio-drewskii.git
```

2. Copy the skills directory to your Claude configuration:
```bash
cp -r skills-studio-drewskii/.claude ~/.claude/
```

Or if you prefer to symlink:
```bash
ln -s $(pwd)/skills-studio-drewskii/.claude/skills ~/.claude/skills
```

3. Restart Claude Code

## Available Skills

### Systematic Debugging

Structured debugging workflow that prevents shotgun debugging by forcing you to understand the problem before proposing solutions.

**Usage:** `/systematic-debugging [describe your bug, error, or unexpected behavior]`

**Triggers:**
- Encountering any bug or error
- Tests failing without clear cause
- User reports unexpected behavior
- About to make "just a quick fix"

**Key Features:**
- Observe → Define → Isolate → Hypothesize → Test → Confirm workflow
- Forces generation of 3+ hypotheses before fixing
- Prevents random code changes without understanding

## Skill Structure

Each skill follows this structure:

```
.claude/skills/
└── [skill-name]/
    └── skill.md
```

The `skill.md` file contains:
- Skill name and description
- Usage examples
- Motivation/problem statement
- Detailed instructions
- Examples (good vs bad approaches)
- When to use (triggers)
- Anti-patterns to avoid
- Quick reference

## Contributing

Got a skill idea or improvement? Here's how:

### Adding a New Skill

1. Create a new directory under `.claude/skills/[skill-name]/` (use kebab-case)
2. Create a `skill.md` file following the [skill structure](#skill-structure)
3. Test your skill by invoking it in Claude Code
4. Open a pull request with your skill

### Skill Guidelines

- **Single responsibility**: Each skill should do one thing well
- **Practical over theoretical**: Focus on real-world workflows
- **Clear triggers**: Define when the skill should be invoked
- **Examples**: Show concrete before/after scenarios
- **Anti-patterns**: Always pair wrong with right approaches
- **Self-contained**: Skills should work independently

Review [AGENTS.md](AGENTS.md) for detailed style guidelines.

## License

MIT License - feel free to use, modify, and distribute these skills.