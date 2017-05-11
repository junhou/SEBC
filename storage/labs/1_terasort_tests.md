# Hadoop Benchmark #

----------
To simplify the operation, turn HDFS ACL or use hdfs account to do the following task:

    <property>  
    <name>dfs.permissions.enabled</name>  
    <value>false</value>  
    </property> 
     
    <property>  
    <name>dfs.namenode.acls.enabled</name>  
    <value>false</value>  
    </property> 

TO calculate some iuput parameter in taragen:
Num of line: 

10 GB = 10 * 1000 MB * 1000KB * 1000B = 10000000000
10 GB / 100B per line = 100000000


Taragen:

    time hadoop jar /opt/cloudera/parcels/CDH/lib/hadoop-mapreduce/hadoop-mapreduce-examples.jar teragen -D dfs.block.size=32m 10000000 /user/root/teragen


Tarasort:

    time hadoop jar /opt/cloudera/parcels/CDH/lib/hadoop-mapreduce/hadoop-mapreduce-examples.jar terasort /user/root/teragen /user/root/teragen/sorted





