# Create a migration job {#concept_xqy_22m_qfb .concept}

This topic describes the operations and considerations for data migration.

## Precautions {#section_lfs_tth_yfb .section}

When creating a migration job, you must note the following issues:

-   A migration job occupies the network resources of the source data address and destination data address. To ensure business continuity, we recommend that you specify a speed limit for a migration job or perform the migration job during off-peak hours.
-   Before a migration job is performed, files at both the source data address and the destination data address are checked. The files at the destination data address are overwritten during a migration job. This occurs if the source files have the same name as the destination files, but have a later update time. If two files have the same name but different content, you must change the name of one file or back up the files.
-   With Data Transport, you can only migrate data of a single bucket rather than all data in an account at a time.

## Step 1: Create a source data address {#procedure_one .section}

1.  Log on to the [Data Transport console](https://mgw.console.aliyun.com/#/job?_k=6w2hbo).
2.  Choose **Data Online Migration** \> **Data Address**, and then click **Create Data Address**.
3.  In the Create Data Address dialog box, set the required options and click **OK**. The options are described as follows:

    |Option|Required|Description|
    |:-----|:-------|:----------|
    |**Data Type**|Yes|Select **QI NIU**.|
    |**Data Name**|Yes|The data name can be 3 to 63 characters in length. Special characters are not supported, except for hyphens \(-\) and underscores \(\_\).|
    |**Endpoint**|Yes|Enter an endpoint. The endpoint corresponds to the region that hosts a bucket. The format is **http://<a Qiniu Cloud-Integrated CDN \(FUSION\) test domain\>** or **http://<a custom FUSION accelerating domain\>**. You can log on to the Qiniu Cloud console. Open the Object Storage page, select a bucket, and view a domain in the **Test Domain** column or the **Accelerating Domain** column.

 **Note:** FUSION sets a daily flow limit of 10 GB for **test domains**. When the size of data to be migrated exceeds 10 GB, we recommend that you separate data into multiple collections. You can either migrate these collections one by one or use **FUSION accelerating domains**. For more information, see [Limits on test domains](https://developer.qiniu.com/fusion/kb/1319/test-domain-access-restriction-rules).

 |
    |**Bucket**|Yes|Qiniu uses buckets to store data. You can only enter the custom name of a bucket. For a bucket name such as `tony-1234567890`, you can enter **tony**.|
    |**Prefix**|Yes|     -   Migrate all data: indicates that all data in a bucket is migrated.

When you migrate all data, you do not need to enter a prefix.

    -   Migrate partial data: indicates that files in a specified directory or prefix have been migrated. A prefix cannot start with a forward slash \(/\) and must end with a forward slash \(/\). For example: `data/to/oss/`.
 |
    |**AccessKey and SecretKey**|Yes|Enter an Accesskey that is used to migrate data. We recommend that you create a new AccessKey for this migration and delete it after the migration.|

4.  You must apply for whitelist permissions because this feature is still in the beta testing phase. Click **Application**.
5.  Enter the required information and submit the beta testing application for migration. After the application has been approved, you will receive an SMS notification.

## Step 2: Create a destination data address {#section_std_hkf_qfb .section}

1.  Choose **Data Online Migration** \> **Data Address** and click **Create Data Address**.
2.  In the Create Data Address dialog box, set the required options and click **OK**. The options are described as follows.

    |Option|Required|Description|
    |:-----|:-------|:----------|
    |**Data Type**|Yes|Select **OSS**.|
    |**Data Region**|Yes|Select a region where the destination data address is located.|
    |**Data Name**|Yes|The data name can be 3 to 63 characters in length. Special characters are not supported, except for hyphens \(-\) and underscores \(\_\).|
    |**OSS Endpoint**|Yes|Select an endpoint based on the region where data is located. For more information, see [Endpoints](../../../../../intl.en-US/Developer Guide/Endpoint/Regions and endpoints.md#).|
    |**AccessKeyId and AccessKeySecret**|Yes|Enter an AccessKey that is used to migrate data. For more information, see [Create an AccessKey](intl.en-US/Migrate data from HTTP__HTTPS sources to OSS/Prerequisites.md#ak).|
    |**OSS Bucket**|Yes|Select a bucket to store migration data.|
    |**OSS Prefix**|No|An OSS prefix cannot start with a forward slash \(/\) and must end with a forward slash \(/\). For example: `data/to/oss/`. If you want to store data to the root directory of a bucket, you can leave the OSS Prefix field blank.|

    **Note:** When the name of a file to be migrated at the source data address starts with a forward slash \(/\), you must add an OSS Prefix to the file name, or the migration job fails. Assume that the name of a file to be migrated is /test/test.png. You must add an OSS Prefix to the file name such as oss/. After a migration job is complete, an OSS file whose name is `/test/test.png` changes to oss//test/test.png.


## Step 3: Create a migration job {#section_ksy_xmy_pfb .section}

1.  Choose **Data Online Migration** \> **Migration Jobs** and click **Create Job**.
2.  In the Create Job dialog box, read the Terms of Data Transport, select **I understand the above terms and conditions, activate Data Transport**, and then click **Next**.
3.  In the Create Job dialog box, set the required options and click **Next**.

    The options are described as follows:

    |Option|Required|Description|
    |:-----|:-------|:----------|
    |**Job Name**|Yes|A job name can be 3 to 63 characters in length and contain lowercase letters, numbers, and hyphens \(-\). A job name cannot start or end with a hyphen \(-\).|
    |**Source Data Address**|Yes|Select the created source data address.|
    |**Destination Data Address**|Yes|Select the created destination data address. **Note:** You can [open a ticket](https://selfservice.console.aliyun.com) to apply for permission to create a cross-country migration job. This occurs if the country where the source data address is located is different from the country where the destination data address is located. You must ensure that your business is legitimate, data does not include illegal information, and data transit conforms to local rules and regulations.

 |
    |**Migration Type**|Yes|     -   **Full**: You can specify the **Start Time Point of File**. The files whose last modification time is later than the specified start time are migrated. After all of the files are migrated, a migration job is closed.**** When you repeat a full migration job, Data Transport only migrates files that have been changed.
    -   **Incremental**: You must specify the **Migration Interval** and **Migration Times** to perform an incremental migration job. You must specify the **Start Point Time of File**. The files whose last modification time is later than the specified start time are migrated for the first time.**** After the first migration job is complete, an incremental migration job is performed based on the **Migration Interval**. A migration interval involves sending files that are created or modified within a specific range from the source data address to the destination data address. The specified range is the time when the last migration job started and before the time when this migration starts. Assume that you specify N for the Migration Times. A full migration is performed once. In the future, an incremental migration will be performed \(N - 1\) times. For example, you set the Migration Interval to 1 and Migration Times to 5. Additionally, you set the **Start Time Point of File** to 2019/03/05 08:00:00. The present date and time is 2019/03/10 08:00. When you perform a migration job for the first time, Data Transport migrates files whose last modification time is between 2019/03/05 08:00 and 2019/03/10 08:00. Assume that the first migration job requires one hour to complete. The second migration job starts at 2019/03/10 10:00, which is two hours later than 2019/03/10 08:00. The migration job takes one hour and the other hour is consumed by the specified migration interval. When you perform the second migration job, files whose last modification time is between 2019/03/10 08:00 and 2019/03/10 10:00 are migrated. The migration job includes a full migration and four incremental migrations.
 **Note:** Before you start a full migration job or an incremental migration job, Data Transport compares files of the source data address with those of the destination data address. If a source file has the same name as a destination file, the destination file is overwritten when at least one of the following conditions is met:

    -   Content-Type of the source file is different from that of the destination file.
    -   The source file has a later modification time.
    -   The size of the source file is different from that of the destination file.
 |
    |**Start Time Point of File**|Yes|     -   All: All files are migrated.
    -   Assign: Files that are created or modified after the specified time are migrated. For example, when you set the Start Time Point of File to 2018/11/01 08:00:00, only files that are created or modified after 2018/11/01 08:00:00 are migrated. Files that are created or modified before the specified time are disregarded.
 |
    |**Migration Interval**|Yes \(only for Incremental migration\)|The default value is 1 Hour and the maximum value is 24 Hours.|
    |**Migration Times**|Yes \(only for Incremental migration\)|The default value is 1 time and the maximum value is 30 times.|

4.  On the Performance tab, navigate to the **Data Prediction**area and enter **Data Size** and **File Count**.

    **Note:** To ensure a successful migration, you must estimate the amount of data to be migrated. For more information, see [Estimate the amount of data to be migrated](intl.en-US/Migrate data from Qiniu Cloud-Object Storage (KODO) to OSS/Prerequisites.md#ul_ern_vcj_nfb).

5.  This step is optional. On the Performance tab, navigate to the **Flow Control** area and set the **Time Range** and the **Max Flow**, and then click **Add**.

    **Note:** To ensure business continuity, we recommend that you set the **Time Range** and **Max Flow** based on the fluctuation of visits.

6.  Click **Create**. Wait until a migration job is complete.

## FAQ {#section_xnl_fgw_wfb .section}

-   Why is the speed of a data migration lower than expected?
    1.  Check whether test domains of Qiniu FUSION are used. If you are using test domains, we recommend that you use accelerating domains to create migration jobs. Qiniu sets visit limits for a single IP and bandwidth restrictions when you use test domains.
    2.  Check the scenarios of domains. The applicable bandwidth changes based on the scenarios of Qiniu domains. For example, when you select websites as the scenario of a domain name, low bandwidth is allocated. When you specify the domain name as the endpoint of the source data address, the transmission speed for data migration is slow due to the low bandwidth. You must submit a ticket to Qiniu to change the scenario of the domain name.

