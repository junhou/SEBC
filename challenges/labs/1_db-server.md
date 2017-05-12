# MySQL setup #

----------
Version:

[root@ip-172-31-7-26 centos]# mysql --version
mysql  Ver 14.14 Distrib 5.6.36, for Linux (x86_64) using  EditLine wrapper


Create Cloudera service db and user:

    create database scm;
    create user 'scm'@'localhost' identified by 'clouderascm';
    create user 'scm'@'%' identified by 'cloudera-scm';
    grant all privileges on scm.* to 'scm'@'localhost';
    grant all privileges on scm.* to 'scm'@'%';
    flush privileges;
    
    create database hive;
    create user 'hive'@'localhost' identified by 'clouderahive';
    create user 'hive'@'%' identified by 'cloudera-hive';
    grant all privileges on hive.* to 'hive'@'localhost';
    grant all privileges on hive.* to 'hive'@'%';
    flush privileges;
    
    create database oozie;
    create user 'oozie'@'localhost' identified by 'clouderaoozie';
    create user 'oozie'@'%' identified by 'cloudera-oozie';
    grant all privileges on oozie.* to 'oozie'@'localhost';
    grant all privileges on oozie.* to 'oozie'@'%';
    flush privileges;
    
    create database hue;
    create user 'hue'@'localhost' identified by 'clouderahue';
    create user 'hue'@'%' identified by 'cloudera-hue';
    grant all privileges on hue.* to 'hue'@'localhost';
    grant all privileges on hue.* to 'hue'@'%';
    flush privileges;
    
    create database rman;
    create user 'rman'@'localhost' identified by 'clouderarman';
    create user 'rman'@'%' identified by 'cloudera-rman';
    grant all privileges on rman.* to 'rman'@'localhost';
    grant all privileges on rman.* to 'rman'@'%';
    flush privileges;
    
    create database sentry;
    create user 'sentry'@'localhost' identified by 'clouderasentry';
    create user 'sentry'@'%' identified by 'cloudera-sentry';
    grant all privileges on sentry.* to 'sentry'@'localhost';
    grant all privileges on sentry.* to 'sentry'@'%';
    flush privileges;
