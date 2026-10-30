# 实验报告 / LAB_REPORT

> 做题者填写本文件后，随功能分支一并提交，并在 PR 中带上。

## 基本信息

- **GitHub 用户名：** Xu-kang-Xing
- **功能分支名：** `lab/`Xu-kang-Xing
- **PR 链接：** <!-- 开好 PR 后粘贴 -->

## 阶段清单

- [1] Stage 0 — Fork、clone、upstream、创建 `lab/<username>`
- [1] Stage 1 — 工作区 / 暂存区 / commit
- [1] Stage 2 — 分支、merge、冲突解决
- [1] Stage 3 — 坏提交 + revert
- [1] Stage 4 — 添加 submodule `third_party/kit`
- [1] Stage 5 — fetch upstream、push、开 PR

## 简答题

### 1. 工作区、暂存区、已提交（commit）分别是什么？

工作区就是工作区，差不多相当于做题时的全部题目步骤加草稿，或者说就是不使用git时的正常使用文件的区域。
暂存区感觉就是一次对当前工作区的保存 但可以选择保存并提交哪些部分而不需要修改工作区的草稿和未完成的内容
commit就是将现在暂存区的东西提交为一个完整的“版本”并保存下来。

### 2. `git pull` 和 `git fetch` 有何不同？你通常何时用哪个？

git fetch只会把更新拉过来显示 git pull会直接自己尝试合并  当自己的代码和目前的最新版本没有多少冲突代码的时候感觉用pull比较方便（？）其他大多数时候基本都得先fetch一下看一眼先吧（）保证不出问题再合并

### 3. 什么情况下不应该对已推送的共享分支使用 `git reset --hard`？为什么？

在这个东西已经推送到线上并且有人已经进行了拉取或者基于他在开发的时候不能用 好像是因为会修改哈希值 导致历史出错然后让别人的程序出bug崩溃之类的（）反正就是会有别人用就不要用吧

### 4. 带 submodule 的仓库 clone 后子目录是空的，常见原因是什么？怎么处理？

直接clone不会拉取伴侣仓库的代码 因为那个位置只有一个指针没有代码本身  可以在克隆前在后面加上--recurse-submodules 或者克隆后用git submodule update --init 

## 关键 commit（填写短 hash 或完整 hash）

| 说明 | Hash |
|------|------|
| Stage 1 第一次较好的 commit |9673fab|
| Stage 2 冲突解决（或合并）commit |263e3ca|
| Stage 3 revert 产生的 commit |715dfce|
| Stage 4 submodule add 的 commit |0a23b30|

## Stage 2 补充

- 普通 merge 结果：■ 快进（fast-forward）　□ 合并提交（merge commit）　□ 其他：
- 冲突解决简述：冲突时多了两行<<<之类的标识 里面装了两个分支的冲突代码 直接删了其中的一段并做了修改 之后直接进行了提交

## Stage 5 补充

- 同步 `upstream/main` 时使用：■ merge　□ rebase
- 选择理由：rebase也有和之前reset类似的问题 会导致其他人已拉取的分支出bug 不太敢用（）并且更熟悉merge

## 其他备注（可选）
stage 1 先加入了3-6行的内容，并使用git add workspace/notes.txt进行暂存
之后使用git status查看修改状态，在文件中添加了7-9行内容后使用git diff进行比较并使用git add -p暂存
后将换行时误加入的空白字符误用删除，并直接使用git add .进行暂存 现将尝试直接进行暂存并将其提交
<!-- 例如：只用了非交互式 add、环境限制、踩过的坑等 -->
