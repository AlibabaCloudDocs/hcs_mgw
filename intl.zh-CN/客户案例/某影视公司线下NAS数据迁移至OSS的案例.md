# 某影视公司线下NAS数据迁移至OSS的案例 {#concept_jfw_nlb_ygb .concept}

本文介绍杭州地区某影视公司内部 NAS 服务器内的数据迁移至阿里云 OSS 长期保存的案例。

## 背景信息 {#section_ctt_cnb_ygb .section}

杭州某影视公司内部 NAS 服务器中存放有公司制作的影音文件、资料等，数据约 20TB 大小，500 万个文件。NAS 服务器在公司机房内，使用 SMB 系统，有安装防火墙，无外网连接，内网访问 IP 地址为 10.0.0.254。

现基于后续维护及线上应用开发需要，希望将 NAS 服务器内的数据存放到 OSS 中长期保存。

## 迁移方案 {#section_fmc_w5g_ygb .section}

根据用户需求及背景信息，制定如下迁移方案：

1.  创建一个杭州地域的存储空间（Bucket），并将默认数据存储地址修改为改存储空间的地址。
2.  安装一条专线，将 NAS 服务器与阿里云 VPC 网络连通，并修改 NAS 服务器的防火墙设置，允许 VPC 网络中的所有地址访问 NAS 服务器。
3.  通过在线迁移将 NAS 数据迁移至 OSS。

## 步骤一：创建 Bucket，并修改存储地址 {#section_pj3_pyg_ygb .section}

1.  在杭州地域，创建用于存储数据的 Bucket，配置方法请参见[创建存储空间](../../../../intl.zh-CN/控制台用户指南/管理存储空间/创建存储空间.md#)。
2.  设置 Bucket Policy，允许公司内部员工访问此Bucket。配置方法请参见[使用Bucket Policy授权其他用户访问OSS资源](../../../../intl.zh-CN/控制台用户指南/上传、下载和管理文件/使用Bucket Policy授权其他用户访问OSS资源.md#)。
3.  公司内部员工将默认数据存储地址修改为此Bucket。

## 步骤二：将 NAS 服务器挂载到阿里云 VPC 网络下 {#section_ntg_jch_ygb .section}

1.  根据需求，安装一条传输速度为 1Gb/s 的专线，将 NAS 服务器挂载到阿里云 VPC 网络下，详细步骤请参考[物理专线接入](../../../../intl.zh-CN/快速入门/物理专线接入.md#)。
2.  修改 NAS 服务器的防火墙设置，允许 VPC 网络所有地址访问 NAS 服务器。

## 步骤三：通过在线迁移将NAS数据迁移至OSS {#section_wk1_lgh_ygb .section}

1.  在阿里云上创建 RAM 子账号，授予子账号创建迁移任务的相关权限，并获取子账号的 AccessKey。配置步骤请参考[创建 RAM 子账号并授予相关权限](../../../../intl.zh-CN/NAS 迁移至 OSS 教程/准备工作.md#ul_z1k_23n_qfb)。
2.  创建 NAS 数据地址，参数介绍请参考[NAS 迁移至 OSS 教程](../../../../intl.zh-CN/NAS 迁移至 OSS 教程/迁移实施.md#procedure_one)。配置详情如下图：

    ![](http://static-aliyun-doc.oss-cn-hangzhou.aliyuncs.com/assets/img/134084/155736902239830_zh-CN.png)

3.  创建 OSS 数据地址，参数介绍请参考[NAS 迁移至 OSS 教程](../../../../intl.zh-CN/NAS 迁移至 OSS 教程/迁移实施.md#section_std_hkf_qfb)。配置详情如下图：![](http://static-aliyun-doc.oss-cn-hangzhou.aliyuncs.com/assets/img/134084/155736902239831_zh-CN.png)
4.  创建一个全量迁移任务，并配置性能调优。参数介绍请参考[NAS 迁移至 OSS 教程](../../../../intl.zh-CN/NAS 迁移至 OSS 教程/迁移实施.md#section_ksy_xmy_pfb)。性能调优配置详情如下图：

    ![](http://static-aliyun-doc.oss-cn-hangzhou.aliyuncs.com/assets/img/134084/155736902239832_zh-CN.png)

    **说明：** 本案例中，客户无其他应用的带宽需求，全部带宽都用于迁移数据，所以，未配置流量控制。实际使用中，请根据自身的带宽使用情况配置合理的限速规则。

5.  迁移数据约需要 2 天时间，迁移完成后，需通过[查看迁移报告](../../../../intl.zh-CN/NAS 迁移至 OSS 教程/后续操作.md#section_iwn_vtn_yfb)，并对比源地址和目的地址的数据，确认数据已经迁移完成。

    **说明：** 若出现文件迁移失败的情况，请参考[迁移失败常见问题及解决方案](../../../../intl.zh-CN/常见问题/迁移失败常见原因及解决方案.md#)。

6.  数据完成迁移之后，用户后续的数据存储、管理等都在 OSS 上进行。

