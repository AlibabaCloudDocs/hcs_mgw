# Prerequisites {#concept_q5q_wgl_qfb .concept}

This topic describes what must to do before creating a migration job.

## Tencent Cloud Object Storage \(COS\) {#section_frm_3my_pfb .section}

-   Estimate the amount of data to be migrated.

    Estimate the size and the number of files to be migrated. You can access[Tencent Cloud OSS console](https://console.cloud.tencent.com/cos5/bucket), Click **Buckets**, select a bucket to be migrated, click ![](http://static-aliyun-doc.oss-cn-hangzhou.aliyuncs.com/assets/img/40745/156015725538562_en-US.png)to view the size of the bucket, and click **File list** to view the number of files \(objects\).

    **Note:** To ensure a successful migration, you must enter the appropriate size and number of files when [creating a migration job](intl.en-US/Migrate data between Alibaba Cloud Object Storage Service (OSS) buckets/Create a migration job.md#section_ksy_xmy_pfb).

-   Create an API key for the migration job.

    Log on to the Tencent Cloud console, choose **Cloud Services** \> **Management Tools** \> **Cloud API Key** \> **API Key Management** to view or create a key.


## Alibaba Cloud Object Storage Service { .section}

-   Create a destination bucket.

    Create a destination bucket, which is used to store the migrated data. For more information, see [Create a bucket](../intl.en-US/Quick Start/Create a bucket.md#).

-   Create and authorize a RAM user
    1.  Log on to the [RAM console](https://ram.console.aliyun.com).
    2.  Choose **Identities** \> **Users** \> **Create User**.
    3.  Select **Console Password Logon** and **Programmatic Access** and then enter the required User Account Information.
    4.  Click OK to save the generated account, password, AccessKeyID, and AccessKeySecret.
    5.  Select the required user account, click **Add Permissions** to grant the read/write permission \(AliyunOSSFullAccess\) and migration permission \(AliyunMGWFullAccess\) for the RAM user. The Add Permissions dialog is shown in the following figure.

        ![](../DNhcs_mgw1849439/../DNhcs_mgw1842487/images/21235_en-US.png)

    6.  In the left-side navigation pane, select **Overview**, click the link in the **RAM user logon** section, and enter the username and password of the newly created RAM user to log on to the console.

        ![](../DNhcs_mgw1849439/../DNhcs_mgw1842487/images/34662_en-US.png)


