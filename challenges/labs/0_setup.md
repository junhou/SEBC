# Challenge Setup #

----------

Cloud provider: AWS

    172.31.1.179   ip-172-31-1-179.cn-north-1.compute.internal   
    172.31.7.26ip-172-31-7-26.cn-north-1.compute.internal   
    172.31.8.99ip-172-31-8-99.cn-north-1.compute.internal   
    172.31.14.160  ip-172-31-14-160.cn-north-1.compute.internal  
    172.31.1.134   ip-172-31-1-134.cn-north-1.compute.internal   


Linux distribute:
Cent os 6.7


yum repolist:

    [centos@ip-172-31-1-179 ~]$ yum repolist
    Loaded plugins: fastestmirror, presto
    Determining fastest mirrors
     * base: mirrors.zju.edu.cn
     * extras: mirrors.sohu.com
     * updates: mirrors.sohu.com
    base  | 3.7 kB 00:00
    base/primary_db   | 4.7 MB 00:00
    extras| 3.4 kB 00:00
    extras/primary_db |  37 kB 00:00
    updates   | 3.4 kB 00:00
    updates/primary_db| 821 kB 00:00
    repo id   repo namestatus
    base  CentOS-6 - Base  6,706
    extrasCentOS-6 - Extras   64
    updates   CentOS-6 - Updates 270
    repolist: 7,040
    

Disk:
    
    [centos@ip-172-31-1-179 ~]$ df -lh
    Filesystem  Size  Used Avail Use% Mounted on
    /dev/xvda1  7.8G  691M  6.7G  10% /
    tmpfs   7.8G 0  7.8G   0% /dev/shm


User:

    zhou:x:2800:2800::/home/zhou:/bin/bash
    chen:x:2900:2900::/home/chen:/bin/bash

Group:

    shanghai:x:2901:chen
    beijing:x:2902:zhou




