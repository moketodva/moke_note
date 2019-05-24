### 各种备份概念

```
冷备：cold backup，下线后备份
温备：warm backup，全局施加共享锁，只能读，不能写
热备：hot backup，不离线，读写都能正常进行
完全备份：full backup
部分备份：partial backup
物理备份：physical backup，拷贝文件
逻辑备份：logical backup，读出数据存入文件
增量备份：increment backup
差异备份：fidderential backup
```

### mysqldump
> mysql自带的逻辑备份工具，但属于温备

例子：

```
mysqldump -u user -p --all-databases > /usr/local/mysql/backup/db_'date +%F'.sql
```

常用参数：

```bash
--all-databases #备份所有库
--databases db1 db2... #备份指定的多个库
--lock-all-tables #锁定所有表之后在备份
--lock-tables #锁定指定的表
--single-transaction #能够对InnoDB存储引擎实现热备
--events #备份事件调度器代码
--routines #备份存储过程和存储函数
--triggers #备份触发器
--flush-logs #备份前，请求到锁之后滚动日志
--master-data=[0|1|2] #0表示不记录,1表示记录change master语句,2记录为注释的change master语句
```

### xtrabackup(热备，待续)
