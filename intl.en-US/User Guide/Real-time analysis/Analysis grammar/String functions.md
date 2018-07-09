# String functions {#LogService_user_guide_0107 .reference}

The query and analysis function of Log Service supports analyzing logs by using string functions. The specific statements and description are as follows.

|Function name| Description|
|:------------|:-----------|
|`chr(x)`|Converts the int type to the corresponding unicode string, for example chr\(65\)=’A’.|
|`length(x)`|Returns the length of a field.|
|`levenshtein_distance(string1, string2)`|Returns the minimum edit distance between two strings.|
|`lower(string)`|Converts the string to lowercase characters.|
|`lpad(string, size, padstring)`|Aligns the string to the size. If it is smaller than the size, uses padstring to fill the size from the left side. If it is larger than size, it is truncated to size.|
|`rpad(string, size, padstring)`|The same as the lpad, complement the string from the right.|
|`ltrim(string)`|Deletes the white-space characters on the left.|
|`replace(string, search)`|Deletes search from the string.|
|`replace(string, search,rep)`|Replaces search with rep in the string.|
|`reverse(string)`|Returns a string with the characters in the reverse order.|
|`rtrim(string)`|Deletes the white-space characters at the end of the string.|
|`split(string,delimeter,limit)`|Split the string into array and get a maximum of limit values. The generated result is an array with subscripts starting at 1.|
|`split_part(string,delimeter,offset)`|Splits the string into an array and obtains the offset string. The generated result is an array with subscripts starting at 1.|
|`split_to_map(string, entryDelimiter, keyValueDelimiter) → map<varchar, varchar>`|The string is divided into multiple entries according to entryDelemiter, and each entry is divided into key values according to keyValueDelimiter. Eventually returns a map.|
|`position(substring IN string)`|Get the position in the string where the substring starts.|
|`strpos(string, substring)`|Finds the starting position of the substring in the string. The returned result starts at 1. If not found, 0 is returned.|
|`substr(string, start)`|Returns a substring of a string with a subscript starting at 1.|
|`substr(string, start, length)`|Returns a substring of a string with a subscript starting at 1 and length.|
|`trim(string)`|Deletes the white-space characters at the beginning and end of the string.|
|`upper(string)`|Converts the string to uppercase characters.|
|`concat(string,string......)`|Splices two or more strings into a single string.|
|`hamming_distance (string1,string2)`|Returns the hamming distance between two strings.|

**Note:** Strings must be enclosed in single quotation marks, and double quotation marks indicate column names. For example, a=’abc’ indicates column a = string abc, and a = “abc” means column a = column abc.

