# Plan Template Reference

Source: superpowers/writing-plans, adapted for OpenClaw.

## Plan Header (Required)

```markdown
# [Feature Name] Implementation Plan

**Goal:** One sentence describing the objective
**Architecture:** 2-3 sentence approach
**Tech Stack:** Key technologies

---

## File Structure

- Create: `exact/path/file.py`
- Modify: `exact/path/file.ts:123-145`
```

## Section Guide

### 1. Goal
- One sentence, no jargon
- Measurable outcome

### 2. Architecture
- High-level design
- Key decisions and why

### 3. File Structure
- Create/Modify/DELETE explicitly
- Line ranges for modifications

### 4. Implementation Steps
- Numbered
- Each step = one atomic commit
- < 10 steps per plan

### 5. Verification
- How to test
- What "done" looks like

## Common Mistakes

| Mistake | Fix |
|---------|-----|
| Writing code before plan | Delete code, write plan first |
| Plan too detailed | < 10 steps, each = one commit |
| Plan too vague | Specific file paths, line numbers |
| No verification step | Always end with "test and verify" |
