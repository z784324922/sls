# External Store related APIs {#reference_psc_s44_12b .reference}

## Create an External Store Store {#section_iq3_nk5_12b .section}

**Request syntax**

```
POST /externalstores HTTP/1.1
'x-log-bodyrawsize': '0',
'Content-Type': 'application/json', 
'Content-Length': <ContentLength>, 
'Content-MD5': <Md5>, 
'x-log-apiversion': '0.6.0', 
'x-log-signaturemethod': 'hmac-sha1', 
'Host': <Project Endpoint> 
'Date': <GMT Date>, 
'Authorization': <AuthorizationString> 
'{"externalStoreName": "<ExternalStoreName>", 
  "storeType": "rds-vpc", 
  "parameter": {
               "vpc-id": "<vpc-id>", 
               "instance-id": "<instance-id>", 
               "host": "<host>", 
               "port": "<port>", 
               "username": "<username>", 
               "password": "<password>", 
               "db": "<db>", 
               "table": "<table>", 
               "region": "region>"
               }
}'
```

**Request Parameters**

|Attribute name|Type|Required or not|Description|
|:-------------|:---|:--------------|:----------|
|ExternalStoreName|String|Yes|The External Store name, which is unique in a project and does not conflict with the Logstore name.|
|vpc-id|String|No|The Virtual Private Cloud \(VPC\) ID of the RDS instance.|
|instance-id|String|No|The RDS instance ID.  vpc-id and instance-id can be empty at the same time, which indicates the External Store is not in the VPC environment and is directly accessible.|
|host|String|No|The host in which the RDS instance resides, which is required if both vpc-id and instance-id are left empty.|
|port|String|Yes| The RDS port.|
|username|String|Yes|The name of the user.|
|password |String|Yes|The password.|
|db|String|Yes|Name of a database.|
|Table|String|Yes|The table name.|
|region|String|Yes|The region in which the RDS instance resides, which currently only supports cn-qingdao, cn-beijing, and cn-hangzhou.|

**Request Header**

The CreateExternalStore API does not have a special request header. For more information about the public request headers of Log Service APIs, see  Public request header.

**Example**

```
POST /externalstores HTTP/1.1
'x-log-bodyrawsize': '0',
'Content-Type': 'application/json', 
'Content-Length': '307', 
'Content-MD5': '7C1D14659C0BBBA7C7BFF9E5A1A46705', 
'x-log-apiversion': '0.6.0', 
'x-log-signaturemethod': 'hmac-sha1', 
'Host': 'ali-yunlei-chengdu.cn-chengdu.log.aliyuncs.com', 
'Date': 'Thu, 19 Apr 2018 02:15:41 GMT', 
'Authorization': 'LOG LTAIDbs0FWzQtYAl:E2CeD7yIRXsZjkb+nUDoJFbGM1E='}
'{"externalStoreName": "rds_store", 
  "storeType": "rds-vpc", 
  "parameter": {
               "vpc-id": "vpc-m5eq4irc1pucpk85frr5j", 
               "instance-id": "i-m5eeo2whsnfg4kzq54ah", 
               "host": "47.104.78.128", 
               "port": "3306", 
               "username": "root", 
               "password": "sfdsfldsfksflsdfs", 
               "db": "meta", 
               "table": "join_meta", 
               "region": "cn-qingdao"
               }}'
```

## Update an External Store Store {#section_ozr_rn5_12b .section}

**Request syntax**

```
PUT /externalstores/<ExternalStoreName> HTTP/1.1
'x-log-bodyrawsize': '0',
'Content-Type': 'application/json', 
'Content-Length': <ContentLength>, 
'Content-MD5': <Md5>, 
'x-log-apiversion': '0.6.0', 
'x-log-signaturemethod': 'hmac-sha1', 
'Host': <Project Endpoint> 
'Date': <GMT Date>, 
'Authorization': <AuthorizationString> 
'{"externalStoreName": "<ExternalStoreName>", 
  "storeType": "rds-vpc", 
  "parameter": {
               "vpc-id": "<vpc-id>", 
               "instance-id": "<instance-id>", 
               "host": "<host>", 
               "port": "<port>", 
               "username": "<username>", 
               "password": "<password>", 
               "db": "<db>", 
               "table": "<table>", 
               "region": "region>"
               }
}'
```

