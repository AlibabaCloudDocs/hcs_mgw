# Create a migration job {#concept_vhs_qth_yfb .concept}

This topic describes the operations and considerations for data migration.

## Precautions {#section_lfs_tth_yfb .section}

When creating a migration job, you must note the following issue:

-   Symbolic link files that exist at the source data address are ignored during migration.

## Step 1: Create a source data address {#procedure_one .section}

1.  In the Create Data Address dialog box, configure the required settings and click **OK**. Settings are described in the following table.

    |Name|Required|Description|
    |:---|:-------|:----------|
    |**Data Type**|Yes|Select **NAS**.|
    |**Data Region**|Yes|Select the region where the NAS file system is located. If you select Alibaba Cloud in the NAS Type field, select the region where the NAS file system is located. If you select Others for in the NAS Type field, select the region that hosts the VPC on which the third-party NAS file system is attached.

 |
    |**Data Name**|Yes|A data name is 3 to 63 characters in length. Special characters are not supported, except for hyphens \(-\) and underscores \(\_\).|
    |**NAS Type**|Yes|Select the source of a NAS file system.     -   Alibaba Cloud: a file system that is created by using Alibaba Cloud NAS.
    -   Others: a file system that is created by using a third-party NAS service. You must mount the file system on an ECS instance that is located in a VPC.
 |
    |**File System**|Yes \(only for Alibaba Cloud NAS\)|Select the required NAS file system.|
    |**Mount Point**|Yes \(only for Alibaba Cloud NAS\)|Select the mount point of the destination NAS file system. **Note:** You can only mount a NAS file system on an ECS instance that is located in a VPC network \(classic network not supported\).

 |
    |**VPC**|Yes \(only for third-party NAS services\)|Select a VPC network where the third-party NAS file system is located.|
    |**Switches**|Yes \(only for third-party NAS services\)|Select a VSwitch that is owned by the VPC network.|
    |**NAS Address**|Yes \(only for third-party NAS services\)|Enter the IP address of the third-party NAS instance. This IP address must be accessible by the VPC.|
    |**Sub Folder**|Optional|Enter the directory to be migrated. If you leave this field blank, all data stored in the root directory \(/\) is migrated. **Note:** If you specify a directory, ensure that the directory exists in the NAS instance. Otherwise, the data address fails to be created.

 |
    |**Connection Method**|Yes \(only for third-party NAS services\)|Select a protocol type for NAS.|
    |**Connection Password**|Yes \(only for third-party NAS services\)|Select whether a password is required.     -   **No password**: No password is required to access the NAS service.
    -   **Use Password**: A username and password are required to access the NAS service. You must enter a valid username and password.
 |

    **Note:** For more information about the status of a new data address, see [Data address status](#).


## Step 2: Create a destination data address {#section_std_hkf_qfb .section}

1.  Choose **Data Online Migration** \> **Data Address** and click **Create Data Address**.
2.  In the Create Data Address dialog box, configure the required settings and click **OK**. Settings are described in the following table.

    |Name|Required|Description|
    |:---|:-------|:----------|
    |**Data Type**|Yes|Select **OSS**.|
    |**Data Region**|Yes|Select a region where the destination data address is located.|
    |**Data Name**|Yes|A data name is 3 to 63 characters in length. Special characters are not supported, except for hyphens \(-\) and underscores \(\_\).|
    |**OSS Endpoint**|Yes|Select an endpoint based on the region where data is located. For more information, see [Endpoints](../../../../../intl.en-US/Developer Guide/Endpoint/Regions and endpoints.md#).|
    |**AccessKey ID and AccessKey Secret**|Yes|Enter an AccessKey pair that is used to migrate data. For more information, see [Create an AccessKey pair](intl.en-US/Migrate data from NAS to OSS/Prerequisites.md#ak).|
    |**OSS Bucket**|Yes|Select a bucket to store migration data.|
    |**OSS Prefix**|No|An OSS prefix cannot start with a forward slash \(/\) and must end with a forward slash \(/\). For example: `data/to/oss/`. If you want to store data to the root directory of a bucket, you can leave the OSS Prefix field blank.|


## Step 3: Create a migration job {#section_ksy_xmy_pfb .section}

1.  In the Create Job dialog box, configure the required settings and click **Next**.

    Settings are described in the following table.

    |Name|Required|Description|
    |:---|:-------|:----------|
    |**Job Name**|Yes|A job name is 3 to 63 characters in length and can contain lowercase letters, digits, and hyphens \(-\). A job name cannot start or end with a hyphen \(-\).|
    |**Source Data Address**|Yes|Select the new source data address.|
    |**Destination Data Address**|Yes|Select the new destination data address. **Note:** You can [open a ticket](https://selfservice.console.aliyun.com) to apply for permissions to create a cross-country migration job. This occurs if the country where the source data address is located is different from the country where the destination data address is located. You must ensure that your business is legitimate, data transit conforms to local rules and regulations, and that the data does not include illegal information.

 |
    |**Migration Type**|Yes|     -   **Full**: You can specify the **Start Time Point of File**. ****If the last modification time of files is later than the specified start time, these files are migrated. After all of the files are migrated, a migration job is closed. When you repeat a full migration job, Data Transport only migrates files that have been changed.
    -   **Incremental**: You must specify the **Migration Interval** and **Migration Times** to perform an incremental migration job. You must specify the **Start Point Time of File**. ****If the last modification time of files is later than the specified start time, all the files are migrated during the first migration. After the first migration is complete, an incremental migration is performed based on the **Migration Interval**. An incremental migration job only migrates files that are created or modified between the time when the last migration job started and before the time when this migration starts. Assume that you specify N for the Migration Times. Full migration is performed once. Incremental migration will be performed \(N - 1\) times. For example, you can set the Migration Interval to 1 and Migration Times to 5. Additionally, you can set the **Start Time Point of File** to 2019/03/05 08:00. The present date and time is 2019/03/10 08:00. When the first migration starts, Data Transport migrates files that have the last modification time between 2019/03/05 08:00 and 2019/03/10 08:00. Assume that the first migration requires one hour to complete. The second migration starts at 2019/03/10 10:00, which is two hours later than 2019/03/10 08:00. The first migration takes one hour, and the other hour is consumed by the specified migration interval. During the second migration, if the last modification time of files is between 2019/03/10 08:00 and 2019/03/10 10:00, these files are migrated. The migration job includes a full migration and four incremental migrations.
    -   **Sync**: You can synchronize data from the source data address to the destination data address. A synchronization job continues to run based on the specified **Synchronization Interval** until you manually stop the job. When a synchronization job is performed for the first time, files are synchronized based on the **Start Time Point of File**. After the first synchronization is complete, files that are created or modified after the start time of the last synchronization will be synchronized when the specified synchronization interval ends. For example, the first synchronization is performed at 2018/11/01 08:00. For the second synchronization, files that are created or modified after 2018/11/01 08:00 are synchronized.
 **Note:** 

    -   You can select **Sync** when the source data address and the destination data address are located in the same region. When the source data address and the destination data address are not located in the same region, you cannot select this option.
    -   Before you start a full migration job or an incremental migration job, Data Transport compares files of the source data address with those of the destination data address. If a source file has the same name as a destination file, the destination file is overwritten when at least one of the following conditions is met:
        -   The source file has a later modification time.
        -   The size of the source file is different from that of the destination file.
 |
    |**Start Time Point of File**|Yes \(only for full and incremental migration\)|     -   All: All files are migrated.
    -   Assign: Files that are created or modified after the specified time are migrated. For example, when you set the Start Time Point of File to 2018/11/01 08:00:00, only files that are created or modified after 2018/11/10 08:00:00 are migrated. Files that are created or modified before the specified time will be disregarded.
 |
    |**Migration Interval**|Yes \(only for incremental migration\)|The default value is 1 hour and the maximum value is 24 hours.|
    |**Migration Times**|Yes \(only for incremental migration\)|The default value is 1 time and the maximum value is 30 times.|
    |**Start Time Point of File**|Yes \(only for synchronization\)|     -   All: All files are synchronized.
    -   Assign: Files that are created or modified after the specified time are synchronized. For example, when you set the Start Time Point of File to 2018/11/01 08:00:00, only files that are created or modified after 2018/11/01 08:00:00 are synchronized. The files that are created or modified before the specified time will be ignored.
 |
    |**Start Time of Job**|Yes \(only for synchronization\)|     -   Immediately: A synchronization job immediately runs after a migration job is complete.
    -   Schedule: You can set the scheduled time to synchronize data at the specified time.
 |
    |**Job Period**|Yes \(only for synchronization\)|The time interval between two synchronizations. A synchronization runs when the specified Job Period ends. Valid units: hour, day, and week.|
    |**The next synchronization runs until the last synchronization ends.**|Yes \(only for synchronization\)|Don't trigger a new task if another task is running. Assume that you set the Job Period to 1 Hour and this setting is not selected. The next synchronization runs regardless of whether the last synchronization job is completed within one hour. This setting is selected by default.|

2.  Click Next to open the Performance tab.
    -   If you select **Full** or **Incremental**, enter the **Data Size** and **File Count**.

        **Note:** To ensure a successful migration, you must estimate the amount of data to be migrated. For more information, see [Estimate the amount of data to be migrated](intl.en-US/Migrate data between Alibaba Cloud Object Storage Service (OSS) buckets/Prerequisites.md#section_wvw_v12_qfb).

    -   If you select **Sync**, enter the **Subtask File Count** and **Subtask File Size**.

        -   Subtask File Count: You can separate a migration job into multiple subtasks based on the number of files that you specify. You can run a maximum of 20 subtasks at a time. Set the appropriate number of files for each subtask to reduce the time of a migration job. The default value is 1000. Assume that you need to migrate 10,000 files. When you set the Subtask File Count to 500, the migration job is separated into 20 subtasks that run at the same time. When you set the Subtask File Count to 100, the migration job is separated into 100 subtasks. Each time 20 subtasks run and the remaining subtasks are queued.
        -   Subtask File Size: You can separate a migration job into multiple subtasks based on the number of files that you specify. You can run a maximum of 20 subtasks at a time. Set the appropriate size of files for each subtask to reduce the time of a migration job. The default value is 1 GB. Assume that you need to migrate files with a total size of 40 GB. When you set the Subtask File Size to 2 GB, the migration job is separated into 20 subtasks that run at the same time. When you set the Subtask File Size to 1 GB, the migration job is separated into 40 subtasks. You can run a maximum of 20 subtasks at a time and the remaining subtasks are queued.
        **Note:** A subtask is generated when either the specified Subtask File Count or Subtask File Size is met. When the specified Subtask File Count is reached, but the specified Subtask File Size is not, a subtask is generated based on the number of files. When the specified Subtask File Size is reached, but the specified Subtask File Count is not, a subtask is generated based on the file size. Assume that you set the Subtask File Count to 1000 and Subtask File Size to 1 GB. When the number of files reaches 1,000 but the file size does not reach 1 GB, a subtask is generated based on the number of files. When the file size reaches 1 GB but the number of files does not reach 1,000, a subtask is generated based on the file size.


## View the status of a data address {#status .section}

After you create the data address of an ECS instance, only one status for the data address of an ECS instance is displayed. The status can be one of the following:

-   Normal: indicates that a data address is created.
-   Creating: indicates that a data address is being created. This process of creating the first NAS data address requires about three minutes. If the status of a data address is Creating for a long period of time, you can click Refresh in the upper-right corner to update the status.
-   Invalid: indicates that an error occurred while creating a data address. You can verify that the configuration is accurate and Data Transport is allowed to access the shared files of an ECS instance. For further assistance, contact [Alibaba Cloud technical support](https://selfservice.console.aliyun.com/).

