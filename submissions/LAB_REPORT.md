# 实验报告 / LAB_REPORT

> 做题者填写本文件后，随功能分支一并提交，并在 PR 中带上。

## 基本信息

- **GitHub 用户名：** simon6-smarter
- **功能分支名：** `lab/simon6-smarter`
- **PR 链接：** （重交 PR 创建后填回）

## 阶段清单

- [x] Stage 0 — Fork、clone、upstream、创建 `lab/<username>`
- [x] Stage 1 — 工作区 / 暂存区 / commit
- [x] Stage 2 — 分支、merge、冲突解决
- [x] Stage 3 — 坏提交 + revert
- [x] Stage 4 — 添加 submodule `third_party/kit`
- [x] Stage 5 — fetch upstream、push、开 PR

Stage 0 确认：`origin` 指向自己的 fork（simon6-smarter/git-practice-lab），`upstream` 指向原仓库（kiramint/git-practice-lab），`git remote -v` 两者均在。

---

## 作业要求逐条自检（重交补充说明）

上一次 PR 主动撤回，原因是**没有先把 `docs/ASSIGNMENT.md` 的要求读懂就交卷**。
这次按每个 Stage 的「要求动作 / 交卷要写什么」重新核对一遍，列成下表：

| 阶段 | 作业要求的动作 | 实际做到 | 可核对的证据 |
| --- | --- | --- | --- |
| Stage 0 | Fork → clone **自己的 fork** → 添加 `upstream` → 从默认分支建 `lab/<username>` | 四项齐全 | `git remote -v` 同时有 `origin` 和 `upstream`；分支 `lab/simon6-smarter` |
| Stage 1 | 改 `notes.txt`；用 `status` / `diff` / `diff --staged` 观察两区差异；练习只暂存部分改动；一次清晰 commit；再用 `restore` / `reset` 搬动改动 | 全部做过 | 第一次好 commit `3667436`；第二轮笔记 `680dc0d`；`notes.txt` 留有 `MM` 状态与 restore 教训；搬动步骤见下方专节 |
| Stage 2 | 建 `feature/greeting` → 提交 → merge 回 lab 分支 → **故意改同一行**制造冲突 → 解决并完成合并提交 | 全部做过 | 首次 merge 为 fast-forward（`102a1ed`）；冲突解决合并提交 `57b298e`，`git log --graph` 可见两个父提交 |
| Stage 3 | 新增 `oops.txt` 做坏提交 → 用 `log` 定位 → **优先 `git revert`** 撤销 | 用 revert，未对共享历史用 `reset --hard` | 坏提交 `c72f8bb`；revert 新提交 `cb27952`；revert 后 `workspace/oops.txt` 已不存在 |
| Stage 4 | `git submodule add https://github.com/kiramint/git-practice-kit.git third_party/kit`；查看 `kit/VERSION`、`kit/TIPS.md` 确认可读；提交 `.gitmodules` 与指针 | 全部做过 | 提交 `5220ea3`；`.gitmodules` 记录了 URL；gitlink 指向 `c0e8c49`；`kit/VERSION` 为 `1.0.0`，`kit/TIPS.md` 内容可读 |
| Stage 5 | `fetch upstream` → 同步 `upstream/main`（写明 merge / rebase 及理由）→ `push -u origin lab/<username>` → 按 PR 模板开 PR、标题规范 | 全部做过 | 同步方式见下方；本次 PR 正文使用 `.github/PULL_REQUEST_TEMPLATE.md` 并勾选全部清单 |

### 上次被撤回的具体原因（复盘）

不是阶段动作没做，而是**交卷方式没按作业要求来**：

1. PR 正文只写了一句话，没有使用仓库自带的 PR 模板，阶段清单一个都没勾；
   而 Stage 5 明确写了「正文可使用仓库自带的 PR 模板；确认 LAB_REPORT 与清单已勾选」。
