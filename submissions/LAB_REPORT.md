# 实验报告 / LAB_REPORT

> 做题者填写本文件后，随功能分支一并提交，并在 PR 中带上。

## 基本信息

- **GitHub 用户名：** simon6-smarter
- **功能分支名：** `lab/simon6-smarter`
- **PR 链接：** https://github.com/kiramint/git-practice-lab/pull/6

## 阶段清单

- [x] Stage 0 — Fork、clone、upstream、创建 `lab/<username>`
- [x] Stage 1 — 工作区 / 暂存区 / commit
- [x] Stage 2 — 分支、merge、冲突解决
- [x] Stage 3 — 坏提交 + revert
- [x] Stage 4 — 添加 submodule `third_party/kit`
- [x] Stage 5 — fetch upstream、push、开 PR

Stage 0 确认：`origin` 指向自己的 fork（simon6-smarter/git-practice-lab），`upstream` 指向原仓库（kiramint/git-practice-lab），`git remote -v` 两者均在。

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

## 关键 commit（填写短 hash 或完整 hash）

| 说明 | Hash |
|------|------|
| Stage 1 第一次较好的 commit | 3667436 |
| Stage 2 冲突解决（或合并）commit | 57b298e |
| Stage 3 revert 产生的 commit | cb27952 |
| Stage 4 submodule add 的 commit | 5220ea3 |

（Stage 3 被撤销的坏提交：c72f8bb）

## Stage 2 补充

- 普通 merge 结果：☑ 快进（fast-forward）　□ 合并提交（merge commit）　□ 其他：
- 冲突解决简述：第一次 merge 时 `lab/simon6-smarter` 自分支点后没有新提交，直接 fast-forward 到 `102a1ed`。随后故意在 lab 分支和 feature/greeting 分支各改 `workspace/greeting.md` 同一行并分别提交，merge 时 git 报 `CONFLICT (content)`、文件状态 `UU`。我把 `<<<<<<<` / `=======` / `>>>>>>>` 标记段改写成一句同时保留两边语意的话，删掉全部冲突标记后 `add` + `commit` 完成合并（`57b298e`）。判定"解决完成"的标准：文件里不再有任何冲突标记、`git status` 不再有 `UU`，`git log --graph` 能看到带两个父提交的 merge commit。

## Stage 5 补充

- 同步 `upstream/main` 时使用：☑ merge　□ rebase
- 选择理由：本次 `fetch upstream` 后 `upstream/main` 与我的分支基点相同（merge 结果 Already up to date），两种方式效果一致；一般情况下我选 merge，因为 lab 分支是要 push 交卷的共享历史，merge 不改写已有提交、对审阅者最友好。rebase 更适合还没推送、想把本地提交整理成一条直线再合入的场景。

## 其他备注（可选）

- 环境限制：非交互终端，无法用 `git add -p` 逐块暂存，改用"分轮编辑、分轮 add"的替代方案达到只暂存部分改动的效果（第一次 commit 只含第一部分，第二部分留在工作区）。
- 踩过的坑：`git reset` 把改动从暂存区搬回工作区后，我又执行了 `git restore`，结果把工作区里想保留的改动一并丢弃——教训是 restore 丢弃的是"全部未暂存改动"，想留的部分要先 commit 或 stash。该过程已如实记录在 notes.txt。
- 本机 GitHub 直连 DNS 被污染（解析到无效 IP），git/gh 网络操作通过本地中转完成。
- 加分项（新建空仓库首次 push 的步骤，未实际操作）：`gh repo create <name> --private` 或在网页建空仓 → 本地 `git init` → `git add -A && git commit` → `git remote add origin <url>` → `git push -u origin main`。
