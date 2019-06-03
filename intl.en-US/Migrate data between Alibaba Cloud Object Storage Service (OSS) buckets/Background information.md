# Background information {#concept_pyw_sjg_qfb .concept}

This topic describes how to migrate data between Alibaba Cloud Object Storage Service \(OSS\) buckets that are owned by multiple accounts, in the same region, or located in multiple regions.

Alibaba Cloud Data Transport is used as a data channel between various data stores. With Data Transport, you can migrate data from third-party data stores to OSS or between OSS buckets.

With Data Transport, you only need to log on to the console, specify a source data address and a destination OSS endpoint, and then create a migration job. After starting a migration job, you can perform management tasks for the job such as viewing the progress and status of the job. Additionally, you can generate the migration report to view the list of migrated files and the list of files that failed to migrate.

**Note:** 

-   During a migration job, when you read data from the source data address, this produces an expense incurred by outbound traffic. You are charged by the storage service provider of the source data address.
-   By default, Data Transport does not support cross-country data migration. For example, you cannot migrate data from a data address that is located in China \(Beijing\) to a data address that is located in US \(Silicon Valley\). If you have similar requirements, you must [open a ticket](https://selfservice.console.aliyun.com) before creating a migration job. You must apply for permission to create a cross-country migration job. You must ensure that your business is legitimate, data does not include illegal information, and data transit conforms to local rules and regulations.
-   When you migrate data between OSS buckets, files whose storage class is Archive are disregarded.

This guide includes the following topics:

-   [Prerequisites](intl.en-US/Migrate data between Alibaba Cloud Object Storage Service (OSS) buckets/Prerequisites.md#)
-   [Create a migration job](intl.en-US/Migrate data between Alibaba Cloud Object Storage Service (OSS) buckets/Create a migration job.md#)
-   [Manage migration jobs](intl.en-US/Migrate data between Alibaba Cloud Object Storage Service (OSS) buckets/Manage migration jobs.md#)

