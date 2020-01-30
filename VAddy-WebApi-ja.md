VAddy Web API Scan Document
======================

Document Version 2.0.0

VAddy WebAPI仕様書です。
本仕様では、VAddyのスキャン開始、スキャンキャンセル、スキャン結果の取得の3つを定義します。

## Scan開始
クライアント（Jenkins等)からWebAPIに送信するスキャン開始リクエスト。

### Scan開始リクエスト
https://api.vaddy.net/v2/scan  
Method : POST  

    action=start
    user=vaddyuser
    auth_key=123456
    project_id=6eb1f9fcbdb6a5a
    crawl_id=30

crawl_idはオプション項目です。指定がない場合は最新のクロールデータを利用して検査します。クロールIDの値は、管理画面にログインし、Proxy Crawling画面からご確認ください。

auth_keyは、ユーザ毎に発行する認証キーです。VAddyログイン後のWebAPI管理画面にて取得してください。  
管理画面のUser Idをuserパラメータに、API Auth Keyをauth_keyパラメータにセットしてください。  

project_idは、検査対象サーバを管理するID。 Server画面にProjectIDとして表示されます。

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


- 存在しないproject_id
- 前回のスキャンが終了しておらず開始できない
- クロールデータが1件も存在しない

----------------------------------------------------------------------------------

## Scanキャンセル
クライアント（Jenkins等)からWebAPIに送信するスキャンキャンセルリクエスト。

### Scanキャンセルリクエスト
https://api.vaddy.net/v2/scan  
Method : POST  

    action=cancel
    user=vaddyuser
    auth_key=123456
    scan_id=xxxxxxxxxxxx

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

- 存在しないproject_id
- 存在しないscan_id
- 既に処理が終わっていてキャンセルできない
- 不測の事態によりキャンセルできない


----------------------------------------------------------------------------------


## Scan結果取得

クライアント（Jenkins等)からWebAPIに送信するスキャン結果を取得するリクエスト

### Scan結果リクエスト
https://api.vaddy.net/v2/scan/result  
Method : GET  

    user=vaddyuser
    auth_key=123456
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
      "project_id" : "6eb1f9fcbdb6a5a"
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
alert_countは、発見された脆弱性の件数になります。

#### 認証エラー
ステータスコード : 401  Unauthorized  

#### 失敗
ステータスコード : 400  
content-type  : application/json  
コンテンツ:

    {"error_message":"xxxxxx"}

- 存在しないproject_id
- 存在しないscan_id


----------------------------------------------------------------------------------


## Scan実行中の確認

VAddyでは、同一プロジェクトに対する検査の同時実行は行えません。  
現在、検査が実行中かどうか把握するリクエスト。

### 検査実行チェックリクエスト
https://api.vaddy.net/v2/scan/runcheck  
Method : GET  

    user=vaddyuser
    auth_key=123456
    project_id=6eb1f9fcbdb6a5a


### レスポンス

#### スキャンが実行中でない場合
この場合は、スキャン開始リクエスト送信すると、検査開始されます。  

ステータスコード : 200  
content-type  : application/json  
コンテンツ:

    {"running_process": 0}

検査実行中のプロセスが0件という意味です。  


#### スキャン中
スキャン実行中の場合のレスポンス  
この場合は、スキャン開始リクエスト送信してもエラーが返ってきます。  
running_process : 0になるまで一定間隔で確認をしてください。  

ステータスコード : 200  
content-type  : application/json  
コンテンツ:

    {"running_process": 1}
