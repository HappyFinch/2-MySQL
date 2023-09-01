1. **插入数据**
- insert优化：批量插入、手动事务提交、主键顺序插入
- 大批量插入数据：使用load指令进行插入
    步骤：
    1. 客户端连接服务端时，加上参数 --local-infile

        ```mysql --local-infile - u root -p```

    2. 设置全局参数

        ```set global local_infile = 1;```

    3. 执行load指令

        ```load data local infile '/root/sql1.log' into table `tb_user` fields terminated by ',' lines terminated by '\n'```

2. **主键优化**
    尽量顺序插入（auto_increment），主键长度尽量短点

3. **order by优化**
    注意创建索引时的asc和desc
    explain的extra中：
    - Using filesort：需要将返回的结果在排序缓冲区进行排序，效率低
    - Using index：索引排序，性能高

4. **group by优化**
    索引，多字段恩组满足最左前缀法则

5. **limit优化**
    覆盖索引 + 子查询

6. **count优化**
    效率：count(字段) < count(主键) < count(1) 约等于 count(*)
    count(*)和count(1)都不取值，直接累加；而count(字段) 和 count(主键)都要把行里的值取出来，效率相对低
7. **update优化**
    - 尽量根据主键/索引进行更新数据
    - InnoDB的行锁是针对索引加的锁，不是针对记录加的锁，并且该索引不能失效，否则会从行锁升级为表锁，使得并发性能下降

