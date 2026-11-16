# Atlas 通讯录 · 代码规范

> 本文件是作业要求中的“代码规范链接”入口。工具配置以仓库根目录的
> `.editorconfig`、`.prettierrc.json`、`eslint.config.js` 为准；本文说明团队约定。

## 1. 通用

- 编码 **UTF-8**，换行 **LF**，文件末尾保留一个换行。
- 缩进 **2 个空格**，不用 Tab。
- 行宽尽量 ≤ **100** 字符。
- 文件名：React 组件用 `PascalCase.tsx`，其他模块用 `camelCase.ts`，脚本用 `kebab-case`。
- 不提交 `node_modules/`、`dist/`、`*.db`、`.env*`（`.env.example` 除外）、日志和本地缓存。
- 不把密钥、AccessKey、SSH 私钥、云服务器密码写进代码或文档。

## 2. TypeScript / React

- 使用函数组件 + Hooks，不使用 class 组件。
- 类型优先用 `interface` / `type` 显式声明；避免 `any`，确实需要时写注释说明原因。
- 领域逻辑放在 `src/lib/`，保持**纯函数**，方便 Vitest 测试；组件只负责展示与交互。
- 所有网络请求统一走 `src/lib/api.ts`，组件内不直接 `fetch`。
- 状态更新使用不可变写法（展开、`map`、`filter`），不直接修改原数组/对象。
- 副作用放在 `useEffect`，并在依赖数组中列全依赖；需要清理的定时器/监听器必须 `return` 清理函数。
- 组件 props 必须写类型；可复用逻辑抽成自定义 Hook 或 `lib/` 函数。
- 可访问性：按钮有可读文本或 `aria-label`；星标用 `aria-pressed`；对话框支持 Esc 关闭。

## 3. 样式

- 统一使用 `src/styles.css` 中的 CSS 变量（颜色、圆角、阴影、间距），不写魔法值。
- 类名用语义化小写 + 中划线，如 `.contact-card`、`.contact-card__star`。
- 移动端优先保证 375px 无横向滚动；断点统一在样式文件顶部注释说明。
- 不引入新的 UI 框架或 CSS-in-JS；需要新依赖先和对方确认。

## 4. Python

- 遵循 **PEP 8**：4 空格缩进、函数/变量 `snake_case`、常量 `UPPER_CASE`。
- 只使用 Python 标准库（当前后端刻意零第三方依赖）。
- 数据库访问集中在 `server/app.py`；SQL 用参数化查询，不拼接用户输入。
- 对外返回统一结构；错误信息可读，不泄露堆栈和服务器路径。
- 测试用 `unittest`，文件名 `test_*.py`，每个测试独立使用临时数据库。

## 5. Git 提交规范

提交信息格式：

```text
<type>(<scope>): <一句话说明>
```

| type | 用途 |
|---|---|
| `feat` | 新功能 |
| `fix` | 修复缺陷 |
| `test` | 测试 |
| `docs` | 文档 |
| `style` | 格式/样式（不改逻辑） |
| `refactor` | 重构（不改功能） |
| `chore` | 构建、配置、部署 |
| `ci` | CI 配置 |

示例：

```text
feat(contacts): 实现收藏、搜索与多联系方式
feat(excel): 实现 Excel 导入导出
test(ui): 覆盖领域逻辑、Excel 与页面交互
chore(deploy): 增加 Docker/Nginx 与阿里云部署
docs: 补充运行说明、分工、规范与截图
```

- 一次提交只做一件事；提交信息用中文或英文都可，但同一仓库保持一致。
- 不提交与本次任务无关的格式化改动。
- 提交前跑 `git diff --stat`，改动文件应与任务卡一致。

## 6. 分支与 PR

```text
main                 默认分支，只通过 PR 合并
feat/a-platform      A：工程配置 + API + 部署
feat/b-product       B：界面 + 收藏 + 多联系方式 + Excel + 文档
ci/verify            A：CI 流水线
```

- 分支名小写，用中划线分隔。
- PR 标题与提交信息同格式；描述里贴验收命令输出。
- 作者不合并自己的 PR；合并用 **Create a merge commit**，禁止 Squash。
- 合并前至少一人 review，重点看：越界文件、能否构建/测试、是否提交了密钥。

## 7. 本地检查命令

```bash
pnpm format:check     # Prettier 检查
pnpm lint             # ESLint
pnpm test             # Vitest（前端）
pnpm build            # tsc + vite build
python -m unittest server.test_app -v
```

提交前至少跑：




```bash
pnpm lint && pnpm test && pnpm build
```



## 8. 目录边界

| 目录 | 负责人 | 说明 |
|---|---|---|
| `src/` | B | React 前端、领域逻辑、Excel、前端测试 |
| `server/` | A | Python API、SQLite、后端测试 |
| `deploy/`、Docker/Nginx | A | 部署配置 |
| `docs/` | 共同（按文件） | 分工、规范、博客、PSP；部署文档归 A |
| 根配置 | A | `package.json`、`tsconfig*`、`vite.config.ts` 等 |

完整文件归属与提交拆分见 [`team-division.md`](./team-division.md)。
