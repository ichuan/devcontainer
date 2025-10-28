# 🧭 Claude 全局开发规则与工作流 (Global CLAUDE.md)
Claude 在生成代码、命令和建议时，**必须严格遵守以下规则与顺序**。
若任何阶段测试失败、命令出错或未确认 Mockup，则**不得继续执行下一步**。

---

## 1. 通用开发与测试工作流

### 1.1 测试驱动开发 (TDD Workflow)
* 每个新功能或修复，**Claude 必须同时生成单元测试与集成测试**。
* 测试命令：
  * 前端: `pnpm run test`
  * Python: `poetry run pytest`

### 1.2 即时测试与验证
每次代码变更后必须：
```bash
pnpm run test || poetry run pytest
```
若失败：1. Claude 分析错误；2. 生成修复；3. 重新运行测试；4. 输出 “✅ All tests passed”。

---

### 1.3 最终完整性测试
在功能全部完成后，必须执行全局测试：
```bash
pnpm run test || poetry run pytest
```
若任何测试失败，必须修复直至全部通过。

---

### 1.4 清理与禁止提交文件
Claude 在任务结束时应删除调试文件：`test_tmp_*.py`, `playwright-debug.log` 等。这些文件不得提交到 Git。

---

## 2. 通用开发规范

### 2.1 Git 提交规范
* 提交信息格式：
  ```
  type(scope): description
  ```
  示例：`feat(api): add user endpoint`
* 禁止包含 “Claude”, “AI generated”；测试失败不得提交。

---

### 2.2 Pre-Commit 流程
1. 格式化与 Lint 检查：
   ```bash
   pnpm run lint --fix || ruff check --fix .
   ```
2. 测试：
   ```bash
   pnpm run test || poetry run pytest
   ```
3. 测试通过后输出：✅ All tests passed. Ready to commit.
4. 提交并等待用户确认推送。

---

## 3. Claude 行为控制
* **优先级**: 用户指令 > CLAUDE.md 规则 > 默认逻辑
* **禁止操作**: 删除 `.env`、`package.json`、`pyproject.toml`；覆盖未确认文件
* **错误自修复**: 出错 → 记录 → 修复 → 重试3次 → 仍失败则等待用户。

---

## 4. 依赖锁定与一致性
必须提交依赖锁文件：`pnpm-lock.yaml`, `poetry.lock`。

---

## 5. ✅ Claude 自检模板
```
[CLAUDE CHECKLIST]
- [x] Read CLAUDE.md successfully
- [x] TDD workflow active
- [x] Pre-commit flow enforcement active
- [x] Auto-repair on failure active
```

---

## ✅ 行动总结
Claude 必须确保：功能实现 → 测试 → 格式化 → Commit → 推送确认 → 清理临时文件。
