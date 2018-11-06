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
2.  在编辑状态下，单击**创建markdown图表**。
3.  在弹出页面中设置Markdown图表属性。

    |配置项|说明|
    |:--|:-|
    |图表名称|您创建的Markdown图表名称。|
    |显示边框|选择**显示边框**，为您的Markdown图表增加边框。|
    |显示标题|选择**显示标题**，会在仪表盘中为您展示Markdown图表的标题。|
    |显示背景|选择**显示背景**，为您的Markdown图表添加白色背景。|

4.  编辑**Markdown**内容。

    在**markdown内容**中输入您的Markdown语句，右侧的**图表展示**区域会实时为您展示预览界面。您可以根据预览内容调整Markdown语句。

5.  配置完成后单击**确定**。

    ![](images/7248_zh-CN.png "创建Markdown图表")


配置完成后，您刚刚创建的Markdown图表会出现在当前仪表盘的下方位置。

## 修改Markdown图表 {#section_pv4_3zv_m2b .section}

-   **修改图表位置和大小**

    1.  在仪表盘页面单击右上角的**编辑**。
    2.  鼠标拖动Markdown图表到指定位置，拖动图表右下角调整图表大小。
    3.  单击页面右上角的**保存**。
-   **修改图表标题**
    1.  在仪表盘页面单击右上角的**编辑**。
    2.  在图表标题框中输入新的标题。
    3.  单击仪表盘页面右上角的**保存**，在弹出对话框中单击**确定**。
-   **修改图表内容**
    1.  在仪表盘页面单击右上角的**编辑**。
    2.  单击Markdown图表右上角的**编辑**。
    3.  修改图表配置，并单击**确定**。
-   **删除图表**
    1.  在仪表盘页面单击右上角的**编辑**。
    2.  单击Markdown图表右上角的**删除**。
    3.  单击仪表盘页面右上角的**保存**，在弹出对话框中单击**确定**。

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


关于Markdown语法的详细说明，请查看[Markdown语法](https://daringfireball.net/projects/markdown/syntax)。