2. 报告缺少几处「交卷要写什么」点名的内容：Stage 1 的三区搬动步骤、
   Stage 3 的坏提交 hash、Stage 4 对 `kit/VERSION` / `kit/TIPS.md` 的确认。
3. 报告里的 PR 链接指向已关闭的旧 PR，没有反映重交后的实际链接。

本次已逐项补齐：模板正文 + 全清单勾选 + 上述三项说明 + 更新后的 PR 链接。

---

## 简答题

### 1. 工作区、暂存区、已提交（commit）分别是什么？

- **工作区**：磁盘上正在编辑的文件本体，`git diff` 看到的是它和暂存区的差异。
- **暂存区**：`git add` 之后的"待提交清单"，是下一次 commit 的快照草稿，`git diff --staged` 看的是它和 HEAD 的差异。本次练习中我曾让同一文件处于 `MM` 状态（部分改动已暂存、部分还在工作区），证明两区可以各装一部分。
- **已提交**：`git commit` 后进入本地仓库历史的不可变快照，每个 commit 有唯一 hash，可被 log/revert/merge 引用。

### 2. `git pull` 和 `git fetch` 有何不同？你通常何时用哪个？

`git fetch` 只把远端的新提交下载到本地（更新 `origin/main`、`upstream/main` 这类远端跟踪分支），不动你当前的工作分支；`git pull` = `fetch` + 立刻 `merge`（或 rebase）进当前分支。想先看看上游改了什么再决定怎么合，用 fetch；确认远端就是要的内容、想一步到位时，才用 pull。本次 Stage 5 我就是先 `fetch upstream` 再手动 merge，过程可控。

### 3. 什么情况下不应该对已推送的共享分支使用 `git reset --hard`？为什么？

只要分支已经 push 且别人可能基于它工作，就绝不该用 `reset --hard` 回退后强推。因为 reset 是改写历史：别人本地的提交和你会分叉，他们下次 pull 会产生诡异的合并甚至丢工作。已共享的历史只能用 `git revert` 这类"新增一个反向提交"的方式撤销——历史只增不改，协作者无感。`reset --hard` 只适合还没 push 的纯本地分支。

### 4. 带 submodule 的仓库 clone 后子目录是空的，常见原因是什么？怎么处理？

常见原因：父仓库只记录子模块的指针（一个 commit SHA + `.gitmodules` 里的 URL），普通 `git clone` 默认不初始化、不拉取子模块内容，所以 `third_party/kit` 只是个空目录。处理办法二选一：

- clone 时一步到位：`git clone --recurse-submodules <url>`
- 已经 clone 了再补：`git submodule update --init --recursive`

（更新指针的原理：进子模块目录 checkout/拉取到新 commit，回父仓库 `add` + `commit`，把新 SHA 记进父仓库。）

---

## Stage 1 补充：三个区域之间搬动改动的具体步骤

作业要求「用 `restore` / `reset` 选合适的，在工作区与暂存区之间搬动改动，并记录做了哪几步、结果如何」：

1. 第一次只编辑 `notes.txt` 的前半部分 → `git add notes.txt` → 此时 `git diff` 为空、
   `git diff --staged` 能看到这部分改动 → `git commit`，得到 `3667436`。
2. 继续编辑同一文件追加第二轮笔记，先 `git add` 一部分，再改一点但不 add。
   这时 `git status` 显示 `MM`：第一列 M 是暂存区相对 HEAD 有改动，
   第二列 M 是工作区相对暂存区还有改动。用 `git diff` 和 `git diff --staged`
   分别看，两边内容不同，直观证明「两个区可以各装一部分」。
3. 用 `git reset` 把暂存区的改动搬回工作区（不动文件内容），
   再用 `git restore` 丢弃工作区改动。这里踩了坑：
   `restore` 丢的是**全部**未暂存改动，把本来想留的那部分也一起删了。
   教训是想保留的部分必须先 commit 或先 `stash`。
   该过程已如实记在 `workspace/notes.txt` 里，随后重新编辑并提交为 `680dc0d`。

