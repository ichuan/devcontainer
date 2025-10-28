# 🧭 Claude Python Backend 开发规范
本文件适用于 Python / FastAPI / Flask / Django 等后端项目。

---

## 1. 测试驱动开发
* 每个新功能伴随测试：`poetry run pytest`
* 测试未通过不得提交，测试通过输出 ✅ All tests passed。

---

## 2. 项目结构与依赖管理
* Python 3.12+；`poetry` + `pyenv`
* `pyproject.toml` 示例：
  ```toml
  [tool.poetry]
  packages = [{ include = "my_package", from = "src" }]
  ```

---

## 3. 代码规范与安全
* 必须类型注解；字符串用 f-string；路径用 pathlib；
* 禁止硬编码密钥 → 使用 `.env`；
* 输入验证与异常处理完善。

---

## 4. Lint & 格式化 (Ruff)
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
运行命令：`ruff check --fix .`

---

## 5. Pre-Commit 流程
1. 格式化：`ruff check --fix .`
2. 测试：`poetry run pytest`
3. 测试通过 → ✅ All tests passed → Commit

---

## 6. CI/CD 模板
```yaml
- run: poetry install
- run: ruff check .
- run: poetry run pytest
```

---

## 7. 常用命令
| 操作 | 命令 |
|------|------|
| 安装依赖 | poetry install |
| 测试 | poetry run pytest |
| 格式化 | ruff check --fix . |
| 启动服务 | poetry run start |

---

## ✅ 自检模板
```
[CLAUDE CHECKLIST - Python]
- [x] Poetry + pyenv active
- [x] TDD workflow active
- [x] Ruff lint passed
- [x] Tests passed
- [x] Ready for commit
```
