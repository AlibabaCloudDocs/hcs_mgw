# HDFS

如果您的数据存储在HDFS中，需要您提供计算节点（配置不低于16核64G），通过将HDFS和闪电立方Mini设备都挂载至计算节点上进行数据迁移。本文介绍挂载HDFS和闪电立方Mini设备至计算节点的操作步骤。

## 前提条件

-   已准备计算节点。
-   在挂载闪电立方Mini设备至计算节点前，请先确认闪电立方Mini设备的共享目录已完成共享。
    1.  登录硬件控制台，详情请参见[安装硬件](/cn.zh-CN/离线迁移教程（闪电立方Mini型）/迁移实施/安装硬件.md)。

    2.  选择**控制台**\>**权限**\>**共享文件夹**。

    3.  在**共享文件夹**页签下，确认是否已存在filedata目录。

        一般情况下，默认存在filedata目录。如果不存在，则参考创建共享目录。


## （可选）创建共享目录

创建共享目录前，先登录闪电立方服务器确认是否存在共享目录，确认目录存在后，通过浏览器登录硬件控制台添加共享目录。

## 登录闪电立方服务器确认是否存在共享目录

-   通过Linux系统或Mac系统登录闪电立方服务器

    1.  在您的笔记本电脑中打开命令行窗口。

    2.  执行`ssh 用户名@管理IP地址`命令。

        管理IP地址默认为`192.168.1.1`，详情请参见[安装硬件](/cn.zh-CN/离线迁移教程（闪电立方Mini型）/迁移实施/安装硬件.md)。

    3.  输入密码，登录闪电立方服务器。

    4.  执行`cd /share/CACHEDEV1_DATA`命令，进入目录后执行`ls`命令，查看是否存在名为`filedata`的目录，如果存在则跳过下面步骤，直接浏览器访问硬件管理控制台。
    5.  如不存在则执行`mkdir filedata`，创建共享目录文件夹。
-   通过Windows系统登录闪电立方服务器

    1.  在您的笔记本电脑中下载远程连接工具。

        此处以PuTTY为例，下载地址为[Download PuTTY](https://www.chiark.greenend.org.uk/~sgtatham/putty/latest.html)，请选择符合您电脑型号的版本进行安装。

    2.  打开PuTTY工具，配置如下信息，登录闪电立方服务器。

        Host Name（or IP Address）配置为管理IP地址，默认为`192.168.1.1`，详情请参见[安装硬件](/cn.zh-CN/离线迁移教程（闪电立方Mini型）/迁移实施/安装硬件.md)。

        ![](http://docs-aliyun.cn-hangzhou.oss.aliyun-inc.com/assets/pic/145498/cn_zh/1575272146600/%E6%8E%A5%E5%85%A5%E6%95%B0%E6%8D%AE%E6%BA%90-001.png)

    3.  执行`cd /share/CACHEDEV1_DATA`命令，进入目录后执行`ls`命令，查看是否存在名为`filedata`的目录，如果存在则跳过下面步骤，直接浏览器访问硬件管理控制台。
    4.  如不存在则执行`mkdir filedata`，创建共享目录文件夹。 以上步骤完成后，浏览器登录闪电立方硬件控制台，创建共享文件夹。

## 硬件管理控制台创建共享目录

1.  登录硬件控制台，详情请参见[安装硬件。](/cn.zh-CN/离线迁移教程（闪电立方Mini型）/迁移实施/安装硬件.md)

2.  选择**控制台**\>**权限**\>**共享文件夹**，请单击**创建**\>**共享文件夹**。创建共享文件夹时，请完成如下配置。

    共享文件夹名称：输入filedata。

    -   路径：选择**手动输入路径**，并选择**filedata**。
3.  单击filedata目录右侧的编辑图标。

    ![](http://docs-aliyun.cn-hangzhou.oss.aliyun-inc.com/assets/pic/145506/cn_zh/1575278791921/%E6%8E%A5%E5%85%A5%E6%95%B0%E6%8D%AE%E6%BA%90-%E6%8C%82%E8%BD%BD-005.png)

4.  在**编辑共享文件夹权限**对话框中，选择**NFS主机访问**，确认filedata目录的访问权限为**读写**。

    ![](http://docs-aliyun.cn-hangzhou.oss.aliyun-inc.com/assets/pic/145506/cn_zh/1575279606632/%E6%8E%A5%E5%85%A5%E6%95%B0%E6%8D%AE%E6%BA%90-%E6%8C%82%E8%BD%BD-006.png)

5.  选择**控制台**\>**网络&文件服务**\>**Win/Mac/NFS**，单击**Linux NFS服务**。

6.  勾选**激活NFS V4服务**，单击**应用**，完成激活。

    ![](http://docs-aliyun.cn-hangzhou.oss.aliyun-inc.com/assets/pic/145506/cn_zh/1575280820286/%E6%8E%A5%E5%85%A5%E6%95%B0%E6%8D%AE%E6%BA%90-%E6%8C%82%E8%BD%BD-003.png)


## 步骤一：配置业务IP地址

1.  登录硬件控制台，详情请参见[安装硬件](/cn.zh-CN/离线迁移教程（闪电立方Mini型）/迁移实施/安装硬件.md)。

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

挂载HDFS和闪电立方Mini设备至计算节点后，需要通过闪电立方II、III型的迁移方式完成迁移任务，详情请参见离线迁移教程（闪电立方II、III型）中的[创建及执行数据迁移任务](/cn.zh-CN/离线迁移教程（闪电立方II、III型）/迁移实施/创建及执行数据迁移任务.md)。

