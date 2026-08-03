把一台client切换为media server。实施过程如下


```comment

1.在master端，对与client有关的计划，job都停掉或删除掉

2.在client端，先停止有关进程，ps -ef|grep openv ，然后再删除/usr/openv，推荐mv先做备份

3.在client端安装NetBackup_7.6.0.1_LinuxR_x86_64，执行./install

4.注意指定其为media server 而非master server

5.重启master server服务，直到识别出新的media server为止。

```
