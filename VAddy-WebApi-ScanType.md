VAddy Web API Scan Type Document
======================

## Scan type option list

WebAPIから検査項目で指定できる項目は次の通りです。カンマ区切りで複数指定できます。

| VADDY_SCAN_TYPE    | 検査項目名                        | プラン                              |
| ------------------ | ------------------------------- |------------------------------------|
| SQLI               | SQL Injection                   | Advanced, Enterprise, Pro, Starter |
| BLIND_SQLI         | Blind SQL Injection             | Advanced, Enterprise               |
| XSS                | 反射型XSS                        | Advanced, Enterprise, Pro, Starter |
| XSS_STORE          | 蓄積型XSS                        | Advanced, Enterprise, Pro, Starter |
| RFI                | リモートファイルインクルージョン(RFI) | Advanced, Enterprise, Pro          |
| COMMAND_INJECTION  | コマンドインジェクション            | Advanced, Enterprise, Pro          |
| PATH_TRAV          | ディレクトリトラバーサル            | Advanced, Enterprise, Pro          |
| HEADER_INJECTION   | HTTP Header Injection           | Advanced, Enterprise               |
| XXE                | XXE                             | Advanced, Enterprise               |
| DESERIALIZATION    | 安全でないデシリアライゼーション     | Advanced, Enterprise               |
| SSRF               | SSRF                            | Advanced, Enterprise               |
| PRIVATE_FILE       | 非公開ファイル検査                 | Advanced, Enterprise               |
| CSRF               | CSRF                            | Advanced                           |
| MAIL_HEADER        | メールヘッダインジェクション         | Advanced                           |
| CLICK_JACKING      | クリックジャッキング               | Advanced                           |
| BUFFER_OVERFLOW    | バッファオーバーフロー              | Advanced                           |
| SESSION_FIXATION   | セッション管理不備                 | Advanced                           |
| ACCESS_CTL         | アクセス・認可制御不備              | Advanced                           |

## Scan type list for each plan
WebAPIやgo-vaddyツールで指定する際に、次のプラン毎の検査項目をコピーして調整してご利用ください。

### Advanced Plan
SQLI,BLIND_SQLI,XSS,XSS_STORE,RFI,COMMAND_INJECTION,PATH_TRAV,HEADER_INJECTION,XXE,DESERIALIZATION,SSRF,PRIVATE_FILE,
MAIL_HEADER,CLICK_JACKING,BUFFER_OVERFLOW,SESSION_FIXATION,ACCESS_CTL

### Enterprise Plan
SQLI,BLIND_SQLI,XSS,XSS_STORE,RFI,COMMAND_INJECTION,PATH_TRAV,HEADER_INJECTION,XXE,DESERIALIZATION,SSRF,PRIVATE_FILE

### Professional Plan
SQLI,XSS,XSS_STORE,RFI,COMMAND_INJECTION,PATH_TRAV

### Starter Plan 
SQLI,XSS,XSS_STORE

