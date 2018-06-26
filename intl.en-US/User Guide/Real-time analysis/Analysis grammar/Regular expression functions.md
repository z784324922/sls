# Regular expression functions {#concept_v5n_nlq_zdb .concept}

A regular expression function parses a string and returns the needed substrings.

The common regular expression functions and the meanings are as follows:

|Function name|Meaning|Example|
|:------------|:------|:------|
|`regexp_extract_all(string, pattern)`|Returns all the substrings that match the regular expression in the string as a string array.|`|*SELECT regexp_extract_all('5a 67b 890m',   '\d+')`, results in `['5','67','890']`，`*|SELECT regexp_extract_all('5a 67a 890m', '(\d+)a')`returns `['5a','67a']`.|
|`regexp_extract_all(string, pattern, group)`|Returns the part of the string that hits the regular \(\) part of the group, returns the result as an array of strings.|`*| ` SELECT regexp_extract_all(‘5a 67a 890m’, ‘(\d+)a’,1)`returns `[‘5’,’67’]`|
|`regexp_extract(string, pattern)`|Returns the first substring that matches the regular expression in the string.|`*|SELECT regexp_extract('5a 67b 890m', '\d+')`returns `'5'`|
|`regexp_extract(string, pattern,group)`|Returns the first substring within the regular group \(\) that hit the string.|`*|SELECT regexp_extract('5a 67b 890m', '(\d+)([a-z]+)',2)`returns `'b'`|
|`regexp_like(string, pattern)`|Determines if the string matches the regular expression and returns a bool result. The regular expression is allowed to match part of the string.|`*|SELECT regexp_like('5a 67b 890m', '\d+m')`returns true|
|`regexp_replace(string, pattern, replacement)`|Replaces the part that matches the regular expression in the string with replacement.|`*|SELECT regexp_replace('5a 67b 890m', '\d+','a')`returns `'aa ab am'`|
|`regexp_replace(string, pattern)`|Removes the part that matches the regular expression in the string, which is equivalent to `regexp_replace(string,patterm,'')`.|`*|SELECT regexp_replace('5a 67b 890m', '\d+')`returns `'a b m'`|
|`regexp_split(string, pattern)`|Splits the string to an array by using the regular expression.|`*|SELECT regexp_split('5a 67b 890m', '\d+')`returns `['a','b','m']`|

