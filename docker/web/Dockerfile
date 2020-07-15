FROM php:7.4-apache

# composer インストール
COPY --from=composer:latest /usr/bin/composer /usr/bin/composer
ENV PATH $PATH:~/.composer/vendor/bin

# PHP から sendmail する想定のため Apache 実行ユーザ（www-data）のホームディレクトリに .msmtprc を置く
# （root ユーザでもシェルから sendmail する可能性があるため一応 root ホームディレクトリにも置いておく）
COPY ./conf/.msmtprc /var/www/.msmtprc
COPY ./conf/.msmtprc /root/.msmtprc

# 開発に必要なパッケージ等のインストール
RUN apt-get update && apt-get install -y git libicu-dev mailutils unzip vim && \
    docker-php-ext-install intl pdo pdo_mysql && \
    : 'Install nodejs' && \
    curl -sL https://deb.nodesource.com/setup_10.x | bash - && \
    apt-get install -y nodejs && \
    npm install -g yarn && \
    : 'enable RewriteEngine' && \
    a2enmod rewrite && \
    : 'create log directory' && \
    mkdir -p /var/log/httpd/ && \
    : 'install phpstan (PHPコード静的解析ツール)' && \
    composer global require phpstan/phpstan && \
    : 'install msmtp (sendmail 互換の送信専用 MTA; ssmtp の後継)' && \
    : 'msmtp-mta も入れておくとデフォルトの MTA を sendmail から置き換えてくれるため便利' && \
    apt-get install -y msmtp msmtp-mta && \
    : '.msmtprc のパーミッションを修正' && \
    chown www-data:www-data /var/www/.msmtprc && \
    chmod 600 /var/www/.msmtprc && \
    chown root:root /root/.msmtprc && \
    chmod 600 /root/.msmtprc && \
    : 'cleanup apt-get caches' && \
    apt-get clean && rm -rf /var/lib/apt/lists/*

# 作業ディレクトリ
## docker://web:/var/www/app/ => host:./app/
WORKDIR /var/www/app/
