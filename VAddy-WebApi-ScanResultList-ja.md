VAddy Web API Scan Result List Document
============================

Document Version 2.0.1  

VAddy WebAPI 検査結果一覧取得の仕様書です。


## 検査結果一覧の取得
プロジェクト単位に検査結果一覧を取得するリクエスト。結果には検査中のデータは含まれません。  
本APIはVAddyのV1/V2全てのプロジェクトが対象です。

### リクエスト
https://api.vaddy.net/v2/scan/results/list  
Method : GET  

    user=vaddyuser
    auth_key=123456
    project_number=123

検索結果は1回で最大20件取得できます。結果は最新順（降順）になります。  

`auth_key`は、ユーザ毎に発行する認証キーです。VAddyログイン後のWebAPI管理画面にて取得してください。  
管理画面のログインIDを`user`パラメータに、API Auth Keyを`auth_key`パラメータにセットしてください。  
`auth_key` はHTTPヘッダーの `X-API-KEY` に指定することもできます。


`project_number`は、プロジェクト単位で自動付与される数字の番号です。  
https://console.vaddy.net/project/123  
のように各プロジェクト画面のURLの末尾にある数字がproject_numberです。  
project_idとは異なりますのでご注意ください。  


#### リクエスト例 (最新の20件を取得)：

    https://api.vaddy.net/v2/scan/results/list?user=vaddyuser&auth_key=123456&project_number=123



### レスポンス
#### 成功
ステータスコード : 200  
content-type  : application/json  
コンテンツ:

    {"project_number":123,
     "page":1,
     "limit":20,
     "items":[
        {"scan_id":"123-aaaaa-bbbbb-ccccc",
         "status":"Complete",
         "scan_count":26,
         "alert_count":1,
         "complete":100,
         "crawl_id":221,
         "crawl_label":null,
         "time_zone":"UTC",
         "start_time":"2019-12-02T04:07:37+0000",
         "end_time":"2019-12-02T04:07:42+0000",
         "scan_result_url":"https://console.vaddy.net/scan_status/123/123-aaaaa-bbbbb-ccccc"},
        {"scan_id":"123-eeeee-fffff-ggggg",
         "status":"Cancel",
         "scan_count":null,
         "alert_count":null,
         "complete":null,
         "crawl_id":221,
         "crawl_label":null,
         "time_zone":"UTC",
         "start_time":"2020-01-30T02:05:24+0000",
         "end_time":"2020-01-30T02:05:27+0000",
         "scan_result_url":"https://console.vaddy.net/scan_status/123/123-eeeee-fffff-ggggg"}
     ]
    }

結果は見やすくするため改行をいれています。  
statusは、Complete, Cancelの2つです。Completeは検査完了、Cancelはキャンセルです。  
開始・終了時刻はUTCで、ISO 8601 timestamp形式です。  
alert_countは脆弱性件数、scan_countは検査リクエスト数、completeは検査完了割合(検査の時間切れの場合に100未満になります)、scan_result_urlは検査結果詳細画面のURLになります。




#### 成功 (結果0件)
ステータスコード : 200  
content-type  : application/json  
コンテンツ:

    {"project_number":123,
     "page":1,
     "limit":20,
     "items":[]
    }


#### 認証エラー
ステータスコード : 401  Unauthorized  

#### 失敗
ステータスコード : 400  
content-type  : application/json  
コンテンツ:

    {"error_message":"not match the Project Number"}
