# 第 11 天：Uptime Kuma 监控面板

## 做了什么
- 拉取了 `louislam/uptime-kuma:1` 镜像
- 创建了容器（端口 3002:3001），但卡在端口冲突
- 旧容器已删除，新容器已创建（e898c294c382）

## 重启后要做

### 1. 连服务器
```
C:\Windows\System32\OpenSSH\ssh.exe root@8.163.121.232
```

### 2. 检查 uptime-kuma 状态
```
docker ps -a
docker logs uptime-kuma
```

### 3. 如果容器没跑起来（Exited），重启它
```
docker start uptime-kuma
```

### 4. 确认阿里云防火墙已开 3002 端口
→ 阿里云控制台 → 轻量应用服务器 → 防火墙 → 添加规则：3002 TCP

### 5. 浏览器访问
http://8.163.121.232:3002

### 6. 创建管理员账号，添加两个监控目标
- http://8.163.121.232（HTML 页面）
- http://8.163.121.232:8080（WordPress）


