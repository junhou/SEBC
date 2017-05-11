# HDFS Snapshot #

----------
**In case of accidential remove operation, HDFS provide a trash machinism (automatically save for a period) and snapshot (manual saved check point)**

Create a directory for snapshot and put some sampel data:

    hadoop fs -mkdir /user/root/precious
    hadoop fs -put 6-cm5.8/abc.zip /user/root/precious

Enable snapshot:

    sudo -su hdfs hdfs dfsadmin -allowSnapshot /user/root/precious

Create snapshot:

    sudo -su hdfs hdfs dfs -createSnapshot /user/root/precious sebc-hdfs-test

Varify if a directory has snapshot:

    hdfs lsSnapshottableDir
    drwxr-xr-x 0 root root 0 2017-05-11 01:38 1 65536 /user/root/precious

----------

Snapshot recoviry (Simply copy the file in snapshoted directory):

    hdfs dfs -cp precious/.snapshot/sebc-hdfs-test/abc.zip /user/root/precious







