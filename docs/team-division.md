# Atlas 通讯录 · 两人分工与 GitHub 提交方案

> 关联：`assignment.md`（截止 **2026-11-17 23:59:59**）
> 项目：React 19 + TypeScript + Vite / Python 3.12 + SQLite / Docker + Nginx
> 前提：两人使用**两台电脑**（各自配置自己的 GitHub 身份），提交到**同一个公开 GitHub 仓库**
> 本文是提交前的唯一操作口径；`分工协作.md` 是启动前的计划稿，结构与实际代码不一致，以本文为准。

---

## 0. 一句话结论

**不要各自 fork 两个仓库。** 建一个两人共用的 public 仓库，按“文件边界”把已完成的代码拆成：

- **成员 A（平台与领域线）**：工程配置、领域模型/收藏/搜索、Python API、SQLite、Docker/Nginx、部署、CI —— **7 次提交**
- **成员 B（界面与数据交换线）**：React 页面、多联系方式、Excel 导入导出、前端测试、文档与交付 —— **5 次提交**

两人在**各自的电脑**上配置**本人的 GitHub 提交身份**。A 先把 A0–A2（初始化 + 工程 + 领域模型）推到 `main` 作为共同基线；然后 **A3–A5 在 `feat/a-platform`、B1–B5 在 `feat/b-product`，两条分支各自提交、互不依赖**；最后按 **A → B → CI** 的顺序合并回 `main`，由对方 review 后点 **Create a merge commit**（绝对不要 Squash），打 `v1.0.0` 标签并发 Release。这样总提交 **12 次**，两人各 **5 次以上**，满足“提交次数 ≥ 3”和“每位贡献者提交次数统计”。

> ⚠️ 底线：只把**本人实际负责、实际改过、能讲清**的文件记到本人名下。如果某一块实际只有一个人做过，就如实写，不要为了凑次数伪造作者；老师看提交历史时也会看 PR 里的 review 和代码归属。

---

## 1. 当前仓库核查结果（2026-11-16 20:45）

| 项目 | 结果 |
|---|---|
| 压缩包 | `xp-contacts-app-github.zip`，67 个文件，`MANIFEST.sha256` 全部校验通过 |
| 解压位置 | 已解压到仓库根目录 `C:\Users\Lenovo\Documents\GitHub\project2` |
| Git 仓库 | **尚未初始化**（没有 `.git`） |
| 当前这台电脑的 Git 身份 | `fu-yi-hang <3266389474@qq.com>`；另一台电脑必须配置**另一人本人**的姓名/邮箱，两台电脑不要共用同一身份 |
| 后端测试 | `python -m unittest server.test_app -v` → **3 个用例全部通过** |
| 前端验证 | `pnpm test` → **34/34 通过**；`pnpm build` 成功；`pnpm lint` 退出码 0（2026-11-16 21:02） |
| 线上部署 | `http://120.55.102.189/` 返回 **HTTP 200**，是当前 Atlas 页面 |
| 未提交的打包元数据 | `xp-contacts-app-github.zip`、`MANIFEST.sha256`、`EXPORT_INFO.txt`、`GIT_UPLOAD_GUIDE.md` 不应进入仓库 |

---

## 2. 分工原则

1. **按文件边界切，不按“谁写前端谁写后端”空泛切**：同一文件只能有一个 owner，避免两边同时改。
2. **一人一台电脑、一条分支线**：`feat/a-platform`、`feat/b-product`，最后一条 `ci/verify`；两台电脑各自登录本人 GitHub 账号。
3. **每台电脑只配置本人身份**：`user.name` 与 `user.email` 必须是本人的 GitHub 身份，另一台电脑配另一人，不要共用。
4. **交叉 review**：A 审 B 的 PR，B 审 A 的 PR；作者不合并自己的 PR。
5. **合并必须保留提交**：用 *Create a merge commit*，否则 Squash 会把多条提交压成 1 次，贡献统计直接失真。
6. **最终 `main` 必须可构建、可测试**：中间提交不要求单独可跑，但每个 PR 合并前跑一次全量检查。
7. **B 必须有可构建的起点**：A 先把 A0–A2（初始化、工程、领域模型）推到 `main`，A/B 都从 `main` 拉自己的分支；这样两条分支都能独立 `pnpm install / build / test`。

