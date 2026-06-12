# 第 8 天：SQL + Python 银行技能

## 一、SQL 速查

```sql
-- 查全部
SELECT * FROM wp_users;

-- 查指定列
SELECT ID, user_login, user_email FROM wp_users;

-- 条件筛选
SELECT * FROM wp_posts WHERE post_status = 'publish';

-- 模糊搜索
SELECT * FROM wp_posts WHERE post_title LIKE '%关键词%';

-- 分组统计
SELECT post_status, COUNT(*) AS count FROM wp_posts GROUP BY post_status;

-- 排序
SELECT * FROM wp_posts ORDER BY post_date DESC;

-- 限制条数
SELECT * FROM wp_posts LIMIT 5;

-- 插入
INSERT INTO 表名 (列1, 列2) VALUES (值1, 值2);

-- 更新（一定要带 WHERE！）
UPDATE 表名 SET 列 = 新值 WHERE 条件;

-- 删除（一定要带 WHERE！）
DELETE FROM 表名 WHERE 条件;
```

**如何连接 MySQL**：
```bash
docker exec -it wordpress-db-1 mysql -u root -p密码
```

然后：
```sql
USE wordpress;
SHOW TABLES;
```

## 二、Python 核心技能

### 1. 调用系统命令（Python 跑 Shell）
```python
import subprocess
result = subprocess.run(["dir"], shell=True, capture_output=True, text=True)
print(result.stdout)
```

### 2. 文件读写
```python
with open("文件名", "w", encoding="utf-8") as f:
    f.write("内容\n")
    f.write("银行里经常要写日志\n")

with open("文件名", "r", encoding="utf-8") as f:
    content = f.read()
```

### 3. JSON（系统间数据交换）
```python
import json
data = json.loads(json字符串)
print(data["name"])

# 带 try/except 保护
try:
    data = json.loads(raw_text)
    print(data["balance"])
except json.JSONDecodeError:
    print("格式错误")
except KeyError as e:
    print(f"缺少字段: {e}")
```

### 4. CSV（导出 Excel 报表）
```python
import csv

# 写
data = [["姓名", "金额"], ["张三", 100]]
with open("报表.csv", "w", newline="", encoding="utf-8-sig") as f:
    csv.writer(f).writerows(data)

# 读
with open("报表.csv", "r", encoding="utf-8-sig") as f:
    for row in csv.reader(f):
        print(row)
```

### 5. 数据处理（遍历 + 条件）
```python
data = [{"name": "张三", "score": 85}, ...]

for item in data:
    if item["score"] > 80:
        print(f"{item['name']}: {item['score']}分")
```

### 6. 异常处理
```python
try:
    可能出错的代码
except 具体错误类型:
    出错后做什么
except Exception as e:
    print(f"未知错误: {e}")
```

## 三、建行面试 Python 可能问题

**Q：你看得懂 Python 脚本吗？**
A：能。我写过处理 JSON、读写 CSV 的脚本，用 subprocess 在 Python 里调度系统命令。Shell 脚本也能写（我有定时备份的 cron 任务）。

**Q：代码报错了你怎么排查？**
A：用 try/except 捕获具体异常类型，打日志定位。如果是别人的代码，从报错行数往前看调用链。

**Q：Python 里怎么操作数据库？**
A：我没直接用 Python 连过 MySQL，但我会长 Shell 脚本里用 docker exec 调 mysqldump 和 mysql 命令做备份和查询。

## 四、你现在面向建行实习的技能栈
Linux · Nginx · Docker · MySQL · SQL · Shell(Bash) · Cron · Python · Git · GitHub · 阿里云 · Excel(基础) · CET-4
