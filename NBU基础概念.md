NBU组件：

Master Server-从EMM服务器获取可用资源，调度和管理Media Server执行任务。

EMM Server-可用资源管理

Media Server-数据管理，负责读写数据。

Clients-客户端代理

OpsCenter-监视，管理，报告，支持多个NBU域。


NBU的架构

NBU Server，只能有一台备份服务器。

NBU Enterprise Server，可以有多台备份服务器。

Multiple Domain，每个域可以有单独的EMM,Master和Media，OpsCenter可以管理多个域。


NBU映像(Image)

在一个会话中，来自一个客户端的一个数据流，会形成一个image，备份数据以GNU tar的方式来压缩，通过备份ID识别。


SU=Storage Unit

逻辑备份设备，其定义包括物理设备和使用些设备的服务器。


卷Volume

即一个备份介质，如磁带或光盘。



NBU缺省有四个卷池：

1，NetBackup 缺省池

2，DataStore 用于DataStore卷的缺省卷池

3，CatalogBackup 用于在线的Catalog备份。

4，None 未指定卷池的卷，包括清洗带。



NBU Catalog:

1，映像数据库 备份数据的信息。

2，NBU配置文件 主要是策略和计划信息。

3，关系数据库 包括介质和设备配置信息（EMM），BMR配置信息等。



NBU策略

1，属性 How

2，计划 When

3，客户端 Where

4，选择 What



备份类型：

1，全备

2，增备（Differential Incremental）差分增量

3，差备（Cumulative Incremental）累积增量

4，用户备份

5，用户归档v



NetBackup Vault的三项功能：

1，创建介质副本

2，自动弹带

3，自动管理介质循环。


Netbackup快照客户端代理的功能：

1，FlashBackup，用于大量小文件的快速备份

2，Instant Reovery，基于本地快照的备份技术，用于快速还原。

3，与阵列集成，调用硬件的快照功能

4，ServerFree，脱机备份

5，Oracle块级增量
