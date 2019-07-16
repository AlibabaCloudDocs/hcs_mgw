# Pricing {#concept_592780 .concept}

Data Transport is now free of charge. However, you are still charged for traffic, such as uploading or downloading data and calling APIs during migration. This topic describes how to calculate charges incurred by Data Transport.

## Migration process {#section_0yp_889_qg8 .section}

You can use Data Transport to migrate data as follows:

1.  Retrieve the details of data to be migrated: You can use Data Transport to compare the size, content type, and last modification time of files at the source data address with those at the destination data address. Source files with the latest modification time are migrated. Otherwise, these files are disregarded.
2.  Migrate data: You can use Data Transport to migrate data from the source data address to the destination data address.
3.  Check data: You can use Data Transport to compare migrated data with that at the source data address to ensure data integrity.
4.  Generate migration logs: You can use Data Transport to access migrated data to generate migration logs.

## Number of API requests {#section_wf2_tmv_u70 .section}

API requests are created to compare files before migration, migrate data to the source data address, and check data after migration. The number of API requests changes in different scenarios. For a single file, the number of API requests is as follows:

**Note:** 

-   The following content is only applicable when the data source is a third-party data store or Object Storage Service \(OSS\). When you migrate data from Alibaba Cloud Network Attached Storage \(NAS\), HTTP/HTTPS endpoints, and ECS instances, you are not charged for the traffic of calling API operations for these data resources.
-   By default, the following API requests are created for a successful migration job. Additional API requests that will be created when a migration job fails are not described in this topic.

-   Files with different names at the source data address and destination data address
    -   API requests for the source data address
        -   API requests to compare data before migration: GetSize and GetLastModify
        -   API request to migrate data: GetObject
        -   API requests to check data after migration: GetSize and GetMeta
    -   API requests for the destination data address
        -   API request to compare data before migration: GetMeta
        -   API request to migrate data: PutObject.

            **Note:** If the size of a file to be migrated exceeds 150 MB, the file is split into multiple pieces before being uploaded. The size of each piece is less than or equal to 50 MB. The number of API requests changes based on the size of data to be migrated.<span data-goldlog="/cat.main.addNote" class="next-icon next-icon-AddNote next-icon-medium add-note-icon"\> For example, a file with a size of 150 MB is split into three pieces. Therefore, three PutObject requests are created.

        -   API requests to check data after migration: GetSize and GetMeta
        -   API requests to retrieve migration logs: GetSize and GetMeta
-   For migration files with the same name
    -   API requests for the source data address
        -   API requests to compare data before migration: GetSize, GetLastModify, and GetContentType
        -   API request to migrate data: GetObject
        -   API requests to check data after migration: GetSize and GetMeta
    -   API requests for the destination data address
        -   API requests to compare data before migration: GetSize and GetContentType
        -   API request to migrate data: PutObject

            **Note:** If the size of a file to be migrated exceeds150 MB, the file is split into multiple pieces before being uploaded during migration. The size of each piece is less than or equal to 50 MB. The number of API requests changes based on the size of data to be migrated. For example, a file with a size of 150 MB is split into three pieces. Therefore, three PutObject requests are created.

        -   API requests to check data after migration: GetSize and GetMeta
        -   API requests to retrieve migration logs: GetSize and GetMeta
    -   For non-migration files with the same name
        -   API requests for the source data address
            -   API requests to compare data before migration: GetSize and GetLastModify
            -   API requests to check data after migration: GetSize and GetMeta
        -   API requests for the destination data address
            -   API request to compare data before migration: GetMeta
            -   API requests to check data after migration: GetSize and GetMeta
            -   API requests to retrieve migration logs: GetSize and GetMeta

Example: Assume that you need to migrate 1,000 files and the size of each file is 150 MB. 500 of the 1,000 files have a unique name. 300 of the remaining 500 files need to be migrated to the destination data address and they have the same name at both the source and destination data address. However, 200 files do not need to be migrated to the destination data address and they have the same name at both the source and destination data address. The total number of API requests that are created for both the source data address and the destination data address is calculated as follows. The following equations are based on a successful migration job without migration errors.

-   For the source data address

    ``` {#codeblock_k9p_74r_4ts}
    500 × 5 + 300 × 6 + 200 × 4 = 5100
    ```

-   For the destination data address

    ``` {#codeblock_rps_7w4_gdx}
    500 × 6 + 300 × 7 + 200 × 5 = 6100
    ```


**Note:** For charges incurred by API requests, contact the service provider of the data store for further details. For more information about the pricing structure of OSS, see [Billing items](../../../../intl.en-US/Pricing/Billing items.md#).

## Traffic charges incurred by uploading or downloading data {#section_ad2_e9a_apd .section}

When you migrate data, data is downloaded from the source data address before it is uploaded to OSS. This process will incur a specific amount of traffic fees. The traffic fees for different scenarios are as follows:

**Note:** 

-   The following content is only applicable when the data source is a third-party data store or Object Storage Service \(OSS\). When you migrate data from Alibaba Cloud Network Attached Storage \(NAS\), HTTP/HTTPS endpoints, and ECS instances, you are not charged for the traffic of calling APIs for these data resources.
-   By default, the following API requests are created for a successful migration job. Additional API requests that will be created when a migration job fails are not described in this topic.

-   For third-party data sources

    Charges are incurred for the traffic of downloading data at the source data address. The traffic changes based on the size of data to be downloaded. You are charged by the service provider that owns the data source. You are not charged for the traffic of uploading data to OSS.

-   OSS used for both the source data address and the destination data address and data migrated by using Alibaba Cloud

    Data upload or download by using Alibaba Cloud does not incur any charges.

-   OSS used for both the source data address and the destination data address and data migrated by using the Internet

    Charges are incurred for the traffic of downloading data at the source data address. The traffic changes based on the size of data to be downloaded. You are charged by Alibaba Cloud OSS. You are not charged for the traffic of uploading data to OSS.


