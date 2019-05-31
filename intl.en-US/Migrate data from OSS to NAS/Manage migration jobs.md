# Manage migration jobs {#concept_187407 .concept}

This topic describes several subsequent operations after you create a migration job.

Subsequent operations change based on the migration type. You can manage different types of migration jobs as follows.

## Manage full migration and incremental migration jobs {#section_0cm_srb_vnw .section}

-   **View the status of a migration job** 

    After you create a migration job, only one migration job status is displayed. The status can be one of the following:

    -   Migrating: indicates that data is being migrated. This process takes a while.
    -   Create Failed: indicates that you failed to create a migration job. You can view the cause of the failure and recreate a migration job.
    -   Completed: indicates that a migration job is complete. You can view a migration report.
    -   Failed: indicates that a migration job failed. You can view the migration report and migrate failed files.
-   **Modify flow control settings** 

    During a migration job, you can modify flow control settings at any time based on your needs.

    1.  In the [Data Transport console](https://mgw.console.aliyun.com/#/job?_k=6w2hbo), choose **Data Online Migration** \> **Migration Jobs**. On the Migration Jobs page, locate a migration job and click **Manage** next to the job.
    2.  Click **Stop** and ensure that the job is stopped.
    3.  On the Flow Control Time Schedule chart, click **Reset**.
        -   To add a flow control setting, select the appropriate Time Range and Max Flow, and click **Add**.
        -   To delete a flow control setting, click ![](http://static-aliyun-doc.oss-cn-hangzhou.aliyuncs.com/assets/img/40521/155930223530945_en-US.png) next to the flow control setting.
        -   To modify a flow control setting, you must first delete the previous setting and add a new flow control setting.
    4.  Click **OK** and click **Start** to restart the job.
-   **View a migration report** 
    1.  On the Migration Jobs page, locate a job and click **Manage** next to the job.
    2.  Click **Generate Migration Report**. After a report is generated, click **Export** to export the report.

        In a migration report, the following file names appear in the **File list** section:

        -   The name of a file ends with `_total_list`. This file contains a list of total migration files.
        -   The name of a file ends with `_completed_list`. This file contains a list of successful migration files.
        -   The name of a file ends with `_error_list`. This file contains a list of failed migration files.
    3.  In the target NAS, locate the automatically generated folder aliyun\_mgw\_import\_report/. The three files that appear in the migration report are included in this folder. You can download and view a detailed list of files.

        The file formats are as follows:

        -   The file name includes the source data address, file name, file size \(measured in bytes\), and last modified time. This file contains a list of total migration files. The format of a source data address is `nas://<the name of a mount point>:/<prefix>/<objectName>`. For example, `nas://0a28888892-afr82.cn-hangzhou.nas.aliyuncs.com:/myprefix/testfile.txt`.
        -   The file name includes the file name, file size \(measured in bytes\), checksum \(CRC64\), and migration completion time. This file contains a list of successful migration files.
        -   The file name includes the file name, migration start time, migration end time, and error description. This file contains a list of failed migration files.
-   **Retry after a migration failure** 

    If a migration job failed, you can view the generated file whose name ends with \_error\_list to find the causes of failure and troubleshoot the issue. On the Migration Jobs page, locate the failed job, click**Manage** next to the job, and click**Retry** to migrate failed files. For more information about FAQs for migration failures, see [Common causes of a migration failure and solutions](../intl.en-US/FAQ/Common causes of a migration failure and solutions.md#).


## Manage synchronization jobs { .section}

-   View the status of a synchronization job

    After you create a synchronization job, one of the following states can occur:

    -   Migrating: the synchronization task is in progress.
    -   Stopped: Click **Manage** next to a synchronization job to enter the Migration Report page. After you click**Stop**, the job status changes to Stopped.
    -   Create Failed: indicates that you failed to create a synchronization job. You can view the causes of failure and recreate a synchronization job.
-   Manage a synchronization job
    -   View the details of a synchronization job: On the Migration Jobs page, click **Manage** next to a synchronization job to view the job details, such as **Basic**, **Schedule**, and **Flow Control Time schedule**.
    -   Stop or start a synchronization job: On the **Migration Report** page, you can stop or start the synchronization job at any time.
    -   View the history of a job: On the Migration Jobs page, locate a job and click **Check History** next to the job to view the job history.
        -   After a synchronization job is complete, one of the following states for a task is displayed:
            -   Scanning: indicates that a synchronization job is scanning the files of the source data address. The number of scanned files is displayed in the File Count column.
            -   Scan Finished: indicates that a scan is complete. The total number and size of files are display in the File Count and File Size columns, respectively.
            -   Success: indicate that a synchronization job is complete. The number of synchronized files is displayed. You can click ****![](../DNHCS_MGW18101164/../DNHCS_MGW18101150/images/33279_en-US.png) next to Completed to download **the list of completed files**.
            -   Failed: An error may occur when you run a synchronization job. Click **Retry** to resynchronize failed files. You can click ****![](../DNHCS_MGW18101164/../DNHCS_MGW18101150/images/33279_en-US.png) next to Failed to download the **list of failed files**. Based on the list, you can view the details of failed files, such as deleted or lost source files.

## More information {#section_nda_vvu_e7f .section}

For more information, see the following topics:

-   [Migrate data between Alibaba Cloud Object Storage Service \(OSS\) buckets](../intl.en-US/Migrate data between Alibaba Cloud Object Storage Service (OSS) buckets/Background information.md#)
-   [Migrate data from HTTP/HTTPS sources to OSS](../intl.en-US/Migrate data from HTTP__HTTPS sources to OSS/Background information.md#)
-   [Migrate data from Tencent Cloud Object Service \(COS\) to OSS](../intl.en-US/Migrate data from Tencent Cloud Object Service (COS) to OSS/Background information.md#)
-   [Migrate data from Amazon Simple Storage Service \(Amazon S3\) to OSS](../intl.en-US/Migrate data from Amazon Simple Storage Service (Amazon S3) to OSS/Background information.md#)
-   [Migrate data from Azure Blob to OSS](../intl.en-US/Migrate data from Azure Blob to OSS/Background information.md#)
-   [Migrate data from Qiniu Cloud Object Storage \(KODO\) to OSS](../intl.en-US/Migrate data from Qiniu Cloud-Object Storage (KODO) to OSS/Background information.md#)
-   [Migrate data from Baidu Object Storage \(BOS\) to OSS](../intl.en-US/Migrate data from Baidu Object Storage (BOS) to OSS/Background information.md#)
-   [Migrate data from Kingsoft Standard Storage Service \(KS3\) to OSS](../intl.en-US/Migrate data from Kingsoft Standard Storage Service (KS3) to OSS/Background information.md#)
-   [Migrate data from UPYUN Storage Service \(USS\) to OSS](../intl.en-US/Migrate data from UPYUN Storage Service (USS) to OSS/Background information.md#)
-   [Migrate data from Google Cloud Storage to OSS](../intl.en-US/Migrate data from Google Cloud Storage to OSS/Background information.md#)
-   [Migrate data from NAS to OSS](../intl.en-US/Migrate data from NAS to OSS/Background information.md#)
-   [Migrate data from ECS instances to OSS](../intl.en-US/Migrate data from ECS instances to OSS/Background information.md#)
-   [Migrate data between NAS file systems](../intl.en-US/Migrate data between NAS file systems/Background information.md#)

