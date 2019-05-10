# Create a migration job {#concept_ryz_pjj_yfb .concept}

This topic describes the operations and considerations for data migration.

## Precautions {#section_lfs_tth_yfb .section}

When creating a migration job, you need to note the following issues:

-   A migration job occupies the network resources of the source data address and the destination data address. To ensure business continuity, we recommend that you specify a speed limit for a migration job or perform the migration job during off-peak hours.
-   Before a migration job is performed, files at both the source data address and the destination data address are checked. The files at the destination data address are overwritten during a migration job. This occurs if the source files have the same name as the destination files, but have a later update time. If two files have the same name but different content, you must change the name of one file or back up the files.
-   Symbolic link files that exist at the source data address are disregarded during migration.

## Step 1: Create a source data address {#one .section}

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
    -   Others: a file system that is created in a third-party NAS service. You must mount the file system on an ECS instance that is located in a VPC.
 |
    |**File System**|Yes \(only for Alibaba Cloud\)|Select the required NAS file system.|
    |**Mount Point**|Yes \(only for Alibaba Cloud\)|Select the required NAS mount point. **Note:** You can only mount a NAS file system on an ECS instance that is located in a VPC network \(classic network not supported\).

 |
    |**VPC**|Yes \(only for Others\)|Select a VPC network where the third-party NAS file system is located.|
    |**Switches**|Yes \(only for Others\)|Select a VSwitch that is owned by the VPC network.|
    |**NAS Address**|Yes \(only for Others\)|Enter the private IP address of the third-party NAS instance that is located in the VPC network.|
    |**Sub Folder**|Optional|Enter the directory to be migrated. If you leave this field blank, all data in the root directory \(/\) is migrated. **Note:** If you enter the Sub Folder, ensure that the directory exists in the NAS instance. Otherwise, the data address fails to be created.

 |
    |**Connection Method**|Yes \(only for Others\)|Select a protocol type for NAS.|
    |**Connection Password**|Yes \(only for Others\)|Select whether a password is required.     -   **No password**: If no password is required to access the NAS service, select no password.
    -   **Use Password**: Enter the required username and password. You must use the username and password to access the shared folder.
 |

    **Note:** For more information about the status of a new data address, see [Data address status](#).

4.  You must apply for whitelist permissions because this feature is still in the beta testing phase. Click **Application**.
5.  Enter the required information and submit the beta testing application for migration. After the application has been approved, you will receive an SMS notification.

## Step 2: Create a destination data address {#section_jzz_pjj_yfb .section}

-   If the source NAS data address and the destination NAS data address are located in the same VPC, see [Step 1: Create a source data address](#).
-   If the source NAS data address and the destination NAS data address are located in two different VPCs, you must follow these steps to create a destination data address:
    -   **Data Type**: Select **NAS**.
    -   **Data Region**: Select the same region as that of the source NAS data address.
    -   **NAS Type**: Select **Others**.
    -   **VPC**: Select the same VPC as that of the source NAS data address.
    -   **Switches**: Select the same VSwitch as that of the source NAS data address.
    -   **NAS Address**: If the type of the destination NAS data address is Alibaba Cloud, enter the mount point of the data address. If the type of the destination NAS data address is Others, enter the private IP address of the data address that is located in a VPC.
    -   For the remaining options, conform to the rules that are used to create a source data address.

## Step 3: Create a migration job {#section_ksy_xmy_pfb .section}

1.  Select **Data Online Migration** \> **Migration Jobs** and click **Create Job**.
2.  In the Create Job dialog box, read the Terms of Data Transport, select **I understand the above terms and conditions, activate Data Transport**, and then click **Next**.
3.  In the Create Job dialog box, set the required options and click **Next**.

    The options are described as follows:

    |Option|Required|Description|
    |:-----|:-------|:----------|
    |**Job Name**|Yes|The job name can be 3 to 63 characters in length and can contain lowercase letters, numbers, and hyphens \(-\). A job name cannot start or end with a hyphen \(-\).|
    |**Source Data Address**|Yes|Select the created source data address.|
    |**Destination Data Address**|Yes|Select the created destination data address. **Note:** You can [open a ticket](https://selfservice.console.aliyun.com) to apply for the permission of creating a cross-country migration job. This occurs if the country where the source data address is located is different from the country where the destination data address is located. You must ensure that your business is legitimate, data does not include illegal information, and data transit conforms to local rules and regulations.

 |
    |**Migration Type**|Yes|Before you start a migration job, Data Transport compares files of the source data address with those of the destination data address. The files at the source data address and destination data address, which have the same name, ContentType, size, and latest update time are disregarded during migration. However, the other files are migrated.     -   **Full**: You can specify the **Start Time Point of File**. Files whose last modified time is later than the specified start time are migrated. After all of the files are migrated, a migration job is closed.**** When you repeat a full migration job, Data Transport only migrates files that have been changed.
    -   **Incremental**: You must specify the **Migration Interval** and **Migration Times** to perform an incremental migration job. You must specify the **Start Point Time of File**. Files whose last modified time is later than the specified start time are migrated for the first time.**** After the first migration job is complete, an incremental migration job is performed based on the **Migration Interval**. A migration interval involves sending files that are created or modified within a specific range from the source data address to the destination data address. The specified range is the time when the last migration job started and before the time when this migration starts. Assume that you specify N for the Migration Times. A full migration is performed once. In the future, an incremental migration will be performed \(N - 1\) times. For example, you set the Migration Interval to 1 and Migration Times to 5. Additionally, you set the **Start Time Point of File** to 2019/03/05 08:00:00. The present time is at 2019/03/10 08:00. When you perform a migration job for the first time, Data Transport migrates files whose last modified time is between 2019/03/05 08:00 and 2019/03/10 08:00. Assume that the first migration job requires one hour to complete. The second migration job starts at 2019/03/10 10:00, which is two hours later than 2019/03/10 08:00. The migration job takes one hour, and the other hour is consumed by the specified migration interval. When you perform the second migration job, files whose last modified time is between 2019/03/10 08:00 and 2019/03/10 10:00 are migrated. The migration job includes a full migration and four incremental migrations.
    -   **Sync**: You can synchronize data from the source data address to the destination data address. A synchronization job continuously runs based on the specified **Synchronization Interval** until you manually stop the job. When you perform a synchronization for the first time, files are synchronized based on the **Start Time Point of File**. After the first synchronization is complete, files that are created or modified after the start time of the last synchronization will be synchronized after the specified synchronization interval. For example, you perform the first synchronization at 2018/11/01 08:00. For the second synchronization, files that are created or modified after 2018/11/01 08:00 are synchronized.

**Note:** You can select **Sync** when the source data address and the destination data address are located in the same region. When the source data address and the destination data address are not located in the same region, you cannot select this option.

 |
    |**Start Time Point of File**|Yes \(only for Full and Incremental\)|     -   All: All files are migrated.
    -   Assign: Files that are created or modified after the specified time are migrated. For example, when you set the Start Time Point of File to 2018/11/01 08:00:00, only files that are created or modified after 2018/11/01 08:00:00 are migrated. The files that are created or modified before the specified time will be disregarded.
 |
    |**Migration Interval**|Yes \(only for Incremental migration\)|The default value is 1 Hour and the maximum value is 24 Hours.|
    |**Migration Times**|Yes \(only for Incremental migration\)|The default value is 1 time and the maximum value is 30 times.|
    |**Start Time Point of File**|Yes \(only for Sync\)|     -   All: All files are synchronized.
    -   Assign: Files that are created or modified after the specified time are synchronized. For example, when you set the Start Time Point of File to 2018/11/01 08:00:00, only files that are created or modified after 2018/11/01 08:00:00 are synchronized. The files that are created or modified before the specified time will be disregarded.
 |
    |**Start Time of Job**|Yes \(only for Sync\)|     -   Immediately: A synchronization immediately runs after a migration job is complete.
    -   Schedule: You can set the scheduled time and synchronize data at the specified time.
 |
    |**Job Period**|Yes \(only for Sync\)|The time interval between two synchronizations. A synchronization runs whenever a job period ends. Valid units: hour, day, and week.|
    |**The next synchronization runs until the last synchronization ends.**|Yes \(only for Sync\)|Don't trigger a new task if another task is running. Assume that you set the Job Period to 1 Hour and you forgot to select this option. The next synchronization runs regardless of whether the last synchronizion is completed within one hour. This option is selected by default.|

4.  Click Next to enter the Performance tab.
    -   When you select **Full** or **Incremental**, enter the **Data Size** and the **File Count**.

        **Note:** To ensure a successful migration, you must accurately estimate the amount of data to be migrated. For more information, see [Estimate the amount of data to be migrated](intl.en-US/Migrate data between Alibaba Cloud Object Storage Service (OSS) buckets/Prerequisites.md#section_wvw_v12_qfb).

    -   When you select **Sync**, enter the **Subtask File Count** and **Subtask File Size**.

        -   Subtask File Count: You can separate a migration job into multiple subtasks based on the number of files that you specify. You can run a maximum of 20 subtasks at a time. Set the appropriate number of files for each subtask to reduce the time of a migration job. The default value is 1000. Assume that you need to migrate 10,000 files. When you set the Subtask File Count to 500, the migration job is separated into 20 subtasks that run at the same time. When you set the Subtask File Count to 100, the migration job is separated into 100 subtasks. Each time 20 subtasks run and the remaining subtasks are queued.
        -   Subtask File Size: You can separate a migration job into multiple subtasks based on the number of files that you specify. You can run a maximum of 20 subtasks at a time. Set the appropriate size of files for each subtask to reduce the time of a migration job. The default value is 1 GB. Assume that you need to migrate files with a total size of 40 GB. When you set the Subtask File Size to 2 GB, the migration job is separated into 20 subtasks that run at the same time. When you set the Subtask File Size to 1 GB, the migration job is separated into 40 subtasks. Each time 20 subtasks run and the remaining subtasks are queued.
        **Note:** A subtask is generated when either the specified Subtask File Count or Subtask File Size is met. When the number of files reaches the specified Subtask File Count but the file size does not reach the specified Subtask File Size, a subtask is generated based on the number of files. When the file size reaches the specified Subtask File Size but the number of files does not reach the specified Subtask File Count, a subtask is generated based on the file size. Assume that you set the Subtask File Count to 1000 and Subtask File Size to 1 GB. When the number of files reaches 1,000 but the file size does not reach 1 GB, a subtask is generated based on the number of files. When the file size reaches 1 GB but the number of files does not reach 1,000, a subtask is generated based on the file size.

5.  This step is optional. On the Performance tab, navigate to the **Flow Control** section, set the **Time Range** and **Max Flow**, and then click **Add**.

    **Note:** To ensure business continuity, we recommend that you set the **Time Range** and **Max Flow** based on the fluctuation of visits. The default value of the **Time Range** is 06:00 - 12:00. The default value of the **Max Flow** is 5 MB/s.

6.  Click **Create**. Wait until a migration job is complete.

## View the status of a data address {#status .section}

After you create the data address of an ECS instance, only one status for the data address of an ECS instance is displayed. The status can be one of the following:

-   Normal: indicates that a data address is created.
-   Creating: requires about three minutes to create the first NAS data address. This process takes a while. If the status of a data address is in the Creating state for a long time, you can click Refresh in the upper-right corner to update the status.
-   Invalid: an error occurred while creating a data address. You can verify that the configuration is correct and Data Transport is allowed to access the shared files of an ECS instance. If this issue persists, you can contact [Alibaba Cloud technical support](https://selfservice.console.aliyun.com/).

