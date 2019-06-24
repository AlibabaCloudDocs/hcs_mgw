# Prerequisites {#concept_uwx_4th_yfb .concept}

This topic describes what you need to do before creating a migration job.

## ECS instances {#section_hr3_k1t_2gb .section}

On an ECS instance, you can use the following steps to share folders:

-   Windows

    If Windows is running on the instance, proceed as follows:

    1.  Place data to be migrated in a folder and share the folder. Follow the version-specific instructions to share a folder.
    2.  Modify the settings of Windows Defender Firewall and anti-virus software to enable access to port 445 of the instance \(by all IP addresses in the VPC where the instance is located\). Skip this step if both Windows Defender Firewall and anti-virus software are disabled.
    3.  Add ECS [security group rules](../../../../../intl.en-US/Security/Security groups/Add security group rules.md#) to enable access to port 445 of the instance by all IP addresses in a VPC where the instance is located.
-   Linux

    If Linux is running on the instance, proceed as follows:

    1.  Start the NFS service and share the folder to be migrated. For more information, see [Start the NFS service](#). Skip this step if the NFS service is enabled.
    2.  Modify the settings of Linux firewalls to enable access to the corresponding port of the NFS service. You can use the rpcinfo -p localhostcommand to view the corresponding ports to be enabled of the `portmapper`, `mountd`, and `nfs` services. For more information, see [Firewall settings](#). If firewalls are not enabled, skip this step.
    3.  Add ECS [security group rules](../../../../../intl.en-US/Security/Security groups/Add security group rules.md#) to enable access to the corresponding port of the NFS service by all IP addresses in a VPC where the instance is located.

        **Warning:** To ensure data security, disable access to the port of the NFS service by external networks.


## Alibaba Cloud Object Storage Service {#section_adi_x71_a2p .section}

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


## Alibaba Cloud Object Storage Service {#section_pvq_r3l_qfb .section}

## Appendix: How to use NFS {#section_o2g_5l4_1gb .section}

Before using NFS, you need to start the NFS service and enable access to the port of the NFS service in the firewall.

-   Assume that you need to share the /data folder as the source data address. Proceed as follows:
    1.  Enable the NFS file system.

        ``` {#codeblock_cez_eqv_qq5}
        [root@test ~]# yum install -y nfs-utils
        ```

    2.  Share the /data folder. In the /etc/exports file, add /data \*\(rw,no\_root\_squash,insecure\).

        ``` {#codeblock_s5s_ev4_f2a}
        [root@test ~]# vi /etc/exports
        
        #If the port number of mountd is greater than 1024, you need to add the insecure parameter.
        /data *(rw,no_root_squash,insecure)
        
        								
        ```

    3.  Start the NFS service.

        ``` {#codeblock_m3t_dpx_00l}
        [root@localhost ~]#systemctl start nginx.service
        ```

    4.  View the status of the NFS service. The following status indicates that the service is running.

        ``` {#codeblock_luq_7wi_mgi}
        [root@localhost ~]#systemctl status nginx.service
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

        ``` {#codeblock_bxw_wk8_vyw}
        [root@localhost ~]# systemctl enable nginx.service
        ```

    6.  View the status of the rpcbind service. The following status indicates that the service is running.

        ``` {#codeblock_83z_n5f_52i}
        [root@test ~]# systemctl status rpcbind.service
        â— rpcbind.service - RPC bind service
        Loaded: loaded (/usr/lib/systemd/system/rpcbind.service; enabled; vendor preset: enabled)
        Active: active (running) since Thu 2018-12-06 15:47:03 CST; 7min ago
        Main PID: 10598 (rpcbind)
        CGroup: /system.slice/rpcbind.service
        â””â”€10598 /sbin/rpcbind -w
        
        
        Dec 06 15:47:03 test systemd[1]: Starting RPC bind service...
        Dec 06 15:47:03 test systemd[1]: Started RPC bind service.
        Hint: Some lines were ellipsized, use -l to show in full.
        ```

-   Firewalld is used by default for ECS instances that run CentOS 7. You can use the systemctl status firewalld command to check whether firewalld is enabled. If you are using iptables, use the related iptables commands to enable access to the ports \(that are required by NFS\) based on the following firewalld settings. Configure firewalld as follows:
    1.  View the list of enabled ports that are required for NFS.

        ``` {#codeblock_x4t_8dw_0ov}
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

    2.  Add the following firewall rules to enable the corresponding ports of the `portmapper`, `mountd`, and `nfs` services. These ports include port 111, port 20048, and port 2049 for the TCP and UDP protocols respectively.

        **Note:** As the `mountd` service uses a random port number, you must use one of the following methods to retrieve the port number of the `mountd` service and then configure firewalld.

        -   Use the rpcinfo -p localhost command to view the port number used by the `mountd` service.
        -   Open the /etc/sysconfig/nfs file, replace xxx with a port number in the `MOUNTD_PORT=xxx` expression to specify a fixed port number for the `mountd` service.
    3.  Add the following firewall rules:

        ``` {#codeblock_l8t_3eb_yh2}
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

    4.  Update firewall rules.

        ``` {#codeblock_4tw_blu_bzr}
        [root@test ~]# firewall-cmd --reload
        success
        ```


