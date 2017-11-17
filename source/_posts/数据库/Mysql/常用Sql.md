---
layout: post
title: 常用SQL
date: '2017-11-18 01:20'
toc: true
tags:
  - mysql
top: 1
---

# 批量更新
gist：[multi_update.php](https://gist.github.com/hlf513/035ae964db80e122c262)

# 在线动态抓取SQL
``` sh
tcpdump -i eth0 -s 0 -l -w - dst port 3306|strings
```

# 随机一条

``` sql
SELECT t1.*
FROM `table` AS t1 JOIN (SELECT ROUND(RAND() * (SELECT MAX(id) FROM `table`)) AS id) AS t2
WHERE t1.id >= t2.id
ORDER BY t1.id ASC LIMIT 1;
```

# 导出数据字典

gist：[generator_mysql_dict.php](https://gist.github.com/hlf513/3ccaf696d3cd335dcaca)

# 分页
SQL采用内连接（INNER JOIN）实现，更高效

```sql
select cols from tables inner join (
  select pk from tables
    where col1 = $col1 order by col2 limit 1000000,10
) using (pk);
```

# 检查数据表大小
1. 进去指定 schema 数据库（存放了其他的数据库的信息）
  ```sql
  mysql> use information_schema;
  Database changed
  ```

1. 查询所有数据的大小
  ```sql
  mysql> select concat(round(sum(DATA_LENGTH/1024/1024), 2), 'MB')
  -> as data from TABLES;
  +-----------+
  | data |
  +-----------+
  | 6674.48MB |
  +-----------+
  1 row in set (16.81 sec)
  ```

1. 查看指定数据库实例的大小，比如说数据库 forexpert
  ``` sql
  mysql> select concat(round(sum(DATA_LENGTH/1024/1024), 2), 'MB')
  -> as data from TABLES where table_schema='forexpert';
  +-----------+
  | data |
  +-----------+
  | 6542.30MB |
  +-----------+
  1 row in set (7.47 sec)
  ```

1. 查看指定数据库的表的大小，比如说数据库 forexpert 中的 member 表
  ```sql
  mysql> select concat(round(sum(DATA_LENGTH/1024/1024),2),'MB') as data
  -> from TABLES where table_schema='forexpert'
  -> and table_name='member';
  +--------+
  | data |
  +--------+
  | 2.52MB |
  +--------+
  1 row in set (1.88 sec)
  ```

# 使用 OR 时,使用 union all 替换

``` sql
select * from table where id = 1 union all select * from table where id =2;

```

# 导入导出数据

## 导入
- 常规使用 values 导入
  字段顺序必须一致
  ``` sh
  mysql>source ~/dump.sql
  或
  mysql -u root -p database_name < ~/dump.sql
  ```

- 使用 load data infile
  方式一：直接导入
  ``` sh
  #!/bin/bash
  #$1,数据文件的绝对地址
  username='root'
  password=''
  host='127.0.0.1'
  database="thirdsite_grab"
  table="resumes_contacts"
  field="src,src_no,resume_updated_at,name,phone,tel,email,create_at,updated_at,is_deleted,status,icdc_id,error_msg,source,type"
  if [ -f "$1" ]
  then
  mysql -u $username -p$passwoed -h $host $database -e "load data local infile '$1' replace into table $table ($field)"
  else
  echo $1'不存在'
  fi
  ```

  方式二：不更新索引，加快导入速度
  1. 执行 `FLUSH TABLES` 语句或命令 `mysqladmin flush-tables`
  1. 使用`myisamchk –keys-used=0 -rq /path/to/db/tbl_name`。这将从表中取消所有索引的使用
  1. 用`LOAD DATA INFILE`把数据插入到表中
  1. 用`myisamchk -r -q /path/to/db/tbl_name`重新创建索引.
    这将在写入磁盘前在内存中创建索引树，并且它更快，因为避免了大量磁盘搜索。结果索引树也被完美地平衡
  1. 执行`FLUSH TABLES`语句或`mysqladmin flush-tables`命令。

  更简洁的方式二：
  使用这种方式，不需要执行`FLUSH TABLES`。
  1. 使用`ALTER TABLE tbl_name DISABLE KEYS`代替`myisamchk –keys-used=0 -rq/path/to/db/tbl_name`，
  1. 使用`ALTER TABLE tbl_name ENABLE KEYS`代替`myisamchk -r -q/path/to/db/tbl_name`。


## 导出

1. 导出 sql
  ``` sh
  mysqldump -uxxx -pxxx -h127.0.0.1 database_name table_name -t --where="where条件" > dump.sql
  ```

2. 导出文本文件
  ``` sh
  #!/bin/bash
  username='root'
  password=''
  host='127.0.0.1'
  database="thirdsite_grab"
  table="resumes_contacts"
  if [ -n "$1" ]
  then
  save_path=$1
  else
  save_path=/tmp/export.data
  fi
  if [ -f "$save_path" ]
  then
  echo '删除旧文件'
  `rm $save_path`
  fi
  echo '保存路径为'$save_path
  mysql -u $user -p$password $database -h $host -Ne "set names UTF8;select $field from $table" > $save_path
  ```

# 删除重复的数据

demo：表名；
id：自增 ID；
site：具有相同数据的列

## 只有 crud 权限

* 删除 ID 大的数据
  ```sql
  delete from a using demo as a, demo as b where (a.id > b.id) and (a.site = b.site);
  ```

* 删除 ID 小的数据
  ``` sql
  delete from a using demo as a, demo as b where (a.id < b.id) and (a.site = b.site);
  ```

## 有索引权限

``` sql
//自动删除重复数据（id 大的删除）
alter ignore table demo add unique index ukey (site);
//删除刚建立的索引
alter table demo drop index ukey;
```


<!--以下是脚注-->