---

## 3. 文件归属（按实际目录）

### 成员 A —— 平台与领域线

| 范围 | 文件 |
|---|---|
| 工程配置 | `package.json`、`pnpm-lock.yaml`、`pnpm-workspace.yaml`、`tsconfig.json`、`tsconfig.app.json`、`tsconfig.node.json`、`vite.config.ts`、`index.html`、`eslint.config.js`、`.prettierrc.json`、`.prettierignore`、`.editorconfig`、`.gitignore`、`public/`、`LICENSE` |
| 领域模型 | `src/types.ts`、`src/lib/contacts.ts`、`src/lib/storage.ts`、`src/lib/api.ts`、`src/lib/format.ts`、`src/lib/labels.ts`、`src/lib/id.ts`、`src/data/seed.ts` |
| 后端 | `server/app.py`、`server/seed.json`、`server/__init__.py`、`server/test_app.py` |
| 容器与部署 | `Dockerfile`、`Dockerfile.runtime`、`docker-compose.yml`、`nginx.conf`、`server/Dockerfile`、`.dockerignore`、`.env.aliyun.example`、`deploy/`、`scripts/deploy_ecs.ps1`、`scripts/deploy_oss.py`、`scripts/ecs_bootstrap.sh` |
| CI | `.github/workflows/ci.yml` |

### 成员 B —— 界面与数据交换线

| 范围 | 文件 |
|---|---|
| 页面与组件 | `src/main.tsx`、`src/App.tsx`、`src/styles.css`、`src/vite-env.d.ts`、`src/components/` 全部 |
| Excel | `src/lib/excel.ts`、`src/components/ImportDialog.tsx`、`ExportMenu.tsx`、`docs/fixtures/`、`scripts/make-import-fixture.py` |
| 前端测试 | `src/lib/contacts.test.ts`、`src/lib/storage.test.ts`、`src/lib/excel.test.ts`、`src/App.test.tsx`、`src/test/` |
| 文档与交付 | `README.md`、`docs/deployment-aliyun.md`（A 编写，随 B5 提交）、`docs/screenshots/`、`scripts/capture-screenshots.mjs`、`docs/team-division.md`、`docs/code-style.md`、`docs/blog-outline.md`、`docs/psp/`、`assignment.md`、`分工协作.md` |

### 不属于任何人的提交

`xp-contacts-app-github.zip`、`MANIFEST.sha256`、`EXPORT_INFO.txt`、`GIT_UPLOAD_GUIDE.md` 是打包/导出元数据，**不要 `git add`**。`.env.aliyun`、`node_modules/`、`dist/`、`server/*.db` 已被 `.gitignore` 覆盖。

---

## 4. 提交拆分（12 次，A 7 + B 5）

> 提交图（顺序按实际执行）：
>
> ```text
> main ── A0 ── A1 ── A2 ─────────────● PR #1(A3–A5) ──● PR #2(B1–B5) ──● PR #3(CI) / v1.0.0
>          ├─ feat/a-platform: A3 ─ A4 ─ A5
>          └─ feat/b-product:  B1 ─ B2 ─ B3 ─ B4 ─ B5
> ```
>
> **A0–A2 先落在 `main` 作为共同基线**；A3–A5 和 B1–B5 分别从 `main` 拉出，两条分支完全独立，互不包含对方的提交。最后先合并 A，再合并 B；B 在开 PR 前把 `main` 合进自己的分支即可。A6（CI）最后合并。

