---
source_url: https://www.doubao.com/thread/wb7331b7dfc9ae6df
ingested: 2026-05-07
sha256: c469576b4bdac5a0fb18769e40ccb323af8b361e524e3ef33af87fcff37f5301
---

# 查询 proc 的含义

> 来源：https://www.doubao.com/thread/wb7331b7dfc9ae6df
> 用户：小蠢货 | 助手：豆包
> 共 8 轮对话

---

## 👤 用户

搜索一下

## 🤖 AI

### 一、/proc（Linux 虚拟文件系统）
**/proc**（procfs）是 Linux 内核提供的**虚拟/伪文件系统**，全程在内存中，不占磁盘空间，用于实时暴露内核与进程状态。
- 核心用途
  - **查看进程**：/proc/PID/ 对应每个进程的目录（cmdline、maps、fd 等）。
  - **硬件信息**：/proc/cpuinfo、/proc/meminfo、/proc/diskstats。
  - **内核参数**：/proc/sys/ 可动态修改（如 net.ipv4.ip_forward）。
  - **系统状态**：/proc/loadavg、/proc/uptime、/proc/net/。
- 常用示例
```bash
cat /proc/cpuinfo   # CPU 详情
cat /proc/meminfo   # 内存使用
ls /proc/$$/fd      # 当前进程文件句柄
```

### 二、编程中的 proc（procedure/process）
- **procedure（过程）**：无返回值的函数/子程序。
  - SQL：`CREATE PROC`（存储过程，简写 proc）。
  - 汇编：`PROC` 关键字定义过程。
- **process（进程）**：运行中的程序实例（OS 语境）。
- **Ruby Proc**：代码块对象，可闭包、可调用。
```ruby
p = Proc.new { |x| x * 2 }
p.call(3)  #=> 6
```

### 三、其他含义
- **缩写**：process（流程）、procedure（步骤）、procurement（采购）。
- **生物**：PROC（Protein C，蛋白 C）。

要不要我把 /proc 常用文件和目录整理成一份速查清单，附常见操作示例？

## 👤 用户

class User(Base):
        tablename = "users"
    
        id = Column(Integer, primary_key=True)
        name = Column(String(50))
        age = Column(Integer)详细解释一下

## 🤖 AI

这段代码是 **Python SQLAlchemy ORM** 的核心用法，我用**最通俗、最详细**的方式给你讲明白，保证你一看就懂。

# 完整正确代码（先补全）
```python
from sqlalchemy import Column, Integer, String
from sqlalchemy.orm import declarative_base

# 1. 创建基类（所有模型都要继承它）
Base = declarative_base()

# 2. 定义用户表模型
class User(Base):
    __tablename__ = "users"  # 数据库表名

    id = Column(Integer, primary_key=True)  # 主键ID
    name = Column(String(50))               # 姓名
    age = Column(Integer)                   # 年龄
```

---

# 逐行详细解释

## 1. `class User(Base):`
- 定义一个**模型类**，名字叫 `User`
- 继承自 `Base`（SQLAlchemy 的基类）
- **作用**：告诉 SQLAlchemy：这是一个**数据库表对应的类**

---

## 2. `__tablename__ = "users"`
- 必须写，固定格式
- 意思是：**这个类对应数据库里的 `users` 表**
- 表名可以自己改，比如 `user_info`、`t_user` 都行

---

## 3. `id = Column(Integer, primary_key=True)`
这是**表的第一列**：

- `Column`：表示这是一个**数据库字段（列）**
- `Integer`：字段类型是**整数**
- `primary_key=True`：表示这是**主键**
  - 唯一
  - 非空
  - 自动递增（默认行为）

**对应数据库语句：**
```sql
id INT PRIMARY KEY AUTO_INCREMENT
```

---

## 4. `name = Column(String(50))`
第二列：姓名

- `String(50)`：字符串类型，**最长50个字符**
- 不写 `primary_key`，就是普通字段

对应数据库：
```sql
name VARCHAR(50)
```

---

## 5. `age = Column(Integer)`
第三列：年龄

- `Integer`：整数类型
- 可以存 0、18、99 等数字

对应数据库：
```sql
age INT
```

---

# 整体一句话总结
这段代码的作用是：

**用 Python 类定义一个数据库表 `users`，包含 3 个字段：id（主键）、name（字符串）、age（整数）。**

以后你操作这个类，SQLAlchemy 会自动帮你生成 SQL 语句，不用手写增删改查。

---

# 它能做什么？（举例）
```python
# 新增用户
user1 = User(name="张三", age=20)
session.add(user1)
session.commit()

# 查询用户
user = session.query(User).first()
print(user.name, user.age)
```

