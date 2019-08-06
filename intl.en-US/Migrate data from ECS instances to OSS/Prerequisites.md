# Prerequisites {#concept_uwx_4th_yfb .concept}

This topic describes what you need to prepare before migration.

## ECS instances {#section_hr3_k1t_2gb .section}

On an ECS instance, you can perform the following steps to share a folder:

**Note:** You can only migrate data from an ECS instance that is located in a VPC rather than a classic network by using Data Transport.

-   For instances running Windows

    If Windows is running on an ECS instance, you can perform the following steps to share a folder:

    1.  Move data to be migrated to a folder and share the folder. We recommend that you follow version-specific instructions to share a folder.
    2.  Modify the settings of the Windows firewall and anti-virus software to enable access to port 445 of the instance by all IP addresses in the VPC where the instance is located. Skip this step if both the Windows firewall and anti-virus software are disabled.
    3.  Add ECS [security group rules](../../../../../intl.en-US/Security/Security groups/Add security group rules.md#) to enable access to port 445 of the instance from all IP addresses in the VPC where the instance is located.
-   For instances running Linux

    If Linux is running on an ECS instance, you can perform the following steps to share a folder:

    1.  Start the NFS service and share the folder to be migrated. For more information, see [Start the NFS service](#). Skip this step if the NFS service is started.
    2.  Modify the settings of the Linux firewall to enable access to the corresponding port of the NFS service. Use the rpcinfo -p localhost command to view the corresponding ports to be enabled of the `portmapper`, `mountd`, and `nfs` services. For more information, see [Firewall settings](#). If the firewall is not enabled, skip this step.
    3.  Add ECS [security group rules](../../../../../intl.en-US/Security/Security groups/Add security group rules.md#) to enable access to the corresponding port of the NFS service from all IP addresses in the VPC where the instance is located.

        **Warning:** To ensure data security, we recommend that you disable access to the port of the NFS service from external networks.


## Alibaba Cloud Object Storage Service {#section_kry_7tx_wyu .section}

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


## Appendix: Use NFS {#section_o2g_5l4_1gb .section}

Before using NFS, you need to start the NFS service and allow access to the port of the NFS service in the firewall.

-   Assume that you need to share the /data folder as the source data address. You can perform the following steps:
    1.  Enable the NFS file system.

        ``` {#codeblock_8ne_vol_d7g}
        [root@test ~]# yum install -y nfs-utils
        ```

    2.  Share the /data folder. In the /etc/exports file, add the /data \*\(rw,no\_root\_squash,insecure\) entry.

        ``` {#codeblock_twj_8uo_4fq}
        [root@test ~]# vi /etc/exports
        
        #If the port number of mountd is greater than 1024, you need to add the insecure parameter.
        /data *(rw,no_root_squash,insecure)
        								
        ```

        **Note:** 

        We recommend that you follow the formats required by the /etc/exports file to configure settings. You can use the `man 5 exports` command to view the required formats.

        If a configuration error occurs, the file system fails to be mounted on a client.

    3.  Start the NFS service.

        ``` {#codeblock_qs5_nyj_snp}
        [root@test ~]# systemctl start nfs.service
        ```

    4.  View the status of the NFS service. The following status indicates that the service is running.

        ``` {#codeblock_s57_nyo_t4t}
        [root@test ~]# systemctl status nfs.service
        â— nfs-server.service - NFS server and services
        Loaded: loaded (/usr/lib/systemd/system/nfs-server.service; disabled; vendor preset: disabled)
        Active: active (exited) since Thu 2018-12-06 15:47:03 CST; 58s ago
        Process: 10641 ExecStartPost=/bin/sh -c if systemctl -q is-active gssproxy; then systemctl restart gssproxy ; fi (code=exited, status=0/SUCCESS)
        Process: 10623 ExecStart=/usr/sbin/rpc.nfsd $RPCNFSDARGS (code=exited, status=0/SUCCESS)
        Process: 10621 ExecStartPre=/usr/sbin/exportfs -r (code=exited, status=0/SUCCESS)
        Main PID: 10623 (code=exited, status=0/SUCCESS)
        CGroup: /system.slice/nfs-server.service
        
        
        Dec 06 15:47:03 test systemd[1]: Starting NFS server and s...
        Dec 06 15:47:03 test systemd[1]: Started NFS server and se...
        Hint: Some lines were ellipsized, use -l to show in full.
        ```

    5.  Enable the service to run at startup.

        ``` {#codeblock_a6y_zfo_4wx}
        [root@localhost ~]# systemctl enable nginx.service
        ```

    6.  View the status of the rpcbind service. The following status indicates that the service is running.

        ``` {#codeblock_k98_rpd_p36}
        [root@test ~]# systemctl status rpcbind.service
        â— rpcbind.service - RPC bind service
        Loaded: loaded (/usr/lib/systemd/system/rpcbind.service; enabled; vendor preset: enabled)
        Active: active (running) since Thu 2018-12-06 15:47:03 CST; 7min ago
        Main PID: 10598 (rpcbind)
        CGroup: /system.slice/rpcbind.service
        â""â"€10598 /sbin/rpcbind -w
        
        
        Dec 06 15:47:03 test systemd[1]: Starting RPC bind service...
        Dec 06 15:47:03 test systemd[1]: Started RPC bind service.
        Hint: Some lines were ellipsized, use -l to show in full.
        ```

-   Firewalld is used by default for ECS instances that run CentOS 7. You can use the systemctl status firewalld command to check whether firewalld is enabled. If you are using iptables, you can use the related iptables commands to allow access to the ports that are required by NFS based on the following firewalld settings. Configure firewalld as follows:
    1.  View a list of ports you need to enable to start the NFS service.

        ``` {#codeblock_ww8_iq3_f4f}
        [root@test ~]# rpcinfo -p localhost
           program vers proto   port  service
            100000    4   tcp    111  portmapper
            100000    3   tcp    111  portmapper
            100000    2   tcp    111  portmapper
            100000    4   udp    111  portmapper
            100000    3   udp    111  portmapper
            100000    2   udp    111  portmapper
            100024    1   udp  50382  status
            100024    1   tcp  59133  status
            100005    1   udp  20048  mountd
            100005    1   tcp  20048  mountd
            100005    2   udp  20048  mountd
            100005    2   tcp  20048  mountd
            100005    3   udp  20048  mountd
            100005    3   tcp  20048  mountd
            100003    3   tcp   2049  nfs
            100003    4   tcp   2049  nfs
            100227    3   tcp   2049  nfs_acl
            100003    3   udp   2049  nfs
            100003    4   udp   2049  nfs
            100227    3   udp   2049  nfs_acl
            100021    1   udp  37473  nlockmgr
            100021    3   udp  37473  nlockmgr
            100021    4   udp  37473  nlockmgr
            100021    1   tcp  37688  nlockmgr
            100021    3   tcp  37688  nlockmgr
            100021    4   tcp  37688  nlockmgr
        ```

    2.  Add the following firewall rules to enable the corresponding ports of the `portmapper`, `mountd`, and `nfs` services. These ports include port 111, port 20048, and port 2049 for the TCP and UDP protocols.

        **Note:** As the `mountd` service uses a random port number, you must use one of the following methods to retrieve the port number of the `mountd` service and then configure firewalld.

        -   Use the rpcinfo -p localhost command to view the port number used by the `mountd` service.
        -   Open the /etc/sysconfig/nfs file, replace xxx with a port number in the `MOUNTD_PORT=xxx` expression to specify a port number for the `mountd` service.
    3.  Add the firewall rules by running the following commands:

        ``` {#codeblock_4jd_j8u_qnn}
        [root@test ~]# firewall-cmd --zone=public --add-port=111/tcp --permanent
        success
        [root@test ~]# firewall-cmd --zone=public --add-port=20048/tcp --permanent
        success
        [root@test ~]# firewall-cmd --zone=public --add-port=2049/tcp --permanent
        success
        [root@test ~]# firewall-cmd --zone=public --add-port=111/udp --permanent
        success
        [root@test ~]# firewall-cmd --zone=public --add-port=20048/udp --permanent
        success
        [root@test ~]# firewall-cmd --zone=public --add-port=2049/udp --permanent
        success
        ```

    4.  Update firewall rules by running the following command.

        ``` {#codeblock_rj9_rwj_551}
        [root@test ~]# firewall-cmd --reload
        success
        ```