| # | 分支 | 作者 | 提交信息 | 包含文件 | 验收 |
|---|---|---|---|---|---|
| A0 | `main` | A | `chore: 初始化仓库与许可` | `.gitignore`、`.gitattributes`、`LICENSE` | `git log` 可见首个提交 |
| A1 | `main` | A | `chore: 搭建 React + TypeScript + Vite 工程` | 工程配置文件、`index.html`、`public/` | `pnpm install` 成功 |
| A2 | `main` | A | `feat(domain): 实现联系人模型、收藏、搜索与本地缓存` | `src/types.ts`、`src/lib/{contacts,storage,api,format,labels,id}.ts`、`src/data/seed.ts` | 收藏/搜索/合并的单元测试可在 B4 复跑 |
| A3 | `feat/a-platform` | A | `feat(api): 实现 SQLite 通讯录 REST API` | `server/app.py`、`server/seed.json`、`server/__init__.py` | `python server/app.py` 后 `/api/health` 返回 200 |
| A4 | `feat/a-platform` | A | `test(api): 覆盖仓储校验与种子数据` | `server/test_app.py` | 3 个后端用例通过 |
| A5 | `feat/a-platform` | A | `chore(deploy): 增加 Docker/Nginx 与阿里云部署` | Docker/Nginx/部署脚本（部署 md 放 B5） | `docker compose config` 通过；公网 URL 200 |
| B1 | `feat/b-product` | B | `feat(ui): 搭建响应式通讯录界面与收藏视图` | `App.tsx`、`main.tsx`、`styles.css`、列表/侧边栏/弹窗组件 | `pnpm dev` 页面可用；星标与收藏视图正常 |
| B2 | `feat/b-product` | B | `feat(contacts): 实现多联系方式表单与详情展示` | `ContactForm.tsx`、`ContactDetail.tsx` | 可加多条不同类型；详情按类型分组 |
| B3 | `feat/b-product` | B | `feat(excel): 实现 Excel 导入导出` | `excel.ts`、导入/导出对话框、样例文件 | 导出后一人一行、每类一列；回灌成功 |
| B4 | `feat/b-product` | B | `test(ui): 覆盖领域逻辑、Excel 与页面交互` | 前端测试 | `pnpm test` 全绿 |
| B5 | `feat/b-product` | B | `docs: 提交运行说明、分工、规范、博客与全部 md` | `README.md`、`docs/` 全部 md、截图、博客/PSP 模板 | 链接可点；全部 md 已入库 |
| A6 | `ci/verify` | A | `ci: 增加 lint/test/build 流水线` | `.github/workflows/ci.yml` | GitHub Actions 绿 |

> 为什么 A 有 7 次、B 有 5 次：A0 是仓库初始化，且 A 多了领域模型、后端、部署、CI 四条线；B 的 5 次覆盖全部界面、Excel 与测试/文档。两人都 ≥ 5 次，统计好看且真实。
>
> **所有 Markdown 文档统一放在 B5 提交**：`README.md`、`assignment.md`、`分工协作.md`、`docs/deployment-aliyun.md`、`docs/team-division.md`、`docs/code-style.md`、`docs/blog-outline.md`、`docs/psp/*.md`。A5 只提交 Docker/Nginx/部署脚本，不再重复 add 部署文档。
>
> 每次提交的代码/文档已打包到 `release/commits/`（A0–A6、B1–B5，共 12 个 zip），文件名即提交名称；`B5-文档与全部md.zip` 包含全部 9 个 md。`release/` 已在 `.gitignore` 中，压缩包只作本地交接/核对，不推送到 GitHub。

---

## 5. 两台电脑：各自配置本人的 GitHub 身份

GitHub 是按 commit 的 **author email** 归属贡献的，不看电脑、不看谁 push。两台电脑反而简单：每台电脑只对应一个人，不需要在同一台机器上切换身份。

1. 每台电脑用本人的 **GitHub 已验证邮箱**，或 noreply 邮箱；
2. 每台电脑的 `user.name` / `user.email` 只配置本人；
3. 每台电脑用本人的 GitHub 账号登录 / SSH key 推送；
4. 两台电脑不要共用同一个 GitHub 账号，也不要共用 Personal Access Token。

