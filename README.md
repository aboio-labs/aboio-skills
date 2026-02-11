# Aboio Agent Skills

Agent Skills to help developers using AI agents with Gleam and Postgres. Agent Skills are folders of instructions, scripts, and resources that agents like Claude Code, Cursor, Github Copilot, etc. can discover and use to do things more accurately and efficiently.

The skills in this repo follow the [Agent Skills](https://agentskills.io/) format.

## Disclaimer & Safety

**Experimental Nature**: These skills are experimental and tailored to Aboio's specific software architecture and use cases. They do not aim to be exhaustive documentation for the entire Gleam language or Postgres ecosystem.

**Review Before Use**: We strongly recommend reading all `.md` files within a skill before adding it to your environment. Understanding the instructions and context injected into your agent is crucial to prevent unintended behavior or prompt injection risks.

**Contributions**: If you find anti-patterns, better approaches, or want to add support for libraries not currently covered, we welcome your contributions! Please see [CONTRIBUTING.md](./CONTRIBUTING.md) for details.

## Installation

```bash
# Example installation (if using a skills CLI)
npx skills add aboio/agent-skills
```

## Available Skills

### [gleam](./skills/gleam)

Comprehensive Gleam skill covering language fundamentals, backend (OTP, Squirrel, HTTP, auth), frontend (Lustre 5.x, rsvp, modem, plinth), and library design.

### [pg-gleam](./skills/pg-gleam)

Postgres performance optimization and best practices for a Gleam + Squirrel + POG + Cigogne stack.

### [code-simplifier](./skills/code-simplifier)

Expert Gleam code simplification specialist. Refines recently modified code for clarity, consistency, and maintainability without changing behavior.

### [observability-logging-expert](./skills/observability-logging-expert)

Senior observability engineer specializing in Gleam/Erlang/OTP logging. Finds unlogged error branches and adds structured logging with context.

## Skill Structure

Each skill follows the [Agent Skills Open Standard](https://agentskills.io/):

- `SKILL.md` - Required skill manifest with frontmatter (name, description, metadata)
- `references/` - Individual reference files

## License

MIT
