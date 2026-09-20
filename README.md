# Agent Skills

Reusable skills for autonomous coding agents.

## Available skills

### Autopilot

Runs a user-approved work session autonomously while keeping durable state in an append-only backbone document. It is designed for long-running work that may span context compactions or unattended hours.

The skill requires the user to approve a verifiable goal, evaluation criteria, scope and authority boundaries, and explicit resource ceilings before the run begins.

Source: `skills/autopilot/SKILL.md`

### Bay Area Resy Credit

Answers where an American Express Resy Credit can actually be spent in the San Francisco Bay Area. Bundles 248 restaurants individually verified on their own Resy venue page, 75 Tock-redirect venues to check separately, and the eligibility rules that decide both.

The data is a September 19, 2026 snapshot. Eligibility is a live state, so the skill treats every entry as a lead to confirm rather than a guarantee.

Source: `skills/bay-area-resy-credit/SKILL.md`

## Install

Copy or symlink a skill directory into your agent's personal skills directory. For example:

```bash
mkdir -p ~/.claude/skills
ln -s /absolute/path/to/agent-skills/skills/autopilot ~/.claude/skills/autopilot
ln -s /absolute/path/to/agent-skills/skills/bay-area-resy-credit ~/.claude/skills/bay-area-resy-credit
```

For Codex, use the same packages under `~/.codex/skills/`.

Each package is standalone. If your agent provides goal persistence, independent reviewers, browser automation, or evaluation skills, Autopilot will use them; otherwise it follows the equivalent workflow directly.