### 5.1 取每个人的 noreply 邮箱

GitHub → **Settings → Emails → Keep my email addresses private**，会显示：

```text
12345678+github-username@users.noreply.github.com
```

前面的数字是 GitHub 用户 ID，必须带；不要自己拼。也可以直接用账号主邮箱。

### 5.2 每台电脑配置本人身份（只做一次）

**A 电脑：**

```powershell
git config --global user.name  "A的姓名"
git config --global user.email "A的noreply邮箱"
git var GIT_AUTHOR_IDENT          # 应显示 A 的姓名/邮箱
```

**B 电脑：**

```powershell
git config --global user.name  "B的姓名"
git config --global user.email "B的noreply邮箱"
git var GIT_AUTHOR_IDENT          # 应显示 B 的姓名/邮箱
```

每次 commit 后立刻检查：

```powershell
git log -1 --format="%h  author=%an <%ae>  committer=%cn <%ce>  %s"
```

author 和 committer 都应是本人。若只有 author 对、committer 是别人，说明这台电脑的 Git 身份配错了。

> 两台电脑不要共用同一套 `--global` 配置。如果某台电脑以前配过队友的身份，先 `git config --global user.name/user.email` 改成本人。

### 5.3 每台电脑的 GitHub 认证

**HTTPS（推荐，最省事）：**

第一次 `git push` 时 Git Credential Manager 会弹窗，登录**本人**账号即可。如果这台电脑以前登录过别人的账号，先清掉旧凭据：

```powershell
cmdkey /delete:git:https://github.com
```

再 push，重新用本人账号登录。

**SSH（可选）：**

每台电脑生成一把自己的 key，把公钥加到**本人**的 GitHub → Settings → SSH and GPG keys：

```powershell
ssh-keygen -t ed25519 -C "本人的GitHub邮箱"
Get-Content "$HOME\.ssh\id_ed25519.pub" | Set-Clipboard
ssh -T git@github.com      # 应显示 Hi 本人username!
```

> 不需要 `github-a` / `github-b` 两个 Host，也不需要 `push-a` / `push-b` remote。两台电脑各用各的账号，天然隔离。

### 5.4 remote 与推送

每台电脑都只加一个 `origin`：

```powershell
git remote add origin https://github.com/<org>/<repo>.git
# 或者用 SSH：git@github.com:<org>/<repo>.git
```

- A 电脑推：`git push -u origin main`、`git push -u origin feat/a-platform`、`git push -u origin ci/verify`
- B 电脑推：`git push -u origin feat/b-product`
- 拉取统一用 `git pull origin main`

---

## 6. 建仓、提交、PR、Release 的完整顺序

### 6.1 建仓库

1. 在 GitHub 新建 **public** 仓库（建议组织 `xp-contacts-team`，或队长账号）。
2. **Settings → Collaborators → Add people**，把队友加成 **Write**。
3. 不要让两人各自 fork 后再互提 PR：提交历史会被拆到两个仓库，贡献者统计不完整。
4. 如果课程要求先 Fork 课程模板，**只由队长 Fork 一次**，再把队友加成 collaborator；之后都往这一个仓库提交。

### 6.2 A 电脑：初始化 + A0 + 推 main

在 A 的电脑上，项目根目录（`C:\Users\Lenovo\Documents\GitHub\project2`）：

```powershell
git init -b main

git config user.name  "A的姓名"
git config user.email "A的noreply邮箱"

git add .gitignore .gitattributes LICENSE
git commit -m "chore: 初始化仓库与许可"

git remote add origin https://github.com/<org>/<repo>.git
git push -u origin main
```

> 如果 GitHub 上建仓库时勾选了 README，先 `git pull --rebase origin main` 再推，避免 non-fast-forward。

### 6.3 A 电脑：A1–A2 作为共同基线推到 main

