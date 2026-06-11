# 第 5 天：Git 基础

## 一、Git 是什么

Git 是**版本控制系统**。记录文件的每一次修改，可以随时回到历史版本，可以多人协作不怕冲突。

**类比**：写论文时你可能会这样：
```
论文.docx → 论文_修改版.docx → 论文_最终版.docx → 论文_打死不改版.docx
```

Git 就是把这些做成一条时间线，随时可以回到任意版本。

## 二、核心概念

| 概念 | 含义 |
|------|------|
| **仓库 (repo)** | 一个被 Git 管理的文件夹，包含所有历史记录 |
| **暂存区 (staging area)** | 临时存放要提交的文件，`git add` 放入暂存区 |
| **提交 (commit)** | 把暂存区的改动正式记录到历史 |
| **分支 (branch)** | 一条独立的开发线，互不影响 |
| **合并 (merge)** | 把分支的改动合并到另一条分支 |
| **HEAD** | 指向当前所在的分支/提交 |

## 三、工作流程

```
工作目录（改文件） → git add → 暂存区 → git commit → 仓库历史
```

每次提交保存的是当前的**快照**，不是差异。

## 四、常用命令

### 仓库操作
```bash
git init                    # 初始化新仓库
git clone 仓库地址           # 从远程克隆仓库
```

### 查看状态
```bash
git status                  # 看哪些文件改过、哪些暂存了
git log                     # 看提交历史（详细）
git log --oneline           # 看提交历史（一行一个，简洁）
git log --oneline --all     # 看所有分支的提交历史
git diff                    # 看具体改了什么（未暂存的）
```

### 提交
```bash
git add 文件名              # 暂存某个文件
git add .                   # 暂存所有修改的文件
git commit -m "提交信息"    # 提交（必须有信息说明做了什么）
```

### 分支
```bash
git branch                  # 列出本地分支
git checkout -b 新分支名    # 创建并切换到新分支
git checkout 分支名          # 切换到已有分支
git merge 分支名             # 把指定分支合并到当前分支
```

### 配置
```bash
git config --global user.name "你的名字"     # 设置用户名
git config --global user.email "你的邮箱"    # 设置邮箱
```

## 五、今天走过的实操

1. `git init` — 在 `D:\36\ai create\git-demo` 建了仓库
2. 创建 README.md → git add → git commit
3. 创建 about.txt → git add → git commit
4. 创建 feature-test 分支 → 修改文件 → 提交
5. 切回 master → 文件不变（分支隔离）
6. `git merge feature-test` → master 合并了改动

## 六、面试常见 Git 问题

**Q：Git add 和 commit 的区别？**
A：add 把改动放入暂存区，commit 把暂存区的内容正式记录到仓库历史。add 是预备，commit 是确认。

**Q：什么是分支？**
A：分支是一条独立的开发线。通常 master/main 是稳定版本，开发新功能时开新分支，测好了再合并回 master。

**Q：merge 时出现冲突怎么办？**
A：Git 会在文件里标记冲突的地方，手动选择保留哪部分，然后 add + commit 完成合并。
```

## 七、你现在有的项目

| 项目 | 位置 | 内容 |
|------|------|------|
| git-demo | `D:\36\ai create\git-demo` | Git 练手仓库 |
| cloud-server-notes | `D:\36\ai create\` | 服务器运维笔记 |
| 线上服务器 | root@8.163.121.232 | Nginx + WordPress |
