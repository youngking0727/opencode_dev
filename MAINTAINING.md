# OpenBioMedClaw — Fork 维护规范

> 本文件位于 `core/dev` 分支。三层结构的导航卡片。  
> 累了/慌了/忘了的时候,先看这一页。

---

## 1. 三层分支速查

| 分支 | 这层只放 | 不放 |
|---|---|---|
| `sync/dev` | 上游 `anomalyco/opencode` 的纯镜像 | 你的任何改动 |
| `core/dev` | 改 OpenCode 原文件的品牌化(logo、文案、默认 prompt、XDG 路径) | 新功能、业务代码 |
| `product/dev` | 新增目录的业务代码(`biomedical/`、`openbiomedclaw-tools/` 等) | 改 OpenCode 原文件 |

**判断口诀**:
- 改的是 OpenCode 原本就有的文件 → `core/dev`
- 新增一个 OpenCode 没有的目录 → `product/dev`
- 同步上游 → `sync/dev`

---

## 2. 日常 Commit 规范

每个 commit 必带前缀,格式 `[前缀] 简短描述`:

| 前缀 | 用途 | 落在哪层 |
|---|---|---|
| `[upstream]` | 同步上游的 merge | sync/dev → core/dev → product/dev |
| `[brand]` | 品牌化(logo、文案、命令名、XDG 路径、默认 prompt) | core/dev |
| `[biomed]` | 生物医药业务功能(MCP 工具、Skills、UI 组件) | product/dev |
| `[chore]` | 工程配置(.gitignore、构建脚本、CI) | 看具体改啥,通常 core/dev |
| `[fix]` | 修 bug | 看修的哪层的 bug |
| `[doc]` | 改文档 | 看文档归属哪层 |

**示例**:
- `[brand] Replace OpenCode → OpenBioMedClaw in README header`
- `[biomed] Add PubMed search MCP tool`
- `[upstream] Merge upstream v1.1.0, resolved 8 conflicts`
- `[chore] gitignore: add NOTES.md`

---

## 3. 日常工作流(改东西时)

### 改品牌化(改 OpenCode 原文件)

```bash
git checkout core/dev
# 改代码
git add <files>
git commit -m "[brand] <描述>"
git push

# 同步到 product/dev(让业务分支拿到这次品牌改动)
git checkout product/dev
git merge core/dev
git push
```

### 加业务功能(新增 biomedical/ 等目录)

```bash
git checkout product/dev
# 加代码
git add biomedical/
git commit -m "[biomed] <描述>"
git push
```

业务功能**不需要**反向 merge 到 core 或 sync。

---

## 4. 同步上游 SOP(每月 1-2 次)

完整 5 步,跟着走就行。

```bash
# Step 1: 拉上游最新到本地
git fetch upstream

# Step 2: 把上游变化合到 sync/dev(0 冲突,因为 sync 没改过任何东西)
git checkout sync/dev
git merge upstream/dev
git push

# Step 3: 把 sync/dev 合到 core/dev(可能有品牌冲突)
git checkout core/dev
git merge sync/dev
# 若有冲突:
#   - 冲突文件多半是你改过品牌的(README、logo 路径、默认 prompt 等)
#   - 手动解决,保留你的品牌改动 + 上游的功能改动
#   - git add <file>; git commit
git push

# Step 4: 把 core/dev 合到 product/dev(几乎零冲突,因为业务代码全在新增目录里)
git checkout product/dev
git merge core/dev
git push

# Step 5: 验证
git log --oneline -10
bun install   # 上游可能新增依赖
bun run --cwd packages/opencode --conditions=browser src/index.ts web --port 4196 --hostname 127.0.0.1
# 浏览器访问 http://localhost:4196 看能不能跑
```

**同步失败的退路**:任何一步 merge 出问题,用 `git merge --abort` 撤销当前 merge,回到 merge 前的状态。

---

## 5. 紧急情况速查

### 不小心在错的分支上改了东西

```bash
# 先看当前位置
git branch     # 看 * 标记的那个

# 如果改动还没 commit:
git stash                    # 暂存
git checkout <正确分支>       # 切过去
git stash pop                # 取出来

# 如果已经 commit 但没 push:
git log --oneline -1         # 记下错误的 commit hash
git reset --hard HEAD~1      # 撤销最后一次 commit
git checkout <正确分支>
git cherry-pick <刚才记下的 hash>   # 把那个 commit 转移过来
```

### 不小心 push 到错分支

```bash
# 在错分支上撤销
git reset --hard HEAD~1
git push --force-with-lease   # 注意:--force-with-lease 比 --force 安全

# 然后在对的分支上重做这个 commit
```

### Merge 方向搞反了(把 product 合进 core)

```bash
git merge --abort            # 如果还没完成
# 或者
git reset --hard ORIG_HEAD   # 如果已经 merge 完了
```

---

## 6. 启动命令(开发用)

```bash
# 起 web server(单进程,前后端打包)
bun run --cwd packages/opencode --conditions=browser src/index.ts web --port 4196 --hostname 127.0.0.1

# 浏览器访问(通过 VSCode Remote SSH 端口转发)
# http://localhost:4196

# 注意:本服务器 4096 端口已被 bolong 用户的 OpenCode 占用,我们用 4196
```

---

## 7. 关键约束

- ❌ **不要**在 `sync/dev` 上做任何改动(它是镜像)
- ❌ **不要**在 `dev` 分支(原版)上改东西——保留它作为 fork 起点的历史快照
- ❌ **不要**直接 `git push --force` —— 用 `--force-with-lease`
- ❌ **不要**反向 merge:product/dev → core/dev 是错的,只允许 core → product
- ❌ **不要**忘记 commit 前缀

---

## 修订记录

| 日期 | 版本 | 内容 |
|---|---|---|
| 2026-05-10 | v0.1 | 初版,三层分支结构 + commit 规范 |
