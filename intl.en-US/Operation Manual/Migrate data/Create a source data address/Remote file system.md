# Remote file system

This topic describes how to configure a service IP address and create a data address for a remote file system. To migrate data from a remote file system, such as a remote Windows file system, remote Linux file system, or network-attached storage \(NAS\) server, you must create a data address for the remote file system.

## Prerequisites

The device that stores source data is connected to the Ethernet port or optical port of the Data Transport Mini device directly or over network switches. The Ethernet cable or the optical fiber cable and optical module are connected properly. The port indicator is normal.

**Note:** If network switches are used, make sure that the switches are connected to both the source device and the Data Transport Mini device.

## Step 1: Configure a service IP address

1. Log on to the Data Transport console. For more information, see [Set up the device](/intl.en-US/Operation Manual/Migrate data/Install hardware.md).

2. Choose **Control Panel** \> **Network & Virtual Switch** \> **Interfaces**.

3. Find the adapter that is in the **Connected** state and click the edit icon.

![](https://static-aliyun-doc.oss-accelerate.aliyuncs.com/assets/img/en-US/6117016951/p127057.jpg)

4. On the IPv4 tab, select **Use static IP address**. Specify the **Fixed IP address**, **Subnet Mask**, and **Default Gateway** based on your network configurations, and then click **Apply**.

**Note:** This IP address is used to create a data address in the Data Transport console. We recommend that you store this IP address for future reference.

![](https://static-aliyun-doc.oss-accelerate.aliyuncs.com/assets/img/en-US/6117016951/p127110.png)

## Step 2: Create a data address

The source device of data migration based on a Data Transport Mini device can be a remote NAS server, remote Windows file system, or remote Linux file system.

**Create a data address for a remote Windows file system**

1. In a Windows system, share a folder or disk. In this example, the local disk `D:` is shared.

-   i. Right-click the local disk `D:` and choose **Share with** \> **Advanced Sharing**.

-   ii. Click **Advanced Sharing \(D\)**.

-   iii. Select **Share this folder**, and click **OK** to share the Windows folder.

    **Note:** After you share a folder in Windows, make sure that you have added an inbound rule to the Windows Firewall and that the inbound rule allows access from the Data Transport device to port 445. After you add the inbound rule, check the connectivity by using telnet to connect the device to port 445. Alternatively, you can temporarily disable the Windows Firewall until the data migration is complete.


2. In the Data Transport console, create a data address for a remote Windows file system.

-   In the address bar of the browser, enter `https://<Management IP address>:8090`.

**Note:** The default management IP address is `192.168.1.1`. Replace it with the actual management IP address of the Data Transport Mini device. 8090 indicates the port number of the HTTPS service that is enabled on the server.

3. Enter the username and password to log on to the Data Transport console.

To obtain the username and password, contact Alibaba Cloud.

4. Select **Data Address**, and click **Create**.

5. In the **Create Data Address** dialog box, configure the parameters and click **OK**.

**The following section describes several important parameters:**

-   **Data Name**: specifies the name of a data address. We recommend that you use an identifiable name.
-   **Windows Network Address**: specifies the service IP address. For more information, see Step 1: Configure a service IP address.
-   **Windows Subfolder**: specifies the address of a shared Windows file or folder.
-   **Username**: specifies a username that is used to log on to a Windows system.
-   **Password**: specifies the password of the username.

![](https://static-aliyun-doc.oss-accelerate.aliyuncs.com/assets/img/en-US/6117016951/p127102.png)

**Create a data address for a remote Linux file system**

Before you share files in a remote Linux file system, create an NFS file system.

1. Create an NFS file system. CentOS 7 system is used in this example.

-   Install the NFS file system.

    ```
    yum install -y nfs-utils
    ```


-   Modify the /etc/exports file.

    ```
    /data     *(rw,no_root_squash)
    /data1    *(rw,no_root_squash)
    ```

-   Start the NFS service.

    ```
    systemctl start nfs.service
    ```

-   Check the status of the NFS service.

    ```
    systemctl status rpcbind.service
    ```

-   Enable the service to run at startup.

    ```
    systemctl enable nfs.service
    ```


**Note:** You must add a rule to the Linux Firewall and the rule must allow access from the Data Transport Mini device to the NFS service. You can also temporarily disable the Linux Firewall until the data migration is complete.

2. In the Data Transport console, create a remote Linux data address.

-   In the address bar of the browser, enter `https://<Management IP address>:8090`.

**Note:** The default management IP address is `192.168.1.1`. Replace it with the actual management IP address of the Data Transport Mini device. 8090 indicates the port number of the HTTPS service that is enabled on the server.

3. Enter the username and password to log on to the Data Transport console.

To obtain the username and password, contact Alibaba Cloud.

4. Select **Data Address**, and click **Create**.

5. In the **Create Data Address** dialog box, configure the parameters and click **OK**.

**The following section****describes several important parameters**:

-   **Data Name**: specifies the name of a data address. We recommend that you use an identifiable name.
-   **Linux Network Address**: specifies the service IP address. For more information, see Step 1: Configure a service IP address.
-   **Linux Subfolder**: specifies the address of a shared Linux file or folder.
-   **Connection Protocol**: NFS is selected by default. The value of this parameter cannot be modified.

![](https://static-aliyun-doc.oss-accelerate.aliyuncs.com/assets/img/en-US/6117016951/p127103.png)

**Create a data address for a NAS server**

The NFS service is installed and running on a NAS server by default. You do not need to configure the NFS service. If no NFS service runs on a NAS server, you can refer to Step 1 in the "Create a data address in a remote Linux file system" section to configure the NFS service.

1. In the address bar of the browser, enter `https://<Manangement IP address>:8090`.

**Note:** The default management IP address is 192.168.1.1. Replace it with the actual management IP address of the Data Transport Mini device. 8090 indicates the port number of the HTTPS service that is enabled on the server.

2. Enter the username and password to log on to the Data Transport console.

To obtain the username and password, contact Alibaba Cloud.

3. Select **Data Address**, and click **Create**.

4. In the **Create Data Address** dialog box, configure the following parameters and click **OK**.

**The following section describes several important parameters**:

-   **Data Name**: specifies the name of a data address. We recommend that you use an identifiable name.
-   **NAS Mount Target**: specifies the service IP address. For more information, see Step 1: Configure a service IP address.
-   **NAS Subfolder**: specifies the path to the shared directory in the NAS server.
-   **Connection Protocol**: NFS is selected by default. The value of this parameter cannot be modified.

![](https://static-aliyun-doc.oss-accelerate.aliyuncs.com/assets/img/en-US/6117016951/p127104.png)

