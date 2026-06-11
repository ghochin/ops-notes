# 第 4 天：Shell 脚本 + 简历包装

## 一、Shell 脚本

### 什么是 Shell 脚本
把多条 Linux 命令写在一个 `.sh` 文件里，一次性执行。运维日常大量重复工作依赖脚本自动化。

### 脚本结构
```bash
#!/bin/bash              # 声明用 bash 解释器
DATE=$(date +%Y%m%d)     # 获取当前日期，存进变量 DATE
tar -czf 文件名 路径      # 打包压缩
echo "..."               # 输出提示信息
```

### 写过的两个脚本

**备份网页文件** (`/opt/backups/backup-html.sh`)：
```bash
#!/bin/bash
DATE=$(date +%Y%m%d)
tar -czf /opt/backups/html-$DATE.tar.gz /var/www/html/
echo "备份完成：html-$DATE.tar.gz"
```

**备份数据库** (`/opt/backups/backup-wp.sh`)：
```bash
#!/bin/bash
DATE=$(date +%Y%m%d-%H%M)
docker exec wordpress-db-1 mysqldump -u root -plinge6188 wordpress > /opt/backups/wp-db-$DATE.sql
echo "数据库备份完成：wp-db-$DATE.sql"
```

### chmod +x
给脚本文件添加执行权限，否则 bash 不能运行它。

## 二、Cron 定时任务

Cron 是 Linux 的定时任务系统。

| 格式 | `分 时 日 月 星期 命令` |
|------|--------------------------|
| `0 2 * * *` | 每天凌晨 2:00 |
| `0 9 * * 1-5` | 工作日早 9 点 |
| `*/5 * * * *` | 每 5 分钟 |
| `0 * * * *` | 每小时整点 |

**查看当前定时任务**：`crontab -l`
**编辑定时任务**：`crontab -e`

你当前的定时任务：
```
0 2 * * * bash /opt/backups/backup-html.sh && bash /opt/backups/backup-wp.sh
```
每天凌晨 2 点自动备份网页和数据库。

## 三、简历项目描述

以下两段可以直接用于简历的项目经历栏：

### 版本一：运维项目（精炼版，约100字）

**云服务器环境搭建与运维实践**
- 在阿里云 Ubuntu 24.04 服务器上独立完成 Nginx Web 服务搭建、配置与日志管理
- 使用 Docker + Docker Compose 实现 WordPress + MySQL 一键部署，配置数据卷实现持久化存储
- 编写 Shell 备份脚本并通过 Cron 实现每日自动备份
- 具备故障排查能力：通过 `docker logs`、`tail` 日志分析、配置检查等方式定位并修复服务异常

**涉及技术**：Linux/Ubuntu、Nginx、Docker、Docker Compose、MySQL、Shell/Bash、Cron

### 版本二：扩写版（面试口述用，约300字）

> 我在阿里云上部署了一台 Ubuntu 云服务器，从头搭建了完整的网站运维环境。先用 Nginx 做了静态网站，配了防火墙、看了日志、做了故障演练（改了错误路径制造 404 再修好）。然后用 Docker + Docker Compose 一键部署了 WordPress+MySQL，两个容器通过内部网络通信，数据通过 Docker Volume 持久化。配了镜像加速器解决了国内拉不了 Docker Hub 的问题。还写了 Shell 备份脚本，用 Cron 做了每日凌晨定时自动备份。整个流程从 SSH 连接开始到最终上线了两个可访问的网站地址，掌握了基础的 Linux 运维、容器化部署和故障排查。

### 面试可能会问的问题

**Q：为什么用 Docker 而不是直接在服务器上装？**
A：Docker 环境隔离，WordPress 和 MySQL 不会因为 PHP 版本、依赖库冲突互相影响。而且 docker-compose 一条命令就能重建，迁移服务器也方便。

**Q：Docker Compose 的 depends_on 是干什么的？**
A：确保 WordPress 容器在 MySQL 容器之后启动。但不保证 MySQL 内部已准备好接受连接，只是控制启动顺序。

**Q：备份是怎么做的？**
A：文件用 tar 打包，数据库用 mysqldump 导出 SQL 文件，然后用 Cron 每天凌晨 2 点自动执行备份脚本。

**Q：遇到过什么问题？**
A：Docker Hub 国内连不上，配了镜像加速器解决。还有一次删了容器重建，数据通过 volume 保留了下来，只是登录态丢了需要重新登录。

## 四、命令速查：Shell & Cron

```bash
# 创建脚本
nano 文件名.sh
# 添加执行权限
chmod +x 文件名.sh
# 运行脚本
bash 文件名.sh

# 查看定时任务
crontab -l
# 查看备份文件
ls -lh /opt/backups/
```
