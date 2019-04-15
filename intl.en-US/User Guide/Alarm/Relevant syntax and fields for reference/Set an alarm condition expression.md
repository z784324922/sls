# Set an alarm condition expression {#reference_byz_wrd_yfb .reference}

To use the alarm function, you can set an expression of alarm conditions. Based on the true or false result of the expression, the system determines whether the alarm conditions are met.

When the system determines whether an alarm condition expression is true or false, the results of your configured queries are used as fixed values and log fields are used as variables. If the conditions of your expression are true, an alarm is triggered.

## Limits {#section_q2c_std_yfb .section}

-   You must enclose negative numbers in parentheses \(\), for example, x+\(-100\)<100.
-   The numeric value type is a 64-bit floating-point number. If you perform comparison operations, errors may occur. For example, using the equal to operator \(==\) may cause errors.
-   A variable can contain only letters and numbers, and must start with a letter.
-   An expression can be up to 128 characters in length.
-   If you need to combine multiple result sets to evaluate your expression, up to 1000 combinations of result sets can be calculated. If all the combinations of result sets are false, your expression is then considered false.
-   Up to three queries can be configured for an alarm.
-   An alarm is triggered only when the value of an expression is the Boolean value true. For example, the expression of 100+100 does not trigger an alarm because the expression calculation result of is 200.
-   `true`,`false`,`$`, and`.` are reserved and cannot be used as variables.

## Basic syntax {#section_wqm_csd_yfb .section}

Alarm condition expressions support the following types of syntax.

|Syntax type|Description|Example|
|:----------|:----------|:------|
|Basic operators|Available basic operators are: addition \(+\), subtraction \(-\), multiplication \(\*\), division \(/\), and modulus \(%\).| x\*100+y\>200

 x%10\>5

 |
|Comparison operators|Available comparison operators are: greater than \(\>\), greater than or equal to \(\> =\), less than \(<\), less than or equal to \(<=\), equal to \(==\), not equal to \(! =\), regular expression match \(= ~\), regular expression not-match \(! ~\) .**Note:** 

-   Slashes must be escaped.
-   Regular expressions support syntax that meets the requirements of [RES2 Guide](https://github.com/google/re2/wiki/Syntax).

| x \>= 0

 x < 100

 x <= 100

 x == 100

 x == "foo"

 Regular expression match: x =~ "\\\\w+"

 |
|Logical operators|Available logical operators are: and \(&&\) and or \(||\).| x \>=0&&y <=100

 x \> 0 || y \> 0

 |
|Not operator|Not operator \(!\).| !\( a < 1 && a \> 100\)

 |
|Numeric constants|Numeric constants are handled as 64-bit floating-point numbers.| x \> 100

 |
|String constants|The form of a string constant is a string enclosed in single quotation marks. For example, 'string '.| foo == 'string'

 |
|Boolean constants|Available Boolean constants are true and false.| \(x \> 100\) == true

 |
|Parentheses \(\)|Parentheses \(\) raise calculation precedence.| x\*\(y+100\)\>100

 |
|Contains function|The contains function is used to determine whether a sub-string is included. For example, if the contains\(field, 'xxxx'\) expression returns the true result, you can determine that the xxxx sub-string is included in the field string.| contains\(foo, 'hello'\)

 |

## Combine multiple result sets to evaluate an expression {#section_ojz_4td_yfb .section}

-   **Syntax**

    You can associate multiple charts with an alarm. The system will then obtain multiple query results to evaluate the condition expression that you set. In this case, you must prefix the variables of your condition expression. Then the system can determine from which query result to obtain the corresponding values of the variables when evaluating your expression. The format of the variables is **$N.fieldname**, where N indicates the order number of a query. You can configure up to three queries. The value range of N is 0 to 2. For example, $0.foo indicates the foo filed of the first query. If you configure only one query, no prefix is required.

-   **Sequence numbers of charts associated with an alarm**

    The **Associated Chart** area on the Create Alert page provides a sequence number \(0, 1, or 2\) for each of the charts to be associated with the alarm. These numbers are based on the order in which these three charts are associated with the alarm chronologically.

    ![](http://static-aliyun-doc.oss-cn-hangzhou.aliyuncs.com/assets/img/65186/155532295739405_en-US.png)

-   **Evaluate an expression**

    If multiple query results are returned, the system determines which query result to use to evaluate your expression according to the variables set in your expression. For example, if you configure three queries, the number of results returned by the queries are x, y, and z, and your expression is $0.foo \> 100 && $1.bar < 100. In this case, only the first two result sets are needed to evaluate the expression. The system will evaluate your expression for **x\*y** times until the true value is returned, or continue calculating until the limit of calculation attempts is reached and the false value is returned. The maximum limit of calculation attempts is 1000.


## Operation methods {#section_iw3_b5d_yfb .section}

**Note:** 

-   Numbers used in operations are 64-bit floating-point numbers.
-   Each string constant must be enclosed in single quotation marks or double quotation marks, for example, 'string', and "string".
-   Boolean values include true and false.

|Operator|Operation method|
|Operation on two variables|Operation on a non-string constant and a variable|Operation on a string constant and a variable|
|:-------|:---------------|
|:-------------------------|:------------------------------------------------|:--------------------------------------------|
|Arithmetic operations \(+-\*/%\)|The left and right values are converted to numbers to be used in an operation.|Not supported.|
|Comparison operations:Greater than \(\>\), greater than or equal to \(\> =\), less than \(<\), less than or equal to \(<=\), equal to \(==\), and not equal to \(! =\)

|The order of operation precedence is as follows:1.  The left and right values are converted to numbers and then used in an operation in the numeric value order. If the left and right values fail to be converted, then they are used in an operation of a lower precedence.
2.  The left and right values are used as string-type values in an operation of the lexicographic order.

|The left and right values are converted to numbers to be used in an operation of the numeric order.|The left and right values are used as string-type values in an operation of the lexicographic order.|
|Whether a regular expression is matched:regular match \(= ~\), regular not match \(! ~\)

|The left and right values are used as string-type values in an operation.|Not supported.|The left and right values are used as string-type values in an operation.|
|Logical operations:and \(&&\) and or \(||\)

|These two operators cannot be directly used on the query result fields. The left and right values must both be sub-expressions, and the operation results are of both the bool type.|
|Not operator \(!\)|This operator cannot be directly used on the query result fields. The inverted value must be a sub-expression and the operation result is of the bool type.|
|String lookup function \(contains\)|The left and right values are converted to the string-type values to participate in an operation.|Not supported.|The left and right values are used as string-type values in an operation.|
|Parentheses \(\)|Determine the order of operation precedence.|

