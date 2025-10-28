# 🧭 Claude 前端开发工作流 (JS/TS/Next.js)
本文件专用于 JS/TS/Vite/Next.js 项目。Claude 必须严格执行以下工作流。

---

## 1. Mockup 优先工作流
1. 开发新功能前生成可运行 Mockup/Demo；
2. Mockup 展示完整 UI/UX，可使用 mock data；
3. 等待用户确认：✅ Mockup Confirmed – Proceed to Implementation；
4. 未确认前不得编写真实逻辑或 API 代码。

---

## 2. 测试驱动开发与验证
* 单元与集成测试：`pnpm run test`
* E2E 测试：`pnpm exec playwright test`
* 测试失败 → Claude 修复 → 重测 → 输出 “✅ All tests passed”

---

## 3. Pre-Commit 工作流
1. 格式化与 Lint：`pnpm run lint --fix`
2. 测试与 E2E：`pnpm run test && pnpm exec playwright test`
3. 全部通过后提交。

---

## 4. 语言与风格
* 2空格缩进，优先 `async/await`、`const/let`
* 使用可选链(`?.`)、空值合并(`??`)
* 参数>3个使用对象解构
* 格式化命令：`pnpm run lint && pnpm run format`

---

## 5. 架构与 CI 一致性
* 测试覆盖率 ≥80%，文档放在 `docs/`
* `.github/workflows/test.yml` 模板：
```yaml
- run: pnpm install
- run: pnpm run lint
- run: pnpm run test
- run: pnpm exec playwright test
```

---

## 6. 常用命令
| 操作 | 命令 |
|------|------|
| 安装依赖 | pnpm install |
| 测试 | pnpm run test |
| E2E 测试 | pnpm exec playwright test |
| 格式化 | pnpm run lint --fix |
| 本地运行 | pnpm dev |

---

## ✅ 自检模板
```
[CLAUDE CHECKLIST - Frontend]
- [x] Mockup confirmed
- [x] Test & E2E workflow active
- [x] Pre-commit flow passed
- [x] Cleanup ready
```
