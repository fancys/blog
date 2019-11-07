
# mssql查看死锁和解锁
--查看被锁表
```sql
select   request_session_id   锁表进程,OBJECT_NAME(resource_associated_entity_id) 被锁表名
from   sys.dm_tran_locks where resource_type='OBJECT';
```

--解锁
```sql
declare @spid  int
Set @spid  = 55 --锁表进程
declare @sql varchar(1000)
set @sql='kill '+cast(@spid  as varchar)
exec(@sql)
```