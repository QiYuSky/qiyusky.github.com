# ACME.SH 证书自动签发

[acme.sh](https://github.com/acmesh-official/acme.sh) 是一个用 Shell 脚本编写的工具，用于自动化从 Let's Encrypt 或 ZeroSSL 等证书颁发机构（CA）获取和更新 TLS/SSL 证书。

## 准备环境

```bash
# 安装
curl https://get.acme.sh | sh
# 说明：
# 会从 GitHub 上下载 sh 脚本并执行
# 然后把文件解压到用户的 ~/.acme.sh目录下
# 并且给命令行设置一个acme.sh的 alias 别名

# 注册账号
acme.sh --register-account -m name@mail.com

# 注册成功后，会在 ~/.acme.sh/acme.sh.env 中记录账号信息
cat ~/.acme.sh/acme.sh.env
```

## 签发证书

1. HTTP文件验证
适用于可以直接控制 Web 服务器（如 Nginx、Apache）的情况：

    ```bash
    # 生成证书
    acme.sh --issue -d example.com --webroot /path/to/webroot
    # /path/to/webroot 是您的 Web 服务器文档根目录的路径。
    # 该命令会在指定路径下生成测试文件，并提示访问 http://example.com/.well-known/acme-challenge/test.txt 验证是否成功。
    # 如果成功，则会在 ~/.acme.sh/example.com/ 目录下生成证书文件。
    ```
    >
    >- acme.sh 如果不能直接访问 Web 服务器文档根目录下的文件。可以先执行一遍命令以生成测试文件，把测试文件上传到服务器上，然后再次执行 --issue 命令来正常生成证书。

2. DNS验证
适用于需要通过修改 DNS 记录进行验证的情况：

    ```bash
    # 生成证书
    acme.sh --issue -d example.com --dns dns_aliacme.sh --issue -d example.com --dns dns_cloudflare --dns_cloudflare_email your@email.com --dns_cloudflare_api_key your_api_key
    # 请替换为实际的 DNS 服务商、邮箱地址和 API 密钥。
    # 该命令会在 Cloudflare 上创建一个 TXT 记录，用于验证 DNS 记录。
    ```

3. 生成结果
验证通过后，Acme.sh 会为您签发证书。证书默认保存在 ~/.acme.sh/example.com/ 目录下，包括：

    - example.com.crt: 包含完整证书链的PEM格式证书文件。
    - example.com.key: 私钥文件。
    - example.com.cer: 只包含域名证书的PEM格式文件。

## 配置 Web 服务器

```bash
# 复制证书文件
acme.sh --installcert -d example.com \
--key-file          /opt/ssl/cert/key.pem \
--fullchain-file    /opt/ssl/cert/cert.pem \
--reloadcmd         "systemctl reload nginx"

# key.pem 是私钥文件，cert.pem 是证书文件。
# --reloadcmd 指定在证书更新成功后执行的命令。
```

## 自动续期

Acme.sh 默认会设置定时任务（cron job），定期检查并自动续期即将过期的证书。会根据之前执行的命令行参数，自动更新证书。
~/.acme.sh/example.com/example.com.conf 中记录操作参数。

## 其他操作

```bash
# 查看生成的证书列表
acme.sh --list

# 查看证书的详细信息
acme.sh --info -d example.com
```

## * nginx ssl 配置

```bash
# 配置nginx
server {
    listen 443 ssl;
    server_name example.com;

    # SSL 证书文件路径
    ssl_certificate /opt/ssl/cert.pem;
    # SSL 私钥文件路径
    ssl_certificate_key /opt/ssl/key.pem;
    # 安全的加密套件列表
    ssl_ciphers ECDHE-RSA-AES128-GCM-SHA256:ECDHE:ECDH:AES:HIGH:!NULL:!aNULL:!MD5:!ADH:!RC4;
    # 允许的协议版本
    ssl_protocols TLSv1.2 TLSv1.3;
    # 使用服务器提供的加密套件
    ssl_prefer_server_ciphers on;
    # 设置会话超时时间
    ssl_session_timeout  5m;
}
```
