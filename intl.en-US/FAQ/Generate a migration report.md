# Generate a migration report {#concept_185898 .concept}

This topic describes how to generate a migration report after a migration job is complete.

## Use OSS as the destination data address {#section_4o1_g1y_dq2 .section}

If you migrate data from various data sources to OSS, refer to the following steps.

1.  On the Migration Jobs page, locate a job and click **Manage** next to the job.
2.  Click **Generate Migration Report**. After a report is generated, click **Export** to export the report.

    In a migration report, the following file names appear in the **File list** section:

    -   The name of a file ends with `_total_list`. This file contains a list of total migration files.
    -   The name of a file ends with `_completed_list`. This file contains a list of successful migration files.
    -   The name of a file ends with `_error_list`. This file contains a list of failed migration files.
3.  In the [OSS console](https://oss.console.aliyun.com), locate the generated folder aliyun\_mgw\_import\_report/. The three files that appear in the migration report are included in this folder. You can download and view a detailed list of files.

    The file formats are as follows:

    -   The file name includes the source data address, file name, file size \(measured in bytes\), and last modification time. This file contains a list of total migration files. The format of the data source address is: `<vendor>://<bucketName>/<prefix>/<objectName>`. For example, `oss://bucket-test1022/myprefix/testfile.txt`.
    -   The file name includes the file name, file size measured in bytes, CRC-64 checksum, and migration completion time. This file contains a list of successful migration files.
    -   The file name includes the file name, migration start time, migration end time, and error description. This file contains a list of failed migration files.

## Use NAS as the destination data address {#section_me1_5h0_n8w .section}

If you migrate data from various data sources to NAS, refer to the following steps:

1.  On the Migration Jobs page, locate a job and click **Manage** next to the job.
2.  Click **Generate Migration Report**. After a report is generated, click **Export** to export the report.

    In a migration report, the following file names appear in the **File list** section:

    -   The name of a file ends with `_total_list`. This file contains a list of total migration files.
    -   The name of a file ends with `_completed_list`. This file contains a list of successful migration files.
    -   The name of a file ends with `_error_list`. This file contains a list of failed migration files.
3.  At the destination data address, locate the automatically generated aliyun\_mgw\_import\_report/ file folder. The preceding files are included in the folder. You can download and view a detailed list of files. We recommend that you use the [ossbrowser](../../../../intl.en-US/Tools/ossbrowser/Quick start.md#) tool to view these files.

    The file formats are as follows:

    -   The file name includes the source data address, file name, file size \(measured in bytes\), and last modification time. This file contains a list of total migration files. The format of a source data address is `nas://<the name of a mount point>:/<prefix>/<objectName>`. For example, `nas://0a28888892-afr82.cn-hangzhou.nas.aliyuncs.com:/myprefix/testfile.txt`.
    -   The file name includes the file name, file size measured in bytes, CRC-64 checksum, and migration completion time. This file contains a list of successful migration files.
    -   The file name includes the file name, migration start time, migration end time, and error description. This file contains a list of failed migration files.

