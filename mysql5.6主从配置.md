# 修改配置：
`log-bin=mysql-bin`
`server-id=152`

# 创建mester语句：
`GRANT REPLICATION SLAVE,RELOAD,SUPER ON *.* TO 'rep'@'193.168.2.45' IDENTIFIED BY 'test';`
`show master status;`

# 创建slave的语句：
`change master to master_host='193.168.0.42',master_user='rep',master_password='test',master_log_file='mysql-bin.000001',master_log_pos=454;`
`start slave;`
`show slave status;`
