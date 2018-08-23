# 从"show  slave  status\G"的输出中，找到如下信息：
    Relay_Master_Log_File: dd-bin.002540     //slave库已读取的master的binlog```
    Exec_Master_Log_Pos: 950583017           //在slave上已经执行的position位置点```
    
# 停掉slave，以slave已经读取的binlog文件，和已经执行的position为起点，重新设置同步。会产生新的中继日志，问题解决。（不需要指定host，user，password等，默认使用当前已经设置好的）
    mysql>stop slave;
    mysql>change master to master_log_file='$(Relay_Master_Log_File):dd-bin.002540' master_log_pos=$(Exec_Master_Log_Pos):950583017;
    mysql>start slave;
    
# 如果是GUID的话用：
    mysql>stop slave;
    mysql>change master to master_auto_position='$(Exec_Master_Log_Pos):950583017';
    mysql>start slave;
