FROM catatnight/postfix

# aptを更新する。
RUN apt-get update
## 必要なツールをインストールする。
RUN apt-get -y install vim \
  && apt-get -y install telnet \
  && apt-get -y install heirloom-mailx

# 設定ファイルを追加する。
COPY main.cf /etc/postfix/main.cf
COPY restart.sh /etc/postfix/restart.sh

# DBを作成する。
COPY sasl_passwd /etc/postfix/sasl_passwd
RUN chmod 600 /etc/postfix/sasl_passwd
RUN postmap /etc/postfix/sasl_passwd
RUN chmod 600 /etc/postfix/sasl_passwd.db
