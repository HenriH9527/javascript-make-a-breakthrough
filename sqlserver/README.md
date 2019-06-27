#  SQL Server从入门到放弃

#### 数据库常用对象

1. 表
2. 字段
3. 索引
4. 视图
5. 存储过程

### 数据库组成

1. 文件
   - 主要数据文件   .mdf
   - 次要数据文件   .ndf
   - 事务日志文件   .ldf
2. 文件组
   - 主文件组
   - 用户定义文件组



### 创建数据库

- 用户界面创建
- 代码创建

```sql
create database mrkj  --创建数据库  mrkj
on                    --主数据文件
(name = mrdat,        --name 文件名,filename 文件路径
 filename='F:\SQL\mrkj.mdf',
 size=10,             --文件大小
 maxsize=100,         --最大值
 filegrowth=5         --标识增量
)
log on                --事物日志文件
(name='mingrilog',
 filename='F:\SQL\mrkj.ldf',
 size=8mb,
 maxsize=50mb,
 filegrowth=8mb+++
)

exec sp_renamedb 'mr','mrsoft'    --数据库重命名
```

##### 参数

ON：指定显示定义用来

### 修改数据库

- [x] 更改数据库文件
- [x] 添加和删除文件组
- [x] 更改选项
- [x] 更改跟踪
- [x] 更改权限
- [x] 更改扩展属性
- [x] 更改镜像
- [x] 更改事物日志传送

#### 更改方式：

1. 视图界面更改
2. 使用 ALTER DATABASE 语句修改数据库

```sql
ALTER DATABASE Mingri                     --更改数据库
ADD FILE								  --添加文件					
(
NAME=mrkj,                                --文件名
Filename='G:\mrkj.ndf',                   --路径
size=10MB,                                --大小
Maxsize=100MB,                            --最大值
Folegrowth=2MB                            --标识增量
)
```



### 删除数据库

###### 删除数据库时必须满足以下条件：

- [ ] 如果涉及日志传送操作，在删除之前必须取消日志传送操作
- [ ] 若要删除为事务复制发布的数据库，或删除为合并复制发布或订阅的数据库，必须首先从数据库中删除备份。如果数据库已损坏，不能删除备份，可以先将数据库设置为脱机状态，然后再删除数据库
- [ ] 如果数据库上存在数据库快照，必须首先删除数据库快照。

#### 删除方式

1. 用户试图界面删除
2. 使用 DROP DATABASE 语句删除数据库

```sql
DROP DATABASE database_name
```



### 数据表基础

##### 基本数据类型

![1561533393416](C:\Users\Administrator\AppData\Roaming\Typora\typora-user-images\1561533393416.png)