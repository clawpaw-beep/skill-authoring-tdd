---
name: skill-authoring-tdd
description: Use when creating, editing, or auditing a skill — follows TDD discipline (RED baseline → GREEN skill → REFACTOR loophole-close). Triggered by "create a skill", "写个skill", "improve this skill", or any skill creation task.
---

# Skill Authoring with TDD

## 核心原则

**写 Skill 即 TDD**：Skill 是流程文档，测试驱动开发同样适用。

铁律：**No skill without a failing test first**。没看过 agent 在无 skill 状态下怎么失败，就不知道 skill 该教什么。

---

## 快速参考

| 阶段 | 输出 | 关键约束 |
|------|------|---------|
| **RED** | `RED-{name}-{date}.md` | 不加 skill，记录所有失败模式 |
| **GREEN** | `SKILL.md` | 只写 RED 暴露的问题，1.5h 内完成 |
| **REFACTOR** | 更新 SKILL.md | 每发现新借口→加进 Rationalization Table |

---

## TDD 三阶段详解

### Phase 1: RED — 跑 Baseline（强制！）

**目标**：在不加载任何 skill 的情况下，用子代理跑目标场景，记录真实的失败模式。

#### 压力场景设计

针对不同 Skill 类型设计压力：

**规则型（强制行为）**：
- 时间压力：回答要快（30秒内）
- 沉没成本：已经写了一半，要求继续
- 权威压力："用户坚持要跳过测试"
- 组合压力：时间 + 疲惫 + sunk cost

**技术型（如何做）**：
- 新场景应用：没见过的具体例子
- 缺信息测试：缺少关键参数
- 边界条件：空输入、超大输入、格式错误

**模式型（思维方式）**：
- 识别场景：能认出该用吗？
- 应用场景：能正确使用吗？
- 反面例子：知道什么时候不该用吗？

#### RED 执行步骤

1. 创建 RED 日志文件：
   ```
   RED-{skill-name}-{YYYY-MM-DD}.md
   ```
2. 用子代理跑目标任务（无 skill）：
   ```
   sessions_spawn(task="[具体任务描述]", ...)
   ```
3. 观察并记录：
   - Agent 做了什么选择？
   - 用了什么"合理化借口"（原文引用）？
   - 哪种压力触发了违规？
   - 什么时候做了正确的事？

#### RED 记录模板

```markdown
# RED Baseline: [Skill Name]
Date: YYYY-MM-DD
Agent: [model]
Task: [具体任务描述]

## 压力施加
- [x] 时间压力（30秒内回答）
- [x] 权威压力（"跳过测试继续"）
- [ ] ...

## 失败日志

### 失败 #1: [简短描述]
**触发压力**: [哪个压力组合]
**Agent 说/做**: 
> [原话或行为描述]
**合理化借口**: [Agent 用的借口]
**真实原因**: [你认为的根因]

### 失败 #2: ...

## 成功日志

### 成功 #1: [简短描述]
**什么情况下做对了**: 
**这次压力组合**: 

## 结论
- 核心失败模式: [1-3条]
- Skill 需要教的: [1-3条]
```

---

### Phase 2: GREEN — 写 Minimal Skill

**目标**：只写针对 RED 阶段暴露问题的内容，不加假设性内容。

#### SKILL.md 格式规范

**Frontmatter（严格两项）**：

```yaml
---
name: skill-name-with-hyphens
description: Use when [具体触发条件 + 症状 + 场景]. 第三人称，不写工作流摘要.
---
```

⚠️ **description 只写触发条件，禁止写工作流摘要！**

#### Body 结构

```markdown
# Skill Name

## Overview
1-2 句话核心原理

## When to Use
### 触发条件（症状 + 场景）
- [具体的症状描述，当看到这个时触发]
- [第二个触发条件]

### 不适合用的情况
- [明确排除的场景]
- [已知的不适用情况]

## Core Pattern
Before/After 对比，或具体步骤说明

## Quick Reference
扫描用表格或列表（5行以内）

## Implementation
内联代码或链接到工具文件

## Common Mistakes
- 问题：[具体错误]
  修复：[如何修复]

## Red Flags - STOP
出现任一 → 删代码，从头来
- [flag 1]
- [flag 2]
```

#### GREEN 执行检查