因环境是非交互终端，无法用 `git add -p` 逐块暂存，改用「分轮编辑 + 分轮 add」
达到只暂存部分改动的效果，并在报告中说明了替代方案。

---

## 关键 commit（填写短 hash 或完整 hash）

| 说明 | Hash |
|------|------|
| Stage 1 第一次较好的 commit | 3667436 |
| Stage 2 冲突解决（或合并）commit | 57b298e |
| Stage 3 被撤销的坏提交 | c72f8bb |
| Stage 3 revert 产生的 commit | cb27952 |
| Stage 4 submodule add 的 commit | 5220ea3 |

## Stage 2 补充

- 普通 merge 结果：☑ 快进（fast-forward）　□ 合并提交（merge commit）　□ 其他：
- 冲突解决简述：第一次 merge 时 `lab/simon6-smarter` 自分支点后没有新提交，直接 fast-forward 到 `102a1ed`。随后故意在 lab 分支和 feature/greeting 分支各改 `workspace/greeting.md` 同一行并分别提交，merge 时 git 报 `CONFLICT (content)`、文件状态 `UU`。我把 `<<<<<<<` / `=======` / `>>>>>>>` 标记段改写成一句同时保留两边语意的话，删掉全部冲突标记后 `add` + `commit` 完成合并（`57b298e`）。判定"解决完成"的标准：文件里不再有任何冲突标记、`git status` 不再有 `UU`，`git log --graph` 能看到带两个父提交的 merge commit。

## Stage 3 补充

- 坏提交：`c72f8bb`（新增 `workspace/oops.txt`，内容为无意义文字）。
- 用 `git log --oneline` 定位后执行 `git revert c72f8bb`，生成 `cb27952`。
- 结果：`oops.txt` 被删除，但 `c72f8bb` 仍留在历史里，历史只增不改——
  这正是作业要求「优先使用 revert、适合已分享历史」的原因。

## Stage 4 补充

- `.gitmodules` 中路径为 `third_party/kit`，URL 为 `https://github.com/kiramint/git-practice-kit.git`。
- 父仓库提交的是 gitlink（指向子模块 commit `c0e8c49`），不是子模块全部文件。
- 已按要求查看子模块内容并确认可读：`kit/VERSION` 为 `1.0.0`，
  `kit/TIPS.md` 里也写了 `--recurse-submodules` / `submodule update --init` 两种 clone 办法，
  与简答题第 4 题互相印证。

## Stage 5 补充

- 同步 `upstream/main` 时使用：☑ merge　□ rebase
- 选择理由：本次 `fetch upstream` 后 `upstream/main` 与我的分支基点相同（merge 结果 Already up to date），两种方式效果一致；一般情况下我选 merge，因为 lab 分支是要 push 交卷的共享历史，merge 不改写已有提交、对审阅者最友好。rebase 更适合还没推送、想把本地提交整理成一条直线再合入的场景。

## 其他备注（可选）

- 环境限制：非交互终端，无法用 `git add -p` 逐块暂存，改用"分轮编辑、分轮 add"的替代方案达到只暂存部分改动的效果（第一次 commit 只含第一部分，第二部分留在工作区）。
- 踩过的坑：`git reset` 把改动从暂存区搬回工作区后，我又执行了 `git restore`，结果把工作区里想保留的改动一并丢弃——教训是 restore 丢弃的是"全部未暂存改动"，想留的部分要先 commit 或 stash。该过程已如实记录在 notes.txt。
- 本机 GitHub 直连 DNS 被污染（解析到无效 IP），git/gh 网络操作通过本地中转完成。
- 加分项（按作业说明「不必真的建第二个作业仓」，仅写步骤、未实际操作）：`gh repo create <name> --private` 或在网页建空仓 → 本地 `git init` → `git add -A && git commit` → `git remote add origin <url>` → `git push -u origin main`。
