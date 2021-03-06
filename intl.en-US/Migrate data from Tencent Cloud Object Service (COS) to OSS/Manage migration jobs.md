# Manage migration jobs {#concept_h3k_kcf_qfb .concept}

This topic describes several subsequent operations after you create a migration job.

## View the status of a migration job {#section_njn_5ty_pfb .section}

After you create a migration job, only one migration job status is displayed. The status can be one of the following:

-   Migrating: indicates that data is being migrated. This process takes a while.
-   Create Failed: indicates that you failed to create a migration job. You can view the cause of the failure and recreate a migration job.
-   Completed: indicates that a migration job is complete. You can view a migration report.
-   Failed: indicates that a migration job failed. You can view the migration report and migrate failed files again.

## Modify flow control settings { .section}

During a migration job, you can modify flow control settings at any time based on your needs.

1.  In the [Data Transport console](https://mgw.console.aliyun.com/#/job?_k=6w2hbo), choose **Data Online Migration** \> **Migration Jobs**. On the Migration Jobs page, locate a migration job and click **Manage** next to the job.
2.  Click **Stop** and ensure that the job is stopped.
3.  On the Flow Control Time Schedule chart, click **Reset**.
    -   To add a flow control setting, select the appropriate Time Range and Max Flow, and click **Add**.
    -   To delete a flow control setting, click ![](http://static-aliyun-doc.oss-cn-hangzhou.aliyuncs.com/assets/img/40521/155746925630945_en-US.png) next to the flow control setting.
    -   To modify a flow control setting, you must first delete the previous setting and add a new flow control setting.
4.  Click **OK** and click **Start** to restart the job.

## View the migration report {#section_jxv_xty_pfb .section}

1.  In the [Alibaba Cloud Data Transport console](https://mgw.console.aliyun.com/), choose **Data Online Migration** \> **Migration Jobs**. Locate a migration job and click **Manage** next to the job.
2.  Click **Generate Migration Report**. After a report is generated, click **Export** to export the report.

    In a migration report, the following file names appear in the **File list** section:

    -   The name of a file ends with `_total_list`. This file contains a list of total migration files.
    -   The name of a file ends with `_completed_list`. This file contains a list of successful migration files.
    -   The name of a file ends with `_error_list`. This file contains a list of failed migration files.
3.  In the [OSS console](https://oss.console.aliyun.com), locate the automatically generated folder aliyun\_mgw\_import\_report/. The three files that appear in the migration report are included in this folder. You can download these files to view the detailed lists.

    The file formats files are as follows:

    -   The file name includes the source data address, file name, file size \(measured in bytes\), and last modified time. This file contains a list of total migration files. The format of the source data address is: `<vendor>://<bucketName>/<prefix>/<objectName>` such as `cos://mybucket/myprefix/testfile.txt`.
    -   The file name includes the file name, file size \(measured in bytes\), checksum \(CRC64\), and migration completion time. This file contains a list of successful migration files.
    -   The file name includes the file name, migration start time, migration end time, and error description. This file contains a list of failed migration files.

## Retry after a migration failure {#section_zyb_v5y_pfb .section}

Locate a failed migration job in migration job records, click **Manage**, and then click **Retry** to migrate those failed files in this migration job again. During migration, you can pause and resume this migration job at any time.

## More information {#section_oy2_jg4_yfb .section}

For more information, see the following topics:

-   [Migrate data between Alibaba Cloud Object Storage Service \(OSS\) buckets](../intl.en-US/Migrate data between Alibaba Cloud Object Storage Service (OSS) buckets/Background information.md#)
-   [Migrate data from HTTP/HTTPS sources to OSS](../intl.en-US/Migrate data from HTTP__HTTPS sources to OSS/Background information.md#)
-   [Migrate data from ECS instances to OSS](../intl.en-US/Migrate data from ECS instances to OSS/Background information.md#)
-   [Migrate data from Amazon Simple Storage Service \(Amazon S3\) to OSS](../intl.en-US/Migrate data from Amazon Simple Storage Service (Amazon S3) to OSS/Background information.md#)
-   [Migrate data from Azure Blob to OSS](../intl.en-US/Migrate data from Azure Blob to OSS/Background information.md#)
-   [Migrate data from Qiniu Cloud-Object Storage \(KODO\) to OSS](../intl.en-US/Migrate data from Qiniu Cloud-Object Storage (KODO) to OSS/Background information.md#)
-   [Migrate data from Baidu Object Storage \(BOS\) to OSS](../intl.en-US/Migrate data from Baidu Object Storage (BOS) to OSS/Background information.md#)
-   [Migrate data from Kingsoft Standard Storage Service \(KS3\) to OSS](../intl.en-US/Migrate data from Kingsoft Standard Storage Service (KS3) to OSS/Background information.md#)
-   [Migrate data from UPYUN Storage Service \(USS\) to OSS](../intl.en-US/Migrate data from UPYUN Storage Service (USS) to OSS/Background information.md#)
-   [Migrate data from Google Cloud Storage to OSS](../intl.en-US/Migrate data from Google Cloud Storage to OSS/Background information.md#)
-   [Migrate data from NAS to OSS](../intl.en-US/Migrate data from NAS to OSS/Background information.md#)
-   [Migrate data between NAS file systems](../intl.en-US/Migrate data between NAS file systems/Background information.md#)

