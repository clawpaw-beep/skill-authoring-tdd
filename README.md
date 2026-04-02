# Skill Authoring with TDD

A methodology for authoring AI agent skills using Test-Driven Development principles.

## The Problem

Most skills are written after the fact — they document what the author thinks the agent should do, not what the agent actually does when left alone.

## The Solution: TDD for Skills

**No skill without a failing test first.**

| Phase | Goal | Action |
|-------|------|--------|
| RED | Capture failures | Run agent without skill, document rationalizations |
| GREEN | Write minimal skill | Address RED findings, nothing more |
| REFACTOR | Close loopholes | Add constraints, retest |

## Key Rationalizations

| "It's fine" | Reality |
|-------------|---------|
| "Skill is clear enough" | Clear to you ≠ clear to agent |
| "Testing is overkill" | Untested skills are the real overkill |

See SKILL.md for the complete methodology.

## License

MIT
