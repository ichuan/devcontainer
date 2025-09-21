# 核心开发指令与工作流

请在生成代码、命令和建议时严格遵守以下流程和规则。

## 1. 开发与测试工作流 (TDD Workflow)

这是一个强制性的工作流程，必须在每次代码生成中遵循：

1.  **伴随测试生成 (Test-First)**: 当我请求实现一个功能时，**你必须同时生成该功能的单元测试代码**。测试代码应与功能代码一同交付。
2.  **增量测试与修复 (Incremental Testing)**:
    - 每完成一个独立功能或一次代码修改，**立即执行与该功能相关的测试**。
    - 如果测试失败，**必须先修复问题**，直到测试通过，然后再继续下一步。
3.  **最终完整性测试 (Final Integration Test)**:
    - 当所有项目代码都生成完毕后，**必须执行一次完整的、全局的测试** (例如: `pnpm run test` 或 `poetry run pytest`)。
    - 如果发现任何问题，进行最终修复，确保所有测试都能通过。
4.  **清理临时文件 (Cleanup)**:
    - 任务结束时，**必须删除所有在开发过程中创建的临时测试文件**（例如项目根目录下的 `test_timeout_fixes.py`）。
    - **这些临时文件绝不能提交到 Git 仓库**。

## 2. 绝对规则 (Must-Dos)

- **技术栈**:
    - **JS/TS**: **只使用 `pnpm`**。绝不使用 npm 或 yarn。
    - **Python**: 使用 `poetry` 和 `pyenv` (版本 3.12+)。
    - **Git**: Commit Message **必须遵循 `type(scope): description` 格式** (例如: `feat(api): add user endpoint`)。

- **Python 项目结构**:
    - **必须采用 "src" 布局**。主代码包必须放在 `src` 目录下。
    - **`pyproject.toml` 必须配置为可编辑模式安装**，以确保 `src` 内的包可被直接导入。配置示例如下：
      ```toml
      [tool.poetry]
      packages = [{include = "my_package", from = "src"}]
      ```
      _（这里的 `my_package` 是 `src` 目录下的包名）_

- **代码质量与安全**:
    - 核心业务逻辑**必须有单元测试**覆盖。
    - 所有用户输入**必须经过验证和清理**。
    - 敏感信息（密钥、Token）**必须通过环境变量加载**，禁止硬编码。

## 3. 语言与工具偏好

### JavaScript / TypeScript
- **风格**: 2空格缩进，优先 `async/await`, `interface` > `type`, `const`/`let`。
- **语法**: 优先使用可选链 (`?.`) 和空值合并 (`??`)。
- **函数**: 参数超过3个时，使用对象解构 `{}`。

### Python
- **格式化 (Ruff)**:
  ```toml
  [tool.ruff]
  line-length = 88
  target-version = "py312"
  [tool.ruff.lint]
  select = ["E", "F", "I"] # 基础规则、Pyflakes、isort
  [tool.ruff.format]
  quote-style = "single"
  ```
- **代码**: 必须使用 Type Hints，字符串用 f-strings，文件路径用 `pathlib`。
- **结构**: 导入顺序（标准库 → 第三方 → 本地），数据结构用 `dataclasses` 或 `Pydantic`。

### Git & 常用命令
- **工作流**: 优先 `git rebase -i` 保持历史整洁，而非 `git merge`。
- **分支命名**: `feature/`, `fix/`, `refactor/`。
- **常用命令**: `pnpm dev`, `poetry install`, `git log --oneline`, `ruff check --fix .`。

## 4. 架构与质量要求

- **编程范式**: 优先函数式编程，遵循 DRY 原则。
- **项目结构**: 按功能（Feature）组织文件，而非按类型（Type）。配置文件集中管理。
- **文档管理**: 所有项目文档（如 `TROUBLESHOOTING.md`, `MIGRATION_GUIDE.md` 等）**必须统一存放在 `doc/` 目录下**。
- **测试**: 重点测试边界条件和异常情况。
- **代码审查要点**: 检查错误处理、性能影响（循环和DB查询）、安全漏洞和代码耦合度。
