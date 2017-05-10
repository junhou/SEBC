# My SQL HA setup #

## Master side setting ##

Adding below code into /etc/my.cnf
    
    [mysqld]    
    server-id = 1
    relay-log = /var/lib/mysql/mysql-relay-bin
    relay-log-index = /var/lib/mysql/mysql-relay-bin.index
    master-info-file = /var/lib/mysql/mysql-master.info
    relay-log-info-file = /var/lib/mysql/mysql-relay-log.info
    log-bin = /var/lib/mysql/mysql-bin
    
Then restart mysql:

    service mysqld restart

Add a user for slave sync with master:

    mysql> CREATE USER 'repl'@'%' IDENTIFIED BY 'Pass123$';
    mysql> GRANT REPLICATION SLAVE ON *.* TO 'repl'@'%' IDENTIFIED BY 'Pass123$';
    mysql> FLUSH PRIVILEGES;
    mysql> FLUSH TABLES WITH READ LOCK;
    mysql> SHOW MASTER STATUS;

Record the value in output: 'File','Position'

    mysql> show master status;
    +------------------+----------+--------------+------------------+-------------------+
    | File | Position | Binlog_Do_DB | Binlog_Ignore_DB | Executed_Gtid_Set |
    +------------------+----------+--------------+------------------+-------------------+
    | mysql-bin.000003 |  154 |  |  |   |
    +------------------+----------+--------------+------------------+-------------------+
    

Dump all master db data:

    mysqldump -u root -p --all-databases --master-data > /root/dbdump.db

    mysql> UNLOCK TABLES;
    mysql> quit;


scp /root/dbdump.db root@VM2:/root/

---

##On Slave side:

Adding below code into /etc/my.cnf

    server-id = 2
    relay-log = /var/lib/mysql/mysql-relay-bin
    relay-log-index = /var/lib/mysql/mysql-relay-bin.index
    master-info-file = /var/lib/mysql/mysql-master.info
    relay-log-info-file = /var/lib/mysql/mysql-relay-log.info
    log-bin = /var/lib/mysql/mysql-bin

Manually sync Master DB to slave


    mysql -u root -p < /root/dbdump.db
    /etc/init.d/mysqld restart
    mysql -u root -p



change master to master_host='172.31.19.91', master_user='repl', master_password='Pass123$';

start slave;


    mysql> stop slave ;
    mysql> CHANGE MASTER TO MASTER_HOST='172.31.19.91', MASTER_USER='repl', MASTER_PASSWORD='Pass123$', MASTER_LOG_FILE='mysql-bin.000003', MASTER_LOG_POS=154;
    mysql> start slave;
    mysql> show slave status \G
    

    mysql> SHOW SLAVE STATUS \G
    *************************** 1. row ***************************
       Slave_IO_State: Waiting for master to send event
      Master_Host: 172.31.19.91
      Master_User: repl
      Master_Port: 3306
    Connect_Retry: 60
      Master_Log_File: mysql-bin.000003
      Read_Master_Log_Pos: 154
       Relay_Log_File: mysql-relay-bin.000002
    Relay_Log_Pos: 320
    Relay_Master_Log_File: mysql-bin.000003
     Slave_IO_Running: Yes
    Slave_SQL_Running: Yes
      Replicate_Do_DB:
      Replicate_Ignore_DB:
       Replicate_Do_Table:
       Replicate_Ignore_Table:
      Replicate_Wild_Do_Table:
      Replicate_Wild_Ignore_Table:
       Last_Errno: 0
       Last_Error:
     Skip_Counter: 0
      Exec_Master_Log_Pos: 154
      Relay_Log_Space: 527
      Until_Condition: None
       Until_Log_File:
    Until_Log_Pos: 0
       Master_SSL_Allowed: No
       Master_SSL_CA_File:
       Master_SSL_CA_Path:
      Master_SSL_Cert:
    Master_SSL_Cipher:
       Master_SSL_Key:
    Seconds_Behind_Master: 0
    Master_SSL_Verify_Server_Cert: No
    Last_IO_Errno: 0
    Last_IO_Error:
       Last_SQL_Errno: 0
       Last_SQL_Error:
      Replicate_Ignore_Server_Ids:
     Master_Server_Id: 1
      Master_UUID: fbf5326a-3487-11e7-a279-02e54c651bbe
     Master_Info_File: /var/lib/mysql/mysql-master.info
    SQL_Delay: 0
      SQL_Remaining_Delay: NULL
      Slave_SQL_Running_State: Slave has read all relay log; waiting for more updates
       Master_Retry_Count: 86400
      Master_Bind:
      Last_IO_Error_Timestamp:
     Last_SQL_Error_Timestamp:
       Master_SSL_Crl:
       Master_SSL_Crlpath:
       Retrieved_Gtid_Set:
    Executed_Gtid_Set:
    Auto_Position: 0
     Replicate_Rewrite_DB:
     Channel_Name:
       Master_TLS_Version:
    1 row in set (0.00 sec)
    


references:
https://www.tecmint.com/how-to-setup-mysql-master-slave-replication-in-rhel-centos-fedora/


    
