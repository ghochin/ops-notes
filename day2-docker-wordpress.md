# 第 2 天：Docker + WordPress 部署

## 一、Docker 是什么

Docker 是一个**容器引擎**。你可以把容器理解成一个轻量级的"小虚拟机"，但它比虚拟机快得多、省资源。

**类比**：传统部署 = 你直接在桌子上组装配件（各种环境冲突）；Docker = 每个软件自己带了一个盒子里所有东西，互不干扰。

**核心概念**：
- **镜像(Image)**：一个打包好的软件包，比如 `wordpress:latest`、`mysql:8.0`
- **容器(Container)**：镜像跑起来之后的实例
- **Docker 仓库(Registry)**：下载镜像的地方，默认是 Docker Hub（国内连不上，所以配了阿里云加速）

## 二、Docker 常用命令

| 命令 | 作用 |
|------|------|
| `docker pull 镜像名` | 从仓库下载镜像 |
| `docker images` | 列出本机已有的镜像 |
| `docker run 镜像名` | 用镜像启动一个容器 |
| `docker ps` | 列出正在运行的容器 |
| `docker ps -a` | 列出所有容器（含已停止的） |
| `docker stop 容器名` | 停止容器 |
| `docker start 容器名` | 启动已停止的容器 |
| `docker restart 容器名` | 重启容器 |
| `docker logs 容器名` | 查看容器日志 |
| `docker exec -it 容器名 bash` | 进入容器内部命令行 |
| `docker rm 容器名` | 删除容器 |
| `docker rmi 镜像名` | 删除镜像 |

## 三、Docker Compose

`docker-compose.yml` 是用来**一次性定义和启动多个容器**的配置文件。比如 WordPress 需要两个容器（wordpress + mysql），用 compose 可以一起管理。

```yaml
services:
  db:                          # 服务名，容器间通过服务名相互访问
    image: mysql:8.0           # 用的镜像
    environment:               # 环境变量（设置密码、库名等）
    volumes:                   # 数据持久化，不丢数据
    restart: always            # 容器挂了自动重启

  wordpress:
    image: wordpress:latest
    ports:
      - "8080:80"              # 左边是你服务器的端口，右边是容器内端口
    environment:
      WORDPRESS_DB_HOST: db    # 这里直接用服务名 db，docker 内部自动解析
    depends_on:                # 依赖关系，先等 db 启动
      - db

volumes:                       # 声明数据卷
  db_data:
```

**compose 命令**：
| 命令 | 作用 |
|------|------|
| `docker compose up -d` | 后台启动所有服务 |
| `docker compose down` | 停止并删除所有服务 |
| `docker compose down -v` | 停止并删除服务 + 删数据卷（数据没了！） |
| `docker compose ps` | 查看 compose 管理的容器 |
| `docker compose logs -f` | 看所有容器实时日志 |

## 四、Docker 镜像加速

国内直接访问 Docker Hub 会被墙。解决办法：在 `/etc/docker/daemon.json` 配镜像加速器。

```json
{
  "registry-mirrors": ["https://docker.1panel.live"]
}
```

改了配置文件后要重启 Docker：`systemctl restart docker`

## 五、阿里云防火墙

轻量应用服务器自带防火墙（类似你家路由器的防火墙），需要在控制台手动开放端口。

**Web 界面路径**：阿里云控制台 → 轻量应用服务器 → 点机器 → 防火墙 → 添加规则

**默认开放的端口**：
- 22（SSH）
- 80（HTTP）
- 443（HTTPS）
- 3389（RDP，远程桌面）

**你本次开放的**：8080 TCP

## 六、Nginx 站点配置

默认站点配置文件：`/etc/nginx/sites-enabled/default`

关键指令：
```
root /var/www/html;           # 网站文件根目录
index index.html;              # 默认首页文件名
server_name _;                 # _ 表示匹配所有域名
try_files $uri $uri/ =404;    # 文件找不到就返回 404
```

改网页的流程：
1. 编辑 `/var/www/html/` 下的文件
2. Nginx 实时读取，无需重启（静态文件）
3. 浏览器刷新即可看到变化

## 七、今天踩过的坑

1. **Docker Hub 拉不下来** → 配镜像加速器。第一个加速器（阿里云 registry.cn-hangzhou）不起作用，换成 `docker.1panel.live` 才拉成功。不同的镜像加速器效果不一样，一个不行换另一个。

2. **YAML 缩进错误**：`docker compose up -d` 报 `did not find expected key`，因为 YAML 靠缩进（空格）来识别层级结构。`volumes:` 必须和 `services:` 同级对齐。

3. **多行命令卡在 `>`**：终端里 heredoc（`<<EOF`）要等结束标记才能执行。如果多行粘错，`Ctrl+C` 退出重来。一行命令 `echo '...' | tee file` 更稳。

4. **Docker 容器端口映射**：`"8080:80"` 的格式是 `宿主端口:容器端口`。WordPress 容器内监听 80，映射到你服务器的 8080 端口。

## 八、你现在有什么

| 资源 | 地址 |
|------|------|
| 自建 HTML 页面 | http://8.163.121.232 |
| WordPress 博客 | http://8.163.121.232:8080 |
| 服务器 SSH | root@8.163.121.232 |
| 笔记文件夹 | D:\36\ai create\ |

## 九、命令速查

```bash
# Docker
docker ps                              # 看运行中的容器
docker images                          # 看本机镜像
docker compose up -d                   # 启动 compose 项目
docker compose down                    # 停止 compose 项目
docker logs 容器名                      # 看容器日志

# 系统
systemctl restart docker              # 重启 Docker 服务
cat /etc/docker/daemon.json           # 看 Docker 配置

# Nginx
cat /etc/nginx/sites-enabled/default  # 看默认站点配置
ls /var/www/html/                     # 看网站文件
```
