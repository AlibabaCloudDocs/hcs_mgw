# Create a migration job {#concept_aws_xzl_qfb .concept}

This topic describes the operations and considerations for data migration.

## Precautions {#section_lfs_tth_yfb .section}

When creating a migration job, you must note the following issues:

-   A migration job occupies the network resources of the source data address and destination data address. To ensure business continuity, we recommend that you specify a speed limit for a migration job or perform the migration job during off-peak hours.
-   Before a migration job is performed, files at both the source data address and the destination data address are checked. The files at the destination data address are overwritten during a migration job. This occurs if the source files have the same name as the destination files, but have a later update time. If two files have the same name but different content, you must change the name of one file or back up the files.
-   With Data Transport, you can only migrate data of a single bucket rather than all data in an account at a time.

## Step 1: Create a source data address {#section_dyf_jmy_pfb .section}

1.  Log on to the [Data Transport console](https://mgw.console.aliyun.com/#/job?_k=6w2hbo).
2.  Choose **Data Online Migration** \> **Data Address**, and then click **Create Data Address**.
3.  In the Create Data Address dialog box, set the required options and click **OK**. The options are described as follows:

    |Option|Required|Description|
    |:-----|:-------|:----------|
    |**Data Type**|Yes|Select **Tencent COS**.|
    |**Data Name**|Yes|The data name can be 3 to 63 characters in length. Special characters are not supported, except for hyphens \(-\) and underscores \(\_\).|
    |**Region**|Yes|Enter the name of a region that hosts a bucket, such as **ap-guangzhou** represents the Chine South region. **Note:** In the [Tecent COS console](https://console.cloud.tencent.com/cos5), choose **Buckets** \> **Configuration** \> **Bacic Information** to view the region that hosts the bucket.

 |
    |**Bucket**|Yes|Enter the name of a Tencent COS bucket. The format of a Tencent COS bucket name is `<custom name>-<APPID>`. For a bucket name such as `tony-1234567890`. **Note:** Log on to the Tencent COS console to view details of APPID on the Accounts page.

 |
    |**Prefix**|Yes|     -   Migrate all data: indicates that all data in a bucket is migrated.

When you migrate all data, you do not need to enter a prefix.

    -   Migrate partial data: indicates that files in a specified directory or prefix are migrated. A prefix cannot start with a forward slash \(/\) and must end with a forward slash \(/\). For example, `/data/to/oss/` or `/docs/`.
 |
    |**SecretId and SecretKey**|Yes|Enter an Accesskey that is used to migrate data. We recommend that you create a new AccessKey for this migration and delete it after the migration.|

4.  You must apply for whitelist permissions because this feature is still in the beta testing phase. Click **Application**.
5.  Enter the required information and submit the beta testing application for migration. After the application has been approved, you will receive an SMS notification.

## Step 2: Create a destination data address {#section_bzy_smy_pfb .section}

1.  Choose **Data Online Migration** \> **Data Address** and click **Create Data Address**.
2.  In the Create Data Address dialog box, set the required options and click **OK**. The options are described as follows:

    |Option|Required|Description|
    |:-----|:-------|:----------|
    |**Data Type**|Yes|Select **OSS**.|
    |**Data Region**|Yes|Select a region where the destination data address is located.|
    |**Data Name**|Yes|The data name can be 3 to 63 characters in length. Special characters are not supported, except for hyphens \(-\) and underscores \(\_\).|
    |**OSS Endpoint**|Yes|Select an endpoint based on the region where data is located. For more information, see [Endpoints](../../../../../intl.en-US/Developer Guide/Endpoint/Regions and endpoints.md#).|
    |**AccessKeyId and AccessKeySecret**|Yes|Enter an AccessKey that is used to migrate data. For more information, see [Create an AccessKey](intl.en-US/Migrate data from HTTP__HTTPS sources to OSS/Prerequisites.md#ak).|
    |**OSS Bucket**|Yes|Select a bucket to store migration data.|
    |**OSS Prefix**|No|An OSS prefix cannot start with a forward slash \(/\) and must end with a forward slash \(/\). For example: `data/to/oss/`. If you want to store data to the root directory of a bucket, you can leave the OSS Prefix field blank.|

    **Note:** When the name of a file to be migrated at the source data address starts with a forward slash \(/\), you must add an OSS Prefix to the file name, or the migration job fails. Assume that the name of a file to be migrated is /test/test.png. You must add an OSS Prefix to the file name. Fox example, oss/. After a migration job is complete, an OSS file whose name is`/test/test.png` changes to oss//test/test.png.


## Step 3: Create a migration job {#section_ksy_xmy_pfb .section}

1.  Choose **Data Online Migration** \> **Migration Jobs** and click **Create Job**.
2.  In the Create Job dialog box, read the Terms of Data Transport, select **I understand the above terms and conditions, activate Data Transport**, and then click **Next**.
3.  In the Create Job dialog box, set the required options and click **Next**.

    The options are described as follows:

    |Option|Required|Description|
    |:-----|:-------|:----------|
    |**Job Name**|Yes|The job name can be 3 to 63 characters in length and contain lowercase letters, numbers, and hyphens \(-\). A job name cannot start or end with a hyphen \(-\).|
    |**Source Data Address**|Yes|Select the created source data address.|
    |**Destination Data Address**|Yes|Select the created destination data address. **Note:** If the country where the source data address is located is different from the country where the destination data address is located, you can [open a ticket](https://selfservice.console.aliyun.com) to apply for the permission of creating a cross-country migration job. You must ensure that your business is legitimate, data transit conforms to local rules and regulations, and that the data does not include illegal information.

 |
    |**Migration Type**|Yes|     -   **Full**: You can specify the **Start Time Point of File**. The files whose last modification time is later than the specified start time are migrated. After all of the files are migrated, a migration job is closed.**** When you repeat a full migration job, Data Transport only migrates files that have been changed.
    -   **Incremental**: You must specify the **Migration Interval** and **Migration Times** to perform an incremental migration job. You must specify the **Start Point Time of File**. The files whose last modification time is later than the specified start time are migrated for the first time.**** After the first migration job is complete, an incremental migration job is performed based on the **Migration Interval**. A migration interval involves sending files that are created or modified within a specific range from the source data address to the destination data address. The specified range is the time when the last migration job started and before the time when this migration starts. Assume that you specify N for the Migration Times. A full migration is performed once. In the future, an incremental migration will be performed \(N - 1\) times. For example, you set the Migration Interval to 1 and Migration Times to 5. Additionally, you set the **Start Time Point of File** to 2019/03/05 08:00:00. The present date and time is 2019/03/10 08:00. When you perform a migration job for the first time, Data Transport migrates files whose last modification time is between 2019/03/05 08:00 and 2019/03/10 08:00. Assume that the first migration job requires one hour to complete. The second migration job starts at 2019/03/10 10:00, which is two hours later than 2019/03/10 08:00. The migration job takes one hour and the other hour is consumed by the specified migration interval. When you perform the second migration job, files whose last modification time is between 2019/03/10 08:00 and 2019/03/10 10:00 are migrated. The migration job includes a full migration and four incremental migrations.
 **Note:** Before you start a full migration job or an incremental migration job, Data Transport compares files of the source data address with those of the destination data address. If a source file has the same name as a destination file, the destination file is overwritten when at least one of the following conditions is met:

    -   Content-Type of the source file is different from that of the destination file.
    -   The source file has a later modification time.
    -   The size of the source file is different from that of the destination file.
 |
    |**Start Time Point of File**|Yes|     -   All: All files are migrated.
    -   Assign: Files that are created or modified after the specified time are migrated. For example, when you set the Start Time Point of File to 2018/11/01 08:00:00, only files that are created or modified after 2018/11/01 08:00:00 are migrated. The files that are created or modified before the specified time will be skipped.
 |
    |**Migration Interval**|Yes \(only for Incremental migration\)|The default value is 1 Hour and the maximum value is 24 Hours.|
    |**Migration Times**|Yes \(only for Incremental migration\)|The default value is 1 time and the maximum value is 30 times.|

4.  On the Performance tab, navigate to the **Data Prediction**area and enter the **Data Size** and **File Count**.

    **Note:** To ensure a successful migration, you must estimate the amount of data to be migrated. For more information, see [Estimate the amount of data to be migrated](intl.en-US/Migrate data from Tencent Cloud Object Service (COS) to OSS/Prerequisites.md#volume).

5.  This step is optional. On the Performance tab, navigate to the **Flow Control** area and set the **Time Range** and the **Max Flow**, and then click **Add**.

    **Note:** To ensure business continuity, we recommend that you set the **Time Range** and **Max Flow** based on the fluctuation of visits.

6.  Click **Create**. Wait until a migration job is complete.

