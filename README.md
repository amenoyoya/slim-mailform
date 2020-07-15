# slim-mailform

PHP製のシンプルなメールフォーム

## Environment

- OS:
    - Windows 10: Ubuntu 20.04 on WSL2
    - Ubuntu 18.04
- Docker: 19.03.8
    - docker-compose: 1.24.0

### 構成
```bash
./
|_ 00-nginx-proxy/ # ローカルロードバランサ用 Nginx
|                  ## 基本的に全ての docker 開発環境（本プロジェクト以外含む）で使用されるため、
|                  ## 別起動することを推奨（docker service 起動時に自動起動する）
|                  ## http, https ポート（80, 443）を占有するため注意
|_ docker/ # dockerコンテナ設定
|   |_ postfix/ # postfixコンテナ設定
|   |_ web/ # webコンテナ設定
|
|_ app/ # webコンテナのドキュメントルート => docker://web:/var/www/app/
|   |_ index.php # TOPページ
|
|_ docker-compose.yml # Dockerコンテナ構成
    # - docker://web/ <php:7.2-apache>
    #     - メインWEBサーバ
    #     - https://slim-mailform.localhost => /var/www/app/
    # - docker://postfix/ <catatnight/postfix>
    #     - MTA + ローカルSMTPサーバ
    #     - 外部にメール送信する場合は、外部SMTPサーバにリレーする必要がある
    #         - $RELAY_SMTP_HOST: 外部SMTPサーバホスト環境変数
    #         - $RELAY_SMTP_PORT: 外部SMTPサーバポート環境変数
    #         - $RELAY_SMTP_USER: 外部SMTPサーバ接続ユーザ環境変数
    #         - $RELAY_SMTP_PASSWORD: 外部SMTPサーバ接続パスワード環境変数
```
