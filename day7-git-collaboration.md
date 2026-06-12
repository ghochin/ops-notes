# 第 7 天：Git 协作（clone / pull）

## 一、GitHub 远程协作模型

```
本地A（你）—push→ GitHub —pull→ 本地B（另一个目录，模拟同事）
```

## 二、git clone — 入职第一天做的事

```bash
git clone git@github.com:ghochin/git-demo.git git-demo-clone
```

把 GitHub 上的仓库**完整复制**到本地。仓库的历史、分支、文件全部下来。

进公司第一天就是这句话——同事给你仓库地址，你 clone 到电脑，开始干活。

## 三、git pull — 每天开工第一件事

```bash
git pull
```

把远程仓库的新提交拉下来，合并到本地。

**每天先 pull 再改代码**。不然别人改的东西你没看到，容易冲突。

## 四、完整协作流程

```
早晨：git pull（拉别人的更新）
白天：改代码 → git add → git commit
下班前：git push（推自己的更新）
```

## 五、SSH 密钥配对

这次踩的坑：SSH 公钥没加到 GitHub，clone 时报 `Permission denied (publickey)`。

**一辈子做一次的事**：
1. `ssh-keygen -t ed25519 -C "邮箱"` 生成密钥
2. `type %USERPROFILE%\.ssh\id_ed25519.pub` 查看公钥
3. 去 GitHub → Settings → SSH and GPG keys → 添加公钥

加完之后所有 GitHub 操作走 SSH，不用反复输密码。

## 六、HTTPS vs SSH 对比

| 方式 | 地址 | 国内 | 需配 |
|------|------|------|------|
| HTTPS | `https://github.com/user/repo.git` | 经常超时 | 无 |
| SSH | `git@github.com:user/repo.git` | 相对稳 | 需加公钥一次 |

**建议**：两个都配好，HTTPS 不行就用 SSH。

## 七、今天的命令

```bash
git clone git@github.com:用户名/仓库名.git    # 克隆仓库
git pull                                       # 拉取远程更新
git push                                       # 推送本地更新（配了-u后可省参数）
```

## 八、你的 Git 全技能

| 命令 | 作用 |
|------|------|
| git init | 初始化仓库 |
| git add | 暂存文件 |
| git commit -m | 提交 |
| git status | 看状态 |
| git log --oneline | 看历史 |
| git checkout -b | 创建切换分支 |
| git merge | 合并分支 |
| git push | 推到远程 |
| git pull | 拉取远程更新 |
| git clone | 克隆仓库 |
