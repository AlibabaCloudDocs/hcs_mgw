# 某医药企业线下NAS数据迁移至阿里云NAS的案例 {#concept_187556 .concept}

本文主要介绍线下NAS服务器内的数据迁移至阿里云文件存储NAS长期保存的案例。

## 背景信息 {#section_vit_81o_gi3 .section}

杭州某医药企业内部NAS服务器中存放有公司产品资料、实验数据等，数据约10TB大小，1000万个文件。NAS 服务器在公司机房内，使用NFS系统，有安装防火墙，无外网连接，内网访问IP地址为10.0.0.254。

现基于数据安全及成本考虑，希望将数据存储至阿里云NAS。

## 迁移方案 {#section_da8_t0e_6ce .section}

根据用户需求及背景信息，制定如下迁移方案：

1.  在杭州地域创建用于存储数据的阿里云NAS，并挂载至阿里云VPC网络下。
2.  安装一条专线，将NAS服务器与您杭州地域的阿里云VPC网络连通，并修改NAS服务器的防火墙设置，允许VPC网络中的所有地址访问NAS服务器。
3.  通过在线迁移将线下NAS服务器内的数据迁移至阿里云NAS。

## 步骤一：创建阿里云NAS {#section_0sj_4th_w8w .section}

1.  在杭州地域创建一个NFS协议类型的阿里云NAS，详情请参见[创建文件系统](../../../../cn.zh-CN/快速配置指南/创建文件系统.md#)。
2.  将阿里云NAS挂载到VPC网络下。详情请参见[添加挂载点](../../../../cn.zh-CN/快速配置指南/添加挂载点.md#)。
3.  修改安全组，允许VPC内所有地址可以读写此NAS。详情请参见[管理文件系统数据访问权限](../../../../cn.zh-CN/使用指南/管理文件系统数据访问权限.md#)。

## 步骤二：将NAS服务器挂载至阿里云VPC网络下 {#section_398_pm5_ads .section}

1.  根据需求，安装一条传输速度为1Gbps/s的专线，将NAS服务器连接到阿里云NAS挂载的杭州地域的VPC网络中。详情请参见[物理专线接入](../../../../cn.zh-CN/快速入门/物理专线接入.md#)。
2.  修改NAS服务器的防火墙设置，允许VPC网络所有地址访问NAS服务器。

## 步骤三：创建迁移任务 {#section_3eo_026_sj7 .section}

1.  在阿里云上创建RAM子账号，授予子账号创建迁移任务的相关权限。配置步骤请参见[创建 RAM 子账号并授予相关权限](../../../../cn.zh-CN/NAS 迁移至 OSS 教程/准备工作.md#ul_z1k_23n_qfb)。
2.  使用刚刚创建的子账号登录[数据迁移服务控制台](https://mgw.console.aliyun.com/#/source?_k=0k9yvg)，使用NAS服务器信息创建源数据地址。参数介绍请参见[创建源数据地址](../../../../cn.zh-CN/NAS 之间迁移教程/迁移实施.md#one)，配置详情如下图。

    ![](http://static-aliyun-doc.oss-cn-hangzhou.aliyuncs.com/assets/img/161261/155650822445626_zh-CN.png)

3.  使用阿里云NAS的信息创建目的数据地址，配置详情如下图。

    ![](http://static-aliyun-doc.oss-cn-hangzhou.aliyuncs.com/assets/img/161261/155650822545627_zh-CN.png)

4.  创建一个**全量迁移**的在线迁移任务，将NAS服务器内的数据迁入阿里云NAS。参数介绍请参见[创建迁移任务](../../../../cn.zh-CN/NAS 之间迁移教程/迁移实施.md#section_ksy_xmy_pfb)，配置详情如下图。

    ![](http://static-aliyun-doc.oss-cn-hangzhou.aliyuncs.com/assets/img/161261/155650822545630_zh-CN.png)

    性能调优配置如下图。

    ![](http://static-aliyun-doc.oss-cn-hangzhou.aliyuncs.com/assets/img/161261/155650822545631_zh-CN.png)

5.  迁移数据约需要 1 天时间，迁移完成后，需通过[查看迁移报告](../../../../cn.zh-CN/NAS 之间迁移教程/后续操作.md#ol_fkt_yty_pfb)，并对比源地址和目的地址的数据，确认数据已经迁移完成。

    **说明：** 若出现文件迁移失败的情况，请参考[迁移失败常见问题及解决方案](../../../../cn.zh-CN/常见问题/迁移失败常见原因及解决方案.md#)。