```powershell
# 仍在 main 分支
# A1 工程配置
git add package.json pnpm-lock.yaml pnpm-workspace.yaml tsconfig.json tsconfig.app.json tsconfig.node.json vite.config.ts index.html eslint.config.js .prettierrc.json .prettierignore .editorconfig public
git commit -m "chore: 搭建 React + TypeScript + Vite 工程"

# A2 领域模型与本地存储
git add src/types.ts src/lib/contacts.ts src/lib/storage.ts src/lib/api.ts src/lib/format.ts src/lib/labels.ts src/lib/id.ts src/data/seed.ts
git commit -m "feat(domain): 实现联系人模型、收藏、搜索与本地缓存"

git push origin main
```

> A1/A2 直接落在 `main`，作为两条功能分支的共同基线。B 的界面依赖 `package.json`、`src/types.ts`、`src/lib/*`；没有这两个提交，B 的分支无法 `pnpm install / build / test`。
>
> 如果 `main` 开了分支保护，先把 A1/A2 提交到 `chore/base` 分支，开一个 base PR 让 B 合并，再从最新 `main` 拉功能分支；没有保护就直接推 `main`。
>
> 此时 A 的工作区里还有 B 的文件和尚未提交的 A3–A5，`git status` 会显示大量未跟踪文件，这是正常的；**只 add 本文列出的文件，不要 `git add -A`**。

### 6.4 B 电脑：从 main 拉出 B 分支

A 推完 A1–A2 后，B 在自己的电脑上操作。前提：

- B 的电脑上已有**完整项目文件**（把 `xp-contacts-app-github.zip` 拷过去解压，或从共享盘复制）；
- B 的电脑装好 Node.js ≥ 20、pnpm 11、Python ≥ 3.10。

先 clone 含基线的 `main`，再把完整源码覆盖进工作区：

```powershell
git clone https://github.com/<org>/<repo>.git
cd <repo>

git config user.name  "B的姓名"
git config user.email "B的noreply邮箱"

git fetch origin
git checkout -b feat/b-product origin/main

# 把完整项目文件复制/解压到当前目录，覆盖同名文件
# 确认基线在（两个都应输出 True）
Test-Path package.json
Test-Path src\types.ts
```

> clone 只包含 A0–A2 的基线；`src/components/`、`src/lib/excel.ts` 等 B 的文件要靠压缩包覆盖进来。覆盖后 `git status` 会看到 A3–A5、部署文件等未跟踪文件，这是正常的，**不要 `git add -A`**，只 add 下面列出的 B 文件。

然后提交 B1–B5：

```powershell
# B1 页面骨架与收藏视图
git add src/main.tsx src/App.tsx src/styles.css src/vite-env.d.ts src/components/Sidebar.tsx src/components/ContactList.tsx src/components/Modal.tsx src/components/ConfirmDialog.tsx src/components/Toast.tsx src/components/Avatar.tsx
git commit -m "feat(ui): 搭建响应式通讯录界面与收藏视图"

# B2 多联系方式表单与详情
git add src/components/ContactForm.tsx src/components/ContactDetail.tsx
git commit -m "feat(contacts): 实现多联系方式表单与详情展示"

# B3 Excel
git add src/lib/excel.ts src/components/ImportDialog.tsx src/components/ExportMenu.tsx docs/fixtures scripts/make-import-fixture.py
git commit -m "feat(excel): 实现 Excel 导入导出"

# B4 测试
git add src/lib/contacts.test.ts src/lib/storage.test.ts src/lib/excel.test.ts src/App.test.tsx src/test
git commit -m "test(ui): 覆盖领域逻辑、Excel 与页面交互"

# B5 文档（全部 md 一次提交）
git add README.md docs/screenshots docs/deployment-aliyun.md docs/team-division.md docs/code-style.md docs/blog-outline.md docs/psp assignment.md 分工协作.md scripts/capture-screenshots.mjs
git commit -m "docs: 提交运行说明、分工、规范、博客与全部 md"

git push -u origin feat/b-product
```

