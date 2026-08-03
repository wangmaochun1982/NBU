```bash

[root@nbu ~]# mount Rocky-9.8-x86_64-dvd.iso /media/

[root@nbu ~]# localectl set-locale LANG=en_US.UTF-8


[root@nbu ~]# chrony


[root@nbu ~]# chronyc sources
[root@nbu ~]# sed -i "s|SELINUX=enforcing|SELINUX=disabled|g" /etc/selinux/config
cat /etc/selinux/config
[root@nbu ~]# setenforce Permissive


[root@nbu ~]# systemctl stop firewalld


[root@nbu ~]#  yum install libnsl
[root@nbu ~]# cd /etc/yum.repos.d/
[root@nbu yum.repos.d]# mkdir bak
[root@nbu yum.repos.d]# mv *.repo ./bak/
[root@nbu yum.repos.d]# vi local.repo

[root@nbu software]# cat /etc/yum.repos.d/local.repo 
[BaseOS]
name=BaseOS
baseurl=file:///media/BaseOS
gpgcheck=0
enable=1

[AppStream]
name=AppStream
baseurl=file:///media/AppStream
gpgcheck=0
enable=1

[root@nbu yum.repos.d]# yum install libnsl

[root@nbu yum.repos.d]# yum install nginx

[root@nbu yum.repos.d]# systemctl enable nginx --now

[root@nbu yum.repos.d]# yum install libtirpc

[root@nbu yum.repos.d]# groupadd nbwebgrp
[root@nbu yum.repos.d]# cd ~
[root@nbu ~]# useradd -c 'NetBackup Services Account' nbsvcusr
[root@nbu ~]# usermod -a -G nbwebgrp nbsvcusr

[root@nbu ~]# echo "*       soft    nofile  8192" >> /etc/security/limits.conf
echo "*       hard    nofile  8192" >> /etc/security/limits.conf

[root@nbu ~]# echo "kernel.sem = 300 307200 100 1024" >> /etc/sysctl.conf

[root@nbu ~]# sysctl -p
kernel.sem = 300 307200 100 1024

[root@nbu ~]# vi .bash_profile
[root@nbu software]# cat /root/.bash_profile 
# .bash_profile

# Get the aliases and functions
if [ -f ~/.bashrc ]; then
	. ~/.bashrc
fi

# User specific environment and startup programs

export PATH=$PATH:/usr/local/sbin:/usr/local/bin:/usr/sbin:/usr/bin:/usr/local/bin:/usr/openv/netbackup/bin/goodies:/usr/openv/netbackup/bin/admincmd:/usr/openv/volmgr/bin:/usr/openv/netbackup/bin:/usr/openv/db/bin:/usr/openv/netbackup/bin/support:/usr/openv/netbackup/bin/goodies/support:/opt/VRTSpbx/bin/



[root@nbu ~]# source .bash_profile
[root@nbu ~]# cd /ap/software/
[root@nbu software]# tar -zxvf NetBackup_11.2_LinuxR_x86_64.tar.gz

[root@nbu software]# cd NetBackup_11.2_LinuxR_x86_64/
[root@nbu NetBackup_11.2_LinuxR_x86_64]# ./install

Do you wish to continue? [y,n] (y)
Starting NetBackup Deduplication installer
checking dir: /tmp/pdde_pkg_dir_370667
NetBackup Deduplication preinstall check passed
Running install analysis tool.

Is this host the primary server? [y,n] (y)
Are you currently performing a disaster recovery of a primary server? [y,n] (n)
您当前是否正在对主服务器执行灾难恢复？[y，n]（n）

Enter the name of the service user account to be used to start most of the daemons: nbsvcusr


Do you want to install IT Analytics Data Collector? [y,n] (y) n


Do you want to install NetBackup and Media Manager files? [y,n] (y)

Choose an option from the list below.
    1) Include the BAR GUI.
    2) Exclude the BAR GUI.

BAR GUI option [1,2] (2) :
Excluding the installation of BAR GUI package.


Choose an option from the list below.
    1) Include the NetBackup DirectIO package.
    2) Exclude the NetBackup DirectIO package.


NetBackup DirectIO option: [1,2] (2) :
Excluding the installation of NetBackup DirectIO package.


Are the license files downloaded from the licensing portal? (y/n): n

Do you want to use a NetBackup evaluation license (y/n): y


Would you like to use "nbu" as the configured
NetBackup server name of this machine? [y,n] (y) n

Enter the name of this NetBackup server: nbu.keentech-xm.com


Do you want to add any media servers now? [y,n] (n)




[root@nbu msdp]# lsof +L1 | grep /msdp
[root@nbu msdp]# mkfs.xfs -f /dev/mapper/vg_data-lv_data
meta-data=/dev/mapper/vg_data-lv_data isize=512    agcount=88, agsize=268435392 blks
         =                       sectsz=4096  attr=2, projid32bit=1
         =                       crc=1        finobt=1, sparse=1, rmapbt=0
         =                       reflink=1    bigtime=1 inobtcount=1 nrext64=0
data     =                       bsize=4096   blocks=23438769152, imaxpct=1
         =                       sunit=64     swidth=256 blks
naming   =version 2              bsize=4096   ascii-ci=0, ftype=1
log      =internal log           bsize=4096   blocks=521728, version=2
         =                       sectsz=4096  sunit=1 blks, lazy-count=1
realtime =none                   extsz=4096   blocks=0, rtextents=0
[root@nbu msdp]# mount /msdp
[root@nbu msdp]# df -h



[root@nbu msdp]# cd /ap/software/
[root@nbu software]# chmod 744 eebinstaller_4229816_1_linuxR_x86
[root@nbu software]# ./eebinstaller_4229816_1_linuxR_x86

Do you acknowledge this disclaimer?   [y,n] (y)
This installer will use the following as the base installation directory:
  /usr/openv

Is this the correct path?   [y,n] (y)

Shutdown all NetBackup services and then run this installer. Restart all services after the installation succeeds.

Continue?  [y,n] (y)

The installer has detected services are running when none should be.
Do you want services shut down now?  [y,n] (y)

Installation complete.

Do you want services re-started now?  [y,n] (y)




Installation complete.

Do you want services re-started now?  [y,n] (y)


[root@nbu software]# mkdir /usr/openv/netbackup/db/altnames
[root@nbu software]# touch /usr/openv/netbackup/db/altnames/No.Restrictions
[root@nbu software]# cd /usr/openv/netbackup/
[root@nbu netbackup]# vi bp.conf
[root@nbu netbackup]# hostnamectl set-hostname help
[root@nbu netbackup]# hostnamectl help set-hostname


[root@nbu software]# cd /usr/openv/netbackup/
[root@nbu netbackup]# cat bp.conf 
SERVER = nbu.keentech-xm.com
CLIENT_NAME = nbu.keentech-xm.com
CONNECT_OPTIONS = localhost 1 0 2
NB_FIPS_MODE = DISABLE
EMMSERVER = nbu.keentech-xm.com
VXDBMS_NB_PGCONF = /usr/openv/db/data/instance
VXDBMS_NB_DATA = /usr/openv/db/data
CHECK_RANSOMWARE_EXTENSIONS = ALWAYS
MAX_VAULT_JOBS = 50
CLIENT_CONNECT_TIMEOUT = 1800
CLIENT_READ_TIMEOUT = 1800
SERVICE_USER = nbsvcusr
DATABASE_USER = nbsvcusr
WEBSVC_GROUP = nbwebgrp
WEBSVC_USER = nbwebsvc
TELEMETRY_UPLOAD = NO
KEEP_JOBS_SUCCESSFUL_HOURS = 168
KEEP_JOBS_HOURS = 168



[root@nbu netbackup]# hostnamectl set-hostname nbu.keentech-xm.com
[root@nbu netbackup]# hostnamectl


[root@nbu netbackup]# hostname


```
