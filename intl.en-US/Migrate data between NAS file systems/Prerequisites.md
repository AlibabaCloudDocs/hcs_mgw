# Prerequisites {#concept_uwx_4th_yfb .concept}

When you migrate data between NAS data addresses, Data Transport must be allowed to access both the source NAS data address and the destination NAS data address. You must ensure that Data Transport can access two NAS data addresses using a VPC.

## Two NAS data addresses that are not located in a VPC {#section_l53_fln_qfb .section}

If NAS data addresses are not located in VPCs, you must attach both the source NAS data address and destination NAS data address to the same VPC. This enables access to both data addresses.

-   Alibaba Cloud
    -   You must mount both the source NAS file system and the destination NAS file system on the same mount point. For more information, see [Add a mount point](../../../../../intl.en-US/Quick Start/Add a mount point.md#).
    -   If you set access permissions for NAS file systems, you must enable access to these NAS file systems from all IP addresses in the same VPC where the NAS files systems are located. For more information, see [Manage access permissions of a file system](../../../../../intl.en-US/User Guide/Use permission groups.md#).
-   Others
    -   You can use one of the following methods to attach both the source NAS file system and destination NAS file system to the same VPC:
        -   Enable access to a NAS file system from a VPC by connecting to the VPC by using a dedicated data circuit. For more information, contact Alibaba Cloud technical support.
        -   Enable access to a NAS file system by using a VPN network to connect to a VPC.
    -   If you set access permissions for NAS file systems, you must enable access to these NAS file systems from all IP addresses in the same VPC where the NAS files systems are located.

## Two NAS data addresses that are located in the same VPC {#section_edz_1zq_zfb .section}

If both the source NAS file system and the destination NAS file system are located in the same VPC, the connection between the file systems is enabled by default. If you set access permissions for NAS file systems, you must enable access to these NAS file systems from all IP addresses in the same VPC where the NAS files systems are located.

## Two NAS data addresses that are located in different VPC networks {#section_xww_mzq_zfb .section}

-   If NAS file systems are located in different VPC networks, you must connect the VPC networks by using Cloud Enterprise Network \(CEN\)
    -   When the source NAS file system and destination NAS file system are located in the same account, same region but different VPC networks, see [Connect VPC networks in the same account and same region](../../../../../intl.en-US/Quick Start/Connect networks in the same region using the same account.md#).
    -   You must connect the file systems by using CEN. This occurs if the source NAS file system and the destination NAS file system are owned by the same account but located in different VPCs of separate regions. For more information, see [Connect VPC networks that are owned by the same account but located in multiple regions](../../../../../intl.en-US/Quick Start/Connect network instances in different regions using same account.md#).
    -   When the source NAS file system and destination NAS file system are owned by different accounts and located in different VPC of the same region, see [Connect VPC networks that are located in the same region but owned by multiple accounts.](../../../../../intl.en-US/Quick Start/Connect networks in the same region using the different account.md#)
    -   When the source NAS file system and destination NAS file system are owned by different accounts and located in different VPC of the same region, see [Connect VPC networks that are owned by multiple accounts and located in multiple regions](../../../../../intl.en-US/Quick Start/Connect network instances in different regions using different accounts.md#).
-   If you want to set the access permissions for NAS file systems, you must perform the following steps. You must enable access to the source NAS file system and the destination NAS file system from all IP addresses in respective VPCs where the NAS files systems are located.

## Create and authorize a RAM user { .section}

1.  Log on to the [RAM console](https://ram.console.aliyun.com).
2.  Choose **Identities** \> **Users** \> **Create User**.
3.  Select **Console Password Logon** and **Programmatic Access** and enter the required User Account Information.
4.  Click OK to save the generated account, password, AccessKeyID, and AccessKeySecret.
5.  Select the required user account, click **Add Permissions** to grant the read/write permission \(AliyunOSSFullAccess\) and migration permission \(AliyunMGWFullAccess\) for the RAM user. The Add Permissions dialog is shown in the following figure.

    ![](http://static-aliyun-doc.oss-cn-hangzhou.aliyuncs.com/assets/img/40745/155738210521235_en-US.png)

6.  Choose **OK** \> **Finished**.
7.  In the left-side navigation pane, select **Overview**, click the link in the **RAM user logon** section, and enter the username and password of the newly created RAM user to log on to the console.

    ![](http://static-aliyun-doc.oss-cn-hangzhou.aliyuncs.com/assets/img/40745/155738210534662_en-US.png)


