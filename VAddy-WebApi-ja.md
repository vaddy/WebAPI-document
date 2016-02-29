VAddy Web API Document
======================

Document Version 1.0.1  

VAddy WebAPI仕様書です。
本仕様では、VAddyのスキャン開始、スキャンキャンセル、スキャン結果の取得の3つを定義します。

## Scan開始
クライアント（Jenkins等)からWebAPIに送信するスキャン開始リクエスト。

### Scan開始リクエスト
https://api.vaddy.net/v1/scan  
Method : POST  

    action=start
    user=vaddyuser
    auth_key=123456
    fqdn=www.example.jp
    crawl_id=30

crawl_idはオプション項目です。指定がない場合は最新のクロールデータを利用して検査します。クロールIDの値は、管理画面にログインし、Proxy Crawling画面からご確認ください。

auth_keyは、ユーザ毎に発行する認証キーです。VAddyログイン後のWebAPI管理画面にて取得してください。  
管理画面のUser Idをuserパラメータに、API Auth Keyをauth_keyパラメータにセットしてください。  


### Scan開始レスポンス
#### 成功
ステータスコード : 200  
content-type  : application/json  
コンテンツ:

    {"scan_id": "xxxxxxxxxxxx"}

scan_idの値は毎回異なり、スキャン結果を取得するために必要となるキーです。  


#### 認証エラー
ステータスコード : 401  Unauthorized  

#### 失敗
ステータスコード : 400  
content-type  : application/json  
コンテンツ:

    {"error_message":"xxxxxx"}


- 存在しないfqdn
- 前回のスキャンが終了しておらず開始できない
- クロールデータが1件も存在しない

----------------------------------------------------------------------------------

## Scanキャンセル
クライアント（Jenkins等)からWebAPIに送信するスキャンキャンセルリクエスト。

### Scanキャンセルリクエスト
https://api.vaddy.net/v1/scan  
Method : POST  

    action=cancel
    user=vaddyuser
    auth_key=123456
    scan_id=xxxxxxxxxxxx
    fqdn=www.example.jp

scan_idは、スキャンを開始すると発行されるIDです。  


### Scanキャンセルレスポンス
#### 成功
ステータスコード : 200  
content-type  : application/json  
コンテンツ:

    {"scan_id": "xxxxxxxxxxxx"}


#### 認証エラー
ステータスコード : 401  Unauthorized  

#### 失敗
ステータスコード : 400  
content-type  : application/json  
コンテンツ:

    {"error_message":"xxxxxx"}

- 存在しないfqdn
- 存在しないscan_id
- 既に処理が終わっていてキャンセルできない
- 不測の事態によりキャンセルできない


----------------------------------------------------------------------------------


## Scan結果取得

クライアント（Jenkins等)からWebAPIに送信するスキャン結果を取得するリクエスト

### Scan結果リクエスト
https://api.vaddy.net/v1/scan/result  
Method : GET  

    user=vaddyuser
    auth_key=123456
    fqdn=www.example.jp
    scan_id=xxxxxxxxxxxx


scan_idはScan開始リクエストのレスポンスに含まれているものをセットしてください  


### Scan結果レスポンス
#### スキャン中
ステータスコード : 200  
content-type  : application/json  
コンテンツ:

    {"status":"scanning"}

#### キャンセル、スキャン異常終了
ステータスコード : 200  
content-type  : application/json  
コンテンツ:

    {"status":"canceled"}

#### 成功
ステータスコード : 200  
content-type  : application/json  
コンテンツ:

    { "status":"finish",
      "fqdn" : "www.example.com",
      "scan_id" : "1-837b5f9f-e088-4af5-9491-67f7ce8035a4",
      "scan_count" : 22,
      "alert_count" : 1,
      "timezone" : "UTC",
      "start_time" :  "2014-06-12T11:11:11+0000",
      "end_time" :  "2014-06-12T11:11:11+0000",
      "scan_result_url" : "https://console.vaddy.net/scan_status/1/1-837b5f9f-e088-4af5-9491-67f7ce8035a4",
      "complete" : 100,
      "crawl_id" : 30,
      "scan_list" : ["XSS","SQL Injection"]
    }

時刻はUTCで、ISO 8601 timestamp形式。


#### 認証エラー
ステータスコード : 401  Unauthorized  

#### 失敗
ステータスコード : 400  
content-type  : application/json  
コンテンツ:

    {"error_message":"xxxxxx"}

- 存在しないfqdn
- 存在しないscan_id

