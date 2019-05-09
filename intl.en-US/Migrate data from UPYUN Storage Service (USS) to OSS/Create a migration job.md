# Create a migration job {#concept_yr2_3cf_qfb .concept}

This section describes the operations and considerations for data migration.

## Precautions {#section_lfs_tth_yfb .section}

When creating a migration job, you must note the following issues:

-   A migration job occupies the network resources of the source data address and the destination data address. To ensure business continuity, we recommend that you specify a speed limit for a migration job or perform the migration job during off-peak hours.
-   Before a migration job is performed, files at both the source data address and the destination data address are checked. The files at the destination data address are overwritten during a migration job. This occurs if the source files have the same name as the destination files, but have a later update time. If two files have the same name but different content, you must change the name of one file or back up the files.
-   With Data Transport, you can only migrate data of a single bucket rather than all data in an account at a time.

## Step 1: Create a source data address {#section_ctl_xyd_qfb .section}

1.  Log on to the [Data Transport console](https://mgw.console.aliyun.com/#/job?_k=6w2hbo).
2.  Choose **Data Online Migration** \> **Data Address**, and then click **Create Data Address**.
3.  In the Create Data Address dialog box, set the required options and click **OK**. The options are described as follows:

    |Option|Required|Description|
    |:-----|:-------|:----------|
    |**Data Type**|Yes|Select **YOU PAI**.|
    |**Data Name**|Yes|The data name can be 3 to 63 characters in length. Special characters are not supported, except for hyphens \(-\) and underscores \(\_\).|
    |**Domain Address**|Yes|An endpoint of an OSS SDK or endpoint of a RESTful API.You can use one of the following endpoints:

    -   `http://v0.api.upyun.com` \(recommended\)
    -   `http://v1.api.upyun.com` \(China Telecom\)
    -   `http://v2.api.upyun.com` \(China Unicom\)
    -   `http://v3.api.upyun.com` \(China Tietong\)

**Note:** UPYUN sets flow limits when you migrate large amounts of data. When you migrate large amounts of data, we recommend that you contact UPYUN customer service to remove flow limits. Otherwise, you can set the **Use CDN** option to migrate data using a CDN.

|
    |**Use CDN**|No|Select whether you can use a CDN to migrate data.     -   **Not Use**: Without using a CDN, you can migrate data from the address that you enter in the **Endpoint** field.
    -   **Use**: Enter the required name of a CDN domain. You can migrate data from the CDN domain.

**Note:** With a CDN, you can increase the migration speed and diminish the impact of flow limits that are set by UPYUN.

|
    |**Service Name**|Yes|You can log on to the UPYUN console to view the service name on the [Cloud Storage Service](https://console.upyun.com/services/file/) page.|
    |**Migration Folder**|Yes|     -   **Migrate all data**: indicates that all data in a bucket is migrated.

When you migrate all data, you do not need to enter a migration folder.

    -   **Migrate partial data**: indicates that files in a specified folder or prefix are migrated. An OSS prefix cannot start with a forward slash \(/\) and must end with a forward slash \(/\). For example, `docs/`.
 |
    |**Operator Name** and **Operator Secret**|Yes|     1.  Log on to the YOU PAI console, select the [Accounts](https://console.upyun.com/account/operators/?spm=a2c4e.11153940.blogcont644709.68.330a29f4cGJrIS) tab, and click **Create Operator** to create an operator for a migration job. The new operator is granted the **Read** permission.
    2.  On the Cloud Storage tab, click the name of a migration job to open the Configuration page. Select **Content**, and click **Authorize** to select the required operator.
 |

4.  You must apply for whitelist permissions because this feature is still in the beta testing phase. Click **Application**.
5.  Enter the required information and submit the beta testing application for migration. After the application has been approved, you will receive an SMS notification.

## Step 2: Create a destination data address {#section_grt_fzd_qfb .section}

1.  Select **Data Online Migration** \> **Data Address** and click **Create Data Address**.
2.  In the Create Data Adress dialog box, set the required options and click **OK**. The options are described as follows:

    |Option|Required|Description|
    |:-----|:-------|:----------|
    |**Type**|Yes|Select **OSS**.|
    |**Data Region**|Yes|Select a region where the destination data address is located.|
    |**Data Name**|Yes|The data name can be 3 to 63 characters in length. Special characters are not supported, except for hyphens \(-\) and underscores \(\_\).|
    |**OSS Endpoint**|Yes|Select an endpoint based on the region where data is located. For more information, see [Endpoints](../../../../intl.en-US/Developer Guide/Endpoint/Regions and endpoints.md#).|
    |**AccessKeyId and AccessKeySecret**|Yes|Enter an AccessKey that is used to migrate data. For more information, see [Create an AccessKey](intl.en-US/Migrate data from HTTP__HTTPS sources to OSS/Prerequisites.md#ak).|
    |**OSS Bucket**|Yes|Select a bucket to store migration data.|
    |**OSS Prefix**|No|An OSS prefix cannot start with a forward slash \(/\) and must end with a forward slash \(/\). For example: `data/to/oss/`. If you want to store data to the root directory of a bucket, you can leave the OSS Prefix field blank.|


## Step 3: Create a migration job {#section_ksy_xmy_pfb .section}

1.  Select **Data Online Migration** \> **Migration Jobs** and click **Create Job**.
2.  In the Create Job dialog box, read the Terms of Data Transport, select **I understand the above terms and conditions, activate Data Transport**, and then click **Next**.
3.  In the Create Job dialog box, set the required options and click **Next**.

    The options are described as follows:

    |Option|Required|Description|
    |:-----|:-------|:----------|
    |**Job Name**|Yes|The job name can be 3 to 63 characters in length and contain lowercase letters, numbers, and hyphens \(-\). A job name cannot start or end with a hyphen \(-\).|
    |**Source Data Address**|Yes|Select the new source data address.|
    |**Destination Data Address**|Yes|Select the new destination data address.**Note:** You can [open a ticket](https://selfservice.console.aliyun.com) to apply for the permission of creating a cross-country migration job. This occurs if the country where the source data address is located is different from the country where the destination data address is located. You must ensure that your business is legitimate, data transit conforms to local rules and regulations, and that the data does not include illegal information.

|
    |**Migration Type**|Yes|Before you start a migration job, Data Transport compares files of the source data address with those of the destination data address. The files at the source data address are disregarded during migration. This occurs if the source files with an earlier update time have the same name, ContentType, and size as the destination files. However, all the other files are migrated.    -   **Full**: You can specify the **Start Time Point of File**. The files whose last modification time is later than the specified start time are migrated. After all of the files are migrated, a migration job is closed.**** When you perform a full migration job again, Data Transport only migrates files that have been changed.
    -   **Incremental**: You must specify the **Migration Interval** and **Migration Times** to perform an incremental migration job. You must specify the **Start Point Time of File**. The files whose last modification time is later than the specified start time are migrated for the first time.**** After the first migration job is complete, an incremental migration job is performed based on the **Migration Interval**. A migration interval involves sending files that are created or modified within a specific range from the source data address to the destination data address. The specified range is the time when the last migration job started and before the time when this migration starts. Assume that you specify N for the Migration Times. A full migration is performed once. In the future, an incremental migration will be performed \(N - 1\) times. For example, you set the Migration Interval to 1 and Migration Times to 5. Additionally, you set the **Start Time Point of File** to 2019/03/05 08:00:00. The present date and time is 2019/03/10 08:00. When you perform a migration job for the first time, Data Transport migrates files whose last modification time is between 2019/03/05 08:00 and 2019/03/10 08:00. Assume that the first migration job requires one hour to complete. The second migration job starts at 2019/03/10 10:00, which is two hours later than 2019/03/10 08:00, the migration job takes one hour, the other hour is consumed by the specified migration interval. When you perform the second migration job, files whose last modification time is between 2019/03/10 08:00 and 2019/03/10 10:00 are migrated. The migration job includes a full migration and four incremental migrations.
|
    |**Start Time Point of File**|Yes|     -   All: All files are migrated.
    -   Assign: Files that are created or modified after the specified time are migrated. For example, when you set the Start Time Point of File to 2018/11/01 08:00:00, only files that are created or modified after 2018/11/01 08:00:00 are migrated. Files that are created or modified before the specified time will be disregarded.
 |
    |**Migration Interval**|Yes \(only for Incremental migration\)|The default value is 1 Hour and the maximum value is 24 Hours.|
    |**Migration Times**|Yes \(only for Incremental migration\)|The default value is 1 time and the maximum value is 30 times.|

4.  On the Performance tab, navigate to the **Data Prediction** section, and enter the **Data Size** and **File Count**.

    **Note:** To ensure a successful migration, you must estimate the amount of data to be migrated. For more information, see [Estimate the amount of data to be migrated](intl.en-US/Migrate data from UPYUN Storage Service (USS) to OSS/Prerequisites.md#section_z1p_1df_qfb).

5.  This step is optional. On the Performance tab, navigate to the **Flow Control** area, set the **Time Range** and **Max Flow**, and then click **Add**.

    **Note:** To ensure business continuity, we recommend that you set the **Time Range** and **Max Flow** based on the fluctuation of visits. The default value of the **Time Range** is 06:00 - 12:00. The default value of the **Max Flow** is 5 MB/s.

6.  Click **Create**. Wait until a migration job is complete.
