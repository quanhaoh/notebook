## Redis持久化

### 持久化机制

#### RDB：默认方式，间隔一段时间将数据持久化到硬盘

1. 编辑.conf配置文件，修改持久化间隔时间和规则
   - save 10 5 ：十秒内发生5个key的变化就进行持久化操作
   - 重新启动Redis服务器，并制定配置文件名称

#### AOF：日志记录的方式，每次命令之后，持久化数据

1. 编辑.conf配置文件，修改持久化间隔时间和规则
   - appendonly no（关闭AOF）、appendonly yes（启动AOF）
   - appendfsync always：每一次操作都进行持久化
   - appendfsync everysec：每隔一秒进行持久化
   - appendfsync no：不进行持久化