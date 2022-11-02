# postfix

## 動作環境
- macOS v13.0
- Docker Engine v20.10.12
- Docker Desktop v4.12.0
- postfix v2.11.0

## 事前準備
1. メール転送サーバサービス([SendGrid](https://app.sendgrid.com/))にアカウントを作成する。
1. 上記サービスのメール転送サーバにアクセスするためのAPIキーを取得する。

## コンテナの起動
1. 任意のディレクトリにリポジトリをチェックアウトする。
1. `./postfix/postfix-docker-compose/`に移動する。
1. `docker-compose.yml.sample`をもとに`docker-compose.yml`を作成する。
1. 作成した`docker-compose.yml`内の設定値を自環境に合わせて編集する。
1. `sasl_passwd.sample`をもとに`sasl_passwd`を作成する。
1. 作成した`sasl_passwd`内のAPIキーの値を事前準備2.で取得したAPIキーで上書きして保存する。
1. `docker-compose up -d`のコマンドを実行する。
1. `docker inspect postfix | grep Gateway`のコマンドを実行して、IPアドレスを控える。
1. 任意のディレクトリで`docker exec -it postfix /bin/bash`のコマンドを実行する。
1. コンテナに入った後、`/etc/postfix/`に移動する。
1. `main.cf`内の一部を以下の通りに編集して保存する。
    - `mynetworks = 127.0.0.0/8 [::ffff:127.0.0.0]/104 [::1]/128 172.0.0.1 {8.で控えたIPアドレス}`
    - `smtpd_recipient_restrictions = permit_mynetworks,permit_sasl_authenticated,reject_unauth_destination`
1. `sh restart.sh`のコマンドを実行して、postfixを再起動する。
1. `sendmail`のコマンドを使用して、任意のメールアドレス宛(自身のメールアドレスなど)にテストメールを送信する。
1. 宛先に指定したメールアドレスにテストメールが受信されたことを確認する。
