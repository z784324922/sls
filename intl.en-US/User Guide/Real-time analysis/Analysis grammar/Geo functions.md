# Geo functions {#reference_sm3_1zx_g2b .reference}

For more information about functions that determine the country, province, city, ISP, and the longitude and latitude of specified IP addresses, see [IP functions](intl.en-US/User Guide/Real-time analysis/Analysis grammar/IP functions.md).

|Function|Description|Example|
|:-------|:----------|:------|
|geohash\(string\)|Returns the geohash value of the specified geographical coordinate. The geographical coordinate is represented by a string in the format of "latitude, longitude" \(the values for latitude and longitude are separated by a comma\). |`select geohash('34.1,120.6')= 'wwjcbrdnzs'`|
|geohash\(lat,lon\)|Returns the geohash value of the specified geographical coordinate. The geographical coordinate is represented by two separate parameters that indicate the latitude and longitude.|`select geohash(34.1,120.6)= 'wwjcbrdnzs'`|

