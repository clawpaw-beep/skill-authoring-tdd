---
name: skill-authoring-tdd
description: Use when creating, editing, or auditing a skill — follows TDD discipline (RED baseline → GREEN skill → REFACTOR loophole-close). Triggered by "create a skill", "写个skill", "improve this skill", or any skill creation task.
---

# Skill Authoring with TDD

## 核心原则

**写 Skill 即 TDD**：Skill 是流程文档，测试驱动开发同样适用。

铁律：**No skill without a failing test first**。没看过 agent 在无 skill 状态下怎么失败，就不知道 skill 该教什么。

## TDD 三阶段

| 阶段 | 对应 | 动作 |
|------|------|------|
| RED | 测试用例 | 用子代理跑压力场景，记录"合理化借口" |
| GREEN | 生产代码 | 针对这些借口写 skill |
| REFACTOR | 重构 | 堵漏洞 → 再测 → 直到无懈可击 |

---

## Step 1: RED — 跑 Baseline（强制！）

**先用子代理跑场景，不加任何 skill**：观察 agent 在无引导状态下怎么出错。

### 压力场景设计

**针对不同 Skill 类型**：

**规则型（强制行为）**：
- 加入时间压力：回答要快
- 加入沉没成本：已经写了一半
- 加入权威压力："用户坚持要跳过测试"
- 组合压力：时间 + 疲惫 + sunk cost

**技术型（如何做）**：
- 新场景应用：没见过的具体例子
- 缺信息测试：缺少关键参数

**模式型（思维方式）**：
- 识别场景：能认出该用吗？
- 应用场景：能正确使用吗？
- 反面例子：知道什么时候不该用吗？

### 记录内容

必须记录：
- Agent 做出了什么选择？
- 用了什么"合理化借口"（原话）？
- 哪种压力触发了违规？

---

## Step 2: GREEN — 写 Minimal Skill

**只写针对 RED 阶段暴露问题的内容**，不要加假设性内容。

### SKILL.md 格式规范

**Frontmatter（严格两项）**：

```yaml
---
name: skill-name-with-hyphens
description: Use when [具体触发条件 + 症状 + 场景]. 第三人称，不写工作流摘要.
---
```

⚠️ **description 只写触发条件，禁止写工作流摘要！**

原因：测试发现，description 写了工作流摘要，agent 会走捷径跳过正文。

**Body 结构**：

```markdown
# Skill Name

## Overview
1-2 句话核心原理

## When to Use
[触发条件列表：症状 + 场景]
[不适合用的情况]

## Core Pattern
Before/After 对比，或步骤说明

## Quick Reference
扫描用表格或列表

## Implementation
内联代码或链接到工具文件

## Common Mistakes
什么问题 + 怎么修

## Red Flags - STOP
- [flag 1]
- [flag 2]
**出现任一 → 删代码，从头来**
```

### 常见合理化借口（来自 superpowers 测试）

| 借口 | 真相 |
|------|------|
| "Skill 已经很清晰了" | 对你清晰 ≠ 对 agent 清晰，测一下 |
| "只是参考资料" | 参考资料也可能缺内容、说不清楚 |
| "测试太overkill了" | 不测试的 skill 才真的 overkilling |
| "简单改改不用测" | "改完没测" = "假设没问题" |
| "spirit not ritual" | 违反文字即违反 spirit |

---

## Step 3: REFACTOR — 堵漏洞

**加更严格的约束**：

1. **显式禁止 workarounds**：
```markdown
Write code before test? Delete it. Start over.
No exceptions:
- Don't keep as "reference"
- Don't "adapt" while writing tests
- Don't look at it
```

2. **加 foundational principle**：
```markdown
**Violating the letter of the rules is violating the spirit of the rules.**
```

3. **加 Rationalization Table**：每次测试发现新借口 → 加进表格

4. **加 Red Flags List**：让 agent 自我检查

---

## Skill 命名规范

- 小写字母 + 数字 + 连字符
- 动词优先：`creating-skills`（不是 `skill-creation`）
- 动名词(-ing)适合流程：`writing-plans`, `debugging-with-logs`
- 最多 64 字符

## 目录结构

```
skill-name/
├── SKILL.md              # 必须（总在最外层）
├── scripts/              # 可执行工具（Python/Bash）
├── references/          # 重型参考资料（>100行才拆出去）
└── assets/              # 输出用文件（模板、图片）
```

**扁平命名空间** — 所有 skill 在同一搜索域

**何时拆文件**：
- 参考 > 100 行 → 拆到 references/
- 工具可复用 → 拆到 scripts/
- 其他保持内联

---

## 部署检查清单

每次完成 skill 后强制执行：

- [ ] **RED**：无 skill baseline 已跑完并记录
- [ ] **GREEN**：针对 baseline 缺点的 skill 已写完
- [ ] **REFACTOR**：新的合理化借口已堵上
- [ ] description 开头是 "Use when..."，无工作流摘要
- [ ] 核心原理 1-2 句话说明
- [ ] Common Mistakes + Red Flags 有内容
- [ ] 有 Quick Reference 扫描表格
- [ ] Commit 到 git

---

## 参考

已有现成实现的 skill 不用重写：
- **plan-mode**：直接参考 `~/.openclaw/workspace/superpowers/skills/writing-plans/SKILL.md`
- **creating-skills**：直接参考 `~/.openclaw/workspace/superpowers/skills/writing-skills/SKILL.md`

可以直接把 superpowers 的 skill 内容适配到 OpenClaw 体系使用。
