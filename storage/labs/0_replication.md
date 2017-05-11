# Move Data Between Cluster #


----------
Generate 10 file in HDFS use teregen (Or using any existing data source)

    hadoop jar /opt/cloudera/parcels/CDH/lib/hadoop-0.20-mapreduce/hadoop-examples.jar teragen -Dmapred.map.tasks=10 -Dmapred.map.tasks.speculative.execution=false 10000000 /user/root/sourceData

Copy the source to dectination cluster:

    hadoop distcp hdfs://ec2-52-80-43-137.cn-north-1.compute.amazonaws.com.cn:8020/user/root/sourceData hdfs://ec2-54-223-212-211.cn-north-1.compute.amazonaws.com.cn:8020/user/root/targetData

Check result:

    hdfs fsck /user/root/sourceData -files -blocks
    hdfs fsck /user/root/targetData -files -blocks




