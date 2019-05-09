# Prerequisites {#concept_sxd_w3l_qfb .concept}

This section describes what you must do before creating a migration job.

## Azure Blob {#section_e5x_jnm_qfb .section}

-   Estimate the amount of data to be migrated.

    Estimate the size and the number of files to be migrated. On the [Dashboard](https://portal.azure.com/) page of the Microsoft Azure console, click the name of a bucket to be migrated and then check the size and the number of objects \(files\).

    **Note:** To ensure a successful migration, you must enter the appropriate size and number of files when [creating a migration job](intl.en-US/Migrate data between Alibaba Cloud Object Storage Service (OSS) buckets/Create a migration job.md#section_ksy_xmy_pfb).

-   Manage keys
    1.  In the Microsoft Azure console, click **Storage accounts** and select a storage account.
    2.  Choose **Settings** \> **Access keys** for the account to be migrated, and view the access key and the connection string.

## Alibaba Cloud Object Storage Service {#section_wyg_d3n_qfb .section}

-   Create a destination bucket.

    Create a destination bucket, which is used to store the migrated data. For more information, see [Create a bucket](../intl.en-US/Quick Start/Create a bucket.md#).

-   Create and authorize a RAM user
    1.  Log on to the [RAM console](https://ram.console.aliyun.com).
    2.  Choose **Identities** \> **Users** \> **Create User**.
    3.  Select **Console Password Logon** and **Programmatic Access** and enter the required User Account Information.
    4.  Click OK to save the generated account, password, AccessKeyID, and AccessKeySecret.
    5.  Select the required user account, click **Add Permissions** to grant the read/write permission \(AliyunOSSFullAccess\) and migration permission \(AliyunMGWFullAccess\) for the RAM user. The Add Permissions dialog is shown in the following figure.

        ![](http://static-aliyun-doc.oss-cn-hangzhou.aliyuncs.com/assets/img/40745/155737011521235_en-US.png)

    6.  Choose **OK** \> **Finished**.
    7.  In the left-side navigation pane, select **Overview**, click the link in the **RAM user logon** section, and enter the username and password of the new RAM user to log on to the console.

        ![](http://static-aliyun-doc.oss-cn-hangzhou.aliyuncs.com/assets/img/40745/155737011534662_en-US.png)


