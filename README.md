# Skill Authoring with TDD

**Write skills the way developers write code: test first.**

Most skills are written after the fact — they document what the author thinks the agent should do, not what the agent actually does when left alone. The result: skills that look good on paper but fail in practice.

This methodology fixes that by applying Test-Driven Development to skill authoring.

## The Core Rule

> **No skill without a failing test first.**

Before writing a skill, run the agent without one and observe how it fails. Only then do you know what to teach.

## Three Phases

| Phase | Output | Goal |
|-------|---------|------|
| **RED** | `RED-{name}-{date}.md` | Capture all failure modes |
| **GREEN** | `SKILL.md` | Address RED findings, nothing more |
| **REFACTOR** | Updated `SKILL.md` | Add constraints, close loopholes |

## Quick Example

```
RED: Agent skips writing tests, says "let's just run it and see"
     ↓
GREEN: Write skill rule — "Test before code. No exceptions."
       Add Red Flag — "Skipped test" → Delete and restart
     ↓
REFACTOR: Agent says "it's just a small change"
          → Add to Rationalization Table
          → Update Red Flags: "small change ≠ skip test"
```

## Key Insight: Rationalization

Agent failures follow predictable patterns:

| "It's fine" | Reality |
|-------------|---------|
| "Skill is clear enough" | Clear to you ≠ clear to agent |
| "Testing is overkill" | Untested skills are the real overkill |
| "Just a small change" | Small change + no test = assumed broken |
| "Spirit not ritual" | Violating the letter violates the spirit |

## File Structure

```
skill-authoring-tdd/
├── SKILL.md                    # Complete methodology (this file is the skill)
├── README.md                   # This overview
├── LICENSE                     # MIT
└── references/
    └── plan-template.md        # Plan template for skill implementation
```

## Getting Started

1. **RED phase**: Spawn a sub-agent to run your target task without any skill loaded. Log what it does wrong.
2. **GREEN phase**: Write a minimal `SKILL.md` addressing only the failures you observed.
3. **REFACTOR phase**: Add constraints and the Rationalization Table to close loopholes.

See `SKILL.md` for the complete methodology.

## Why TDD for Skills?

Because skills fail in predictable ways — agents take shortcuts, skip tests, and rationalize. TDD makes these failure modes explicit and teachable.

## License

MIT