**Request Parameters**

|Attribute name|Type|Required or not|Description|
|:-------------|:---|:--------------|:----------|
|ExternalStoreName|string|Yes|The External Store name, which is unique in a project and does not conflict with the Logstore name.|
|vpc-id|String|No|The Virtual Private Cloud \(VPC\) ID of the RDS instance.|
|intance-id|String|No|The RDS instance ID.  vpc-id and instance-id can be empty at the same time, which indicates the External Store is not in the VPC environment and is directly accessible.|
|host|String|No|The host in which the RDS instance resides, which is required if both vpc-id and instance-id are left empty.|
|port|String|Yes|The RDS port.|
|username|String|Yes|The name of the user.|
|password |String|Yes|The password.|
|db|String|Yes|Name of a database.|
|Table|String|Yes|The table name.|
|region|string|Yes|The region in which the RDS instance resides, which currently only supports cn-qingdao, cn-beijing, and cn-hangzhou.|

**Sample**

```
PUT http://ali-yunlei-chengdu.cn-chengdu.log.aliyuncs.com:80/externalstores/rds_store HTTP/1.1
'x-log-bodyrawsize': '0',
'Content-Type': 'application/json', 
'Content-Length': '307', 
'Content-MD5': '7C1D14659C0BBBA7C7BFF9E5A1A46705', 
'x-log-apiversion': '0.6.0', 
'x-log-signaturemethod': 'hmac-sha1', 
'Host': 'ali-yunlei-chengdu.cn-chengdu.log.aliyuncs.com', 
'Date': 'Thu, 19 Apr 2018 02:15:41 GMT', 
'Authorization': 'LOG LTAIDbs0FWzQtYAl:E2CeD7yIRXsZjkb+nUDoJFbGM1E='}
'{"externalStoreName": "rds_store", 
  "storeType": "rds-vpc", 
  "parameter": {
               "vpc-id": "vpc-m5eq4irc1pucpk85frr5j", 
               "instance-id": "i-m5eeo2whsnfg4kzq54ah", 
               "host": "47.104.78.128", 
               "port": "3306", 
               "username": "root", 
               "password": "sfdsfldsfksflsdfs", 
               "db": "meta", 
               "table": "join_meta", 
               "region": "cn-qingdao"
               }}'
```

## List all External Stores {#section_ivh_vn5_12b .section}

**Request syntax**

```
GET /externalstores? externalStoreName=<external_store_name_prefix>&offset=<offset>&lines=<lines>
'Content-Length': '0', 
'x-log-bodyrawsize': '0', 
'x-log-apiversion': '0.6.0', 
'x-log-signaturemethod': 
'hmac-sha1', 
'Host': '<endpoint>',
'Date': 'Thu, 19 Apr 2018 03:03:16 GMT', 
'Authorization': 'LOG LTAIDbs0FWzQtYAl:iZJXISaDEju6TJpPDf+CLOiserk='}
```

**Request Parameters**

|Attribute name|Type|Required or not|Description|
|:-------------|:---|:--------------|:----------|
|externalStoreName|String|No|Filter the External Stores containing this string.|
|The value range is 0–100 and the default value is 100.|Integer|No|The offset from which you start to obtain External Stores.|
|lines|Integer|No| The number of External Stores to be obtained.|

**Example**

