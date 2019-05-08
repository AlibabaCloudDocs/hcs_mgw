# Prerequisites {#concept_igj_s12_qfb .concept}

This section describes what you must do before creating a migration job.

## Estimate the amount of data to be migrated. {#section_z1p_1df_qfb .section}

Estimate the size and the number of files to be migrated. Log on to the [Object Storage Service console](https://oss.console.aliyun.com), click the name of the bucket to be migrated, and then check the size and the number of objects \(files\).

**Note:** To ensure a successful migration, you must enter the appropriate size and number of files when [creating a migration job](intl.en-US/Migrate data between Alibaba Cloud Object Storage Service (OSS) buckets/Create a migration job.md#section_ksy_xmy_pfb).

## Create a destination bucket. {#section_p11_xff_qfb .section}

Create a destination bucket to store the migrated data. For more information, see [Create a bucket](../intl.en-US/Quick Start/Create a bucket.md#).

## Create and authorize a RAM user {#section_oss_y3f_qfb .section}

1.  Log on to the [RAM console](https://ram.console.aliyun.com).
2.  Choose **Identities** \> **Users** \> **Create User**.
3.  Select **Console Password Logon** and **Programmatic Access** and enter the required User Account Information.
4.  Click OK to save the generated account, password, AccessKeyID, and AccessKeySecret.
5.  Select the required user account, click **Add Permissions** to grant the read/write permission \(AliyunOSSFullAccess\) and migration permission \(AliyunMGWFullAccess\) for the RAM user. The Add Permissions dialog is shown in the following figure.

    ![](../DNhcs_mgw1842487/images/21235_en-US.png)

6.  Choose **OK** \> **Finished**.
7.  In the left-side navigation pane, select **Overview**, click the link in the **RAM user logon** section, and enter the username and password of the new RAM user to log on to the console.

    ![](../DNhcs_mgw1842487/images/34662_en-US.png)


