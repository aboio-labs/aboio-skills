# Contributing to Aboio Agent Skills

We welcome contributions to add new skills or improve existing ones!

## Creating a New Skill

1. Create a new directory in `skills/` with your skill name (kebab-case).
2. Add a `SKILL.md` file with the following frontmatter:

```markdown
---
name: my-skill-name
description: A short description of what the skill does.
---

# My Skill Name

Detailed instructions and documentation...
```

3. Add your reference files in a `references/` subdirectory.
4. Update `registry.json` to include your new skill.

## Updating Existing Skills

- improvements to `SKILL.md` prompts
- updates to `references/` documentation
- bug fixes in code examples

## Style Guide

- Use clear, concise language.
- Provide examples for complex concepts.
- Organize references logically.