> B 的分支从 `main` 拉出，包含 A0–A2 基线，不包含 A3–A5；这是正常的，B 可以独立 `pnpm test / build`。A3–A5 合入 `main` 后，B 再 `git merge origin/main` 即可。
>
> 先把 B 分支推上去备份，**PR #2 等 A 合并后再开**，这样 main 合并后一次就能构建通过。

### 6.5 A 电脑：从 main 拉出 A 分支，做 A3–A5

```powershell
git checkout main
git pull origin main
git checkout -b feat/a-platform

# A3 Python API
git add server/app.py server/seed.json server/__init__.py
git commit -m "feat(api): 实现 SQLite 通讯录 REST API"

# A4 后端测试
git add server/test_app.py
git commit -m "test(api): 覆盖仓储校验与种子数据"

# A5 部署（部署 md 统一放 B5）
git add Dockerfile Dockerfile.runtime docker-compose.yml nginx.conf server/Dockerfile .dockerignore .env.aliyun.example deploy scripts/deploy_ecs.ps1 scripts/deploy_oss.py scripts/ecs_bootstrap.sh
git commit -m "chore(deploy): 增加 Docker/Nginx 与阿里云部署"

git push -u origin feat/a-platform
```

> A 与 B 并行；A 不需要 B 的界面文件。A3–A5 分支也不包含 B1–B5，两条分支完全独立。

### 6.6 最后合并 PR #1 / #2

1. A 开 **PR #1：`feat/a-platform` → `main`**，描述里贴：
   - 改动范围（API / SQLite / Docker / 部署）
   - `python -m unittest server.test_app -v`、`docker compose config` 的输出
   - 线上地址 `http://120.55.102.189/` 截图

   由 **B review 并 merge**（Create a merge commit）。

   > PR #1 合并后 `main` 还没有 `src/main.tsx`，`pnpm build` 会失败，这是正常的；CI 故意放到最后的 A6，等 PR #2 合并后再加，避免 PR #1 出现红灯。

2. A 合并后，B 把最新 `main` 合进自己的分支：

   ```powershell
   git fetch origin
   git checkout feat/b-product
   git merge origin/main
   # 文件不重叠，正常无冲突；若有冲突当场解决
   git push origin feat/b-product
   ```

3. B 开 **PR #2：`feat/b-product` → `main`**，描述里贴：
   - `pnpm test`、`pnpm build`、`pnpm lint` 的输出
   - 收藏/多联系方式/Excel 导入导出的截图或短视频
   - 说明“本 PR 只改 `src/` 与文档，未动 `server/`”

   由 **A review 并 merge**。

> 先合并 A 是为了让 PR #2 合并后的 `main` 一次就能构建通过；B 的 PR 只包含 B 自己的提交，不会出现 A 的 commit。合并完 PR #2 后，`main` 才是完整应用；此时在任意一台电脑跑一次 `pnpm test && pnpm build`。

### 6.7 PR #3：CI 收尾

```powershell
git checkout main
git pull origin main
git checkout -b ci/verify

git add .github/workflows/ci.yml
git commit -m "ci: 增加 lint/test/build 流水线"
git push -u origin ci/verify
```

开 **PR #3：`ci/verify` → `main`**。此时 `main` 已有完整代码，CI 应该绿。由 **B review 并 merge**。

### 6.8 打版本

```powershell
git checkout main
git pull origin main

git tag -a v1.0.0 -m "Atlas 通讯录 v1.0.0：收藏、多联系方式、Excel 导入导出、云部署"
git push origin v1.0.0
```

GitHub → **Releases → Draft a new release** → 选 `v1.0.0`，标题 `Atlas 通讯录 v1.0.0`，正文写功能清单 + 线上地址 + 构建/测试结果。

### 6.9 合并后的身份核对

```powershell
git shortlog -sne --all
git log --format="%an <%ae>" | Sort-Object | Group-Object | Select-Object Count,Name
git rev-list --count HEAD
git log --oneline --graph --all -20
```

