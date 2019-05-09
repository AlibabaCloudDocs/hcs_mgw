# Prerequisites {#concept_q5q_wgl_qfb .concept}

This section describes what you must do before creating a migration job.

## Google Cloud Storage {#section_frm_3my_pfb .section}

-   Estimate the amount of data to be migrated.

    Estimate the size and the number of files to be migrated. You can use the gsutil tool or storage logs to view the capacity of a bucket. For more information, see [How to obtain the details of a bucket](https://cloud.google.com/storage/docs/getting-bucket-information?hl=zh-cn).

    **Note:** You must enter the appropriate size of a bucket and number of objects \(files\) when [creating a migration job](intl.en-US/Migrate data between Alibaba Cloud Object Storage Service (OSS) buckets/Create a migration job.md#section_ksy_xmy_pfb).

-   Create the private key of a service account for a migration job.

    1.  Log on to the [IAM & admin console](https://console.cloud.google.com/iam-admin).
    2.  Select **Service accounts** and click the drop-down arrow next to the Google logo in the page header section to select the required project.
    3.  Click **CREATE SERVICE ACCOUNT** to create a service account that has the read permission for a bucket to be migrated. In Step 3, click **CREATE KEY**, and select JSON as the key type. Click **CREATE** to save the JSON file to a local PC and click **SAVE**.

        **Note:** For an existing service account, choose **MODIFY** \> **CREATE KEY**. Select JSON as the key type and choose **CREATE** \> **SAVE**.


## Alibaba Cloud Object Storage Service {#section_pvq_r3l_qfb .section}

-   Create a bucket

    Create a bucket to store migrated data For more information, see [Create a bucket](../intl.en-US/Quick Start/Create a bucket.md#).

-   Create and authorize a RAM user
    1.  Log on to the [RAM console](https://ram.console.aliyun.com).
    2.  Choose **Identities** \> **Users** \> **Create User**.
    3.  Select **Console Password Logon** and **Programmatic Access** and enter the required User Account Information.
    4.  Click OK to save the generated account, password, AccessKeyID, and AccessKeySecret.
    5.  Select the required user account, click **Add Permissions** to grant the read/write permission \(AliyunOSSFullAccess\) and migration permission \(AliyunMGWFullAccess\) for the RAM user. The Add Permissions dialog is shown in the following figure.

        ![](http://static-aliyun-doc.oss-cn-hangzhou.aliyuncs.com/assets/img/40745/155737032221235_en-US.png)

    6.  Choose **OK** \> **Finished**.
    7.  In the left-side navigation pane, select **Overview**, click the link in the **RAM user logon** section, and enter the username and password for the new RAM user to log on to the console.

        ![](http://static-aliyun-doc.oss-cn-hangzhou.aliyuncs.com/assets/img/40745/155737032234662_en-US.png)


