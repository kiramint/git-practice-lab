# 实验报告 / LAB_REPORT

> 做题者填写本文件后，随功能分支一并提交，并在 PR 中带上。

## 基本信息

- **GitHub 用户名：** <!-- 填写 -->zyy355
- **功能分支名：** `lab/`<!-- 填写用户名 -->/lab/zyy355
- **PR 链接：** <!-- 开好 PR 后粘贴 -->origin upstream 已配置

## 阶段清单

- [ ] Stage 0 — Fork、clone、upstream、创建 `lab/<username>`
- [ ] Stage 1 — 工作区 / 暂存区 / commit
- [ ] Stage 2 — 分支、merge、冲突解决
- [ ] Stage 3 — 坏提交 + revert
- [ ] Stage 4 — 添加 submodule `third_party/kit`
- [ ] Stage 5 — fetch upstream、push、开 PR

## 简答题

### 1. 工作区、暂存区、已提交（commit）分别是什么？

工作区：电脑里面实际看到的文件夹，是正在写代码、改文件的地方
暂存区：改完代码之后，用git add把想提交的改动放进这里，准备下一次提交
已提交：永久保存到git的历史记录里，以后随时可以找回

### 2. `git pull` 和 `git fetch` 有何不同？你通常何时用哪个？

git fetch只是去远程仓库把最新的代码下载下来，但不会合并到当前的代码里；git pull相当于git fetch加git merge，下载完会自动合并。
我通常用fetch来安全地查看远程有什么更新，确认没问题再手动merge。如果想直接拉取并合并最新代码，就用pull

### 3. 什么情况下不应该对已推送的共享分支使用 `git reset --hard`？为什么？

多人协作且已经推送到远程（已共享）的分支
因为reset hard会直接删除历史记录，强行把指针回退。如果这么做并force push，会把别人已经提交的代码覆盖或抹除，导致其他人的本地仓库和远程仓库乱掉

### 4. 带 submodule 的仓库 clone 后子目录是空的，常见原因是什么？怎么处理？

默认的git clone不会自动下载子模块里面的内容，只会留下一个空文件夹
可以在克隆的时候加上参数：git clone --recurse-submodules <仓库地址>。如果已经克隆完了，可以在仓库里运行git submodule update --init --recursive来补下载

## 关键 commit（填写短 hash 或完整 hash）

| 说明 | Hash |
|------|------|
| Stage 1 第一次较好的 commit | |8902b12
| Stage 2 冲突解决（或合并）commit | | 103fc22
| Stage 3 revert 产生的 commit | |42dd574 
| Stage 4 submodule add 的 commit | | 730baf1
## Stage 2 补充

- 普通 merge 结果：□ 快进（fast-forward）　□ 合并提交（merge commit）　□ 其他：
- 冲突解决简述：删除冲突标记，保留了我想要的最终代码，重新git add和git commit

## Stage 5 补充

- 同步 `upstream/main` 时使用：□ merge
- 选择理由：为了保留完整的历史记录，不想改变已有的commit结构。

## 其他备注（可选）

<!-- 例如：只用了非交互式 add、环境限制、踩过的坑等 -->
