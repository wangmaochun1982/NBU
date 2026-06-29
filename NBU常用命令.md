# [NBU基本常用命令 ](https://www.cnblogs.com/yihr/p/7865718.html)



**阅读目录**

- [一、Netbackup管理维护](https://www.cnblogs.com/yihr/p/7865718.html#_label0)
- [二、巡检常用命令](https://www.cnblogs.com/yihr/p/7865718.html#_label1)
- [三、备份作业、策略](https://www.cnblogs.com/yihr/p/7865718.html#_label2)

[回到顶部](https://www.cnblogs.com/yihr/p/7865718.html#_labelTop)

# 一、Netbackup管理维护

1 显示NetBackup的进程

```bash
/usr/opnev/netbackup/bin/bpps -x
```



2 启动NetBackup进程

```bash
/usr/openv/netbackup/bin/goodies/netbackup start
/usr/openv/netbackup/bin/goodies/bp.start_all
```



3 停止NetBackup进程

```bash
/usr/openv/netbackup/bin/goodies/netbackup stop
/usr/openv/netbackup/bin/goodies/bp.kill_all
```



4 启动PBX进程

```bash
/opt/VRTSpbx/bin/pbx_exchange
```



 

[回到顶部](https://www.cnblogs.com/yihr/p/7865718.html#_labelTop)

# 二、巡检常用命令

1 文件系统使用率

```bash
df -h 或 df -k
```



 

2 空白磁带 

```bash
available_media >/tmp/list.txt

vmquery -pn Scratch -bx|grep -v "NONE" |more

cat /tmp/list.txt |grep -i tld |grep -i ava|wc -l
```



 

3 最近失败的200条备份作业

```bash
bpdbjobs |grep -v " 0 " |head -200
```



 

4 带库驱动器检查

```bash
robtest

vmoprcmd -d

tpconfig -l
```



 

 

[回到顶部](https://www.cnblogs.com/yihr/p/7865718.html#_labelTop)

# 三、备份作业、策略

1.列出所有作业摘要

```bash
/usr/openv/netbackup/bin/admincmd/bpdbjobs -summary
```



查看某个作业的详细情况

```bash
bpdbjobs -jobid  -all_columns
```



 

2.查询目前有哪些磁带正在被哪个驱动器调用，以及正在运行哪些备份任务

```bash
/usr/openv/netbackup/bin/admincmd/nbrbutil -dump
```



 

3.调整作业

```bash
bpdbjob -cancle -jobid 手工取消作业

bpbackup -i -p policy-name  手工启动策略进行备份
```



 

4.查看策略详情

```bash
bppllist 策略名 -U
```



 

5.把一条策略状态激活或禁用

```bash
bpplinfo 策略名 -modify -active(激活) / -inactive(去激活)
```



 

6.查看策略最近24小时备份量

```bash
bpimagelist -U -hoursago 24 -policy 策略名 |awk '{sum+=$5} END {print sum/1024/1024 }'
```





```bash
#日志保留的吧
NetBackup/bin/admincmd/bpdbjobs -clean -keep_days 15
NetBackup/bin/admincmd/bpdbjobs -clean -keep_successful_days 15

#恢复catalog
NetBackup/bin/admincmd/bprecover -wizard

```





```bash
查看备份：
#cd /usr/openv/netbackup/bin
#./bplist -l -C fgedu81  -S fgedu99 -t 4 -R /
```



```bash
$rman target / catalog rman/rman@rman

RMAN>list backup;

#恢复control file
RMAN>
run{
ALLOCATA CHANNEL ch00 TYPE 'SBT_TAPE' PARMS 'SBT_LIBRARY=/usr/openv/netbackup/bin/libobk.so64';
send'NB_ORA_SERV=fgedu99,NB_ORA_CLIENT=fgedu';
restore controlfile from '';
release channel ch00;
}


RMAN> alter database mount;


恢复数据文件
RMAN>
run{
ALLOCATA CHANNEL d1 TYPE 'SBT_TAPE' PARMS 'SBT_LIBRARY=/usr/openv/netbackup/bin/libobk.so64';
ALLOCATA CHANNEL d2 TYPE 'SBT_TAPE' PARMS 'SBT_LIBRARY=/usr/openv/netbackup/bin/libobk.so64';
ALLOCATA CHANNEL d3 TYPE 'SBT_TAPE' PARMS 'SBT_LIBRARY=/usr/openv/netbackup/bin/libobk.so64';
ALLOCATA CHANNEL d4 TYPE 'SBT_TAPE' PARMS 'SBT_LIBRARY=/usr/openv/netbackup/bin/libobk.so64';
send 'NB_ORA_SERV=fgedu99,NB_ORA_CLIENT=fgedu';
restore database;
release channel d1;
release channel d2;
release channel d3;
release channel d4;
}





run{
ALLOCATA CHANNEL ch00 TYPE 'SBT_TAPE' PARMS 'SBT_LIBRARY=/usr/openv/netbackup/bin/libobk.so64';
ALLOCATA CHANNEL ch01 TYPE 'SBT_TAPE' PARMS 'SBT_LIBRARY=/usr/openv/netbackup/bin/libobk.so64';
send 'NB_ORA_SERV=fgedu99,NB_ORA_CLIENT=fgedu';
recover database until scn 976718;

release channel ch00;
release channel ch01;
}

```

```sql
$sqlplus "/as sysdba"

SQL>alter database open resetlogs;
```







````bash
#异机
1>安装与生产环境一样的OS环境，包括os操作系统，数据库版本，补丁，用户组，提前准备数据文件目录与归档路径等
2>安装NBU客户端，与备份服务器连接正常
hosts:
192.168.1.99 fgeduu99
192.168.1.81  fgedu81
192.168.1.80  fgedu80
3>客户端认证：
在Master Server上添加No.Restrictions文件
Linux: /usrs/openv/netbackup/db/altnames/No.Restrictions
Windows: install_path\Netbackup\db\altnames\No.Restrictions
重启NBU

模拟环境：
把fgedu81关机，复制一个虚拟机，改IP，改主机名(192.168.1.80 fgedu80)

如是克隆的要先卸掉NBU
# /usr/openv/netbackup/bin/goodies/netbackup stop


# rm -rf /usr/openv

#cd /mnt/hgfs/soft
安装客户端会做CA认证
到CA的地方，按Y的时候，服务器上就会有token

#cd /usrs/openv/netbackup
#scp fgedu81:/usr/openv/netbackup/bp.conf ./

vi bp.conf

SERVER = fgedu99
SERVER = fgedu81
CLIENT_NAME = fgedu80
EMMSERVER = fgedu99
FORCE_RESTORE_MEDIA_SERVER = fgedu81  fgedu99#优先从哪个介质恢复，因为有多个介质

#接状启动
# /usr/openv/netbackup/bin/goodies/netbackup stop
# /usr/openv/netbackup/bin/goodies/netbackup start
````

```bash
#可以先连到catalog 
$sqlplus rman/rman@rman
SQL> select * from rc_database;
可以查dbid

$ rman target / catalog rman/rman@rman

RMAN> set dbid=
RMAN> list backup

也可以从异机的nbu上去查
#./bplist -l -C fgedu81  -S fgedu99 -t 4 -R /
-c是客户端，原来的备份的客户端
-s是服务器端
-t  4是ORACLE

#先恢复控制文件
#恢复control file ，这边send的客户端写的是以前备份的那台客户端，才能查得到
RMAN>
run{
ALLOCATA CHANNEL ch00 TYPE 'SBT_TAPE' PARMS 'SBT_LIBRARY=/usr/openv/netbackup/bin/libobk.so64';
send'NB_ORA_SERV=fgedu99,NB_ORA_CLIENT=fgedu81';
restore controlfile from '';
release channel ch00;
}



RMAN> alter database mount;


恢复数据文件
RMAN>
run{
ALLOCATA CHANNEL d1 TYPE 'SBT_TAPE' PARMS 'SBT_LIBRARY=/usr/openv/netbackup/bin/libobk.so64';
ALLOCATA CHANNEL d2 TYPE 'SBT_TAPE' PARMS 'SBT_LIBRARY=/usr/openv/netbackup/bin/libobk.so64';
ALLOCATA CHANNEL d3 TYPE 'SBT_TAPE' PARMS 'SBT_LIBRARY=/usr/openv/netbackup/bin/libobk.so64';
ALLOCATA CHANNEL d4 TYPE 'SBT_TAPE' PARMS 'SBT_LIBRARY=/usr/openv/netbackup/bin/libobk.so64';
send 'NB_ORA_SERV=fgedu99,NB_ORA_CLIENT=fgedu';
restore database;
recover database;
release channel d1;
release channel d2;
release channel d3;
release channel d4;
}
如果直接写recover database，那备份的归档里可能比如只有到12，而用这句会提示到13

报：RMAN-00571
RMAN-06054:media recovery requesting unknow archived log for thread 1 with sequence 13

RMAN > list backup;
最后只有12，只备到12






$sqlplus "/as sysdba"
SQL>recover database using backup controlfile until cancel;

告诉控制文件我已经恢复OK了，不需要再恢复了
SQL>alter database open resetlogs;

SQL>SELECT STATUS FROM v$instance;
```

