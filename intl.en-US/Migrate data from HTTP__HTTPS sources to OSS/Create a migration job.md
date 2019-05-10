# Create a migration job {#concept_skk_b2l_qfb .concept}

This topic describes the operations and considerations for data migration.

## Precautions {#section_lfs_tth_yfb .section}

When creating a migration job, you need to note the following issues:

-   A migration job occupies the network resources of the source data address and destination data address. To ensure business continuity, we recommend that you specify a speed limit for a migration task or perform the migration task during off-peak hours.
-   Before a migration task is performed, files at both the source data address and the destination data address are checked. If files at the source data address have the same names as those at the destination data address and have a later update time than those at the destination data address, the files at the destination data address are overwritten during a migration task. If the content for two files is different, you must change the name of one of the files and back up these files.

## Step 1: Create a source data address {#procedure_one .section}

1.  Log on to the [Data Transport console](https://mgw.console.aliyun.com/#/job?_k=6w2hbo).
2.  Choose **Data Online Migration** \> **Data Address**, and then click **Create Data Address**.
3.  In the Create Data Address dialog box, set the required options and click **OK**. The options are described as follows:

    |Option|Required|Description|
    |:-----|:-------|:----------|
    |**Data Type**|Yes|Select **Http/Https**.|
    |**Data Name**|Yes|The data name can be 3 to 63 characters in length. Special characters are not supported, except for hyphens \(-\) and underscores \(\_\).|
    |**File Path**|Yes|Enter oss://\{bucket\}/\{the name of a list file\}. For more information, see [Create a list file](intl.en-US/Migrate data from HTTP__HTTPS sources to OSS/Prerequisites.md#ol_gtr_vzd_qfb).

 |
    |**List Access Endpoint**|Yes|Enter the appropriate endpoint based on the comparison table for regions and endpoints. For more information, see [Regions and endpoints](../../../../intl.en-US/Developer Guide/Endpoint/Regions and endpoints.md#table_z4s_lvy_5db).|
    |**List Access AK and List Access SK**|Yes|Enter an Accesskey that is used to migrate data.|

4.  You are required to apply for whitelist permissions because this feature is in the beta testing phase. Click **Application**.
5.  Enter the required information and submit the beta testing application for migration. After the application has been approved, you will receive an SMS notification.

## Step 2: Create a destination data address {#section_std_hkf_qfb .section}

1.  Select **Data Online Migration** \> **Data Address** and click **Create Data Address**.
2.  In the Create Data Adress dialog box, set the required options and click **OK**. The options are described as follows.

    |Option|Required|Description|
    |:-----|:-------|:----------|
    |**Data Type**|Yes|Select **OSS**.|
    |**Data Region**|Yes|Select a region where the destination data address is located.|
    |**Data Name**|Yes|The data name can be 3 to 63 characters in length. Special characters are not supported, except for hyphens \(-\) and underscores \(\_\).|
    |**OSS Endpoint**|Yes|Select an endpoint based on the region where data is located. For more information, see [Endpoints](../../../../intl.en-US/Developer Guide/Endpoint/Regions and endpoints.md#).|
    |**AccessKeyId and AccessKeySecret**|Yes|Enter an AccessKey to migrate data. For more information, see [Create an AccessKey](intl.en-US/Migrate data from HTTP__HTTPS sources to OSS/Prerequisites.md#ak).|
    |**OSS Bucket**|Yes|Select a bucket to store migration data.|
    |**OSS Prefix**|No|An OSS prefix cannot start with a forward slash \(/\) and must end with a forward slash \(/\). For example: `data/to/oss/`. If you want to store data to the root directory of a bucket, you can leave the OSS Prefix field blank.|


## Step 3: Create a migration job {#section_ksy_xmy_pfb .section}

1.  Select **Data Online Migration** \> **Migration Jobs** and click **Create Job**.
2.  In the Create Job dialog box, read the Terms of Migration Service, select **I understand the above terms and conditions, and activate Data Transport**, and then click **Next**.
3.  In the Create Job dialog box, set the required options and click **Next**.

    The options are described as follows:

    |Option|Required|Description|
    |:-----|:-------|:----------|
    |**Job Name**|Yes|The job name can 3 to 63 characters in length and can contain lowercase letters, numbers, and hyphens \(-\). A job name cannot start or end with a hyphen \(-\).|
    |**Source Data Address**|Yes|Select the created source data address.|
    |**Destination Data Address**|Yes|Select a destination data address that you have created.|
    |**Migration Type**|Yes|Before you start a migration job, Data Transport compares files of the source data address with those of the destination data address. The files at the source data address are disregarded during migration. This occurs if the source files with an earlier update time have the same name, ContentType, and size as the destination files. However, all the other files are migrated.     -   **Full**: performs a full migration. After all of the files are migrated, a migration job is closed. When you perform a full migration job again, the migration service only migrates files that have been changed after the last full migration job.
 |

4.  On the Performance tab, navigate to the **Data Prediction** section, and enter the **Data Size** and **File Count**.

    **Note:** To ensure a successful migration, you must estimate the amount of data to be migrated. For more information, see [Estimate the amount of data to be migrated](intl.en-US/Migrate data from HTTP__HTTPS sources to OSS/Prerequisites.md#ul_a5j_gln_qfb).

5.  This step is optional. On the Performance tab, navigate to the **Flow Control** section, set the **Time Range** and **Max Flow**, and then click **Add**.

    **Note:** To ensure business continuity, we recommend that you set the **Time Range** and **Max Flow** based on the fluctuation of visits. The default value of the **Time Range** is 06:00 - 12:00. The default value of the **Max Flow** is 5 MB/s.

6.  Click **Create**. Wait until a migration job is complete.

