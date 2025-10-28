# 🧭 核心开发指令与工作流

Claude 在生成代码、命令和建议时，**必须严格遵守以下规则与顺序**。
若任何阶段测试失败、命令出错或未确认 Mockup，则**不得继续执行下一步**。

---

## 1. 开发与测试工作流 (TDD Workflow)

### 1.1 前端原型优先 (Frontend Mockup-First)

1. 开发新的前端项目或功能时，**必须先创建可运行的 Mockup/Demo**。
2. Mockup 应展示完整 UI/UX 流程，可使用 mock data。
3. Claude 需等待用户明确回复以下确认语句后，方可继续：

   ```
   ✅ Mockup Confirmed – Proceed to Implementation
   ```
4. 确认前，Claude 不得生成真实业务逻辑代码或 API 对接代码。

---

### 1.2 伴随测试生成 (Test-First)

* 每个新功能或修复，**Claude 必须同时生成单元测试与集成测试**。
* 测试代码需与功能代码同步提交，并能通过以下命令直接运行：

  * JS/TS: `pnpm run test`
  * Python: `poetry run pytest`

---

### 1.3 即时测试与验证 (Immediate Testing & Validation)

每次代码变更后必须：

1. **自动运行全部相关测试**

   ```bash
   pnpm run test  # 或 poetry run pytest
   ```

2. **若项目为前端项目（检测存在 vite.config.ts 或 next.config.js）**
   Claude 必须执行 Playwright E2E 测试：

   ```bash
   pnpm exec playwright test
   ```

3. 若任何测试失败：

   * Claude 必须立即分析失败原因；
   * 生成对应修复补丁；
   * 重新执行测试，直至全部通过；
   * 向用户汇报 “✅ All tests passed”。

---

### 1.4 最终完整性测试 (Final Integration Test)

在功能全部完成后，Claude 必须执行全局测试：

```bash
pnpm run test
# 若为前端项目（含 vite.config.ts / next.config.js）
pnpm exec playwright test
# 或
poetry run pytest
```

若失败，修复并重复测试，直至所有测试通过。

---

### 1.5 清理临时文件 (Cleanup)

Claude 在任务结束时应：

* 删除所有临时测试或调试文件（如 `test_tmp_*.py` / `playwright-debug.log`）。
* 这些文件 **不得提交到 Git**。

---

## 2. 绝对规则 (Must-Dos)

### 技术栈

* **JS/TS**: 仅使用 `pnpm`（禁止 npm/yarn）。
* **Python**: 使用 `poetry` + `pyenv` (Python 3.12+)。
* **E2E 测试**: 若项目为前端类型，Playwright 为唯一标准框架。

---

### Git

* Commit 信息必须遵循：

  ```
  type(scope): description
  ```

  示例：`feat(api): add user endpoint`
* 禁止出现 “Claude”, “AI generated”等字样。
* 所有 commit 前必须执行完整测试。测试失败则禁止提交或推送。

---

### ✅ 提交前顺序 (Pre-Commit Flow)

Claude 必须严格遵循以下顺序：

1. **格式化与 Lint 检查**

   ```bash
   pnpm run lint --fix || ruff check --fix .
   ```
2. **运行测试**

   ```bash
   pnpm run test && pnpm exec playwright test || poetry run pytest
   ```
3. **测试通过确认**
   Claude 必须输出：

   ```
   ✅ All tests passed. Ready to commit.
   ```
4. **执行提交**

   ```bash
   git add .
   git commit -m "type(scope): description"
   ```
5. **仅在用户确认后方可执行推送**

   ```bash
   git push
   ```

---

### Python 项目结构

* 必须采用 `src/` 布局：

  ```toml
  [tool.poetry]
  packages = [{ include = "my_package", from = "src" }]
  ```
* `poetry install` 后应能直接导入包。

---

### 代码质量与安全

* 核心逻辑必须有单元测试。
* 所有输入必须验证与清理。
* 禁止硬编码密钥，必须使用 `.env` 环境变量。

---

## 3. 语言与工具偏好

### JavaScript / TypeScript

* 2空格缩进，`const`/`let`，优先 `async/await`
* 使用可选链 (`?.`)、空值合并 (`??`)
* 参数超过 3 个时使用对象解构
* Lint & 格式化：

  ```bash
  pnpm run lint && pnpm run format
  ```

---

### Python

* 使用 Ruff 格式化：

  ```toml
  [tool.ruff]
  line-length = 88
  target-version = "py312"
  fix = true
  [tool.ruff.lint]
  select = ["E", "F", "I"]
  [tool.ruff.format]
  quote-style = "single"
  ```
* 必须加类型注解；字符串用 f-strings；文件路径用 pathlib。
* 导入顺序：标准库 → 第三方 → 本地。

---

### Linting & Formatting 一致性

| 语言     | 检查命令            | 自动修复                  |
| ------ | --------------- | --------------------- |
| JS/TS  | `pnpm run lint` | `pnpm run lint --fix` |
| Python | `ruff check .`  | `ruff check --fix .`  |

---

## 4. 架构与质量要求

* **范式**：函数式优先，DRY 原则。
* **结构**：按功能组织文件，配置集中化管理。
* **文档**：统一放在 `docs/` 目录。
* **测试覆盖率**：单元、集成、E2E 总覆盖率需 ≥80%。

---

### CI/CD 一致性

Claude 生成的项目应包含：

```
.github/workflows/test.yml
```

内容至少包括：

```yaml
- run: pnpm install
- run: pnpm run lint
- run: pnpm run test
- run: pnpm exec playwright test
```

> 若检测为非前端项目（无 vite.config.ts / next.config.js），可省略最后一行。

---

### 依赖锁定

Claude 必须提交：

* `pnpm-lock.yaml`
* `poetry.lock`

---

## 5. Claude 行为控制 (Operational Rules)

* **指令优先级**: 用户直接指令 > CLAUDE.md 规则 > 默认推理
* **禁止操作**：

  * 删除 `.env`、`package.json`、`pyproject.toml`
  * 覆盖用户未确认文件
* **错误处理与自修复**：

  * 执行命令失败时，应：

    1. 输出错误日志；
    2. 分析错误原因；
    3. 自动尝试修复；
    4. 重新执行命令；
    5. 若三次仍失败，则暂停并等待用户确认。

---

## 附录：执行一致性模板

| 环境     | 安装依赖           | 运行测试              | E2E测试                     | 格式化                 | 本地运行             |
| ------ | -------------- | ----------------- | ------------------------- | ------------------- | ---------------- |
| JS/TS  | pnpm install   | pnpm run test     | pnpm exec playwright test | pnpm run lint --fix | pnpm dev         |
| Python | poetry install | poetry run pytest | -                         | ruff check --fix .  | poetry run start |

> 若检测为非前端项目，Playwright 测试自动跳过。

---

## ✅ Claude 自测机制

Claude 应在读取本文件后执行以下自检：

```
[CLAUDE CHECKLIST]
- [x] Read CLAUDE.md successfully
- [x] TDD workflow active
- [x] Conditional Playwright tests enabled
- [x] Pre-commit flow enforcement active
- [x] Auto-repair on failure active
```

---

## ✅ 行动总结

**Claude 每次执行任务时必须确保：**

1. Mockup → 用户确认 ✅
2. 功能实现 → 生成测试 → 执行测试
3. (如前端) 执行 E2E 测试 → 出错自动修复
4. 测试全部通过 → 格式化 → Commit → 用户确认推送
5. 清理临时文件，完成任务
