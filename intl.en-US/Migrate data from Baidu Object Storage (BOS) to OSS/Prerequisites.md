# Prerequisites {#concept_igj_s12_qfb .concept}

This section describes what you must do before creating a migration job.

## Baidu Object Storage {#section_z1p_1df_qfb .section}

-   Estimate the amount of data to be migrated.

    Estimate the size and the number of files to be migrated. Log on to the [BOS console](https://console.bce.baidu.com/bos/), click the name of the bucket to be migrated, and then check the size and the number of objects \(files\).

    **Note:** To ensure a successful migration, you need to enter the appropriate size and number of files when [creating a migration job](intl.en-US/Migrate data between Alibaba Cloud Object Storage Service (OSS) buckets/Create a migration job.md#section_ksy_xmy_pfb).

-   Create an access key for migration.

    Log on to the [BOS console](https://console.bce.baidu.com/bos/), choose **Authorization** \> **Access Key**, and then create an access key.


## Alibaba Cloud Object Storage Service \(OSS\) {#section_p11_xff_qfb .section}

-   Create a destination bucket to store migrated data. For more information, see [Create a bucket](../intl.en-US/Quick Start/Create a bucket.md#).
-   Create and authorize a RAM user
    1.  Log on to the [RAM console](https://ram.console.aliyun.com).
    2.  Choose **Identities** \> **Users** \> **Create User**.
    3.  Select **Console Password Logon** and **Programmatic Access** and enter the required User Account Information.
    4.  Click OK to save the generated account, password, AccessKeyID, and AccessKeySecret.
    5.  Select the required user account, click **Add Permissions** to grant the read/write permission \(AliyunOSSFullAccess\) and migration permission \(AliyunMGWFullAccess\) for the RAM user. The Add Permissions dialog is shown in the following figure.

        ![](../DNhcs_mgw1842487/images/21235_en-US.png)

    6.  Choose **OK** \> **Finished**.
    7.  In the left-side navigation pane, select **Overview**, click the link in the **RAM user logon** section, and enter the username and password of the new RAM user to log on to the console.

        ![](../DNhcs_mgw1842487/images/34662_en-US.png)