```
GET http://ali-yunlei-chengdu.cn-chengdu.log.aliyuncs.com:80/externalstores?externalStoreName=&offset=0&lines=10
'Content-Length': '0', 
'x-log-bodyrawsize': '0', 
'x-log-apiversion': '0.6.0', 
'x-log-signaturemethod': 
'hmac-sha1', 
'Host': 'ali-yunlei-chengdu.cn-chengdu.log.aliyuncs.com',
'Date': 'Thu, 19 Apr 2018 03:03:16 GMT', 
'Authorization': 'LOG LTAIDbs0FWzQtYAl:iZJXISaDEju6TJpPDf+CLOiserk='}
```

**Response**

```
{'count': 3, 'externalstores': ['ecs_store', 'rds_store', 'ecs_store1'], 'total': 3}
```

## Obtain External  Store details {#section_qxr_yn5_12b .section}

```
http://ali-yunlei-chengdu.cn-chengdu.log.aliyuncs.com:80/externalstores/<external_store_name> 
'Content-Length': '0', 
'x-log-bodyrawsize': '0', 
'x-log-apiversion': '0.6.0', 
'x-log-signaturemethod': 'hmac-sha1', 
'Host': 'ali-yunlei-chengdu.cn-chengdu.log.aliyuncs.com', 
'Date': 'Thu, 19 Apr 2018 03:26:49 GMT', 
'Authorization': 'LOG LTAIDbs0FWzQtYAl:a16W7ej3c0RqdQn3Pvnq5aHWEOk='
```

**Request Parameters**

|Attribute name|Type|Required or not|Description|
|:-------------|:---|:--------------|:----------|
|external\_store\_name|String|Yes|The External Store name to be obtained.|

**Example**

```
http://ali-yunlei-chengdu.cn-chengdu.log.aliyuncs.com:80/externalstores/rds_store
'Content-Length': '0', 
'x-log-bodyrawsize': '0', 
'x-log-apiversion': '0.6.0', 
'x-log-signaturemethod': 'hmac-sha1', 
'Host': 'ali-yunlei-chengdu.cn-chengdu.log.aliyuncs.com', 
'Date': 'Thu, 19 Apr 2018 03:26:49 GMT', 
'Authorization': 'LOG LTAIDbs0FWzQtYAl:a16W7ej3c0RqdQn3Pvnq5aHWEOk='
```

**Response**

```
{
  'storeType': 'rds-vpc', 
  'parameter': {
               'region': 'cn-qingdao', 
               'vpc-id': 'vpc-m5eq4irc1pucpk85frr5j', 
               'instance-id': 'i-m5eeo2whsnfg4kzq54ah', 
               'host': '47.104.78.128', 
               'port': '3306', 
               'username': 'root', 
               'db': 'meta', 
               'table': 'join_meta'
               }
}
```

## Delete an External Store Store {#section_nq2_j45_12b .section}

```
DELETE /externalstores/<external_store_name>
'Content-Length': '0',
'x-log-bodyrawsize': '0', 
'x-log-apiversion': '0.6.0', 
'x-log-signaturemethod': 'hmac-sha1', 
'Host': 'ali-yunlei-chengdu.cn-chengdu.log.aliyuncs.com', 
'Date': 'Thu, 19 Apr 2018 03:32:49 GMT', 
'Authorization': 'LOG LTAIDbs0FWzQtYAl:7CtslFnYg+WEIt56Bsf2Xy4faw8='
```

**Request Parameters**

|Attribute name|Type|Required or not|Description|
|:-------------|:---|:--------------|:----------|
|external\_store\_name|string|Yes|The External Store name to be deleted.|

**Example**

```
 DELETE http://ali-yunlei-chengdu.cn-chengdu.log.aliyuncs.com:80/externalstores/rds_store
'Content-Length': '0',
'x-log-bodyrawsize': '0', 
'x-log-apiversion': '0.6.0', 
'x-log-signaturemethod': 'hmac-sha1', 
'Host': 'ali-yunlei-chengdu.cn-chengdu.log.aliyuncs.com', 
'Date': 'Thu, 19 Apr 2018 03:32:49 GMT', 
'Authorization': 'LOG LTAIDbs0FWzQtYAl:7CtslFnYg+WEIt56Bsf2Xy4faw8='
```

