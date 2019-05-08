# Common causes of a migration failure and solutions {#concept_xdc_qxl_sfb .concept}

This section describes the common causes of a migration failure and solutions when you use Data Transport to migrate data.

If a migration job fails, you can [view the list of failed migration files](../../../../intl.en-US/Migrate data between Alibaba Cloud Object Storage Service (OSS) buckets/Manage migration jobs.md#section_jxv_xty_pfb) to analyze the causes of the failure and troubleshoot the issue. On the Migration Jobs page, locate a failed job, click **Manage** next to the job, and click **Retry** to migrate failed files.

The common causes of migration failure and solutions are described as follows:

## Error message: because downloader gets input stream failed. {#section_qvk_kjb_3gb .section}

Description: Assume that Customer A migrates data from Baidu Object Storage \(BOS\) to Alibaba Cloud Object Storage Service \(OSS\). The following error occurs in the failed migration file list.

![](images/35068_en-US.png)

When you view the log file of BOS, the following error message appears in the log file.

![](images/35069_en-US.png)

Cause: The error messages that appear at both sides show that an error occurs in BOS. BOS sets flow limits on the source data address due to a large number of requests. This causes a migration failure.

Solution: You can contact Baidu customer service to remove the flow limits or retry after setting speed limits for the migration job.

**Note:** Known cloud service providers who set flow limits that result in a migration failure are listed as follows:

-   BOS: You can contact the Baidu customer service to remove flow limits or set speed limits when you create a migration job.
-   Qiniu Cloud: Flow and concurrency limits are set by Qiniu Cloud-Integrated CDN \(FUSION\) for test endpoints. We recommend that you separate large amounts of data into multiple collections and migrate these collections one by one or use accelerating domains.
-   UPYUN: UPYUN sets flow limits when you download large amounts of data. When you migrate large amounts of data, we recommend that you contact UPYUN customer service to remove flow limits or use CDN.

## Error message: check size failed. {#section_jkl_zjb_3gb .section}

Description: When you migrate data from third-party data stores to OSS, an error occurs in the failed migration file list.

![](images/35080_en-US.png)

Cause: This issue occurs when the last modification time of files at the source data address is later than the last modification time of files at the destination data address. A file checksum error occurs when you update source files after these files have been migrated to the destination bucket.

Solution: You can migrate the updated files again.

## Error message: premature end of content-length delimited message body. {#section_wnr_kkb_3gb .section}

Description: When you migrate data from third-party data stores to OSS, an error occurs in the failed migration file list.

![](images/35092_en-US.png)

Cause: This issue occurs when a connection that is used to send or received data is closed by OSS. If the interval between two uploads exceeds one minute, the connection that is used to upload data is closed by OSS. Generally, the error is caused by network issues such as high network latency.

Solution: Repeat a migration job.

## Error message: check content-length failed error. {#section_r52_skb_3gb .section}

Description: When you migrate data from third-party data stores to OSS, the error occurs in the failed migration list.

![](images/35134_en-US.png)

Cause: This issue occurs when the name of a source file is the same as that of a destination file but the last modification time of the destination file is later than the source file. Data Transport disregards the file during migration. However, an error may occur when Data Transport checks the file after the migration job is complete.

Solution: When you need to migrate the file, you can repeat the migration after deleting or changing the name of the destination file. When you do not need to migrate the file, you can disregard the error.

## Error message: http status code 403. {#section_b51_rzv_fgb .section}

Description: When you migrate data between OSS buckets, an error occurs in the failed migration file list.

![](images/35649_en-US.png)

Cause: This issue occurs when insufficient permissions are granted. When creating data addresses, you must use an account that has the read permission of the source data address and the write permission of the destination data address. After a migration job is started, an error occurs when you change either the permissions of the account or the access permissions of storage space such as the bucket policy of OSS.

Solution: You can repeat the migration after restoring the permissions of the account.

## Error message: The operation is not valid for the object's state. {#section_k1c_23b_3gb .section}

Description: When you migrate data between OSS buckets, the error occurs in the failed migration file list.

![](images/35651_en-US.png)

Cause: This issue occurs when Data Transport does not support migrating archive files. The error occurs when archive files exist at the source data address.

Solution: If you need to migrate Archive files, repeat the migration after changing the storage class of these files to the Standard.

## Error message: check usermeta failed. {#section_w2y_zbb_wgb .section}

Description: An error occurs when you migrate data from BOS to OSS.

Cause: This issue occurs when the HTTP header or user meta of a file includes special characters that cannot be identified by Data Transport.

Solution: Migrate failed files or repeat the migration after changing either the HTTP header or user meta of the file.

