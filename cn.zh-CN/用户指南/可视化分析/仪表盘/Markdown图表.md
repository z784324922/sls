# Markdown图表 {#concept_dg2_lsv_m2b .concept}

日志服务支持在仪表盘中增加Markdown图表，在Markdown图表插入图片、链接、视频等多种元素，使您的仪表盘页面更加友好。

在查询分析日志数据时，将多个分析图表添加到仪表盘中，有利于您快速查看多项分析结果、实时监控多项业务的状态信息。日志服务还支持在仪表盘中增加Markdown图表，该图表使用Markdown语言编辑。您可以在Markdown中插入图片、链接、视频等多种元素，使您的仪表盘页面更加友好。

Markdown图表都是根据不同的需求来创建的。您可以在Markdown图表中插入背景信息、图表说明、页面注释和扩展信息等文字内容，优化仪表盘的信息表达；插入快速查询或其他Project的仪表盘链接，方便其他查询页面的跳转；插入自定义的图片，让您的仪表盘信息更加丰富、功能更为灵活。

## 应用场景 {#section_obg_w2w_m2b .section}

通过Markdown图表可以自定义跳转链接到当前Project的其他仪表盘，同时还可以插入图片以便快速区分。当需要对图表参数进行介绍时，也可以插入Markdown图表进行说明。

![](images/7247_zh-CN.png "应用场景")

## 前提条件 {#section_ac5_btv_m2b .section}

1.  已成功采集到日志数据。
2.  已配置仪表盘。

## 操作步骤 {#section_wg3_1tv_m2b .section}

1.  在仪表盘页面单击右上角的**编辑**按钮。
2.  在编辑模式下，将操作栏中的Markdown图标![](http://static-aliyun-doc.oss-cn-hangzhou.aliyuncs.com/assets/img/16006/154718390036999_zh-CN.png)拖动到指定位置，即可创建markdown图表。
3.  选中新建的Markdown图表，从右上角展开菜单，并单击**编辑**。
4.  在弹出页面中设置Markdown图表属性。

    |配置项|说明|
    |:--|:-|
    |图表名称|您创建的Markdown图表名称。|
    |显示边框|选择**显示边框**，为您的Markdown图表增加边框。|
    |显示标题|选择**显示标题**，会在仪表盘中为您展示Markdown图表的标题。|
    |显示背景|选择**显示背景**，为您的Markdown图表添加白色背景。|
    |绑定查询|选择**绑定查询**并设置查询属性后，会在Markdown图表中动态显示查询结果。|

5.  绑定查询（可选）
    1.  选择待查询的日志库，在查询框中输入完整的查询分析语句。查询分析语句由查询语句和分析语句构成，格式为`查询语句|分析语句`。详细说明请参考[简介](cn.zh-CN/用户指南/查询与分析/简介.md#)。
    2.  单击**15分钟（相对）**，设置查询的时间范围。

        您可以选择相对时间、整点时间和自定义时间范围。

        **说明：** 查询结果相对于指定的时间范围来说，有1min以内的误差。

    3.  单击**查询**，显示当前查询结果的第一条数据。
    4.  单击字段旁的“⊕”，即可将查询结果放置在**markdown内容**中光标所在位置。
6.  编辑**Markdown内容**。

    在**Markdown内容**中输入您的Markdown语句，右侧的**图表展示**区域会实时展示预览界面。您可以根据预览内容调整Markdown语句。

7.  配置完成后单击**确定**。

     ![](images/32307_zh-CN.png "创建Markdown图表") 


配置完成后，您可以在当前仪表盘中查看Markdown图表。

## 修改Markdown图表 {#section_pv4_3zv_m2b .section}

-   **修改图表位置和大小**

    1.  在仪表盘页面单击右上角的**编辑**。
    2.  鼠标拖动Markdown图标到指定位置，拖动图表右下角调整图表大小。
    3.  在页面右上角单击**保存**。
-   **修改图表标题**
    1.  在仪表盘页面单击右上角的**编辑**。
    2.  单击指定Markdown图表，并在右上角的折叠列表中单击**编辑**。
    3.  **图表名称**中输入新的标题，并单击**确定**。
    4.  仪表盘页面右上角单击**保存**，退出仪表盘，并在弹出对话框中单击**确定**。
-   **修改图表内容**
    1.  在仪表盘页面单击右上角的**编辑**。
    2.  单击指定Markdown图表，并在右上角的折叠列表中单击**编辑**。
    3.  修改图表配置，并单击**确定**。
    4.  仪表盘页面右上角单击**保存**，退出编辑模式，并在弹出对话框中单击**确定**。
-   **删除图表**
    1.  在仪表盘页面单击右上角的**编辑**。
    2.  单击指定Markdown图表，并在右上角的折叠列表中单击**删除**。
    3.  仪表盘页面右上角单击**保存**，退出编辑模式，并在弹出对话框中单击**确定**。

## 常用Markdown语法 {#section_zhh_y2w_m2b .section}

-   **标题**

    Markdown语句：

    ```
    # 一级标题
    ## 二级标题
    ### 三级标题
    ```

    ![](images/7249_zh-CN.png "标题预览")

-   **链接**

    Markdown语句：

    ```
    ### 目录
    
    [图表说明](https://help.aliyun.com/document_detail/69313.html)
    
    [仪表盘](https://help.aliyun.com/document_detail/59324.html)
    ```

    ![](images/7250_zh-CN.png "链接预览")

-   **图片**

    Markdown语句：

    ```
    <div align=center>
    
    ![Alt txt][id]
    
    With a reference later in the document defining the URL location
    
    [id]: https://octodex.github.com/images/dojocat.jpg  "The Dojocat"
    ```

    ![](images/7251_zh-CN.png "图片预览")

-   **特殊标记**

    Markdown语句：

    ```
    ---
    
    __Advertisement :)__
    
    ==some mark==   `some code`
    > Classic markup: :wink: :crush: :cry: :tear: :laughing: :yum:
    >> Shortcuts (emoticons): :-)  8-) ;)
    
    __This is bold text__
    
    *This is italic text*
    
    ---
    ```

    ![](images/7252_zh-CN.png "特殊标记预览")


关于Markdown语法的详细说明，请查看[Markdown语法](http://www.markdown.cn)。

