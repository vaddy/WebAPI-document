VAddy Web API Crawl Document
============================

Document Version 1.0.1  

VAddy WebAPI Crawl仕様書です。
本仕様では、VAddyのクロール情報の取得を定義します。

## クロール一覧情報取得
クライアントからWebAPIに送信するクロール一覧取得リクエスト。  
取得対象はクロール記録完了済みのものに限ります。クロール途中のもの、クロールキャンセルのものは対象外となります。


### リクエスト
https://api.vaddy.net/v1/crawl  
Method : GET  

    user=vaddyuser
    auth_key=123456
    fqdn=www.example.jp
    search_label=all-crawl (optional)
    page=1 (optional)
    sort=desc (optional)
    
page, sort, search_labelはオプション項目です。検索結果は1回で最大30件取得できます。  

- pageはページ番号を数字で指定します。指定をしない場合はデフォルトは1です。  
- sortはasc(昇順)もしくはdesc(降順)を指定します。指定しない場合はデフォルトはdescです。
- search_labelを付けない場合は全てのクロール情報を対象に一覧を取得します。search_labelを付けると、指定のキーワードで部分一致した検索結果を返します。

auth_keyは、ユーザ毎に発行する認証キーです。VAddyログイン後のWebAPI管理画面にて取得してください。  
管理画面のUser Idをuserパラメータに、API Auth Keyをauth_keyパラメータにセットしてください。  


#### リクエスト例 (最新の30件を取得)：

    https://api.vaddy.net/v1/crawl?user=vaddyuser&auth_key=123456&fqdn=www.example.com

#### リクエスト例 (検索条件追加)：

    https://api.vaddy.net/v1/crawl?user=vaddyuser&auth_key=123456&fqdn=www.example.com&search_label=all-data


### レスポンス
#### 成功
ステータスコード : 200  
content-type  : application/json  
コンテンツ:

    {"total":3,
     "page":1,
     "limit":30,
     "items":[
        {"id":2, "label": "all-data", "start":"2016-03-12T11:11:11+0000", "end":"2016-03-12T12:00:00+0000"}, 
        {"id":1, "label": null, "start":"2016-03-12T11:11:11+0000", "end":"2016-03-12T12:00:00+0000"}
     ]
    }

結果は見やすくするため改行をいれています。

- total: 検索結果の全件数
- page: 現在の表示ページ数
- limit: 1ページの最大表示件数
- items: クロール情報の一覧

クロール開始・終了時刻はUTCで、ISO 8601 timestamp形式。


#### 成功 (結果0件)
ステータスコード : 200  
content-type  : application/json  
コンテンツ:

    {"total":0,
     "page":1,
     "limit":30,
     "items":[]
    }


#### 認証エラー
ステータスコード : 401  Unauthorized  

#### 失敗
ステータスコード : 400  
content-type  : application/json  
コンテンツ:

    {"error_message":"xxxxxx"}


存在しないfqdnなど。


