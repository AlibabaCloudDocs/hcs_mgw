# HDFS

如果您的数据存储在HDFS中，需要您提供计算节点（配置不低于16核64G），通过将HDFS和闪电立方Mini设备都挂载至计算节点上进行数据迁移。本文介绍挂载HDFS和闪电立方Mini设备至计算节点的操作步骤。

## 前提条件

-   已准备计算节点。
-   在挂载闪电立方Mini设备至计算节点前，请先确认计算节点，已经通过网线直连方式或交换机方式，连接到闪电立方Mini设备的网口或光口上，并确认网线、光纤线和光模块连接正常，端口连接指示灯正常。

## 步骤一：配置业务IP地址

1.  登录硬件控制台，详情请参见[安装硬件](/intl.zh-CN/数据迁移教程/迁移实施/安装硬件.md)。

2.  选择**控制台\>网络与虚拟交换机\>网络适配器**。

3.  选择状态为**已连接**的适配卡，单击编辑图标。

    ![](http://docs-aliyun.cn-hangzhou.oss.aliyun-inc.com/assets/pic/145500/cn_zh/1575273855276/%E6%8E%A5%E5%85%A5%E6%95%B0%E6%8D%AE%E6%BA%90-%E7%BD%91%E5%8F%A3-001.png)

4.  在IPv4中，选择**使用固定IP地址**，并根据您的实际网络环境配置**固定IP地址**、**子网掩码**、**默认网关**，单击**应用**。

    **说明：** 请记录此IP地址，在迁移任务控制台中创建数据地址时，需配置此IP地址。

    ![](http://docs-aliyun.cn-hangzhou.oss.aliyun-inc.com/assets/pic/119543/cn_zh/1558424094029/Image%2050.png)


## 步骤二：挂载HDFS至计算节点

将HDFS挂载至计算节点，详情请参见[Configuring Mountable HDFS](https://docs.cloudera.com/documentation/enterprise/5-7-x/topics/cdh_ig_hdfs_mountable.html)。

## 步骤三：挂载闪电立方至计算节点

1.  登录计算节点（Linux系统）。

2.  执行以下命令创建本地目录，作为闪电立方的挂载目录。

    `mkdir /mnt/cube`

3.  执行以下命令查看闪电立方的共享目录。

    `showmount -e 172.168.1.1`

    `172.168.1.1`为业务IP地址，请根据实际情况替换。

    如果回显信息中存在filedata目录，则表明闪电立方共享目录可以被挂载。

4.  执行以下命令挂载闪电立方Mini设备至计算节点。

    `mount 172.168.1.1:/filedata /mnt/cube`

5.  执行以下命令查看挂载结果。

    `df -h`

    如果回显包含如下类似信息，说明挂载成功。

    ![](http://docs-aliyun.cn-hangzhou.oss.aliyun-inc.com/assets/pic/145506/cn_zh/1575342409513/%E6%8E%A5%E5%85%A5%E6%95%B0%E6%8D%AE%E6%BA%90-%E6%8C%82%E8%BD%BD-007.png)


## 后续步骤

挂载HDFS和闪电立方Mini设备至计算节点后，需要通过闪电立方II、III型的迁移方式完成迁移任务，详情请参见离线迁移教程（闪电立方II、III型）中的[创建及执行数据迁移任务]()。

