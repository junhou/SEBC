#Pre-install#

----------

##Deploy AMS cluster##

Number of Instance:5
Instance type: t2.xlarge

IP:

172.31.19.91      VM1.HJ.SEBC.EC2.AWS  VM1

172.31.17.189     VM2.HJ.SEBC.EC2.AWS  VM2

172.31.29.238     VM3.HJ.SEBC.EC2.AWS  VM3

172.31.19.151     VM4.HJ.SEBC.EC2.AWS  VM4

172.31.27.114     VM5.HJ.SEBC.EC2.AWS  VM5

##SSH Key-Exchange (Optional)##

Key-Exchange.sh:
    #for host_no in {"vm1","vm2","vm3","vm4","vm5"}
    #do
       #echo "#Remove SSH keypiar ${host_no} ..."  
       #ssh -t -i "/home/centos/SEBC_KEY.pem" centos@${host_no} "sudo #rm -r /root/.ssh/"
    #done
    
    for host_no in {"vm1","vm2","vm3","vm4","vm5"}
    do
       echo "#Operation on ${host_no} ..."
       ssh -t -i "/home/centos/SEBC_KEY.pem" centos@${host_no} "sudo hostname -i"
       ssh -t -i "/home/centos/SEBC_KEY.pem" centos@${host_no} "sudo ssh-keygen -t rsa"
       ssh -t -i "/home/centos/SEBC_KEY.pem" centos@${host_no} "sudo scp -p /root/.ssh/id_rsa.pub root@172.31.19.91:/root/.ssh/authorized_keys_${host_no}"
       sudo cat /root/.ssh/authorized_keys_${host_no} >>/root/.ssh/authorized_keys
       sudo rm /root/.ssh/authorized_keys_${host_no}
    done
    
    for host_no in {"vm1","vm2","vm3","vm4","vm5"}
    do
       echo "#Operation on ${host_no} ..."
       sudo scp -p /root/.ssh/authorized_keys root@${host_no}:/root/.ssh/authorized_keys
       ssh -t -i "/home/centos/SEBC_KEY.pem" centos@${host_no} "sudo chmod 755 ~; sudo chmod 700 ~/.ssh; sudo chmod 600 ~/.ssh/authorized_keys"
    done


##Service Check##
check.sh:

    for host_no in {"vm1","vm2","vm3","vm4","vm5"}
    do
       echo "#Operation on ${host_no} ..."
       echo "Memory:"
       ssh -t -i "/home/centos/SEBC_KEY.pem" centos@${host_no} "sudo cat /proc/meminfo | grep \"MemTotal\""
       echo "CPU:"
       ssh -t -i "/home/centos/SEBC_KEY.pem" centos@${host_no} "sudo cat /proc/cpuinfo | grep name | cut -f2 -d: | uniq -c"
       ssh -t -i "/home/centos/SEBC_KEY.pem" centos@${host_no} "sudo lscpu | grep Architecture"
       ssh -t -i "/home/centos/SEBC_KEY.pem" centos@${host_no} "sudo lscpu | grep \"Thread(s) per core\""
       echo "Storage:"
       ssh -t -i "/home/centos/SEBC_KEY.pem" centos@${host_no} "sudo df -lh"
       ssh -t -i "/home/centos/SEBC_KEY.pem" centos@${host_no} "sudo lsblk"  
       echo "Base system:"
       ssh -t -i "/home/centos/SEBC_KEY.pem" centos@${host_no} "sudo cat /etc/redhat-release | head -1"
       ssh -t -i "/home/centos/SEBC_KEY.pem" centos@${host_no} "echo "SELinux Status:"; sudo getenforce"
       ssh -t -i "/home/centos/SEBC_KEY.pem" centos@${host_no} "echo "Transparent Huge Page:"; sudo grep -i HugePages_Total /proc/meminfo"
       echo "Network:"
       ssh -t -i "/home/centos/SEBC_KEY.pem" centos@${host_no} "hostname -f"
       ssh -t -i "/home/centos/SEBC_KEY.pem" centos@${host_no} "hostname -i"
       ssh -t -i "/home/centos/SEBC_KEY.pem" centos@${host_no} "nslookup $host_no"
       ssh -t -i "/home/centos/SEBC_KEY.pem" centos@${host_no} "nslookup \`hostname -i\`"
       ssh -t -i "/home/centos/SEBC_KEY.pem" centos@${host_no} "ifconfig -a"
       echo "Java version:"
       ssh -t -q $ssh -t -i "/home/centos/SEBC_KEY.pem" centos@${host_no}  "java -version"
       ssh -t -q $ssh -t -i "/home/centos/SEBC_KEY.pem" centos@${host_no}  "echo $JAVA_HOME"
       echo "Python version:"
       ssh -t $ssh -t -i "/home/centos/SEBC_KEY.pem" centos@${host_no}  "python -V"  
    done


