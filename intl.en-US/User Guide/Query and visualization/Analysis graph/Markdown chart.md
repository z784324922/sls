# Markdown chart {#concept_dg2_lsv_m2b .concept}

With Log Service, you can add a markdown chart to the dashboard. In the markdown chart, you can insert images, links, videos, and other elements to make your dashboard page more friendly.

By adding multiple analysis charts to the dashboard when querying and analyzing log data, you can quickly view multiple analysis results and monitor the status of multiple services in real time. With Log Service, you can also add a markdown chart to the dashboard. The markdown chart is edited by using the markdown language. You can insert images, links, videos, and other elements to the markdown chart to make your dashboard page more friendly.

Markdown charts are created according to different requirements. To optimize the dashboard information expression, you can insert text such as background information, chart descriptions, page notes, and extension information in a markdown chart. To easily switch to other query pages, you can insert saved searches or dashboard links of other projects in a markdown chart. To enrich your dashboard information and make your dashboard functions more flexible, you can insert custom images in a markdown chart.

## Scenarios {#section_obg_w2w_m2b .section}

By using a markdown chart, you can customize links that redirect to other dashboards of the current project. You can also insert an image to go with each link to make it easier to tell them apart. You can also insert a markdown chart to describe the parameters in a chart.

![](images/7247_en-US.png "Scenarios")

## Prerequisites {#section_ac5_btv_m2b .section}

1.  Log data is collected.
2.  A dashboard is configured.

## Procedure {#section_wg3_1tv_m2b .section}

1.  On the Dashboard page, click **Edit** in the upper-right corner.
2.  Click **Create Markdown**.
3.  In the displayed page, configure markdown chart properties.

    |Configuration item|Description|
    |:-----------------|:----------|
    |Chart name|Name of your markdown chart.|
    |Show border|Turn on the **Show Border** switch to add borders for your markdown chart.|
    |Show title|Turn on the **Show Title** switch to display your markdown chart title in the dashboard.|
    |Show background|Turn on the **Show Background** switch to add white background for your markdown chart.|

4.  Edit the **Markdown Content**.

    In the **Markdown Content** area, enter markdown statements. The **Show Chart** section on the right displays the preview in real time. Modify the markdown statements according to the preview content.

5.  After you complete the configuration, click **OK**.

    ![](images/7248_en-US.png "Create a markdown chart")


After you complete the configuration, the created markdown chart is displayed under the current dashboard.

## Modify a markdown chart {#section_pv4_3zv_m2b .section}

-   **Modify the chart location and size**

    1.  On the Dashboard page, click **Edit** in the upper-right corner.
    2.  Drag the markdown chart to adjust its location, and drag the lower-right corner of the chart to adjust its size.
    3.  Click **Create** in the upper-right corner.
-   **Modify the chart title**
    1.  On the Dashboard page, click **Edit** in the upper-right corner.
    2.  Enter a new title in the chart title box.
    3.  On the Dashboard page, click **Save** in the upper-right corner, and click **OK** in the displayed dialog box.
-   **Modify the chart content**
    1.  On the Dashboard page, click **Edit** in the upper-right corner.
    2.  Click **Edit** in the upper-right corner of the markdown chart.
    3.  Modify the chart configuration and click **OK**.
-   **Delete a chart**
    1.  On the Dashboard page, click **Edit** in the upper-right corner.
    2.  Click **Delete** in the upper-right corner of the markdown chart.
    3.  On the Dashboard page, click **Save** in the upper-right corner, and click **OK** in the displayed dialog box.

## Common markdown syntax {#section_zhh_y2w_m2b .section}

-   **Title**

    Markdown statement:

    ```
    # Level 1 title
    ## Level 2 title
    ### Level 3 title
    ```

    ![](images/7249_en-US.png "Title preview")

-   **Link**

    Markdown statement:

    ```
    ### Contents
    
    [Chart description](https://help.aliyun.com/document_detail/69313.html)
    
    [Dashboard](https://help.aliyun.com/document_detail/59324.html)
    ```

    ![](images/7250_en-US.png "Link preview")

-   **Image**

    Markdown statement:

    ```
    <div align=center>
    
    ![ Alt txt][id]
    
    With a reference later in the document defining the URL location
    
    [id]: https://octodex.github.com/images/dojocat.jpg  "The Dojocat"
    ```

    ![](images/7251_en-US.png "Image preview")

-   **Special mark**

    Markdown statement:

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

    ![](images/7252_en-US.png "Special mark preview")


For more information about markdown syntax, see [Markdown syntax](https://daringfireball.net/projects/markdown/syntax).

