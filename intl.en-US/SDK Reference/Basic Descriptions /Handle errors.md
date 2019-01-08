# Handle errors {#reference_jpz_bwq_12b .reference}

Possible SDK errors are classified as follows:

-   Errors returned by the Log Service. This type of errors is returned by the Log Service and handled by SDKs. For more information about this error type, see the [Common error codes](../../../../../reseller.en-US/API Reference/Common error codes.md) of the Log Service APIs and the descriptions of each API.
-   Network errors that occur when SDKs send requests to the Log Service. This type of errors includes network interruptions and Log Service return timeout.
-   Errors that are produced by SDKs and related to platforms or languages, for example, memory overflow.

Currently, the SDKs in various languages handle errors by throwing exceptions. The specific principles are as follows:

-   The first and second types of errors are encapsulated as the LogException class and thrown to users by SDKs.
-   The third type of errors is not handled by SDKs, but is thrown to users as the platform- and language-specific Native Exception class.

## LogException {#section_nyz_jsh_sy .section}

The LogException class is defined by SDKs to handle the logical errors of the Log Service. It inherits the basic exception classes from each language and provides the following exception information:

-   Error code: Indicates the error type. For errors returned by Log Service, the error code is the same as that returned by APIs. For network errors of SDK requests, the error code is  "Requesterror ". For more information, see the complete API reference of each language.
-   Error message: Indicates the message that comes with an error. For errors returned by Log Service, the error message is the same as that returned by APIs. For network errors of SDK  requests, the error message is “request is failed”.  For more information, see the complete API reference of each language.
-   Request ID: Indicates the request ID in Log Service that corresponds to the current error. This ID is valid only when Log Service returns an error message. Otherwise, it is an empty string. When a request error occurs, you can provide the request ID  to the Log Service team to troubleshoot the problem.

## Request failure and retry {#section_ogy_lsh_sy .section}

When you use SDKs   to access Log Service, the request may fail because of temporary network interruptions, transmission delay, and slow processing in Log Service.  Currently, these errors are directly thrown as exceptions and the Log Service does not implement any retry logic internally. Therefore, you must define the processing logic \(retry the request or directly report an error\) when using SDKs.

## Example  {#section_fqt_msh_sy .section}

Assume that you want to access the project  **big-game**  in the region China East 1 \(Hangzhou\) and retry the request for the specified number of times when a network exception occurs. The code snippets in various languages are as follows: The code snippets in various languages are as follows:

**Java:**

```
//Other code...
String accessId = "your_access_id"; //TODO: Use your Alibaba Cloud AccessKey ID.
String accessKey = "your_access_key"; //TODO: Use your Alibaba Cloud AccessKey Secret.
String project = "big-game";
String endpoint = "cn-hangzhou.sls.aliyuncs.com";
int max_retries = 3;
/*
 * Construct a client
 */
Client client = new client (adord, accesskey, endpoint );
ListLogStoresRequest lsRequest = new ListLogStoresRequest(project);
for (int i = 0; i < max_retries; i++)
{
    try 
    {
        ListLogStoresResponse res = client.ListLogStores(lsRequest)
        //TODO: Process the returned response...
        break;
    }
    catch(LogException ex)
    {
        if (e.GetErrorCode() == "RequestError")
        {
            if ( i == max_retries - 1)
            {
                System.out.println("request is still failed after all retries.");
                break;
            }
            else
                System.out.println("request error happens, retry it!") ;
        }
        else
        {
            System.out.println("error code :" + e.GetErrorCode());
            System.out.println("error message :" + e.GetErrorMessage());
            System.out.println("error requestId :" + e.GetRequestId());
            break;
        }
    }
    catch(...)
    {
        System.out.println("unrecoverable exception when listing logstores.");
        break;
    }
}
//Other code...
```

