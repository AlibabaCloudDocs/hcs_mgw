# Create a migration job {#concept_vhs_qth_yfb .concept}

This topic describes how to migrate data.

## Precautions {#section_lfs_tth_yfb .section}

When creating a migration job, you must note the following issues:

-   A migration job occupies the network resources of the source data address and the destination data address. To ensure business continuity, we recommend that you specify a speed limit for a migration job or perform the migration job during off-peak hours.
-   Before a migration job is performed, files at both the source data address and the destination data address are checked. The files at the destination data address are overwritten during a migration job. This occurs if the source files have the same name as the destination files, but have a later update time. If two files have the same name but different content, you must change the name of either one file and back up the files.
-   Symbolic link files that exist at the source data address are disregarded during migration.

## Step 1: Create a source data address {#procedure_one .section}

1.  Log on to the [Data Transport console](https://mgw.console.aliyun.com/#/job?_k=6w2hbo).
2.  Choose **Data Online Migration** \> **Data Address**, and then click **Create Data Address**.
3.  In the Create Data Address dialog box, set the required options and click **OK**. The options are described as follows:

    |Option|Required|Description|
    |:-----|:-------|:----------|
    |**Data Type**|Yes|Select **NAS**.|
    |**Data Region**|Yes|Select the region where the NAS file system is located. If you select Alibaba Cloud for the NAS Type, select the region where the NAS file system is located. If you select Others for the NAS Type, select the region that hosts the VPC where a third-party NAS file system is located.

 |
    |**Data Name**|Yes|The data name can be 3 to 63 characters in length. Special characters are not supported, except for hyphens \(-\) and underscores \(\_\).|
    |**NAS Type**|Yes|Select the required source of a NAS file system.     -   Alibaba Cloud: a file system that is created in NAS.
    -   Others: a file system that is created in a third-party NAS service. You need to mount the file system on an ECS instance that is located in an Alibaba Cloud VPC network.
 |
    |**File System**|Yes \(only for Alibaba Cloud\)|Select the required NAS file system.|
    |**Mount Point**|Yes \(only for Alibaba Cloud\)|Select the required NAS mount point. **Note:** Currently, you can only mount a NAS file system on an ECS instance that is located in a VPC network \(classic network not supported\).

 |
    |**VPC**|Yes \(only for Others\)|Select a VPC network where the third-party NAS file system is located.|
    |**Switches**|Yes \(only for Others\)|Select a VSwitch that is owned by the VPC network.|
    |**NAS Address**|Yes \(only for Others\)|Enter the IP address of the third-party NAS instance. This IP address must be accessible to the VPC.|
    |**Sub Folder**|Optional|Enter the directory to be migrated. If you leave this field blank, all data in the root directory \(/\) is migrated. **Note:** If you enter the Sub Folder, ensure that the directory exists in the NAS instance. Otherwise, the data address fails to be created.

 |
    |**Connection Method**|Yes \(only for Others\)|Select a protocol type for NAS.|
    |**Connection Password**|Yes \(only for Others\)|Select whether a password is required.     -   **No password**: If no password is required to access the NAS service, select no password.
    -   **Use Password**: Enter the required username and password. You must use the username and password to access the shared folder.
 |

    **Note:** For more information about the status of a new data address, see [Data address status](#).

4.  You must apply for whitelist permissions because this feature is in the beta testing phase. Click **Application**.
5.  Enter the required information and submit the beta testing application for migration. After the application has been approved, you will receive an SMS notification.

## Step 2: Create a destination data address {#section_std_hkf_qfb .section}

1.  Select **Data Online Migration** \> **Data Address** and click **Create Data Address**.
2.  In the Create Data Adress dialog box, set the required options and click **OK**. The options are described as follows:

    |Option|Required|Description|
    |:-----|:-------|:----------|
    |**Data Type**|Yes|Select **OSS**.|
    |**Data Region**|Yes|Select a region where the destination data address is located.|
    |**Data Name**|Yes|The data name can be 3 to 63 characters in length. Special characters are not supported, except for hyphens \(-\) and underscores \(\_\).|
    |**OSS Endpoint**|Yes|Select an endpoint based on the region where data is located. For more information, see [Endpoints](../../../../intl.en-US/Developer Guide/Endpoint/Regions and endpoints.md#).|
    |**AccessKeyId and AccessKeySecret**|Yes|Enter an AccessKey that is used to migrate data. For more information, see [Create an AccessKey](intl.en-US/Migrate data from NAS to OSS/Prerequisites.md#ak).|
    |**OSS Bucket**|Yes|Select a bucket to store migration data.|
    |**OSS Prefix**|No|An OSS prefix cannot start with a forward slash \(/\) and must end with a forward slash \(/\). For example: `data/to/oss/`. If you want to store data to the root directory of a bucket, you can leave the OSS Prefix field blank.|


## Step 3: Create a migration job {#section_ksy_xmy_pfb .section}

1.  Select **Data Online Migration** \> **Migration Jobs** and click **Create Job**.
2.  In the Create Job dialog box, read the Terms of Data Transport, select **I understand the above terms and conditions, activate Data Transport**, and then click **Next**.
3.  In the Create Job dialog box, set the required options and click **Next**.

    The options are described as follows:

    |Option|Required|Description|
    |:-----|:-------|:----------|
    |**Job Name**|Yes|The job name can be 3 to 63 characters in length and can contain lowercase letters, numbers, and hyphens \(-\). A job name cannot start or end with a hyphen \(-\).|
    |**Source Data Address**|Yes|Select the new source data address.|
    |**Destination Data Address**|Yes|Select the new destination data address. **Note:** You can [open a ticket](https://selfservice.console.aliyun.com) to apply for the permission of creating a cross-country migration job. If the country where the source data address is located is different from the country where the destination data address is located You must ensure that your business is legitimate, data does not include illegal information, and data transit conforms to local rules and regulations.

 |
    |**Migration Type**|Yes|Before you start a migration job, Data Transport compares files of the source data address with those of the destination data address. The files at the source data address are disregarded during migration. This occurs if the source files with an earlier update time have the same name as the destination files. However, all the other files are migrated.     -   **Full**: You can specify the **Start Time Point of File**. Files whose last modified time is later than the specified start time are migrated. After all of the files are migrated, a migration job is closed.**** When you perform a full migration job again, Data Transport only migrates files that have been changed after the last full migration job.
    -   **Incremental**: You must specify the **Migration Interval** and **Migration Times** to perform an incremental migration job. You must specify the **Start Point Time of File**. Files whose last modified time is later than the specified start time are migrated for the first time.**** After the first migration job is complete, an incremental migration job is performed based on the **Migration Interval**. At the source data address, files that are created or modified between the time when the last migration job started and before the time when this migration starts will be migrated to the destination data address. Assume that you specify N for the Migration Times. A full migration is performed once. In the future, an incremental migration will be performed \(N - 1\) times. For example, you set the Migration Interval to 1 and Migration Times to 5. Additionally, you set the **Start Time Point of File** to 2019/03/05 08:00:00. The present date and time is 2019/03/10 08:00. When you perform a migration job for the first time, Data Transport migrates files whose last modified time is between 2019/03/05 08:00 and 2019/03/10 08:00. Assume that the first migration job requires one hour to complete. The second migration job starts at 2019/03/10 10:00, which is two hours later than 2019/03/10 08:00. The migration job takes one hour, and the other hour is consumed by the specified migration interval. When you perform the second migration job, only files whose last modified time is between 2019/03/10 08:00 and 2019/03/10 10:00 are migrated. The migration job includes a full migration and four incremental migrations.
    -   **Sync**: You can synchronize data from the source data address to the destination data address. A synchronization job continuously runs based on the specified **Synchronization Interval** until you manually stop the job. When you perform a synchronization for the first time, files are synchronized based on the **Start Time Point of File**. After the first synchronization is completed, files that are created or modified after the start time of the last synchronization will be synchronized after the specified synchronization interval. Fox example, you perform the first synchronization at 2018/11/01 08:00. For the second synchronization, files that are created or modified after 2018/11/01 08:00 are synchronized.

**Note:** You can select **Sync** when the source data address and the destination data address are located in the same region. You cannot select this option when the source data address and the destination data address are not located in the same region.

 |
    |**Start Time Point of File**|Yes \(only for Full and Incremental\)|     -   All: All files are migrated.
    -   Assign: Files that are created or modified after the specified time are migrated. For example, when you set the Start Time Point of File to 2018/11/01 08:00:00, only files that are created or modified after 2018/11/01 08:00:00 are migrated. The files that are created or modified before the specified time will be disregarded.
 |
    |**Migration Interval**|Yes \(only for Incremental migration\)|The default value is 1 Hour and the maximum value is 24 Hours.|
    |**Migration Times**|Yes \(only for Incremental migration\)|The default value is 1 time and the maximum value is 30 times.|
    |**Start Time Point of File**|Yes \(only for Sync\)|     -   All: All files are synchronized.
    -   Assign: Files that are created or modified after the specified time are synchronized. For example, when you set the Start Time Point of File to 2018/11/01 08:00:00, only files that are created or modified after 2018/11/01 08:00:00 are synchronized. Files that are created or modified before the specified time will be skipped.
 |
    |**Start Time of Job**|Yes \(only for Sync\)|     -   Immediately: A synchronization immediately runs after a migration job is complete.
    -   Schedule: You can set the scheduled time and synchronize data at the specified time.
 |
    |**Job Period**|Yes \(only for Sync\)|The time interval between two synchronizations. A synchronization runs whenever a job period ends. Valid units: hour, day, and week.|
    |**The next synchronization runs until the last synchronization ends.**|Yes \(only for Sync\)|Don't trigger a new task if another task is running. Assume that you set the Job Period to 1 Hour and you forgot to select this option. The next synchronization runs regardless of whether the last synchronization is completed within one hour. This option is selected by default.|

4.  Click Next to open the Performance tab.
    -   When you select **Full** or **Incremental**, enter the **Data Size** and the **File Count**.

        **Note:** To ensure a successful migration, you must accurately estimate the amount of data to be migrated. For more information, see [Estimate the amount of data to be migrated](intl.en-US/Migrate data between Alibaba Cloud Object Storage Service (OSS) buckets/Prerequisites.md#section_wvw_v12_qfb).

    -   When you select **Sync**, enter the**Subtask File Count** and **Subtask File Size**.

        -   Subtask File Count: You can separate a migration job into multiple subtasks based on the number of files that you specify. You can simultaneously run a maximum of 20 subtasks. Set an appropriate number of files for each subtask to reduce the time for a migration job. The default value is 1000. Assume that you need to migrate 10,000 files. When you set the Subtask File Count to 500, the migration job is separated into 20 simultaneous subtasks that run. When you set the Subtask File Count to 100, the migration job is separated into 100 subtasks. Each time 20 subtasks run and the remaining subtasks are queued.
        -   Subtask File Size: You can separate a migration job into multiple subtasks based on the number of files that you specify. You can simultaneously run a maximum of 20 subtasks. Set the appropriate size of files for each subtask to reduce the time of a migration job. The default value is 1 GB. Assume that you need to migrate 40 GB of files. When you set the Subtask File Size to 2 GB, the migration job is separated into 20 simultaneous subtasks. When you set the Subtask File Size to 1 GB, the migration job is separated to 40 subtasks. Each time 20 simultaneous subtasks are run and the other subtasks are spooled.
        **Note:** A subtask is generated when either the specified Subtask File Count or Subtask File Size is met. When the number of files reaches the specified Subtask File Count but the file size does not reach the specified Subtask File Size, a subtask is generated based on the number of files. When the file size reaches the specified Subtask File Size but the number of files does not reach the specified Subtask File Count, a subtask is generated based on the file size. Assume that you set the Subtask File Count to 1,000 and Subtask File Size to 1 GB. When the number of files reaches 1,000 but the file size does not reach 1 GB, a subtask is generated based on the number of files. When the file size reaches 1 GB but the number of files does not reach 1,000, a subtask is generated based on the file size.

5.  This step is optional. On the Performance tab, navigate to the **Flow Control** area and set the **Time Range** and **Max Flow**, and then click **Add**.

    **Note:** To ensure business continuity, we recommend that you set the **Time Range** and **Max Flow** based on the fluctuation of visits.

6.  Click **Create**. Wait until a migration job is completed.

## View the status of a data address {#status .section}

After you create a NAS data address, only one data address status is displayed. The status can be one of the following:

-   Normal: A data address is created.
-   Creating: It takes time to create the first data address of an ECS instance \(about three minutes\). If the status of a data address is in the Creating state for a long time, you can click Refresh in the upper-right corner to update the status.
-   Invalid: An error occurred while creating a data address. You can verify that the configuration is correct and Data Transport is allowed to access the shared files of an ECS instance. If this issue persists, you can contact [Alibaba Cloud technical support](https://selfservice.console.aliyun.com/).

