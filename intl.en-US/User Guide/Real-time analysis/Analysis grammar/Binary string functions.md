# Binary string functions {#concept_nbt_dmq_zdb .concept}

The binary string type varbinary is different from the string type varchar.

|Statement|Description|
|:--------|:----------|
|Connection function |||The result of `a || b`  is `ab`.|
|length\(binary\) → bigint|Returns the length in binary.|
|concat\(binary1, …, binaryN\) → varbinary|Connect the binary strings, which is equivalent to ||.|
|to\_base64\(binary\) → varchar|Convert a binary string to a Base64 string.|
|from\_base64\(string\) → varbinary|Convert a Base64 string to a binary string.|
|to\_base64url\(binary\) → varchar|Convert a string to a URL-safe Base64 string.|
|from\_base64url\(string\) → varbinary| Convert a URL-safe Base64 string to a binary string.|
|to\_hex\(binary\) → varchar| Convert a binary string to a hexadecimal string.|
|from\_hex\(string\) → varbinary|Convert a hexadecimal string to a binary string.|
|to\_big\_endian\_64\(bigint\) → varbinary|Convert a number to a binary string in big endian mode.|
|from\_big\_endian\_64\(binary\) → bigint|Convert a binary string in big endian mode to a number.|
|md5\(binary\) → varbinary|Calculate the MD5 value of a binary string.|
|sha1\(binary\) → varbinary|Calculate the SHA1 value of a binary string.|
|sha256\(binary\) → varbinary|Calculate the SHA256 hash value of a binary string.|
|sha512\(binary\) → varbinary|Calculate the SHA512 value of a binary string.|
|xxhash64\(binary\) → varbinary|Calculate the xxhash64 value of a binary string.|

