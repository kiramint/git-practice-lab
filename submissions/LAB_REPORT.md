# 实验报告 / LAB_REPORT

> 做题者填写本文件后，随功能分支一并提交，并在 PR 中带上。

## 基本信息

- **GitHub 用户名：** canlangoo 
- **功能分支名：** `lab/canlangoo`
- **PR 链接：** https://github.com/kiramint/git-practice-lab/pull/2

## 阶段清单

- [x] Stage 0 — Fork、clone、upstream、创建 `lab/<username>`
- [x] Stage 1 — 工作区 / 暂存区 / commit
- [x] Stage 2 — 分支、merge、冲突解决
- [x] Stage 3 — 坏提交 + revert
- [x] Stage 4 — 添加 submodule `third_party/kit`
- [x] Stage 5 — fetch upstream、push、开 PR

## 简答题

### 1. 工作区、暂存区、已提交（commit）分别是什么？

工作区：正在编写的文件，其中作出的改动还未被git记录
暂存区：工作区完成改动的文件，下一次commit的快照
commit：写进历史，无法改变的快照


### 2. `git pull` 和 `git fetch` 有何不同？你通常何时用哪个？

fetch只是把远程的新提交下载到本地的分支，不会改动当前的分支和工作区，pull会直接改动当前分支。我通常用fetch看清上游有什么新提交再决定合并方式。

### 3. 什么情况下不应该对已推送的共享分支使用 `git reset --hard`？为什么？

不应该对已经推送并且其他人基于其进行分支工作的分支使用 。reset --hard会强行回退到某个历史节点，之前的提交就会被抹除。已经推送的就只能push --force覆盖远程历史，其他人的提交会被顶替掉。

### 4. 带 submodule 的仓库 clone 后子目录是空的，常见原因是什么？怎么处理？

子模块是个独立的仓库，父仓库只会记录它与某个commit有关。默认clone不会拉取子模块的内容，所以目录存在但是里面是空的。解决方法是用"git clone  --recurse-submodules <url>"

## 关键 commit（填写短 hash 或完整 hash）

| 说明 | Hash |
|------|------|
| Stage 1 第一次较好的 commit |8e60e0defcf78f6efe0394143f9e0466f914e950 |
| Stage 2 冲突解决（或合并）commit | be21534a4ab3e5f4c3296d8278ac11e5ab390374|
| Stage 3 revert 产生的 commit |08852147c65c0b386b665eb7486bfc22ee5178de |
| Stage 4 submodule add 的 commit |a0e2d9bd6d547db82fa7e27b87a02298d8a5682a |

## Stage 2 补充

- 普通 merge 结果：☑ 快进（fast-forward）　□ 合并提交（merge commit）　□ 其他：
- 冲突解决简述：feature/greeting-conflict 与 lab/canlangoo 都改了workspace/greeting.md 第 3 行；merge 时产生冲突，删掉三行标记后合并为"Hello from the lab and conflict branches."，git add 后 commit 完成合并。

## Stage 5 补充

- 同步 `upstream/main` 时使用：☑ merge　□ rebase
- 选择理由：分支已经推送，merge不会改写历史，无需force push。fetch后执行merge返回Already up to date，说明上游没有当前分支未包含的提交。

## 其他备注（可选）

<!-- 例如：只用了非交互式 add、环境限制、踩过的坑等 -->
