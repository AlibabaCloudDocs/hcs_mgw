# Migrate data from a local NAS file system to OSS {#concept_jfw_nlb_ygb .concept}

This section describes how to migrate data from a local Network Attached Storage \(NAS\) file system to Object Storage Service \(OSS\) for long-term storage.

## Background information {#section_ctt_cnb_ygb .section}

Assume that an Hangzhou entertainment company stores data, such as media files and documents on its local NAS file server. The size of data that includes 5,000,000 files is about 20 TB. The NAS server is located in an on-premises server room. The SMB protocol is used on the server that is enabled with a firewall. As no Internet connection exists, the private IP of the server is 10.0.0.254.

To facilitate later maintenance and develop online applications, the company needs to migrate data from the NAS server to OSS.

## Procedure {#section_fmc_w5g_ygb .section}

Based on the needs and background information, you can migrate data as follows:

1.  Create a bucket in China \(Hangzhou\) and change the default storage location to the data address of the bucket.
2.  Connect the NAS server to an Alibaba Cloud VPC using a dedicated leased line. Modify the firewall settings of the NAS server and enable access to the NAS server by all IP addresses in the VPC.
3.  With Data Transport, proceed as follows to migrate data from NAS to OSS.

## Step 1: Create a bucket and modify a storage location {#section_pj3_pyg_ygb .section}

1.  In China \(Hangzhou\), create a bucket to store data. For more information, see [Create a bucket](../../../../intl.en-US/Console User Guide/Manage buckets/Create a bucket.md#).
2.  Set the bucket policy and only enable access to the bucket from company employees. For more information, see [Use bucket policies to authorize other users to access OSS resources](../../../../intl.en-US/Console User Guide/Upload、download and manage objects/Use bucket policies to authorize other users to access OSS resources.md#).
3.  Inform internal employees of changing the default storage location to the data address of the bucket.

## Step 2: Connect the NAS server to a VPC {#section_ntg_jch_ygb .section}

1.  Connect the NAS server to a VPC using a dedicated leased line with a maximum bandwidth of 1 Gbit/s. For more information, see [Connect an on-premises IDC to a VPC through a physical connection](../../../../intl.en-US/Getting Started (New Console)/Connect an on-premises IDC to a VPC through a physical connection.md#).
2.  Modify the firewall settings of the NAS server to enable access to the NAS server by all IP addresses of the 192.168.1.0/24 IP segment in the VPC.

## Step 3: Migrate data from NAS to OSS using Data Transport {#section_wk1_lgh_ygb .section}

1.  Create a RAM user and authorize the user to create migration jobs. Additionally, obtain the AccessKey of the RAM user. For more information, see [Create and authorize a RAM user](../../../../intl.en-US/Migrate data from NAS to OSS/Prerequisites.md#ul_z1k_23n_qfb).
2.  Create a NAS data address. For more information about options, see [Migrate data from NAS to OSS](../../../../intl.en-US/Migrate data from NAS to OSS/Create a migration job.md#procedure_one). The options are shown in the following figure.

    ![](http://static-aliyun-doc.oss-cn-hangzhou.aliyuncs.com/assets/img/134084/155736914139830_en-US.png)

3.  Create an OSS data address. For more information about options, see [Migrate data from NAS to OSS](../../../../intl.en-US/Migrate data from NAS to OSS/Create a migration job.md#section_std_hkf_qfb). The options are shown in the following figure.

    ![](http://static-aliyun-doc.oss-cn-hangzhou.aliyuncs.com/assets/img/134084/155736914139831_en-US.png)

4.  Create a full migration job and configure performance options. For more information about options, see [Migrate data from NAS to OSS](../../../../intl.en-US/Migrate data from NAS to OSS/Create a migration job.md#section_ksy_xmy_pfb). The performance options are shown in the following figure.

    ![](http://static-aliyun-doc.oss-cn-hangzhou.aliyuncs.com/assets/img/134084/155736914139832_en-US.png)

    **Note:** In this case, the customer is migrating data and has no bandwidth needs for other applications. Therefore, no flow control is set. In actual practice, you can set appropriate flow limits based on the usage status of the bandwidth.

5.  A migration job requires about two days to complete. To ensure that all data is migrated after migration, you need to [view a migration report](../../../../intl.en-US/Migrate data from NAS to OSS/Manage migration jobs.md#section_iwn_vtn_yfb) and compare data at both the source data address and the destination data address.

    **Note:** If a migration job fails, see [Common causes of a migration failure and solutions](../../../../intl.en-US/FAQ/Common causes of a migration failure and solutions.md#).

6.  After the data is migrated, subsequent data storage and management will be performed on OSS.

