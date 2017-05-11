Kerberos Host level deployment:

Install KDC server,client


yum install krb5-workstation krb5-libs krb5-server


Edit Krb5.conf, and delieve to all host:

    
    [libdefaults]
    + default_realm = CN-NORTH-1.COMPUTE.AMAZONAWS.COM.CN
    
    [realms]
    + CN-NORTH-1.COMPUTE.AMAZONAWS.COM.CN = {
    +	kdc = ec2-52-80-43-137.cn-north-1.compute.amazonaws.com.cn
    +	admin_server = ec2-52-80-43-137.cn-north-1.compute.amazonaws.com.cn
    + }
    


Edit kadm5.acl:

    +	*/admin@CN-NORTH-1.COMPUTE.AMAZONAWS.COM.CN *
    +	cloudera-scm@CN-NORTH-1.COMPUTE.AMAZONAWS.COM.CN admilc

Create metadata entry for realm in kdb

    kdb5_util create -r CN-NORTH-1.COMPUTE.AMAZONAWS.COM.CN

Start service:

    service krb5kdc start
    service kadmin start


Add a user headless principal:

    kinit admin/admin
    kadmin
    addprinc -p password root
    quit
    ---
    kdestroy
    kinit root

