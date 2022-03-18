VAddy Web API Scan Type Document
======================

## Scan type option list

WebAPIから検査項目で指定できる項目は次の通りです。カンマ区切りで複数指定できます。

| VADDY_SCAN_TYPE    | 検査項目名                        | プラン                    |
| ------------------ | ------------------------------- |--------------------------|
| SQLI               | SQL Injection                   | Enterprise, Pro, Starter |
| BLIND_SQLI         | Blind SQL Injection             | Enterprise               |
| XSS                | 反射型XSS                        | Enterprise, Pro, Starter |
| XSS_STORE          | 蓄積型XSS                        | Enterprise, Pro, Starter |
| RFI                | リモートファイルインクルージョン(RFI) | Enterprise, Pro          |
| COMMAND_INJECTION  | コマンドインジェクション            | Enterprise, Pro          |
| PATH_TRAV          | ディレクトリトラバーサル            | Enterprise, Pro          |
| HEADER_INJECTION   | HTTP Header Injection           | Enterprise               |
| XXE                | XXE                             | Enterprise               |
| DESERIALIZATION    | 安全でないデシリアライゼーション     | Enterprise               |
| SSRF               | SSRF                            | Enterprise               |
| PRIVATE_FILE       | 非公開ファイル検査                 | Enterprise               |
| CSRF               | CSRF                            | 検査追加オプション          |
| MAIL_HEADER        | メールヘッダインジェクション         | 検査追加オプション          |
| CLICK_JACKING      | クリックジャッキング               | 検査追加オプション          |
| BUFFER_OVERFLOW    | バッファオーバーフロー              | 検査追加オプション          |
| SESSION_FIXATION   | セッション管理不備                 | 検査追加オプション          |
| ACCESS_CTL         | アクセス・認可制御不備              | 検査追加オプション          |