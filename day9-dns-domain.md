# 第 9 天：域名注册 + DNS 解析

## 域名信息
- 域名：**yoorchin.xyz**
- 注册商：阿里云
- 价格：几元/年（.xyz 后缀）
- 服务器 IP：8.163.121.232

## 域名是什么
域名是 IP 的名字。浏览器输入 yoorchin.xyz → DNS 翻译 → 连上 8.163.121.232 → 你的服务器。

## DNS 解析记录（Aliyun Cloud DNS 已添加）
| 主机记录 | 类型 | 记录值 |
|---------|------|--------|
| @ | A | 8.163.121.232 |
| www | A | 8.163.121.232 |
| blog | A | 8.163.121.232 |

## 状态
- DNS 同步延迟中（当日注册，次日生效）
- 等待生效后验证：`nslookup yoorchin.xyz`

## 明天要做
1. 验证 DNS 生效：`nslookup yoorchin.xyz` 或 `ping yoorchin.xyz`
2. 连服务器配置 Nginx 反向代理：
   - `yoorchin.xyz` → 显示你的 HTML 页面（/var/www/html/）
   - `blog.yoorchin.xyz` → 代理到 WordPress（localhost:8080）
3. 配 Let's Encrypt 免费 SSL 证书
