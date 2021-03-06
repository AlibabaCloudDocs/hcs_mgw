# 阿里云对象存储 OSS 之间数据迁移 {#concept_igj_s12_qfb .concept}

本文介绍如何在阿里云对象存储 OSS 之间进行数据迁移。

## 前提条件 {#section_wvw_v12_qfb .section}

-   已确定待迁移存储量和迁移文件个数。

    登录 [OSS 控制台](https://oss.console.aliyun.com)后单击某个待迁移存储空间的名称，查看待迁移存储空间（Bucket）的存储量和对象（文件）数量。

-   已创建用于存储迁移后数据及文件的 Bucket。

    如果您还未创建存储空间，请参见[创建存储空间](../../../../cn.zh-CN/快速入门/创建存储空间.md#)文档创建用于存储迁移后的数据及文件的 Bucket。

-   已创建用于迁移的 AccessKey。详情请参见[创建 AccessKey](../../../../cn.zh-CN/通用参考/创建AccessKey.md#)。
-   该 AccessKey 拥有该存储空间的读写权限，即 AliyunOSSFullAccess 权限。详情请参见[为 RAM 用户授权](../../../../cn.zh-CN/快速入门/为 RAM 用户授权.md#)。

## 步骤一：创建源地址 {#procedure_one .section}

1.  登录[阿里云数据在线迁移控制台](https://mgw.console.aliyun.com/)。
2.  选择**在线迁移服务** \> **数据地址**，然后单击**创建数据地址**。
3.  在创建数据地址页面，配置相关参数，然后单击**下一步**。参数说明如下：

    |参数|是否必需|说明|
    |:-|:---|:-|
    |数据类型|是|选择 **OSS**。|
    |当前区域|是|选择源地址所在的地域，比如**华北 3（张家口）**。|
    |数据名称|是|输入 3-63 位字符。不支持短横线（-）和下划线（\_）之外的特殊字符。|
    |OSS Endpoint|是|选择一个 Endpoint，如`oss-cn-zhangjiakou-internal.aliyuncs.com`。|
    |AccessKey Id 和 AccessKey Secret|是|输入用于迁移的 AccessKey。|
    |OSS Bucket|是|选择一个存储空间，用于存储待迁移数据。|
    |OSS Prefix|是|格式要求不能以正斜线（/） 开头，要以正斜线（/） 结尾，如`data/to/oss/`。|

4.  系统提示该功能在公测中，需要提交白名单权限申请。单击**去申请**。
5.  填写相关信息，提交迁移公测申请。申请通过后，您将收到短信提醒。

## 步骤二：创建目的地址 { .section}

创建目的地址的步骤与创建源地址相同，相关参数配置请参考[步骤一](#)中的参数说明。

## 步骤三：创建迁移任务 {#section_ksy_xmy_pfb .section}

1.  选择**在线迁移服务** \> **迁移任务**，然后单击**创建迁移任务**。
2.  在创建迁移任务页面，阅读迁移服务条款协议，勾选**我理解如上条款，并申请开通数据迁移服务**，然后单击**下一步**。
3.  在配置任务页签，设置相关参数，然后单击**下一步**。

    参数说明如下：

    |参数|是否必需|说明|
    |:-|:---|:-|
    |任务名称|是|输入 3-63 位小写字母、数字、短横线（-），且不能以短横线（-）开头或结尾。|
    |源地址|是|选择已创建的源地址。|
    |目的地址|是|选择已创建的目的地址。|
    |迁移方式|是|     -   单次迁移：数据迁移完成后任务将立即停止，不再对增量数据进行迁移。
    -   增量迁移：首次数据迁移完成后，按指定迁移间隔和迁移次数对增量数据进行迁移。例如，设置迁移间隔 1 小时，迁移次数 10 次，则每次增量数据迁移完成后 1 小时再次启动迁移任务进行新增数据的迁移，如此循环 10 次。
 |
    |迁移间隔|是（针对增量迁移）|默认值 1 小时，最大值 24 小时。|
    |迁移次数|是（针对增量迁移）|默认值 1 次，最大值 30 次。|

4.  在性能调优页签，填写**迁移存储量**和**迁移文件个数**。

    **说明：** 为了迁移任务的顺利进行，请尽量准确进行数据预估。

5.  （可选）在性能调优页签，设置**限流时间段**和**最大流量**，然后单击**添加**。
6.  单击**创建**。

## 步骤四：查看迁移任务状态 {#section_njn_5ty_pfb .section}

等待迁移完成后，查看任务状态。迁移任务有三种状态：

-   创建失败：迁移任务创建失败。您可以查看失败原因，并重试。
-   已完成：迁移任务完成。您可以继续以下步骤，查看迁移报告。
-   失败：迁移任务失败。您可以继续以下步骤，查看迁移报告并重新迁移失败的文件。

## （可选）步骤五：查看迁移报告 {#section_jxv_xty_pfb .section}

1.  在迁移任务记录中，单击**管理**。
2.  单击**生成迁移报表**。待报告生成后，单击**导出**，导出迁移报告。

    迁移报告中，**文件列表**一栏包含三个文件名：

    -   以`_total_list`结尾的文件名代表总迁移文件列表。
    -   以`_completed_list`结尾的的文件名代表已迁移完成文件列表。
    -   以`_error_list`结尾的文件代表迁移失败文件列表。
3.  在[OSS控制台](https://oss.console.aliyun.com)，找到自动生成的文件夹aliyun\_mgw\_import\_report/，其中包含迁移报告中列出的三个文件。您可以下载这些文件，查看详细的文件列表。

    文件格式如下：

    -   总迁移文件列表：原地址 + 文件名 + 文件大小 \(Byte\) + 最后修改时间。原地址的格式为：`<厂商>://<bucketName>/<prefix>/<objectName>`，例如 `oss://bucket-test1022/myprefix/testfile.txt`
    -   已迁移完成文件列表：文件名 + 文件大小 \(Byte\) + 校验值 \(CRC64\) + 迁移完成时间
    -   迁移失败文件列表：文件名 + 迁移开始时间 + 迁移结束时间 + 错误描述

## （可选）步骤六：重试 {#section_zyb_v5y_pfb .section}

如果迁移任务失败，在迁移任务记录中，单击**管理**，然后单击**重试**，重新迁移失败的文件。迁移过程中，您可以随时停止和启动迁移任务。

