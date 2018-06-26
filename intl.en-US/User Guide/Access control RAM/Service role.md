# Service role {#concept_ugt_r4q_zdb .concept}

Log Service currently offers an alarm function that works with your log contents. To read log data, the service account of Log Service must be given explicit authorization to access your data.  If you have read this document and completed the authorization, skip the following sections and create alarm rules directly. For how to authorize a service role, see the following sections.

## Create a Resource Access Management \(RAM\) role {#section_pl3_514_12b .section}

1.  Log on to the Resource Access Management \(RAM\) console. Click **Roles** in the left-side navigation pane and click **Create Role** in the upper-right corner. The Create Role dialog box appears. Select **Service Role** in the Select Role Type step.

    ![](http://static-aliyun-doc.oss-cn-hangzhou.aliyuncs.com/assets/img/13194/5836_en-US.png "Select the role type.")

2.  Select **LOG Log Service** in the Enter Type step.

    ![](http://static-aliyun-doc.oss-cn-hangzhou.aliyuncs.com/assets/img/13194/5837_en-US.png "Fill in type information")

3.  Enter **aliyunlogreadrole** in the Role Name field. \(By default, this role is assumed to access data. Therefore, do not change this role name.\) Then, click **Create**.

    ![](http://static-aliyun-doc.oss-cn-hangzhou.aliyuncs.com/assets/img/13194/5838_en-US.png "Configure basic role information.")


## Authorize the role to access log data {#section_glb_514_12b .section}

After creating the role, click **Authorize** at the right of **aliyunlogreadrole** on the **Role Management** page. The Edit Role Authorization Policy dialog box appears. Search for the **AliyunLogReadOnlyAccess**permission under Available Authorization Policy Names. Select this permission and click 1 to add it to the Selected Authorization Policy Name. Then, click OK.

![](http://static-aliyun-doc.oss-cn-hangzhou.aliyuncs.com/assets/img/13194/5839_en-US.png "Edit Role authorization policy")

Then, Log Service has the permissions to regularly read data from specified Logstores to perform alarm checks.

