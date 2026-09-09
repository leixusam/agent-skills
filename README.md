# Agent Skills

Reusable skills for autonomous coding agents.

## Available skill

### Autopilot

Runs a user-approved work session autonomously while keeping durable state in an append-only backbone document. It is designed for long-running work that may span context compactions or unattended hours.

The skill requires the user to approve a verifiable goal, evaluation criteria, scope and authority boundaries, and explicit resource ceilings before the run begins.

Source: `skills/autopilot/SKILL.md`

## Install

Copy or symlink `skills/autopilot` into your agent's personal skills directory. For example:

```bash
mkdir -p ~/.claude/skills
ln -s /absolute/path/to/agent-skills/skills/autopilot ~/.claude/skills/autopilot
```

For Codex, use the same package under `~/.codex/skills/autopilot`.

The package is intentionally standalone. If your agent provides goal persistence, independent reviewers, browser automation, or evaluation skills, Autopilot will use them; otherwise it follows the equivalent workflow directly.
