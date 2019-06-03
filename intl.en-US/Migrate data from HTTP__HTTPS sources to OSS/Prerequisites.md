# Prerequisites {#concept_kfw_zdl_qfb .concept}

This topic describes what you need to do before creating a migration job.

## Network resources {#section_l53_fln_qfb .section}

-   Estimate the amount of data to be migrated.

    Estimate the size and number of files to be migrated.

    **Note:** To ensure a successful migration, you need to enter the appropriate size and number of files when [creating a migration job](intl.en-US/Migrate data between Alibaba Cloud Object Storage Service (OSS) buckets/Create a migration job.md#section_ksy_xmy_pfb).

-   Load network resources
    1.  Create a list file on the local PC.

        The list file has two columns.

        -   The first column includes a list of HTTP/HTTPS addresses. The migration service uses the HTTP GET method to download a file from an HTTP/HTTPS address and the HTTP HEAD method to obtain metadata of a file.
        -   The second column includes a list of file names. After a file is migrated, the object name of the files includes a prefix and a file name. Separate two columns with a tab \(`\t`\).
        **Note:** You must specify a file in a list file rather than a file folder.

        Each line includes a file name. Separate two lines with a line feed `\n`.

        Assume that a list file is named list.txt.

        ```
        http://docs-aliyun.cn-hangzhou.oss.aliyun-inc.com/docs/my.doc    docs/my.doc 
        http://docs-aliyun.cn-hangzhou.oss.aliyun-inc.com/pics/my.jpg    pics/my.jpg 
        ```

        **Note:** If a file name includes special characters, such as Chinese characters, spaces, and tabs, you need to perform URL encoding.

        -   When a file name includes signs, you must transcode the link and file name of the file. For example, a file is named \#￥.jpg. After the file name is transcoded, the file name is displayed as \#%EF%BF%A5.jpg and you need to change the link and file name of a row in a list file as follows.

            ```
            http://docs-aliyun.cn-hangzhou.oss.aliyun-inc.com/pics/#%EF%BF%A5.jpg    pics/#%EF%BF%A5.jpg
            ```

        -   When a file name includes Chinese characters, you must transcode the link of the file and keep the file name. For example, a file is named <a Chinese file name\>.jpg. After the file name is transcoded, the file name is displayed as %e5%9b%be%e7%89%87.jpg and you need to change the link of a row in a list file as follows.

            ```
            http://docs-aliyun.cn-hangzhou.oss.aliyun-inc.com/pics/%e5%9b%be%e7%89%87.jpg    pics/<a Chinese file name>.jpg
            ```

    2.  Upload the list file to OSS.

        The address of the list file has the following format: `oss://{bucket}/{list file name}`.


## Alibaba Cloud Object Storage Service {#section_wyg_d3n_qfb .section}

-   Create a destination bucket.

    Create a destination bucket, which is used to store the migrated data. For more information, see [Create a bucket](../intl.en-US/Quick Start/Create a bucket.md#).

-   Create and authorize a RAM user
    1.  Log on to the [RAM console](https://ram.console.aliyun.com).
    2.  Choose **Identities** \> **Users** \> **Create User**.
    3.  Select **Console Password Logon** and **Programmatic Access** and then enter the required User Account Information.
    4.  Click OK to save the generated account, password, AccessKeyID, and AccessKeySecret.
    5.  Select the required user account, click **Add Permissions** to grant the read/write permission \(AliyunOSSFullAccess\) and migration permission \(AliyunMGWFullAccess\) for the RAM user. The Add Permissions dialog is shown in the following figure.

        ![](http://static-aliyun-doc.oss-cn-hangzhou.aliyuncs.com/assets/img/40745/155953142521235_en-US.png)

    6.  In the left-side navigation pane, select **Overview**, click the link in the **RAM user logon** section, and enter the username and password of the newly created RAM user to log on to the console.

        ![](http://static-aliyun-doc.oss-cn-hangzhou.aliyuncs.com/assets/img/40745/155953142534662_en-US.png)