期望：两个人名、两个邮箱、A ≥ 7 次、B ≥ 5 次、总数 ≥ 12。截图保存到 `docs/screenshots/`，博客要用。

> 注意：GitHub 的 merge commit 也是提交，作者是点合并的人。3 个 PR 合并后，再加上 B 在 PR #2 前 `git merge origin/main` 的一次合并，`git shortlog` 会比 12 多 2–3 条（B 通常多 2 条、A 多 1 条），属正常现象。博客里的“提交次数”以最终 `git shortlog -sne --all` 的实际数字为准，不要硬写 12。
>
> 截图已包含在 B5；运行视频上传到 B 站/网盘后，把链接写进 CSDN 博客即可，不再新增仓库 md 提交，保持 B5 是最后一个含 md 的提交。

---

## 7. 合并规则（最容易翻车的地方）

| 规则 | 说明 |
|---|---|
| 只允许 **Create a merge commit** | Squash 会把多条提交压成 1 条，作者变成合并人，贡献统计失真 |
| 作者不合并自己的 PR | A 的 PR 由 B 点合并，B 的 PR 由 A 点合并 |
| 每个 PR 至少 1 条 review | 看三件事：有没有越界文件、能不能构建/测试、提交信息是否规范 |
| 合并顺序 | PR #1（A）→ PR #2（B）→ PR #3（CI）；不要并行合并 |
| B 的分支落后 | 在开 PR #2 前 `git merge origin/main`，不要 `rebase` 已推送的共享分支 |
| 合并后 | 立刻 `git pull origin main`，再开下一条分支 |

---

## 8. 最终验收命令

```powershell
# 提交统计（博客截图用）
git shortlog -sne --all
git log --format="%an <%ae>" | Sort-Object | Group-Object | Select-Object Count,Name
git rev-list --count HEAD

# 构建与测试
pnpm install --frozen-lockfile
pnpm lint
pnpm test
pnpm build
python -m unittest server.test_app -v

# 部署
curl.exe -I http://120.55.102.189/
curl.exe http://120.55.102.189/api/health
```

最终 `main` 必须四项全绿：**lint / test / build / 后端测试**。如果 CI 绿，本地也跑一遍，博客里贴 CI 截图和本地输出。

---

## 9. 对照作业要求的缺口清单

| 要求 | 当前状态 | 还需要做什么 | 负责人 | 建议截止 |
|---|---|---|---|---|
| 公共 GitHub 仓库 | 未建 | 建 public 仓库，加 collaborator | A | 11-16 21:30 |
| 提交次数 ≥ 3 | 0 | 按本文做 12 次 | 共同 | 11-17 12:00 |
| 每位贡献者统计 | 两台电脑需各配本人身份 | 各自 `git config` + 本文核对 | 共同 | 11-17 12:00 |
| 发起 Pull Request | 无 | 3 个 PR，交叉 review | 共同 | 11-17 15:00 |
| 发布版本 | 无 | `v1.0.0` + Release | A | 11-17 16:00 |
| 代码规范链接 | 已补 `docs/code-style.md` | 博客/README 引用 | B | 11-16 22:00 |
| 博客 Markdown | 已有模板 `docs/blog-outline.md` | 两人各写/各发一份 | 共同 | **11-17 19:00 发布** |
| 提交历史截图 | 无 | `git shortlog` + GitHub Contributors 截图 | A | 11-17 15:30 |
| 功能实现思路 | 模板已写 | 收藏/多联系方式/Excel/部署四段按实际补 | A+B | 11-17 18:00 |
| 程序截图 | 已生成 7 张 + Excel 导出样例（`docs/screenshots/`） | 写进博客，检查截图里没有密钥/隐私 | B | 11-17 18:00 |
| 完整运行视频 | 无 | 3–5 分钟，按 `docs/blog-outline.md` 脚本 | B | 11-17 17:30 |
| 成员分工 | 本文 | 博客引用本文 | 共同 | 11-17 18:00 |
| 贡献比例合计 100 | 无 | 按提交数 + 模块权重填，见第 10 节 | 共同 | 11-17 19:00 |
| 困难与解决 | 无 | 写 3–5 条真实发生的 | 共同 | 11-17 18:00 |
| PSP 表（每人一张） | 模板已写 | 按实际工时填 | 各自 | 11-17 19:00 |
| 开头作业信息表 | 模板已写 | 博客开头 | B | 11-17 18:00 |
| 两人各自提交博客 | 无 | 两个 CSDN 账号各发一份 | 共同 | 11-17 19:00 |
| 构建通过 | 待验证 | `pnpm build` | A | 11-17 15:00 |
| 云部署 | 已 200 | 保持可访问，手机 4G 再验一次 | A | 11-17 16:00 |
| 无密钥/无数据库 | 已检查 | 提交前再跑一次关键字扫描 | A | 11-17 16:00 |

