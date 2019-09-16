# JMES语法 {#concept_1597617 .concept}

本文档主要介绍JMES的常用语法。

JMES是一个增强型的JSON查询计算语言，不仅可以对JSON数据进行提取，还可以做计算与转换。关于JMES语法的详细介绍请参见[JMES Tutorial](http://jmespath.org/tutorial.html)

数据加工中的`json_select`、`e_json`、`e_split`支持通过JMES提取字段或JSON表达式的值，或者通过JMES计算特定的值。其用法为：

``` {#codeblock_0cw_tdx_ege}
json_select(值, "jmes表达式", ...)
e_json(字段名, jmes="jmes表达式", ...)
e_split(字段名, ... jmes="jmes表达式", ...)
```

函数的具体用法请参见[json\_select](../../../../cn.zh-CN/.md#section_xrt_z1k_awb)，[e\_json](../../../../cn.zh-CN/数据加工/数据加工语法/全局操作函数/字段值提取函数.md#section_o7x_7rl_2qh)和[e\_split](../../../../cn.zh-CN/数据加工/数据加工语法/全局操作函数/事件操作函数.md#section_urg_dob_o79)。

## 通过key获取值 {#section_jeb_ps8_14g .section}

-   原始日志

    ``` {#codeblock_bsq_oj9_6vb}
    "json_data":{
       "a": "foo",
       "b": "bar",
       "c": "baz"
     }
    ```

-   JMES语法

    ``` {#codeblock_7f7_n4h_036}
    json_select(v("json_data"), "a") #返回值 foo
    json_select(v("json_data"), "b") #返回值 bar
    json_select(v("json_data"), "c") # 返回值 baz
    ```


## 通过层级访问获取值 {#section_k2u_ugk_a04 .section}

-   原始日志

    ``` {#codeblock_ndl_nep_a7b}
    "json_data":{"a": 
                  {"b":
                    {"c":
                      {"d":"value"}
                  }
                }
             }
    ```

-   JMES语法

    ``` {#codeblock_zuu_w1g_mc1}
    json_select(v("json_data"), "a.b.c.d") # 返回值 value
    ```


## 通过切片操作获取值 {#section_bdk_sla_pxw .section}

-   原始日志

    ``` {#codeblock_9kr_qjo_uki}
    "json_data":{
       "a": ["b", "c", "d", "e", "f"]
     }
    ```

-   JMES语法

    ``` {#codeblock_anc_9zp_m6n}
    json_select(v("json_data"), "a[2: ]") # 返回值 ["d", "e", "f"]
    ```


## 多种用法综合使用 {#section_54r_6ai_v04 .section}

-   原始日志

    ``` {#codeblock_txa_duj_40y}
    "json_data":{
      "a": {
        "b": {
          "c": [{"d": [0, [1, 2]]}, {"d": [3, 4]}]
        }
      }
    }
    ```

-   JMES语法

    ``` {#codeblock_s8p_cmf_00l}
    json_select(v("json_data"), "a.b.c[0].d[1][0]") # 返回值 1
    ```


## 通过投影获取值 {#section_91n_7km_44b .section}

-   原始日志1

    ``` {#codeblock_vao_paq_dsd}
    "json_data":{
        "people": [
          {"first": "James", "last": "d"},
          {"first": "Jacob", "last": "e"},
          {"first": "Jayden", "last": "f"},
          {"missing": "different"}
        ],
        "foo": {"bar": "baz"}
    }
    ```

-   JMES语法1

    ``` {#codeblock_rmy_dsl_5nc}
    json_select(v("json_data"), "people[*].first") # 返回值 ["James","Jacob","Jayden"]
    ```


-   原始日志2

    ``` {#codeblock_qt2_0m1_pgl}
    "json_data":{
        "ops": {
        "functionA": {"numArgs": 2},
        "functionB": {"numArgs": 3},
        "functionC": {"variadic": true}
        }
      }
    ```

-   JMES语法2

    ``` {#codeblock_h82_3wx_gpk}
    json_select(v("json_data"), "ops.*.numArgs") # 返回值 [2, 3]
    ```


-   原始日志3

    ``` {#codeblock_ux7_4x2_llg}
    "json_data":{
      "machines": [
        {"name": "a", "state": "running"},
        {"name": "b", "state": "stopped"},
        {"name": "c", "state": "running"}
      ]
    }
    ```

-   JMES语法3

    ``` {#codeblock_2o5_u7t_w4m}
    json_select(v("json_data"), "machines[?state=='running'].name") # 返回值 ["a", "c"]
    ```


## 多值选取 {#section_55p_cnq_150 .section}

-   原始日志

    ``` {#codeblock_ggz_036_do5}
    "json_data":{
      "people": [
        {
        "name": "a",
        "state": {"name": "up"}
        },
        {
        "name": "b",
        "state": {"name": "down"}
        }
      ]
    }
    ```

-   JMES语法

    ``` {#codeblock_voy_xt5_6kg}
    json_select(v("json_data"), "people[].[name, state.name]") # 返回值[["a","up"],["b","down"]]
    ```


## 计算数组长度 {#section_i8z_ief_ilc .section}

-   原始日志

    ``` {#codeblock_swp_9an_v0q}
    "json_data":{
       "a": ["b", "c", "d", "e", "f"]
     }
    ```

-   JMES语法

    ``` {#codeblock_bwc_00t_mb7}
    json_select(v("json_data"), "length(a)") # 返回值 5
    ```

    ``` {#codeblock_mfc_b1y_zok}
    # length(a) > 0, 设置"no-empty"字段为true
    e_if(json_select(v("json_data"), "length(a)", default=0), e_set("no-empty", true))
    ```


