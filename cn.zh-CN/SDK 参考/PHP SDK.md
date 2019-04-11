# PHP SDK {#reference_ahs_2wq_12b .reference}

## 下载地址 {#section_o4l_ptf_jdb .section}

该 SDK GitHub 地址如下：[https://github.com/aliyun/aliyun-log-php-sdk](https://github.com/aliyun/aliyun-log-php-sdk)。

## 操作步骤 {#section_ylg_r1r_12b .section}

为快速开始使用 Log Service PHP SDK，请按照如下步骤进行。

## 步骤 1 创建阿里云账号 {#section_zlg_r1r_12b .section}

具体方法请参考 [阿里云账号注册流程](https://www.alibabacloud.com/help/zh/doc-detail/50482.htm)。

## 步骤 2 获取阿里云访问密钥 {#section_amg_r1r_12b .section}

为了使用 Log Service PHP SDK，您必须申请阿里云的 [访问秘钥](../../../../../intl.zh-CN/API 参考/访问秘钥.md)。

登录阿里云[秘钥管理页面](https://ak-console.aliyun.com/#/accesskey) 。选择一对用于 SDK 的访问密钥对。如果没有，请创建一对新访问密钥，且保证它处于**启用**状态。有关如何创建访问密钥，参见 [准备流程](../../../../../intl.zh-CN/用户指南/准备工作/准备流程.md)。

该密钥对会在下面的步骤使用，且需要保管好，不能对外泄露。另外，您可以参考 [配置](intl.zh-CN/SDK 参考/基本介绍/配置.md) 了解更多 SDK 如何使用访问密钥的信息。

## 步骤 3 创建日志服务项目和日志库 {#section_zrq_dbr_12b .section}

在使用日志服务PHP SDK之前，请先在控制台上创建好项目（Project）和日志库（Logstore）。

有关如何创建Project和Logstore，参见[准备流程](../../../../../intl.zh-CN/用户指南/准备工作/准备流程.md)。

**说明：** 

-   请确保使用同一阿里云账号获取阿里云访问密钥和创建日志项目及日志库。
-   关于日志的项目、日志库等概念请参考 Log [基本概念](../../../../../intl.zh-CN/产品简介/基本概念/简介.md#)。
-   Log 的 Project 名称为日志服务全局唯一，而 Logstore 名称在一个 Project 下面唯一。
-   Log 的 Project 一旦创建则无法更改它的所属区域。目前也不支持在不同阿里云 Region 间迁移 Log Project。

## 步骤 4 安装 PHP 开发环境 {#section_d1f_wdj_sy .section}

PHP SDK 支持 PHP 5.2.1 及以上版本，您可以在本地安装 SDK 所支持的任何 PHP 版本并搭建好相应的 PHP 开发环境。

## 步骤 5 下载并安装 PHP SDK {#section_fvz_wdj_sy .section}

搭建好 PHP 开发环境后，您需要安装 PHP SDK。具体如下：

1.  从 [GitHub](https://github.com/aliyun/aliyun-log-php-sdk) 下载最新的 PHP SDK 包。
2.  解压完整下载的包到指定的目录即可。PHP SDK 是一个软件开发包，不需要额外的安装操作。在个软件开发包除了包括 SDK 自身的代码外，还有一组第三方依赖包和一个 autoloader 类帮助你简化使用流程。您可以按照下面的步骤直接在自己的 PHP 项目中使用。

## 步骤 6 开始一个新的 PHP 项目 {#section_rv2_ydj_sy .section}

在，您可以开始使用 SDK PHP SDK。使用任何文本编辑器或者 PHP IDE，运行如下示例代码即可与日志服务端交互并得到相应输出。

```
<?php
/* 使用 autoloader 类自动加载所有需要的 PHP 模块。注意使用合适的路径指向 autoloader 类所在文件*/
require_once realpath(dirname(__FILE__) . '/../Log_Autoload.php');
$endpoint = 'cn-hangzhou.sls.aliyuncs.com'; // 选择与上面步骤创建 project 所属区域匹配的 Endpoint
$accessKeyId = 'your_access_key_id';        // 使用你的阿里云访问秘钥 AccessKeyId
$accessKey = 'your_access_key';             // 使用你的阿里云访问秘钥 AccessKeySecret
$project = 'your_project';                  // 上面步骤创建的项目名称
$logstore = 'your_logstore';                // 上面步骤创建的日志库名称
$client = new Aliyun_Log_Client($endpoint, $accessKeyId, $accessKey);
#列出当前 project 下的所有日志库名称
$req1 = new Aliyun_Log_Models_ListLogstoresRequest($project);
$res1 = $client->listLogstores($req1);
var_dump($res1);
#创建 logstore
$req2 = new Aliyun_Log_Models_CreateLogstoreRequest($project,$logstore,3,2);
$res2 = $client -> createLogstore($req2);
#等待 logstore 生效
sleep(60);
#写入日志
$topic = "";
$source = "";
$logitems = array();
for ($i = 0; $i < 5; $i++)
{
    $contents = array('index1'=> strval($i));
    $logItem = new Aliyun_Log_Models_LogItem();
    $logItem->setTime(time());
    $logItem->setContents($contents);
    array_push($logitems, $logItem);
}
$req2 = new Aliyun_Log_Models_PutLogsRequest($project, $logstore, $topic, $source, $logitems);
$res2 = $client->putLogs($req2);
var_dump($res2);
#不必等待，立即拖数据
#首先遍历有哪些 shardId
$listShardRequest = new Aliyun_Log_Models_ListShardsRequest($project,$logstore);
$listShardResponse = $client -> listShards($listShardRequest);
foreach($listShardResponse-> getShardIds()  as $shardId)
{
    #对每一个 ShardId，先获取 Cursor
    $getCursorRequest = new Aliyun_Log_Models_GetCursorRequest($project,$logstore,$shardId,null, time() - 60);
    $response = $client -> getCursor($getCursorRequest);
    $cursor = $response-> getCursor();
    $count = 100;
    while(true)
    {
        #从 cursor 开始读数据
        $batchGetDataRequest = new Aliyun_Log_Models_BatchGetLogsRequest($project,$logstore,$shardId,$count,$cursor);
        var_dump($batchGetDataRequest);
        $response = $client -> batchGetLogs($batchGetDataRequest);
        if($cursor == $response -> getNextCursor())
        {
            break;
        }
        $logGroupList = $response -> getLogGroupList();
        foreach($logGroupList as $logGroup)
        {
            print ($logGroup->getCategory());
            foreach($logGroup -> getLogsArray() as $log)
            {
                foreach($log -> getContentsArray() as $content)
                {
                    print($content-> getKey().":".$content->getValue()."\t");
                }
                print("\n");
            }
        }
        $cursor = $response -> getNextCursor();
    }
}
#等待 1 分钟让日志可查询
sleep(60);
#查询日志分布情况询（注意，要查询日志，必须保证已经创建了索引，PHP SDK 不提供该接口，请在控制台创建）
$topic = "";
$query='';
$from = time()-3600;
$to = time();
$res3 = NULL;
while (is_null($res3) || (! $res3->isCompleted()))
{
    $req3 = new Aliyun_Log_Models_GetHistogramsRequest($project, $logstore, $from, $to, $topic, $query);
    $res3 = $client->getHistograms($req3);
}
var_dump($res3);
#查询日志数据
$res4 = NULL;
while (is_null($res4) || (! $res4->isCompleted()))
{
    $req4 = new Aliyun_Log_Models_GetLogsRequest($project, $logstore, $from, $to, $topic, $query, 5, 0, False);
    $res4 = $client->getLogs($req4);
}
var_dump($res4);
```

