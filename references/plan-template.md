# Plan 模板参考

来源：superpowers/writing-plans，精简自用。

## Plan Header（必须）

```markdown
# [功能名] Implementation Plan

**Goal:** [一句话描述目标]
**Architecture:** [2-3 句话方案]
**Tech Stack:** [关键技术栈]

---

## File Structure

[文件映射]
- Create: `exact/path/file.py`
- Modify: `exact/path/file.ts:123-145`
- Test: `tests/exact/path/test.py`
```

## Task 结构

```markdown
### Task N: [组件名]

**Files:**
- Create: `exact/path/to/file.py`
- Modify: `exact/path/to/existing.py:123-145`
- Test: `tests/exact/path/to/test.py`

- [ ] **Step 1: Write the failing test**
```python
def test_specific_behavior():
    result = function(input)
    assert result == expected
```

- [ ] **Step 2: Run test to verify it fails**
`Run: pytest tests/path/test.py::test_name -v`
`Expected: FAIL with "function not defined"`

- [ ] **Step 3: Write minimal implementation**
```python
def function(input):
    return expected
```

- [ ] **Step 4: Run test to verify it passes**
`Expected: PASS`

- [ ] **Step 5: Commit**
```bash
git add tests/path/test.py src/path/file.py
git commit -m "feat: add specific feature"
```

## Task 粒度标准

**每个 Step = 2-5 分钟**：
- "Write the failing test" - 一个 step
- "Run it to make sure it fails" - 一个 step
- "Implement the minimal code" - 一个 step
- "Run tests to verify" - 一个 step
- "Commit" - 一个 step

## 原则

- **精确文件路径**，不是"相关文件"
- **完整代码**，不是"添加验证"
- **精确命令 + 预期输出**
- **DRY, YAGNI, TDD, frequent commits**