**. NET\(C\#\):**

```
//Other code...
String accessId = "your_access_id"; //TODO: Use your Alibaba Cloud AccessKey ID.
String accessKey = "your_access_key"; //TODO: Use your Alibaba Cloud AccessKey Secret.
String project = "big-game";
String endpoint = "cn-hangzhou.sls.aliyuncs.com";
int max_retries = 3;
//Construct a client
SLSClient client = new SLSClient(endpoint, accessId, accessKey);
ListLogstoresRequest request = new ListLogstoresRequest();
request.Project = project;
for (int i = 0; i < max_retries; i++)
{
    try
    {
        ListLogstoresResponse response = client.ListLogstores(request);
        //TODO: Process the returned response...
        break;
    }
    catch(LogException ex)
    {
        if (e.errorCode == "SLSRequestError")
        {
            if ( i == max_retries - 1)
            {
                Console.Writeline(“request is still failed after all retries.”);
                break;
            }
            else
            {
                Console.Writeline("request error happens, retry it!") ;
            }
        }
        else
        {
            Console.Writeline("error code :" + e.errorCode;
            Console.Writeline("error message :" + e.Message;
            Console.Writeline("error requestId :" + e.RequestId;
            break;
        }
    }
    catch(...)
    {
        Console.Writeline("unrecoverable exception when listing logstores.");
        break;
    }
}
//Other code...
```

**PHP:**

```
<? php
//Other code...
$endpoint = 'cn-hangzhou.sls.aliyuncs.com';
$accessId = 'your_access_id'; // TODO：Use your Alibaba Cloud AccessKey ID.
$accessKey = 'your_access_key'; //TODO: Use your Alibaba Cloud AccessKey Secret.
$maxRetries = 3;
// Build a client.
$client = new Aliyun_Sls_Client($endpoint, $accessId, $accessKey);
$project = 'big-game';
$request = new Aliyun_Sls_Models_ListLogstoresRequest($project);
for($i = 0; $i < $maxRetries; ++$i)
{
    try
    {
        $response = $client->ListLogstores($request);
        //TODO: Process the returned response...
        break;
    }
    catch (Aliyun_Sls_Exception $e)
    {
        if ($e->getErrorCode()=='RequestError')
        {
            if ($i+1 == $maxRetries)
            {
                echo "error code :" . $e->getErrorCode() . PHP_EOL;
                echo "error message :" . $e->getErrorMessage() . PHP_EOL;
                break;
            }
            echo 'request error happens, retry it!' . PHP_EOL;
        }
        else
        {
            echo "error code :" . $e->getErrorCode() . PHP_EOL;
            echo "error message :" . $e->getErrorMessage() . PHP_EOL;
            echo "error requestId :" . $e->getRequestId() . PHP_EOL;
            break;
        }
    }
    catch (Exception $ex)
    {
        echo 'unrecoverable exception when listing logstores.' . PHP_EOL;
        var_dump($ex);
        break;
    }
}
//Other code...
```

**Python:**

```
//Other code...
endpoint = 'cn-hangzhou.sls.aliyuncs.com'
accessId = 'your_access_id' # TODO: Use your Alibaba Cloud AccessKey ID.
$accessKey = 'your_access_key'; //TODO: Use your Alibaba Cloud AccessKey Secret.
maxRetries = 3
# Construct a client
client = Client(endpoint, accessId, accessKey)
project = 'big-game'
lsRequest = ListLogstoresRequest(project)
for i in xrange(maxRetries):
    try:
        res = client.ListLogstores(lsRequest)
        # TODO: Process the returned response...
        break
    except LogException as e:
        if e.getErrorCode() == "RequestError":
            if i+1 == maxRetries:
                print "error code :" + e.getErrorCode()
                print "error message :" + e.getErrorMessage()
                break
            else:
                print "request error happens, retry it!"
        else:
            print "error code :" + e.getErrorCode()
            print "error message :" + e.getErrorMessage()
            print "error requestId :" + e.getRequestId()
            break
    except Exception as e:
        print 'unrecoverable exception when listing logstores.'
        break
//Other code...
```