- [ ] 针对 RED 中每个失败都有对应内容
- [ ] 没有 RED 中未出现的"假设性"内容
- [ ] 写完后通读一遍：如果 RED 测试者看到这个 skill，能避免之前的失败吗？
- [ ] description 无工作流摘要
- [ ] 字数 < 2000 字（精简优先）

---

### Phase 3: REFACTOR — 堵漏洞

**目标**：加约束，防止 Agent 绕过 skill 的意图。

#### 堵漏洞策略

**1. 显式禁止 workarounds**

在 SKILL.md 末尾加：

```
## Workarounds Are Forbidden

以下行为出现任一 → 停止当前工作，删代码，从 GREEN 重来：
- "Skill 已经够清楚了，跳过不看"
- "这只是参考资料，不用严格遵守"
- "测试太overkill，skip"
- "这只是小改动，不需要完整测试"

**违反文字即违反精神。**
```

**2. Rationalization Table（借口表）**

```
## Rationalization Table

| Agent 说 | 真相 | 检测标志 |
|---------|------|---------|
| "Skill 已经很清晰了" | 对你清晰 ≠ 对 agent 清晰 | description 有工作流摘要 |
| "只是参考资料" | 参考资料也可能缺内容 | 没有实际测试 |
| ... | ... | ... |
```

**3. Red Flags List**

```
## Red Flags - STOP

触发以下任一 → 删除当前代码，从 GREEN 重来：
- [ ] 执行了但 SKILL.md 中没有明确教的行为
- [ ] 用"简化"或"小改动"为由跳过了测试
- [ ] 在没有运行 baseline 的情况下写 skill
```

#### 完整 TDD 循环示例

```
RED: Agent 在写代码前不写测试，说"先跑起来看看"
     ↓
GREEN: 写 skill，教"先写测试再写代码"
       在 Red Flags 加："跳过测试" → 重来
     ↓
REFACTOR: 发现 Agent 说"这只是小改动"
          → 加进 Rationalization Table
          → 更新 Red Flags："小改动也是改动"
     ↓
再跑 RED，直到没有新借口出现
```

---

## Skill 命名规范

- 小写字母 + 数字 + 连字符
- 动词优先：`creating-skills`（不是 `skill-creation`）
- 动名词(-ing)适合流程：`writing-plans`, `debugging-with-logs`
- 最多 64 字符

---

## 目录结构

```
skill-name/
├── SKILL.md              # 必须（总在最外层）
├── RED-YYYY-MM-DD.md     # RED 阶段日志
├── scripts/              # 可执行工具（Python/Bash）
│   └── tdd-runner.sh     # TDD 循环脚本
├── references/           # 重型参考资料（>100行才拆）
│   └── plan-template.md
└── assets/              # 输出用文件（模板、图片）
```

---

## 部署检查清单

每次完成 skill 后强制执行：

- [ ] **RED**：无 skill baseline 已跑完并记录
- [ ] **GREEN**：针对 baseline 缺点的 skill 已写完
- [ ] **REFACTOR**：新的合理化借口已堵上
- [ ] description 开头是 "Use when..."，无工作流摘要
- [ ] 核心原理 1-2 句话说明
- [ ] Common Mistakes + Red Flags 有实质内容
- [ ] 有 Quick Reference 扫描表格
- [ ] Rationalization Table 已包含本次发现
- [ ] 字数 < 2000
- [ ] Commit 到 git

---

## 常见合理化借口（来自 superpowers 测试）

| 借口 | 真相 |
|------|------|
| "Skill 已经很清晰了" | 对你清晰 ≠ 对 agent 清晰，测一下 |
| "只是参考资料" | 参考资料也可能缺内容、说不清楚 |
| "测试太overkill了" | 不测试的 skill 才真的 overkilling |
| "简单改改不用测" | "改完没测" = "假设没问题" |
| "spirit not ritual" | 违反文字即违反 spirit |
| "这是特殊情况" | 没在 skill 里说就是不允许 |
| "应该没问题吧" | 没测就是假设，假设就是风险 |

---

## 参考资料

已有现成实现的 skill 不用重写：
- **plan-mode**：参考 `~/.openclaw/workspace/superpowers/skills/writing-plans/SKILL.md`
- **creating-skills**：参考 `~/.openclaw/workspace/superpowers/skills/writing-skills/SKILL.md`