---

### 总结
- `User` 类 = 数据库表
- `__tablename__` = 表名
- `id/name/age` = 表的列
- `Column(类型)` = 定义列的类型

如果你愿意，我还能教你：
- 怎么创建表
- 怎么增删改查
- 怎么加非空、唯一、默认值

你想学哪一部分？

## 👤 用户

1. ORM ── 把数据库表变成 Python 对象
    
    这是后端最重要的思维模型之一。看代码中的对应关系：
    
    python
    数据库表 "users"
    class User(Base):
        tablename = "users"
    
        id = Column(Integer, primary_key=True)
        name = Column(String(50))
        age = Column(Integer)
    
    
    数据库表格 users：
    
    ┌────┬──────────┬─────┐
    │ id │  name    │ age │
    ├────┼──────────┼─────┤
    │ 1  │ xiaoming │ 25  │
    │ 2  │ xiaohong │ 23  │
    └────┴──────────┴─────┘
    
    
    Python 中操作就像操作普通对象：
    python
    new_user = User(name="xiaoming", age=25)  # 创建新行
    db.add(new_user)                          # 插入数据库
    db.commit()                               # 保存
    
    
    这就是 ORM（对象关系映射） ── 你不用写 SQL 语句，用 Python 对象操作数据库。解释一下什么是orm

## 🤖 AI

我用**最简单、最形象、最不绕弯**的方式，一次性把 **ORM 到底是什么** 给你讲透，保证你彻底明白。

# 一句话解释 ORM
**ORM = 对象关系映射（Object Relational Mapping）**
它的作用只有一个：
**把数据库里的「表、行、列」，变成 Python 里的「类、对象、属性」。**

让你**不用写 SQL**，只用 Python 代码就能操作数据库。

---

# 用最通俗的比喻理解 ORM
你可以把 ORM 当成一个 **翻译官**：

- 你说 Python 话（创建对象、改属性）
- ORM 自动帮你翻译成 SQL 话（INSERT、UPDATE、SELECT）
- 数据库只懂 SQL，ORM 帮你们沟通

你不用懂数据库语法，照样能增删改查。

---

# 用你的代码举例：ORM 到底做了什么？

## 1. 数据库那边长这样
```sql
表名：users
列：id（整数，主键）、name（字符串）、age（整数）
```

## 2. ORM 把它变成 Python 类
```python
class User(Base):
    __tablename__ = "users"

    id = Column(Integer, primary_key=True)
    name = Column(String(50))
    age = Column(Integer)
```

这就是 **映射关系**：
- 表 → 类（User 类 = users 表）
- 一行数据 → 一个对象
- 一列 → 对象的一个属性

---

## 3. 你用 Python 对象操作数据库
```python
# 创建一个用户（相当于数据库里新增一行）
user = User(name="小明", age=20)

# 保存到数据库
db.add(user)
db.commit()
```

## ORM 自动在背后帮你生成 SQL：
```sql
INSERT INTO users (name, age) VALUES ('小明', 20);
```

**你完全没写 SQL，但数据库执行了 SQL。**
这就是 ORM 的魔力。

---

# ORM 的三大核心好处（一定要记住）
1. **不用写复杂 SQL**，代码更干净
2. **不怕 SQL 注入**，更安全
3. **一套代码适配多种数据库**（MySQL、SQLite、PostgreSQL 通用）

---

# 终极总结（最精简版）
**ORM = 让你用 Python 对象操作数据库，不用写 SQL。**

- 表 → 类
- 一行 → 一个对象
- 一列 → 对象属性
- 操作对象 = 操作数据库

---

如果你愿意，我可以继续教你：
- ORM 怎么查数据
- 怎么改数据
- 怎么删数据
- 怎么连表查询（一对多、多对多）

你想继续学哪一部分？

## 👤 用户

我懂了，这个orm就是为python操作数据库准备的

## 🤖 AI

对，**你总结得完全到位**👍

一句话定调：
✅ **ORM 就是专门给 Python 用的，把「操作数据库」变成「操作普通Python类和对象」」**

再帮你固化几个关键对应，记下来以后永远不会乱：
1. 数据库**表**  ↔  Python **类**（`User` 类）
2. 表里**每一行数据**  ↔  Python **实例对象**（`User(name="小明",age=20)`）
3. 表里**字段/列**  ↔  对象**属性**（`user.name`、`user.age`）
4. 增删改查  ↔  直接调对象、调用会话方法，**不用手写SQL**

核心价值就两点：
- 不用背、写繁琐 SQL
- 完全用 Python 语法思维操控数据库，后端开发标配

