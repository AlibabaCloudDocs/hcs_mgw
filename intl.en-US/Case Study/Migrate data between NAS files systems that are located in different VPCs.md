# Migrate data between NAS files systems that are located in different VPCs {#concept_183077 .concept}

This topic describes how to migrate data between NAS file systems that are located in different VPCs.

## Background information {#section_rl5_wdl_rve .section}

Assume that a Shenzhen company is named Company A. As Company A grows and needs to expand, this company establishes a sub-branch in Hangzhou, which is named Branch B. The data of Branch B is stored in a NAS file system. However, you must synchronize the data of Branch B to another NAS file system where the data of Company A is stored. Each day, Branch B generates about 100,000 files whose size is about 100 GB.

The NAS file system of Company A and the NAS file system of Branch B are located in separate VPCs. The 172.16.1.0/24 IP segment is used for the VPC where the file system of Company A is located, which is abbreviated as VPC A. The 10.0.0.0/24 IP segment is used for the VPC where the file system of Branch B is located, which is abbreviated as VPC B.

**Note:** If you are using third-party NAS file systems, you must attach the NAS server to an Alibaba Cloud VPC by using a dedicated data circuit. For more information, see [Connect an on-premises IDC to a VPC through a physical connection](../../../../intl.en-US/Getting Started (New Console)/Connect an on-premises IDC to a VPC through a physical connection.md#).

## Create a migration job {#section_0qo_vzs_lxl .section}

1.  You can establish the connection between VPC A and VPC B by using Cloud Enterprise Network \(CEN\) and configure permission groups.
2.  Create a synchronization job that synchronizes the data of Branch B to Company A on a regular basis.

## Step 1: Connect VPC A with VPC B by using CEN {#section_fr0_bx7_ym0 .section}

1.  With CEN, you can connect VPC A with VPC B. For more information, see [Connect VPCs that are located in multiple regions and owned by different accounts](../../../../intl.en-US/Quick Start/Connect network instances in different regions using different accounts.md#).
2.  Modify the NAS permission groups of Company A and Branch B. This allows all devices in the 10.0.0.0/24 IP segment to read data from the NAS file system of Branch B and write data to the NAS file system of Company A. For more information, see [Use permission groups](../../../../intl.en-US/User Guide/Use permission groups.md#).

## Step 3: Create a migration job {#section_dao_p6e_jyo .section}

1.  Create a RAM user and grant the RAM user the permission to create migration jobs. For more information, see [Create and authorize a RAM user](../../../../intl.en-US/Migrate data from NAS to OSS/Prerequisites.md#ul_z1k_23n_qfb).
2.  Create the source NAS data address. For more information about options, see [Create the source data address](../../../../intl.en-US/Migrate data between NAS file systems/Create a migration job.md#one). The options are shown in the following figure.

    ![](http://static-aliyun-doc.oss-cn-hangzhou.aliyuncs.com/assets/img/156897/155850516845080_en-US.png)

3.  Create the destination NAS data address. For more information about options, see [Create the destination data address](../../../../intl.en-US/Migrate data between NAS file systems/Create a migration job.md#section_jzz_pjj_yfb). The options are shown in the following figure.

    ![](http://static-aliyun-doc.oss-cn-hangzhou.aliyuncs.com/assets/img/156897/155850516945081_en-US.png)

4.  Create a **Sync** migration job. To ensure business continuity, set the daily start time of a synchronization job to 22:00. For more information about options, see [Create a migration job](../../../../intl.en-US/Migrate data between NAS file systems/Create a migration job.md#section_ksy_xmy_pfb). The options are shown in the following figure.

    ![](http://static-aliyun-doc.oss-cn-hangzhou.aliyuncs.com/assets/img/156897/155850516945090_en-US.png)

    **Note:** 

    -   A synchronization job keeps running until you manually stop the job. Therefore, you only need to create one synchronization job to meet the needs of synchronizing data on a regular basis.
    -   In this case, as the customer synchronizes a small amount of data during off-peak hours, you can use the default settings in the Performance step. In actual practice, you can set appropriate performance options based on the usage status of the bandwidth.
5.  After each synchronization subtask is complete, you must perform specific actions to ensure data is synchronized. These actions include viewing the status of a subtask, and comparing the data of the source data address with that of the destination data address. For more information about how to view the status of synchronization subtasks, see [Manage synchronization subtasks](../../../../intl.en-US/Migrate data between NAS file systems/Manage migration jobs.md#ul_rs2_4vn_yfb).

