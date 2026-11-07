# 实验报告 / LAB_REPORT

> 做题者填写本文件后，随功能分支一并提交，并在 PR 中带上。

## 基本信息

- **GitHub 用户名：** <TheTop123-lab>
- **功能分支名：** `lab/`<TheTop123-lab>
- **PR 链接：** <!-- 开好 PR 后粘贴 -->

## 阶段清单

- [x] Stage 0 — Fork、clone、upstream、创建 `lab/<username>`
- [x] Stage 1 — 工作区 / 暂存区 / commit
- [x] Stage 2 — 分支、merge、冲突解决
- [x] Stage 3 — 坏提交 + revert
- [ ] Stage 4 — 添加 submodule `third_party/kit`
- [ ] Stage 5 — fetch upstream、push、开 PR

## 简答题

### 1. 工作区、暂存区、已提交（commit）分别是什么？

<工作区是编辑，改动的区域/暂存区是改动后等待提交的区域/已提交是已经提交的区域>

### 2. `git pull` 和 `git fetch` 有何不同？你通常何时用哪个？

<!-- 你的回答 -->

### 3. 什么情况下不应该对已推送的共享分支使用 `git reset --hard`？为什么？

<当分支已经推送到共享仓库，比如github,不可使用reset --hard,因为它会强行篡改历史记录，导致其他协作者拉取代码时发生严重冲突甚至代码丢失，这时必须用revert,因为它会生成一个新的提交来抵消之前的修改，不会破坏历史>

### 4. 带 submodule 的仓库 clone 后子目录是空的，常见原因是什么？怎么处理？

<!-- 你的回答 -->

## 关键 commit（填写短 hash 或完整 hash）

| 说明 | Hash |
|------|------|
| Stage 1 第一次较好的 commit |ed69c7c |
| Stage 2 冲突解决（或合并）commit |4591bf6 |
| Stage 3 revert 产生的 commit |7678ad3 |
| Stage 4 submodule add 的 commit | |

## Stage 2 补充

- 普通 merge 结果：□ 快进（fast-forward）　[x]合并提交（merge commit）　□ 其他：
- 冲突解决简述：在feature和lab分支同时修改了greeting.md同一行，合并时产生冲突
，手动保留了功能分支的修改，删除了git自动生成的冲突符号
## Stage 5 补充

- 同步 `upstream/main` 时使用：□ merge　□ rebase
- 选择理由：

## 其他备注（可选）

<!-- 例如：只用了非交互式 add、环境限制、踩过的坑等 -->
