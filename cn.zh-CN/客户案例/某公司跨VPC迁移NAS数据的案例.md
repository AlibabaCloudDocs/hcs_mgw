# 某公司跨VPC迁移NAS数据的案例 {#concept_183077 .concept}

本文主要介绍某公司跨VPC迁移NAS数据的案例。

## 背景信息 {#section_rl5_wdl_rve .section}

深圳某公司A（简称为A）发展壮大后，在杭州创建了子公司B（简称为B）。B的数据单独存放在阿里云NAS服务中，但是需要每天将数据同步至A的阿里云NAS上保存。B每天约产生10万个，共约100GB大小的文件。

A和B的阿里云NAS均已挂载在阿里云VPC网络下。A的VPC网段是172.16.1.0/24，B的VPC网段是10.0.0.0/24。

**说明：** 若您使用的是非阿里云NAS，您需要通过专线将您的NAS服务器挂载到阿里云VPC网络下。详情请参见[物理专线接入](../../../../dita-oss-bucket/SP_72/DNexpressconnect1847151/ZH-CN_TP_13831.md#)。

## 迁移方案 {#section_0qo_vzs_lxl .section}

1.  通过云企业网将A和B的VPC网络连通并设置权限组，允许B的VPC网络所有地址可以只读访问B的NAS，可以读写访问A的NAS。
2.  创建在线迁移任务，定期将B的数据同步给A。

## 步骤一：通过云企业网将A和B的VPC网络连通 {#section_fr0_bx7_ym0 .section}

1.  通过云企业网，将A和B账号下的VPC网络连通，详情配置步骤请参见[跨账号跨地域VPC互通](../../../../dita-oss-bucket/SP_17/DNBACK1824530/ZH-CN_TP_3047.md#)。
2.  修改A和B的NAS权限组，允许10.0.0.0/24网段内所有设备可以读取B的NAS数据，可以在A的NAS中写入数据。详情请参见[管理文件系统数据访问权限](../../../../dita-oss-bucket/SP_111/DNnas1882233/ZH-CN_TP_18697_V5.md#)。

## 步骤二：创建迁移任务 {#section_dao_p6e_jyo .section}

1.  在阿里云上创建RAM子账号，授予子账号创建迁移任务的相关权限。配置步骤请参考[创建RAM子账号并授予相关权限](../../../../dita-oss-bucket/SP_188/DNHCS_MGW18101150/ZH-CN_TP_65250.md#ul_z1k_23n_qfb)。
2.  创建源NAS数据地址，参数介绍请参考[创建源数据地址](../../../../dita-oss-bucket/SP_188/DNHCS_MGW18101151/ZH-CN_TP_65295_V5.md#one)，配置详情如下图。

    ![](images/45080_zh-CN.png)

3.  创建目的NAS数据地址，参数介绍请参考[创建目的数据地址](../../../../dita-oss-bucket/SP_188/DNHCS_MGW18101151/ZH-CN_TP_65295_V5.md#section_jzz_pjj_yfb)，配置详情如下图。

    ![](images/45081_zh-CN.png)

4.  创建一个**数据同步**类型的迁移任务。为了不影响正常工作，每天22:00:00开始数据同步。参数介绍请参考[创建迁移任务](../../../../dita-oss-bucket/SP_188/DNHCS_MGW18101151/ZH-CN_TP_65295_V5.md#section_ksy_xmy_pfb)。任务配置详情如下图：

    ![](images/45090_zh-CN.png)

    **说明：** 

    -   数据同步类型的迁移任务在手动停止任务前，会一直在运行，所以定期的数据同步需求，您仅需创建一条迁移任务即可。
    -   本案例中，客户在非工作时间段同步数据，且数据量不大，所以性能调优使用默认设置。实际使用中，请根据自身的实际情况配置性能调优参数。
5.  每次同步任务完成后，您可以查看同步任务，对比源地址和目的地址的数据，确认数据已经同步完成。查看同步任务的方法请参见[管理同步任务](../../../../dita-oss-bucket/SP_188/DNHCS_MGW18101151/ZH-CN_TP_65296_V6.md#ul_rs2_4vn_yfb)。

