# 第 10 天：Gitee 仓库 + 运维配置版本管理

## Gitee 账号
- 用户名：**ghochin6**
- 已加 SSH 公钥
- ops-notes 已推送：https://gitee.com/ghochin6/ops-notes

## 新建仓库
- **ops-notes**（学习笔记）——已推送
- **ops-configs**（运维配置文件）——待创建+推送

## 明天要做

### 1. 从服务器拉配置文件到本地
在 CMD 里执行：
```
C:\Windows\System32\OpenSSH\ssh.exe root@8.163.121.232 cat /etc/nginx/sites-enabled/default > D:\36\ai create\nginx-site.conf
C:\Windows\System32\OpenSSH\ssh.exe root@8.163.121.232 cat /opt/wordpress/docker-compose.yml > D:\36\ai create\docker-compose.yml
C:\Windows\System32\OpenSSH\ssh.exe root@8.163.121.232 cat /opt/backups/backup-html.sh > D:\36\ai create\backup-html.sh
C:\Windows\System32\OpenSSH\ssh.exe root@8.163.121.232 cat /opt/backups/backup-wp.sh > D:\36\ai create\backup-wp.sh
```

### 2. 推 ops-configs 到 Gitee
```
D:
cd D:\36\ai create\ops-configs
git init
copy D:\36\ai create\nginx-site.conf .
copy D:\36\ai create\docker-compose.yml .
copy D:\36\ai create\backup-html.sh .
copy D:\36\ai create\backup-wp.sh .
git add .
git commit -m "运维配置文件：Nginx + Docker Compose + Shell备份脚本"
git remote add origin git@gitee.com:ghochin6/ops-configs.git
git push -u origin main
```

### 3. 继续做运维项目
- Nginx 反向代理（不用域名，纯 IP 版：WordPress 关 8080 端口对外，Nginx proxy_pass 转发）
- Uptime Kuma 监控面板（docker run 一条命令，10 分钟搞定）

### 4. 追安检暑假工
