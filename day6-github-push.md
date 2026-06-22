# 第 6 天：GitHub 远程仓库

## 一、GitHub 是什么

GitHub 是托管 Git 仓库的网站——你的代码不只是存在本地，还能存一份到云端。好处：不怕电脑坏了丢代码；能和别人协作；面试时面试官能看到你写的项目。

## 二、关联远程仓库

```bash
git remote add origin 仓库地址    # 把本地仓库和远程仓库关联
git branch -M main                # 把主分支改名 main（GitHub 默认用 main）
git push -u origin main           # 推送代码到远程，-u 设置默认追踪
```

**remote 是什么意思**：本地仓库可以关联多个远程仓库，`origin` 是默认名字，习惯上指 GitHub/Gitee 上的那个主仓库。

**push 是什么意思**：把本地的提交"推"到远程。相当于本地写完了，上传到云端备份。

## 三、SSH vs HTTPS

访问 GitHub 有两种方式：

| 方式 | 地址格式 | 国内稳定性 |
|------|---------|-----------|
| HTTPS | `https://github.com/用户名/仓库.git` | 经常被墙，不稳定 |
| SSH | `git@github.com:用户名/仓库.git` | 相对更稳，但需配密钥 |

**日常建议**：HTTPS 能通则通（无需额外配置），HTTPS 不通就换 SSH。

## 四、SSH 密钥配置

```bash
ssh-keygen -t ed25519 -C "你的邮箱"   # 生成密钥对
type %USERPROFILE%\.ssh\id_ed25519.pub  # 查看公钥
```

把公钥添加到 GitHub → Settings → SSH and GPG keys → New SSH key。

然后切换地址：
```bash
git remote set-url origin git@github.com:用户名/仓库.git
```

## 五、Git 完整命令清单

### 本地操作
| 命令 | 作用 |
|------|------|
| `git init` | 初始化仓库 |
| `git add 文件名` | 暂存文件 |
| `git add .` | 暂存所有修改 |
| `git commit -m "信息"` | 提交 |
| `git status` | 查看工作区状态 |
| `git log --oneline` | 查看提交历史（简洁版） |
| `git diff` | 查看未暂存的改动 |
| `git checkout -b 分支名` | 创建并切换分支 |
| `git checkout 分支名` | 切换已有分支 |
| `git merge 分支名` | 合并分支到当前分支 |
| `git branch` | 列出分支 |

### 远程操作
| 命令 | 作用 |
|------|------|
| `git remote add origin 地址` | 关联远程仓库 |
| `git remote -v` | 查看远程仓库地址 |
| `git remote set-url origin 地址` | 修改远程仓库地址 |
| `git push -u origin main` | 推送并设默认分支 |
| `git push` | 推送到远程（设过 -u 后可以省参数） |
| `git clone 地址` | 克隆远程仓库到本地 |
| `git pull` | 拉取远程更新到本地 |

### 配置
| 命令 | 作用 |
|------|------|
| `git config --global user.name "名字"` | 设置用户名（显示在提交记录里） |
| `git config --global user.email "邮箱"` | 设置邮箱 |
| `git config --global --list` | 查看所有配置 |

## 六、踩过的坑

1. **GitHub 连不上**：国内 HTTPS 经常被干扰。如果加速器不行，换 SSH 方式。
2. **fatal: not a git repository**：你没在执行 `git init` 的目录里。用 `cd` 进到正确目录。
3. **error: remote origin already exists**：之前关联过了，不用重新 add，直接 push。
4. **Permission denied**：浏览器登录的是另一个 GitHub 账号，退出换正确的。
5. **SSH 首次连接问 yes/no**：和 SSH 连服务器一样，第一次要确认指纹。


