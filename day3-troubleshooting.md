# 第 3 天：故障排查

## 一、排障三步法

运维修故障永远是这个流程：

```
发现问题 → 查状态/日志定位根因 → 修复 → 验证
```

不要上来就改配置、重启。先确定是什么坏了，再对症修。

## 二、故障 1：数据库挂了

**症状**：WordPress 报 `Error establishing a database connection`

**定位**：
```bash
docker ps -a | grep db
# 看到 STATUS: Exited — 说明容器停了
```

**修复**：
```bash
docker start wordpress-db-1
```

**验证**：刷新网页，恢复正常。

**教训**：WordPress = PHP 程序 + MySQL。MySQL 是唯一存数据的地方，MySQL 挂了整个站就没了。所以生产环境数据库一般要配主从复制或备份。

## 三、故障 2：Nginx 配错导致 404

**制造故障**：
```bash
sed -i 's|root /var/www/html;|root /var/www/notexist;|' /etc/nginx/sites-enabled/default
systemctl reload nginx
```

**症状**：浏览器返回 `404 Not Found`

**定位**：
1. 先做语法检查：`nginx -t`（没问题就说明不是语法错，是逻辑错）
2. 看访问日志：`tail -5 /var/log/nginx/access.log`
   - 403/404 看 access.log（Nginx 在正常工作，只是找不到文件）
   - 500/502 看 error.log（程序内部出错了）

**修复**：
```bash
sed -i 's|root /var/www/notexist;|root /var/www/html;|' /etc/nginx/sites-enabled/default
systemctl reload nginx
```

**重要区别**：
- 改 Nginx 配置用 `reload`（不停服加载新配置），不是 `restart`（停掉再启动）
- `nginx -t` 只查语法，不查路径是否存在——所以改了不存在的路径也能通过检查

## 四、故障 3：容器删了，数据还在吗

**操作**：
```bash
docker compose -f /opt/wordpress/docker-compose.yml down  # 删除容器
docker compose -f /opt/wordpress/docker-compose.yml up -d  # 重建
```

**结果**：WordPress 内容全部保留，只需重新登录。

**原理**：docker-compose.yml 里的 `volumes` 数据卷是独立于容器的持久存储。
```
容器 = 锅（可以扔）   数据卷 = 冰箱里的菜（永远在）
```

**注意**：需要重新登录是因为 WordPress 的登录态（session）存在 PHP 容器里，不是 MySQL 里。容器删了 session 就没了。

## 五、常用排障命令

| 场景 | 命令 |
|------|------|
| 看容器状态 | `docker ps -a` 或 `docker ps` |
| 看容器日志 | `docker logs 容器名` |
| 看容器实时日志 | `docker logs -f 容器名` |
| 看 Nginx 状态 | `systemctl status nginx` |
| 看 Nginx 错误日志 | `tail -20 /var/log/nginx/error.log` |
| 看 Nginx 访问日志 | `tail -20 /var/log/nginx/access.log` |
| 检查 Nginx 配置语法 | `nginx -t` |
| 不中断服务重载配置 | `systemctl reload nginx` |
| 查看数据卷 | `docker volume ls` |
| 查看端口监听 | `ss -tlnp` |

## 六、关于安全

你的访问日志里出现了不认识的 IP `39.100.71.117` 在扫你的 80 端口。互联网上任何公网 IP 都会被自动扫描器不停探测。这是正常的，不用慌。但长期来看：

1. 不要用弱密码（你现在 WordPress 密码带字母数字，够了）
2. 不要在生产服务器上跑没用过的脚本
3. 轻量服务器的防火墙规则要定期检查

## 七、核心心法

> 运维不是"不挂"——运维是"挂了知道为什么、知道怎么修、知道怎么不挂第二次"。