---

## 10. 贡献比例怎么填（合计 100）

不要无依据写 50/50。建议按下面四项打分后取整：

| 依据 | 权重 | 数据 |
|---|---:|---|
| 有效提交数 | 30% | `git shortlog -sne --all` |
| 任务模块权重 | 50% | 收藏 25 + 多联系方式 10 + Excel 25 + 部署 15 + 样式 10 |
| 测试与 review | 10% | 测试用例、PR review 条数 |
| 交付物 | 10% | 博客、视频、截图、PSP |

按本文拆分，A 负责工程配置、领域逻辑（收藏/搜索/合并）、Python API/SQLite、部署、CI；B 负责全部页面与交互、多联系方式、Excel 引擎、前端测试与交付文档。若实际贡献接近，填 **A 50% / B 50%**；若某一方明显多做了，按上表算到 **±5 分以内**（如 52/48、48/52），并在博客写清依据。

---

## 11. 时间安排（剩余约 26 小时）

```text
11-16 21:00  A 电脑：建仓、配 A 身份、A0 推 main
11-16 21:30  A 电脑：A1-A2 直接推 main（共同基线，B 的起点）
11-16 22:00  A 拉 feat/a-platform；B 电脑 clone main、配 B 身份、拉 feat/b-product
11-16 24:00  A3-A5 与 B1-B5 两条分支并行；两边各自 push
11-17 09:00  A 完成 A3-A5；B 完成 B1-B5
11-17 12:00  PR #1（A3-A5）合并；B `git merge origin/main` 后开 PR #2
11-17 14:00  PR #2（B1-B5）合并；PR #3（CI）合并，main 全量 lint/test/build
11-17 15:00  打 v1.0.0、发 Release、截提交统计图
11-17 16:30  补截图、跑公网冒烟
11-17 17:30  录 3–5 分钟视频
11-17 19:00  两人各自发布博客（必须提前，审核要时间）
11-17 21:00  PSP、贡献比例、自查表、提交任务
11-17 23:59  截止（预留 3 小时缓冲）
```

---

## 12. 最后提醒

- **不要**用 GitHub 网页 “Upload files” 上传整包：作者归属会错，diff 不可读。
- **不要** `git add -A`：容易把对方的文件、`node_modules`、数据库一起提交。
- **不要**两台电脑共用同一个 GitHub 账号、PAT 或 SSH key；每台机器只登录本人。
- **不要**让 B 从空的 `main` 直接开工：先等 A 把 A0–A2 推到 `main`，B 再从 `main` 拉 `feat/b-product`。
- **不要**伪造提交日期或作者；提交时间就是现在，作业只看次数与内容。
- **不要**把 `.env.aliyun`、AccessKey、SSH 私钥、`*.db` 提交进仓库。
- 博客里如实写 AI 辅助编码的使用范围；AI 生成的代码本人要能讲清，否则“功能实现思路”会扣分。
- 提交前把 `assignment.md` 最后的自查表逐项打勾，别漏“两人各自发博客”和“PSP 每人一张”。
