# Prerequisites {#concept_187511 .concept}

This topic describes the actions you must take before creating a migration job.

## Alibaba Cloud Object Storage Service { .section}

-   Create a destination bucket.

    Create a destination bucket, which is used to store the migrated data. For more information, see [Create a bucket](../intl.en-US/Quick Start/Create a bucket.md#).

-   Create and authorize a RAM user
    1.  Log on to the [RAM console](https://ram.console.aliyun.com).
    2.  Choose **Identities** \> **Users** \> **Create User**.
    3.  Select **Console Password Logon** and **Programmatic Access** and enter the required User Account Information.
    4.  Click OK to save the generated account, password, AccessKeyID, and AccessKeySecret.
    5.  Select the required user account, click **Add Permissions** to grant the read/write permission \(AliyunOSSFullAccess\) and migration permission \(AliyunMGWFullAccess\) for the RAM user. The Add Permissions dialog is shown in the following figure.

        ![](../DNHCS_MGW18101150/../DNhcs_mgw1842487/images/21235_en-US.png)

    6.  Choose **OK** \> **Finished**.
    7.  In the left-side navigation pane, select **Overview**, click the link in the **RAM user logon** section, and enter the username and password of the new RAM user to log on to the console.

        ![](../DNHCS_MGW18101150/../DNhcs_mgw1842487/images/34662_en-US.png)


## Alibaba Cloud Network Attached Storage \(NAS\) {#section_4ua_joh_vx3 .section}

-   For more information about how to add a mount point for a NAS file system, see [Add a mount point](../intl.en-US/Quick Start/Add a mount point.md#).
-   When you configure access permissions for a NAS instance, you must allow access to the NAS instance by all IP addresses in the same VPC where the NAS file system is located. For more information, see [Manage the access permissions of a file system](../intl.en-US/User Guide/Use permission groups.md#).

