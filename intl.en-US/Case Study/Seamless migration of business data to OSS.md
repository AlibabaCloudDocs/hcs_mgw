# Seamless migration of business data to OSS {#concept_orz_jyc_3gb .concept}

This section describes how to migrate data from a cloud service to Alibaba Cloud OSS.

## Background information {#section_ptv_4yc_3gb .section}

Company A is an Internet service provider that deploys the main business application in a cloud service provided by Cloud Service Provider B. The main business application of Company A provides online editing services for media files \(such as images, videos\). The existing data that is stored in Service Provider B includes 100,000,000 files and has a total size of about 320 TB with a daily increase of 20 GB. The bandwidth for both the storage service of Service Provider B and OSS is 250 Mbit/s. The business application requires a maximum bandwidth of 50 Mbit/s.

The company is considering a switch to OSS because of its further development needs. When you switch over businesses between two data stores, you must migrate existing data and incremental data to OSS. To ensure a successful migration of large amounts of data and business continuity, the following needs must be met:

-   During the migration job, you must ensure business continuity and diminish the impacts of normal data access from end users.
-   After the migration job is complete, you must check data integrity to ensure a seamless switch of the business to OSS.

## Procedure {#section_s3z_51d_3gb .section}

Based on the needs and background information, proceed as follows to migrate data:

1.  With Data Transport, you can migrate existing data from a cloud service to OSS. Before a migration job is complete, ensure that no updates occur on the customer side.
2.  After existing data is migrated, you can create back-to-origin rules in OSS for users to access un-migrated incremental data.
3.  Switch businesses to OSS.
4.  After the business switchover is complete, you can migrate incremental data to OSS using Data Transport.
5.  After all data is migrated and validated, delete the data at the source data address.

## Step 1: Migrate existing data {#section_dpk_z5j_3gb .section}

1.  Create an OSS bucket to store migrated data. For more information, see [Create a bucket](../../../../intl.en-US/Console User Guide/Manage buckets/Create a bucket.md#).
2.  Create the AccessKey of a RAM user that is used to migrate data.
    -   To obtain the AccessKey of the storage service provided by Service Provider B, log on to storage service console to view the AccessKey.
    -   To obtain the AccessKey of a RAM user, see [Create and authorize a RAM user](../../../../intl.en-US/Migrate data from Baidu Object Storage (BOS) to OSS/Prerequisites.md#section_p11_xff_qfb).
3.  Create data addresses and a full migration job. For more information, see [Data Transport](https://www.alibabacloud.com/help/product/94157.htm) documents. Configure the required options on the **Job Config** tab as follows.

    ![](http://static-aliyun-doc.oss-cn-hangzhou.aliyuncs.com/assets/img/88193/155737932236102_en-US.png)

    Configure the required options on the **Performance** tab as follows.

    ![](http://static-aliyun-doc.oss-cn-hangzhou.aliyuncs.com/assets/img/88193/155737932236097_en-US.png)

4.  To ensure that all data is migrated after migration, you need to [view a migration report](../../../../intl.en-US/Migrate data from Baidu Object Storage (BOS) to OSS/Manage migration jobs.md#section_jxv_xty_pfb) and compare data at both the source data address and the destination data address.

    **Note:** If a migration job fails, see [Common causes of a migration failure and solutions](../../../../intl.en-US/FAQ/Common causes of a migration failure and solutions.md#).


## Step 2: Create back-to-origin rules {#section_dw2_jbk_3gb .section}

It takes about 25 days to migrate the existing data. During the migration process, data is continuously growing at the source data address. To ensure business continuity and a seamless switchover, you need to create Back-to-Origin rules. When files that are requested by end users do not exist in OSS, OSS fetches these files from the source data address and return them to end users.

1.  Log on to the [OSS console](https://oss.console.aliyun.com/).
2.  On the list of buckets, select the bucket where migrated data is located.
3.  Select **Basic Settings** and click **Configure** in the **Back-to-Origin** section.
4.  Click **Create Rule**. In the **Create Rule** dialog box, configure the required options.

    ![](http://static-aliyun-doc.oss-cn-hangzhou.aliyuncs.com/assets/img/88193/155737932236104_en-US.png)

    -   **Mode**: Select **Mirroring**.
    -   **Prerequisite**: **HTTP Status Code 404** is selected by default. You can configure the **File Name Prefix** as needed.
    -   **Origin URL**: Enter the address of an endpoint.
    -   For more parameter settings, see [Create back-to-origin rules](../../../../intl.en-US/Console User Guide/Manage buckets/Set back-to-origin rules.md#).
    **Note:** You can create a maximum of five back-to-origin rules. The five rules take effect at the same time. For multiple source data addresses, you can create multiple back-to-origin rules. You can enable OSS to fetch various types of data by setting different values for the **File Name Prefix**.

5.  Click **OK**.

## Step 3: Switch businesses to OSS {#section_z25_fqq_3gb .section}

Change the previous data address where the business application obtain data to OSS

## Step 4: Migrate incremental data {#section_xsv_jqq_3gb .section}

During the migration of existing data, about 100,000 files that reach a total size of about 500 GB are generated. You must migrate these incremental files to OSS.

1.  Create an incremental migration job based on the steps described in Step 1. Configure the required options on the **Job Config** tab as follows:

    ![](http://static-aliyun-doc.oss-cn-hangzhou.aliyuncs.com/assets/img/88193/155737932236098_en-US.png)

    Configure the required options on the **Performance** tab as follows:

    ![](http://static-aliyun-doc.oss-cn-hangzhou.aliyuncs.com/assets/img/88193/155737932336101_en-US.png)

2.  Click **Create** to create a migration job.
3.  To ensure that all data is migrated after migration, you need to [view a migration report](../../../../intl.en-US/Migrate data from Baidu Object Storage (BOS) to OSS/Manage migration jobs.md#section_jxv_xty_pfb) and compare data at both the source data address and the destination data address.

    **Note:** If a migration job fails, see [Common causes of a migration failure and solutions](../../../../intl.en-US/FAQ/Common causes of a migration failure and solutions.md#).


## Step 5: Delete data at the source data address {#section_ffv_lcr_3gb .section}

After a migration job is complete, you can create a lifecycle rule for files at the source data address to avoid extra charges for storage. This rule sets an expiration date for files of one day after the time when the migration job is complete. All data at the source data address is deleted on the expiration date.

