# AGENTS.md

This is a Claude Skills Studio repository for custom agent skills.

## Repository Structure

```
skills-studio-drewskii/
└── .claude/
    └── skills/
        └── [skill-name]/
            └── skill.md
```

## Adding Skills

1. Create new directory under `.claude/skills/[skill-name]/`
2. Add `skill.md` file with your skill definition
3. Skill filename must be exactly `skill.md`
4. Directory name should be kebab-case (e.g., `systematic-debugging`, `code-reviewer`)

## Skill File Structure

```markdown
# Skill Name

One-line description of what this skill does.

## Usage
[How to invoke the skill with example command]

## Context/Motivation
[Why this skill exists, what problem it solves]

## Detailed Instructions
[Core instructions - the meat of the skill]

## Examples
[Concrete examples showing good vs bad approaches]

## When to Use
[Clear triggers for when this skill should be invoked]

## Anti-Patterns
[What NOT to do, common mistakes]

## Quick Reference
[Cheat sheet format for fast lookup]
```

## Markdown Style Guidelines

### Headers
- H1 (`#`) for skill name only
- H2 (`##`) for major sections
- H3 (`###`) for subsections
- Keep header hierarchy logical

### Code Blocks
- Use ``` for shell commands
- Specify language when relevant: ```typescript, ```bash, ```python
- Keep examples minimal but complete

### Emphasis
- **Bold** for key terms, important warnings, and emphasis
- Use sparingly - too much bold loses impact
- Don't use ALL CAPS except for section headings like ANTI-PATTERNS

### Lists
- Numbered lists for sequential steps
- Bullet points for non-ordered items
- Nested lists for hierarchy (max 2 levels)

### Tone
- Direct and practical
- Avoid fluff and filler
- Be prescriptive when helpful
- Use "you" and "we" naturally

### Tables
- Use for comparisons (before/after, do/don't)
- Keep column headers concise
- Use simple tables - avoid complex structures

### Checkboxes
- Use for quick reference checklists: `□` or `- [ ]`
- Group related items together

### Line Length
- Wrap around 80-100 characters for readability
- Code blocks can be longer

## Content Guidelines

### Examples
- Show concrete before/after when possible
- Use realistic scenarios, not toy examples
- Explain why each approach is good or bad

### Anti-Patterns
- Always pair with correct approach
- Be specific about what makes it wrong
- Focus on root causes, not symptoms

### When to Use Section
- Clear triggers (phrases, situations, error types)
- Red flags to watch for
- Edge cases

## Build/Testing

No build system, tests, or linting. Skills are validated by:
1. Manual invocation in Claude
2. Verifying the skill activates on intended triggers
3. Checking that instructions are followed correctly

## Notes

- Skills are invoked via slash commands: `/[skill-name]`
- Each skill should be self-contained
- Reference other skills if relevant, but avoid circular dependencies
- Keep skills focused on single responsibility
