# Create a migration job {#concept_ryz_pjj_yfb .concept}

This topic describes the operations and considerations for data migration.

## Precautions {#section_lfs_tth_yfb .section}

When creating a migration job, you need to note the following issues:

-   A migration job occupies the network resources of the source data address and destination data address. To ensure business continuity, we recommend that you specify a speed limit for a migration job or perform the migration job during off-peak hours.
-   Before a migration job is performed, files at both the source data address and the destination data address are checked. The files at the destination data address are overwritten during a migration job. This occurs if the source files have the same name as the destination files, but have a later update time. If two files have the same name but different content, you must change the name of one file or back up the files.

## Step 1: Create a source data address {#one .section}

1.  Log on to the [Data Transport console](https://mgw.console.aliyun.com/#/job?_k=6w2hbo).
2.  Choose **Data Online Migration** \> **Data Address**, and then click **Create Data Address**.
3.  In the Create Data Address dialog box, set the required options and click **OK**. The options are described as follows:

    |Option|Required|Description|
    |:-----|:-------|:----------|
    |**Data type**|Yes|Select **NAS**.|
    |**Data Region**|Yes|Select a region where an ECS instance is located.|
    |**Data Name**|Yes|Enter 3 to 63 characters. Special characters, except for hyphens \(-\) and underscores \(\_\), are not supported.|
    |**NAS Type**|Yes|Select **Others**.|
    |**VPC**|Yes|Select a VPC where the instance is located.|
    |**Switches**|Yes|Select a VSwitch that the VPC uses.|
    |**NAS Address**|Yes|Enter the private IP address of the ECS instance.|
    |**Sub Folder**|Yes|Enter the path of the shared folder where data to be migrated is located.|
    |**Connection Method**|Yes|Select the type of protocol used to share access to files.|
    |**Connection Password**|Optional|Select whether a password is required to access the shared folder.     -   **No Password**: You can access the shared folder without a username and password.
    -   **Use Password**: Enter the required username and password. You must use the username and password to access the shared folder.
 **Note:** When a Windows operating system is running on the instance, you must enter the username and password. For a Linux operating system, you can select one of the preceding options.

 |

    **Note:** For more information about the status of a data address after you create it, see [Data address status](#).

4.  You must apply for whitelist permissions because this feature is still in the beta testing phase. Click **Application**.
5.  Enter the required information and submit the beta testing application for migration. After the application has been approved, you will receive an SMS notification.

## Step 2: Create a destination data address {#section_jzz_pjj_yfb .section}

1.  Select **Data Online Migration** \> **Data Address** and click **Create Data Address**.
2.  In the Create Data Adress dialog box, set the required options and click **OK**. The options are described as follows:

    |Option|Required|Description|
    |:-----|:-------|:----------|
    |**Data Type**|Yes|Select **OSS**.|
    |**Data Region**|Yes|Select a region where the destination data address is located.|
    |**Data Name**|Yes|Enter 3 to 63 characters. Special characters, except for hyphens \(-\) and underscores \(\_\), are not supported.|
    |**OSS Endpoint**|Yes|Select an endpoint based on the region where data is located. For more information, see [Endpoints](../../../../../intl.en-US/Developer Guide/Endpoint/Regions and endpoints.md#).|
    |**AccessKeyId and AccessKeySecret**|Yes|Enter an AccessKey that is used to migrate data. For more information, see [Create an AccessKey](intl.en-US/Migrate data from ECS instances to OSS/Prerequisites.md#ak).|
    |**OSS Bucket**|Yes|Select a bucket to store migration data.|
    |**OSS Prefix**|No|An OSS prefix cannot start with a forward slash \(/\) and must end with a forward slash \(/\). For example: `data/to/oss/`. If you want to store data to the root directory of a bucket, you can leave the OSS Prefix field blank.|


## Step 3: Create a migration job {#section_ksy_xmy_pfb .section}

1.  Choose **Data Online Migration** \> **Migration Jobs** and click **Create Job**.
2.  In the Create Job dialog box, read the Terms of Data Transport, select **I understand the above terms and conditions, activate Data Transport**, and then click **Next**.
3.  In the Create Job dialog box, set the required options and click **Next**.

    The options are described as follows:

    |Option|Required|Description|
    |:-----|:-------|:----------|
    |**Job Name**|Yes|Enter 3 to 63 characters including lowercase letters, numbers, and hyphens \(-\). A job name cannot start or end with a hyphen \(-\).|
    |**Source Data Address**|Yes|Select the created source data address.|
    |**Destination Data Address**|Yes|Select the created destination data address. **Note:** If the region where the source data address is located is different from the region where the destination data address is located, you can [open a ticket](https://selfservice.console.aliyun.com) to apply for the permission of creating a cross-region migration job. You must ensure that your business is legitimate, data transit conforms to local rules and regulations, and that the data does not include illegal information.

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
    |**Start Time Point of File**|Yes \(only for Full and Incremental\)|     -   All: All files are migrated.
    -   Assign: Files that are created or modified after the specified time are migrated. For example, when you set the Start Time Point of File to 11/01/2018 08:00:00, only files that are created or modified after 11/01/2018 08:00:00 are migrated. Files that are created or modified before the specified time will be skipped.
 |
    |**Migration Interval**|Yes \(only for Incremental migration\)|The default value is 1 Hour and the maximum value is 24 Hours.|
    |**Migration Times**|Yes \(only for Incremental migration\)|The default value is 1 time and the maximum value is 30 times.|
    |**Start Time Point of File**|Yes \(only for Sync\)|     -   All: All files are synchronized.
    -   Assign: Files that are created or modified after the specified time are synchronized. For example, when you set the Start Time Point of File to 11/01/2018 08:00:00, only files that are created or modified after 11/01/2018 08:00:00 are synchronized. Files that are created or modified before the specified time will be skipped.
 |
    |**Start Time of Job**|Yes \(only for Sync\)|     -   Immediately: A synchronization immediately runs after a migration job is complete.
    -   Schedule: You can set the scheduled time and synchronize data at the specified time.
 |
    |**Job Period**|Yes \(only for Sync\)|The time interval between two synchronizations. A synchronization runs whenever a job period ends. Valid units: hour, day, and week.|
    |**The next synchronization runs until the last synchronization ends.**|Yes \(only for Sync\)|Don't trigger a new task if another task is running. Assume that you set the Job Period to 1 Hour and you forgot to select this option. The next synchronization runs regardless of whether the last synchronized is completed within one hour. This option is selected by default.|

4.  Click Next to enter the Performance tab.
    -   When you select **Full** or **Incremental**, enter the **Data Size** and the **File Count**.

        **Note:** To ensure a successful migration, you must accurately estimate the amount of data to be migrated. For more information, see [Estimate the amount of data to be migrated](intl.en-US/Migrate data between Alibaba Cloud Object Storage Service (OSS) buckets/Prerequisites.md#section_wvw_v12_qfb).

    -   When you select **Sync**, enter the**Subtask File Count** and **Subtask File Size**.

        -   Subtask File Count: You can separate a migration job into multiple subtasks based on the number of files that you specify. You can run a maximum of 20 subtasks at a time. Set the appropriate number of files for each subtask to reduce the time of a migration job. The default value is 1000. Assume that you need to migrate 10,000 files. When you set the Subtask File Count to 500, the migration job is separated into 20 subtasks that run at the same time. When you set the Subtask File Count to 100, the migration job is separated into 100 subtasks. Each time 20 subtasks run and the remaining subtasks wait to run.
        -   Subtask File Size: You can separate a migration job into multiple subtasks based on the number of files that you specify. You can run a maximum of 20 subtasks at a time. Set the appropriate size of files for each subtask to reduce the time of a migration job. The default value is 1 GB. Assume that you need to migrate a total size of 40 GB files. When you set the Subtask File Size to 2 GB, the migration job is separated to 20 subtasks that run at the same time. When you set the Subtask File Size to 1 GB, the migration job is separated to 40 subtasks. Each time 20 subtasks run and the remaining subtasks wait for running.
        **Note:** A subtask is generated when either the specified Subtask File Count or Subtask File Size is met. When the number of files reaches the specified Subtask File Count but the file size does not reach the specified Subtask File Size, a subtask is generated based on the number of files. When the file size reaches the specified Subtask File Size but the number of files does not reach the specified Subtask File Count, a subtask is generated based on the file size. Assume that you set the Subtask File Count to 1000 and Subtask File Size to 1 GB. When the number of files reaches 1000 but the file size does not reach 1 GB, a subtask is generated based on the number of files. When the file size reaches 1 GB but the number of files does not reach 1000, a subtask is generated based on the file size.

5.  This step is optional. On the Performance tab, navigate to the **Flow Control** area and set the **Time Range** and the **Max Flow**, and then click **Add**.

    **Note:** To ensure business continuity, we recommend that you set the **Time Range** and **Max Flow** based on the fluctuation of visits.

6.  Click **Create**. Wait until a migration job is complete.

## View the status of a data address {#status .section}

After you create the data address of an ECS instance, one of the following states is displayed:

-   Normal: A data address is properly created.
-   Creating: It takes time to create the first data address of an ECS instance \(about three minutes\). Wait for a while. If the status of a data address is in the Creating state for a long time, you can click Refresh in the upper-right corner to update the state.
-   Invalid: An error occurred while creating a data address. You can verify that the configuration information is correct and Data Migration Service is allowed to access the shared files of an ECS instance. If this issue persists, you can contact the [Technical support center](https://selfservice.console.aliyun.com/).

