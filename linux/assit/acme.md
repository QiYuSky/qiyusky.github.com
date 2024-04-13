# ACME.SH 证书自动签发

## 开始前准备

1. 下载脚本
    > curl https://get.acme.sh | sh
1. 注册账户
    > acme.sh --register-account -m name@mail.com

1. 签发证书
    > acme.sh --issue -d example.com --webroot /opt/webroot/

1. 复制证书
    ```
    acme.sh --installcert -d example.com \
    --cert-file         /opt/ssl/example.com.cert \
    --key-file          /opt/ssl/example.com.key  \
    --fullchain-file    /opt/ssl/example.com.fullchain
    ```

1. 添加nginx配置信息
    ```
    listen       443 ssl;
    ssl_session_timeout  5m;
    ssl_certificate        /opt/ssl/example.com.fullchain;
    ssl_certificate_key    /opt/ssl/example.com.key;
    ssl_ciphers ECDHE-RSA-AES128-GCM-SHA256:ECDHE:ECDH:AES:HIGH:!NULL:!aNULL:!MD5:!ADH:!RC4;
    ssl_protocols TLSv1 TLSv1.1 TLSv1.2;
    ssl_prefer_server_ciphers on;

    ```
