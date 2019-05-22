# Migrate data from a local NAS file system to OSS {#concept_jfw_nlb_ygb .concept}

This topic describes how to migrate data from a local Network Attached Storage \(NAS\) file system to Object Storage Service \(OSS\) for long-term storage.

## Background information {#section_ctt_cnb_ygb .section}

Assume that an entertainment company in Hangzhou needs to stores data, such as media files and documents on its local NAS file server. The data includes 5,000,000 files, which is about 20 TB. The NAS server is located in a local server room. The SMB protocol is used on the server that is enabled with a firewall. As no Internet connection exists, the private IP of the server is 10.0.0.254.

To facilitate maintenance and further development of online applications, the company needs to migrate data from the NAS server to OSS.

## Procedure {#section_fmc_w5g_ygb .section}

Based on the needs and background information, you can migrate data as follows:

1.  Create a bucket in the China \(Hangzhou\) region and change the default storage location to the data address of this bucket.
2.  Connect the NAS server to an Alibaba Cloud VPC by using a dedicated leased line. Modify the firewall settings of the NAS server and enable access to the NAS server by all IP addresses in the VPC.
3.  With Data Transport, proceed as follows to migrate data from NAS to OSS.

## Step 1: Create a bucket and modify a storage location {#section_pj3_pyg_ygb .section}

1.  In the China \(Hangzhou\) region, create a bucket to store data. For more information, see [Create a bucket](../../../../intl.en-US/Console User Guide/Manage buckets/Create a bucket.md#).
2.  Set the bucket policy and only enable access to the bucket from company employees. For more information about configurations, see [Use bucket policies to authorize other users to access OSS resources](../../../../intl.en-US/Console User Guide/Upload、download and manage objects/Use bucket policies to authorize other users to access OSS resources.md#).
3.  Inform internal employees of changing the default storage location to the data address of the bucket.

## Step 2: Connect the NAS server to the VPC {#section_ntg_jch_ygb .section}

1.  Connect the NAS server to the VPC using a dedicated data circuit with a maximum bandwidth of 1 Gbit/s. For more information, see [Connect an on-premises IDC to a VPC through a physical connection](../../../../intl.en-US/Getting Started (New Console)/Connect an on-premises IDC to a VPC through a physical connection.md#).
2.  Modify the firewall settings of the NAS server to enable access to the NAS server by all IP addresses in the VPC.

## Step 3: Migrate data from NAS to OSS by using Data Transport {#section_wk1_lgh_ygb .section}

1.  Create a RAM user in Alibaba Cloud and grant the RAM user the permission to create migration jobs. Additionally, obtain the AccessKey of the RAM user. For more information, see [Create and authorize a RAM user](../../../../intl.en-US/Migrate data from NAS to OSS/Prerequisites.md#ul_z1k_23n_qfb).
2.  Create a NAS data address. For more information about options, see [Migrate data from NAS to OSS](../../../../intl.en-US/Migrate data from NAS to OSS/Create a migration job.md#procedure_one). The options are shown in the following figure.

    ![](http://static-aliyun-doc.oss-cn-hangzhou.aliyuncs.com/assets/img/134084/155850639239830_en-US.png)

3.  Create an OSS data address. For more information about options, see [Migrate data from NAS to OSS](../../../../intl.en-US/Migrate data from NAS to OSS/Create a migration job.md#section_std_hkf_qfb). The options are shown in the following figure.![](http://static-aliyun-doc.oss-cn-hangzhou.aliyuncs.com/assets/img/134084/155850639239831_en-US.png)
4.  Create a full migration job and configure the required options in the Performance step. For more information about options, see [Migrate data from NAS to OSS](../../../../intl.en-US/Migrate data from NAS to OSS/Create a migration job.md#section_ksy_xmy_pfb). The options that you can configure in the Performance steps are shown in the following figure.

    ![](http://static-aliyun-doc.oss-cn-hangzhou.aliyuncs.com/assets/img/134084/155850639239832_en-US.png)

    **Note:** In this case, the entertainment company has no bandwidth needs for other applications while migrating data. Therefore, no flow control is set. In actual practice, you can set appropriate flow limits based on the usage of the bandwidth.

5.  A migration job requires about two days to complete. To ensure that all data is migrated, you need to [view a migration report](../../../../intl.en-US/Migrate data from NAS to OSS/Manage migration jobs.md#section_iwn_vtn_yfb) and compare data at both the source data address and the destination data address.

    **Note:** If a migration job fails, see [Common causes of a migration failure and solutions](../../../../intl.en-US/FAQ/Common causes of a migration failure and solutions.md#).

6.  After a migration job is complete, the storage of data, management of data, and other subsequent actions are all performed in OSS.

