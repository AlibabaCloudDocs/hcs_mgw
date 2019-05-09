# Background information {#concept_svj_b2m_qfb .concept}

This section describes how to migrate data from Qiniu Cloud-Object Storage \(KODO\) to Alibaba Cloud Object Storage Service \(OSS\).

Alibaba Cloud Data Transport is used as a data channel between various data stores. With Data Transport, you can migrate data from third-party data stores to OSS or between OSS buckets.

With Data Transport, you only need to log on to the console, specify a data source address and a destination OSS endpoint, and then create a migration job. After starting a migration job, you can perform management tasks for the job, such as viewing the progress and status of the job. Additionally, you can generate a migration report to view the list of migrated files and the list of files that failed to migrate.

**Note:** 

-   When you read data from the source data address during a migration job, this produces an expense incurred by outbound traffic. You are charged by the storage service provider of the source data address.
-   By default, Data Transport does not support cross-country data migration. For example, you cannot migrate data from a data address that is located in China \(Beijing\) to a data address that is located in US \(Silicon Valley\). If you have similar requirements, you must [open a ticket](https://selfservice.console.aliyun.com) before creating a migration job to apply for the permission of creating a cross-country migration job. You must ensure that your business is legitimate, data does not include illegal information, and data transit conforms to local rules and regulations.

This guide includes the following sections:

-   [Prerequisites](intl.en-US/Migrate data from Qiniu Cloud-Object Storage (KODO) to OSS/Prerequisites.md#)
-   [Create a migration job](intl.en-US/Migrate data from Qiniu Cloud-Object Storage (KODO) to OSS/Create a migration job.md#)
-   [Manage migration jobs](intl.en-US/Migrate data from Qiniu Cloud-Object Storage (KODO) to OSS/Manage migration jobs.md#)