Example output:

    #Operation on vm5 ...
    Memory:
    MemTotal:   16330872 kB
    Connection to vm5 closed.
    CPU:
      4  Intel(R) Xeon(R) CPU E5-2676 v3 @ 2.40GHz
    Connection to vm5 closed.
    Architecture:  x86_64
    Connection to vm5 closed.
    Thread(s) per core:1
    Connection to vm5 closed.
    Storage:
    Filesystem  Size  Used Avail Use% Mounted on
    /dev/xvda1  7.8G  875M  6.5G  12% /
    tmpfs   7.8G 0  7.8G   0% /dev/shm
    Connection to vm5 closed.
    NAMEMAJ:MIN RM SIZE RO TYPE MOUNTPOINT
    xvda202:00  29G  0 disk
    └─xvda1 202:10   8G  0 part /
    Connection to vm5 closed.
    Base system:
    CentOS release 6.7 (Final)
    Connection to vm5 closed.
    SELinux Status:
    Permissive
    Connection to vm5 closed.
    Transparent Huge Page:
    HugePages_Total:   0
    Connection to vm5 closed.
    Network:
    ip-172-31-27-114.cn-north-1.compute.internal
    Connection to vm5 closed.
    172.31.27.114
    Connection to vm5 closed.
    Server: 172.31.0.2
    Address:172.31.0.2#53
    
    ** server can't find vm5: NXDOMAIN
    
    Connection to vm5 closed.
    Server: 172.31.0.2
    Address:172.31.0.2#53
    
    Non-authoritative answer:
    114.27.31.172.in-addr.arpa  name = ip-172-31-27-114.cn-north-1.compute.internal.
    
    Authoritative answers can be found from:
    
    Connection to vm5 closed.
    bash: ifconfig: command not found
    Connection to vm5 closed.
    Java version:
    bash: java: command not found
    
    Python version:
    Python 2.6.6
    Connection to vm5 closed.


##Base Service Install##
base_service_installation.sh:

    for host_no in {"vm1","vm2","vm3","vm4","vm5"}
    do
       echo "#Operation on ${host_no} ..."
       ssh -t -i "/home/centos/SEBC_KEY.pem" centos@${host_no} "sudo yum update -y"
       ssh -t -i "/home/centos/SEBC_KEY.pem" centos@${host_no} "sudo yum install -y wget"
       ssh -t -i "/home/centos/SEBC_KEY.pem" centos@${host_no} "sudo yum install -y vim"
       ssh -t -i "/home/centos/SEBC_KEY.pem" centos@${host_no} "sudo yum install -y bind-utils"
       ssh -t -i "/home/centos/SEBC_KEY.pem" centos@${host_no} "sudo yum install -y nscd"
       ssh -t -i "/home/centos/SEBC_KEY.pem" centos@${host_no} "sudo yum install -y ntp ntpdate"
    done
    
##System Modification##
operation.sh
    
    for host_no in {"vm1","vm2","vm3","vm4","vm5"}
    do
       echo "#Operation on ${host_no} ..."
       ssh -t -i "/home/centos/SEBC_KEY.pem" centos@${host_no} "sudo echo 'vm.swappiness=1' >> /etc/sysctl.conf; sysctl -p"
       ssh -t -i "/home/centos/SEBC_KEY.pem" centos@${host_no} "sudo chkconfig iptables off"
       ssh -t -i "/home/centos/SEBC_KEY.pem" centos@${host_no} "sudo setenforce 0"
       ssh -t -i "/home/centos/SEBC_KEY.pem" centos@${host_no} "sudo echo 'transparent_hugepage=never' >> /boot/grub/grub.conf"
       ssh -t -i "/home/centos/SEBC_KEY.pem" centos@${host_no} "sudo echo never > /sys/kernel/mm/redhat_transparent_hugepage/enabled"
       ssh -t -i "/home/centos/SEBC_KEY.pem" centos@${host_no} "sudo echo never > /sys/kernel/mm/redhat_transparent_hugepage/defrag"
       ssh -t -i "/home/centos/SEBC_KEY.pem" centos@${host_no} "sudo echo never > /sys/kernel/mm/transparent_hugepage/enabled"
       ssh -t -i "/home/centos/SEBC_KEY.pem" centos@${host_no} "sudo echo never > /sys/kernel/mm/transparent_hugepage/defrag"
       ssh -t -i "/home/centos/SEBC_KEY.pem" centos@${host_no} "sudo sysctl vm.nr_hugepages"
       ssh -t -i "/home/centos/SEBC_KEY.pem" centos@${host_no} "sudo getent hosts"
      
    done

##Additional Service##
###NSCD
    service nscd start
    service nscd status

###NTPD
    service ntpd start
    ntpdate pool.ntp.org
    service ntpd status
    chkconfig ntpd on









