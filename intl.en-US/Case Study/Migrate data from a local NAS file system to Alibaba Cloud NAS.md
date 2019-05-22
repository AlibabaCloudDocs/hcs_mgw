# Migrate data from a local NAS file system to Alibaba Cloud NAS {#concept_187556 .concept}

This topic describes how to migrate data from a local Network Attached Storage \(NAS\) file system to Alibaba Cloud NAS for long-term storage.

## Background information {#section_vit_81o_gi3 .section}

Assume that a Hangzhou pharmaceutical enterprise stores data, such as product documents and experimental data on its local NAS file server. The data includes 10,000,000 files, which is about 10 TB. The NAS server is located in a local s server room. The SMB protocol is used on the server that is enabled with a firewall. As no Internet connection exists, the private IP address of the server is 10.0.0.254.

Based on the considerations of data security and cost, the company needs to migrate data from the NAS server to Alibaba Cloud NAS.

## Procedure {#section_da8_t0e_6ce .section}

Based on the needs and background information, you can migrate data as follows:

1.  Create a NAS file system in the China \(Hangzhou\) region and mount the file system on an ECS instance that is located in a VPC.
2.  Connect the NAS server to a VPC by using a dedicated data circuit. Modify the firewall settings of the NAS server and enable access to the NAS server by all IP addresses in the VPC.
3.  With Data Transport, proceed as follows to migrate data from the NAS server to Alibaba Cloud NAS:

## Step 1: Create a file system in Alibaba Cloud NAS {#section_0sj_4th_w8w .section}

1.  In the China \(Hangzhou\) region, create a NAS file system whose protocol type is Network File System \(NFS\). For more information, see [Create a file system](../../../../intl.en-US/Quick Start/Create a file system.md#).
2.  Mount the file system on an ECS instance that is locate in the VPC For more information, see [Add a mount point](../../../../intl.en-US/Quick Start/Add a mount point.md#).
3.  Modify the security group of the VPC to enable access to the NAS file system from all IP addresses in the VPC. For more information, see [Use permission groups](../../../../intl.en-US/User Guide/Use permission groups.md#).

## Step 2: Attach the NAS server to the VPC {#section_398_pm5_ads .section}

1.  Connect the NAS server to the VPC where the ECS instance hosting the NAS file system is located. The connection is established using a dedicated data circuit with a maximum bandwidth of 1 Gbit/s. For more information, see [Connect an on-premises IDC to a VPC through a physical connection](../../../../intl.en-US/Getting Started (New Console)/Connect an on-premises IDC to a VPC through a physical connection.md#).
2.  Modify the firewall settings of the NAS server to enable access to the NAS server by all IP addresses in the VPC.

## Step 3: Create a migration job {#section_3eo_026_sj7 .section}

1.  Create a RAM user in Alibaba Cloud and grant the RAM user the permission to create a migration job. For more information, see [Create and authorize a RAM user](../../../../intl.en-US/Migrate data from NAS to OSS/Prerequisites.md#ul_z1k_23n_qfb).
2.  Log on to the [Data Transport](https://mgw.console.aliyun.com/#/source?_k=0k9yvg) console by using the RAM user account. Use the information of the local NAS server to create the source data address. For more information about options, see [Create a source data address](../../../../intl.en-US/Migrate data between NAS file systems/Create a migration job.md#one). The configuration details are shown in the following figure.

    ![](http://static-aliyun-doc.oss-cn-hangzhou.aliyuncs.com/assets/img/161261/155850460645626_en-US.png)

3.  Use the information of the Alibaba Cloud NAS file system to create the destination data address. The configuration details are shown in the following figure.

    ![](http://static-aliyun-doc.oss-cn-hangzhou.aliyuncs.com/assets/img/161261/155850460645627_en-US.png)

4.  Create a **full** migration job to migrate data from the NAS server to Alibaba Cloud NAS. For more information about options, see [Create a migration job](../../../../intl.en-US/Migrate data between NAS file systems/Create a migration job.md#section_ksy_xmy_pfb).

    ![](http://static-aliyun-doc.oss-cn-hangzhou.aliyuncs.com/assets/img/161261/155850460745630_en-US.png)

    The options that you can specify in the Performance step are shown in the following figure.

    ![](http://static-aliyun-doc.oss-cn-hangzhou.aliyuncs.com/assets/img/161261/155850460745631_en-US.png)

5.  A migration job requires about two days to complete. To ensure that all data is migrated, you need to [view a migration report](../../../../intl.en-US/Migrate data between NAS file systems/Manage migration jobs.md#ol_fkt_yty_pfb) and compare data at both the source data address and the destination data address.

    **Note:** If a migration job fails, see [Common causes of a migration failure and solutions](../../../../intl.en-US/FAQ/Common causes of a migration failure and solutions.md#).


