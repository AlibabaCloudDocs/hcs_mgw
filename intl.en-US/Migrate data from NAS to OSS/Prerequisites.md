# Prerequisites {#concept_uwx_4th_yfb .concept}

This section describes what you must do before creating a migration job.

## Network Attached Storage {#section_l53_fln_qfb .section}

-   Alibaba Cloud NAS
    -   For more information about how to add a mount point for a NAS file system, see [Add a mount point](../../../../../intl.en-US/Quick Start/Add a mount point.md#).
    -   When you configure access permissions for a NAS instance, you must allow access to the NAS instance by all IP addresses in the same VPC where the NAS file system is located. For more information, see [Manage the access permissions of a file system](../../../../../intl.en-US/User Guide/Use permission groups.md#).
-   Third-party NAS file systems
    -   You need to attach a NAS instance to a VPC.
        -   You can connect to the VPC by using a dedicated leased line to enable access to the NAS instance from all IP addresses in the VPC. For more information, contact Alibaba Cloud technical support.
        -   You can connect to the VPC by using a VPN network to enable access to the NAS instance from all IP addresses in the VPC.
    -   When you configure access policies for a NAS instance, you must allow access to the NAS instance by all IP addresses in the same VPC where the NAS file system is located.

## Alibaba Cloud Object Storage Service {#section_wyg_d3n_qfb .section}

-   Create a destination bucket.

    Create a destination bucket, which is used to store the migrated data. For more information, see [Create a bucket](../intl.en-US/Quick Start/Create a bucket.md#).

-   Create and authorize a RAM user
    1.  Log on to the [RAM console](https://ram.console.aliyun.com).
    2.  Choose **Identities** \> **Users** \> **Create User**.
    3.  Select **Console Password Logon** and **Programmatic Access** and enter the required User Account Information.
    4.  Click OK to save the generated account, password, AccessKeyID, and AccessKeySecret.
    5.  Select the required user account, click **Add Permissions** to grant the read/write permission \(AliyunOSSFullAccess\) and migration permission \(AliyunMGWFullAccess\) for the RAM user. The Add Permissions dialog is shown in the following figure.

        ![](http://static-aliyun-doc.oss-cn-hangzhou.aliyuncs.com/assets/img/40745/155730981021235_en-US.png)

    6.  Choose **OK** \> **Finished**.
    7.  In the left-side navigation pane, select **Overview**, click the link in the **RAM user logon** section, and enter the username and password of the new RAM user to log on to the console.

        ![](http://static-aliyun-doc.oss-cn-hangzhou.aliyuncs.com/assets/img/40745/155730981034662_en-US.png)


