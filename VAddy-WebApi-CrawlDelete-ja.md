VAddy Web API Crawl Delete Document
============================

Document Version 2.0.0  

VAddy WebAPI クロールデータ削除の仕様書です。


## クロールデータ削除
クロールIDを指定してクロールレデータを削除するAPIです。  
本APIはVAddyのV1/V2全てのプロジェクトが対象です。

クロールステータスが`Recorded`、`Cancel`のものが削除可能です。`Crawling`のものは削除できません。

### リクエスト
https://api.vaddy.net/v2/crawl/delete  
Method : POST  

    user=vaddyuser
    auth_key=123456
    project_number=123
    crawl_id=2


auth_keyは、ユーザ毎に発行する認証キーです。VAddyログイン後のWebAPI管理画面にて取得してください。  
管理画面のUser Idをuserパラメータに、API Auth Keyをauth_keyパラメータにセットしてください。  

project_numberは、プロジェクト単位で自動付与される数字の番号です。  
https://console.vaddy.net/project/123  
のように各プロジェクト画面のURLの末尾にある数字がproject_numberです。  
project_idとは異なりますのでご注意ください。  

crawl_idはVAddy管理画面のクロール一覧画面のテーブルの`Crawl ID`列にある数字です。

#### リクエスト例 (crawl_id 88 を削除)：

    curl 'https://api.vaddy.net/v2/crawl/delete' \
    -X POST -d 'user=foo&auth_key=xxxxx&project_number=99999&crawl_id=88'



### レスポンス
#### 削除成功
ステータスコード : 200  
content-type  : application/json  
コンテンツ:

    {"message":"ok"}


#### 認証エラー
ステータスコード : 401  Unauthorized  

#### 失敗（該当crawl_idが見つからない）
ステータスコード : 404  
content-type  : application/json  
コンテンツ:

    {"error_message":"crawl_id not found."}

#### 失敗
ステータスコード : 400  
content-type  : application/json  
コンテンツ:

    {"error_message":"not match project"}
