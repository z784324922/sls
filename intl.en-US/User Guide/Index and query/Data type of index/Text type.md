# Text type {#concept_wg1_y3q_zdb .concept}

Similar to search engines, text data is queried based on terms. Therefore, you must configure word segmentation, case sensitivity, including full text query options.

## Instructions {#section_clf_yyt_tdb .section}

-   **Case sensitivity** 

    Determine whether to support case sensitivity when querying raw logs. For example, the raw log is internalError .

    -   After turning off the Case Sensitive switch, the sample log can be queried based on the keyword INTERNALERROR or internalerror .
    -   After turning on the Case Sensitive switch, the sample log can only be queried based on the keyword internalError .
-   **Token** 

    You can separate the contents of a raw log into several keywords by using a token.

    For example, the raw log is

    ``` {#codeblock_m0u_790_ejo}
    /url/pic/abc.gif
    ```

    -   If no token is set, the string is considered as an individual word `/url/pic/abc.gif`. You can only query this log by using the complete string or fuzzy match such as `/url/pic/*` .
    -   If `/` is set as the token, the raw log is separated into three words: `url` , `pic` , and `abc.gif` . You can query this log by using any of the three words or fuzzy match, for example, `url` , `abc.gif` , or `pi*` . You can also use `/url/pic/abc.gif` to query this log \( `url and pic and abc.gif` is separated into the following three conditions during the query: url , pic , and abc.gif \).
    -   If `/.`. is set as the token, the raw log is separated into four words: `url` , `pic` , `abc` , and `gif` .
    **Note:** You can broaden the query range by setting appropriate tokens.

-   **Full text index** 

    By default, full text query \(index\) considers all the fields and keys of a log, except the time field, as text data, and does not need to specify keys. For example, the following log is composed of four fields \(time/status/level/message\):

    ``` {#codeblock_xfv_ug9_ws7}
    [20180102 12:00:00] 200,error,some thing is error in this field
    ```

    -   time:2018-01-02 12:00:00
    -   level:”error”
    -   status:200
    -   message:”some thing is error in this field”
    After enabling full text index, the following text data is assembled in the “key:value + space” mode.

    ``` {#codeblock_n2j_m65_5to}
    status:200 level:error message:"some thing is error in this field"
    ```

    **Note:** 

    -   Prefix is not required for full text query. Enter error as the keyword, both level field and message field meet the query condition.
    -   You must set a token for the full text query. If a space is set as the token, status:200 is considered as a phrase. If : is set as the token, status and 200 are considered as two independent phrases.
    -   Numbers are processed as texts. For example, you can use the keyword 200 to query this log. The time field is not processed as a text.
    -   You can query this log if you enter a key such as ”status" .

