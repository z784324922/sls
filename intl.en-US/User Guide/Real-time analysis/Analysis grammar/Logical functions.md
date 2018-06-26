# Logical functions {#concept_vsj_kmq_zdb .concept}

## Logical operators {#section_btz_bkv_tdb .section}

|Operator|Description|Example|
|:-------|:----------|:------|
|AND|Returns TRUE only when both the left and right operands are TRUE.|a AND b|
|OR|Returns TRUE if either the left or right operand is TRUE.|a OR b|
|NOT|Returns TRUE only when the right operand is FALSE.|NOT a|

## NULL involved in logical operation {#section_acw_ckv_tdb .section}

The following table lists the true values when the values of a and b are TRUE, FALSE, and NULL respectively.

|a|b|a AND b|A or B|
|:-|:-|:------|:-----|
|TRUE|TRUE|TRUE|TRUE|
|TRUE|FALSE|FALSE|TRUE|
|TRUE|NULL|NULL|TRUE|
|FALSE|TRUE|FALSE|TRUE|
|FALSE|FALSE|FALSE|FALSE|
|FALSE|NULL|FALSE|NULL|
|NULL|TRUE|NULL|TRUE|
|NULL|FALSE|FALSE|NULL|
|NULL|NULL|NULL|NULL|

|a|NOT a|
|:-|:----|
|TRUE|FALSE|
|FALSE|TRUE|
|NULL|NULL|

